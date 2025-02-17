---
title: Beispiel für Azure Sentinel Arbeitsbereichsentwürfe | Microsoft-Dokumentation
description: Hier finden Sie mehrere Beispiele für Azure Sentinel-Architekturentwürfe mit mehreren Mandanten, Clouds oder Regionen.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 07/18/2021
ms.openlocfilehash: dca6beedfe8fa3d57674323490c2d79cf5aa048f
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342742"
---
# <a name="azure-sentinel-sample-workspace-designs"></a>Beispiele für Azure Sentinel-Arbeitsbereichsentwürfe

In diesem Artikel werden empfohlene Arbeitsbereichsentwürfe für Organisationen mit den folgenden Beispielanforderungen beschrieben:

- Mehrere Mandanten und Regionen, mit europäischen Anforderungen an die Datenhoheit
- Einzelner Mandant mit mehreren Clouds
- Mehrere Mandanten mit mehreren Regionen und zentralisierter Sicherheit

Bei den Beispielen in diesem Artikel wird die [Entscheidungsstruktur für den Entwurf eines Azure Sentinel-Arbeitsbereichs](design-your-workspace-architecture.md) angewendet, um den besten Arbeitsbereichsentwurf für Organisationen zu bestimmen: Weitere Informationen finden Sie unter [Bewährte Methoden für die Azure Sentinel-Arbeitsbereichsarchitektur](best-practices-workspace-architecture.md).

## <a name="sample-1-multiple-tenants-and-regions"></a>Beispiel 1: Mehrere Mandanten und Regionen

Die Contoso Corporation ist ein multinationales Unternehmen mit Hauptsitz in London. Contoso verfügt über Niederlassungen auf der ganzen Welt mit besonders wichtigen Standorten in New York City und Tokio. Vor Kurzem hat Contoso seine Productivity Suite zu Office 365 migriert, wobei viele Workloads zu Azure migriert wurden.

### <a name="contoso-tenants"></a>Contoso-Mandanten

Aufgrund einer Übernahme vor einigen Jahren verfügt Contoso über zwei Azure AD-Mandanten: `contoso.onmicrosoft.com` und `wingtip.onmicrosoft.com`. Jeder Mandant verfügt über eine eigene Office 365-Instanz und mehrere Azure-Abonnements, wie in der folgenden Abbildung dargestellt:

:::image type="content" source="media/best-practices/contoso-tenants.png" alt-text="Diagramm der Contoso-Mandanten, die jeweils über separate Abonnementsätze verfügen." border="false":::

### <a name="contoso-compliance-and-regional-deployment"></a>Compliance und regionale Bereitstellung bei Contoso

Contoso verfügt derzeit über Azure-Ressourcen, die in drei verschiedenen Regionen gehostet werden: „USA, Osten“, „EU, Norden“ und „Japan, Westen“. Darüber hinaus besteht die strikte Anforderung, alle in Europa generierten Daten innerhalb der Regionen „Europa“ zu behalten.

Beide Azure AD-Mandanten von Contoso verfügen über Ressourcen in allen drei Regionen: „USA, Osten“, „EU, Norden“ und „Japan, Westen“.

### <a name="contoso-resource-types-and-collection-requirements"></a>Ressourcentypen und Sammlungsanforderungen von Contoso

Contoso muss Ereignisse aus den folgenden Datenquellen sammeln:

-   Office 365
-   Anmeldungs- und Überwachungsprotokolle von Azure AD
-   Azure-Aktivität
-   Windows-Sicherheitsereignisse, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   Syslog, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   CEF, von mehreren lokalen Netzwerkgeräten wie Palo Alto, Cisco ASA und Cisco Meraki
-   Mehrere Azure-PaaS-Ressourcen, z. B. Azure Firewall, AKS, Key Vault, Azure Storage und Azure SQL
-   Cisco Umbrella

Die Azure-VMs befinden sich größtenteils in der Region „EU, Norden“, nur wenige sind in den Regionen „USA, Osten“ und „Japan, Westen“. Contoso verwendet Azure Defender für die Server auf allen Azure-VMs.

