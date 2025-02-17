---
title: Log Analytics-Tutorial
description: In diesem Tutorial erfahren Sie, wie Sie mit den Features von Log Analytics in Azure Monitor eine Protokollabfrage erstellen und ausführen und die Ergebnisse im Azure-Portal analysieren.
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 06/28/2021
ms.openlocfilehash: 6a78bcdb6f8f3036da4f273256d681f94f87a1bb
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113356633"
---
# <a name="log-analytics-tutorial"></a>Log Analytics-Tutorial
Log Analytics ist ein Tool im Azure-Portal, mit dem Protokollabfragen aus den von Azure Monitor-Protokollen erfassten Daten bearbeitet und ausgeführt sowie deren Ergebnisse interaktiv analysiert werden können. Mit Log Analytics-Abfragen können Sie Datensätze abrufen, die bestimmte Kriterien erfüllen, Trends identifizieren, Muster analysieren und eine Vielzahl von Erkenntnissen aus Ihren Daten gewinnen. 

In diesem Tutorial werden die Log Analytics-Benutzeroberfläche, die ersten Schritte mit einigen einfachen Abfragen und die Möglichkeiten zur Verwendung der Ergebnisse erläutert. Sie lernen Folgendes:

> [!div class="checklist"]
> * Das Schema von Protokolldaten nachvollziehen
> * Einfache Abfragen schreiben und ausführen sowie den Zeitbereich für Abfragen ändern
> * Abfrageergebnisse filtern, sortieren und gruppieren
> * Visuelle Elemente der Abfrageergebnisse anzeigen, ändern und teilen
> * Abfragen und Ergebnisse laden, exportieren und kopieren

> [!IMPORTANT]
> In diesem Tutorial werden Features von Log Analytics zum Erstellen und Ausführen einer Abfrage verwendet, anstatt mit der Abfrage selbst zu arbeiten. Sie nutzen Log Analytics-Features, um eine Abfrage zu erstellen, und verwenden eine weitere Beispielabfrage. Wenn Sie sich über die Syntax von Abfragen informieren und direkt mit dem Bearbeiten der Abfrage selbst beginnen möchten, schauen Sie sich das [Tutorial zur Kusto-Abfragesprache](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor) an. In diesem Tutorial werden mehrere Beispielabfragen erläutert, die Sie in Log Analytics mit einigen der hier vorgestellten Features bearbeiten und ausführen können.


## <a name="prerequisites"></a>Voraussetzungen
In diesem Tutorial wird die [Demoumgebung von Log Analytics](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade) verwendet, die zahlreiche Beispieldaten zum Ausführen der Beispielabfragen enthält. Sie können auch Ihr eigenes Azure-Abonnement verwenden, verfügen dann aber möglicherweise nicht über Daten in denselben Tabellen.

## <a name="open-log-analytics"></a>Öffnen von Log Analytics
Öffnen Sie die [Demoumgebung von Log Analytics](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade), oder wählen Sie im Azure Monitor-Menü in Ihrem Abonnement die Option **Protokolle** aus. Dadurch wird der Anfangsbereich auf einen Log Analytics-Arbeitsbereich festgelegt. Dies bedeutet, dass Ihre Abfrage die Auswahl auf Grundlage aller Daten in diesem Arbeitsbereich trifft. Wenn Sie die Option **Protokolle** im Menü einer Azure-Ressource auswählen, wird der Bereich nur auf Datensätze in dieser Ressource festgelegt. Ausführliche Informationen zum Bereich finden Sie unter [Protokollabfragebereich](./scope.md).

Sie können den Bereich in der oberen linken Ecke des Bildschirms anzeigen. Wenn Sie Ihre eigene Umgebung verwenden, wird eine Option zum Auswählen eines anderen Bereichs angezeigt, in der Demoumgebung ist diese Option jedoch nicht verfügbar.

:::image type="content" source="media/log-analytics-tutorial/scope.png" alt-text="Log Analytics-Bereich" lightbox="media/log-analytics-tutorial/scope.png":::

## <a name="table-schema"></a>Tabellenschema
Auf der linken Seite des Bildschirms finden Sie die Registerkarte **Tabellen**, auf der Sie die im aktuellen Bereich verfügbaren Tabellen untersuchen können. Diese sind standardmäßig nach **Lösung** gruppiert, Sie können die Gruppierung jedoch ändern oder die Tabellen filtern. 

Erweitern Sie die Projektmappe **Protokollverwaltung**, und suchen Sie nach der Tabelle **AppRequests**. Sie können die Tabelle erweitern, um das Schema anzuzeigen, oder auf ihren Namen zeigen, um zusätzliche Informationen zu erhalten. 

:::image type="content" source="media/log-analytics-tutorial/table-details.png" alt-text="Tabellenansicht" lightbox="media/log-analytics-tutorial/table-details.png":::

Klicken Sie auf den Link unten um **Hilfreiche Informationen** zu erhalten, um zum Tabellenverweis zu wechseln, unter dem die einzelnen Tabellen und die zugehörigen Spalten dokumentiert sind. Klicken Sie auf **Datenvorschau**, um einen kurzen Blick auf einige der aktuellen Datensätze in der Tabelle zu werfen. So können Sie sicherstellen, dass es sich hierbei um die erwarteten Daten handelt, bevor Sie damit eine Abfrage ausführen.

