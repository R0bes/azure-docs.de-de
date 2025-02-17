---
title: Problembehandlung für die Leistung der Kopieraktivität
titleSuffix: Azure Data Factory & Azure Synapse
description: Es wird beschrieben, wie Sie in Azure Data Factory und in Azure Synapse Analytics die Problembehandlung in Bezug auf die Leistung der Kopieraktivität durchführen.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 08/24/2021
ms.openlocfilehash: cea4999752d416f36f435ba88403fd9dc735f689
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2021
ms.locfileid: "123255814"
---
# <a name="troubleshoot-copy-activity-performance"></a>Problembehandlung für die Leistung der Kopieraktivität

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie Probleme mit der Leistung der Kopieraktivität in Azure Data Factory behandeln. 

Nachdem Sie eine Kopieraktivität ausgeführt haben, können Sie das Ausführungsergebnis und die Leistungsstatistik in der Ansicht zur [Überwachung der Kopieraktivität](copy-activity-monitoring.md) erfassen. Im Folgenden finden Sie ein Beispiel.

![Details zum Überwachen der Ausführung der Kopieraktivität](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="performance-tuning-tips"></a>Tipps zur Leistungsoptimierung

In einigen Szenarien werden bei der Ausführung einer Kopieraktivität in oben **„Tipps zur Leistungsoptimierung“** angezeigt. Dies ist im obigen Beispiel dargestellt. Die Tipps geben Ihnen Aufschluss über den Engpass, der von dem Dienst für den jeweiligen Kopiervorgang identifiziert wurde. Sie erhalten außerdem Vorschläge, wie der Kopierdurchsatz erhöht werden kann. Versuchen Sie, die empfohlene Änderung vorzunehmen, und führen Sie den Kopiervorgang dann erneut durch.

Als Hilfe ist hier angegeben, für welche Fälle in den Tipps zur Leistungsoptimierung derzeit Vorschläge gemacht werden:

| Category              | Tipps zur Leistungsoptimierung                                      |
| --------------------- | ------------------------------------------------------------ |
| Bezogen auf den Datenspeicher   | Laden von Daten in **Azure Synapse Analytics**: Es wird vorgeschlagen, PolyBase oder eine COPY-Anweisung zu verwenden, falls dies nicht bereits genutzt wird. |
| &nbsp;                | Kopieren von Daten für **Azure SQL-Datenbank**: Wenn für die DTU eine hohe Auslastung besteht, wird ein Upgrade auf einen höheren Tarif vorgeschlagen. |
| &nbsp;                | Kopieren von Daten für **Azure Cosmos DB**: Wenn für die RU eine hohe Auslastung besteht, wird ein Upgrade auf eine höhere RU vorgeschlagen. |
|                       | Kopieren von Daten aus **SAP Table**: Es wird vorgeschlagen, beim Kopieren von großen Datenmengen die Partitionsoption des SAP-Connectors zum Aktivieren der parallelen Last und zur Erhöhung der maximalen Partitionsanzahl zu nutzen. |
| &nbsp;                | Erfassen von Daten aus **Amazon Redshift**: Es wird vorgeschlagen, UNLOAD zu verwenden, falls dies nicht bereits genutzt wird. |
| Drosselung des Datenspeichers | Wenn beim Kopieren vom Datenspeicher einige Lese-/Schreibvorgänge gedrosselt werden, wird vorgeschlagen, die zulässige Anforderungsrate für den Datenspeicher zu überprüfen und zu erhöhen oder die gleichzeitige Arbeitsauslastung zu reduzieren. |
| Integration Runtime  | Wenn Sie eine **selbstgehostete Integration Runtime (IR)** nutzen und die Kopieraktivität lange in der Warteschlange verbleibt, bis die IR über die verfügbare Ressource für die Ausführung verfügt, wird das horizontale bzw. vertikale Hochskalieren Ihrer IR vorgeschlagen. |
| &nbsp;                | Bei Verwendung einer **Azure Integration Runtime**, die sich nicht in einer optimalen Region befindet, und einer daraus resultierenden langsamen Durchführung von Lese-/Schreibvorgängen wird vorgeschlagen, die Verwendung einer IR in einer anderen Region zu konfigurieren. |
| Fehlertoleranz       | Wenn Sie die Fehlertoleranz konfigurieren und das Überspringen von inkompatiblen Zeilen zu einer langsamen Leistung führt, lautet der Vorschlag, die Kompatibilität von Quellen- und Senkendaten sicherzustellen. |
| gestaffeltem Kopieren           | Wenn das gestaffelte Kopieren konfiguriert, aber für Ihr Quell-/Senkenpaar nicht hilfreich ist, wird die Entfernung vorgeschlagen. |
| Fortfahren                | Wenn die Kopieraktivität ab dem letzten Fehlerpunkt fortgesetzt wird und Sie die DIU-Einstellung nach der ursprünglichen Ausführung ändern, sollte Ihnen bewusst sein, dass die neue DIU-Einstellung nicht wirksam wird. |

## <a name="understand-copy-activity-execution-details"></a>Grundlegendes zur Ausführung der Kopieraktivität

In den Details zur Ausführung und Dauer unten in der Ansicht zur Überwachung der Kopieraktivität sind die wichtigen Phasen beschrieben, die Ihre Kopieraktivität durchläuft (siehe Beispiel am Anfang dieses Artikels). Dies ist besonders hilfreich für die Problembehandlung in Bezug auf die Kopierleistung. Der Engpass Ihrer Kopierausführung ist die Ausführung mit der längsten Dauer. Sehen Sie sich die folgende Tabelle mit den Definitionen zu den einzelnen Phasen an, und informieren Sie sich darüber, wie Sie anhand dieser Informationen die [Problembehandlung für die Kopieraktivität bei Azure IR](#troubleshoot-copy-activity-on-azure-ir) und die [Problembehandlung für die Kopieraktivität bei selbstgehosteter Integration Runtime](#troubleshoot-copy-activity-on-self-hosted-ir) durchführen können.

| Phase           | BESCHREIBUNG                                                  |
| --------------- | ------------------------------------------------------------ |
| Warteschlange           | Die bis zum tatsächlichen Start der Kopieraktivität in der Integration Runtime verstrichene Zeit. |
| Kopiervorbereitungsskript | Die Zeit, die zwischen dem Starten der Kopieraktivität in der IR und dem Abschließen der Kopieraktivität für die Ausführung des Kopiervorbereitungsskripts im Senkendatenspeicher verstrichen ist. Wird angewendet, wenn Sie das Kopiervorbereitungsskript für Datenbanksenken konfigurieren, z. B. beim Schreiben von Daten in Azure SQL-Datenbank zur Bereinigung vor dem Kopieren neuer Daten. |
| Übertragen        | Die verstrichene Zeit zwischen dem Ende des vorherigen Schritts und dem Übertragen aller Daten aus der Quelle in die Senke durch die IR. <br/>Beachten Sie, dass die Teilschritte unter „Übertragen“ parallel ausgeführt werden und dass einige Vorgänge derzeit nicht angezeigt werden, z. B. Analysieren und Generieren des Dateiformats.<br><br/>- **Zeit bis zum ersten Byte:** Die Zeit vom Ende des vorherigen Schritts bis zu dem Zeitpunkt, zu dem das erste Byte aus dem Quelldatenspeicher von der IR empfangen wird. Wird auf nicht dateibasierte Quellen angewendet.<br>- **Quellenauflistung:** Die Dauer, die für das Auflisten von Quelldateien oder Datenpartitionen benötigt wird. Letzteres gilt, wenn Sie Partitionsoptionen für Datenbankquellen konfigurieren, z. B. beim Kopieren von Daten aus Datenbanken wie Oracle, SAP HANA, Teradata, Netezza usw.<br/>-**Lesen von der Quelle:** Die Dauer für das Abrufen von Daten aus dem Quelldatenspeicher.<br/>- **Schreiben in die Senke:** Die Dauer für das Schreiben von Daten in den Senkendatenspeicher. Beachten Sie, dass einige Connectors diese Metrik aktuell nicht enthalten, z. B. Azure Cognitive Search, Azure Data Explorer, Azure Table Storage, Oracle, SQL Server, Common Data Service, Dynamics 365, Dynamics CRM, Salesforce und Salesforce Service Cloud. |

## <a name="troubleshoot-copy-activity-on-azure-ir"></a>Problembehandlung für die Kopieraktivität bei Azure IR

Führen Sie die [Schritte zur Optimierung der Leistung](copy-activity-performance.md#performance-tuning-steps) aus, um den Leistungstest für Ihr Szenario zu planen und durchzuführen. 

Gehen Sie wie folgt vor, falls die Leistung der Kopieraktivität nicht Ihre Erwartungen erfüllt: Wenn Sie die Problembehandlung für eine einzelne ausgeführte Kopieraktivität unter Azure Integration Runtime durchführen möchten und in der Ansicht für die Überwachung des Kopiervorgangs [Tipps zur Leistungsoptimierung](#performance-tuning-tips) angezeigt werden, sollten Sie den Vorschlag befolgen und es dann erneut versuchen. Lesen Sie andernfalls den Abschnitt [Grundlegendes zur Ausführung der Kopieraktivität](#understand-copy-activity-execution-details), überprüfen Sie, welche Phase am **längsten** dauert, und befolgen Sie die unten angegebene Anleitung, um die Kopierleistung zu steigern:

- **Lange Dauer für „Kopiervorbereitungsskript“:** Dies bedeutet, dass es lange dauert, bis das in der Senkendatenbank ausgeführte Kopiervorbereitungsskript abgeschlossen ist. Optimieren Sie die Logik des angegebenen Kopiervorbereitungsskripts, um die Leistung zu verbessern. Wenden Sie sich an Ihr Datenbankteam, falls Sie weitere Hilfe zur Verbesserung des Skripts benötigen.

- **Lange Dauer für „Übertragen: Zeit bis zum ersten Byte“** : Dies bedeutet, dass es lange dauert, bis von Ihrer Quellabfrage Daten zurückgegeben werden. Überprüfen und optimieren Sie die Abfrage oder den Server. Wenden Sie sich an Ihr Datenspeicherteam, falls Sie weitere Hilfe benötigen.

- **Lange Dauer für „Übertragen: Quellenauflistung“** : Dies bedeutet, dass das Auflisten von Quelldateien oder der Datenpartitionen der Quelldatenbank nur langsam erfolgt.
  - Beachten Sie beim Kopieren von Daten von einer dateibasierten Quelle Folgendes, wenn Sie einen **Platzhalterfilter** für einen Ordnerpfad oder Dateinamen (`wildcardFolderPath` oder `wildcardFileName`) oder **Filter für das letzte Änderungsdatum der Datei** (`modifiedDatetimeStart` oder `modifiedDatetimeEnd`) verwenden: Diese Filterung führt dazu, dass von der Kopieraktivität alle Dateien des angegebenen Ordners auf der Clientseite aufgelistet werden und anschließend der Filter angewendet wird. Eine solche Auflistung von Dateien kann zu einem Engpass werden. Dies gilt besonders, wenn nur eine geringe Menge von Dateien die Filterregel erfüllt.

    - Überprüfen Sie, ob Sie [Dateien basierend auf einem Dateipfad oder -namen mit datetime-Partition kopieren](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md) können. Diese Vorgehensweise ist nicht mit einer höheren Belastung bei der Auflistung aufseiten der Quelle verbunden.

    - Überprüfen Sie, ob Sie stattdessen den nativen Filter des Datenspeichers verwenden können, also „**prefix**“ für Amazon S3/Azure Blob Storage/Azure File und „**listAfter/listBefore**“ für ADLS Gen1. Diese Filter sind serverseitige Datenspeicherfilter mit einer deutlich besseren Leistung.

    - Erwägen Sie, große Einzeldatasets in mehrere kleinere Datasets zu unterteilen, und ermöglichen Sie die gleichzeitige Ausführung dieser Kopieraufträge, die jeweils für einen Teil der Daten bestimmt sind. Hierfür können Sie „Lookup/GetMetadata + ForEach + Copy“ verwenden. Die Lösungsvorlagen [Kopieren von Dateien aus mehreren Containern](solution-template-copy-files-multiple-containers.md) und [Migrieren von Daten aus Amazon S3 zu ADLS Gen2](solution-template-migration-s3-azure.md) dienen hier als allgemeine Beispiele.

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

  - Verwenden Sie Azure IR in derselben oder in einer Region, die nicht weit von der Region Ihres Quelldatenspeichers entfernt ist.

- **Lange Dauer für „Übertragen: Lesen von der Quelle“** : 

  - Halten Sie sich an die bewährte Methode für das connectorspezifische Laden von Daten, falls sie für Sie zutrifft. Wenn Sie beispielsweise Daten von [Amazon Redshift](connector-amazon-redshift.md) kopieren, sollten Sie konfigurieren, dass Redshift UNLOAD verwendet wird.

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

  - Überprüfen Sie Ihre Kopierquelle und das Senkenmuster: 

    - Wenn für Ihr Kopiermuster mehr als vier Datenintegrationseinheiten (DIUs) unterstützt werden, helfen Ihnen die Informationen in [diesem Abschnitt](copy-activity-performance-features.md#data-integration-units) weiter. Sie können generell versuchen, die Anzahl von DIUs zu erhöhen, um eine bessere Leistung zu erzielen. 

    - Erwägen Sie andernfalls, große Einzeldatasets in mehrere kleinere Datasets zu unterteilen, und ermöglichen Sie die gleichzeitige Ausführung dieser Kopieraufträge, die jeweils für einen Teil der Daten gelten. Hierfür können Sie „Lookup/GetMetadata + ForEach + Copy“ verwenden. Die Lösungsvorlagen [Kopieren von Dateien aus mehreren Containern](solution-template-copy-files-multiple-containers.md), [Migrieren von Daten aus Amazon S3 zu ADLS Gen2](solution-template-migration-s3-azure.md) und [Massenkopieren mit Steuertabelle](solution-template-bulk-copy-with-control-table.md) dienen hier als allgemeine Beispiele.

  - Verwenden Sie Azure IR in derselben oder in einer Region, die nicht weit von der Region Ihres Quelldatenspeichers entfernt ist.

- **Lange Dauer für „Übertragen: Schreiben in die Senke“** :

  - Halten Sie sich an die bewährte Methode für das connectorspezifische Laden von Daten, falls sie für Sie zutrifft. Verwenden Sie beispielsweise beim Kopieren von Daten in [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) PolyBase oder die COPY-Anweisung. 

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

  - Überprüfen Sie Ihre Kopierquelle und das Senkenmuster: 

    - Wenn für Ihr Kopiermuster mehr als vier Datenintegrationseinheiten (DIUs) unterstützt werden, helfen Ihnen die Informationen in [diesem Abschnitt](copy-activity-performance-features.md#data-integration-units) weiter. Sie können generell versuchen, die Anzahl von DIUs zu erhöhen, um eine bessere Leistung zu erzielen. 

    - Gehen Sie andernfalls so vor, dass Sie die [parallelen Kopien](copy-activity-performance-features.md) allmählich optimieren. Beachten Sie, dass sich zu viele parallele Kopien ggf. negativ auf die Leistung auswirken können.

  - Verwenden Sie Azure IR in derselben oder in einer Region, die nicht weit von der Region Ihres Senkendatenspeichers entfernt ist.

## <a name="troubleshoot-copy-activity-on-self-hosted-ir"></a>Problembehandlung für die Kopieraktivität bei selbstgehosteter Integration Runtime

Führen Sie die [Schritte zur Optimierung der Leistung](copy-activity-performance.md#performance-tuning-steps) aus, um den Leistungstest für Ihr Szenario zu planen und durchzuführen. 

Gehen Sie wie folgt vor, falls die Kopierleistung nicht Ihre Erwartungen erfüllt: Wenn Sie die Problembehandlung für eine einzelne ausgeführte Kopieraktivität unter Azure Integration Runtime durchführen möchten und in der Ansicht für die Überwachung des Kopiervorgangs [Tipps zur Leistungsoptimierung](#performance-tuning-tips) angezeigt werden, sollten Sie den Vorschlag befolgen und es dann erneut versuchen. Lesen Sie andernfalls den Abschnitt [Grundlegendes zur Ausführung der Kopieraktivität](#understand-copy-activity-execution-details), überprüfen Sie, welche Phase am **längsten** dauert, und befolgen Sie die unten angegebene Anleitung, um die Kopierleistung zu steigern:

- **Lange Dauer für „Warteschlange“:** Dies bedeutet, dass die Kopieraktivität lange in der Warteschlange verbleibt, bis Ihre selbstgehostete Integration Runtime über die Ressource für die Ausführung verfügt. Überprüfen Sie die Kapazität und Nutzung für die IR, und führen Sie je nach Workload das [horizontale oder vertikale Hochskalieren](create-self-hosted-integration-runtime.md#high-availability-and-scalability) durch.

- **Lange Dauer für „Übertragen: Zeit bis zum ersten Byte“** : Dies bedeutet, dass es lange dauert, bis von Ihrer Quellabfrage Daten zurückgegeben werden. Überprüfen und optimieren Sie die Abfrage oder den Server. Wenden Sie sich an Ihr Datenspeicherteam, falls Sie weitere Hilfe benötigen.

- **Lange Dauer für „Übertragen: Quellenauflistung“** : Dies bedeutet, dass das Auflisten von Quelldateien oder der Datenpartitionen der Quelldatenbank nur langsam erfolgt.

  - Überprüfen Sie, ob der Computer mit der selbstgehosteten IR bei der Verbindungsherstellung mit dem Quelldatenspeicher eine geringe Latenz aufweist. Wenn sich Ihre Quelle in Azure befindet, können Sie [dieses Tool](http://www.azurespeed.com/Azure/Latency) verwenden, um die Latenz zwischen dem Computer mit der selbstgehosteten IR und der Azure-Region zu überprüfen. Je geringer sie ist, desto besser ist es.

  - Beachten Sie beim Kopieren von Daten von einer dateibasierten Quelle Folgendes, wenn Sie einen **Platzhalterfilter** für einen Ordnerpfad oder Dateinamen (`wildcardFolderPath` oder `wildcardFileName`) oder **Filter für das letzte Änderungsdatum der Datei** (`modifiedDatetimeStart` oder `modifiedDatetimeEnd`) verwenden: Diese Filterung führt dazu, dass von der Kopieraktivität alle Dateien des angegebenen Ordners auf der Clientseite aufgelistet werden und anschließend der Filter angewendet wird. Eine solche Auflistung von Dateien kann zu einem Engpass werden. Dies gilt besonders, wenn nur eine geringe Menge von Dateien die Filterregel erfüllt.

    - Überprüfen Sie, ob Sie [Dateien basierend auf einem Dateipfad oder -namen mit datetime-Partition kopieren](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md) können. Diese Vorgehensweise ist nicht mit einer höheren Belastung bei der Auflistung aufseiten der Quelle verbunden.

    - Überprüfen Sie, ob Sie stattdessen den nativen Filter des Datenspeichers verwenden können, also „**prefix**“ für Amazon S3/Azure Blob Storage/Azure File und „**listAfter/listBefore**“ für ADLS Gen1. Diese Filter sind serverseitige Datenspeicherfilter mit einer deutlich besseren Leistung.

    - Erwägen Sie, große Einzeldatasets in mehrere kleinere Datasets zu unterteilen, und ermöglichen Sie die gleichzeitige Ausführung dieser Kopieraufträge, die jeweils für einen Teil der Daten bestimmt sind. Hierfür können Sie „Lookup/GetMetadata + ForEach + Copy“ verwenden. Die Lösungsvorlagen [Kopieren von Dateien aus mehreren Containern](solution-template-copy-files-multiple-containers.md) und [Migrieren von Daten aus Amazon S3 zu ADLS Gen2](solution-template-migration-s3-azure.md) dienen hier als allgemeine Beispiele.

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

- **Lange Dauer für „Übertragen: Lesen von der Quelle“** : 

  - Überprüfen Sie, ob der Computer mit der selbstgehosteten IR bei der Verbindungsherstellung mit dem Quelldatenspeicher eine geringe Latenz aufweist. Wenn sich Ihre Quelle in Azure befindet, können Sie [dieses Tool](http://www.azurespeed.com/Azure/Latency) verwenden, um die Latenz zwischen dem Computer mit der selbstgehosteten IR und den Azure-Regionen zu überprüfen. Je geringer sie ist, desto besser ist es.

  - Überprüfen Sie, ob der Computer mit der selbstgehosteten IR über genügend Eingangsbandbreite verfügt, um die Daten effizient lesen und übertragen zu können. Wenn sich Ihr Quelldatenspeicher in Azure befindet, können Sie [dieses Tool](https://www.azurespeed.com/Azure/Download) verwenden, um die Downloadgeschwindigkeit zu überprüfen.

  - Überprüfen Sie, ob der Nutzungstrend für die CPU und den Arbeitsspeicher der selbstgehosteten IR, im Azure-Portal auf die Übersichtsseite Ihrer Data Factory oder den Synapse-Arbeitsbereich angezeigt wird. Erwägen Sie, die [IR horizontal oder vertikal hochzuskalieren](create-self-hosted-integration-runtime.md#high-availability-and-scalability), falls die CPU-Auslastung hoch ist oder nur wenig Arbeitsspeicher verfügbar ist.

  - Halten Sie sich an die bewährte Methode für das connectorspezifische Laden von Daten, falls sie für Sie zutrifft. Beispiel:

    - Aktivieren Sie die Datenpartitionsoptionen zum parallelen Kopieren von Daten, wenn Sie Daten aus [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) kopieren.

    - Konfigurieren Sie beim Kopieren von Daten aus [HDFS](connector-hdfs.md) die Verwendung von DistCp.

    - Konfigurieren Sie beim Kopieren von Daten aus [Amazon Redshift](connector-amazon-redshift.md) die Verwendung von Redshift UNLOAD.

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

  - Überprüfen Sie Ihre Kopierquelle und das Senkenmuster: 

    - Erwägen Sie beim Kopieren von Daten aus Datenspeichern mit aktivierter Partitionsoption, die [parallelen Kopien](copy-activity-performance-features.md) allmählich zu optimieren. Beachten Sie, dass sich zu viele parallele Kopien ggf. negativ auf die Leistung auswirken können.

    - Erwägen Sie andernfalls, große Einzeldatasets in mehrere kleinere Datasets zu unterteilen, und ermöglichen Sie die gleichzeitige Ausführung dieser Kopieraufträge, die jeweils für einen Teil der Daten gelten. Hierfür können Sie „Lookup/GetMetadata + ForEach + Copy“ verwenden. Die Lösungsvorlagen [Kopieren von Dateien aus mehreren Containern](solution-template-copy-files-multiple-containers.md), [Migrieren von Daten aus Amazon S3 zu ADLS Gen2](solution-template-migration-s3-azure.md) und [Massenkopieren mit Steuertabelle](solution-template-bulk-copy-with-control-table.md) dienen hier als allgemeine Beispiele.

- **Lange Dauer für „Übertragen: Schreiben in die Senke“** :

  - Halten Sie sich an die bewährte Methode für das connectorspezifische Laden von Daten, falls sie für Sie zutrifft. Verwenden Sie beispielsweise beim Kopieren von Daten in [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) PolyBase oder die COPY-Anweisung. 

  - Überprüfen Sie, ob der Computer mit der selbstgehosteten IR bei der Verbindungsherstellung mit dem Senkendatenspeicher eine geringe Latenz aufweist. Wenn sich Ihre Senke in Azure befindet, können Sie [dieses Tool](http://www.azurespeed.com/Azure/Latency) verwenden, um die Latenz zwischen dem Computer mit der selbstgehosteten IR und der Azure-Region zu überprüfen. Je geringer sie ist, desto besser ist es.

  - Überprüfen Sie, ob der Computer mit der selbstgehosteten IR über genügend Ausgangsbandbreite verfügt, um die Daten effizient übertragen und schreiben zu können. Wenn sich Ihr Senkendatenspeicher in Azure befindet, können Sie [dieses Tool](https://www.azurespeed.com/Azure/UploadLargeFile) verwenden, um die Uploadgeschwindigkeit zu überprüfen.

  - Überprüfen Sie, ob der Nutzungstrend für die CPU und den Arbeitsspeicher der selbstgehosteten IR, im Azure-Portal auf die Übersichtsseite Ihrer Data Factory oder den Synapse-Arbeitsbereich angezeigt wird. Erwägen Sie, die [IR horizontal oder vertikal hochzuskalieren](create-self-hosted-integration-runtime.md#high-availability-and-scalability), falls die CPU-Auslastung hoch ist oder nur wenig Arbeitsspeicher verfügbar ist.

  - Überprüfen Sie, ob von dem Dienst Drosselungsfehler für die Quelle gemeldet werden oder für Ihren Datenspeicher ein hoher Auslastungszustand besteht. Wenn dies der Fall ist, sollten Sie entweder Ihre Workloads im Datenspeicher verringern, oder Ihren Datenspeicheradministrator darum bitten, das Drosselungslimit oder die verfügbaren Ressourcen zu erhöhen.

  - Erwägen Sie, die [parallelen Kopien](copy-activity-performance-features.md) allmählich zu optimieren. Beachten Sie, dass sich zu viele parallele Kopien ggf. negativ auf die Leistung auswirken können.


## <a name="connector-and-ir-performance"></a>Leistung von Connector und Integration Runtime 

In diesem Abschnitt werden einige Hinweise zum Beheben Leistungsproblemen für bestimmte Connectortypen oder Integration Runtimes (IRs) erläutert.

### <a name="activity-execution-time-varies-using-azure-ir-vs-azure-vnet-ir"></a>Ausführungszeit für Aktivitäten variiert zwischen Azure IR und VNet IR

Die Ausführungszeit für Aktivitäten variiert je nachdem, auf welcher Integration Runtime das Dataset basiert.

- **Symptome:** Durch einfaches Umschalten der Dropdownliste „Verknüpfter Dienst“ im Dataset werden dieselben Pipelineaktivitäten ausgeführt, die Laufzeiten unterscheiden sich jedoch stark. Wenn das Dataset auf der Managed Virtual Network Integration Runtime basiert, dauert es im Durchschnitt länger als die Ausführung auf Basis der Standard Integration Runtime.  

- **Ursache:** Wenn Sie die Details der Pipelineausführungen überprüfen, können Sie feststellen, dass die langsame Pipeline in der IR für das verwaltete VNET (virtuelles Netzwerk) ausgeführt wird, während die normale Pipeline in der Azure IR ausgeführt wird. Die Warteschlangenzeit ist im IR für das verwaltete VNET länger als in der Azur IR. Der Grund dafür ist, dass wir nicht einen Computerknoten pro Dienstinstanz reservieren und somit vor dem Starten jeder Kopieraktivität eine Aufwärmphase erfolgt. Dies geschieht hauptsächlich beim Beitritt zu einem VNET statt in der Azure IR. 

    
### <a name="low-performance-when-loading-data-into-azure-sql-database"></a>Geringe Leistung beim Laden von Daten in Azure SQL-Datenbank

- **Symptome:** Daten werden nur langsam nach Azure SQL-Datenbank kopiert.

- **Ursache:** Die Grundursache dieses Problems ist meistens ein Engpass bei der Azure SQL-Datenbank. Folgende Ursachen kommen in Betracht:

    - Die Azure SQL-Datenbank wird auf einer zu niedrigen Ebene ausgeführt.

    - Der DTU-Verbrauch der Azure SQL-Datenbank beträgt nahezu 100 %. Sie können [die Leistung überwachen](../azure-sql/database/monitor-tune-overview.md) und ein Upgrade der Ebene der Azure SQL-Datenbank in Erwägung ziehen.

    - Die Indizes sind nicht ordnungsgemäß festgelegt. Entfernen Sie vor dem Laden der Daten alle Indizes, und erstellen Sie sie nach Abschluss des Ladevorgangs neu.

    - WriteBatchSize ist nicht groß genug für die Zeilengröße des Schemas. Versuchen Sie, den Wert der Eigenschaft zu erhöhen, um das Problem zu beheben.

    - Anstelle einer Masseneinfügung wird eine gespeicherte Prozedur verwendet, bei der eine geringere Leistung zu erwarten ist. 


### <a name="timeout-or-slow-performance-when-parsing-large-excel-file"></a>Timeout oder geringe Leistung beim Analysieren einer großen Excel-Datei

- **Symptome:**

    - Wenn Sie ein Excel-Dataset erstellen und über „Aus Verbindung/Speicher“ ein Schema importieren oder wenn Sie Arbeitsblätter auflisten, aktualisieren oder eine Datenvorschau anzeigen, kann ein Timeoutfehler auftreten, falls die Excel-Datei sehr groß ist.

    - Wenn Sie die Kopieraktivität zum Kopieren von Daten aus einer großen Excel-Datei (>= 100 MB) in einen anderen Datenspeicher verwenden, wird der Vorgang unter Umständen nur langsam ausgeführt, oder es tritt ein Fehler vom Typ „Nicht genügend Arbeitsspeicher“ auf.

- **Ursache**: 

    - Für Vorgänge wie das Importieren von Schemas, das Anzeigen einer Datenvorschau und das Auflisten von Arbeitsblättern eines Excel-Datasets beträgt der Timeoutzeitraum 100 Sekunden und ist statisch. Bei sehr umfangreichen Excel-Dateien können diese Vorgänge unter Umständen nicht innerhalb des Timeoutzeitraums abgeschlossen werden.

    - Durch die Kopieraktivität wird die gesamte Excel-Datei in den Arbeitsspeicher eingelesen. Anschließend wird das angegebene Arbeitsblatt ermittelt, und die Daten werden aus den Zellen ausgelesen. Dieses Verhalten basiert auf dem zugrunde liegenden SDK, das von dem Dienst verwendet wird.

- **Lösung:** 

    - Zum Importieren von Schemas können Sie eine kleinere Beispieldatei generieren, bei der es sich um eine Teilmenge der Originaldatei handelt, und anstelle von „Schema aus Verbindung/Speicher importieren“ die Option „Schema aus Beispieldatei importieren“ auswählen.

    - Zum Auflisten von Arbeitsblättern können Sie in der Dropdownliste „Arbeitsblatt“ auf „Bearbeiten“ klicken und stattdessen den Arbeitsblattnamen bzw. den Index eingeben.

    - Zum Kopieren von großen Excel-Dateien (> 100 MB) in einen anderen Speicher können Sie den Datenfluss für Excel-Quellen verwenden, der Lesevorgänge streamt und eine bessere Leistung bietet.
    
## <a name="other-references"></a>Andere Referenzen

Hier finden Sie Referenzen zur Leistungsüberwachung und -optimierung für einige der unterstützten Datenspeicher:

* Azure Blob Storage: [Skalierbarkeits- und Leistungsziele für Blob Storage](../storage/blobs/scalability-targets.md) und [Checkliste zu Leistung und Skalierbarkeit für Blob Storage](../storage/blobs/storage-performance-checklist.md).
* Azure Table Storage: [Skalierbarkeits- und Leistungsziele für Table Storage](../storage/tables/scalability-targets.md) und [Checkliste zu Leistung und Skalierbarkeit für Table Storage](../storage/tables/storage-performance-checklist.md).
* Azure SQL-Datenbank: Sie können [die Leistung überwachen](../azure-sql/database/monitor-tune-overview.md) und den prozentualen Anteil der Datenbanktransaktionseinheit (Database Transaction Unit, DTU) überprüfen.
* Azure Synapse Analytics: Die Funktion wird in DWUs (Data Warehouse-Einheiten) gemessen. Weitere Informationen finden Sie unter [Verwalten von Computeressourcen im Azure Synapse Analytics-Data Warehouse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Leistungsebenen in Azure Cosmos DB](../cosmos-db/performance-levels.md)
* SQL Server: [Überwachen und Optimieren der Leistung](/sql/relational-databases/performance/monitor-and-tune-for-performance).
* Lokaler Dateiserver: [Leistungsoptimierung für Dateiserver](/previous-versions//dn567661(v=vs.85))

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Handbuch zur Leistung und Skalierbarkeit der Kopieraktivität](copy-activity-performance.md)
- [Features für die Leistungsoptimierung bei Kopieraktivitäten](copy-activity-performance-features.md)
- [Migrieren von Daten aus Ihrem Data Lake oder Data Warehouse zu Azure](data-migration-guidance-overview.md)
- [Migrieren von Daten aus Amazon S3 zu Azure Storage](data-migration-guidance-s3-azure-storage.md)
