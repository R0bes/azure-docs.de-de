---
title: Datenreferenz zur App Service-Überwachung
description: Wichtiges Referenzmaterial für die App Service-Überwachung
author: msangapu-msft
ms.topic: reference
ms.author: msangapu
ms.service: app-service
ms.custom: subject-monitoring
ms.date: 04/16/2021
ms.openlocfilehash: 330629c958e036db382bed63b9ae0b9642e14bad
ms.sourcegitcommit: 92dd25772f209d7d3f34582ccb8985e1a099fe62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114227876"
---
# <a name="monitoring-app-service-data-reference"></a>Datenreferenz zur App Service-Überwachung

Diese Referenz gilt für die Verwendung von Azure Monitor zur App Service-Überwachung. Ausführliche Informationen über das Erfassen und Analysieren von Überwachungsdaten für App Service finden Sie unter [App Service-Überwachung](monitor-app-service.md).

## <a name="metrics"></a>Metriken

In diesem Abschnitt werden alle automatisch für App Service erfassten Plattformmetriken aufgeführt.  

|Metriktyp | Ressourcenanbieter/Typnamespace<br/> und Link zu einzelnen Metriken |
|-------|-----|
| App Service-Pläne | [Microsoft.Web/serverfarms](../azure-monitor/essentials/metrics-supported.md#microsoftwebserverfarms)
| Web-Apps | [Microsoft.Web/sites](../azure-monitor/essentials/metrics-supported.md#microsoftwebsites) |
| Stagingslots | [Microsoft.Web/sites/slots](../azure-monitor/essentials/metrics-supported.md#microsoftwebsitesslots) 
| App Service-Umgebung | [Microsoft.Web/hostingEnvironments](../azure-monitor/essentials/metrics-supported.md#microsoftwebhostingenvironments)
| Front-End der App Service-Umgebung | [Microsoft.Web/hostingEnvironments/multiRolePools](../azure-monitor/essentials/metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)


Weitere Informationen finden Sie in der Liste [aller in Azure Monitor unterstützten Plattformmetriken](../azure-monitor/essentials/metrics-supported.md).


## <a name="metric-dimensions"></a>Metrikdimensionen

Weitere Informationen zu Metrikdimensionen finden Sie unter [Mehrdimensionale Metriken](../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

App Service hat keine Metriken, die Dimensionen enthalten.

## <a name="resource-logs"></a>Ressourcenprotokolle

> [!NOTE]
> Die Azure Monitor-Integration in App Service befindet sich in der [Vorschauphase](https://aka.ms/appsvcblog-azmon).
>

In diesem Abschnitt werden die Ressourcenprotokolltypen aufgeführt, die für App Service erfasst werden können. 

| Protokolltyp | Windows | Windows-Container | Linux | Linux-Container | BESCHREIBUNG |
|-|-|-|-|-|-|
| AppServiceConsoleLogs | Java SE & Tomcat | Ja | Ja | Ja | Standardausgabe und Standardfehler |
| AppServiceHTTPLogs | Ja | Ja | Ja | Ja | Webserverprotokolle |
| AppServiceEnvironmentPlatformLogs | Ja | – | Ja | Ja | App Service-Umgebung: Skalierung, Konfigurationsänderungen und Statusprotokolle|
| AppServiceAuditLogs | Ja | Ja | Ja | Ja | Anmeldeaktivität per FTP und Kudu |
| AppServiceFileAuditLogs | Ja | Ja | Wird noch angekündigt | Wird noch angekündigt | Dateiänderungen am Websiteinhalt; **nur für Premium-Tarif und höher verfügbar** |
| AppServiceAppLogs | ASP.NET | ASP.NET | Java SE und Tomcat Images <sup>1</sup> | Java SE und Tomcat Blessed Images <sup>1</sup> | Anwendungsprotokolle |
| AppServiceIPSecAuditLogs  | Ja | Ja | Ja | Ja | Anforderungen von IP-Regeln |
| AppServicePlatformLogs  | Wird noch angekündigt | Ja | Ja | Ja | Containervorgangsprotokolle |
| AppServiceAntivirusScanAuditLogs | Ja | Ja | Ja | Ja | [Protokolle für Antivirenscans](https://azure.github.io/AppService/2020/12/09/AzMon-AppServiceAntivirusScanAuditLogs.html) mithilfe von Microsoft Defender; **nur verfügbar für Premium-Tarif** | 

<sup>1</sup> Fügen Sie für Java SE-Apps den App-Einstellungen „$WEBSITE_AZMON_PREVIEW_ENABLED“ hinzu, und legen Sie sie auf „1“ oder „true“ fest.

Eine Referenz finden Sie in der Liste [aller Typen von Ressourcenprotokollkategorien, die in Azure Monitor unterstützt werden](../azure-monitor/essentials/resource-logs-schema.md).

## <a name="azure-monitor-logs-tables"></a>Tabellen in Azure Monitor-Protokollen

Azure App Service verwendet Kusto-Tabellen aus Azure Monitor-Protokollen. Sie können diese Tabellen mit Log Analytics abfragen. Eine Liste der von Kusto verwendeten App Service-Tabellen finden Sie unter [Azure Monitor-Tabellenreferenz nach Ressourcentyp: App Service-Tabellen](/azure/azure-monitor/reference/tables/tables-resourcetype#app-services). 

## <a name="activity-log"></a>Aktivitätsprotokoll

In der folgenden Tabelle finden Sie die allgemeinen Vorgänge in Zusammenhang mit App Service, die im Aktivitätsprotokoll erstellt werden können. Diese Liste ist nicht vollständig.

| Vorgang | BESCHREIBUNG |
|:---|:---|
|Erstellen oder Aktualisieren einer Web-App| App wurde erstellt oder aktualisiert|
|Löschen der Web-App| App wurde gelöscht |
|Erstellen einer Web-App-Sicherung| Sicherung einer App|
|Abrufen von Web-App-Veröffentlichungsprofilen| Herunterladen eines Veröffentlichungsprofils |
|Veröffentlichen einer Web-App| App bereitgestellt |
|Neustarten einer Web-App| App neu gestartet|
|Starten der Web-App| App gestartet |
|Beenden von Web-App| App beendet|
|Austauschen von Web-App-Slots| Slots wurden ausgetauscht|
|Abrufen von Unterschieden bei Web-App-Slots| Unterschieden bei Slots|
|Anwenden der Web-App-Konfiguration| Angewandte Änderungen der Konfiguration|
|Zurücksetzen der Web-App-Konfiguration| Zurücksetzen der Konfigurationsänderungen|
|Bestätigt Verbindungen mit privaten Endpunkten| Genehmigte Verbindungen mit privaten Endpunkten|
|Netzwerkablaufverfolgungs-Web-Apps| Gestartete Netzwerkablaufverfolgung|
|Web-Apps für neues Kennwort| Neues Kennwort erstellt |
|Abrufen gezippter Containerprotokolle für die Web-App| Abrufen von Containerprotokollen |
|Wiederherstellen einer Web-App aus dem Sicherungsblob| Aus einer Sicherung wiederhergestellte App|

Weitere Informationen zum Schema von Aktivitätsprotokolleinträgen finden Sie unter [Aktivitätsprotokollschema](../azure-monitor/essentials/activity-log-schema.md). 

## <a name="see-also"></a>Weitere Informationen

- Eine Beschreibung der Überwachung von Azure App Service finden Sie unter [Überwachen von Azure App Service](monitor-app-service.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).