Contoso geht davon aus, dass etwa 300 GB pro Tag aus allen Datenquellen erfasst werden.

### <a name="contoso-access-requirements"></a>Zugriffsanforderungen bei Contoso

Die Azure-Umgebung von Contoso verfügt bereits über einen einzigen Log Analytics-Arbeitsbereich, der vom Operations-Team zum Überwachen der Infrastruktur verwendet wird. Dieser Arbeitsbereich befindet sich im Contoso-AAD-Mandanten in der Region „EU, Norden“ und wird zum Sammeln von Protokollen von Azure-VMs in allen Regionen verwendet. Derzeit werden ca. 50 GB pro Tag erfasst.

Das Operations-Team von Contoso benötigt Zugriff auf alle Protokolle, die sich derzeit im Arbeitsbereich befinden. Dies umfasst auch mehrere Datentypen, die nicht vom SOC benötigt werden, z. B. **Perf**, **InsightsMetrics**, **ContainerLog** und mehr. Das Operations-Team darf *keinen* Zugriff auf die neuen Protokolle haben, die in Azure Sentinel gesammelt werden.

### <a name="contosos-solution"></a>Die Lösung von Contoso

In den folgenden Schritten wird die [Entscheidungsstruktur für den Entwurf eines Azure Sentinel-Arbeitsbereichs](design-your-workspace-architecture.md) angewendet, um den besten Arbeitsbereichsentwurf für Contoso zu bestimmen:

