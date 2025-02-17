---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 03/30/2021
ms.author: parkerra
ms.openlocfilehash: 2cd6fff6cc68baa28e85fa0c17294bcfe783ae54
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121721787"
---
Im nächsten Schritt werden die Azure Spatial Anchors-Pakete für Unity heruntergeladen. 

> [!WARNING]
> Die unterstützte Mindestversion von ASA SDK ist 2.7.0. Bei Verwendung von Unity 2020 ist ASA SDK 2.9.0 die unterstützte Mindestversion.

Damit Sie Azure Spatial Anchors in Unity verwenden können, müssen Sie das Hauptpaket sowie ein plattformspezifisches Paket für alle Plattformen (Android/iOS/HoloLens) herunterladen, die Sie unterstützen möchten.

| Plattform | Paketname                                    |
|----------|-------------------------------------------------|
| Android/iOS/HoloLens  | com.microsoft.azure.spatial-anchors-sdk.core@<version_number> |
| Android  | com.microsoft.azure.spatial-anchors-sdk.android@<version_number> |
| iOS      | com.microsoft.azure.spatial-anchors-sdk.ios@<version_number>     |
| HoloLens | com.microsoft.azure.spatial-anchors-sdk.windows@<version_number> |

# <a name="download-with-web-browser"></a>[Download über Webbrowser](#tab/unity-package-web-ui)

Suchen Sie [hier](https://dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging?_a=feed&feed=Unity-packages) das Azure Spatial Anchors-Kernpaket (com.microsoft.azure.spatial-anchors-sdk.core) für Unity. Wählen Sie die gewünschte Version aus, und laden Sie das Paket über die Schaltfläche **Download** herunter. Wiederholen Sie diesen Schritt, um das Paket für alle Plattformen herunterzuladen, die Sie unterstützen möchten.

# <a name="download-with-npm"></a>[Download mit npm](#tab/unity-package-npm)

Für diesen Schritt ist erforderlich, dass <a href="https://www.npmjs.com/get-npm" target="_blank">npm</a> installiert und verfügbar ist.

Führen Sie den folgenden Befehl aus. Ersetzen Sie dabei `<version_number>` durch die Version der Azure Spatial Anchors-Instanz, die Sie in den aktuellen Ordner herunterladen möchten:

```bash
npm pack com.microsoft.azure.spatial-anchors-sdk.core@<version_number> --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/
```

> [!NOTE]
> Führen Sie zum Auflisten der verfügbaren Versionen des Azure Spatial Anchors-Pakets den folgenden Befehl aus:
>
> ```bash
> npm view com.microsoft.azure.spatial-anchors-sdk.core --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/ versions
> ```

Das Azure Spatial Anchors-Hauptpaket wird in den Ordner heruntergeladen, in dem Sie den Befehl ausgeführt haben. Wiederholen Sie diesen Schritt, um das Paket für alle Plattformen herunterzuladen, die Sie unterstützen möchten.

# <a name="install-with-mixed-reality-feature-tool"></a>[Installation mit dem Mixed Reality-Featuretool](#tab/unity-package-mixed-reality-feature-tool)

Fahren Sie mit dem nächsten Schritt fort. Das <a href="/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool" target="_blank">Mixed Reality-Featuretool</a> verwenden Sie in einem späteren Schritt.

---