---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 04/07/2021
ms.author: juliako
ms.openlocfilehash: fc2978bb78f25cae8bf42af255bbc3186082fc72
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114444521"
---
Als Teil der Voraussetzungen haben Sie den Beispielcode in einen Ordner heruntergeladen. Führen Sie die unten angegebenen Schritte aus, um die Beispieldateien zu überprüfen und zu bearbeiten.

1. Navigieren Sie in Visual Studio Code zu **src/edge**. Ihre ENV-Datei und einige Bereitstellungsvorlagendateien werden angezeigt.

    Die Bereitstellungsvorlage verweist auf das Bereitstellungsmanifest für das Edge-Gerät. Es enthält einige Platzhalterwerte. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie zum Ordner **src/cloud-to-device-console-app**. Darin finden Sie Ihre Datei *appsettings.json* und einige andere Dateien:

    * **c2d-console-app.csproj**: Die Projektdatei für Visual Studio Code.
    * **operations.json**: Eine Liste mit den Vorgängen, die vom Programm ausgeführt werden sollen.
    * **main.py**: Der Beispielprogrammcode. Mit diesem Code wird Folgendes durchgeführt:
        
        * Laden der App-Einstellungen
        * Aufrufen direkter Methoden, die vom Azure Video Analyzer-Modul verfügbar gemacht werden. 
        * Anhalten der Ausführung, sodass Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen
1. Bearbeiten Sie die Datei **operations.json**:

    * Ändern Sie den Link zur Pipeline: <br/>`"pipelineTopologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/evr-motion-file-sink/topology.json" `
    * Bearbeiten Sie unter „livePipelineSet“ den Namen der Pipelinetopologie, sodass er mit dem Wert im vorherigen Link übereinstimmt: <br/>`"topologyName" : "EVRToFilesOnMotionDetection" `
    * Bearbeiten Sie unter „PipelineTopologyDelete“ den Namen: <br/>`"name": "EVRToFilesOnMotionDetection" `

