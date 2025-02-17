---
title: Kopieren von Daten in den Suchindex
titleSuffix: Azure Data Factory & Azure Synapse
description: Erfahren Sie, wie Daten mit der Kopieraktivität in einer Azure Data Factory-Pipeline mithilfe von Push auf einen Azure Search-Index übertragen oder in einen Azure Search-Index kopiert werden.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 08/30/2021
ms.openlocfilehash: fbeeb112712afa5a47401d10b3f4b2d9f01d1713
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2021
ms.locfileid: "123313733"
---
# <a name="copy-data-to-an-azure-cognitive-search-index-using-azure-data-factory"></a>Kopieren von Daten in einen Azure Cognitive Search-Index mithilfe von Azure Data Factory

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-azure-search-connector.md)
> * [Aktuelle Version](connector-azure-search.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie die Copy-Aktivität in Azure Data Factory verwenden, um Daten in einen Azure Cognitive Search-Index zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einem beliebigen unterstützten Quelldatenspeicher in einen Suchindex kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-azure-search-using-ui"></a>Erstellen eines verknüpften Diensts mit Azure Search über die Benutzeroberfläche

Verwenden Sie die folgenden Schritte, um einen verknüpften Dienst für Azure Search auf der Azure-Portal-Benutzeroberfläche zu erstellen.

1. Navigieren Sie in Ihrem Azure Data Factory- oder Synapse-Arbeitsbereich zu der Registerkarte „Verwalten“, wählen Sie „Verknüpfte Dienste“ aus und klicken Sie dann auf „Neu“:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Erstellen Sie einen neuen verknüpften Dienst mithilfe der Azure Data Factory Benutzeroberfläche.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Erstellen Sie einen neuen verknüpften Dienst mithilfe der Azure Synapse Benutzeroberfläche.":::

2. Suchen Sie nach Search und wählen Sie den Azure Search-Connector aus.

   :::image type="content" source="media/connector-azure-search/azure-search-connector.png" alt-text="Wählen Sie den Azure Search-Connector aus.":::    


1. Konfigurieren Sie die Dienstdetails, testen Sie die Verbindung und erstellen Sie den neuen verknüpften Dienst.

   :::image type="content" source="media/connector-azure-search/configure-azure-search-linked-service.png" alt-text="Konfigurieren Sie einen verknüpften Dienst für Azure Search.":::

## <a name="connector-configuration-details"></a>Details zur Connector-Konfiguration

Die nachfolgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Azure Cognitive Search-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Die folgenden Eigenschaften werden für den mit Azure Cognitive Search verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **AzureSearch** festgelegt werden. | Ja |
| url | URL für den Suchdienst | Ja |
| Schlüssel | Administratorschlüssel für den Suchdienst Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

> [!IMPORTANT]
> Wenn Sie Daten aus einem Clouddatenspeicher in den Suchindex in dem mit Azure Cognitive Search verknüpften Dienst kopieren, müssen Sie auf Azure Integration Runtime mit expliziter Region in connectVia verweisen. Legen Sie die Region fest, in der sich Ihr Suchdienst befindet. Weitere Informationen finden Sie unter [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime).

**Beispiel:**

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure Cognitive Search-Dataset unterstützt werden.

Beim Kopieren von Daten in Azure Cognitive Search werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **AzureSearchIndex** festgelegt werden. | Ja |
| indexName | Name des Suchindex. Data Factory erstellt den Index nicht. Der Index muss in Azure Cognitive Search vorhanden sein. | Ja |

**Beispiel:**

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "typeProperties" : {
            "indexName": "products"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Cognitive Search linked service name>",
            "type": "LinkedServiceReference"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure Cognitive Search-Quelle unterstützt werden.

### <a name="azure-cognitive-search-as-sink"></a>Azure Cognitive Search als Senke

Legen Sie zum Kopieren von Daten in Azure Cognitive Search den Quelltyp in der Copy-Aktivität auf **AzureSearchIndexSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureSearchIndexSink** festgelegt werden. | Ja |
| writeBehavior | Gibt an, ob ein Dokument zusammengeführt oder ersetzt werden soll, wenn es bereits im Index vorhanden ist. Siehe [Eigenschaft „WriteBehavior“](#writebehavior-property).<br/><br/>Zulässige Werte sind **Merge** (Standard) und **Upload**. | Nein |
| writeBatchSize | Lädt Daten in den Suchindex hoch, wenn die Puffergröße writeBatchSize erreicht. Einzelheiten finden Sie unter [Eigenschaft „WriteBatchSize“](#writebatchsize-property).<br/><br/>Zulässige Werte sind ganze Zahlen von 1 bis 1.000 (Standardwert „1.000“). | Nein |
| maxConcurrentConnections |Die Obergrenze gleichzeitiger Verbindungen mit dem Datenspeicher während der Aktivitätsausführung. Geben Sie diesen Wert nur an, wenn Sie die Anzahl der gleichzeitigen Verbindungen begrenzen möchten.| Nein |

### <a name="writebehavior-property"></a>Eigenschaft „WriteBehavior“

AzureSearchSink fügt Daten ein/aktualisiert beim Schreiben von Daten. Dies bedeutet, dass Azure Cognitive Search beim Schreiben eines Dokuments das bestehende Dokument aktualisiert, anstatt eine Konfliktausnahme auszulösen, wenn der Dokumentenschlüssel bereits im Suchindex vorhanden ist.

AzureSearchSink bietet die folgenden zwei Verhalten zum Einfügen/Aktualisieren (mithilfe des Azure Search SDK):

- **Merge** (Zusammenführen): kombiniert alle Spalten im neuen Dokument mit dem bestehenden. Bei Spalten mit Null-Wert im neuen Dokument wird der Wert im bestehenden Dokument beibehalten.
- **Upload** (Hochladen): Das neue Dokument ersetzt das bestehende. Bei Spalten, die nicht im neuen Dokument angegeben werden, wird der Wert auf Null gesetzt, unabhängig davon, ob ein Null-Wert im bestehenden Dokument vorhanden ist oder nicht.

Das Standardverhalten ist **Merge**.

### <a name="writebatchsize-property"></a>Eigenschaft „writeBatchSize“

Der Azure Cognitive Search-Dienst unterstützt das Schreiben von Dokumenten als Batch. Ein Batch kann 1 bis 1.000 Aktionen enthalten. Eine Aktion bearbeitet ein Dokument, um den Vorgang upload/merge auszuführen.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Cognitive Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

## <a name="data-type-support"></a>Datentypunterstützung

In der folgenden Tabelle wird angegeben, ob ein Azure Cognitive Search-Datentyp unterstützt wird oder nicht.

| Azure Cognitive Search-Datentyp | In Azure Cognitive Search-Senke unterstützt |
| ---------------------- | ------------------------------ |
| String | J |
| Int32 | J |
| Int64 | J |
| Double | J |
| Boolean | J |
| DataTimeOffset | J |
| String Array | N |
| GeographyPoint | N |

Andere Datentypen (etwa ComplexType) werden derzeit nicht unterstützt. Eine vollständige Liste der unterstützten Azure Cognitive Search-Datentypen finden Sie unter [Unterstützte Datentypen (Azure Cognitive Search)](/rest/api/searchservice/supported-data-types).

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).