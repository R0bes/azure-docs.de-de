---
title: Anleitung zur Leistung und Optimierung des Zuordnungsdatenflusses
titleSuffix: Azure Data Factory & Azure Synapse
description: Erfahren Sie, welche Faktoren sich entscheidend auf die Leistung von Zuordnungsdatenflüssen in Azure Data Factory- und Azure Synapse Analytics-Pipelines auswirken.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.subservice: data-flows
ms.custom: synapse
ms.date: 08/24/2021
ms.openlocfilehash: 1595d2984c4130fa89c52aec615941051fa1bb82
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2021
ms.locfileid: "123099353"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Zuordnungsdatenflüsse in Azure Data Factory- und Synapse-Pipelines bieten eine codefreie Benutzeroberfläche zum Entwerfen und Ausführen von Datentransformationen im großen Stil. Wenn Sie mit der Mapping Data Flow-Funktion nicht vertraut sind, finden Sie weitere Informationen in der [Übersicht über Mapping Data Flow](concepts-data-flow-overview.md). In diesem Artikel werden verschiedene Möglichkeiten beschrieben, wie Sie Ihre Datenflüsse so anpassen und optimieren können, dass sie Ihre Leistungsbenchmarks erreichen.

Sehen Sie sich das folgende Video an, das einige Beispiele für die erforderliche Dauer beim Transformieren von Daten mit Datenflüssen enthält:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rNxM]

## <a name="monitoring-data-flow-performance"></a>Überwachen der Datenflussleistung

Nachdem Sie Ihre Transformationslogik im Debugmodus überprüft haben, sollten Sie Ihren Datenfluss per End-to-End-Vorgang als Aktivität in einer Pipeline ausführen. Datenflüsse werden in einer Pipeline mit der [Aktivität zum Ausführen eines Datenflusses](control-flow-execute-data-flow-activity.md) operationalisiert. Die Datenflussaktivität verfügt im Gegensatz zu anderen Aktivitäten über eine einzigartige Überwachungsoberfläche, auf der ein ausführlicher Ausführungsplan und das Leistungsprofil der Transformationslogik angezeigt werden. Klicken Sie in der Ausgabe der Aktivitätsausführung einer Pipeline auf das Brillensymbol, um für den Datenfluss ausführliche Überwachungsinformationen anzuzeigen. Weitere Informationen finden Sie unter [Überwachen von Datenflüssen](concepts-data-flow-monitoring.md).

![Datenflussüberwachung](media/data-flow/monitoring-details.png "Datenflussüberwachung 2")

Beim Überwachen der Leistung von Datenflüssen gibt es vier mögliche Engpässe, auf die Sie achten sollten:

* Startzeit des Clusters
* Lesen von einer Quelle
* Transformationszeit
* Schreiben in eine Senke 

![Datenflussüberwachung](media/data-flow/monitoring-performance.png "Datenflussüberwachung 3")

