---
title: Was ist Azure Digital Twins?
titleSuffix: Azure Digital Twins
description: Übersicht über die Verwendungsmöglichkeiten von Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 8/23/2021
ms.topic: overview
ms.service: digital-twins
ms.openlocfilehash: 6dfb4faac6fd5bb11dfc1fbb928d9ba377ec1fe3
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2021
ms.locfileid: "123224385"
---
# <a name="what-is-azure-digital-twins"></a>Was ist Azure Digital Twins?

**Azure Digital Twins** ist ein PaaS-Angebot (Platform-as-a-Service), mit dem Sie basierend auf digitalen Modellen ganzer Umgebungen Zwillingsgraphen erstellen können. Bei einer solchen Umgebung kann es sich um Gebäude, Fabriken, Höfe, Energieversorgungsnetze, Eisenbahnstrecken, Stadien und mehr handeln. Sogar ganze Städte lassen sich modellieren. Diese digitalen Modelle liefern wichtige Einblicke, um Produkte zu verbessern, Vorgänge zu optimieren, Kosten zu senken und die Benutzerfreundlichkeit zu revolutionieren.

Nutzen Sie Ihre umfassenden Fachkenntnisse, um mit Azure Digital Twins individuell angepasste, verbundene Lösungen zu erstellen, die Folgendes ermöglichen:
* Abbilden beliebiger Umgebungen in naturgetreuen Digital Twins-Modellen, die durch Skalierbarkeit und Sicherheit überzeugen
* Verbinden von Objekten wie IoT-Geräten und vorhandenen Geschäftssystemen
* Einsatz eines zuverlässigen Ereignissystems für eine dynamische Geschäftslogik und Datenverarbeitung
* Integration in Azure-Daten, -Analysefunktionen und -KI-Diensten, um Abläufe der Vergangenheit nachzuverfolgen und anhand der gewonnenen Informationen die Zukunft vorherzusagen

## <a name="azure-digital-twins-capabilities"></a>Funktionen von Azure Digital Twins

Nachfolgend sind die von Azure Digital Twins bereitgestellten Funktionen zusammengefasst.

### <a name="open-modeling-language"></a>Offene Modellierungssprache

In Azure Digital Twins werden die digitalen Entitäten zur Darstellung von Menschen, Orten und Objekten in Ihrer physischen Umgebung mithilfe von zwei benutzerdefinierten Zwillingstypen definiert, die als [Modelle](concepts-models.md) bezeichnet werden. 

Sie können sich diese Modelldefinitionen als eine Art spezialisiertes Vokabular zum Beschreiben Ihres Unternehmens vorstellen. Für eine Gebäudeverwaltungslösung können Sie z. B. Modelle wie „Gebäude“, „Etage“ und „Fahrstuhl“ definieren. Anschließend erstellen Sie basierend auf diesen Modellen **digitale Zwillinge** zur Darstellung Ihrer Umgebung.

[!INCLUDE [digital-twins-versus-device-twins](../../includes/digital-twins-versus-device-twins.md)]

Modelle werden mit der JSON-ähnlichen Sprache [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) definiert. Sie enthalten Angaben wie Statuseigenschaften, Telemetrieereignisse, Befehle, Komponenten und Beziehungen zum Beschreiben der Zwillinge.
* Darüber hinaus werden mit Modellen semantische **Beziehungen** zwischen ihren Entitäten definiert. Mit diesen Beziehungen lassen sich Zwillinge in einem Graphen verbinden, der Aufschluss über die Interaktionen der Zwillinge gibt. In einer Beschreibung Ihrer Umgebung können Modelle als Nomen und die Beziehungen als Verben betrachtet werden.
* Außerdem lassen sich Zwillinge mithilfe der Modellvererbung spezialisieren. Ein Modell kann Informationen von einem anderen Modell erben.

DTDL wird auch in anderen Azure IoT-Diensten für Datenmodelle verwendet. Dazu zählen u. a. [IoT Plug & Play](../iot-develop/overview-iot-plug-and-play.md) und [Time Series Insights (TSI)](../time-series-insights/overview-what-is-tsi.md). Diese Art der Gemeinsamkeit stellt sicher, dass Ihre Azure Digital Twins-Lösung mit anderen Bereichen des Azure-Ökosystems verbunden werden kann und kompatibel ist.

