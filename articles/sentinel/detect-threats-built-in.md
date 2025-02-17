---
title: Erkennen von Bedrohungen mit integrierten Analyseregeln in Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie vorgefertigte Erkennungsregeln verwenden, die auf integrierten Vorlagen basieren, mit denen Sie bei verdächtigen Vorkommnissen eine Benachrichtigung erhalten.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2021
ms.author: yelevin
ms.openlocfilehash: bcb84b0beba762342c6ecf8b8f3be81cbcaf24d8
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345765"
---
# <a name="detect-threats-out-of-the-box"></a>Standardmäßig verfügbare Erkennung von Bedrohungen

Nachdem Sie [Ihre Datenquellen mit Azure Sentinel verbunden haben](quickstart-onboard.md), sollten Sie dafür sorgen, dass Warnungen ausgelöst werden, wenn etwas Verdächtiges geschieht. Aus diesem Grund bietet Azure Sentinel integrierte Standardvorlagen, die Sie beim Erstellen von Regeln zur Bedrohungserkennung unterstützen.

Regelvorlagen wurden von Sicherheitsexperten und -analysten von Microsoft basierend auf bekannten Bedrohungen, häufig verwendeten Angriffsvektoren und Eskalationsketten verdächtiger Aktivitäten entworfen. Mit den anhand dieser Vorlagen erstellten Regeln wird die gesamte Umgebung automatisch nach allen Aktivitäten durchsucht, die verdächtig erscheinen. Viele Vorlagen können so angepasst werden, dass gemäß Ihren Anforderungen nach Aktivitäten gesucht oder gefiltert werden kann. Mit den durch diese Regeln generierten Warnungen werden Incidents erstellt, die Sie in Ihrer Umgebung zuweisen und untersuchen können.

In diesem Artikel erfahren Sie, wie Sie mit Azure Sentinel Bedrohungen erkennen können:

> [!div class="checklist"]
> * Verwenden von standardmäßigen Bedrohungserkennungen
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="view-built-in-detections"></a>Anzeigen integrierter Erkennungen

Um alle Analyseregeln und -erkennungen in Azure Sentinel anzuzeigen, wechseln Sie zu **Analytics** > **Regelvorlagen**. Diese Registerkarte enthält alle integrierten Azure Sentinel-Regeln.

:::image type="content" source="media/tutorial-detect-built-in/view-oob-detections.png" alt-text="Verwenden integrierter Erkennungen für die Suche nach Bedrohungen mit Azure Sentinel":::

Integrierte Erkennungen umfassen Folgendes:

