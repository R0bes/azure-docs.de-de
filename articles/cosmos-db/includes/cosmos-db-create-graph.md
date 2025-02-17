---
title: Datei einfügen
description: include file
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 4f11826c5f4a39a6df8c1b9823c54620cb66f3ee
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "113223442"
---
Sie können nun mithilfe des Daten-Explorer-Tools im Azure-Portal eine Diagrammdatenbank erstellen. 

1. Wählen Sie **Daten-Explorer** > **New Graph** (Neues Diagramm) aus.

    Der Bereich **Add Graph** (Graph hinzufügen) wird rechts angezeigt. Möglicherweise müssen Sie nach rechts scrollen, damit Sie ihn sehen.

    ![„Daten-Explorer“ im Azure-Portal, Seite „Graph hinzufügen“](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. Geben Sie auf der Seite **Graph hinzufügen** die Einstellungen für den neuen Graphen ein.

    Einstellung|Vorgeschlagener Wert|BESCHREIBUNG
    ---|---|---
    Datenbank-ID|sample-database|Geben Sie *sample-database* als Namen für die neue Datenbank ein. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/ \ # ?` noch nachgestellte Leerzeichen enthalten.
    Throughput|400 RUs|Ändern Sie den Durchsatz in 400 Anforderungseinheiten pro Sekunde (RU/s). Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten verringern möchten.
    Graph-ID|sample-graph|Geben Sie *sample-graph* als Namen für die neue Sammlung ein. Für Diagrammnamen gelten dieselben Zeichenanforderungen wie für Datenbank-IDs.
    Partitionsschlüssel| /pk |Alle Cosmos DB-Konten benötigen zum horizontalen Skalieren einen Partitionsschlüssel. Informationen zum Auswählen eines geeigneten Partitionsschlüssels finden Sie unter [Verwenden eines partitionierten Graphen in Azure Cosmos DB](../graph-partitioning.md).

3. Wählen Sie **OK** aus, nachdem Sie das Formular ausgefüllt haben.