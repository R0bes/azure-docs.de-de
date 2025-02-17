---
title: Behandeln von Protokollwarnungen in Azure Monitor | Microsoft-Dokumentation
description: Allgemeine Probleme, Fehler und deren Behandlung für Protokollwarnungsregeln in Azure.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/22/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f7e5340b69bacb14180b4feee7ada22c7ca298d0
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111592696"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Behandeln von Protokollwarnungen in Azure Monitor  

In diesem Artikel wird beschrieben, wie Sie häufig auftretende Probleme mit Protokollwarnungen in Azure Monitor beheben. Er bietet auch Lösungen für häufig auftretende Probleme bezüglich der Funktionalität und der Konfiguration von Protokollwarnungen.

Mithilfe von Protokollwarnungen können Sie eine [Log Analytics](../logs/log-analytics-tutorial.md)-Abfrage verwenden, um Ressourcenprotokolle mit einer bestimmten Häufigkeit auszuwerten und basierend auf den Ergebnissen eine Warnung auszulösen. Durch Regeln können über [Aktionsgruppen](./action-groups.md) einzelne oder mehrere Aktionen ausgelöst werden. Weitere Informationen zur Funktionalität und Terminologie von Protokollwarnungen finden Sie unter [Protokollwarnungen in Azure Monitor](alerts-unified-log.md).