Die Startzeit des Clusters ist die benötigte Dauer für den Start eines Apache Spark-Clusters. Dieser Wert befindet sich oben rechts auf dem Überwachungsbildschirm. Datenflüsse basieren auf einem Just-In-Time-Modell, bei dem für jeden Auftrag ein isolierter Cluster verwendet wird. Diese Startzeit hat normalerweise eine Dauer von drei bis fünf Minuten. Für sequenzielle Aufträge kann dies reduziert werden, indem ein Wert für die Gültigkeitsdauer aktiviert wird. Weitere Informationen finden Sie unter **Gültigkeitsdauer (TTL)** und [Integration Runtime-Leistung](concepts-integration-runtime-performance.md#time-to-live).

Für Datenflüsse wird ein Spark-Optimierer genutzt, mit dem Ihre Geschäftslogik in „Stufen“ neu sortiert und ausgeführt wird, damit die Dauer möglichst kurz ist. Für jede Senke, in die Ihr Datenfluss geschrieben wird, sind in der Ausgabe der Überwachung die Dauer der einzelnen Transformationsstufen sowie die Zeiten enthalten, die zum Schreiben der Daten in die Senke benötigt werden. Anhand der längsten Dauer können Sie wahrscheinlich den Engpass für Ihren Datenfluss ermitteln. Falls die Transformationsstufe mit der längsten Dauer eine Quelle enthält, sollten Sie überlegen, ob Sie Ihre Lesedauer weiter optimieren können. Wenn eine Transformation lange dauert, müssen Sie ggf. neu partitionieren oder die Größe für Ihre Integration Runtime erhöhen. Bei einer langen Dauer der Senkenverarbeitung müssen Sie unter Umständen Ihre Datenbank hochskalieren oder sich vergewissern, dass die Ausgabe nicht nur in eine einzelne Datei erfolgt.

Nachdem Sie den Engpass für Ihren Datenfluss identifiziert haben, sollten Sie die unten angegebenen Strategien nutzen, um die Leistung zu verbessern.

## <a name="testing-data-flow-logic"></a>Testen der Datenflusslogik

Beim Entwerfen und Testen von Datenflüssen über die Benutzeroberfläche können Sie im Debugmodus einen interaktiven Test für einen Spark-Livecluster durchführen. Dies ermöglicht Ihnen das Anzeigen einer Vorschau für die Daten und das Ausführen Ihrer Datenflüsse, ohne auf das „Aufwärmen“ eines Clusters warten zu müssen. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

## <a name="optimize-tab"></a>Registerkarte „Optimieren“

Die Registerkarte **Optimieren** enthält Einstellungen zum Konfigurieren des Partitionsschemas für den Spark-Cluster. Diese Registerkarte ist für jede Transformation des Datenflusses vorhanden und gibt an, ob Sie die Daten neu partitionieren sollten, **nachdem** die Transformation abgeschlossen ist. Das Anpassen der Partitionierung bietet Kontrolle über die Verteilung Ihrer Daten auf Computeknoten und Datenstandortoptimierungen, die sowohl positive als auch negative Auswirkungen auf die gesamte Datenflussleistung haben können.

![Screenshot der Registerkarte „Optimieren“ mit der Option „Partition“, dem Partitionstyp und der Anzahl von Partitionen](media/data-flow/optimize.png)

Standardmäßig ist die Option *Aktuelle Partitionierung verwenden* ausgewählt, die den Dienst anweist, die aktuelle Ausgabepartitionierung der Transformation beizubehalten. Da die erneute Partitionierung einige Zeit in Anspruch nimmt, ist die Verwendung der Option *Aktuelle Partitionierung verwenden* in den meisten Szenarien zu empfehlen. Szenarien, in denen Sie Ihre Daten ggf. neu partitionieren sollten, sind beispielsweise Aggregations- und Joinvorgänge, bei denen es für Ihre Daten zu einer erheblichen Datenschiefe kommt, oder bei Verwendung einer Quellpartitionierung auf einer SQL-Datenbank-Instanz.

Wenn Sie die Partitionierung für eine Transformation ändern möchten, wählen Sie die Registerkarte **Optimieren** und das Optionsfeld **Partitionierung festlegen** aus. Ihnen werden mehrere Optionen für die Partitionierung angezeigt. Die beste Methode der Partitionierung richtet sich jeweils nach den Datenmengen, Kandidatenschlüsseln, NULL-Werten und der Kardinalität. 

> [!IMPORTANT]
> Bei nur einer Partition werden darin alle verteilten Daten kombiniert. Hierbei handelt es sich um einen sehr langsamen Vorgang, bei dem auch alle nachgeschalteten Transformationen und Schreibvorgänge erheblich beeinträchtigt werden. Von dieser Option wird dringend abgeraten, es sei denn, es gibt einen expliziten geschäftlichen Grund, sie zu verwenden.

Die folgenden Partitionierungsoptionen sind in jeder Transformation verfügbar:

### <a name="round-robin"></a>Roundrobin 

Per Roundrobin werden Daten gleichmäßig auf die Partitionen verteilt. Verwenden Sie die Option „Roundrobin“, wenn Sie nicht über gute Kandidaten verfügen, um eine solide, intelligente Partitionierungsstrategie zu implementieren. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="hash"></a>Hash

Der Dienst erzeugt einen Hash von Spalten, um einheitliche Partitionen zu erstellen, damit Zeilen mit ähnlichen Werten in die gleiche Partition eingefügt werden. Führen Sie bei Verwendung der Option „Hash“ einen Test auf mögliche Partitionsungleichmäßigkeiten durch. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="dynamic-range"></a>Dynamischer Bereich

Bei dieser Option werden die dynamischen Spark-Bereiche basierend auf den von Ihnen angegebenen Spalten oder Ausdrücken verwendet. Sie können die Anzahl der physischen Partitionen festlegen. 

### <a name="fixed-range"></a>Fester Bereich

Erstellen Sie einen Ausdruck, der einen festen Bereich für Werte in Ihren partitionierten Datenspalten bereitstellt. Sie sollten über fundiertes Wissen über Ihre Daten verfügen, bevor Sie diese Option verwenden, um Partitionsungleichmäßigkeiten zu vermeiden. Die von Ihnen für den Ausdruck eingegebenen Werte werden als Teil einer Partitionsfunktion verwendet. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="key"></a>Schlüssel

Wenn Sie gut mit der Kardinalität Ihrer Daten vertraut sind, kann die Schlüsselpartitionierung eine gute Strategie darstellen. Die Schlüsselpartitionierung erstellt Partitionen für jeden eindeutigen Wert in der Spalte. Sie können die Anzahl der Partitionen nicht festlegen, weil die Anzahl auf den eindeutigen Werten in den Daten basiert.

> [!TIP]
> Beim manuellen Festlegen des Partitionierungsschemas werden die Daten neu angeordnet, sodass die Vorteile des Spark-Optimierers ggf. nicht voll zur Geltung kommen. Eine bewährte Methode besteht darin, die Partitionierung nicht manuell festzulegen, sofern dies nicht unbedingt erforderlich ist.

## <a name="logging-level"></a>Protokolliergrad

Wenn Sie nicht voraussetzen, dass jede Pipelineausführung Ihrer Datenflussaktivitäten alle ausführlichen Telemetrieprotokolle vollständig protokolliert, können Sie den Protokolliergrad optional auf „Standard“ oder „Kein“ festlegen. Wenn Sie Ihre Datenflüsse im Modus „Ausführlich“ (Standard) ausführen, fordern Sie an, dass der Dienst die Aktivität während der Datentransformation auf den einzelnen Partitionsebenen vollständig protokolliert. Da dies ein kostspieliger Vorgang sein kann, kann nur die ausschließliche Aktivierung von „Ausführlich“ bei der Problembehandlung den gesamten Datenfluss und die Pipelineleistung verbessern. Der Modus „Standard“ protokolliert nur die Transformationszeitspannen, während „Kein“ nur eine Zusammenfassung der Zeitspannen bietet.

![Protokolliergrad](media/data-flow/logging.png "Festlegen des Protokolliergrads")

## <a name="optimizing-sources"></a>Optimieren von Quellen

Für jede Quelle – mit Ausnahme von Azure SQL-Datenbank – lautet die Empfehlung, die Option **Aktuelle Partitionierung verwenden** ausgewählt zu lassen. Beim Lesen von allen anderen Quellsystemen werden Daten von Datenflüssen je nach Größe automatisch gleichmäßig partitioniert. Für ca. 128 MB an Daten wird jeweils eine neue Partition erstellt. Wenn die Datengröße zunimmt, steigt die Anzahl von Partitionen.

Alle benutzerdefinierten Partitionierungen werden durchgeführt, *nachdem* die Daten von Spark eingelesen wurden. Dies wirkt sich negativ auf die Leistung Ihres Datenflusses aus. Dies ist nicht zu empfehlen, da die Daten beim Lesen gleichmäßig partitioniert sind. 

> [!NOTE]
> Die Lesegeschwindigkeiten können aufgrund des Durchsatzes Ihres Quellsystems begrenzt sein.

### <a name="azure-sql-database-sources"></a>Azure SQL-Datenbank-Quellen

Azure SQL-Datenbank verfügt über eine eindeutige Partitionierungsoption, die die Bezeichnung „Quellpartitionierung“ hat. Mit dem Aktivieren der Quellpartitionierung können Sie Ihre Lesedauern für Azure SQL-Datenbank verbessern, indem Sie auf dem Quellsystem parallele Verbindungen aktivieren. Geben Sie die Anzahl von Partitionen und die Partitionierung Ihrer Daten an. Verwenden Sie eine Partitionsspalte mit hoher Kardinalität. Sie können auch eine Abfrage eingeben, die zum Partitionierungsschema Ihrer Quelltabelle passt.

> [!TIP]
> Bei der Quellpartitionierung ist der E/A-Vorgang von SQL Server der Engpass. Das Hinzufügen von zu vielen Partitionen kann dazu führen, dass die Quelldatenbank vollständig ausgelastet ist. Im Allgemeinen sind bei Verwendung dieser Option vier oder fünf Partitionen ideal.

![Quellpartitionierung](media/data-flow/sourcepart3.png "Quellpartitionierung")

#### <a name="isolation-level"></a>Isolationsstufe

Die Isolationsstufe des Lesevorgangs auf einem Azure SQL-Quellsystem hat eine Auswirkung auf die Leistung. Wenn Sie die Option „Lesen ohne Commit“ auswählen, ergibt sich die schnellste Leistung, und Datenbanksperren werden verhindert. Weitere Informationen zu SQL-Isolationsstufen finden Sie unter [Grundlegendes zu Isolationsstufen](/sql/connect/jdbc/understanding-isolation-levels).

#### <a name="read-using-query"></a>Lesen per Abfrage

Sie können Lesevorgänge aus Azure SQL-Datenbank durchführen, indem Sie eine Tabelle oder SQL-Abfrage verwenden. Wenn Sie eine SQL-Abfrage ausführen, muss diese beendet werden, bevor die Transformation gestartet werden kann. SQL-Abfragen können nützlich sein, um einen Pushdown für Vorgänge durchzuführen, damit diese dann ggf. schneller ausgeführt werden, und um die Menge der von einer SQL Server-Instanz gelesenen Daten zu reduzieren, z. B. SELECT-, WHERE- und JOIN-Anweisungen. Bei einem Pushdown von Vorgängen ist es nicht mehr möglich, die Herkunft und Leistung der Transformationen nachzuverfolgen, bevor die Daten in den Datenfluss gelangen.

### <a name="azure-synapse-analytics-sources"></a>Azure Synapse Analytics-Quellen

Bei Verwendung von Azure Synapse Analytics ist in den Quelloptionen die Einstellung **Staging aktivieren** vorhanden. Dadurch kann der Dienst mithilfe von ```Staging``` aus Synapse lesen. Dies verbessert die Leseleistung erheblich, da die [Synapse-COPY-Anweisung](/sql/t-sql/statements/copy-into-transact-sql.md) als leistungsstärkste Funktion zum Massenladen verwendet wird. Beim Aktivieren von ```Staging``` müssen Sie in den Einstellungen für Datenflussaktivitäten einen Azure Blob Storage- oder Azure Data Lake Storage Gen2-Stagingspeicherort angeben.

![Staging aktivieren](media/data-flow/enable-staging.png "Staging aktivieren")

### <a name="file-based-sources"></a>Dateibasierte Quellen

Für Datenflüsse werden zwar verschiedene Dateitypen unterstützt, zum Erzielen von optimalen Lese- und Schreibdauern wird aber das native Spark-Format „Parquet“ empfohlen.

Wenn Sie den gleichen Datenfluss für mehrere Dateien ausführen, empfehlen wir Ihnen, die Daten aus einem Ordner zu lesen, Platzhalterpfade zu verwenden oder die Daten aus einer Liste mit Dateien zu lesen. Mit nur einer Datenfluss-Aktivitätsausführung können Ihre gesamten Dateien als Batch verarbeitet werden. Weitere Informationen zum Konfigurieren dieser Einstellungen finden Sie in der Dokumentation zum [Azure Blob Storage-Connector](connector-azure-blob-storage.md#source-transformation) im Abschnitt **Quellentransformation**.

Vermeiden Sie nach Möglichkeit die Verwendung der For-Each-Aktivität, um Datenflüsse für mehrere Dateien auszuführen. Dies führt dazu, dass bei jeder Iteration des For-Each-Vorgangs ein eigener Spark-Cluster gestartet wird. Häufig ist dies nicht nötig, und diese Vorgehensweise kann teuer werden. 

## <a name="optimizing-sinks"></a>Optimieren von Senken

Wenn Datenflüsse in Senken schreiben, werden alle benutzerdefinierten Partitionierungen direkt vor dem Schreibvorgang durchgeführt. Wie auch bei der Quelle, ist es in den meisten Fällen zu empfehlen, die Auswahl der Partitionierungsoption **Aktuelle Partitionierung verwenden** beizubehalten. Partitionierte Daten werden deutlich schneller als nicht partitionierte Daten geschrieben. Dies gilt auch, wenn Ihr Ziel nicht partitioniert ist. Unten sind die einzelnen Aspekte für die verschiedenen Senkentypen angegeben. 

### <a name="azure-sql-database-sinks"></a>Azure SQL-Datenbank-Senken

Bei Azure SQL-Datenbank sollte die Standardpartitionierung in den meisten Fällen funktionieren. Es besteht die Möglichkeit, dass Ihre Senke über zu viele Partitionen verfügt, sodass sie von Ihrer SQL-Datenbank-Instanz nicht mehr verarbeitet werden können. Reduzieren Sie in diesem Fall die Anzahl von Partitionen, die von Ihrer SQL-Datenbank-Senke ausgegeben werden.

#### <a name="impact-of-error-row-handling-to-performance"></a>Auswirkung der Fehlerzeilenbehandlung auf die Leistung

Wenn Sie die Fehlerzeilenbehandlung („Bei Fehler fortsetzen“) in der Senkentransformation aktivieren, führt der Dienst einen zusätzlichen Schritt aus, bevor die kompatiblen Zeilen in die Zieltabelle geschrieben werden. Dieser zusätzliche Schritt führt zu einer geringen Leistungseinbuße, die für diesen Schritt mit etwa 5 % veranschlagt werden kann. Eine zusätzliche kleine Leistungseinbuße tritt auf, wenn Sie auch die Option für inkompatible Zeilen für eine Protokolldatei festlegen.

#### <a name="disabling-indexes-using-a-sql-script"></a>Deaktivieren von Indizes per SQL-Skript

Durch das Deaktivieren von Indizes vor dem Laden in eine SQL-Datenbank kann die Leistung beim Schreiben in die Tabelle erheblich verbessert werden. Führen Sie den unten angegebenen Befehl aus, bevor Sie in Ihre SQL-Senke schreiben.

`ALTER INDEX ALL ON dbo.[Table Name] DISABLE`

Nachdem der Schreibvorgang abgeschlossen ist, sollten Sie die Indizes mit dem folgenden Befehl neu erstellen:

`ALTER INDEX ALL ON dbo.[Table Name] REBUILD`

Dies kann sowohl nativ mit Pre- und Post-SQL-Skripts auf einer Azure SQL-Datenbank-Instanz als auch über eine Synapse-Senke in Zuordnungsdatenflüssen durchgeführt werden.

![Indizes deaktivieren](media/data-flow/disable-indexes-sql.png "Indizes deaktivieren")

> [!WARNING]
> Beim Deaktivieren von Indizes übernimmt der Datenfluss quasi die Kontrolle über eine Datenbank, und die Durchführung von Abfragen ist dann wahrscheinlich nicht erfolgreich. Aus diesem Grund werden viele ETL-Aufträge nachts ausgelöst, um Konflikte dieser Art zu vermeiden. Weitere Informationen finden Sie im Artikel [Deaktivieren von SQL-Indizes und Einschränkungen](/sql/relational-databases/indexes/disable-indexes-and-constraints).

#### <a name="scaling-up-your-database"></a>Hochskalieren Ihrer Datenbank

Planen Sie die Anpassung der Größe Ihrer Quelle und Senke in Azure SQL-Datenbank und Azure SQL Data Warehouse vor der Pipelineausführung, um den Durchsatz zu erhöhen und die Drosselung durch Azure beim Erreichen der DTU-Grenzwerte zu minimieren. Stellen Sie nach Abschluss der Pipelineausführung die Größe Ihrer Datenbanken für die normale Ausführung wieder her.

### <a name="azure-synapse-analytics-sinks"></a>Azure Synapse Analytics-Senken

Stellen Sie beim Schreiben in Azure Synapse Analytics sicher, dass die Option **Staging aktivieren** aktiviert ist. Hierdurch wird für den Dienst das Schreiben mit dem [SQL-Befehl COPY](/sql/t-sql/statements/copy-into-transact-sql) ermöglicht, sodass für die Daten praktisch ein Massenladevorgang erfolgt. Bei Verwendung von Staging müssen Sie auf ein Azure Data Lake Storage Gen2- oder Azure Blob Storage-Konto für das Staging der Daten verweisen.

Mit Ausnahme von Staging gelten für Azure Synapse Analytics die gleichen bewährten Methoden wie für Azure SQL-Datenbank.

### <a name="file-based-sinks"></a>Dateibasierte Senken 

Für Datenflüsse werden zwar verschiedene Dateitypen unterstützt, zum Erzielen von optimalen Lese- und Schreibdauern wird aber das native Spark-Format „Parquet“ empfohlen.

Wenn die Daten gleichmäßig verteilt sind, ist **Aktuelle Partitionierung verwenden** die schnellste Partitionierungsoption für das Schreiben von Dateien.

#### <a name="file-name-options"></a>Dateinamenoptionen

Beim Schreiben von Dateien können Sie zwischen verschiedenen Benennungsoptionen wählen, die sich jeweils auf die Leistung auswirken.

![Senkenoptionen](media/data-flow/file-sink-settings.png "Senkenoptionen")

Wenn Sie die Option **Standard** auswählen, ist der Schreibvorgang am schnellsten. Jede Partition entspricht einer Datei mit dem Standardnamen von Spark. Dies ist hilfreich, wenn Sie nur aus dem Ordner mit den Daten lesen.

Wenn Sie ein **Muster** für die Benennung auswählen, wird jede Partitionsdatei mit einem benutzerfreundlicheren Namen benannt. Dieser Vorgang erfolgt nach dem Schreibvorgang und ist etwas langsamer als bei der Option „Standard“. Es gibt auch eine Option, bei der Sie jede einzelne Partition manuell benennen können.

Falls eine Spalte bereits die gewünschte Form für die Ausgabe der Daten aufweist, können Sie die Option **Wie Daten in Spalte** auswählen. Die Daten werden dann neu angeordnet und können die Leistung beeinträchtigen, falls die Spalten nicht gleichmäßig verteilt sind.

Bei **Ausgabe in eine einzelne Datei** werden alle Daten in einer Partition kombiniert. Dies führt besonders für große Datasets zu langen Schreibzeiten. Von dieser Option wird dringend abgeraten, es sei denn, es gibt einen expliziten geschäftlichen Grund, sie zu verwenden.

### <a name="cosmosdb-sinks"></a>Cosmos DB-Senken

Beim Schreiben in Cosmos DB kann die Leistung verbessert werden, indem der Durchsatz und die Batchgröße während der Ausführung des Datenflusses geändert werden. Diese Änderungen sind nur während der Datenfluss-Aktivitätsausführung wirksam und werden nach Abschluss des Vorgangs auf die ursprünglichen Sammlungseinstellungen zurückgesetzt. 

**Batchgröße:** Normalerweise reicht als Startwert die Batchgröße aus. Um diesen Wert weiter anzupassen, berechnen Sie die ungefähre Objektgröße Ihrer Daten, und stellen Sie sicher, dass das Produkt aus Objektgröße und Batchgöße kleiner als 2 MB ist. Erhöhen Sie andernfalls die Batchgröße, um einen besseren Durchsatz zu erzielen.

**Durchsatz:** Legen Sie hier einen höheren Durchsatz fest, damit Dokumente schneller in Cosmos DB geschrieben werden können. Beachten Sie die höheren RU-Kosten bei einer höheren Durchsatzeinstellung.

**Write throughput budget** (Schreibdurchsatz): Verwenden Sie einen Wert, der kleiner als die Gesamtanzahl der RUs pro Minute ist. Wenn Ihr Datenfluss eine hohe Anzahl von Spark-Partitionen enthält, können Sie durch das Festlegen eines Durchsatzbudgets eine bessere Balance zwischen diesen Partitionen erzielen.

## <a name="optimizing-transformations"></a>Optimieren von Transformationen

### <a name="optimizing-joins-exists-and-lookups"></a>Optimieren von Joins, Exists-Transformationen und Suchvorgängen

#### <a name="broadcasting"></a>Übertragung

Wenn bei Joins, Suchvorgängen und Exists-Transformationen der Arbeitsspeicher des Workerknotens groß genug für einen oder beide Datenströme ist, können Sie die Leistung optimieren, indem Sie die **Übertragung** aktivieren. Bei einer Übertragung senden Sie kleine Datenrahmen an alle Knoten im Cluster. Dies ermöglicht für die Spark-Engine die Durchführung eines Joinvorgangs, ohne dass die Daten im großen Datenstrom neu angeordnet werden. Standardmäßig entscheidet die Spark-Engine automatisch, ob eine Seite eines Joins übertragen werden soll. Wenn Sie mit Ihren eingehenden Daten vertraut sind und wissen, dass ein Datenstrom erheblich kleiner als der andere ist, können Sie für die Übertragung die Option **Feststehend** auswählen. Bei der feststehenden Übertragung wird Spark gezwungen, den ausgewählten Datenstrom zu übertragen. 

Wenn die Größe der übertragenen Daten für den Spark-Knoten zu hoch ist, tritt ggf. ein Fehler vom Typ „Nicht genügend Arbeitsspeicher“ auf. Verwenden Sie Cluster vom Typ **Arbeitsspeicheroptimiert**, um Fehler vom Typ „Nicht genügend Arbeitsspeicher“ zu vermeiden. Wenn bei den Datenflussausführungen Übertragungstimeouts auftreten, können Sie die Broadcastoptimierung deaktivieren. Dies führt jedoch zu Datenflüssen mit geringerer Leistung.

Für die Arbeit mit Datenquellen, die mehr Zeit für Abfragen erfordern (z. B. große Datenbankabfragen), wird es empfohlen, die Übertragung für Joins zu deaktivieren. Quellen mit langen Abfragezeiten können zu Spark-Timeouts führen, wenn der Cluster versucht, Daten an Computeknoten zu übertragen. Das Deaktivieren der Übertragung ist außerdem eine gute Entscheidung, wenn Sie über einen Datenstrom in Ihrem Datenfluss verfügen, der Werte zur späteren Verwendung in einer Suchtransformation aggregiert. Dieses Muster kann den Spark-Optimierer verwirren und zu Timeouts führen.

![Optimieren der Join-Transformation](media/data-flow/joinoptimize.png "Join-Optimierung")

#### <a name="cross-joins"></a>Kreuzprodukte

Wenn Sie Literalwerte in Ihren Verknüpfungsbedingungen verwenden oder auf beiden Seiten eines Joins über mehrere Übereinstimmungen verfügen, führt Spark den Joinvorgang als „Kreuzprodukt“ aus. Ein Kreuzprodukt ist ein vollständiges kartesisches Produkt, mit dem dann die verknüpften Werte herausgefiltert werden. Dieser Vorgang ist deutlich langsamer als andere Jointypen. Stellen Sie sicher, dass Sie auf beiden Seiten Ihrer Verknüpfungsbedingungen über Spaltenverweise verfügen, um eine Beeinträchtigung der Leistung zu vermeiden.

#### <a name="sorting-before-joins"></a>Sortieren vor Joinvorgängen

Im Gegensatz zu „Merge Join“ in Tools wie SSIS ist die Join-Transformation kein obligatorischer Vorgang vom Typ „Merge Join“. Für die Joinschlüssel ist vor der Transformation kein Sortiervorgang erforderlich. Die Verwendung von „Transformationen sortieren“ wird in Zuordnungsdatenflüssen nicht empfohlen.

### <a name="window-transformation-performance"></a>Leistung der Fenstertransformation

Die [Fenstertransformation im Zuordnungsdatenfluss](data-flow-window.md) partitioniert die Daten nach dem Wert in Spalten, die Sie als Teil der ```over()```-Klausel in den Transformationseinstellungen auswählen. Es gibt eine Reihe sehr gängiger Aggregat- und Analysefunktionen, die in der Fenstertransformation bereitgestellt werden. Wenn Ihr Anwendungsfall aber darin besteht, ein Fenster für das gesamte Dataset für die Rangfolge ```rank()``` oder die Zeilennummer ```rowNumber()``` zu generieren, empfiehlt es sich, stattdessen die [Rangtransformation](data-flow-rank.md) und die [Transformation für Ersatzschlüssel](data-flow-surrogate-key.md) zu verwenden. Die Transformation bietet bei Verwendung dieser Funktionen eine bessere Leistung bei Vorgängen für das gesamte Dataset.

### <a name="repartitioning-skewed-data"></a>Erneutes Partitionieren bei Datenschiefe

Bei bestimmten Transformationen, z. B. Joins und Aggregatvorgängen, werden Ihre Datenpartitionen neu angeordnet und können gelegentlich zu Datenschiefe führen. Datenschiefe bedeutet, dass Daten nicht gleichmäßig auf die Partitionen verteilt werden. Bei hoher Datenschiefe kann es zu einer Verlangsamung von nachgeschalteten Transformationen und Senkenschreibvorgängen kommen. Sie können die Datenschiefe bei Ihnen während einer Datenflussausführung jederzeit überprüfen, indem Sie in der Überwachungsanzeige auf die Transformation klicken.

![Schiefe und Wölbung](media/data-flow/skewness-kurtosis.png "Schiefe und Wölbung")

In der Überwachungsanzeige wird angegeben, wie die Daten auf die einzelnen Partitionen verteilt sind, und die beiden Metriken für „Schiefe“ und „Wölbung“ werden angezeigt. Die **Schiefe** ist ein Maß dafür, wie asymmetrisch die Daten sind. Der Wert kann „Positiv“, „Null“, „Negativ“ oder „Nicht definiert“ lauten. Bei einer negativen Datenschiefe ist der linke Ausläufer länger als der rechte Ausläufer. Die **Wölbung** ist das Maß dafür, ob die Daten eine Verteilung mit schweren oder leichten Rändern aufweisen. Hohe Wölbungswerte sind nicht wünschenswert. Ideale Bereiche für die Datenschiefe liegen zwischen -3 und 3, und bei der Wölbung werden Werte angestrebt, die unter 10 liegen. Eine einfache Möglichkeit zur Interpretation dieser Zahlen besteht darin, im Partitionsdiagramm zu überprüfen, ob ein Balken deutlich größer als die anderen ist.

Falls Ihre Daten nach einer Transformation nicht gleichmäßig partitioniert sind, können Sie die [Registerkarte „Optimieren“](#optimize-tab) verwenden, um sie neu zu partitionieren. Die Neuanordnung von Daten nimmt Zeit in Anspruch und führt ggf. nicht zu einer Verbesserung der Datenflussleistung.

> [!TIP]
> Wenn Sie Ihre Daten neu partitionieren, aber die Daten bei Ihnen von nachgeschalteten Transformationen dann neu angeordnet werden, sollten Sie die Hashpartitionierung für eine Spalte verwenden, die als Joinschlüssel genutzt wird.

## <a name="using-data-flows-in-pipelines"></a>Verwenden von Datenflüssen in Pipelines 

Wenn Sie komplexe Pipelines mit mehreren Datenflüssen erstellen, kann der logische Datenfluss große Auswirkungen auf Zeit und Kosten haben. In diesem Abschnitt werden die Auswirkungen verschiedener Architekturstrategien behandelt.

### <a name="executing-data-flows-in-parallel"></a>Paralleles Ausführen von Datenflüssen

Wenn Sie mehrere Datenflüsse parallel ausführen, richtet der Dienst separate Spark-Cluster für jede Aktivität ein. Dadurch kann jeder Auftrag isoliert und parallel ausgeführt werden, führt aber dazu, dass mehrere Cluster gleichzeitig ausgeführt werden.

Bei paralleler Ausführung Ihrer Datenflüsse wird empfohlen, die Eigenschaft für die Gültigkeitsdauer der Azure IR nicht zu aktivieren, da diese zu mehreren nicht verwendeten aktiven Pools führt.

> [!TIP]
> Anstatt denselben Datenfluss mehrmals für jede Aktivität auszuführen, können Sie die Daten in einem Data Lake bereitstellen und Platzhalterpfade verwenden, um die Daten in einem einzelnen Datenfluss zu verarbeiten.

### <a name="execute-data-flows-sequentially"></a>Sequenzielles Ausführen von Datenflüssen

Wenn Sie Ihre Datenflussaktivitäten nacheinander ausführen, empfiehlt es sich, in der Azure IR-Konfiguration eine Gültigkeitsdauer festzulegen. Die Computeressourcen werden vom Dienst wiederverwendet, wodurch die Startzeit des Clusters verkürzt wird. Jede Aktivität wird weiterhin isoliert und erhält einen neuen Spark-Kontext für jede Ausführung. Aktivieren Sie das Kontrollkästchen „Schnelle Wiederverwendung“ auf der Azure Integration Runtime-Instanz, um den Dienst anzuweisen, den vorhandenen Cluster erneut zu verwenden. So können Sie die Dauer zwischen sequenziellen Aktivitäten noch weiter reduzieren.

### <a name="overloading-a-single-data-flow"></a>Überladen eines einzelnen Datenflusses

Wenn Sie die gesamte Logik innerhalb eines einzelnen Datenflusses anordnen, führt der Dienst den gesamten Auftrag auf einer einzelnen Spark-Instanz aus. Dies mag zwar als eine Möglichkeit zur Kostenreduzierung erscheinen, doch werden unterschiedliche logische Datenflüsse kombiniert und das Überwachen und Debuggen kann schwierig sein. Wenn eine Komponente ausfällt, können auch alle anderen Teile des Auftrags nicht ausgeführt werden. Es wird empfohlen, Datenflüsse nach unabhängigen Flows der Geschäftslogik zu organisieren. Wenn der Datenfluss zu groß wird, werden Überwachung und Debugging durch Aufteilen in separate Komponenten erleichtert. Es gibt zwar kein festes Limit für die Anzahl von Transformationen in einem Datenfluss, doch wird der Auftrag bei zu vielen Transformationen sehr komplex.

### <a name="execute-sinks-in-parallel"></a>Paralleles Ausführen von Senken

Beim Standardverhalten von Datenflusssenken wird jede Senke sequenziell nacheinander ausgeführt, und der Datenfluss schlägt fehl, wenn ein Fehler in der Senke auftritt. Außerdem werden alle Senken standardmäßig der gleichen Gruppe zugeordnet, es sei denn, Sie bearbeiten die Datenflusseigenschaften und legen unterschiedliche Prioritäten für die Senken fest.

Datenflüsse ermöglichen es Ihnen, Senken über die Registerkarte für Datenflusseigenschaften im Benutzeroberflächendesigner in Gruppen zusammenfassen. Sie können sowohl die Ausführungsreihenfolge der Senken festlegen als auch Senken unter Verwendung derselben Gruppennummer gruppieren. Um die Verwaltung von Gruppen zu erleichtern, können Sie den Dienst anweisen, Senken aus der gleichen Gruppe parallel auszuführen.

In der Pipeline zum Ausführen der Datenflussaktivität befindet sich unter dem Abschnitt „Senkeneigenschaften“ eine Option, mit der das parallele Laden von Senken aktiviert werden kann. Wenn Sie die parallele Ausführung aktivieren, weisen Sie Datenflüsse an, gleichzeitig und nicht sequenziell in verbundene Senken zu schreiben. Um die Option für parallele Ausführung zu verwenden, müssen die Senken in einer Gruppe zusammengefasst und über eine neue Verzweigung oder bedingte Teilung mit demselben Datenstrom verbunden sein.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel zu Datenflüssen in Bezug auf die Leistung:

- [Datenflussaktivität](control-flow-execute-data-flow-activity.md)
- [Überwachen der Datenflussleistung](concepts-data-flow-monitoring.md)
- [Leistung der Integration Runtime](concepts-integration-runtime-performance.md)
