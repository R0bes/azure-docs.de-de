---
title: Privates und sicheres Herstellen einer Verbindung mit Azure Purview und Überprüfen von Datenquellen
description: In diesem Artikel wird beschrieben, wie Sie einen privaten Endpunkt einrichten können, um eine Verbindung mit Ihrem Purview-Konto herzustellen und Datenquellen aus einem eingeschränkten Netzwerk auf eine End-to-End-Isolation zu überprüfen
author: zeinam
ms.author: zeinam
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 08/18/2021
ms.openlocfilehash: a4e1270751f0f675f579315bfc69a808ccf903ea
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2021
ms.locfileid: "123250678"
---
# <a name="connect-to-your-azure-purview-and-scan-data-sources-privately-and-securely"></a>Privates und sicheres Herstellen einer Verbindung mit Azure Purview und Überprüfen von Datenquellen

In diesem Leitfaden erfahren Sie, wie Sie private Endpunkte für die _Konten_, das _Portal_ und die _Erfassung_ für Ihr Azure Purview-Konto bereitstellen. So können Sie sicher und privat auf das Purview-Konto zugreifen und Datenquellen mithilfe einer selbstgegehosteten Integration Runtime überprüfen. Dadurch wird eine End-to-End-Netzwerkisolation ermöglicht.

Mit dem privaten Azure Purview-Endpunkt _Konto_ wird eine zusätzliche Sicherheitsschicht hinzugefügt, indem Szenarios ermöglicht werden, bei denen nur Clientaufrufe aus dem virtuellen Netzwerk für den Zugriff auf das Azure Purview-Konto zulässig sind. Dieser private Endpunkt ist auch eine Voraussetzung für den privaten Endpunkt „Portal“.

Der privaten Azure Purview-Endpunkt _Portal_ ist erforderlich, um die Verbindung zu Azure Purview Studio über ein privates Netzwerk zu ermöglichen.

Azure Purview kann Datenquellen in Azure oder in einer lokalen Umgebung mithilfe privater Endpunkte für die _Erfassung_ überprüfen. Drei Ressourcen für private Endpunkte müssen bereitgestellt und mit den verwalteten Azure Purview-Ressourcen verknüpft werden, wenn private Endpunkte für die Erfassung bereitgestellt werden:

- Der private Blob-Endpunkt ist mit einem verwalteten Azure Purview-Speicherkonto verknüpft.
- Der private Warteschlangenendpunkt ist mit einem verwalteten Azure Purview-Speicherkonto verknüpft.
- Der private Endpunkt-Namespace ist mit dem verwalteten Event-Hub-Namespace von Azure Purview verknüpft.

   :::image type="content" source="media/catalog-private-link/purview-private-link-architecture.png" alt-text="Eine Abbildung, die die Architektur von Azure Purview und Private Link zeigt":::.

## <a name="deployment-checklist"></a>Bereitstellungsprüfliste
Mithilfe einer der weiter in diesem Leitfaden erläuterten Bereitstellungsoptionen können Sie ein neues Azure Purview-Konto mit den privaten Endpunkten für das _Konto_, das _Portal_ und die _Erfassung_ bereitstellen. Alternative können Sie diese privaten Endpunkte für ein vorhandenes Azure Purview-Konto bereitstellen:

1. Wählen Sie ein geeignetes virtuelles Azure-Netzwerk und ein Subnetz aus, um private Endpunkte für Azure Purview bereitzustellen. Wählen Sie eine der folgenden Optionen aus:
   - Erstellen Sie ein [neues virtuelles Netzwerk](../virtual-network/quick-create-portal.md) unter Ihrem Azure-Abonnement.
   - Suchen Sie ein vorhandenes virtuelles Azure-Netzwerk und ein Subnetz in Ihrem Azure-Abonnement.
  
