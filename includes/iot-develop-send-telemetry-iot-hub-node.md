---
title: Datei einfügen
description: Datei einfügen
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/05/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: d0f1a0bed557e27da113dcb6177c75198b47e6ad
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712876"
---
[![Code durchsuchen](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp)

In dieser Schnellstartanleitung lernen Sie einen einfachen Entwicklungsworkflow für Azure IoT-Anwendungen kennen. Sie verwenden die Azure CLI, um einen Azure IoT-Hub und ein Gerät zu erstellen. Anschließend verwenden Sie ein Azure IoT-Geräte-SDK-Beispiel, um einen simulierten Temperaturregler auszuführen, ihn sicher mit dem Hub zu verbinden und Telemetriedaten zu senden.

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- [Git](https://git-scm.com/downloads).
- [Node.js](https://nodejs.org)-Version 10 oder höher. Führen Sie zum Überprüfen der Node-Version `node --version` aus.
- Azure-Befehlszeilenschnittstelle. In dieser Schnellstartanleitung gibt es zwei Möglichkeiten zum Ausführen von Azure CLI-Befehlen:
    - Verwenden Sie Azure Cloud Shell. Dabei handelt es sich um eine interaktive Shell, mit der CLI-Befehle im Browser ausgeführt werden. Diese Option wird empfohlen, da Sie nichts installieren müssen. Wenn Sie Cloud Shell zum ersten Mal verwenden, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Führen Sie in der [Cloud Shell-Schnellstartanleitung](../articles/cloud-shell/quickstart.md) die Schritte zum **Starten von Cloud Shell** und **Auswählen der Bash-Umgebung** aus.
    - Führen Sie optional die Azure CLI auf dem lokalen Computer aus. Wenn die Azure CLI bereits installiert ist, führen Sie `az upgrade` aus, um die CLI und die Erweiterungen auf die aktuelle Version zu aktualisieren. Informationen zur Installation der Azure CLI finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

[!INCLUDE [iot-hub-include-create-hub-cli](iot-hub-include-create-hub-cli.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt verwenden Sie das Node.js SDK zum Senden von Nachrichten vom simulierten Gerät an den IoT Hub. 

1. Öffnen Sie ein neues Konsolenfenster. Sie verwenden diese Konsole, um das Node.js SDK zu installieren und mit Node.js-Beispielcode zu arbeiten. Sie sollten nun zwei Konsolenfenster geöffnet haben: das soeben geöffnete und die Cloud Shell- oder CLI-Konsole, die Sie zuvor zum Eingeben von CLI-Befehlen verwendet haben.

1. Klonen Sie in der Node-Konsole die [Azure IoT Node.js SDK-Gerätebeispiele](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) auf Ihrem lokalen Computer:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-node
    ```

1. Navigieren Sie zum Verzeichnis *azure-iot-sdk-node/device/samples/pnp*:

    ```console
    cd azure-iot-sdk-node/device/samples/pnp
    ```

1. Installieren Sie das Node.js SDK von Azure IoT sowie die erforderlichen Abhängigkeiten:

    ```console
    npm install
    ```

    Mit diesem Befehl werden die richtigen Abhängigkeiten installiert, die in der Datei *package.js* im Verzeichnis der Gerätebeispiele angegeben sind.

1. Legen Sie die beiden folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit Azure IoT herstellen kann:
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_CONNECTION_STRING` fest. Verwenden Sie als Variablenwert die Geräteverbindungszeichenfolge, die Sie im vorherigen Abschnitt gespeichert haben.
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_SECURITY_TYPE` fest. Verwenden Sie als Variable den Literalzeichenfolgenwert `connectionString`.

    **Windows (cmd)**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    set IOTHUB_DEVICE_SECURITY_TYPE=connectionString
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Zeichenfolgenwerte nicht in Anführungszeichen eingeschlossen.

    **PowerShell**

    ```azurepowershell
    $env:IOTHUB_DEVICE_CONNECTION_STRING='<your connection string here>'
    $env:IOTHUB_DEVICE_SECURITY_TYPE='connectionString'
    ```

    **Bash (Linux oder Windows)**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    export IOTHUB_DEVICE_SECURITY_TYPE="connectionString"
    ```
1. Führen Sie in der CLI-App den Befehl [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli-interactive
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Führen Sie in der Node-Konsole den Code für die installierte Beispieldatei aus. Dieser Code greift auf das simulierte IoT-Gerät zu und sendet eine Nachricht an den IoT Hub.

    So führen Sie das Node.js-Beispiel aus dem Terminal aus:
    ```console
    node pnpTemperatureController.js
    ```
    > [!NOTE]
    > In diesem Codebeispiel wird Azure IoT Plug & Play verwendet. Dadurch wird die Integration intelligenter Geräte in Ihre Lösungen ohne manuelle Konfiguration ermöglicht.  In den meisten Beispielen in dieser Dokumentation wird standardmäßig IoT Plug & Play verwendet. Weitere Informationen zu den Vorteilen und Einsatzmöglichkeiten von IoT Plug & Play finden Sie unter [Was ist IoT Plug & Play?](../articles/iot-develop/overview-iot-plug-and-play.md).

Wenn der Node.js-Code eine simulierte Telemetrienachricht von Ihrem Gerät an den IoT-Hub sendet, wird die Meldung in der CLI-App angezeigt, in der Ereignisse überwacht werden:

```output
Starting event monitor, use ctrl-c to stop...
event:
  component: thermostat1
  interface: dtmi:com:example:TemperatureController;2
  module: ''
  origin: myDevice
  payload:
    temperature: 70.5897683228018

event:
  component: thermostat2
  interface: dtmi:com:example:TemperatureController;2
  module: ''
  origin: myDevice
  payload:
    temperature: 52.87582619316418
```

Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT Hub.