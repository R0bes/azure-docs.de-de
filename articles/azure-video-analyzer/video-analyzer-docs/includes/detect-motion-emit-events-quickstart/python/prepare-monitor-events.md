---
author: fvneerden
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 05/03/2021
ms.author: faneerde
ms.openlocfilehash: e98039dfe4c8c44be2fc375956339956a495da4f
ms.sourcegitcommit: cc099517b76bf4b5421944bd1bfdaa54153458a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113559396"
---
Sie verwenden das Azure Video Analyzer-Modul, um Bewegungen im eingehenden Livevideostream zu erkennen und Ereignisse an IoT Hub zu senden. Führen Sie zum Anzeigen dieser Ereignisse die folgenden Schritte aus:

1. Öffnen Sie in Visual Studio Code den Explorer-Bereich, und suchen Sie links unten nach „Azure IoT Hub“.
1. Erweitern Sie den Knoten **Geräte**.
1. Klicken Sie mit der rechten Maustaste auf **avasample-iot-edge-device**, und wählen Sie **Start Monitoring Built-in Event Endpoint** (Überwachung des integrierten Ereignisendpunkts starten) aus.

![Überwachung des integrierten Ereignisendpunkts starten](../../../media/vscode-common-screenshots/start-monitoring.png)

[!INCLUDE [provide-builtin-endpoint](../../common-includes/provide-builtin-endpoint.md)]