1. Contoso verfügt bereits über einen Arbeitsbereich, d. h. dass wir zunächst Überlegungen über das Aktivieren von Azure Sentinel in demselben Arbeitsbereich anstellen können.

    Die Erfassung von Nicht-SOC-Daten liegt bei weniger als 100 GB/Tag, daher können wir mit [Schritt 2](design-your-workspace-architecture.md#step-2-keeping-data-in-different-azure-geographies) fortfahren und sicherstellen, dass die entsprechende Option in [Schritt 5](design-your-workspace-architecture.md#step-5-collecting-any-non-soc-data) ausgewählt wird.

1.  Contoso muss bestimmte gesetzliche Anforderungen befolgen, d. h. dass mindestens ein Azure Sentinel-Arbeitsbereich in Europa benötigt wird.

1.  Contoso verfügt über zwei verschiedene Azure AD-Mandanten und sammelt Daten aus Datenquellen auf Mandantenebene, z. B. Office 365 sowie Azure AD-Anmelde- und Überwachungsprotokolle. Daher benötigen wir mindestens einen Arbeitsbereich pro Mandant.

1.  Contoso benötigt keine [verbrauchsbasierte Kostenzuteilung](design-your-workspace-architecture.md#step-4-splitting-billing--charge-back), deshalb können wir mit [Schritt 5](design-your-workspace-architecture.md#step-5-collecting-any-non-soc-data) fortfahren.

1.  Contoso muss Nicht-SOC-Daten sammeln, aber es gibt keine Überlappung zwischen SOC- und Nicht-SOC-Daten. Die SOC-Daten umfassen darüber hinaus ca. 250 GB/Tag, sodass aus Kostengründen separate Arbeitsbereiche verwendet werden sollten.

1.  Die meisten VMs von Contoso befinden sich in der Region „Europa, Norden“. Dort verfügt Contoso bereits über einen Arbeitsbereich. Daher sind die Bandbreitenkosten in diesem Fall kein Problem.

1.  Contoso verfügt über ein einziges SOC-Team, das Azure Sentinel verwenden wird. Somit ist keine zusätzliche Trennung erforderlich.

1.  Alle Mitglieder des SOC-Teams von Contoso haben Zugriff auf alle Daten, sodass keine zusätzliche Trennung erforderlich ist.

Der entsprechende Azure Sentinel-Arbeitsbereichsentwurf für Contoso ist in der folgenden Abbildung dargestellt:

:::image type="content" source="media/best-practices/contoso-solution.png" alt-text="Diagramm der Contoso-Lösung mit einem separaten Arbeitsbereich für das Operations-Team" border="false":::

Die vorgeschlagene Lösung umfasst Folgendes:

- Einen separaten Log Analytics-Arbeitsbereich für das Operations-Team von Contoso. Dieser Arbeitsbereich enthält ausschließlich Daten, die von dem SOC-Team von Contoso nicht benötigt werden, z. B. die **Perf**-, **InsightsMetrics**- oder **ContainerLog**-Tabellen.

- Zwei Azure Sentinel-Arbeitsbereiche (einer in jedem Azure AD-Mandanten), um Daten von Office 365, Azure-Aktivität, Azure AD und allen Azure PaaS-Diensten zu erfassen.

- Alle anderen Daten, die aus lokalen Datenquellen stammen, können an einen der beiden Azure Sentinel-Arbeitsbereiche weitergeleitet werden.


## <a name="sample-2-single-tenant-with-multiple-clouds"></a>Beispiel 2: Einzelner Mandant mit mehreren Clouds

Fabrikam ist eine Organisation mit Hauptsitz in New York City und mehreren Niederlassungen in den USA. Fabrikam steht noch am Anfang der Cloudeinführung. Die Organisation muss im nächsten Schritt ihre erste Azure-Zielzone bereitstellen und die ersten Workloads migrieren. Fabrikam verfügt bereits über einige Workloads in AWS, die mit Azure Sentinel überwacht werden sollen.

### <a name="fabrikam-tenancy-requirements"></a>Mandantenanforderungen von Fabrikam

Fabrikam verfügt über einen einzelnen Azure AD-Mandanten.

### <a name="fabrikam-compliance-and-regional-deployment"></a>Compliance und regionale Bereitstellung bei Fabrikam

Es gibt keine besonderen Compliance-Anforderungen bei Fabrikam. Fabrikam verfügt über Ressourcen in mehreren Azure-Regionen in den USA, aber die regionsübergreifenden Bandbreitenkosten stellen kein großes Problem dar.

### <a name="fabrikam-resource-types-and-collection-requirements"></a>Ressourcentypen und Sammlungsanforderungen von Fabrikam

Fabrikam muss Ereignisse aus den folgenden Datenquellen sammeln:

-   Anmeldungs- und Überwachungsprotokolle von Azure AD
-   Azure-Aktivität
-   Sicherheitsereignisse, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   Windows-Ereignisse, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   Leistungsdaten, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   AWS CloudTrail
-   AKS-Überwachungs- und Leistungsprotokolle

### <a name="fabrikam-access-requirements"></a>Zugriffsanforderungen von Fabrikam

Das Operations-Team von Fabrikam muss auf Folgendes zugreifen:

-   Sicherheitsereignisse und Windows-Ereignisse, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   Leistungsdaten, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   AKS-Leistungs- (Container Insights) und Überwachungsprotokolle
-   Alle Azure-Aktivitätsdaten

Das SOC-Team von Fabrikam muss auf Folgendes zugreifen:
-   Anmeldungs- und Überwachungsprotokolle von Azure AD
-   Alle Azure-Aktivitätsdaten
-   Sicherheitsereignisse, sowohl aus lokalen als auch aus Azure-VM-Quellen
-   AWS CloudTrail-Protokolle
-   AKS-Überwachungsprotokolle
-   Das komplette Azure Sentinel-Portal

### <a name="fabrikams-solution"></a>Die Lösung von Fabrikam

In den folgenden Schritten wird die [Entscheidungsstruktur für den Entwurf eines Azure Sentinel-Arbeitsbereichs](design-your-workspace-architecture.md) angewendet, um den besten Arbeitsbereichsentwurf für Fabrikam zu bestimmen:

1.  Fabrikam besitzt noch keinen Arbeitsbereich, also fahren wir mit [Schritt 2](design-your-workspace-architecture.md#step-2-keeping-data-in-different-azure-geographies) fort.

1.  Fabrikam muss keine bestimmten gesetzlichen Anforderungen erfüllen. Fahren wir daher mit [Schritt 3](design-your-workspace-architecture.md#step-3-do-you-have-multiple-azure-tenants) fort.

1.  Fabrikam verfügt über eine Umgebung mit nur einem Mandanten. Fahren wir mit [Schritt 4](design-your-workspace-architecture.md#step-4-splitting-billing--charge-back) fort.

1.  Fabrikam muss die Gebühren nicht aufteilen. Fahren wir daher mit [Schritt 5](design-your-workspace-architecture.md#step-5-collecting-any-non-soc-data) fort.

1.  Fabrikam benötigt separate Arbeitsbereiche für das SOC- und das Operations-Team:

    Das Operations-Team von Fabrikam muss Leistungsdaten sowohl von VMs als auch von AKS sammeln. Da AKS auf Diagnoseeinstellungen basiert, kann das Team bestimmte Protokolle auswählen, die an bestimmte Arbeitsbereiche gesendet werden sollen. Fabrikam kann AKS-Überwachungsprotokolle an den Azure Sentinel-Arbeitsbereich und sämtliche AKS-Protokolle an einen separaten Arbeitsbereich senden, in dem Azure Sentinel nicht aktiviert ist. In dem Arbeitsbereich, in dem Azure Sentinel nicht aktiviert ist, wird Fabrikam die Container Insights-Lösung aktivieren.

    Für Windows-VMs kann Fabrikam den [Azure Monitoring Agent (AMA)](connect-windows-security-events.md#connector-options) nutzen, um Protokolle zu teilen, Sicherheitsereignisse an den Azure Sentinel-Arbeitsbereich zu senden sowie Leistungs- und Windows-Ereignisse an den Arbeitsbereich zu senden, in dem Azure Sentinel nicht aktiviert ist.

    Fabrikam hat sich dazu entschieden, überlappende Daten wie Sicherheitsereignisse und Azure-Aktivitätsereignisse in die Kategorie der SOC-Daten einzuordnen und diese Daten an den Azure Sentinel-Arbeitsbereich zu senden.

1.  Die Bandbreitenkosten stellen kein Problem für Fabrikam dar, deshalb fahren wir mit [Schritt 7](design-your-workspace-architecture.md#step-7-segregating-data-or-defining-boundaries-by-ownership) fort.

1.  Fabrikam hat sich bereits dazu entschieden, separate Arbeitsbereiche für das SOC- und das Operations-Team zu verwenden. Eine weitere Trennung ist nicht erforderlich.

1.  Fabrikam muss den Zugriff auf überlappende Daten, z. B. Sicherheitsereignisse und Azure-Aktivitätsereignisse, steuern können. Es gibt jedoch keine gesonderten Anforderungen auf Zeilenebene.

    Weder bei Sicherheitsereignissen noch bei Azure-Aktivitätsereignissen handelt es sich um benutzerdefinierte Protokolle, sodass Fabrikam RBAC auf Tabellenebene verwenden kann, um dem Operations-Team Zugriff auf diese beiden Tabellen zu gewähren.

Das daraus resultierende Azure Sentinel-Arbeitsbereichsentwurf für Fabrikam ist in der folgenden Abbildung dargestellt. Sie enthält der Einfachheit halber nur die wichtigsten Protokollquellen:

:::image type="content" source="media/best-practices/fabrikam-solution.png" alt-text="Diagramm der Fabrikam-Lösung mit einem separaten Arbeitsbereich für das Operations-Team" border="false" :::

Die vorgeschlagene Lösung umfasst Folgendes:

- Zwei separate Arbeitsbereiche in der Region „USA“: einer mit Azure Sentinel für das SOC-Team und ein weiterer ohne Azure Sentinel für das Operations-Team.

- Den [Azure Monitoring Agent (AMA)](connect-windows-security-events.md#connector-options), der verwendet wird, um zu bestimmen, welche Protokolle von Azure und den lokalen VMs an den jeweiligen Arbeitsbereich gesendet werden.

- Diagnoseeinstellungen, mit denen bestimmt wird, welche Protokolle von Azure-Ressourcen wie AKS an die einzelnen Arbeitsbereiche gesendet werden.

- Überlappende Daten, die an den Azure Sentinel-Arbeitsbereich gesendet werden. Dort wird dem Operations-Team bei Bedarf Zugriff über RBAC auf Tabellenebene gewährt.

## <a name="sample-3-multiple-tenants-and-regions-and-centralized-security"></a>Beispiel 3: Mehrere Mandanten und Regionen und zentralisierte Sicherheit

Adventure Works ist ein multinationales Unternehmen mit Hauptsitz in Tokio. Adventure Works verfügt über 10 verschiedene untergeordnete Entitäten, die sich in verschiedenen Ländern auf der ganzen Welt befinden.

Adventure Works ist Kunde von Microsoft 365 E5 und verfügt bereits über Workloads in Azure.

### <a name="adventure-works-tenancy-requirements"></a>Mandantenanforderungen von Adventure Works

Adventure Works verfügt über drei Azure AD-Mandanten, einen für jeden Kontinent, auf dem sich die untergeordneten Entitäten befinden: Asien, Europa und Afrika. Die Identitäten der verschiedenen untergeordneten Entitäten befinden sich im jeweiligen Mandanten des Kontinents, zu dem sie gehören. Japanische Benutzer befinden sich z. B. im Mandanten *Asien*, deutsche Benutzer im Mandanten *Europa* und ägyptische Benutzer im Mandanten *Afrika*.

### <a name="adventure-works-compliance-and-regional-requirements"></a>Compliance und regionale Anforderungen von Adventure Works

Adventure Works verwendet derzeit drei Azure-Regionen, die jeweils auf den Kontinent ausgerichtet sind, in dem sich die untergeordneten Entitäten befinden. Adventure Works muss keine strengen Compliance-Anforderungen einhalten.

### <a name="adventure-works-resource-types-and-collection-requirements"></a>Ressourcentypen und Sammlungsanforderungen von Adventure Works

Adventure Works muss die folgenden Datenquellen für jede untergeordnete Entität sammeln:

-   Anmeldungs- und Überwachungsprotokolle von Azure AD
-   Office 365-Protokolle
-   Microsoft 365 Defender for Endpoint-Rohprotokolle
-   Azure-Aktivität
-   Azure Defender
-   Azure PaaS-Ressourcen, z. B. aus Azure Firewall, Azure Storage, Azure SQL und Azure WAF
-   Sicherheits- und Windows-Ereignisse von Azure-VMs
-   CEF-Protokolle von lokalen Netzwerkgeräten

Die Azure-VMs sind zwar auf die drei Kontinente verteilt, aber Bandbreitenkosten stellen kein Problem dar.

### <a name="adventure-works-access-requirements"></a>Zugriffsanforderungen von Adventure Works

Adventure Works verfügt über ein zentralisiertes SOC-Team, das die Sicherheitsvorgänge aller untergeordneten Entitäten überwacht.

Adventure Works verfügt darüber hinaus über drei unabhängige SOC-Teams, eines für jeden Kontinent. Die SOC-Teams der Kontinente dürfen nur auf die Daten zugreifen, die in der jeweiligen Region generiert wurden, ohne dabei die Daten von anderen Kontinenten sehen zu können. Das SOC-Team aus Asien darf beispielsweise nur auf Daten von Azure-Ressourcen, die in Asien bereitgestellt wurden, auf AAD-Anmeldungen vom Asien-Mandanten und auf Defender for Endpoint-Protokolle des Asien-Mandanten zugreifen.

Jedes SOC-Team muss auf die gesamte Azure Sentinel-Benutzeroberfläche zugreifen können.

Das Operations-Team von Adventure Works arbeitet unabhängig und verfügt über eigene Arbeitsbereiche ohne Azure Sentinel.

### <a name="adventure-works-solution"></a>Die Lösung von Adventure Works

In den folgenden Schritten wird die [Entscheidungsstruktur für den Entwurf eines Azure Sentinel-Arbeitsbereichs](design-your-workspace-architecture.md) angewendet, um den besten Arbeitsbereichsentwurf für Adventure Works zu bestimmen:

1.  Das Operations-Team von Adventure Works verfügt bereits über eigene Arbeitsbereiche, fahren wir also mit [Schritt 2](design-your-workspace-architecture.md#step-2-keeping-data-in-different-azure-geographies) fort.

1.  Adventure Works muss keine bestimmten gesetzlichen Anforderungen erfüllen. Fahren wir daher mit [Schritt 3](design-your-workspace-architecture.md#step-3-do-you-have-multiple-azure-tenants) fort.

1.  Adventure Works verfügt über drei Azure AD-Mandanten und muss Datenquellen auf Mandantenebene erfassen, z. B. Office 365-Protokolle. Daher sollte Adventure Works mindestens einen Azure Sentinel-Arbeitsbereich für jeden Mandanten erstellen.

1.  Adventure Works muss die Gebühren nicht aufteilen. Fahren wir daher mit [Schritt 5](design-your-workspace-architecture.md#step-5-collecting-any-non-soc-data) fort.

1.  Da das Operations-Team von Adventure Works über eigene Arbeitsbereiche verfügt, werden alle in diesem Beispiel berücksichtigten Daten vom SOC-Team von Adventure Works verwendet.

1.  Die Bandbreitenkosten stellen kein Problem für Adventure Works dar, deshalb fahren wir mit [Schritt 7](design-your-workspace-architecture.md#step-7-segregating-data-or-defining-boundaries-by-ownership) fort.

1.  Adventure Works muss Daten nach Besitzer trennen, da das SOC-Team eines bestimmten Kontinents nur auf die Daten zugreifen darf, die für diesen Kontinent relevant sind. Jedes SOC-Team benötigt jedoch auch Zugriff auf das gesamte Azure Sentinel-Portal.

1.  Adventure Works muss den Datenzugriff nicht nach Tabellen steuern.

Das daraus resultierende Azure Sentinel-Arbeitsbereichsentwurf für Adventure Works ist in der folgenden Abbildung dargestellt. Sie enthält der Einfachheit halber nur die wichtigsten Protokollquellen:

:::image type="content" source="media/best-practices/adventure-works-solution.png" alt-text="Diagramm der Adventure Works-Lösung mit separaten Arbeitsbereichen für jeden Azure AD-Mandanten" border="false":::

Die vorgeschlagene Lösung umfasst Folgendes:

- Einen separaten Azure Sentinel-Arbeitsbereich für jeden Azure AD-Mandanten. Jeder Arbeitsbereich sammelt Daten im Zusammenhang mit seinem Mandanten für alle Datenquellen.

- Die SOC-Teams der Kontinente haben ausschließlich Zugriff auf den Arbeitsbereich ihrer eigenen jeweiligen Mandanten. So wird sichergestellt, dass jedes SOC-Team nur auf Protokolle zugreifen kann, die innerhalb der Mandantengrenze generiert wurden.

- Das zentrale SOC-Team kann mithilfe von Azure Lighthouse weiterhin über einen separaten Azure AD-Mandanten auf jede der verschiedenen Azure Sentinel-Umgebungen zugreifen. Auch wenn kein zusätzlicher Mandant verfügbar ist, kann das zentrale SOC-Team über Azure Lighthouse auf die Remote-Arbeitsbereiche zugreifen.

- Darüber hinaus kann das zentrale SOC-Team auch einen zusätzlichen Arbeitsbereich erstellen, in dem Artefakte gespeichert werden können, die vor den SOC-Teams der Kontinente verborgen bleiben sollen, oder in dem Daten aufgenommen werden, die für die SOC-Teams der Kontinente nicht relevant sind.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Schnellstart: Ausführen des Onboardings für Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Einblick in Warnungen](get-visibility.md)