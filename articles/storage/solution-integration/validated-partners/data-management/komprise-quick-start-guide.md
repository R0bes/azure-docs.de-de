---
title: Analysieren und Migrieren Ihrer Dateidaten zu Azure mit Komprise Intelligent Data Manager
titleSuffix: Azure Storage
description: Leitfaden zu den ersten Schritten für die Implementierung von Komprise Intelligent Data Manager. Leitfaden zur Analyse Ihrer Dateiinfrastruktur und Migration Ihrer Daten zu Azure Files, Azure NetApp Files, Azure Blob Storage oder einer verfügbaren ISV NAS-Lösung
author: dukicn
ms.author: nikoduki
ms.date: 05/20/2021
ms.topic: conceptual
ms.service: storage
ms.subservice: partner
ms.openlocfilehash: 97ad7067b598ad4aa03f46cc7124dcc05d7f2b42
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112115690"
---
# <a name="analyze-and-migrate-to-azure-with-komprise"></a>Analysieren und Migrieren zu Azure mit Komprise

Dieser Artikel enthält Informationen zur Integration der Komprise Intelligent Data Manager-Infrastruktur mit Azure-Speicherdiensten. Es werden die Aspekte und die Implementierungsschritte zum Analysieren und Migrieren Ihrer Daten beschrieben.

Komprise verfügt über Funktionen zum Durchführen von Analysen und Gewinnen von Erkenntnissen zu NAS-Systemen (Network Attached Storage). Die Anwendung ermöglicht die Migration von Daten zu Azure-Speicherdiensten wie Azure Files, Azure NetApp Files, Azure Blob Storage oder ISV NAS-Lösungen. Weitere Informationen finden Sie im Artikel zu den [verifizierten Partnerlösungen für primären und sekundären Speicher](../primary-secondary-storage/partner-overview.md).

Mit Komprise erhalten Sie Unterstützung in vielen unterschiedlichen Anwendungsfällen, z. B.:

- Analysieren von lokalen unstrukturierten Daten, um Erkenntnisse in den Bereichen Verwaltung, Verschiebung, Positionierung, Archivierung, Schutz und Beschränkung von Daten zu gewinnen
- Migrieren von lokalen unstrukturierten Daten zu Azure Files, Azure NetApp Files oder einer ISV NAS-Lösung
- Kopieren oder Archivieren von lokalen unstrukturierten Daten in Azure Blob Storage mit nativem Zugriff
- Migrieren von Objektspeicherlösungen zu Azure Blob Storage

## <a name="reference-architecture"></a>Referenzarchitektur

Referenzarchitektur für Sicherungsbereitstellungen vom lokalen Standort zu Azure und für Azure-interne Bereitstellungen.

:::image type="content" source="./media/komprise-quick-start-guide/komprise-architecture.png" alt-text="Referenzarchitektur: Grundlegendes Setup für Komprise Intelligent Data Manager":::

Komprise ist eine Softwarelösung, die leicht in einer virtuellen Umgebung bereitgestellt werden kann. Sie umfasst Folgendes:
- **Director**: Die Verwaltungskonsole für Komprise Grid. Sie wird verwendet, um die Umgebung zu konfigurieren, Aktivitäten zu überwachen, Berichte und Diagramme anzuzeigen und Richtlinien festzulegen.
- **Observers**: Ermöglichen die Verwaltung und Analyse von Freigaben, die Zusammenfassung von Berichten, die Kommunikation mit der Director-Lösung und die Verarbeitung von NFS-Datenverkehr.
- **Proxys**: Ermöglichen die Vereinfachung und Beschleunigung des SMB/CIFS-Datenflusses und eine einfache Skalierung, um die Leistungsanforderungen einer wachsenden Umgebung zu erfüllen.

## <a name="before-you-begin"></a>Vorbereitung

Die Vorabplanung trägt dazu bei, das Risiko für die Migration der Daten zu verringern.

### <a name="get-started-with-azure"></a>Erste Schritte mit Azure

Microsoft bietet ein Framework, das Ihnen den Einstieg in Azure erleichtert. Das [Cloud Adoption Framework](/azure/architecture/cloud-adoption/) (CAF) ist ein detailliertes Konzept für die digitale Transformation in Unternehmen und ein umfassender Leitfaden für die Planung der Cloudeinführung in einer Produktionsumgebung. Das CAF enthält eine Schrittanleitung zum [Einrichten von Azure](/azure/cloud-adoption-framework/ready/azure-setup-guide/), damit Sie es schnell und sicher in Betrieb nehmen können. Eine interaktive Version finden Sie im [Azure-Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade). Sie finden dort Beispielarchitekturen, spezielle Best Practices für die Bereitstellung von Anwendungen sowie kostenlose Trainingsressourcen, mit denen Sie die notwendigen Fachkenntnisse zu Azure erhalten.

