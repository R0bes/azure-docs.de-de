---
title: Bewährte Methoden für Azure Sentinel
description: Erfahren Sie mehr über bewährte Methoden für die Verwaltung Ihres Azure Sentinel-Arbeitsbereichs.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 07/21/2021
ms.openlocfilehash: 570dccac5b7ddae323ce37dfbfbf850a54d576d6
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342927"
---
# <a name="best-practices-for-azure-sentinel"></a>Bewährte Methoden für Azure Sentinel

Diese Sammlung bewährter Methoden enthält Anleitungen für die Bereitstellung, Verwaltung und Verwendung von Azure Sentinel, einschließlich Links zu anderen Artikeln, um weitere Informationen zu erhalten.

> [!IMPORTANT]
> Bevor Sie Azure Sentinel bereitstellen, überprüfen und schließen Sie [Aktivitäten und Voraussetzungen vor der Bereitstellung](prerequisites.md) ab.
>


## <a name="regular-soc-activities-to-perform"></a>Durchzuführende reguläre SOC-Aktivitäten

Planen Sie die folgenden Azure Sentinel-Aktivitäten regelmäßig, um kontinuierlich bewährte Sicherheitsmethoden sicherzustellen:

### <a name="daily-tasks"></a>Tägliche Aufgaben

- **Selektieren und Untersuchen von Incidents**.  Überprüfen Sie die Seite **Incidents** von Azure Sentinel, um auf neue Incidents zu prüfen, die von den derzeit konfigurierten Analyseregeln generiert werden, und beginnen Sie mit der Untersuchung neuer Incidents. Weitere Informationen finden Sie im [Tutorial: Untersuchen von Incidents mit Azure Sentinel](investigate-cases.md).

- **Untersuchen Sie Hunting-Abfragen und Textmarken**. Untersuchen Sie die Ergebnisse für alle integrierten Abfragen, und aktualisieren Sie vorhandene Hunting-Abfragen und Textmarken. Generieren Sie manuell neue Incidents, oder aktualisieren Sie ggf. alte Incidents.  Weitere Informationen finden Sie unter:

    - [Automatisches Erstellen von Incidents aus Microsoft-Sicherheitswarnungen](create-incidents-from-alerts.md)
    - [Suchen nach Bedrohungen mit Azure Sentinel](hunting.md)
    - [Behalten des Überblicks über Daten beim Hunting mit Azure Sentinel](bookmarks.md)

- **Analyseregeln**.  Überprüfen und aktivieren Sie ggf. neue Analyseregeln, einschließlich neu veröffentlichter oder neu verfügbarer Regeln von kürzlich verbundenen Datenconnectors.

- **Datenconnectors**. Überprüfen Sie den Status, das Datum und die Uhrzeit des letzten Protokolls, das von jedem Datenconnector empfangen wurde, um sicherzustellen, dass Daten fließen. Überprüfen Sie, ob neue Connectors verfügbar sind, und überprüfen Sie die Erfassung, um sicherzustellen, dass die festgelegten Grenzwerte nicht überschritten wurden. Weitere Informationen finden Sie unter [Bewährte Methoden für Datensammlungen](best-practices-data.md) und [Verbinden von Datenquellen](connect-data-sources.md).

- **Log Analytics-Agent** Stellen Sie sicher, dass Server und Arbeitsstationen aktiv mit dem Arbeitsbereich verbunden sind, und behandeln und beheben Sie Verbindungsfehler.   Weitere Informationen finden Sie unter [Log Analytics – Übersicht über Agents](../azure-monitor/agents/log-analytics-agent.md).

- **Playbookfehler**. Überprüfen Sie den Status der Playbook-Ausführung, und beheben Sie Fehler.   Weitere Informationen finden Sie unter [Tutorial: Verwenden von Playbooks mit Automatisierungsregeln in Azure Sentinel](tutorial-respond-threats-playbook.md).


### <a name="weekly-tasks"></a>Wöchentliche Aufgaben

- **Arbeitsmappenaktualisierungen**. Überprüfen Sie, ob Arbeitsmappen über Aktualisierungen verfügen, die installiert werden müssen. Weitere Informationen finden Sie unter [Häufig verwendete Azure Sentinel-Arbeitsmappen](top-workbooks.md).

