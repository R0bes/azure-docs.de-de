---
title: Migration zu den Read v3.x Containern
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie die Migration zu den v3 Read OCR-Containern ausgeführt wird
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 05/24/2021
ms.author: aahi
ms.openlocfilehash: f8913bc75688184d30711e4ee33b5d3601557ee7
ms.sourcegitcommit: 025a2bacab2b41b6d211ea421262a4160ee1c760
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2021
ms.locfileid: "113303668"
---
# <a name="migrate-to-the-read-v3x-ocr-containers"></a>Migration zu den Read v3.x OCR-Containern

Wenn Sie Version 2 des Read OCR-Containers für maschinelles Sehen verwenden, informieren Sie sich in diesem Artikel über die Möglichkeiten zum Upgrade Ihrer Anwendung für die Verwendung von Version 3.x des Containers. 


## <a name="configuration-changes"></a>Konfigurationsänderungen

* `ReadEngineConfig:ResultExpirationPeriod` wird nicht mehr unterstützt. Der Read OCR-Container verfügt über einen integrierten Cron-Auftrag, der die einer Anforderung zugeordneten Ergebnisse und Metadaten nach 48 Stunden entfernt.
* `Cache:Redis:Configuration` wird nicht mehr unterstützt. Der Cache wird in den Containern der Version 3.x nicht verwendet, sodass sie ihn nicht festlegen müssen.

## <a name="api-changes"></a>API-Änderungen

Der Read v3.2-Container verwendet Version 3 der Maschinelles Sehen-API und verfügt über folgende Endpunkte:

* `/vision/v3.2-preview.1/read/analyzeResults/{operationId}`
* `/vision/v3.2-preview.1/read/analyze`
* `/vision/v3.2-preview.1/read/syncAnalyze`

Ausführliche Informationen zum Aktualisieren Ihrer Anwendungen für die Verwendung von Version 3 der cloudbasierten Read-API finden Sie im [Migrationshandbuch für die REST-API von Maschinelles Sehen v3](./upgrade-api-versions.md). Diese Informationen gelten auch für den Container. Synchronisierungsvorgänge werden nur in Containern unterstützt.

## <a name="memory-requirements"></a>Speicheranforderungen

Die Anforderungen und Empfehlungen basieren auf Benchmarks mit einer einzelnen Anforderung pro Sekunde, wobei ein 8 MB großes Bild eines gescannten Geschäftsbriefs mit 29 Zeilen und insgesamt 803 Zeichen verwendet wird. In der folgenden Tabelle werden die minimale und empfohlene Zuordnung von Ressourcen für jeden OCR-Container für das Lesen beschrieben.

|Container  |Minimum | Empfohlen  |
|---------|---------|------|
|Read 3.2-preview | 8 Kerne, 16 GB Arbeitsspeicher         | 8 Kerne, 24 GB Arbeitsspeicher |

Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die von Docker als Bestandteil des run-Befehls verwendet werden.

## <a name="storage-implementations"></a>Speicherimplementierungen

>[!NOTE]
> MongoDB wird in 3.x-Versionen des Containers nicht mehr unterstützt. Stattdessen unterstützen die Container Azure Storage-und Offlinedateisysteme.

| Implementierung |    Erforderliche Laufzeitargumente |
|---------|---------|
|Dateiebene (Standard)    | Keine Laufzeitargumente erforderlich. Das `/share`-Verzeichnis wird verwendet. |
|Azure Blob    | `Storage:ObjectStore:AzureBlob:ConnectionString={AzureStorageConnectionString}` |

## <a name="queue-implementations"></a>Warteschlangenimplementierungen

In v3.x des Containers wird RabbitMQ derzeit nicht unterstützt. Folgende Unterstützungsimplementierungen werden unterstützt:

| Implementierung | Laufzeitargumente | Beabsichtigte Verwendung |
|---------|---------|-------|
| In-Memory (Standard) | Keine Laufzeitargumente erforderlich. | Entwickeln und Testen |
| Azure-Warteschlangen | `Queue:Azure:ConnectionString={AzureStorageConnectionString}` | Produktion |
| RabbitMQ    | Nicht verfügbar | Produktion |

Zum Erreichen zusätzlicher Redundanz verwendet der Read v3.x-Container einen Zeitgeber für Sichtbarkeit, um sicherzustellen, dass Anforderungen bei einem Setup mit mehreren Containern auch bei einem Absturz erfolgreich verarbeitet werden können. 

Legen Sie den Zeitgeber mit `Queue:Azure:QueueVisibilityTimeoutInMilliseconds` fest, was die Zeit angibt, in der eine Nachricht nicht sichtbar ist, wenn sie von einem anderen Worker verarbeitet wird. Um redundante Verarbeitung von Seiten zu vermeiden, empfehlen wir, den Timeoutzeitraum auf 120 Sekunden festzulegen. Der Standardwert ist 30 Sekunden.

| Standardwert | Empfohlener Wert |
|---------|---------|
| 30.000 |    120000 |


## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](computer-vision-resource-container-config.md).
* Lesen Sie [die OCR Übersicht ](overview-ocr.md), um weitere Informationen zur Erkennung von gedrucktem und handschriftlichem Text zu erhalten.
* Details zu den vom Container unterstützten Methoden finden Sie unter [Lese-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).
* Unter [Häufig gestellte Fragen (FAQ)](FAQ.yml) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktionalität des maschinellen Sehens.
* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)