### <a name="live-execution-environment"></a>Liveausführungsumgebung

Bei den digitalen Modellen in Azure Digital Twins handelt es sich um aktuelle Livedarstellungen der realen Welt. Mithilfe der Beziehungen in Ihren benutzerdefinierten DTDL-Modellen verbinden Sie die Zwillinge in einem **Livediagramm**, mit dem Ihre Umgebung dargestellt wird.

Sie können Ihren Azure Digital Twins-Graphen in [Azure Digital Twins-Explorer](concepts-azure-digital-twins-explorer.md)visualisieren, der die folgende Schnittstelle für die Interaktion mit Ihrem Graphen bereitstellt:

:::image type="content" source="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png" alt-text="Screenshot: Azure Digital Twins-Explorer mit einem Graphen von Knoten, die digitale Zwillinge darstellen" lightbox="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png":::

Azure Digital Twins bietet ein umfangreiches **Ereignissystem**, um sicherzustellen, dass dieses Diagramm mit Datenverarbeitung und Geschäftslogik stets auf dem aktuellen Stand ist. Für eine flexible, individuell angepasste Datenverarbeitung können Sie zudem externe Computeressourcen wie [Azure Functions](../azure-functions/functions-overview.md) verbinden.

Darüber hinaus können Sie die leistungsfähige **Abfrage-API** von Azure Digital Twins verwenden, um Erkenntnisse aus der Liveausführungsumgebung zu extrahieren. Diese API ermöglicht Ihnen die Ausführung von Abfragen mit umfangreichen Suchbedingungen, wie z. B. Eigenschaftswerten, Beziehungen, Beziehungseigenschaften, Modellinformationen usw. Außerdem lassen sich Abfragen kombinieren, um eine große Bandbreite an Erkenntnissen zu Ihrer Umgebung zu gewinnen und benutzerdefinierte Fragen zu beantworten, die für Ihre Zwecke wichtig sind.

### <a name="input-from-iot-and-business-systems"></a>Eingaben aus IoT- und Geschäftssystemen

Um sicherzustellen, dass die Liveausführungsumgebung von Azure Digital Twins stets auf dem aktuellen Stand ist und der realen Umgebung entspricht, können Sie Ihre Lösung mithilfe von [IoT Hub](../iot-hub/about-iot-hub.md) mit IoT- und IoT Edge-Geräten verbinden. Diese vom Hub verwalteten Geräte werden als Teil Ihres Zwillingsdiagramms dargestellt und stellen die Daten für Ihr Modell bereit.

Sie können zu diesem Zweck einen neuen IoT-Hub mit Azure Digital Twins erstellen oder einen vorhandenen IoT-Hub mit den Geräten verbinden, die der Hub bereits verwaltet.

Darüber hinaus können Sie Azure Digital Twins über REST-APIs oder Connectors zu anderen Diensten wie [Logic Apps](../logic-apps/logic-apps-overview.md) auch mit anderen Datenquellen verbinden.

### <a name="output-to-adx-tsi-storage-and-analytics"></a>Ausgabe an ADX, TSI, Speicher- oder Analysedienste

Die Daten in Ihrem Azure Digital Twins-Modell können für weitere Analysen oder zum Speichern an nachgelagerte Azure-Dienste weitergeleitet werden. Diese Funktionalität wird durch **Ereignisrouten** bereitgestellt, die für den Datenfluss [Event Hub](../event-hubs/event-hubs-about.md), [Event Grid](../event-grid/overview.md) oder [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) nutzen.

