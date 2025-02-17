---
title: Verbinden von Sensoren mit einem Proxy
description: Erfahren Sie, wie Sie Azure Defender für IoT für die Kommunikation mit einem Sensor über einen Proxy ohne direkten Internetzugriff konfigurieren.
ms.topic: how-to
ms.date: 07/04/2021
ms.openlocfilehash: f16ec5c45d78237e256dcd5936ac0612d175022b
ms.sourcegitcommit: 6ea4d4d1cfc913aef3927bef9e10b8443450e663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2021
ms.locfileid: "113297896"
---
# <a name="connect-azure-defender-for-iot-sensors-without-direct-internet-access-by-using-a-proxy"></a>Verbinden von Azure Defender für IoT-Sensoren ohne direkten Internetzugriff über einen Proxy 

In diesem Artikel wird beschrieben, wie Sie Azure Defender für IoT für die Kommunikation mit einem Sensor über einen Proxy ohne direkten Internetzugriff konfigurieren. Verbinden Sie den Sensor mit einem Weiterleitungsproxy mit HTTP-Tunneling, und verwenden Sie den Befehl HTTP CONNECT für die Konnektivität. Bei den Anweisungen in diesem Artikel wird der Open-Source-Proxy Squid verwendet; Sie können jeden anderen Proxy verwenden, der CONNECT unterstützt. 

Der Proxy verwendet einen verschlüsselten SSL-Tunnel, um Daten von den Sensoren an den Dienst zu übertragen. Vom Proxy werden keine Daten überprüft, analysiert oder zwischengespeichert. 

Das folgende Diagramm zeigt, wie Daten von einem Azure Defender für IoT-Sensor im OT-Segment über einen Proxy im IT-Netzwerk und der IDMZ (Industrial Demilitarized Zone) an die Cloud gesendet werden.

:::image type="content" source="media/how-to-connect-sensor-by-proxy/cloud-access.png" alt-text="Verbinden des Sensors über die Cloud mit einem Proxy":::

## <a name="set-up-your-system"></a>Einrichten des Systems

Für dieses Szenario installieren und konfigurieren wir die neueste Version von [Squid](http://www.squid-cache.org/) auf einem Ubuntu 18-Server.

> [!Note]
> Azure Defender für IoT bietet keinen Support für Squid oder einen anderen Proxydienst.

**So installieren Sie den Squid-Proxy auf einem Ubuntu 18-Server:**

1. Melden Sie sich am gewünschten Ubuntu-Proxycomputer an.

1. Starten Sie ein Terminalfenster.
 
1. Aktualisieren Sie die Software mit dem folgenden Befehl auf die neueste Version.

    ```bash
    sudo apt-get update 
    ```

1. Installieren Sie mit dem folgenden Befehl das Squid-Paket.

    ```bash
    sudo apt-get install squid 
    ```

1. Suchen Sie die Squid-Konfigurationsdatei, die sich unter `/etc/squid/squid.conf` und `/etc/squid/conf.d/` befindet.

1. Erstellen Sie mit dem folgenden Befehl eine Sicherung der ursprünglichen Datei.

    ```bash
    sudo cp -v /etc/squid/squid.conf{,.factory}'/etc/squid/squid.conf' -> '/etc/squid/squid.conf.factory sudo nano /etc/squid/squid.conf
    ```

1. Öffnen Sie `/etc/squid/squid.conf` in einem Text-Editor.

1. Suchen Sie nach `# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS`.

1. Fügen Sie `acl sensor1 src <sensor-ip>` und `http_access allow sensor1` in die Datei ein.

    :::image type="content" source="media/how-to-connect-sensor-by-proxy/add-lines.png" alt-text="Fügen Sie die folgenden beiden Zeilen in den Text ein, und speichern Sie die Datei.":::

1. (Optional) Fügen Sie weitere Sensoren hinzu, indem Sie für jeden Sensor eine weitere Zeile hinzufügen.

1. Aktivieren Sie mit dem folgenden Befehl den Squid-Dienst, sodass dieser beim Systemstart gestartet wird.

    ```bash
    sudo systemctl enable squid 
    ```

## <a name="set-up-a-sensor-to-use-squid"></a>Einrichten eines Sensors für die Verwendung von Squid

**So richten Sie einen Sensor für die Verwendung von Squid ein:**

1. Melden Sie sich beim Sensor an.

1. Navigieren Sie zu **Systemeinstellungen** > **Netzwerk**.

1. Wählen Sie **Proxy aktivieren** aus.

    :::image type="content" source="media/how-to-connect-sensor-by-proxy/enable-proxy.png" alt-text="Auswählen von „Proxy aktivieren“ im Fenster für die Netzwerkkonfiguration des Sensors":::

1. Geben Sie die Proxyadresse ein.

1. Geben Sie einen Port ein. Der Standardport ist `3128`.

1. (Optional) Geben Sie einen Proxybenutzer und ein Kennwort ein.

1. Wählen Sie **Speichern** aus.

## <a name="see-also"></a>Weitere Informationen

[Verwalten Ihrer Abonnements](how-to-manage-subscriptions.md)
