---
title: 'Analysieren von Livevideos mit Ihrem eigenen Modell: HTTP'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie Livevideos mit Ihrem eigenen Modell (HTTP) per Video Analyzer analysieren.
ms.service: azure-video-analyzer
ms.topic: quickstart
ms.date: 06/01/2021
zone_pivot_groups: video-analyzer-programming-languages
ms.openlocfilehash: a587191a0e5fd80174b3d288d1a9a1d8fd1d2e82
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114605227"
---
# <a name="quickstart-analyze-live-video-with-your-own-model---http"></a>Schnellstart: Analysieren von Livevideos mit einem eigenen Modell - HTTP

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit Azure Video Analyzer einen Livevideofeed einer (simulierten) IP-Kamera analysieren. Sie erfahren, wie Sie ein Modell für maschinelles Sehen anwenden, um Objekte zu erkennen. Eine Teilmenge der Einzelbilder des Livevideofeeds wird an einen Rückschlussdienst gesendet. Die Ergebnisse werden an IoT Edge Hub gesendet.

Für die Schnellstartanleitung werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. Er baut auf der Schnellstartanleitung [Erkennen von Bewegung und Ausgeben von Ereignissen](detect-motion-emit-events-quickstart.md) auf.

## <a name="prerequisites"></a>Voraussetzungen

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/analyze-live-video-use-your-model-http/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/analyze-live-video-use-your-model-http/python/prerequisites.md)]
::: zone-end

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos

Beim Einrichten der Azure-Ressourcen wird ein kurzes Video von Verkehr auf einer Schnellstraße auf die Linux-VM in Azure kopiert, die Sie als IoT Edge-Gerät verwenden. In dieser Schnellstartanleitung wird die Videodatei verwendet, um einen Livestream zu simulieren.

Öffnen Sie eine Anwendung wie etwa den [VLC Media Player](https://www.videolan.org/vlc/). Drücken Sie STRG+N, und fügen Sie dann einen Link zum [Beispielvideo zur Schnellstraßenkreuzung](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) ein, um die Wiedergabe zu starten. Sie sehen das Videomaterial vieler Fahrzeuge, die sich im Verkehr einer Schnellstraße bewegen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4LTY4]

## <a name="create-and-deploy-the-livepipeline"></a>Erstellen und Bereitstellen der Livepipeline

### <a name="examine-and-edit-the-sample-files"></a>Untersuchen und Bearbeiten der Beispieldateien

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/analyze-live-video-use-your-model-http/csharp/sample-files.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/analyze-live-video-use-your-model-http/python/sample-files.md)]
::: zone-end

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests

1. Klicken Sie mit der rechten Maustaste auf die Datei _src/edge/deployment.yolov3.template.json_, und wählen Sie dann **IoT Edge-Bereitstellungsmanifest generieren** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/analyze-live-video-use-your-model-http/generate-deployment-manifest.png" alt-text="Screenshot: IoT Edge-Bereitstellungsmanifest generieren":::

1. Die Manifestdatei _deployment.yolov3.amd64.json_ wird im Ordner _src/edge/config_ erstellt.
1. Klicken Sie mit der rechten Maustaste auf _src/edge/config/deployment.yolov3.amd64.json_, und wählen Sie **Bereitstellung für einzelnes Gerät erstellen** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/analyze-live-video-use-your-model-http/deployment-single-device.png" alt-text= "Screenshot of Create Deployment for Single Device":::

1. Wählen Sie **ava-sample-iot-edge-device** aus, wenn Sie zum Auswählen eines IoT Hub-Geräts aufgefordert werden.
1. Aktualisieren Sie nach ungefähr 30 Sekunden unten links im Fenster den Dienst „Azure IoT Hub“. Das Edge-Gerät zeigt nun die folgenden bereitgestellten Module an:

   - Das Video Analyzer-Modul mit dem Namen **avaedge**
   - Das Modul **rtspsim**, das einen RTSP-Server simuliert und als Quelle eines Livevideofeeds fungiert
   - Das Modul **avaextension**. Hierbei handelt es sich um das YoloV3-Objekterkennungsmodell, das maschinelles Sehen auf die Bilder anwendet und mehrere Klassen von Objekttypen zurückgibt.

     > [!div class="mx-imgBorder"]
     > :::image type="content" source="./media/vscode-common-screenshots/avaextension.png" alt-text= "Screenshot of YoloV3 object detection model":::

## <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

