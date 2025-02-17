---
title: Authentifizieren bei Azure-Ressourcen mit Arc-fähigen Servern
description: In diesem Artikel wird die Unterstützung von Azure Instance Metadata Service für Arc-fähige Server beschrieben und wie Sie sich bei Azure-Ressourcen und lokal mithilfe eines Geheimnisses authentifizieren können.
ms.topic: conceptual
ms.date: 07/16/2021
ms.openlocfilehash: 76f7174792f751322545b1d30bb51476c5339e26
ms.sourcegitcommit: e2fa73b682a30048907e2acb5c890495ad397bd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114389908"
---
# <a name="authenticate-against-azure-resources-with-arc-enabled-servers"></a>Authentifizieren bei Azure-Ressourcen mit Arc-fähigen Servern

Anwendungen oder Prozesse, die direkt auf Azure Arc-fähigen Servern ausgeführt werden, können verwaltete Identitäten für den Zugriff auf andere Azure-Ressourcen verwenden, die die Azure Active Directory-basierte Authentifizierung unterstützen. Eine Anwendung kann ein [Zugriffstoken](../../active-directory/develop/developer-glossary.md#access-token) abrufen, das ihre Identität repräsentiert, die bei Arc-fähigen Servern systemseitig zugewiesen wird. Die Anwendung kann dieses Token als Bearertoken verwenden, um sich bei einem anderen Dienst zu authentifizieren.

Eine detaillierte Beschreibung verwalteter Identitäten sowie Informationen zur Unterscheidung zwischen system- und benutzerseitig zugewiesenen Identitäten finden Sie in der [Übersicht über verwaltete Identitäten](../../active-directory/managed-identities-azure-resources/overview.md).

In diesem Artikel erfahren Sie, wie ein Server mithilfe einer systemseitig zugewiesenen verwalteten Identität auf [Azure Key Vault](../../key-vault/general/overview.md) zugreifen kann. Key Vault dient als Bootstrap und ermöglicht es Ihrer Clientanwendung, mithilfe des Geheimnisses auf Ressourcen zuzugreifen, die nicht von Azure Active Directory (AD) geschützt sind. Beispielsweise können TLS/SSL-Zertifikate, die von Ihren IIS-Webservern verwendet werden, in Azure Key Vault gespeichert werden, um sie dann sicher auf Windows- oder Linux-Servern außerhalb von Azure bereitzustellen.

## <a name="security-overview"></a>Sicherheitsübersicht

Beim Onboarding Ihres Servers auf Azure Arc-fähigen Servern werden mehrere Aktionen ausgeführt, um ähnlich wie bei einer Azure-VM die Verwendung einer verwalteten Identität zu konfigurieren:

- Azure Resource Manager empfängt eine Anforderung zur Aktivierung der systemseitig zugewiesenen verwalteten Identität auf einem Arc-fähigen Server.

- Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die Identität des Servers. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

- Azure Resource Manager aktualisiert den Identitätsendpunkt von Azure Instance Metadata Service (IMDS) für [Windows](../../virtual-machines/windows/instance-metadata-service.md) oder [Linux](../../virtual-machines/linux/instance-metadata-service.md) mit der Client-ID und dem Zertifikat des Dienstprinzipals, um die Identität auf dem Server zu konfigurieren. Der Endpunkt ist ein REST-Endpunkt, auf den nur mit einer bekannten, nicht routingfähigen IP-Adresse vom Server aus zugegriffen werden kann. Dieser Dienst stellt eine Teilmenge der Metadateninformationen zum Arc-fähigen Server zur Verfügung, um diesen zu verwalten und zu konfigurieren.

Die Umgebung eines Servers mit aktivierter verwalteter Identität wird mit den folgenden Variablen auf einem Arc-fähigen Windows-Server eingerichtet:

- **IMDS_ENDPOINT**: Die IP-Adresse des IMDS-Endpunkts `http://localhost:40342` für Arc-fähige Server.

- **IDENTITY_ENDPOINT:** der Localhost-Endpunkt, der der verwalteten Identität des Diensts (`http://localhost:40342/metadata/identity/oauth2/token`) entspricht.

Der auf dem Server ausgeführte Code kann ein Token vom Azure IMDS-Endpunkt (Instance Metadata Service) anfordern, auf das nur vom Server aus zugegriffen werden kann.

Anwendungen ermitteln mithilfe der Systemumgebungsvariable **IDENTITY_ENDPOINT** den Identitätsendpunkt. Die Anwendungen sollten versuchen, die Werte **IDENTITY_ENDPOINT** und **IMDS_ENDPOINT** abzurufen, und diese dann verwenden. Anwendungen aller Zugriffsebenen können Anforderungen an diese Endpunkte übermitteln. Metadatenantworten werden normal verarbeitet und an jeden Prozess auf dem Computer übergeben. Wenn jedoch eine Anforderung übermittelt wird, die ein Token verfügbar macht, muss der Client ein Geheimnis bereitstellt, um zu bestätigen, dass er Zugriff auf Daten hat, die nur für Benutzer mit höheren Berechtigungen verfügbar sind.

## <a name="prerequisites"></a>Voraussetzungen

- Kenntnisse im Bereich verwaltete Identitäten.
- Ein Server, der mit Arc-fähigen Servern verbunden und auf diesen registriert ist.
- Sie müssen Mitglied der Gruppe [Besitzer](../../role-based-access-control/built-in-roles.md#owner)** im Abonnement oder der Ressourcengruppe sein, um die erforderlichen Schritte zur Ressourcenerstellung und Rollenverwaltung durchführen zu können.
- Eine Azure Key Vault-Instanz zum Speichern und Abrufen Ihrer Anmeldeinformationen. Ihnen muss die Azure Arc-Identität für den Zugriff auf den Schlüsseltresor zugewiesen sein.

    - Wenn Sie noch keinen Schlüsseltresor erstellt haben, lesen Sie unter [Erstellen eines Schlüsseltresors](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md#create-a-key-vault-) nach.
    - Informationen zum Konfigurieren des Zugriffs durch die verwaltete Identität, die vom Server verwendet wird, finden Sie unter [Gewähren des Zugriffs für Linux](../../active-directory/managed-identities-azure-resources/tutorial-linux-vm-access-nonaad.md#grant-access) oder [Gewähren des Zugriffs für Windows](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md#grant-access). In Schritt 5 geben Sie den Namen des Arc-fähigen Servers ein. Informationen zum Durchführen dieses Vorgangs mithilfe von PowerShell finden Sie unter [Zuweisen einer Zugriffsrichtlinie mithilfe von PowerShell](../../key-vault/general/assign-access-policy-powershell.md).

## <a name="acquiring-an-access-token-using-rest-api"></a>Abrufen eines Zugriffstokens mithilfe der REST-API

Die Methode zum Abrufen und Verwenden einer systemseitig zugewiesenen verwalteten Identität für die Authentifizierung bei Azure-Ressourcen ähnelt der Vorgehensweise bei einer Azure-VM.

Bei einem Arc-fähigen Windows-Server rufen Sie mithilfe von PowerShell die Webanforderung auf, um das Token vom lokalen Host über den angegebenen Port abzurufen. Geben Sie die Anforderung mithilfe der IP-Adresse oder der Umgebungsvariable **IDENTITY_ENDPOINT** an.

```powershell
$apiVersion = "2020-06-01"
$resource = "https://management.azure.com/"
$endpoint = "{0}?resource={1}&api-version={2}" -f $env:IDENTITY_ENDPOINT,$resource,$apiVersion
$secretFile = ""
try
{
    Invoke-WebRequest -Method GET -Uri $endpoint -Headers @{Metadata='True'} -UseBasicParsing
}
catch
{
    $wwwAuthHeader = $_.Exception.Response.Headers["WWW-Authenticate"]
    if ($wwwAuthHeader -match "Basic realm=.+")
    {
        $secretFile = ($wwwAuthHeader -split "Basic realm=")[1]
    }
}
Write-Host "Secret file path: " $secretFile`n
$secret = cat -Raw $secretFile
$response = Invoke-WebRequest -Method GET -Uri $endpoint -Headers @{Metadata='True'; Authorization="Basic $secret"} -UseBasicParsing
if ($response)
{
    $token = (ConvertFrom-Json -InputObject $response.Content).access_token
    Write-Host "Access token: " $token
}
```

Die folgende Antwort ist ein Beispiel für die Rückgabe:

:::image type="content" source="media/managed-identity-authentication/powershell-token-output-example.png" alt-text="Erfolgreicher Abruf des Zugriffstokens mithilfe von PowerShell":::

Bei einem Arc-fähigen Linux-Server rufen Sie mithilfe der Bash die Webanforderung auf, um das Token vom lokalen Host über den angegebenen Port abzurufen. Geben Sie die folgende Anforderung mithilfe der IP-Adresse oder der Umgebungsvariable **IDENTITY_ENDPOINT** an. Zum Abschließen dieses Schritts benötigen Sie einen SSH-Client.

```bash
ChallengeTokenPath=$(curl -s -D - -H Metadata:true "http://127.0.0.1:40342/metadata/identity/oauth2/token?api-version=2019-11-01&resource=https%3A%2F%2Fmanagement.azure.com" | grep Www-Authenticate | cut -d "=" -f 2 | tr -d "[:cntrl:]")
ChallengeToken=$(cat $ChallengeTokenPath)
if [ $? -ne 0 ]; then
    echo "Could not retrieve challenge token, double check that this command is run with root privileges."
else
    curl -s -H Metadata:true -H "Authorization: Basic $ChallengeToken" "http://127.0.0.1:40342/metadata/identity/oauth2/token?api-version=2019-11-01&resource=https%3A%2F%2Fmanagement.azure.com"
fi
```

Die folgende Antwort ist ein Beispiel für die Rückgabe:

:::image type="content" source="media/managed-identity-authentication/bash-token-output-example.png" alt-text="Erfolgreicher Abruf des Zugriffstokens mit der Bash":::

Die Antwort enthält das Zugriffstoken, das Sie für den Zugriff auf Ressourcen in Azure benötigen. Informationen zum Vervollständigen der Konfiguration für die Authentifizierung bei Azure Key Vault finden Sie unter [Zugreifen auf Key Vault unter Windows](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md#access-data) oder [Zugreifen auf Key Vault unter Linux](../../active-directory/managed-identities-azure-resources/tutorial-linux-vm-access-nonaad.md#access-data).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich unter [Übersicht über Key Vault](../../key-vault/general/overview.md) über Azure Key Vault.

- Erfahren Sie, wie Sie den Zugriff mit einer verwalteten Identität auf eine Ressource mithilfe von [PowerShell](../../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md) oder der [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md) zuweisen.