2. Definieren Sie eine geeignete [Methode zur DNS-Namensauflösung](./catalog-private-link-name-resolution.md#deployment-options), damit Sie über ein privates Netzwerk auf das Azure Purview-Konto zugreifen und die Datenquellen überprüfen können. Sie können auch eine der folgenden Optionen verwenden:
   - Stellen Sie neue Azure DNS-Zonen mithilfe der weiter unten in diesem Leitfaden beschriebenen Schritte bereit.
   - Fügen Sie den vorhandenen Azure DNS-Zonen die erforderlichen DNS-Einträgen hinzu, indem Sie die weiter unten in diesem Leitfaden beschriebenen Schritte ausführen.
   - Fügen Sie nach Abschluss der Schritte in diesem Leitfaden den vorhandenen DNS-Servern die erforderlichen DNS-A-Einträge manuell hinzu.
3. Stellen Sie ein [neues Purview-Konto](#option-1---deploy-a-new-azure-purview-account-with-account-portal-and-ingestion-private-endpoints) mit privaten Endpunkten für das Konto, das Portal und der Erfassung bereit, oder stellen Sie private Endpunkte für ein vorhandenes [Purview-Konto](#option-2---enable-account-portal-and-ingestion-private-endpoint-on-existing-azure-purview-accounts) bereit.
4. [Aktivieren Sie den Zugriff auf Azure Active Directory](#enable-access-to-azure-active-directory), wenn in Ihrem privaten Netzwerk die Regeln für Netzwerksicherheitsgruppen so eingestellt sind, dass der gesamte öffentliche Internetverkehr abgelehnt wird.
5. Bereitstellen und Registrieren der [Selbstgehosteten Integration Runtime](#deploy-self-hosted-integration-runtime-ir-and-scan-your-data-sources) in demselben VNet, in dem die privaten Endpunkte der Azure Purview-Erfassung bereitgestellt werden.
6. Passen Sie nach Abschluss dieses Leitfadens bei Bedarf die DNS-Konfigurationen an.
7. Überprüfen Sie Ihre Netzwerk- und Namensauflösung zwischen dem Verwaltungscomputer, der selbstgehosteten IR auf dem virtuellen Computer und den Datenquellen in Azure Purview. 

## <a name="option-1---deploy-a-new-azure-purview-account-with-_account_-_portal_-and-_ingestion_-private-endpoints"></a>Option 1: Bereitstellen eines neuen Azure Purview-Kontos mit privaten Endpunkten für das _Konto_, das _Portal_ und die _Erfassung_

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com) und dann zu der Seite **Purview-Konto**. Wählen Sie **+ Erstellen** aus, um ein neues Azure Purview Konto zu erstellen.

2. Geben Sie die grundlegenden Informationen ein, und legen Sie auf der Registerkarte **Netzwerk** als Konnektivitätsmethode **Privater Endpunkt** fest. Legen Sie privaten Endpunkt aktivieren auf **Konto, Portal und Erfassung** fest.

3. Navigieren Sie zu **Konto und Portal** und wählen Sie **+ Hinzufügen** aus, um Ihrem Azure Purview-Konto einen privaten Endpunkt hinzuzufügen.

   :::image type="content" source="media/catalog-private-link/purview-pe-scenario-3-1.png" alt-text="Ein Screenshot, der die End-to-End-Seitenauswahl für die Erstellung eines privaten Endpunkts zeigt":::.

4. Wählen Sie auf der Seite für **Purview-Unterressource** die Option **Einen Privaten Endpunkt erstellen** aus. Wählen Sie dann Ihren Standort aus, geben Sie einen Namen für den privaten Endpunkt des _Kontos_ an und klicken Sie dann auf **Konto**. Wählen Sie unter **Netzwerk** Ihr virtuelles Netzwerk und das Subnetz aus. Wählen Sie dann optional **Integration in eine private DNS-Zone** aus, um eine neue Azure Private DNS-Zone zu erstellen. 

   :::image type="content" source="media/catalog-private-link/purview-pe-scenario-1-2.png" alt-text="Ein Screenshot, der sie Seite für das Erstellen eines privaten Endpunkts für ein Konto zeigt":::.

   > [!NOTE]
   > Sie können auch Ihre vorhandenen Azure Private DNS-Zonen verwenden oder später manuell die DNS-Einträge in Ihren DNS-Servern erstellen. Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte für die DNS-Namensauflösung](./catalog-private-link-name-resolution.md)

5. Klicken Sie auf **OK**.
   
6. Wählen Sie unter **Konto- und Portal-Assistent** erneut **+ Hinzufügen** aus, um dem _Portals_ einen privaten Endpunkt hinzuzufügen. 
  
7. Wählen Sie auf der Seite für **Purview-Unterressource** die Option **Einen Privaten Endpunkt erstellen** aus. Wählen Sie dann Ihren Standort aus, geben Sie einen Namen für den privaten Endpunkt des _Portals_ an und klicken Sie dann auf **Portal**. Wählen Sie unter **Netzwerk** Ihr virtuelles Netzwerk und das Subnetz aus. Wählen Sie dann optional **Integration in eine private DNS-Zone** aus, um eine neue Azure Private DNS-Zone zu erstellen. 
   
   > [!NOTE]
   > Sie können auch Ihre vorhandenen Azure Private DNS-Zonen verwenden oder später manuell die DNS-Einträge in Ihren DNS-Servern erstellen. Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte für die DNS-Namensauflösung](./catalog-private-link-name-resolution.md)

8. Klicken Sie auf **OK**.

9.  Richten Sie unter **Erfassung** Ihre privaten Endpunkte für die Erfassung ein, indem Sie die Details für das **Abonnement**, das **virtuelles Netzwerk** und das **Subnetz** angeben, die Sie mit Ihrem privaten Endpunkt verbinden möchten.

10. Wählen Sie optional **Privates DNS Integration** aus, um Azure Private DNS-Zonen zu verwenden.
   
      :::image type="content" source="media/catalog-private-link/purview-pe-scenario-3-2.png" alt-text="Ein Screenshot, der die Übersichtsseite „Privaten Endpunkt erstellen“ zeigt":::.

      > [!IMPORTANT]
      > Es ist wichtig, die richtigen Azure Private DNS-Zonen auszuwählen, um eine korrekte Namensauflösung zwischen Azure Purview und den Datenquellen zu ermöglichen. Sie können auch Ihre vorhandenen Azure Private DNS-Zonen verwenden oder später manuell die DNS-Einträge in Ihren DNS-Servern erstellen. Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte für die DNS-Namensauflösung](./catalog-private-link-name-resolution.md)