1. ::: zone pivot="programming-language-csharp"
    [!INCLUDE [header](includes/common-includes/csharp-run-program.md)]
    ::: zone-end

    ::: zone pivot="programming-language-python"
    [!INCLUDE [header](includes/common-includes/python-run-program.md)]
    ::: zone-end
1. Der Code von operations.json beginnt mit Aufrufen der direkten Methoden `pipelineTopologyList` und `livePipelineList`. Wenn Sie nach dem Durcharbeiten vorheriger Schnellstartanleitungen eine Ressourcenbereinigung durchgeführt haben, werden bei diesem Prozess leere Listen zurückgegeben, und anschließend wird die Ausführung angehalten. Drücken Sie die EINGABETASTE, um den Vorgang fortzusetzen.

   ```
   --------------------------------------------------------------------------
   Executing operation pipelineTopologyList
   -----------------------  Request: pipelineTopologyList  --------------------------------------------------
   {
   "@apiVersion": "1.0"
   }
   ---------------  Response: pipelineTopologyList - Status: 200  ---------------
   {
   "value": []
   }
   --------------------------------------------------------------------------
   Executing operation WaitForInput

   Press Enter to continue
   ```

1. Im **Terminalfenster** werden die nächsten Aufrufe direkter Methoden angezeigt:

   - Ein Aufruf von `pipelineTopologySet`, bei dem das vorherige „pipelineTopologyUrl“-Element verwendet wird
   - Ein Aufruf von `livePipelineSet`, der den folgenden Textkörper verwendet:

     ```
     {
       "@apiVersion": "1.0",
       "name": "Sample-Pipeline-1",
       "properties": {
         "topologyName": "InferencingWithHttpExtension",
         "description": "Sample pipeline description",
         "parameters": [
           {
             "name": "rtspUrl",
             "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
           },
           {
             "name": "rtspUserName",
             "value": "testuser"
           },
           {
             "name": "rtspPassword",
             "value": "testpassword"
           }
         ]
       }
     }
     ```

   - Ein Aufruf von „livePipelineActivate“, mit dem die Livepipeline und der Videodatenfluss gestartet werden
   - Ein zweiter Aufruf von `livePipelineList`, um anzuzeigen, dass sich die Livepipeline im ausgeführten Zustand befindet

1. Die Ausgabe im **Terminalfenster** wird an einer Aufforderung **Drücken Sie die EINGABETASTE, um den Vorgang fortzusetzen** angehalten. Warten Sie noch mit dem Drücken der EINGABETASTE. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wechseln Sie in Visual Studio Code zum **Ausgabefenster**. Es werden Nachrichten angezeigt, die vom Video Analyzer-Modul an den IoT-Hub gesendet werden. Im folgenden Abschnitt dieser Schnellstartanmeldung sind diese Meldungen beschrieben.
1. Die Pipeline wird weiterhin ausgeführt, und es werden Ergebnisse ausgegeben. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Wechseln Sie zum Beenden der Pipeline zurück zum **Terminalfenster**, und drücken Sie die EINGABETASTE.

   Mit den nächsten Aufrufen wird die Ressourcenbereinigung durchgeführt:

   - Mit dem Aufruf von `livePipelineDeactivate` wird die Livepipeline deaktiviert.
   - Mit dem Aufruf von `livePipelineDelete` wird die Livepipeline gelöscht.
   - Mit dem Aufruf von `pipelineTopologyDelete` wird die Topologie gelöscht.
   - Ein abschließender Aufruf von `pipelineTopologyList` zeigt, dass die Liste leer ist.

## <a name="interpret-results"></a>Interpretieren von Ergebnissen

Wenn Sie die Livepipeline ausführen, durchlaufen die Ergebnisse vom Knoten des HTTP-Erweiterungsprozessors den Knoten der IoT Hub-Nachrichtensenke und werden an den IoT-Hub gesendet. Die im **Ausgabefenster** angezeigten Nachrichten enthalten jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“. Weitere Informationen finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](../../iot-hub/iot-hub-devguide-messages-construct.md).

In den folgenden Nachrichten werden vom Video Analyzer-Modul die Anwendungseigenschaften und der Inhalt des Texts definiert.

**MediaSessionEstablished-Ereignis**

Wenn eine Pipeline instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live555“ ausgeführt wird. Wenn die Verbindungsherstellung erfolgreich ist, wird das folgende Ereignis ausgegeben. Der Ereignistyp lautet **Microsoft.VideoAnalyzer.Diagnostics.MediaSessionEstablished**.

