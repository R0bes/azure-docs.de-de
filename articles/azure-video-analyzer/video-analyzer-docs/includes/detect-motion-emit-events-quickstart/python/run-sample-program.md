---
author: fvneerden
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 05/05/2021
ms.author: faneerde
ms.openlocfilehash: 9544a69da1c8ba0e3c2fd248a0bbbc7cbc13206f
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114593517"
---
Führen Sie diese Schritte aus, um den Beispielcode auszuführen:

1. Öffnen Sie in Visual Studio Code die Registerkarte **Erweiterungen** (oder drücken Sie STRG + UMSCHALT + X), und suchen Sie nach Azure IoT Hub.
1. Klicken Sie mit der rechten Maustaste, um das Kontextmenü zu öffnen, und wählen Sie **Erweiterungseinstellungen** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="../../../media/vscode-common-screenshots/extension-settings.png" alt-text="Erweiterungseinstellungen":::

1. Suchen Sie nach dem Kontrollkästchen „Show Verbose Message“ (Ausführliche Meldung anzeigen), und aktivieren Sie es.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="../../../media/vscode-common-screenshots/verbose-message.png" alt-text="Show Verbose Message (Ausführliche Meldung anzeigen)":::

1. Navigieren Sie in Visual Studio Code zu _src/cloud-to-device-console-app/operations.json_.
1. Vergewissern Sie sich auf dem Knoten `pipelineTopologySet`, dass der folgende Wert angezeigt wird:

   ```
   "pipelineTopologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/motion-detection/topology.json"
   ```

1. Vergewissern Sie sich unter den Knoten `livePipelineSet` und `livePipelineDelete`, dass der Wert für **topologyName** dem Wert der Eigenschaft **name** in der Pipelinetopologie entspricht:

   `"topologyName" : "MotionDetection"`

1. * Navigieren Sie zu dem Fenster `TERMINAL` in VS Code
   * Verwenden Sie den Befehl „cd“ um zu dem Verzeichnis /video-analyzer-iot-edge-python-main/src/cloud-to-device-console-app zu navigieren
   * Rufen Sie „python main.py“ auf, dann werden Ihnen die Meldungen im Fenster `TERMINAL` angezeigt
1. Von der Datei _operations.json_ werden zuerst `pipelineTopologyList` und `livePipelineList` aufgerufen. Wenn Sie nach dem Durcharbeiten vorheriger Schnellstartanleitungen eine Ressourcenbereinigung durchgeführt haben, werden bei diesem Prozess leere Listen zurückgegeben.

   ```
   -----------------------  Request: pipelineTopologyList  --------------------------------------------------

   {
   "@apiVersion": "1.0"
   }

   ---------------  Response: pipelineTopologyList - Status: 200  ---------------

   {
     "value": []
   }

   --------------------------------------------------------------------------

   ```

   Im **Terminalfenster** werden die nächsten Aufrufe direkter Methoden angezeigt:

   - Ein Aufruf von `pipelineTopologySet`, bei dem das vorherige „pipelineTopologyUrl“-Element verwendet wird
   - Ein Aufruf von `livePipelineSet`, der den folgenden Textkörper verwendet:

   ```json
   {
     "@apiVersion": "1.0",
     "name": "Sample-Pipeline-1",
     "properties": {
       "topologyName": "MotionDetection",
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

   - Ein Aufruf von `livePipelineActivate`, mit dem die Livepipeline und der Videodatenfluss gestartet werden
   - Ein zweiter Aufruf von `livePipelineList` mit der Anzeige, dass sich die Livepipeline im ausgeführten Zustand befindet

1. Die Ausgabe im **Terminalfenster** wird mit der folgenden Meldung angehalten: `Press Enter to continue`. Warten Sie noch mit dem Drücken der EINGABETASTE. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wechseln Sie in Visual Studio Code zum **Ausgabefenster**. Es werden Nachrichten angezeigt, die vom Azure Video Analyzer-Modul an den IoT-Hub gesendet werden. Im folgenden Abschnitt dieser Schnellstartanmeldung sind diese Meldungen beschrieben.
1. Die Livepipeline wird weiterhin ausgeführt, und es werden Ergebnisse ausgegeben. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Wechseln Sie zum Beenden der Livepipeline zurück zum **Terminalfenster**, und drücken Sie die EINGABETASTE.

   Mit den nächsten Aufrufen wird die Ressourcenbereinigung durchgeführt:

   - Mit dem Aufruf von `livePipelineDeactivate` wird die Pipeline deaktiviert.
   - Mit dem Aufruf von `livePipelineDelete` wird die Pipeline gelöscht.
   - Mit dem Aufruf von `pipelineTopologyDelete` wird die Topologie gelöscht.
   - Ein abschließender Aufruf von `pipelineTopologyList` zeigt, dass die Liste leer ist.