11. Klicken Sie auf **Überprüfen + erstellen**. Auf der Seite **Überprüfen + Erstellen** überprüft Azure Ihre Konfiguration.

12. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus.
   

## <a name="option-2---enable-_account_-_portal_-and-_ingestion_-private-endpoint-on-existing-azure-purview-accounts"></a>Option 2: Aktivieren der privaten Endpunkte für das _Konto_, das _Portal_ und die _Erfassung_ in vorhandenen Azure Purview-Konten

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie auf Ihr Azure Purview-Konto und wählen Sie unter **Einstellungen** **Netzwerk** die Option **Private Endpunktverbindungen** aus.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Ein Screenshot, der das Erstellen eines privaten Endpunkts für ein Konto zeigt":::.

2. Wählen Sie **+ Privater Endpunkt** aus, um einen neuen privaten Endpunkt zu erstellen.

3. Geben Sie die grundlegenden Informationen an.

4. Wählen Sie auf der Registerkarte **Ressource** für **Ressourcentyp** die Option **Microsoft.Purview/accounts** aus.

5. Wählen Sie für **Ressource** das Azure Purview-Konto und für **Zielunterressource** die Option **Konto** aus.

6. Wählen Sie auf der Registerkarte **Konfiguration** das virtuelle Netzwerk aus und wählen Sie optional Azure Private DNS-Zone aus, um eine neue Azure DNS zu erstellen.
   
   > [!NOTE]
   > Für die DNS-Konfiguration können Sie auch Ihre vorhandenen Azure Private DNS-Zonen aus der Dropdownliste verwenden oder die erforderlichen DNS-Einträge später manuell Ihren DNS-Servern hinzufügen. Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte für die DNS-Namensauflösung](./catalog-private-link-name-resolution.md)