```
[IoTHubMonitor] [9:42:18 AM] Message received from [avasampleiot-edge-device/avaedge]:
{
  "body": {
    "sdp&quot;: &quot;SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/videoanalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sources/rtspSource",
    "eventType": "Microsoft.VideoAnalyzers.Diagnostics.MediaSessionEstablished",
    "eventTime": "2021-04-09T09:42:18.1280000Z"
  }
}
```

Beachten Sie in dieser Meldung diese Details:

- Bei der Meldung handelt es sich um ein Diagnoseereignis. „MediaSessionEstablished“ gibt an, dass vom RTSP-Quellknoten („subject“) eine Verbindung mit dem RTSP-Simulator hergestellt wurde und der Empfang eines (simulierten) Livefeeds begonnen hat.
- Mit „subject“ in „applicationProperties“ wird angegeben, dass die Nachricht über den Knoten der RTSP-Quelle in der Pipeline generiert wurde.
- „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.
- „eventTime“ gibt den Zeitpunkt des Ereignisses an.
- Der Abschnitt „body“ enthält Daten zum Diagnoseereignis. In diesem Fall umfassen die Daten die Details zum [Session Description Protocol (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol).

### <a name="inference-event"></a>Ereignis „Inference“

Der Knoten des HTTP-Erweiterungsprozessors empfängt Rückschlussergebnisse vom yolov3-Modul. Anschließend gibt er die Ergebnisse über den Knoten der IoT Hub-Nachrichtensenke als Rückschlussereignisse aus.

In diesen Ereignissen ist der Typ auf „entity“ festgelegt, um anzuzeigen, dass es sich um eine Entität wie etwa ein Auto oder einen LKW handelt. Der Wert von „eventTime“ ist die UTC-Uhrzeit, zu der das Objekt erkannt wurde.

Im folgenden Beispiel wurden zwei Autos auf dem gleichen Video-Einzelbild erkannt, mit unterschiedlichen Konfidenzwerten.

```
[IoTHubMonitor] [1:48:04 PM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "timestamp": 145589011404622,
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": {
          "value": "car",
          "confidence": 0.97052866
        },
        "box": {
          "l": 0.40896654,
          "t": 0.60390747,
          "w": 0.045092657,
          "h": 0.029998193
        }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": {
          "value": "car",
          "confidence": 0.9547283
        },
        "box": {
          "l": 0.20050547,
          "t": 0.6094412,
          "w": 0.043425046,
          "h": 0.037724357
        }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": {
          "value": "car",
          "confidence": 0.94567955
        },
        "box": {
          "l": 0.55363107,
          "t": 0.5320657,
          "w": 0.037418623,
          "h": 0.027014252
        }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": {
          "value": "car",
          "confidence": 0.8916893
        },
        "box": {
          "l": 0.6642384,
          "t": 0.581689,
          "w": 0.034349587,
          "h": 0.027812533
        }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": {
          "value": "car",
          "confidence": 0.8547814
        },
        "box": {
          "l": 0.584758,
          "t": 0.60079926,
          "w": 0.07082855,
          "h": 0.034121
        }
      }
    }
  ]
}
```

Beachten Sie in den Meldungen die folgenden Details:

- Der Abschnitt „body“ enthält Daten zum Analyseereignis. In diesem Fall ist das Ereignis ein Rückschlussereignis. Daher enthält „body“ Daten für „inferences“.
- Der Rückschlussabschnitt gibt an, dass der Typ „entity“ lautet. Dieser Abschnitt enthält zusätzliche Daten zur Entität.
- Mit dem Zeitstempelwert (timestamp) wird die Uhrzeit angegeben, zu der das Ereignis empfangen wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [prerequisites](./includes/common-includes/clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Zusätzliches für fortgeschrittene Benutzer:

- Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.
- Verwenden Sie ein AMD64- oder x64-Linux-Gerät anstelle eines virtuellen Azure-Linux-Computers. Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Sie können die Anleitungen unter [Installieren der Azure IoT Edge-Runtime unter Linux](../../iot-edge/how-to-install-iot-edge.md?view=iotedge-2018-06&preserve-view=true) befolgen. Registrieren Sie das Gerät anschließend beim Azure IoT Hub, indem Sie die Anweisungen in [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](../../iot-edge/quickstart-linux.md?view=iotedge-2018-06&preserve-view=true) ausführen.
