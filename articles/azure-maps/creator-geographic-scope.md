---
title: Geografischer Bereich des Azure Maps Creator-Diensts
description: Erfahren Sie mehr über die geografischen Zuordnungen des Azure Maps Creator-Diensts in Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.custom: mvc, references_regions
ms.openlocfilehash: 1cd2c87f919e5dc68d4dfb7dcf6b38da9d9f67a2
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339058"
---
# <a name="creator-service-geographic-scope"></a>Geografischer Bereich des Creator-Diensts

Azure Maps Creator ist ein Dienst für geografische Bereiche. Creator bietet eine Ressourcenanbieter-API, die bei einer Azure-Region eine Instanz von Creator-Daten erstellt, die auf geografischer Ebene bereitgestellt werden. Die Zuordnung von einer Azure-Region zur Geografie erfolgt im Hintergrund, wie in der folgenden Tabelle beschrieben. Weitere Informationen zu Azure-Regionen und -Geografien finden Sie unter [Azure-Geografien](https://azure.microsoft.com/global-infrastructure/geographies).

## <a name="data-locations"></a>Datenspeicherorte

Für die Notfallwiederherstellung und Hochverfügbarkeit kann Microsoft Kundendaten nur innerhalb desselben geografischen Gebiets in andere Regionen replizieren. Beispielsweise können Daten aus „Europa, Westen“ in „Europa, Norden“ repliziert werden, aber nicht in den USA.  Unabhängig davon, welche geografische Region der Kunde ausgewählt hat, steuert oder beschränkt Microsoft nicht die Standorte, von denen aus die Kunden oder ihre Endbenutzer über die Azure Maps-API auf Kundendaten zugreifen können.  

## <a name="geographic-and-regional-mapping"></a>Geografische und regionale Zuordnung

In der folgenden Tabelle wird die Zuordnung zwischen Geografie und unterstützten Azure-Regionen und dem jeweiligen geografischen API-Endpunkt beschrieben. Wenn beispielsweise ein Creator-Konto in der Region „USA, Westen 2“ bereitgestellt wird, die innerhalb der USA-Geografie liegt, müssen alle API-Aufrufe an den Konvertierungsdienst an `us.atlas.microsoft.com/conversion/convert` erfolgen.


| Geografische Azure-Regionen (geografische Regionen) | Azure-Rechenzentren (Regionen) | Geografischer API-Endpunkt |
|------------------------|----------------------|-------------|
| Europa| Europa, Westen; Europa, Norden | eu.atlas.microsoft.com |
|USA | USA, Westen 2; USA, Osten 2 | us.atlas.microsoft.com |