7. Wechseln Sie zu der Seite mit den Zusammenfassungen und wählen Sie **Erstellen** aus, um den privaten Endpunkt des Portals zu erstellen.
   
8. Führen Sie dieselben Schritte aus, wenn Sie für die **Zielunterressource** die Option **Portal** auswählen.
   
9. Navigieren Sie in Ihrem Azure Purview-Konto zu **Einstellungen** und wählen Sie unter **Netzwerk** die Option **Erfassen von privaten Endpunktverbindungen** aus.

   :::image type="content" source="media/catalog-private-link/purview-pe-scenario-3-3.png" alt-text="Ein Screenshot, der das Hinzufügen eines privaten Endpunkts zu einem vorhandenen Konto zeigt":::.


10. Wählen Sie unter Erfassen privater Endpunktverbindungen für **+ Neu** aus, um einen neuen privaten Endpunkt für die Erfassung zu erstellen.

11. Geben Sie die grundlegenden Informationen ein und wählen Sie Ihr vorhandenes virtuelles Netzwerk und ein Subnetz aus. Wählen Sie optional **Privates DNS Integration** aus, um Azure Private DNS-Zonen zu verwenden. Wählen Sie die richtigen Azure Private DNS-Zonen von jeder Liste aus.

   > [!NOTE]
   > Sie können auch Ihre vorhandenen Azure Private DNS-Zonen verwenden oder später manuell die DNS-Einträge in Ihren DNS-Servern erstellen. Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte für die DNS-Namensauflösung](./catalog-private-link-name-resolution.md)

12. Wählen Sie **Erstellen** aus, um die Einrichtung abzuschließen.

## <a name="enable-access-to-azure-active-directory"></a>Aktivieren des Zugriffs auf Azure Active Directory

> [!NOTE]
> Wenn Ihr virtueller Computer, Ihr VPN-Gateway oder Ihr VNet-Peering-Gateway über öffentlichen Internetzugriff verfügt, kann er bzw. es auf das Azure Purview-Portal und das Azure Purview-Konto zugreifen, das mit privaten Endpunkten aktiviert ist. Daher müssen Sie die übrigen Anweisungen nicht befolgen. Wenn für Ihr privates Netzwerk NSG-Regeln festgelegt sind, die den gesamten öffentlichen Internetdatenverkehr ablehnen, müssen Sie Regeln hinzufügen, um den Azure Active Directory-Zugriff (Azure AD) zu aktivieren. Befolgen Sie dafür die entsprechenden Anweisungen.

Diese Anweisungen gelten für den sicheren Zugriff auf Azure Purview von einem virtuellen Azure-Computer. Wenn Sie ein VPN-Gateway oder ein anderes VNet-Peeringgateway verwenden, müssen Sie ähnliche Schritte ausführen.

1. Navigieren Sie im Azure-Portal zu Ihrem virtuellen Computer, und wählen Sie im Abschnitt **Einstellungen** die Option **Netzwerk** aus. Wählen Sie dann **Regeln für ausgehende Ports** > **Regel für einen ausgehenden Port hinzufügen** aus.

   :::image type="content" source="media/catalog-private-link/outbound-rule-add.png" alt-text="Ein Screenshot, der das Hinzufügen einer Ausgangsregel zeigt":::.

1. Gehen Sie im Bereich **Ausgangssicherheitsregel hinzufügen** wie folgt vor:

   1. Wählen Sie **Diensttag** als **Ziel** aus.
   1. Wählen Sie unter **Zieldiensttag** die Option **AzureActiveDirectory** aus.
   1. Wählen Sie unter **Zielportbereiche** das Sternchen (*) aus.
   1. Wählen Sie für **Aktion** die Option **Zulassen** aus.
   1. Der Wert unter **Priorität** sollte höher sein als die Regel, mit der der gesamte Internetdatenverkehr verweigert wurde.
   
   Erstellen Sie die Regel.

   :::image type="content" source="media/catalog-private-link/outbound-rule-details.png" alt-text="Ein Screenshot, der die Details beim Hinzufügen einer Ausgangsregel zeigt":::.