:::image type="content" source="media/log-analytics-tutorial/sample-data.png" alt-text="Beispieldaten" lightbox="media/log-analytics-tutorial/sample-data.png":::

## <a name="write-a-query"></a>Schreiben Sie eine Abfrage.
Jetzt schreiben Sie mithilfe der Tabelle **AppRequests** eine Abfrage. Doppelklicken Sie auf den Namen der Tabelle, um ihn dem Abfragefenster hinzuzufügen. Sie können den Namen auch direkt in das Fenster eingeben und sogar IntelliSense nutzen, um die Namen von Tabellen im aktuellen Bereich und KQL-Befehle vervollständigen zu lassen.

Dies ist die einfachste Abfrage, die wir schreiben können. Sie gibt lediglich alle Datensätze in einer Tabelle zurück. Führen Sie die Abfrage aus, indem Sie auf die Schaltfläche **Ausführen** klicken oder den Cursor an einer beliebigen Stelle im Abfragetext positionieren und UMSCHALT+EINGABETASTE drücken.

:::image type="content" source="media/log-analytics-tutorial/query-results.png" alt-text="Abfrageergebnisse" lightbox="media/log-analytics-tutorial/query-results.png":::

Wie Sie sehen, werden Ergebnisse angezeigt. Die Anzahl von Datensätzen, die von der Abfrage zurückgegeben werden, wird in der unteren rechten Ecke angezeigt. 

## <a name="filter"></a>Filtern

Nun fügen Sie der Abfrage einen Filter hinzu, um die Anzahl zurückgegebener Datensätze zu verringern. Klicken Sie im linken Bereich auf die Registerkarte **Filter**. Dort werden verschiedene Spalten in den Abfrageergebnissen angezeigt, die Sie zum Filtern der Ergebnisse verwenden können. Die obersten Werte in den Spalten werden mit der Anzahl von Datensätzen angezeigt, die diesen Wert enthalten. Klicken Sie unter **ResultCode** auf **200** und dann **auf Anwenden & Ausführen**. 

:::image type="content" source="media/log-analytics-tutorial/query-pane.png" alt-text="Abfragebereich" lightbox="media/log-analytics-tutorial/query-pane.png":::

Der Abfrage wird eine **where**-Anweisung mit dem von Ihnen ausgewählten Wert hinzugefügt. Die Ergebnisse enthalten jetzt nur die Datensätze mit diesem Wert, und die Anzahl von Datensätzen ist entsprechend geringer.

:::image type="content" source="media/log-analytics-tutorial/query-results-filter-01.png" alt-text="Gefilterte Abfrageergebnisse" lightbox="media/log-analytics-tutorial/query-results-filter-01.png":::


## <a name="time-range"></a>Uhrzeitbereich
Alle Tabellen in einem Log Analytics-Arbeitsbereich verfügen über eine Spalte **TimeGenerated**, die den Zeitpunkt der Erstellung des Datensatzes angibt. Alle Abfragen weisen einen Zeitbereich auf, durch den die Ergebnisse auf Datensätze mit einem **TimeGenerated**-Wert innerhalb dieses Bereichs beschränkt werden. Der Zeitbereich kann entweder in der Abfrage oder mit dem Selektor oben auf dem Bildschirm festgelegt werden.

Standardmäßig gibt die Abfrage mindestens die Datensätze der letzten 24 Stunden zurück. Sie sollten hier eine Meldung sehen, dass uns nicht alle Ergebnisse angezeigt werden. Dies liegt daran, dass Log Analytics maximal 30.000 Datensätze zurückgeben kann und unsere Abfrage mehr Datensätze zurückgegeben hat. Wählen Sie das Dropdownmenü **Zeitbereich** aus, und ändern Sie den Wert in **12 Stunden**. Klicken Sie erneut auf **Ausführen**, um die Ergebnisse zurückzugeben. 

:::image type="content" source="media/log-analytics-tutorial/query-results-max.png" alt-text="Uhrzeitbereich" lightbox="media/log-analytics-tutorial/query-results-max.png":::


## <a name="multiple-query-conditions"></a>Mehrere Abfragebedingungen
Jetzt reduzieren Sie die Ergebnisse nochmals, indem Sie eine weitere Filterbedingung hinzufügen. Eine Abfrage kann beliebig viele Filter enthalten, um genau die gewünschte Gruppe von Datensätzen als Ziel festzulegen. Wählen Sie unter **Name** die Option **Get Home/Index** und klicken Sie auf **Anwenden & Ausführen**. 

:::image type="content" source="media/log-analytics-tutorial/query-results-filter-02.png" alt-text="Abfrageergebnisse bei Verwendung mehrerer Filter" lightbox="media/log-analytics-tutorial/query-results-filter-02.png":::


