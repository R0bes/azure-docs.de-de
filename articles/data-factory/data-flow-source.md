---
title: Quelltransformation in einem Zuordnungsdatenfluss
titleSuffix: Azure Data Factory & Azure Synapse
description: Hier erfahren Sie, wie Sie eine Quelltransformation in einem Zuordnungsdatenfluss in Azure Data Factory- oder Azure Synapse Analytics-Pipelines einrichten können.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.subservice: data-flows
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/24/2021
ms.openlocfilehash: 88765f756e5473243cb22719c00d1e497fbd6ea6
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2021
ms.locfileid: "122822193"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Quelltransformation in einem Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mit einer Quelltransformation wird die Datenquelle für den Datenfluss konfiguriert. Beim Entwerfen von Datenflüssen besteht der erste Schritt stets in der Konfiguration einer Quelltransformation. Um eine Quelle hinzuzufügen, wählen Sie auf der Datenflusscanvas das Feld **Quelle hinzufügen** aus.

Jeder Datenfluss erfordert mindestens eine Quelltransformation, aber Sie können so viele Quellen wie erforderlich hinzufügen, um Ihre Datentransformationen abzuschließen. Sie können diese Quellen zusammen mit einer Join-, Lookup- oder Union-Transformation verwenden.

Jede Quelltransformation ist genau einem Dataset oder einem verknüpften Dienst zugeordnet. Das Dataset definiert die Form und Position der Daten, in die geschrieben oder aus denen gelesen werden soll. Wenn Sie ein dateibasiertes Dataset verwenden, können Sie Platzhalter und Dateilisten in Ihrer Quelle verwenden, um mit mehreren Dateien gleichzeitig zu arbeiten.

## <a name="inline-datasets"></a>Inline-Datasets

Die erste Entscheidung, die Sie beim Erstellen einer Quelltransformation treffen, ist die, ob Ihre Quellinformationen innerhalb eines Datasetobjekts oder innerhalb der Quelltransformation definiert sind. Die meisten Formate sind nur in einem von beiden verfügbar. Weitere Informationen zur Verwendung eines bestimmten Connectors finden Sie im Dokument zu diesem Connector.

Wenn ein Format sowohl für Inline- als auch in einem Datasetobjekt unterstützt wird, ergeben sich Vorteile für beide. Datasetobjekte sind wiederverwendbare Entitäten, die in anderen Datenflüssen und Aktivitäten wie Kopiervorgängen genutzt werden können. Diese wiederverwendbaren Entitäten sind besonders nützlich, wenn Sie ein verstärktes Schema verwenden. Datasets befinden sich nicht in Spark. Gelegentlich müssen Sie bestimmte Einstellungen oder Schemaprojektionen in der Quelltransformation überschreiben.

Inline-Datasets werden empfohlen, wenn flexible Schemas, einmalige Quellinstanzen oder parametrisierte Quellen verwendet werden. Wenn Ihre Quelle stark parametrisiert ist, können Sie mit Inline-Datasets kein „Dummy“-Objekt erstellen. Inline-Datasets befinden sich in Spark, und ihre Eigenschaften sind für den Datenfluss nativ.

Um ein Inline-Dataset zu verwenden, wählen Sie das gewünschte Format im Selektor **Quelltyp** aus. Anstatt ein Quelldataset auszuwählen, wählen Sie den verknüpften Dienst aus, mit dem Sie eine Verbindung herstellen möchten.

![Screenshot mit hervorgehobener Auswahl von „Inline“.](media/data-flow/inline-selector.png "Screenshot mit hervorgehobener Auswahl von „Inline“")

## <a name="workspace-db-synapse-workspaces-only"></a>Workspace DB (nur Synapse-Arbeitsbereiche)

In Azure Synapse-Arbeitsbereichen gibt es in Quelltransformationen von Datenflüssen die zusätzliche Option ```Workspace DB```. Sie ermöglicht es Ihnen, eine Arbeitsbereichsdatenbank jedes verfügbaren Typs als Ihre Quelldaten auszuwählen, ohne dass zusätzliche verknüpfte Dienste oder Datasets erforderlich sind.

> [!NOTE]
> Der Workspace DB-Connector von Azure Synapse befindet sich zurzeit in der öffentlichen Vorschau und funktioniert nur bei Spark Lake-Datenbanken.

![Screenshot mit hervorgehobener Auswahl „workspacedb“](media/data-flow/syms-source.png "Screenshot mit hervorgehobener Auswahl von „workspace DB“")

##  <a name="supported-source-types"></a><a name="supported-sources"></a> Unterstützte Quelltypen