1. Führen Sie die gleichen Schritte aus, um eine weitere Regel zu erstellen, die das Diensttag **AzureResourceManager** zulässt. Wenn Sie auf das Azure-Portal zugreifen müssen, können Sie auch eine Regel für das Diensttag dem **AzurePortal** hinzufügen.

1. Stellen Sie eine Verbindung mit dem virtuellen Computer her, und öffnen Sie den Browser. Navigieren Sie zur Browserkonsole, indem Sie STRG+UMSCHALT+J drücken, und wechseln Sie dort zur Registerkarte „Netzwerk“, um die Netzwerkanforderungen zu überwachen. Geben Sie im Feld „URL“ die Adresse „web.purview.azure.com“ ein, und versuchen Sie, sich mit Ihren Azure AD-Anmeldeinformationen anzumelden. Wahrscheinlich ist die Anmeldung nicht erfolgreich, und Sie können auf der Registerkarte **Netzwerk** der Konsole erkennen, dass Azure AD versucht, auf „aadcdn.msauth.net“ zuzugreifen, der Zugriff aber blockiert wird.

   :::image type="content" source="media/catalog-private-link/login-fail.png" alt-text="Ein Screenshot, der die Details der nicht erfolgreichen Anmeldung zeigt":::.

1. Öffnen Sie in diesem Fall auf dem virtuellen Computer eine Eingabeaufforderung, und pingen Sie „aadcdn.msauth.net“. Nachdem Sie die zugehörige IP-Adresse abgerufen haben, fügen Sie in den Netzwerksicherheitsregeln des virtuellen Computers eine Regel für einen ausgehenden Port für die IP-Adresse hinzu. Legen Sie für das **Ziel** die Option **IP-Adressen** und für die **Ziel-IP-Adressen** die IP-Adresse des AAD-CDN fest. Aufgrund von Azure Load Balancer und Azure Traffic Manager ist die IP-Adresse des Content Delivery Network von Azure AD möglicherweise dynamisch. Nachdem Sie die IP-Adresse abgerufen haben, sollten Sie sie der Hostdatei des virtuellen Computers hinzufügen, um zu erzwingen, dass der Browser diese IP-Adresse für den Zugriff auf Azure AD Content Delivery Network aufruft.

   :::image type="content" source="media/catalog-private-link/ping.png" alt-text="Ein Screenshot, der den Test-Ping zeigt":::.

   :::image type="content" source="media/catalog-private-link/aadcdn-rule.png" alt-text="Einer Screenshot, der die Azure AD Content Delivery Network-Regel zeigt":::.

1. Nachdem die neue Regel erstellt wurde, navigieren Sie zurück zu dem virtuellen Computer, und melden Sie sich erneut mit Ihren Azure AD-Anmeldeinformationen an. Wenn die Anmeldung erfolgreich ist, kann das Azure Purview-Portal verwendet werden. In einigen Fällen führt Azure AD jedoch basierend auf dem Kontotyp eines Kunden eine Umleitung an andere Domänen durch. Bei „live.com“-Konten beispielsweise leitet Azure AD den Benutzer zur Anmeldung an „live.com“ um, in denen diese Anforderungen erneut blockiert werden. Bei Microsoft-Mitarbeiterkonten greift Azure AD auf „msft.sts.microsoft.com“ zu, um die Anmeldeinformationen zu erhalten.

   Überprüfen Sie auf der Registerkarte **Netzwerk** der Browserkonsole die Netzwerkanforderungen, um die Domäne zu ermitteln, deren Anforderungen blockiert werden. Wiederholen Sie dann den vorherigen Schritt, um ihre IP-Adresse abzurufen und fügen Sie in der Netzwerksicherheitsgruppe Regeln für ausgehenden Port hinzu, um Anforderungen für diese IP-Adresse zuzulassen. Fügen Sie die URL und die IP-Adresse nach Möglichkeit der Hostdatei des virtuellen Computers hinzu, um die DNS-Auflösung zu korrigieren. Wenn Sie die genauen IP-Adressbereiche der Anmeldedomäne kennen, können Sie sie auch direkt den Netzwerkregeln hinzufügen.

