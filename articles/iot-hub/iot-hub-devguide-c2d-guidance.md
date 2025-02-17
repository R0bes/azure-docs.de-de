---
title: Azure IoT Hub – C2D-Optionen (Cloud-zu-Gerät) | Microsoft Docs
description: Entwicklerhandbuch – Leitfaden, der angibt, wann direkte Methoden, gewünschte Eigenschaften von Gerätezwillingen oder C2D-Nachrichten für C2D-Kommunikationen verwendet werden sollen.
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: 105374dc214ad77978973a5e7288ac1dd487941b
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346083"
---
# <a name="cloud-to-device-communications-guidance"></a>Leitfaden zur C2D-Kommunikation

IoT Hub bietet drei Optionen für Geräte-Apps, um Funktionen einer Back-End-App verfügbar zu machen:

* [Direkte Methoden](iot-hub-devguide-direct-methods.md) für Kommunikation, die eine sofortige Bestätigung des Ergebnisses erfordert. Direkte Methoden werden häufig für die interaktive Steuerung von Geräten verwendet, beispielsweise zum Einschalten eines Lüfters.

* [Gewünschte Eigenschaften von Gerätezwillingen](iot-hub-devguide-device-twins.md) für Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest.

* [Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D)](iot-hub-devguide-messages-c2d.md) zum Senden unidirektionaler Benachrichtigungen an die Geräte-App.

Informationen zur Verwendung dieser Optionen zum Steuern von IoT Plug & Play-Geräten durch [Azure IoT Plug & Play](../iot-develop/overview-iot-plug-and-play.md) finden Sie im [Leitfaden für IoT Plug & Play-Dienstentwickler](../iot-develop/concepts-developer-guide-service.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die C2D-Kommunikation.

| Kategorien | Direkte Methoden | Gewünschte Eigenschaften von Gerätezwillingen | C2D-Nachrichten |
| ---------- | -------------- | ------------------------- | ------------------------ |
| Szenario | Befehle, die sofortige Bestätigung erfordern, z.B. Einschalten eines Lüfters | Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest. | Senden unidirektionaler Benachrichtigungen an die Geräte-App. |
| Datenfluss | Bidirektional. Die Geräte-App kann sofort auf die Methode reagieren. Das Lösungs-Back-End empfängt ein auf die Anforderung kontextbezogenes Ergebnis. | Unidirektional. Die Geräte-App empfängt eine Benachrichtigung mit der Eigenschaftenänderung. | Unidirektional. Die Geräte-App empfängt die Nachricht.
| Beständigkeit | Mit getrennten Geräten wird kein Kontakt hergestellt. Dem Lösungs-Back-End wird gemeldet, dass das Gerät nicht verbunden ist. | Eigenschaftswerte werden im Gerätezwilling beibehalten. Das Gerät wird diese bei der nächsten erneuten Verbindung lesen. Eigenschaftswerte sind mit der [IoT Hub-Abfragesprache](iot-hub-devguide-query-language.md) erneut abrufbar. | Nachrichten können durch IoT Hub bis zu 48 Stunden aufbewahrt werden. |
| Ziele | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen](iot-hub-devguide-jobs.md). | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen](iot-hub-devguide-jobs.md). | Einzelgerät nach **deviceId**. |
| Size | Die maximale Nutzlast für direkte Methoden beträgt 128KB. | Die Maximalgröße gewünschter Eigenschaften beträgt 32 KB. | Bis zu 64 KB für Nachrichten. |
| Häufigkeit | Hoch. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). | Mittel. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). | Niedrig. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). |
| Protocol | Mit MQTT oder AMQP verfügbar. | Mit MQTT oder AMQP verfügbar. | Mit allen Protokollen verfügbar. Gerät muss bei Verwendung von HTTPS einen Abruf tätigen. |

Erfahren Sie in den folgenden Tutorials, wie Sie direkte Methoden, gewünschte Eigenschaften und C2D-Nachrichten einsetzen:

* [Verwenden von direkten Methoden](quickstart-control-device.md)
* [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten](tutorial-device-twins.md) 
* [Senden von C2D-Nachrichten](iot-hub-node-node-c2d.md)