- **Azure Sentinel: GitHub-Repositorybewertung** Überprüfen Sie das [Azure Sentinel GitHub](https://github.com/Azure/Azure-Sentinel)-Repository, um zu untersuchen, ob neue oder aktualisierte Ressourcen für Ihre Umgebung vorhanden sind, z. B. Analyseregeln, Arbeitsmappen, Hunting-Abfragen oder Playbooks.

- **Azure Sentinel-Überwachung**. Überprüfen Sie Azure Sentinel-Aktivitäten, um zu ermitteln, wer Ressourcen aktualisiert oder gelöscht hat, z. B. Analyseregeln, Textmarken usw. Weitere Informationen finden Sie unter Überwachen von [Überwachen von Azure Sentinel-Abfragen und -Aktivitäten](audit-sentinel-data.md).

### <a name="monthly-tasks"></a>Monatliche Aufgaben

- **Überprüfen Sie den Benutzerzugriff**. Überprüfen Sie Berechtigungen für Ihre Benutzer, und prüfen Sie auf inaktive Benutzer. Weitere Informationen hierzu finden Sie unter [Berechtigungen in Azure Sentinel](roles.md).

- **Überprüfen des Log Analytics-Arbeitsbereichs**. Überprüfen Sie, ob die Datenaufbewahrungsrichtlinie des Log Analytics-Arbeitsbereichs weiterhin mit der Richtlinie Ihrer Organisation in Einklang steht.  Weitere Informationen finden Sie unter [Datenaufbewahrungsrichtlinie](/workplace-analytics/privacy/license-expiration) und [Integrieren von Azure Data Explorer für die langfristige Protokollaufbewahrung](store-logs-in-azure-data-explorer.md).


## <a name="integrate-with-microsoft-security-services"></a>Integrieren in Microsoft-Sicherheitsdienste

Azure Sentinel wird durch die Komponenten unterstützt, die Daten an Ihren Arbeitsbereich senden, und durch Integrationen in andere Microsoft-Dienste gestärkt. Alle Protokolle, die in Produkten wie Microsoft Cloud App Security, Microsoft Defender für Endpunkt und Microsoft Defender for Identity erfasst werden, ermöglichen es diesen Diensten, Erkennungen zu erstellen und diese Erkennungen dann für Azure Sentinel bereitzustellen. Protokolle können ebenfalls direkt in der Azure Sentinel erfasst werden, um ein vollständigeres Bild von Ereignissen und Incidents zu erhalten.

Die folgende Abbildung zeigt z. B., wie Azure Sentinel Daten von anderen Microsoft-Diensten und Multi-Cloud- und Partnerplattformen erfasst, um Abdeckung für Ihre Umgebung bereitzustellen:

:::image type="content" source="media/best-practices/azure-sentinel-and-other-services.png" alt-text="Azure Sentinel-Integration in andere Microsoft- und Partnerdienste":::

Neben der Erfassung von Warnungen und Protokollen aus anderen Quellen gilt für Azure Sentinel auch:

- **Verwendet die Informationen, die erfasst werden, mit [maschinellem Lernen](bring-your-own-ml.md)** , um bessere Ereigniskorrelation, Warnungsaggregation, Anomalieerkennung und vieles mehr zu ermöglichen.
- **Erstellt und präsentiert interaktive visuelle Elemente über [Arbeitsmappen](get-visibility.md)** und zeigt Trends, verwandte Informationen und wichtige Daten an, die sowohl für Administratoraufgaben als auch für Untersuchungen verwendet werden.
- **Führt [Playbooks](tutorial-respond-threats-playbook.md) aus, um auf Warnungen zu reagieren**, Informationen zu sammeln, Aktionen für Elemente durchzuführen und Benachrichtigungen an verschiedene Plattformen zu senden.
- **Integration in Partnerplattformen** wie ServiceNow und Jira, um wichtige Dienste für SOC-Teams zu bieten.
- **Erfassung und Abruf von Anreicherungsfeeds** von [Threat Intelligence-Plattformen](threat-intelligence-integration.md), um wertvolle Daten für die Untersuchung zu erhalten.

##  <a name="manage-and-respond-to-incidents"></a>Anzeigen von und Reagieren auf Incidents

Die folgende Abbildung zeigt die empfohlenen Schritte in einem Incident Management- und Reaktionsprozess.

:::image type="content" source="media/best-practices/incident-handling.png" alt-text="Incident Management-Prozess: Selektierung. Vorbereitung. Korrektur. Beseitigung. Aktivitäten nach Incidents.":::

Die folgenden Abschnitte enthalten allgemeine Beschreibungen zur Verwendung der Azure Sentinel-Funktionen für Incident Management und Reaktion während des gesamten Prozesses. Weitere Informationen finden Sie im [Tutorial: Untersuchen von Incidents mit Azure Sentinel](investigate-cases.md).

### <a name="use-the-incidents-page-and-the-investigation-graph"></a>Verwenden der Seite „Incidents“ und des Untersuchungsdiagramms

Starten Sie alle Selektierungsprozesse für neue Incidents auf der Seite **Incidents** in Azure Sentinel sowie das **Untersuchungsdiagramm**. 

Entdecken Sie wichtige Entitäten wie Konten, URLs, IP-Adressen, Hostnamen, Aktivitäten, Zeitachsen und vieles mehr. Verwenden Sie diese Daten, um zu verstehen, ob ein [falsch positives](false-positives.md) Ergebnis vorliegt. In diesem Fall können Sie den Incident direkt schließen.

Alle generierten Incidents werden auf der Seite **Incidents** angezeigt, die als zentraler Ort für die Selektierung und frühe Untersuchung dient. Auf der Seite **Incidents** werden der Titel, der Schweregrad und die zugehörigen Warnungen, Protokolle und relevanten Entitäten aufgeführt. Incidents bieten auch einen schnellen Einblick in gesammelte Protokolle und alle Tools im Zusammenhang mit dem Incident.

Die Seite **Incidents** arbeitet mit dem **Untersuchungsdiagramm** zusammen, einem interaktiven Tool, mit dem Benutzer eine Warnung untersuchen und genau analysieren können, um den gesamten Umfang eines Angriffs zu erkennen. Benutzer können dann eine Zeitachse von Ereignissen erstellen und das Ausmaß einer Bedrohungskette entdecken.

Wenn Sie feststellen, dass der Incident True Positive ist, können Sie direkt auf der Seite **Incidents** Maßnahmen ergreifen, um Protokolle, Entitäten und die Bedrohungskette zu untersuchen. Nachdem Sie die Bedrohung identifiziert und einen Aktionsplan erstellt haben, verwenden Sie andere Tools in Azure Sentinel und [andere Microsoft-Sicherheitsdienste](best-practices.md#integrate-with-microsoft-security-services), um die Untersuchung fortzusetzen.


### <a name="handle-incidents-with-workbooks"></a>Behandeln von Incidents mit Arbeitsmappen

Neben der [Visualisierung und Anzeige von Informationen und Trends](get-visibility.md) sind Azure Sentinel-Arbeitsmappen wertvolle Tools.

Verwenden Sie beispielsweise die Arbeitsmappe [Erkenntnisse aus Ermittlungen](top-workbooks.md#investigation-insights), um bestimmte Incidents zusammen mit allen zugeordneten Entitäten und Warnungen zu untersuchen. Mit dieser Arbeitsmappe können Sie Entitäten eingehender analysieren, indem Sie zugehörige Protokolle, Aktionen und Warnungen anzeigen.

### <a name="handle-incidents-with-threat-hunting"></a>Behandeln von Incidents mit Bedrohungssuche

Führen Sie beim Untersuchen und Suchen nach Ursachen integrierte Bedrohungssucheabfragen aus, und überprüfen Sie die Ergebnisse auf Anzeichen einer Gefährdung.

Verwenden Sie den [Livestream](livestream.md) während einer Untersuchung oder nachdem Sie Schritte zur Behebung und Beseitigung der Bedrohung unternommen haben, um in Echtzeit zu überwachen, ob noch böswillige Ereignisse vorhanden sind oder diese noch andauern.

### <a name="handle-incidents-with-entity-behavior"></a>Behandeln von Incidents mit Entitätsverhalten

Entitätsverhalten in Azure Sentinel ermöglicht Benutzern das Überprüfen und Untersuchen von Aktionen und Warnungen für bestimmte Entitäten, z. B. das Untersuchen von Konten und Hostnamen. Weitere Informationen finden Sie unter:

- [Aktivieren von User and Entity Behavior Analytics (UEBA) in Azure Sentinel](enable-entity-behavior-analytics.md)
- [Untersuchen von Incidents mit UEBA-Daten](investigate-with-ueba.md)
- [Referenz zu Azure Sentinel-UEBA-Anreicherungen](ueba-enrichments.md)

### <a name="handle-incidents-with-watchlists-and-threat-intelligence"></a>Behandeln von Incidents mit Watchlists und Threat Intelligence

Um Erkennungen zu maximieren, die auf Threat Intelligence basieren, stellen Sie sicher, dass Sie [Threat Intelligence-Datenconnectors](connect-threat-intelligence-tip.md) verwenden, um Anzeichen für Kompromittierung zu erfassen:

- Verbinden von Datenquellen, die für die [Fusion](fusion.md)- und [TI Map](./understand-threat-intelligence.md)-Warnungen erforderlich sind
- Erfassen von Indikatoren von [TAXII- und TIP-Plattformen](./connect-threat-intelligence-tip.md)

Verwenden Sie Indikatoren für Kompromittierung in Analyseregeln, bei der Bedrohungssuche, beim Untersuchen von Protokollen oder Generieren von weiteren Incidents.

Verwenden Sie eine Watchlist, die Daten aus den erfassten Daten und externen Quellen kombiniert, z. B. Anreicherungsdaten. Erstellen Sie beispielsweise Listen mit IP-Adressbereichen, die von Ihrer Organisation oder kürzlich ausgeschiedenen Mitarbeitern verwendet werden. Verwenden Sie Watchlists mit Playbooks, um Anreicherungsdaten zu sammeln und schädliche IP-Adressen beispielsweise Watchlists hinzuzufügen, die während der Erkennung, Bedrohungssuche und bei Untersuchungen verwendet werden.

Verwenden Sie während eines Incidents Watchlists mit Untersuchungsdaten, und löschen Sie diese dann, wenn Ihre Untersuchung abgeschlossen ist, um sicherzustellen, dass vertrauliche Daten nicht angezeigt werden.


## <a name="additional-best-practice-references"></a>Zusätzliche Referenzen für bewährte Methoden

In der Azure Sentinel-Dokumentation finden Sie weitere Anleitungen zu bewährten Methoden in verschiedenen Artikeln. Weitere Informationen finden Sie beispielsweise in den folgenden Artikeln:

- **Administratorbenutzer**:

    - [Aktivitäten vor der Bereitstellung und Voraussetzungen für die Bereitstellung von Azure Sentinel](prerequisites.md)
    - [Bewährte Methoden für die Azure Sentinel-Arbeitsbereichsarchitektur](best-practices-workspace-architecture.md)
    - [Entwerfen der Azure Sentinel-Arbeitsbereichsarchitektur](design-your-workspace-architecture.md)
    - [Beispiele für Azure Sentinel-Arbeitsbereichsentwürfe](sample-workspace-designs.md)
    - [Bewährte Methoden für die Datensammlung](best-practices-data.md)
    - [Azure Sentinel: Kosten und Abrechnung](azure-sentinel-billing.md)
    - [Berechtigungen in Azure Sentinel](roles.md)
    - [Schützen des geistigen Eigentums von MSSPs in Azure Sentinel](mssp-protect-intellectual-property.md)
    - [Threat Intelligence-Integration in Azure Sentinel](threat-intelligence-integration.md)
    - [Überwachen von Azure Sentinel-Abfragen und -Aktivitäten](audit-sentinel-data.md)

- **Analysten**:

    - [Empfohlene Playbooks](automate-responses-with-playbooks.md#recommended-playbooks)
    - [Behandeln falsch positiver Ergebnisse in Azure Sentinel](false-positives.md)
    - [Suchen nach Bedrohungen mit Azure Sentinel](hunting.md)
    - [Häufig verwendete Azure Sentinel-Arbeitsmappen](top-workbooks.md)
    - [Standardmäßig verfügbare Erkennung von Bedrohungen](detect-threats-built-in.md)
    - [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md)
    - [Aufspüren von Sicherheitsrisiken mit Jupyter Notebook](notebooks.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den ersten Schritten mit Azure Sentinel:

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](quickstart-onboard.md)
- [Einblick in Warnungen](get-visibility.md)