Mit Ereignisrouten lassen sich u. a. folgende Aufgaben ausführen:
* Senden von Daten digitaler Zwillinge an ADX zum Abfragen mit dem [Azure Digital Twins-Plug-In für Azure Data Explorer (ADX)](concepts-data-explorer-plugin.md)
* [Verbinden von Azure Digital Twins mit Time Series Insights (TSI)](how-to-integrate-time-series-insights.md), um den Zeitreihenverlauf der einzelnen Zwillinge nachzuverfolgen
* Ausrichten eines Zeitreihenmodells in Time Series Insights auf eine Quelle in Azure Digital Twins
* Speichern von Azure Digital Twins-Daten in [Azure Data Lake](../storage/blobs/data-lake-storage-introduction.md)
* Analysieren von Azure Digital Twins-Daten mit [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) oder anderen Microsoft-Datenanalysetools
* Integrieren umfangreicherer Workflows mit Logic Apps

Diese Option stellt eine andere Möglichkeit dar, mit der sich Azure Digital Twins mit einer umfangreicheren Lösung verbinden kann, um Ihre individuellen Anforderungen bei der kontinuierlichen Arbeit mit diesen Erkenntnissen zu erfüllen.

## <a name="azure-digital-twins-in-a-solution-context"></a>Azure Digital Twins im Lösungskontext

Azure Digital Twins wird häufig mit anderen Azure-Diensten in einer umfangreicheren IoT-Lösung eingesetzt. 

Eine umfassende Lösung mit Azure Digital Twins könnte folgende Elemente beinhalten:
* Die Azure Digital Twins-Dienstinstanz. Dieser Dienst speichert Ihre Zwillingsmodelle sowie Ihr Zwillingsdiagramm mit dem jeweiligen Status und orchestriert die Ereignisverarbeitung.
* Eine oder mehrere Client-Apps, die die Grundlage der Azure Digital Twins-Instanz bilden. Diese Apps werden zum Konfigurieren von Modellen, Erstellen der Topologie und Extrahieren von Erkenntnissen aus dem Zwillingsdiagramm verwendet.
* Eine oder mehrere externe Computeressourcen zur Verarbeitung von Ereignissen, die von Azure Digital Twins generiert werden, oder verbundene Datenquellen (z. B. Geräte). Eine gängige Möglichkeit zur Bereitstellung von Computeressourcen ist die Verwendung von [Azure Functions](../azure-functions/functions-overview.md).
* Ein IoT-Hub für Geräteverwaltung und zur Bereitstellung von IoT-Datenstromfunktionen.
* Nachgelagerte Dienste für Aufgaben wie Workflowintegration (z. B. [Logic Apps](../logic-apps/logic-apps-overview.md), Cold Storage, Azure Data Explorer, Zeitreihenintegration oder Analysen).

Die folgende Abbildung zeigt, wie sich Azure Digital Twins im Kontext einer größeren Azure IoT-Lösung einfügt.

:::image type="content" source="media/overview/solution-context.png" alt-text="Abbildung mit Eingabequellen, Ausgabediensten und bidirektionaler Kommunikation mit Client-Apps und externen Computeressourcen." border="false" lightbox="media/overview/solution-context.png":::

## <a name="service-limits"></a>Diensteinschränkungen

Informationen zu den **Diensteinschränkungen** für Azure Digital Twins finden Sie unter [Einschränkungen des Azure Digital Twins-Diensts](reference-service-limits.md). Diese Ressource kann nützlich sein, wenn Sie mit dem Dienst zusammenarbeiten, um die Funktions-und Raten Einschränkungen zu verstehen, und welche Grenzwerte bei Bedarf angepasst werden können.

## <a name="terminology"></a>Begriff

Eine Liste **allgemeiner IoT-Begriffe** und ihrer Verwendung in den Azure IoT-Diensten, einschließlich Azure Digital Twins, finden Sie im [Glossar mit IoT-Begriffen](../iot-fundamentals/iot-glossary.md?toc=/azure/digital-twins/toc.json&bc=/azure/digital-twins/breadcrumb/toc.json). Diese Ressource kann hilfreich sein, wenn Sie erste Schritte mit Azure Digital Twins ausführen und eine IoT-Lösung entwickeln.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit der Arbeit mit Azure Digital Twins in der folgenden Schnellstartanleitung vertraut: [Erste Schritte mit dem Azure Digital Twins-Explorer](quickstart-azure-digital-twins-explorer.md).

* Oder, Informieren Sie sich über Konzepte für Azure Digital Twins mit [DTDL-Modellen](concepts-models.md).