### <a name="considerations-for-migrations"></a>Überlegungen zu Migrationen

Bei der Migration von Dateidaten zu Azure sind mehrere wichtige Aspekte zu berücksichtigen. Bevor Sie fortfahren, erfahren Sie mehr:

- [Übersicht über die Azure Storage-Migration](../../../common/storage-migration-overview.md)
- Aktueller Stand der Features, die von Komprise Intelligent Data Management unterstützt werden, in der [Vergleichsmatrix für Migrationstools](./migration-tools-comparison.md).

Denken Sie daran, dass Sie genügend Netzwerkkapazität benötigen, um Migrationen zu unterstützen, ohne Produktionsanwendungen zu beeinträchtigen. In diesem Abschnitt werden die Tools und Techniken beschrieben, die für die Bewertung Ihrer Netzwerkanforderungen verfügbar sind.

#### <a name="determine-unutilized-internet-bandwidth"></a>Nicht ausgelastete Internetbandbreite ermitteln

Es ist wichtig zu wissen, wie viel Spielraum an ungenutzter Bandbreite (oder welchen *Toleranzbereich*) Sie täglich haben. Auf diese Weise können Sie bewerten, ob Sie Ihre Ziele für Folgendes erreichen können:

- Anfangszeit für Migrationen, wenn Sie nicht Azure Data Box für die Offlinemethode verwenden
- erforderliche Zeit für die inkrementelle Neusynchronisierung vor dem endgültigen Wechsel zum Zieldateidienst

Verwenden Sie die folgenden Methoden, um den Spielraum an Bandbreite zu ermitteln, der für Azure zur Verfügung steht.

