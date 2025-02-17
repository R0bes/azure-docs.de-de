---
title: 'Tutorial: Migrieren von Ereignisdaten zu Azure Synapse Analytics – Azure Event Hubs'
description: Hier wird erläutert, wie erfasste Event Hubs-Daten mithilfe von Azure Event Grid und Functions zu Azure Synapse Analytics migriert werden.
services: event-hubs
ms.date: 12/07/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: 619bb452b421c4ba21cd03c3fee9692e9c436764
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112416837"
---
# <a name="tutorial-migrate-captured-event-hubs-data-to-azure-synapse-analytics-using-event-grid-and-azure-functions"></a>Tutorial: Migrieren erfasster Event Hubs-Daten zu Azure Synapse Analytics mithilfe von Event Grid und Azure Functions
Mit Azure Event Hubs [Capture](./event-hubs-capture-overview.md) können Sie die Streamingdaten in Event Hubs automatisch in einer Azure Blob Storage- oder Azure Data Lake Store-Instanz erfassen. In diesem Tutorial erfahren Sie, wie Sie erfasste Event Hubs-Daten aus Storage mithilfe einer durch eine [Event Grid](../event-grid/overview.md)-Instanz ausgelösten Azure-Funktion zu Azure Synapse Analytics migrieren.

[!INCLUDE [event-grid-event-hubs-functions-synapse-analytics.md](../event-grid/includes/event-grid-event-hubs-functions-synapse-analytics.md)]

## <a name="next-steps"></a>Nächste Schritte 
Sie können leistungsstarke Datenvisualisierungstools für Ihr Data Warehouse nutzen, um verwertbare Erkenntnisse zu gewinnen.

In [diesem Artikel](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect) erfahren Sie, wie Sie Power BI mit Azure Synapse Analytics verwenden.