---
title: Ausführen von Azure Machine Learning-Pipelines
titleSuffix: Azure Data Factory & Azure Synapse
description: Hier erfahren Sie, wie Sie Ihre Azure Machine Learning-Pipelines in Ihren Azure Data Factory-Pipelines ausführen.
ms.service: data-factory
ms.subservice: tutorials
ms.custom: synapse
ms.topic: conceptual
ms.author: abnarain
author: nabhishek
ms.date: 07/16/2020
ms.openlocfilehash: 19456f33e13a9a472b5c42afc0a3cb3c5041efd2
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122640737"
---
# <a name="execute-azure-machine-learning-pipelines-in-azure-data-factory"></a>Ausführen von Azure Machine Learning-Pipelines in Azure Data Factory-Pipelines

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Führen Sie Ihre Azure Machine Learning-Pipelines als Schritt in Ihren Azure Data Factory-Pipelines aus. Die Machine Learning-Pipelineausführungsaktivität ermöglicht Batchvorhersageszenarien wie etwa die Ermittlung möglicher Kreditausfälle, die Ermittlung der Stimmung oder die Analyse von Kundenverhaltensmustern.

Das folgende Video enthält eine sechsminütige Einführung und Demonstration dieses Features.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/How-to-execute-Azure-Machine-Learning-service-pipelines-in-Azure-Data-Factory/player]

## <a name="syntax"></a>Syntax

```json
{
    "name": "Machine Learning Execute Pipeline",
    "type": "AzureMLExecutePipeline",
    "linkedServiceName": {
        "referenceName": "AzureMLService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mlPipelineId": "machine learning pipeline ID",
        "experimentName": "experimentName",
        "mlPipelineParameters": {
            "mlParameterName": "mlParameterValue"
        }
    }
}

```

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
name | Name der Aktivität in der Pipeline | String | Ja
type | Die Art der Aktivität lautet „AzureMLExecutePipeline“. | String | Ja
linkedServiceName | Mit Azure Machine Learning verknüpfter Dienst | Verweis auf den verknüpften Dienst | Ja
mlPipelineId | ID der veröffentlichten Azure Machine Learning-Pipeline | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Ja
experimentName | Name des Ausführungsverlaufexperiments der Machine Learning-Pipelineausführung | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein
mlPipelineParameters | Schlüssel-Wert-Paare, die an den veröffentlichten Azure Machine Learning-Pipeline-Endpunkt übergeben werden sollen. Die Schlüssel müssen den Namen von Pipelineparametern entsprechen, die in der veröffentlichten Machine Learning-Pipeline definiert sind. | Objekt mit Schlüssel-Wert-Paaren (oder Ausdruck mit resultType-Objekt) | Nein
mlParentRunId | ID der übergeordneten Azure Machine Learning-Pipelineausführung | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein
dataPathAssignments | Wörterbuch, das zum Ändern von Datenpfaden in Azure Machine Learning verwendet wird. Ermöglicht das Wechseln von Datenpfaden. | Objekt mit Schlüssel-Wert-Paaren | Nein
continueOnStepFailure | Gibt an, ob die Ausführung weiterer Schritte in der Machine Learning-Pipelineausführung fortgesetzt werden soll, wenn ein Schritt nicht erfolgreich war. | boolean | Nein

> [!NOTE]
> Zum Auffüllen der Dropdownelemente im Namen und der ID der Machine Learning-Pipeline muss der Benutzer berechtigt sein, ML-Pipelines aufzulisten. ADF UX ruft AzureMLService-APIs mithilfe der Anmeldeinformationen des angemeldeten Benutzers direkt auf.  

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden:

* [Ausführen der Datenflussaktivität](control-flow-execute-data-flow-activity.md)
* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)