|Regeltyp  |BESCHREIBUNG  |
|---------|---------|
|**Microsoft-Sicherheit**     |  Mit Microsoft-Sicherheitsvorlagen werden aus den Warnungen, die in anderen Microsoft-Sicherheitslösungen generiert werden, in Echtzeit Azure Sentinel-Incidents erstellt. Sie können Microsoft-Sicherheitsregeln als Vorlage verwenden, um neue Regeln mit ähnlicher Logik zu erstellen. <br><br>Weitere Informationen zu Sicherheitsregeln finden Sie unter [Automatisches Erstellen von Incidents aus Microsoft-Sicherheitswarnungen](create-incidents-from-alerts.md).       |
|**Fusion**     | Basierend auf der Fusion-Technologie werden für die erweiterte Erkennung mehrstufiger Angriffe in Azure Sentinel skalierbare Machine Learning-Algorithmen verwendet, mit denen viele Low-Fidelity-Warnungen und -Ereignisse über mehrere Produkte hinweg zu verwertbaren High-Fidelity-Incidents korreliert werden können. Fusion ist standardmäßig aktiviert. Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie nur eine Regel mit dieser Vorlage erstellen. <br><br>Die Fusion-Engine kann auch Warnungen, die von [geplanten Analyseregeln](#scheduled) generiert werden, mit denen anderer Systeme korrelieren, was zu High-Fidelity-Incidents führt.      |
|**Machine Learning-Verhaltensanalysen**     |    Vorlagen für Machine Learning-Verhaltensanalysen basieren auf proprietären Microsoft-Algorithmen für maschinelles Lernen, sodass die interne Logik in Bezug auf die Funktionsweise und den Zeitpunkt der Ausführung für Sie nicht erkennbar ist. <br><br>Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie mit jeder Vorlage dieses Typs nur eine Regel erstellen.|
|<a name="anomaly"></a>**Anomalie**     |    Vorlagen für Anomalieregel verwenden SOC-ML (Machine Learning), um bestimmte Arten von anomalen Verhaltensweisen zu erkennen. Jede Regel hat ihre eigenen Parameter und Schwellenwerte, die auf das zu analysierende Verhalten abgestimmt sind. <br><br>Diese Regelkonfigurationen können zwar nicht geändert oder optimiert werden, aber Sie können die Regel duplizieren und das Duplikat ändern und optimieren. Führen Sie in solchen Fällen das Duplikat im Modus **Test-Flighting** und das Original gleichzeitig im Modus **Produktion** aus. Vergleichen Sie dann die Ergebnisse, und stellen Sie das Duplikat auf **Produktion** um, sobald die Optimierung Ihren Wünschen entspricht. <br><br>Weitere Informationen finden Sie unter [Verwenden von SOC-ML-Anomalien zum Erkennen von Bedrohungen in Azure Sentinel](soc-ml-anomalies.md) und [Arbeiten mit Analyseregeln für die Anomalieerkennung in Azure Sentinel](work-with-anomaly-rules.md).     |
| <a name="scheduled"></a>**Geplant**    |    Geplante Analyseregeln basieren auf integrierten Abfragen, die von Microsoft-Sicherheitsexperten geschrieben wurden. Sie sehen die Abfragelogik und können Änderungen daran vornehmen. Sie können die Vorlage für geplante Regeln verwenden und die Abfragelogik- und Planungseinstellungen anpassen, um neue Regeln zu erstellen. <br><br>Mehrere neue Vorlagen für geplante Analyseregel erzeugen Warnungen, die von der Fusion-Engine mit Warnungen anderer Systeme korreliert werden, um High-Fidelity-Incidents zu erzeugen. Weitere Informationen finden Sie unter [Erweiterte Erkennung von mehrstufigen Angriffen](fusion.md#configure-scheduled-analytics-rules-for-fusion-detections).<br><br>**Tipp**: Zu den Planungsoptionen für Regeln gehört es, die Regel so zu konfigurieren, dass sie jede angegebene Anzahl von Minuten, Stunden oder Tagen ausgeführt wird, wobei die Zeit beginnt, wenn Sie die Regel aktivieren. <br><br>Es wird empfohlen, beim Aktivieren einer neuen oder bearbeiteten Analyseregel umsichtig vorzugehen, um sicherzustellen, dass die Regeln den neuen Stapel von Vorfällen rechtzeitig erhalten. Beispielsweise können Sie eine Regel synchron mit dem Arbeitszeitbeginn Ihrer SOC-Analysten ausführen, und die Regeln zu diesem Zeitpunkt aktivieren.|
| | |

> [!IMPORTANT]
>  - Einige der Erkennungsfunktionen in der Fusion-Regelvorlage befinden sich aktuell in der **VORSCHAU**. Unter [Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel](fusion.md) erfahren Sie, welche Erkennungsfunkionen als Vorschau verfügbar sind.
>
> -  Die Vorlagen für Anomalieregeln befinden sich derzeit in der **VORSCHAU**.
>
> - Die Regelvorlagen für Verhaltensanalysen mit maschinellem Lernen befinden sich aktuell in der **VORSCHAU**. Durch Erstellen und Aktivieren von Regeln, die auf den Vorlagen für die ML-Verhaltensanalyse basieren, **erteilen Sie Microsoft die Berechtigung, erfasste Daten nach Bedarf für die Verarbeitung durch die Machine Learning-Engines und -Modelle außerhalb der geografischen Region Ihres Azure Sentinel-Arbeitsbereichs zu kopieren**.
>
> Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="use-built-in-analytics-rules"></a>Verwenden integrierter Analyseregeln

In diesem Verfahren wird beschrieben, wie Sie integrierte Analyseregelvorlagen verwenden.

**So verwenden Sie integrierte Analyseregeln**:

1. Wählen Sie in Azure Sentinel auf der Seite **Analytics** > **Regelvorlagen** einen Vorlagennamen und dann im Detailbereich die Schaltfläche **Regel erstellen** aus, um eine neue aktive Regel auf Grundlage dieser Vorlage zu erstellen. 

    Jede Vorlage enthält eine Liste der erforderlichen Datenquellen. Wenn Sie die Vorlage öffnen, werden die Datenquellen automatisch auf Verfügbarkeit überprüft. Wenn ein Verfügbarkeitsproblem vorliegt, ist möglicherweise die Schaltfläche **Regel erstellen** deaktiviert, oder es wird eine entsprechende Warnung angezeigt.

    :::image type="content" source="media/tutorial-detect-built-in/use-built-in-template.png" alt-text="Bereich für Vorschau der Erkennungsregel":::

1. Durch Auswählen von **Regel erstellen** wird der Assistent zum Erstellen von Regeln auf Grundlage der ausgewählten Vorlage geöffnet. Alle Details werden automatisch ausgefüllt, und Sie können mit der Vorlage **Geplant** oder **Microsoft-Sicherheit** die Logik und andere Regeleinstellungen entsprechend Ihren speziellen Anforderungen anpassen. Sie können diesen Vorgang wiederholen, um zusätzliche Regeln basierend auf der integrierten Vorlage zu erstellen. Nach dem Ausführen aller Schritte im Assistenten zum Erstellen von Regeln haben Sie die Erstellung einer Regel basierend auf der Vorlage abgeschlossen. Die neuen Regeln werden auf der Registerkarte **Aktive Regeln** angezeigt.

    Weitere Informationen zum Anpassen der Regeln im Assistenten zum Erstellen von Regeln finden Sie unter [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md).

> [!TIP]
> - Stellen Sie sicher, dass Sie **alle Regeln, die den verbundenen Datenquellen** zugeordnet sind, aktivieren, um eine vollständige Sicherheitsabdeckung für Ihre Umgebung zu gewährleisten. Die effizienteste Möglichkeit zum Aktivieren von Analyseregeln besteht direkt auf der Seite des Datenconnectors, auf der alle zugehörigen Regeln aufgelistet sind. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zu Datenquellen](connect-data-sources.md).
> 
> - Sie können auch **Regeln per [API](/rest/api/securityinsights/) und [PowerShell](https://www.powershellgallery.com/packages/Az.SecurityInsights/0.1.0) an Azure Sentinel pushen**, obwohl dies zusätzlichen Aufwand mit sich bringt. 
> 
>     Wenn Sie die API oder PowerShell verwenden, müssen Sie die Regeln zunächst in JSON exportieren, bevor Sie die Regeln aktivieren. Die API oder PowerShell kann hilfreich sein, wenn Sie Regeln in mehreren Instanzen von Azure Sentinel mit identischen Einstellungen in jeder Instanz aktivieren.
> 
## <a name="export-rules-to-an-arm-template"></a>Exportieren von Regeln in eine ARM-Vorlage

Sie können [Ihre Regel problemlos in eine Azure Resource Manager-Vorlage (ARM) exportieren](import-export-analytics-rules.md), wenn Sie Ihre Regeln als Code verwalten und bereitstellen möchten. Sie können auch Regeln aus Vorlagendateien importieren und dann auf der Benutzeroberfläche anzeigen und bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

- Um benutzerdefinierte Regeln zu erstellen, verwenden Sie vorhandene Regeln als Vorlagen oder Referenzen. Die Verwendung vorhandener Regeln als Grundlage hilft dabei, den Großteil der Logik zu erstellen, bevor Sie die erforderlichen Änderungen vornehmen. Weitere Informationen finden Sie unter [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md).

- Informationen zum Automatisieren der Reaktionen auf Bedrohungen finden Sie im Tutorial [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).
