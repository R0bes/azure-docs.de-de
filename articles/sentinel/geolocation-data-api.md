---
title: Anreichern von Entitäten mit Geolocationdaten in Azure Sentinel mithilfe der REST-API | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Entitäten in Azure Sentinel über die REST-API mit Geolocationdaten anreichern können.
services: sentinel
documentationcenter: na
author: batamig
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: reference
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2021
ms.author: bagol
ms.openlocfilehash: 483a228dc172acc3ce0cb44e5f9ac0eba1da807c
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114481484"
---
# <a name="enrich-entities-in-azure-sentinel-with-geolocation-data-via-rest-api-public-preview"></a>Anreichern von Entitäten in Azure Sentinel mit Geolocationdaten über die REST-API (Public Preview)

In diesem Artikel erfahren Sie, wie Sie Entitäten in Azure Sentinel mithilfe der REST-API mit Geolocationdaten anreichern.

> [!IMPORTANT]
> Dieses Feature befindet sich derzeit in der VORSCHAU. In den [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) finden Sie weitere rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden oder anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>

## <a name="common-uri-parameters"></a>Allgemeine URI-Parameter

Im Folgenden finden Sie die allgemeinen URI-Parameter für die Geolocation-API:




| Name | Geben Sie in | Erforderlich | type | Beschreibung |
|-|-|-|-|-|
| **{subscriptionId}** | path | ja | GUID | Die Azure-Abonnement-ID |
| **{resourceGroupName}** | path | ja | Zeichenfolge | Der Name der Ressourcengruppe innerhalb des Abonnements |
| **{api-version}** | Abfrage | ja | Zeichenfolge | Die Version des für die Anforderung verwendeten Protokolls. Ab dem 30. April 2021 lautet die Geolocation-API-Version *2019-01-01-preview*.|
| **{ipAddress}** | Abfrage | ja | Zeichenfolge | Die IP-Adresse, für die Geolocationinformationen benötigt werden, im IPv4- oder IPv6-Format   |
|

## <a name="enrich-ip-address-with-geolocation-information"></a>Anreichern der IP-Adresse mit Geolocationinformationen

Dieser Befehl ruft Geolocationdaten für eine bestimmte IP-Adresse ab.

### <a name="request-uri"></a>Anforderungs-URI

| Methode | Anforderungs-URI |
|-|-|
| **GET** | `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SecurityInsights/enrichment/ip/geodata/?ipaddress={ipAddress}&api-version={api-version}` |
|

### <a name="responses"></a>Antworten

|Statuscode  |BESCHREIBUNG  |
|---------|---------|
|**200**     |   Erfolg      |
|**400**     |      Die IP-Adresse ist nicht angegeben oder hat ein ungültiges Format.    |
|**404**     | Geolocationdaten für diese IP-Adresse nicht gefunden         |
|**429**     |      Zu viele Anforderungen, wiederholen Sie den Vorgang im angegebenen Zeitrahmen.    |
|     |         |

### <a name="fields-returned-in-the-response"></a>In der Antwort zurückgegebene Felder

|Feldname  |BESCHREIBUNG  |
|---------|---------|
|**ASN**     |  Die dieser IP-Adresse zugeordnete autonome Systemnummer       |
|**carrier**     |  Der Name des Netzbetreibers für diese IP-Adresse       |
|**city**     |   Der Ort, in der sich diese IP-Adresse befindet      |
|**cityCf**     | Eine numerische Bewertung der Konfidenz auf einer Skala von 0 bis 100, dass der Wert im Feld „city“ korrekt ist        |
|**continent**     | Der Kontinent, auf dem sich diese IP-Adresse befindet        |
|**country**     |Das Land, in dem sich diese IP-Adresse befindet        |
|**countryCf**     |   Eine numerische Bewertung der Konfidenz auf einer Skala von 0 bis 100, dass der Wert im Feld „country“ korrekt ist      |
|**ipAddr**     |   Die Darstellung der IP-Adresse als Zeichenfolge in punktierter Dezimalschreibweise oder durch Doppelpunkte getrennt      |
|**ipRoutingType**     |   Eine Beschreibung des Verbindungstyps für diese IP-Adresse      |
|**latitude**     |     Der Breitengrad dieser IP-Adresse    |
|**longitude**     |  Der Längengrad dieser IP-Adresse       |
|**organization**     |  Der Name der Organisation für diese IP-Adresse       |
|**organizationType**     | Der Typ der Organisation für diese IP-Adresse        |
|**Region**     |    Die geografische Region, in der sich diese IP-Adresse befindet     |
|**state**     |  Das Bundesland/der Bundesstaat, in dem sich diese IP-Adresse befindet       |
|**stateCf**     | Eine numerische Bewertung der Konfidenz auf einer Skala von 0 bis 100, dass der Wert im Feld „state“ korrekt ist        |
|**stateCode**     |   Der abgekürzte Name für das Bundesland/den Bundesstaat, in dem sich diese IP-Adresse befindet      |
|     |         |


## <a name="throttling-limits-for-the-api"></a>Drosselungsgrenzwerte für die API

Diese API ist auf 100 Aufrufe pro Benutzer und Stunde beschränkt.

### <a name="sample-response"></a>Beispiel für eine Antwort

```rest
"body":
{
    "asn": "12345",
    "carrier": "Microsoft",
    "city": "Redmond",
    "cityCf": 90,
    "continent": "north america",
    "country": "united states",
    "countryCf": 99
    "ipAddr": "1.2.3.4",
    "ipRoutingType": "fixed",
    "latitude": "40.2436",
    "longitude": "-100.8891",
    "organization": "Microsoft",
    "organizationType": "tech",
    "region": "western usa",
    "state": "washington",
    "stateCf": null
    "stateCode": "wa"
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Weitere Informationen zu Entitäten finden Sie hier:

    - [Referenz zu Azure Sentinel-Entitätstypen](entities-reference.md)
    - [Klassifizieren und Analysieren von Daten mithilfe von Entitäten in Azure Sentinel](entities-in-azure-sentinel.md)
    - [Zuordnen von Datenfeldern zu Entitäten in Azure Sentinel](map-data-fields-to-entities.md)

- Erkunden anderer Verwendungsmöglichkeiten der [Azure Sentinel-API](/rest/api/securityinsights/)