## <a name="analyze-results"></a>Ergebnisanalysen
Zusätzlich zur Unterstützung beim Schreiben und Ausführen von Abfragen bietet Log Analytics Features, die Ihnen die Arbeit mit den Ergebnissen erleichtern. Erweitern Sie zunächst einen Datensatz, um die Werte für alle zugehörigen Spalten anzuzeigen.

:::image type="content" source="media/log-analytics-tutorial/expand-record.png" alt-text="Erweitern eines Datensatzes" lightbox="media/log-analytics-tutorial/expand-record.png":::

Klicken Sie auf den Namen einer beliebigen Spalte, um die Ergebnisse nach dieser Spalte zu sortieren. Klicken Sie auf das Filtersymbol neben dem Spaltennamen, um eine Filterbedingung anzugeben. Dies ist mit dem Hinzufügen einer Filterbedingung zur Abfrage selbst vergleichbar. Der einzige Unterschied besteht darin, dass dieser Filter bei der erneuten Ausführung der Abfrage gelöscht wird. Verwenden Sie diese Methode, wenn Sie eine Gruppe von Datensätzen im Rahmen der interaktiven Analyse schnell analysieren möchten.

Legen Sie beispielsweise einen Filter für die Spalte **DurationMs** fest, um die Datensätze zu beschränken, die mehr als **100** Millisekunden lang gedauert haben. 

:::image type="content" source="media/log-analytics-tutorial/query-results-filter.png" alt-text="Filter für Abfrageergebnisse" lightbox="media/log-analytics-tutorial/query-results-filter.png":::

Anstatt die Ergebnisse zu filtern, können Sie Datensätze auch nach einer bestimmten Spalte gruppieren. Löschen Sie den soeben erstellten Filter, und aktivieren Sie dann den Schieberegler **Spalten gruppieren**. 

:::image type="content" source="media/log-analytics-tutorial/query-results-group-columns.png" alt-text="Gruppen Spalte" lightbox="media/log-analytics-tutorial/query-results-group-columns.png":::

Ziehen Sie nun die Spalte **Url** in die Gruppierungszeile. Die Ergebnisse werden nun nach dieser Spalte organisiert, und Sie können die einzelnen Gruppen zu Analysezwecken reduzieren.

:::image type="content" source="media/log-analytics-tutorial/query-results-grouped.png" alt-text="Gruppierte Abfrageergebnisse" lightbox="media/log-analytics-tutorial/query-results-grouped.png":::

## <a name="work-with-charts"></a>Arbeiten mit Diagrammen
Sehen wir uns nun eine Abfrage mit numerischen Daten an, die in einem Diagramm angezeigt werden können. Anstatt eine Abfrage zu erstellen, wählen wir eine Beispielabfrage aus.

Klicken Sie im linken Bereich auf **Abfragen**. Dieser Bereich enthält Beispielabfragen, die Sie dem Abfragefenster hinzufügen können. Wenn Sie Ihren eigenen Arbeitsbereich verwenden, haben Sie wahrscheinlich eine Vielzahl von Abfragen in mehreren Kategorien. Bei der Verwendung der Demoumgebung sehen Sie jedoch möglicherweise nur eine einzelne Kategorie namens **Log Analytics-Arbeitsbereiche**. Erweitern Sie diese Kategorie, um die darin enthaltenen Abfragen anzuzeigen.

Klicken Sie in der Kategorie **Anwendungen** auf die Abfrage **Funktionsfehlerrate**. Dadurch wird die Abfrage dem Abfragefenster hinzugefügt. Beachten Sie, dass die neue Abfrage durch eine leere Zeile von der anderen Abfrage getrennt ist. Abfragen in KQL werden beendet, wenn sie auf eine leere Zeile treffen, und daher als separate Abfragen betrachtet. 

:::image type="content" source="media/log-analytics-tutorial/example-query.png" alt-text="Neue Abfrage" lightbox="media/log-analytics-tutorial/example-query.png":::

Die aktuelle Abfrage ist die, auf der der Cursor positioniert ist. Die erste Abfrage ist hervorgehoben, was bedeutet, dass dies die aktuelle Abfrage ist. Klicken Sie an einer beliebigen Stelle in der neuen Abfrage, um sie auszuwählen, und klicken Sie dann auf die Schaltfläche **Ausführen**, um sie auszuführen.

:::image type="content" source="media/log-analytics-tutorial/example-query-output-table.png" alt-text="Tabelle der Abfrageergebnisse" lightbox="media/log-analytics-tutorial/example-query-output-table.png":::

Um die Ergebnisse in einem Diagramm anzuzeigen, wählen Sie im Ergebnisbereich **Diagramm** aus.  Zum Arbeiten mit dem Diagramm stehen Ihnen verschiedene Optionen zur Verfügung, Sie können es z. B. in einen anderen Typ ändern.


:::image type="content" source="media/log-analytics-tutorial/example-query-output-chart.png" alt-text="Diagramm der Abfrageergebnisse" lightbox="media/log-analytics-tutorial/example-query-output-chart.png":::


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit Log Analytics vertraut gemacht haben, können Sie mit dem Tutorial zur Verwendung von Protokollabfragen fortfahren.
> [!div class="nextstepaction"]
> [Schreiben von Azure Monitor-Protokollabfragen](get-started-queries.md)