Der Zuordnungsdatenfluss folgt einem Ansatz zum Extrahieren, Laden und Transformieren (ELT) und funktioniert mit *Stagingdatasets* in Azure. Derzeit können die folgenden Datasets in einer Quelltransformation verwendet werden.

| Connector | Format | Dataset/Inline |
| --------- | ------ | -------------- |
| [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Delta](format-delta.md)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties) <br>[ORC](format-orc.md#mapping-data-flow-properties)<br/>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties) | ✓/✓<br>✓/✓<br>✓/✓<br>✓/✓<br/>✓/✓<br>✓/✓<br/>✓/✓<br>✓/✓ |
| [Azure Cosmos DB (SQL-API)](connector-azure-cosmos-db.md#mapping-data-flow-properties) | | ✓/- |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties)<br>[ORC](format-orc.md#mapping-data-flow-properties)<br/>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties) | ✓/✓<br>✓/✓<br>✓/✓<br/>✓/✓<br>✓/✓<br/>✓/✓<br>✓/✓ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Common Data Model](format-common-data-model.md#source-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Delta](format-delta.md)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties)<br>[ORC](format-orc.md#mapping-data-flow-properties)<br/>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties) | ✓/✓<br/>-/✓<br>✓/✓<br>✓/✓<br>✓/✓<br>✓/✓<br/>✓/✓<br/>✓/✓<br>✓/✓ |
| [Azure Database for MySQL](connector-azure-database-for-mysql.md) |  | ✓/✓ |
| [Azure-Datenbank für PostgreSQL](connector-azure-database-for-postgresql.md) |  | ✓/✓ |
| [Azure SQL-Datenbank](connector-azure-sql-database.md#mapping-data-flow-properties) | | ✓/✓ |
| [Verwaltete Azure SQL-Datenbank-Instanz](connector-azure-sql-managed-instance.md#mapping-data-flow-properties) | | ✓/✓ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties) | | ✓/✓ |
| [Hive](connector-hive.md#mapping-data-flow-properties) | | -/✓ |
| [Snowflake](connector-snowflake.md) | | ✓/✓ |
| [SQL Server](connector-sql-server.md) | | ✓/✓ |

Die für diese Connectors spezifischen Einstellungen befinden sich auf der Registerkarte **Quelloptionen**. Informationen und Beispiele zu Datenflussskripts zu diesen Einstellungen finden Sie in der Connectordokumentation.

Azure Data Factory- und Synapse-Pipelines haben Zugriff auf mehr als [90 native Connectors](connector-overview.md). Um Daten aus diesen anderen Quellen in Ihren Datenfluss einzubeziehen, verwenden Sie die Kopieraktivität, um die Daten in einen der unterstützten Stagingbereiche zu laden.

## <a name="source-settings"></a>Quelleinstellungen

Nachdem Sie eine Quelle hinzugefügt haben, konfigurieren Sie sie auf der Registerkarte **Quelleinstellungen**. Hier können Sie das Dataset auswählen oder erstellen, auf das Ihre Quelle verweist. Sie können auch das Schema und die Samplingoptionen für Ihre Daten auswählen.

Entwicklungswerte für Datasetparameter können in [Debugeinstellungen](concepts-data-flow-debug-mode.md) konfiguriert werden. (Der Debugmodus muss aktiviert sein.)

![Screenshot: Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Screenshot: Registerkarte „Quelleinstellungen“")

**Name des Ausgabedatenstroms:** Der Name der Quelltransformation.

**Quellentyp**: Wählen Sie aus, ob Sie ein Inline-Dataset oder ein bestehendes Datasetobjekt verwenden möchten.

**Verbindung testen:** Testen Sie, ob der Spark-Dienst des Datenflusses erfolgreich eine Verbindung mit dem verknüpften Dienst herstellen kann, der in Ihrem Quelldataset verwendet wird. Damit diese Funktion aktiviert werden kann, muss der Debugmodus aktiviert sein.

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit des Diensts, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne Spaltenänderungen explizit definieren zu müssen.

* Aktivieren Sie das Kontrollkästchen **Schemaabweichung zulassen**, wenn die Quellspalten sich häufig ändern. Diese Einstellung ermöglicht es, dass alle eingehenden Quellfelder durch die Transformationen zur Senke fließen.

* Durch Aktivieren von **Abweichende Spaltentypen ableiten** wird der Dienst angewiesen, Datentypen für jede neue erkannte Spalte zu ermitteln und zu definieren. Wenn dieses Feature deaktiviert ist, weisen alle abweichende Spalten den Typ „String“ auf.

**Schema überprüfen**: Wenn **Schema überprüfen** aktiviert ist, führt das Ausführen des Datenflusses zu einem Fehler, wenn die eingehenden Daten nicht mit dem definierten Schema des Datasets übereinstimmen.

**Anzahl zu überspringender Zeilen**: Das Feld **Anzahl zu überspringender Zeilen** gibt an, wie viele Zeilen am Anfang des Datasets ignoriert werden sollen.

**Stichprobenerstellung**: Aktivieren Sie **Sampling**, um die Anzahl der Zeilen aus der Quelle zu beschränken. Verwenden Sie diese Einstellung, wenn Sie für das Debuggen Stichproben der Daten an der Quelle erstellen möchten. Dies ist beim Ausführen von Datenflüssen im Debugmodus über eine Pipeline sehr nützlich.

Um zu überprüfen, ob die Quelle ordnungsgemäß konfiguriert ist, aktivieren Sie den Debugmodus, und rufen Sie eine Datenvorschau ab. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Wenn der Debugmodus aktiviert ist, überschreibt die Zeilenlimit-Konfiguration in den Debugeinstellungen bei der Datenvorschau die Stichprobenentnahme-Einstellung in der Quelle.

## <a name="source-options"></a>Quelloptionen

Die Registerkarte **Quelloptionen** enthält Einstellungen, die spezifisch für den gewählten Connector und das gewählte Format sind. Weitere Informationen und Beispiele finden Sie in der entsprechenden [Connectordokumentation](#supported-sources).

## <a name="projection"></a>Projektion

Wie Schemas in Datasets definiert die Projektion in einer Quelle die Datenspalten, Datentypen und Datenformate aus den Quelldaten. Bei den meisten Datasettypen, z. B. SQL und Parquet, wird die Projektion in einer Quelle so festgelegt, dass sie das in einem Dataset definierte Schema widerspiegelt. Wenn Ihre Quelldateien nicht stark typisiert sind (z. B. flache CSV-Dateien im Gegensatz zu Parquet-Dateien), können Sie in der Quelltransformation die Datentypen für jedes Feld definieren.

![Screenshot der Einstellungen auf der Registerkarte „Projektion“.](media/data-flow/source3.png "Screenshot der Einstellungen auf der Registerkarte „Projektion“.")

Wenn in Ihrer Textdatei kein Schema definiert wurde, wählen Sie **Datentyp erkennen** aus, damit der Dienst Stichproben erstellt und die Datentypen ableitet. Wählen Sie **Standarddatenformat** aus, um die Standarddatenformate automatisch zu ermitteln.

**Schema zurücksetzen** setzt die Projektion auf die Vorgaben im referenzierten Dataset zurück.

Sie können die Spaltendatentypen in einer späteren Transformation für nachgeschaltete Spalten ändern. Verwenden Sie eine Transformation, um die Spaltennamen zu ändern.

### <a name="import-schema"></a>Schema importieren

Mithilfe der Schaltfläche **Schema importieren** auf der Registerkarte **Projektion** können Sie einen aktiven Debugcluster zum Erstellen einer Schemaprojektion verwenden. Diese Schaltfläche ist für jeden Quelltyp verfügbar. Durch das Importieren des Schemas wird die im Dataset definierte Projektion überschrieben. Das Datasetobjekt wird nicht geändert.

Das Importieren des Schemas ist insbesondere bei Datasets wie Avro und Azure Cosmos DB hilfreich, die komplexe Datenstrukturen unterstützen und keine Schemadefinitionen im Dataset erfordern. Für Inline-Datasets ist das Importieren des Schemas die einzige Möglichkeit, ohne Schemaabweichung auf Spaltenmetadaten zu verweisen.

## <a name="optimize-the-source-transformation"></a>Optimieren der Quelltransformation

Die Registerkarte **Optimieren** ermöglicht die Bearbeitung von Partitionsinformationen bei jedem Transformationsschritt. In den meisten Fällen führt **Aktuelle Partitionierung verwenden** eine Optimierung durch, um die ideale Partitionsstruktur für eine Quelle zu erreichen.

Wenn Sie Daten aus einer Azure SQL-Datenbank-Quelle lesen, liest die benutzerdefinierte **Quellpartition** die Daten wahrscheinlich am schnellsten. Der Dienst liest große Abfragen, indem er Verbindungen mit Ihrer Datenbank parallel herstellt. Diese Quellpartitionierung kann für eine Spalte oder über eine Abfrage erfolgen.

![Screenshot der Einstellungen für die Quellpartition](media/data-flow/sourcepart3.png "Screenshot der Einstellungen der Quellpartition.")

Weitere Informationen zur Optimierung in Mapping Data Flow finden Sie auf der [Registerkarte „Optimieren“](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie die Erstellung Ihres Datenflusses mit einer [Transformation für abgeleitete Spalten](data-flow-derived-column.md) und einer [select-Transformation](data-flow-select.md).