- Wenn Sie ein vorhandener Azure ExpressRoute-Kunde sind, können Sie die [Inanspruchnahme](../../../../expressroute/expressroute-monitoring-metrics-alerts.md#circuits-metrics) im Azure-Portal anzeigen.
- Wenden Sie sich an Ihren ISP, und fordern Sie Berichte an, die Ihre vorhandene tägliche und monatliche Nutzung zeigen.
- Es gibt eine Reihe von Tools, die durch Überwachung des Netzwerkdatenverkehrs auf Ebene des Routers bzw. Switches die Nutzung messen können:
  - [SolarWinds Bandwidth Analyzer Pack](https://www.solarwinds.com/network-bandwidth-analyzer-pack?CMP=ORG-BLG-DNS)
  - [Paessler PRTG](https://www.paessler.com/bandwidth_monitoring)
  - [Cisco Network Assistant](https://www.cisco.com/c/en/us/products/cloud-systems-management/network-assistant/index.html)
  - [WhatsUp Gold](https://www.whatsupgold.com/network-traffic-monitoring)

## <a name="migration-planning-guide"></a>Leitfaden zur Migrationsplanung

Komprise lässt sich einfach einrichten und ermöglicht das gleichzeitige Ausführen mehrerer Migrationen in drei Schritten:

1.  Analysieren Ihrer Daten zur Ermittlung der Dateien, die migriert oder archiviert werden sollen
1.  Definieren von Richtlinien zum Migrieren, Verschieben oder Kopieren unstrukturierter Daten in Azure Storage
1.  Aktivieren von Richtlinien, mit denen Ihre Daten automatisch verschoben werden

Der erste Schritt ist entscheidend, wenn es um das Ermitteln und Priorisieren der richtigen Daten für die Migration geht. Bei der Analyse von Komprise wird Folgendes angegeben:

- Informationen zur Zugriffszeit für die Ermittlung:
  - Dateien mit geringerer Zugriffshäufigkeit, die Sie lokal zwischenspeichern oder unter dem schnellen Dateidienst speichern können
  - Kalte Daten, die Sie im Blobspeicher archivieren können
- Informationen zu den wichtigsten Benutzern, Gruppen oder Freigaben, um die Reihenfolge der Migration zu ermitteln, und zur am stärksten betroffenen Gruppe der Organisation, um die geschäftlichen Auswirkungen zu bewerten
- Anzahl von Dateien oder Kapazität pro Dateityp, um den Typ der gespeicherten Dateien zu ermitteln, und ob Möglichkeiten zur Bereinigung des Inhalts vorhanden sind. Durch die Bereinigung wird der Migrationsaufwand reduziert, und die Kosten für den Zielspeicher werden gesenkt.
- Anzahl von Dateien oder Kapazität pro Dateigröße, um die Dauer der Migration zu ermitteln. Die Migration einer großen Anzahl von kleinen Dateien dauert länger als bei einer geringen Anzahl von großen Dateien.

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-analyze-1.png" alt-text="Analyse nach Dateityp und Zugriffszeit":::

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-analyze-shares.png" alt-text="Beispiel für Freigabenanalyse":::

- Filter für Funktionen benutzerdefinierter Abfragen, damit Sie genau die benötigten Dateien erhalten

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-analyze-custom.png" alt-text="Analyse für benutzerdefinierte Abfrage":::

## <a name="deployment-guide"></a>Bereitstellungshandbuch

Vor der Bereitstellung von Komprise muss der Zieldienst bereitgestellt werden. Weitere Informationen finden Sie hier:

- Erstellen einer [Azure-Dateifreigabe](../../../files/storage-how-to-create-file-share.md)
- Erstellen eines [SMB-Volumes](../../../../azure-netapp-files/azure-netapp-files-create-volumes-smb.md) oder [NFS-Exports](../../../../azure-netapp-files/azure-netapp-files-create-volumes.md) in Azure NetApp Files

Das Komprise Grid wird in einer virtuellen Umgebung (Hyper-V, VMware, KVM) bereitgestellt und dient zur Verbesserung der Geschwindigkeit, Skalierbarkeit und Resilienz. Alternativ können Sie die Umgebung unter Ihrem Azure-Abonnement über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management) einrichten.

1. Öffnen Sie das Azure-Portal, und suchen Sie nach **Speicherkonten**. 

    :::image type="content" source="./media/komprise-quick-start-guide/azure-locate-storage-account.png" alt-text="Zeigt, wo Sie im Suchfeld des Azure-Portals das Wort „Speicher“ eingegeben haben.":::

    Sie können auch auf das standardmäßige Symbol **Speicherkonten** klicken.

    :::image type="content" source="./media/komprise-quick-start-guide/azure-portal.png" alt-text="Zeigt das Hinzufügen von Speicherkonten im Azure-Portal.":::

2. Wählen Sie **Erstellen** aus, um ein Konto hinzuzufügen:
   1. Wählen Sie eine vorhandene Ressourcengruppe aus, oder klicken Sie auf **Neu erstellen**.
   2. Geben Sie einen eindeutigen Namen für Ihr Speicherkonto an.
   3. Wählen Sie die Region aus.
   4. Wählen Sie je nach Ihren Anforderungen die Leistungsoption **Standard** oder **Premium** aus. Wenn Sie **Premium** auswählen, klicken Sie unter **Premium account type** (Premium-Kontotyp) auf **Dateifreigaben**.
   5. Wählen Sie die **[Redundanz](../../../common/storage-redundancy.md)** aus, die Ihre Datenschutzanforderungen erfüllt.
   
   :::image type="content" source="./media/komprise-quick-start-guide/azure-account-create-1.png" alt-text="Zeigt die Speicherkontoeinstellungen im Portal.":::

3. Wir empfehlen, die Standardeinstellungen aus dem Bildschirm **Erweitert** zu übernehmen. Bei der Migration zu Azure Files empfiehlt es sich, **Große Dateifreigaben** zu aktivieren (sofern verfügbar).

   :::image type="content" source="./media/komprise-quick-start-guide/azure-account-create-2.png" alt-text="Zeigt die Registerkarte „Erweiterte Einstellungen“ im Portal an.":::

4. Behalten Sie die standardmäßigen Netzwerkoptionen zunächst bei, und fahren Sie mit **Datenschutz** fort. Sie können die Option „Vorläufiges Löschen“ aktivieren, die es Ihnen ermöglicht, versehentlich gelöschte Daten innerhalb des definierten Aufbewahrungszeitraums wiederherzustellen. Diese Option bietet Schutz vor versehentlichen oder böswilligen Löschvorgängen.

   :::image type="content" source="./media/komprise-quick-start-guide/azure-account-create-3.png" alt-text="Zeigt die Datenschutzeinstellungen im Portal an.":::

5. Fügen Sie ggf. Tags zum Organisieren hinzu, und klicken Sie auf **Erstellen**, um Ihr Konto zu erstellen.
 
6. Nun sind nur noch zwei einfache Schritte nötig, um das Konto in Ihre Komprise-Umgebung aufzunehmen. Navigieren Sie im Azure-Portal zu dem soeben erstellten Konto, und wählen Sie im Dateidienstmenü die Option „Dateifreigaben“ aus. Fügen Sie eine Dateifreigabe hinzu, und wählen Sie einen aussagekräftigen Namen aus. Navigieren Sie dann in den Einstellungen zu Zugriffsschlüssel, und kopieren Sie den Namen des Speicherkontos und einen der beiden Zugriffsschlüssel. Klicken Sie auf **Schlüssel anzeigen**, falls die Schlüssel nicht angezeigt werden.

   :::image type="content" source="./media/komprise-quick-start-guide/azure-access-key.png" alt-text="Zeigt die Einstellungen für Zugriffsschlüssel im Portal an.":::

7. Navigieren Sie zu den **Eigenschaften** der Azure-Dateifreigabe. Notieren Sie sich die URL-Adresse. Sie benötigen sie zum Hinzufügen der Azure-Verbindung zur Komprise-Zieldateifreigabe:

   :::image type="content" source="./media/komprise-quick-start-guide/azure-files-endpoint.png" alt-text="Suchen Sie nach dem Azure Files-Endpunkt.":::

8. _(Optional)_ Sie können Ihrer Bereitstellung zusätzliche Sicherheitsschichten hinzufügen.
 
   1. Konfigurieren Sie den rollenbasierten Zugriff, um den Kreis der Personen einzuschränken, die Änderungen an Ihrem Speicherkonto vornehmen dürfen. Weitere Informationen finden Sie unter [Integrierte Rollen für Verwaltungsvorgänge](../../../common/authorization-resource-provider.md#built-in-roles-for-management-operations).
 
   2.  Beschränken Sie den Kontozugriff mithilfe der [Einstellungen für die Storage-Firewall](../../../common/storage-network-security.md) auf bestimmte Netzwerksegmente. Konfigurieren Sie Firewalleinstellungen, um Zugriff von außerhalb Ihres Unternehmensnetzwerks zu verhindern.

       :::image type="content" source="./media/komprise-quick-start-guide/azure-storage-firewall.png" alt-text="Zeigt Storage Firewall-Einstellungen im Portal an.":::

   3.  Legen Sie eine [Löschsperre](../../../../azure-resource-manager/management/lock-resources.md) für das Storage-Konto fest, um ein versehentliches Löschen des Kontos zu verhindern.

       :::image type="content" source="./media/komprise-quick-start-guide/azure-resource-lock.png" alt-text="Zeigt das Festlegen einer Löschsperre im Portal.":::

   4.  Konfigurieren Sie zusätzliche [bewährte Sicherheitsmethoden](../../../blobs/security-recommendations.md).

1.  Führen Sie den **Download** des virtuellen Komprise Observer-Geräts aus Director durch, stellen Sie es auf Ihrem Hypervisor bereit, und konfigurieren Sie es mit den Angaben für das Netzwerk und die Domäne. Director wird als Clouddienst bereitgestellt, der von Komprise verwaltet wird. Die für den Zugriff auf Director benötigten Informationen werden in der Begrüßungs-E-Mail gesendet, die Sie nach dem Erwerb der Lösung erhalten.

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-setup-1.png" alt-text="Download des entsprechenden Images für Komprise Observer aus Director":::

1. Es gibt zwei Möglichkeiten, die Freigaben für die Analyse und die Migration hinzuzufügen:
   1. **Ermitteln** Sie alle Freigaben Ihrer Speicherumgebung, indem Sie Folgendes eingeben:
       - Plattform für den Quell-NAS
       - Hostname oder IP-Adresse
       - `Display name`
       - Anmeldeinformationen (für SMB-Freigaben)

        :::image type="content" source="./media/komprise-quick-start-guide/komprise-setup-2.png" alt-text="Angeben des zu ermittelnden NAS-Systems":::

    1. Geben Sie zum **Angeben** einer Dateifreigabe Folgendes ein:
       - Speicherinformationen
       - Protocol
       - Pfad
       - Anzeigename
       - Anmeldeinformationen (für SMB-Freigaben)
  
        :::image type="content" source="./media/komprise-quick-start-guide/komprise-setup-3.png" alt-text="Angeben von zu ermittelnden NAS-Lösungen":::

    Dieser Schritt muss wiederholt werden, um weitere Quell- und Zielfreigaben hinzuzufügen. Zum Hinzufügen von Azure Files als Ziel müssen Sie die Details zum Azure-Speicherkonto und zur Dateifreigabe angeben:

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-azure-files-1.png" alt-text="Auswählen von Azure Files als Zieldienst":::

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-azure-files-2.png" alt-text="Eingeben von Details für Azure Files":::

## <a name="migration-guide"></a>Migrationsleitfaden

Komprise ermöglicht eine Livemigration, bei der Endbenutzer und Anwendungen nicht gestört werden und der Zugriff auf die Daten während der Migration weiterhin erfolgen kann. Beim Migrationsprozess wird die Migration von Verzeichnissen, Dateien und Links von einer Quelle zu einem Ziel automatisiert. Bei jedem Schritt wird die Datenintegrität überprüft. Alle Attribute, Berechtigungen und Zugriffssteuerungen der Quelle werden angewendet.

Führen Sie die folgenden Schritte aus, um eine Migration zu konfigurieren und auszuführen:

1. Melden Sie sich bei Ihrer Komprise-Konsole an. Die für den Zugriff auf die Konsole benötigten Informationen werden in der Begrüßungs-E-Mail gesendet, die Sie nach dem Erwerb der Lösung erhalten.
1. Navigieren Sie zu **Migrieren,** , und klicken Sie auf **Migration hinzufügen**.

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-new-migrate.png" alt-text="Hinzufügen eines neuen Migrationsauftrags":::

1. Fügen Sie eine Migrationsaufgabe hinzu, indem Sie die richtige Quell- und Zielfreigabe auswählen. Geben Sie einen Migrationsnamen an. Klicken Sie nach der Konfiguration auf **Migration starten**. 
   
   :::image type="content" source="./media/komprise-quick-start-guide/komprise-add-migration.png" alt-text="Angeben von Details für den Migrationsauftrag":::

   (_Optional_) Legen Sie fest, ob die Zugriffszeit und die SMB-ACLs auf dem Ziel beibehalten werden sollen. Diese Option hängt vom ausgewählten Dateidienst und Protokoll für die Quelle und das Ziel ab.

2. Nachdem die Migration gestartet wurde, können Sie **Migrieren** auswählen, um den Status zu überwachen.

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-monitor-migrations.png" alt-text="Überwachen aller Migrationsaufträge":::

3. Führen Sie nach Abschluss der Migration aller Änderungen eine abschließende Migration durch, indem Sie auf **Actions** (Aktionen) klicken und die Option **Start final iteration** (Letzte Iteration starten) auswählen. Wir empfehlen Ihnen, vor der abschließenden Migration den Zugriff auf Quelldateifreigaben zu beenden oder diese auf den schreibgeschützten Modus (für Benutzer und Anwendungen) umzustellen. Mit diesem Schritt wird sichergestellt, dass auf der Quelle keine Änderungen vorgenommen werden.

    :::image type="content" source="./media/komprise-quick-start-guide/komprise-final-migration.png" alt-text="Durchführen einer abschließenden Migration vor der Umstellung":::

    Führen Sie nach Beendigung der abschließenden Migration für alle Benutzer und Anwendungen die Umstellung auf die Zielfreigabe durch. Für die Umstellung auf den neuen Dateidienst muss normalerweise die Konfiguration von DNS- oder DFS-Servern geändert werden, oder es müssen die Bereitstellungspunkte an das neue Ziel angepasst werden. 

4. Führen Sie nun den letzten Schritt aus, indem Sie die Migration als abgeschlossen kennzeichnen.

## <a name="support"></a>Support

Melden Sie sich zum Erstellen eines Supportfalls für Komprise bei der [Supportwebsite von Komprise](https://komprise.freshdesk.com/) an.

## <a name="marketplace"></a>Marketplace

Sehen Sie sich den Komprise-Eintrag auf dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview) an.

## <a name="next-steps"></a>Nächste Schritte

Sie können verschiedene Ressourcen nutzen, um mehr zu erfahren:

- [Übersicht über die Azure Storage-Migration](../../../common/storage-migration-overview.md)
- Von Komprise Intelligent Data Management unterstützte Features in der [Vergleichsmatrix für Migrationstools](./migration-tools-comparison.md)
- [Komprise-Kompatibilitätsmatrix](https://www.komprise.com/partners/microsoft-azure/)