1. Ihre Azure AD-Anmeldung sollte nun erfolgreich sein. Das Azure Purview-Portal wird erfolgreich geladen, es können jedoch nicht alle Azure Purview-Konten aufgeführt werden, da nur auf ein bestimmtes Azure Purview-Konto zugegriffen werden kann. Geben Sie `web.purview.azure.com/resource/{PurviewAccountName}` ein, um direkt zu dem Azure Purview-Konto zu gelangen, für das Sie erfolgreich einen privaten Endpunkt eingerichtet haben.

## <a name="deploy-self-hosted-integration-runtime-ir-and-scan-your-data-sources"></a>Stellen Sie die selbstgehostete Integration Runtime (IR) bereit und überprüfen Sie Ihre Datenquellen.
Nachdem Sie die privaten Erfassungsendpunkte für Azure Purview bereitgestellt haben, müssen Sie mindestens eine selbstgehostete Integration Runtime (IR) einrichten und registrieren:

- Alle lokalen Quelltypen wie z. B. Microsoft SQL Server, Oracle oder SAP werden derzeit nur über Überprüfungen mit einer selbstgehosteten Integration Runtime (IR) unterstützt. Die selbstgehostete IR muss innerhalb Ihres privaten Netzwerks ausgeführt und mit Ihrem virtuellen Netzwerk in Azure per Peering verbunden werden. 
   
- Für alle Azure-Quelltypen wie Azure Blob Storage und Azure SQL-Datenbank müssen Sie die Überprüfung explizit mithilfe einer selbstgehosteten Integration Runtime ausführen, die in demselben VNet wie der private Azure Purview-Erfassungsendpunkt bereitgestellt wird. 

Führen Sie die Schritte unter [Erstellen und Verwalten einer selbstgehosteten Integration Runtime](manage-integration-runtimes.md), um eine selbstgehostete IR einzurichten. Richten Sie anschließend die Überprüfung für die Azure-Quelle ein, indem Sie in der Dropdownliste **Verbindung über Integration Runtime herstellen** Ihre selbstgehostete IR auswählen, um die Netzwerkisolation sicherzustellen.
    
   :::image type="content" source="media/catalog-private-link/shir-for-azure.png" alt-text="Ein Screenshot, der das Ausführen einer Azure-Überprüfung mithilfe einer selbstgehosteten IR zeigt":::.

> [!IMPORTANT]
> Wenn Sie Ihr Azure Purview-Konto nach dem 18. August 2021 erstellt haben, stellen Sie sicher, dass Sie die neueste Version der selbstgehosteten Integration Runtime aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717) herunterladen und installieren.

## <a name="firewalls-to-restrict-public-access"></a>Firewalls zur Einschränkung des öffentlichen Zugriffs

Führen Sie diese Schritte aus, um den Zugriff auf das Azure Purview-Konto über das öffentliche Internet vollständig zu unterbrechen. Diese Einstellung gilt sowohl für private Endpunktverbindungen als auch für private Endpunktverbindungen für die Erfassung.

1. Navigieren Sie im Azure-Portal zum Azure Purview-Konto, und wählen Sie unter **Einstellungen** > **Netzwerk** die Option **Private Endpunktverbindungen** aus.

1. Navigieren Sie zur Registerkarte **Firewall** und vergewissern Sie sich, dass die Einstellung auf **Verweigern** festgelegt ist.

   :::image type="content" source="media/catalog-private-link/private-endpoint-firewall.png" alt-text="Ein Screenshot, der die Firewalleinstellungen für private Endpunkte zeigt":::.

## <a name="next-steps"></a>Nächste Schritte

-  [Überprüfen der Auflösung für private Endpunkte](./catalog-private-link-name-resolution.md)
-  [Verwalten von Datenquellen in Azure Purview](./manage-data-sources.md)
-  [Problembehandlung bei der Konfiguration des privaten Endpunkts für Ihr Azure Purview-Konto](./catalog-private-link-troubleshoot.md)