> [!NOTE]
> In diesem Artikel werden keine Fälle behandelt, in denen im Azure-Portal angezeigt wird, dass zwar eine Warnungsregel ausgelöst wurde, jedoch keine Benachrichtigung empfangen wird. Informationen zu diesen Fällen finden Sie unter [Eine Aktion oder Benachrichtigung für meine Warnung funktioniert nicht erwartungsgemäß](./alerts-troubleshoot.md#action-or-notification-on-my-alert-did-not-work-as-expected).

## <a name="log-alert-didnt-fire"></a>Protokollwarnung wurde nicht ausgelöst.

### <a name="data-ingestion-time-for-logs"></a>Datenerfassungszeit für Protokolle

Mit Azure Monitor werden Kundenprotokolle im Terabytebereich verarbeitet, die aus der ganzen Welt stammen, und dies kann zu [Latenz bei der Protokollerfassung](../logs/data-ingestion-time.md) führen.

Bei Protokollen handelt es sich um teilweise strukturierte Daten, die eine höhere Latenz aufweisen als Metriken. Wenn es für ausgelöste Warnungen zu einer Verzögerung von mehr als vier Minuten kommt, sollten Sie die Verwendung von [Metrikwarnungen](alerts-metric-overview.md) erwägen. Sie können Daten aus Protokollen an den Metrikspeicher senden, indem Sie [Metrikwarnungen für Protokolle](alerts-metric-logs.md) verwenden.

Die Bewertung von Warnungen wird bei Bedarf mehrmals wiederholt, um die Latenz zu verringern. Sobald die Daten empfangen werden, wird die Warnung ausgelöst. In den meisten Fällen entspricht dies nicht der Dauer für die Protokollaufzeichnung.

### <a name="incorrect-query-time-range-configured"></a>Falscher Abfragezeitraum konfiguriert

Der Abfragezeitraum wird in der Definition für die Regelbedingung festgelegt. Bei Arbeitsbereichen und Application Insights wird dieses Feld als **Periode** bezeichnet. Bei allen anderen Ressourcentypen lautet der Name **Zeitbereich für Außerkraftsetzungsabfrage**. Wie auch bei der Protokollanalyse werden die Abfragedaten durch den Zeitbereich auf den angegebenen Zeitraum begrenzt. Der Zeitbereich gilt auch, wenn in der Abfrage der Befehl **ago** verwendet wird. 

Beispielsweise führt eine Abfrage eine Überprüfung für einen Zeitraum von 60 Minuten durch, wenn ein Zeitbereich von 60 Minuten angegeben ist. Dies gilt auch, wenn der Text **ago(1d)** enthält. Die Filterung für den Zeitbereich und den Abfragezeitraum muss übereinstimmen. Wenn **Periode** / **Zeitbereich für Außerkraftsetzungsabfrage** im Anwendungsfall in 1 Tag geändert wird, führt dies zum erwarteten Ergebnis.

![Zeitraum](media/alerts-troubleshoot-log/LogAlertTimePeriod.png)

### <a name="actions-are-muted-in-the-alert-rule"></a>Aktionen sind in Warnungsregel stummgeschaltet

Protokollwarnungen verfügen über eine Option zum Stummschalten von Aktionen mit ausgelösten Warnungen für einen festgelegten Zeitraum. In Arbeitsbereichen und in Application Insights heißt dieses Feld **Warnungen unterdrücken**. Bei allen anderen Ressourcentypen lautet die Bezeichnung **Aktionen unterdrücken**. 

Häufig wird in diesen Situationen fälschlicherweise vermutet, dass die Warnung die Aktionen aufgrund eines Dienstproblems nicht ausgelöst hat. Der Grund ist jedoch, dass die Aktion durch die Regelkonfiguration unterdrückt wurde.

![Suppress alerts (Warnungen unterdrücken)](media/alerts-troubleshoot-log/LogAlertSuppress.png)

### <a name="alert-scope-resource-has-been-moved-renamed-or-deleted"></a>Warnungsbereichsressource wurde verschoben, umbenannt oder gelöscht

Wenn Sie eine Warnungsregel erstellen, erstellt Log Analytics eine Berechtigungsmomentaufnahme für Ihre Benutzer-ID. Diese Momentaufnahme wird in der Regel gespeichert und enthält die Regelbereichsressource, die Azure Resource Manager-ID. Wenn die Regelbereichsressource verschoben, umbenannt oder gelöscht wird, kommt es für alle Protokollwarnungsregeln zu einem Fehler, die auf diese Ressource verweisen. Für eine ordnungsgemäße Funktionsweise müssen Warnungsregeln mithilfe der neuen Azure Resource Manager-ID neu erstellt werden.

### <a name="metric-measurement-alert-rule-with-splitting-using-the-legacy-log-analytics-api"></a>Warnungsregel vom Typ „Metrische Maßeinheit“ mit Aufteilung per Log Analytics-Legacy-API

[Metrische Maßeinheit](alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value) ist ein Typ einer Protokollwarnung, die auf den zusammengefassten Zeitreihenergebnissen basiert. Sie können diese Regeln für eine Gruppierung nach Spalten verwenden, um [Warnungen zu teilen](alerts-unified-log.md#split-by-alert-dimensions). Wenn Sie die Log Analytics-Legacy-API verwenden, funktioniert die Aufteilung nicht wie erwartet, da keine Gruppierung unterstützt wird.

Sie können die aktuelle ScheduledQueryRules-API verwenden, um **Aggregieren nach** in Regeln vom Typ [Metrische Maßeinheit](alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value) festzulegen, die wie erwartet funktionieren. Weitere Informationen zum Wechsel zur aktuellen ScheduledQueryRules-API finden Sie unter [Upgrade von der Log Analytics-Legacywarnungs-API auf die aktuelle Protokollwarnungs-API]](../alerts/alerts-log-api-switch.md).

## <a name="log-alert-fired-unnecessarily"></a>Protokollwarnung wird unnötigerweise ausgelöst.

Unter Umständen wird unerwarteterweise eine konfigurierte [Protokollwarnungsregel in Azure Monitor](./alerts-log.md) ausgelöst. In den folgenden Abschnitten werden einige häufige Gründe beschrieben.

### <a name="alert-triggered-by-partial-data"></a>Warnung wird durch unvollständige Daten ausgelöst.

Mit Azure Monitor werden Kundenprotokolle im Terabytebereich verarbeitet, die aus der ganzen Welt stammen, und dies kann zu [Latenz bei der Protokollerfassung](../logs/data-ingestion-time.md) führen.

Bei Protokollen handelt es sich um teilweise strukturierte Daten, die eine höhere Latenz aufweisen als Metriken. Falls bei Ihnen viele Warnungen fälschlicherweise ausgelöst werden, sollten Sie erwägen, [Metrikwarnungen](alerts-metric-overview.md) zu nutzen. Sie können Daten aus Protokollen an den Metrikspeicher senden, indem Sie [Metrikwarnungen für Protokolle](alerts-metric-logs.md) verwenden.

Protokollwarnungen funktionieren am besten, wenn Sie versuchen, Daten in den Protokollen zu erkennen. Sie funktionieren weniger gut, wenn Sie versuchen, in den Protokollen das Fehlen von Daten zu erkennen (z. B. Warnungen zum VM-Takt). 

Zwar sind integrierte Funktionen zum Verhindern von falschen Warnungen vorhanden, diese können für Daten mit sehr hoher Latenz (mehr als ca. 30 Minuten) und mit Latenzspitzen jedoch trotzdem auftreten.

### <a name="query-optimization-issues"></a>Probleme mit der Abfrageoptimierung

Ihre Abfrage wird vom Warnungsdienst geändert, um eine Optimierung in Bezug auf eine geringere Auslastung und Warnungswartezeit zu erzielen. Der Warnungsablauf wurde zum Transformieren der Ergebnisse erstellt, die auf das Problem für eine Warnung hinweisen. Ein Beispiel hierfür ist eine Abfrage der folgenden Art:

``` Kusto
SecurityEvent
| where EventID == 4624
```

Wenn die Absicht des Benutzers beim Auftreten dieses Ereignistyps eine Warnung ist, wird von der Warnungslogik `count` an die Abfrage angefügt. Die folgende Abfrage wird ausgeführt:

``` Kusto
SecurityEvent
| where EventID == 4624
| count
```

Es ist nicht erforderlich, der Abfrage Warnungslogik hinzuzufügen. Dies kann sogar zu Problemen führen. Wenn Sie im vorherigen Beispiel `count` zu Ihrer Abfrage hinzufügen, ist das Ergebnis immer der Wert **1**. Der Grund dafür ist, dass der Warnungsdienst einen `count` von `count` durchführt.

Die optimierte Abfrage wird vom Protokollwarnungsdienst ausgeführt. Sie können die geänderte Abfrage im Log Analytics-[Portal](../logs/log-query-overview.md) oder über die [API](/rest/api/loganalytics/) ausführen.

Für Arbeitsbereiche und Application Insights lautet die Bezeichnung im Bereich mit den Bedingungen **Auszuführende Abfrage**. Wählen Sie für alle anderen Ressourcentypen auf der Registerkarte **Bedingung** die Option **Abschließende Warnungsabfrage anzeigen** aus.

![Auszuführende Abfrage](media/alerts-troubleshoot-log/LogAlertPreview.png)

## <a name="log-alert-was-disabled"></a>Protokollwarnung wurde deaktiviert

In den folgenden Abschnitten sind einige Gründe aufgeführt, warum Azure Monitor eine Protokollwarnungsregel ggf. deaktiviert. Nach diesem Abschnitt finden Sie ein [Beispiel für das Aktivitätsprotokoll, das bei deaktivierter Regel gesendet wird](#activity-log-example-when-rule-is-disabled).

### <a name="alert-scope-no-longer-exists-or-was-moved"></a>Warnungsbereich ist nicht mehr vorhanden oder wurde verschoben

Wenn die Bereichsressourcen einer Warnungsregel nicht mehr gültig sind, tritt bei der Ausführung der Regel ein Fehler auf, und die Abrechnung wird angehalten.

Wenn bei einer Protokollwarnung eine Woche lang kontinuierlich ein Fehler auftritt, wird sie von Azure Monitor deaktiviert.

### <a name="query-used-in-a-log-alert-isnt-valid"></a>In der Protokollwarnung verwendete Abfrage ist ungültig

Wenn eine Protokollwarnungsregel erstellt wird, wird die Abfrage auf die richtige Syntax überprüft. Es kann auch vorkommen, dass für die in der Protokollwarnungsregel angegebene Abfrage ein Fehler auftritt. Häufige Gründe:

- Regeln wurden über die API erstellt, und die Überprüfung wurde vom Benutzer übersprungen.
- Die Abfrage wird [auf mehreren Ressourcen ausgeführt](../logs/cross-workspace-query.md), und mindestens eine der Ressourcen wurde gelöscht oder verschoben.
- Für die [Abfrage tritt ein Fehler auf](https://dev.loganalytics.io/documentation/Using-the-API/Errors), weil Folgendes passiert:
    - Da die Protokollierungslösung nicht [im Arbeitsbereich bereitgestellt](../insights/solutions.md#install-a-monitoring-solution) wurde, werden keine Tabellen erstellt.
    - Seit über 30 Tagen sind in eine Tabelle in der Abfrage keine Daten mehr geflossen.
    - [Benutzerdefinierte Protokolltabellen](../agents/data-sources-custom-logs.md) wurden noch nicht erstellt, weil der Datenfluss noch nicht gestartet wurde.
- Änderungen an der [Abfragesprache](/azure/kusto/query/) beinhalten ein überarbeitetes Format für Befehle und Funktionen, sodass die zuvor bereitgestellte Abfrage nicht mehr gültig ist.

[Azure Advisor](../../advisor/advisor-overview.md) warnt Sie vor diesem Verhalten. Es wird eine Empfehlung zur betroffenen Protokollwarnungsregel hinzugefügt. Als Kategorie wird „Hochverfügbarkeit“ mit mittlerer Auswirkung und zusätzlich die Beschreibung „Reparieren Ihrer Protokollwarnungsregel, um die Überwachung sicherzustellen“ verwendet.

## <a name="alert-rule-quota-was-reached"></a>Das Warnungsregelkontingent wurde erreicht

Einzelheiten zur Anzahl von Warnungsregeln für die Protokollsuche pro Abonnement und die Höchstwerte für Ressourcen finden Sie unter [Azure Monitor-Diensteinschränkungen](../service-limits.md).

### <a name="recommended-steps"></a>Empfohlene Schritte
    
Wenn Sie die Kontingentgrenze erreicht haben, helfen Ihnen die folgenden Schritte ggf. bei der Problembehebung.

1. Löschen oder deaktivieren Sie Warnungsregeln für die Protokollsuche, die nicht mehr verwendet werden.
1. [Teilen Sie Warnungen nach Dimensionen auf](alerts-unified-log.md#split-by-alert-dimensions), um die Anzahl von Regeln zu reduzieren. Mit diesen Regeln können viele Ressourcen und Erkennungsfälle überwacht werden.
1. Wenn die Kontingentgrenze erhöht werden muss, können Sie damit fortfahren, eine Supportanfrage mit den folgenden Informationen zu erstellen:

    - Abonnement-IDs und Ressourcen-IDs, für die die Kontingentgrenze erhöht werden muss
    - Grund für die Kontingenterhöhung
    - Ressourcentyp für die Kontingenterhöhung, z. B. **Log Analytics** oder **Application Insights**
    - Angeforderte Kontingentgrenze

### <a name="to-check-the-current-usage-of-new-log-alert-rules"></a>So überprüfen Sie die derzeitige Kontingentnutzung durch neue Protokollwarnungsregeln
    
#### <a name="from-the-azure-portal"></a>Über das Azure-Portal

1. Wählen Sie auf dem Bildschirm „Warnungen“ die Option **Warnungsregeln verwalten** aus.
1. Filtern Sie in der Dropdownliste **Abonnement** nach dem gewünschten Abonnement. (Stellen Sie sicher, dass Sie nicht nach einer bestimmten Ressourcengruppe, einem Ressourcentyp oder einer Ressource filtern.)
1. Wählen Sie in der Dropdownliste **Signaltyp** die Option **Protokollsuche** aus.
1. Vergewissern Sie sich, dass in der Dropdownliste **Status** die Option **Aktiviert** ausgewählt ist.

Die Gesamtanzahl von Warnungsregeln für die Protokollsuche wird über der Regelliste angezeigt.

#### <a name="from-api"></a>Über eine API

- PowerShell: [Get-AzScheduledQueryRule](/powershell/module/az.monitor/get-azscheduledqueryrule)
- REST-API: [Auflisten nach Abonnement](/rest/api/monitor/scheduledqueryrules/listbysubscription)

## <a name="activity-log-example-when-rule-is-disabled"></a>Aktivitätsprotokollbeispiel bei Deaktivierung der Regel

Wenn für die Abfrage sieben Tage hintereinander ein Fehler auftritt, deaktiviert Azure Monitor die Protokollwarnung, und die Abrechnung wird für die Regel beendet. Sie können den genauen Zeitpunkt, zu dem Azure Monitor die Protokollwarnung deaktiviert hat, im [Azure-Aktivitätsprotokoll](../../azure-resource-manager/management/view-activity-logs.md) ermitteln. 

Beispiel:

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokollwarnungen in Azure](./alerts-unified-log.md).
- Informieren Sie sich über das [Konfigurieren von Protokollwarnungen](../logs/log-query-overview.md).
- Weitere Informationen zum [Analysieren von Protokolldaten in Azure Monitor](../logs/log-query-overview.md).
