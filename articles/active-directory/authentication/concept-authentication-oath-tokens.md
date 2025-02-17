---
title: Authentifizierungsmethode mit OATH-Token – Azure Active Directory
description: Erfahren Sie mehr über die Verwendung von OATH-Token zum Verbessern und Sichern von Anmeldeereignissen in Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/26/2021
ms.author: justinha
author: justinha
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d5e34ee5013a6f4ad28779e92536ff2d8ef868
ms.sourcegitcommit: f2eb1bc583962ea0b616577f47b325d548fd0efa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2021
ms.locfileid: "114730766"
---
# <a name="authentication-methods-in-azure-active-directory---oath-tokens"></a>Authentifizierungsmethoden in Azure Active Directory – OATH-Token 

OATH TOTP (Time-based One Time Password) ist ein offener Standard, der angibt, wie Einmalkennwortcodes (one-time password, OTP) generiert werden. OATH TOTP kann entweder mit Hardware oder Software zum Generieren der Codes implementiert werden. OATH HOTP, ein anderer Standard zum Generieren von Code, wird in Azure AD nicht unterstützt.

## <a name="oath-software-tokens"></a>OATH-Softwaretoken

Bei OATH-Softwaretoken handelt es sich in der Regel um Anwendungen (z. B. die Microsoft Authenticator-App und andere Authentifizierungs-Apps). Azure AD generiert den geheimen Schlüssel (bzw. Ausgangswert), der in der App eingegeben und zum Generieren des jeweiligen Einmalkennworts (OTP) verwendet wird.

Die Authentizierungs-App generiert automatisch Codes, wenn sie für Push-Benachrichtigungen eingerichtet wurde. Daher verfügt der Benutzer über eine Sicherung, selbst wenn sein Gerät keine Verbindung herstellen kann. Anwendungen von Drittanbietern, die OATH TOTP zum Generieren von Codes verwenden, können ebenfalls zum Einsatz kommen.

Einige OATH TOTP-Hardwaretoken sind programmierbar, d. h., sie sind nicht mit einem geheimen Schlüssel oder Ausgangswert vorprogrammiert. Diese programmierbaren Hardwaretoken können mithilfe des geheimen Schlüssels oder Ausgangswerts, der aus dem Softwaretoken-Setupflow abgerufen wird, eingerichtet werden. Kunden können diese Token bei dem Anbieter ihrer Wahl erwerben und den geheimen Schlüssel bzw. den Ausgangswert beim Setupvorgang ihres Anbieters verwenden.

## <a name="oath-hardware-tokens-preview"></a>OATH-Hardwaretoken (Vorschau)

Azure AD unterstützt die Verwendung von OATH TOTP SHA-1-Token, die den Code alle 30 oder 60 Sekunden aktualisieren. Kunden können diese Token beim Anbieter ihrer Wahl erwerben. 

OATH TOTP-Hardwaretoken sind in der Regel mit einem geheimen, im Token vorprogrammierten Schlüssel oder Anfangswert versehen. Diese Schlüssel müssen entsprechend der Beschreibung in den folgenden Schritten in Azure AD eingegeben werden. Geheime Schlüssel sind auf 128 Zeichen beschränkt, was möglicherweise nicht mit allen Token kompatibel ist. Der geheime Schlüssel darf nur die Zeichen *a-z* oder *A-Z* und die Ziffern *2-7* enthalten und muss in *Base32* codiert sein.

Programmierbare OATH TOTP-Hardwaretoken, die mit einem neuen Ausgangswert versehen werden können, können ebenfalls im Softwaretoken-Setupflow mit Azure AD eingerichtet werden.

OATH-Hardwaretoken werden in der öffentlichen Vorschau unterstützt. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![Hochladen von OATH-Token auf das Blatt „MFA > OATH-Token“](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Nachdem Sie Token erworben haben, müssen Sie diese in einem Format mit durch Trennzeichen getrennten Werten (CSV) einschließlich UPN, Seriennummer, geheimem Schlüssel, Zeitintervall, Hersteller und Modell hochladen, wie im folgenden Beispiel gezeigt:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,2234567abcdef2234567abcdef,60,Contoso,HardwareKey
```  

> [!NOTE]
> Stellen Sie sicher, dass Sie die Kopfzeile in Ihre CSV-Datei einschließen. Wenn ein UPN ein einzelnes Anführungszeichen enthält, versehen Sie es mit einem weiteren einfachen Anführungszeichen als Escapezeichen. Wenn der UPN beispielsweise „my’user@domain.com“ lautet, ändern Sie ihn beim Hochladen der Datei in „my’’user@domain.com“.

Nach der korrekten Formatierung als CSV-Datei kann sich ein globaler Administrator dann am Azure-Portal anmelden, zu **Azure Active Directory > Sicherheit > MFA > OATH-Tokens** navigieren und die resultierende CSV-Datei hochladen.

Je nach Größe der CSV-Datei kann die Verarbeitung ein paar Minuten dauern. Wählen Sie die Schaltfläche **Aktualisieren** aus, um den aktuellen Status abzurufen. Wenn die Datei Fehler enthält, können Sie eine CSV-Datei herunterladen, in der alle zu behebenden Fehler aufgeführt sind. Die Feldnamen in der heruntergeladenen CSV-Datei unterscheiden sich von der hochgeladenen Version.  

Sobald alle Fehler behoben sind, kann der Administrator die einzelnen Schlüssel aktivieren, indem er für das Token **Aktivieren** auswählt und das in dem Token angezeigte Einmalkennwort eingibt. Sie können maximal 200 OATH-Token alle 5 Minuten aktivieren. 

Benutzer verfügen möglicherweise über eine Kombination aus bis zu fünf OATH-Hardwaretoken oder Authentifizierungsanwendungen wie die Microsoft Authenticator-App, die für die jederzeitige Verwendung konfiguriert sind. Hardware-Oath-Token können Gastbenutzern im Ressourcen Anker nicht zugewiesen werden.

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Übersicht über die Azure AD-Authentifizierungsmethoden](/graph/api/resources/authenticationmethods-overview) finden Sie weitere Informationen zum Konfigurieren von Authentifizierungsmethoden.
Informieren Sie sich über [FIDO2-Sicherheitsschlüsselanbieter](concept-authentication-passwordless.md#fido2-security-key-providers), die mit der kennwortlosen Authentifizierung kompatibel sind.
