---
title: Systemvariablen in Azure Data Factory
description: Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren.
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/12/2018
ms.openlocfilehash: 7e29bd82f9f72651ca0383c680c0b05860fe29b4
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110062234"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Von Azure Data Factory unterstützte Systemvariablen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren.

## <a name="pipeline-scope"></a>Bereich „Pipeline“

Auf diese Systemvariablen kann überall im Pipeline-JSON verwiesen werden.

| Variablenname | BESCHREIBUNG |
| --- | --- |
| @pipeline().DataFactory |Name der Data Factory, in der die Pipelineausführung erfolgt |
| @pipeline().Pipeline |Name der Pipeline |
| @pipeline().RunId |ID der jeweiligen Pipelineausführung |
| @pipeline().TriggerType |Der Typ des Triggers, der die Pipeline aufgerufen hat (z. B. `ScheduleTrigger`, `BlobEventsTrigger`). Eine Liste der unterstützten Triggertypen finden Sie unter [Pipelineausführung und Trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md). Der Triggertyp `Manual` gibt an, dass die Pipeline manuell ausgelöst wurde. |
| @pipeline().TriggerId|ID des Triggers, der die Pipeline aufgerufen hat |
| @pipeline().TriggerName|Name des Triggers, der die Pipeline aufgerufen hat |
| @pipeline().TriggerTime|Zeitpunkt der Triggerausführung, durch die die Pipeline aufgerufen wurde. Dies ist der Zeitpunkt, zu dem der Trigger zum Aufrufen der Pipelineausführung **tatsächlich** ausgelöst wurde. Er kann vom geplanten Zeitpunkt für den Trigger geringfügig abweichen.  |
| @pipeline().GroupId | ID der Gruppe, zu der die Pipelineausführung gehört. |
| @pipeline() __?__ .TriggeredByPipelineName | Name der Pipeline, die die Pipelineausführung auslöst. Gilt, wenn die Pipelineausführung durch eine ExecutePipeline-Aktivität ausgelöst wird. Wird in anderen Fällen als _Null_ ausgewertet. Beachten Sie das Fragezeichen nach @pipeline(). |
| @pipeline() __?__ .TriggeredByPipelineRunId | Ausführungs-ID der Pipeline, die die Pipelineausführung auslöst. Gilt, wenn die Pipelineausführung durch eine ExecutePipeline-Aktivität ausgelöst wird. Wird in anderen Fällen als _Null_ ausgewertet. Beachten Sie das Fragezeichen nach @pipeline(). |

>[!NOTE]
>Triggerbezogene Systemvariablen für Datum/Uhrzeit (in Pipeline- und Triggerbereichen) geben UTC-Datumsangaben im ISO 8601-Format zurück, z. B. `2017-06-01T22:20:00.4061448Z`.

## <a name="schedule-trigger-scope"></a>Bereich „Plantrigger“

Auf diese Systemvariablen kann überall im Trigger-JSON für Trigger vom Typ [ScheduleTrigger](concepts-pipeline-execution-triggers.md#schedule-trigger) verwiesen werden.

| Variablenname | Beschreibung |
| --- | --- |
| @trigger().scheduledTime |Zeitpunkt, zu dem der Trigger die Pipelineausführung planmäßig aufrufen sollte. |
| @trigger().startTime |Zeitpunkt, zu dem der Trigger **tatsächlich** ausgelöst wurde, um die Pipelineausführung aufzurufen. Er kann vom geplanten Zeitpunkt für den Trigger geringfügig abweichen. |

## <a name="tumbling-window-trigger-scope"></a>Bereich „Trigger mit rollierendem Fenster“

Auf diese Systemvariablen kann überall im Trigger-JSON für Trigger vom Typ [TumblingWindowTrigger](concepts-pipeline-execution-triggers.md#tumbling-window-trigger) verwiesen werden.

| Variablenname | Beschreibung |
| --- | --- |
| @trigger().outputs.windowStartTime |Anfang des Fensters, das der Triggerausführung zugeordnet wurde. |
| @trigger().outputs.windowEndTime |Ende des Fensters, das der Triggerausführung zugeordnet wurde. |
| @trigger().scheduledTime |Zeitpunkt, zu dem der Trigger die Pipelineausführung planmäßig aufrufen sollte. |
| @trigger().startTime |Zeitpunkt, zu dem der Trigger **tatsächlich** ausgelöst wurde, um die Pipelineausführung aufzurufen. Er kann vom geplanten Zeitpunkt für den Trigger geringfügig abweichen. |

## <a name="storage-event-trigger-scope"></a>Bereich des Speicherereignisauslösers

Auf diese Systemvariablen kann überall im Trigger-JSON für Trigger vom Typ [BlobEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger) verwiesen werden.

| Variablenname | Beschreibung |
| --- | --- |
| @triggerBody().fileName  |Name der Datei, deren Erstellung oder Löschung bewirkt hat, dass der Trigger ausgelöst wurde.   |
| @triggerBody().folderName  |Pfad zu dem Ordner, der die durch `@triggerBody().fileName`angegebene Datei enthält. Das erste Segment des Ordnerpfads ist der Name des Azure Blob Storage-Containers.  |
| @trigger().startTime |Zeitpunkt, zu dem der Trigger ausgelöst wurde, um die Pipelineausführung aufzurufen. |

## <a name="custom-event-trigger-scope"></a>Triggerbereich für benutzerdefinierte Ereignisse

Auf diese Systemvariablen kann überall im Trigger-JSON für Trigger vom Typ [CustomEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger) verwiesen werden.

>[!NOTE]
>Azure Data Factory erwartet, dass benutzerdefinierte Ereignisse mit dem [Azure Event Grid-Ereignissschema](../event-grid/event-schema.md) formatiert werden.

| Variablenname | Beschreibung
| --- | --- |
| @triggerBody().event.eventType | Hierbei handelt es sich um den Typ der Ereignisse, die die Ausführung des Triggers für benutzerdefinierte Ereignisse ausgelöst hat. Der Ereignistyp ist ein vom Kunden definiertes Feld und akzeptiert alle Werte vom Typ „string“ (Zeichenfolge). |
| @triggerBody().event.subject | Hierbei handelt es sich um den Antragsteller des benutzerdefinierten Ereignisses, der den Trigger ausgelöst hat. |
| @triggerBody().event.data._keyName_ | Hierbei handelt es sich um ein Datenfeld im benutzerdefinierten Ereignis, das Kunden zum Senden von Nachrichten und Daten verwenden können. Verwenden Sie „data._keyName_“, um auf jedes Feld zu verweisen. Beispielsweise gibt „@triggerBody().event.data.callback“ den Wert für das Feld _callback_ (Rückruf) zurück, das unter _data_ (Daten) gespeichert ist. |
| @trigger().startTime | Zeitpunkt, zu dem der Trigger ausgelöst wurde, um die Pipelineausführung aufzurufen. |

## <a name="next-steps"></a>Nächste Schritte

* Informationen darüber, wie diese Variablen in Ausdrücken verwendet werden, finden Sie unter [Ausdruckssprache und Funktionen](control-flow-expression-language-functions.md).
* Informationen zur Verwendung von Systemvariablen für den Triggerbereich finden Sie unter [Verweisen auf Triggermetadaten in Pipelineausführungen](how-to-use-trigger-parameterization.md).
