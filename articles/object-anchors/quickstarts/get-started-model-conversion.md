---
title: 'Schnellstart: Erstellen eines Object Anchors-Modells, das in einer App verwendet werden soll'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Object Anchors-Modell anhand eines 3D-Modells erstellen.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 06/10/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: e3b67bee93a0e9a7b823d3a91d6d95e1bccbbe71
ms.sourcegitcommit: abf31d2627316575e076e5f3445ce3259de32dac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114202830"
---
# <a name="quickstart-create-an-object-anchors-model-from-a-3d-model"></a>Schnellstart: Erstellen eines Object Anchors-Modells aus einem 3D-Modell

Azure Object Anchors ist ein verwalteter Clouddienst, der 3D-Modelle in KI-Modelle konvertiert, die objektbezogene Mixed Reality-Erlebnisse für HoloLens ermöglichen. In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des [Azure Object Anchors Conversion SDK für .NET](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre) ein Object Anchors-Modell aus einem 3D-Modell erstellen.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Object Anchors-Kontos
> * Konvertieren eines 3D-Modells zum Erstellen eines Object Anchors-Modells mithilfe des [Azure Object Anchors Conversion SDK für .NET](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre) ([NuGet](https://www.nuget.org/packages/Azure.MixedReality.ObjectAnchors.Conversion/))

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

* Einen Windows-Computer, auf dem <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> installiert ist.
* <a href="https://git-scm.com" target="_blank">Git für Windows</a>.
* Das <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1">.NET Core 3.1 SDK</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Create Account](../../../includes/object-anchors-get-started-create-account.md)]

## <a name="get-the-sample-project"></a>Herunterladen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

## <a name="convert-a-3d-model"></a>Konvertieren eines 3D-Modells

Nun können Sie Ihr 3D-Modell konvertieren.

1. Öffnen Sie `quickstarts/conversion/Conversion.sln` in Visual Studio. Diese Lösung enthält ein C#-Konsolenprojekt.

2. Öffnen Sie die im Stammverzeichnis des Projekts aufgeführte `Configuration.cs`-Datei, und ersetzen Sie die `set-me`-Werte in den folgenden Feldern:

   | Feld         | BESCHREIBUNG                                                         |
   |---------------|---------------------------------------------------------------------|
   | AccountDomain | Die **Kontodomäne** des oben erstellten Object Anchors-Kontos |
   | AccountId     | Die **Konto-ID** des oben erstellten Object Anchors-Kontos     |
   | AccountKey    | Der **Primärschlüssel** des oben erstellten Object Anchors-Kontos     |

   Es gibt vier zusätzliche Felder, die überprüft werden müssen:

    | Feld                    | BESCHREIBUNG                       |
    | ---                      | ---                               |
    | InputAssetPath           | Der absolute Pfad zu einem 3D-Modell auf dem lokalen Computer. Die unterstützten Dateiformate sind `fbx`, `ply`, `obj`, `glb` und `gltf`. |
    | AssetDimensionUnit       | Dies ist die Maßeinheit für das 3D-Modell. Auf alle unterstützten Maßeinheiten kann mithilfe der `Azure.MixedReality.ObjectAnchors.Conversion.AssetLengthUnit`-Enumeration zugegriffen werden. |
    | Gravity                  | Dies ist die Richtung des Schwerpunktvektors des 3D-Modells. Dieser 3D-Vektor gibt die Abwärtsrichtung im Koordinatensystem Ihres Modells an. Wenn beispielsweise das negative `y` die Abwärtsrichtung im dreidimensionalen Raum des Modells angibt, dann lautet der Wert `Vector3(0.0f, -1.0f, 0.0f)`. |

3. Erstellen Sie das Projekt, und führen Sie es aus, um Ihr 3D-Modell hochzuladen und einen neuen Konvertierungsauftrag bei dem Dienst zu registrieren, und warten Sie, bis dieser abgeschlossen ist. Sobald der Auftrag abgeschlossen ist, wird das Object Anchors-Modell neben der im `InputAssetPath` angegebenen Datei herunterladen. Etwa folgende Konsolenausgabe sollte angezeigt werden:

   ```shell
    Asset   : ***********
    Gravity : ***********
    Unit    : ***********
    Attempting to upload asset...
    Attempting to create asset conversion job...
    Successfully created asset conversion job. Job ID: ***********
    Waiting for job completion...

    Asset conversion job completed successfully.
    Attempting to download result as '***********'...
    Success!
   ```

   Notieren Sie sich die **Auftrags-ID**, um sie später zu verwenden. Diese kann beim Debuggen oder bei der Problembehandlung hilfreich sein.

4. Sobald der Auftrag erfolgreich abgeschlossen ist, sollte eine Datei im Format `<Model-Filename-Without-Extension>_<JobID>.ou` am angegebenen Ausgabespeicherort angezeigt werden. Wenn der Dateiname Ihres 3D-Modells beispielsweise `chair.ply` und Ihre Auftrags-ID `00000000-0000-0000-0000-000000000000` ist, dann lautet der Dateiname, der vom Dienst ausgegeben wird, `chair_00000000-0000-0000-0000-000000000000.ou`.

## <a name="error-codes"></a>Fehlercodes
Ausführliche Informationen zu den verschiedenen Fehlercodes, die bei einem fehlerhaften Objektkonvertierungsauftrag angezeigt werden können, und deren Handhabung finden Sie auf der Seite mit den [Konvertierungsfehlercodes](..\model-conversion-error-codes.md).

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Object Anchors-Konto erstellt und ein 3D-Modell konvertiert, um ein Object Anchors-Modell zu erstellen. Wenn Sie erfahren möchten, wie Sie dieses Modell mit dem Object Anchors-SDK in Ihrer Mixed Reality-App integrieren, fahren Sie mit den folgenden Artikeln fort:

> [!div class="nextstepaction"]
> [Unity (HoloLens)](get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Unity HoloLens mit MRTK](get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [HoloLens DirectX](get-started-hololens-directx.md)

> [!div class="nextstepaction"]
> [SDK: Konvertierung](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre)
