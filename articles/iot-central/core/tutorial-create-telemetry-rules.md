---
title: 'Tutorial: Erstellen und Verwalten von Regeln in Ihrer Azure IoT Central-Anwendung'
description: In diesem Tutorial wird gezeigt, wie Azure IoT Central-Regeln es Ihnen ermöglichen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2021
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: ce10143be81da9ad797ba0ccd68837b647aeb7a7
ms.sourcegitcommit: 025a2bacab2b41b6d211ea421262a4160ee1c760
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2021
ms.locfileid: "113301970"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Tutorial: Erstellen einer Regel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung

Mithilfe von Azure IoT Central können Sie Ihre verbundenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen es Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen (wie das Senden einer E-Mail) automatisch aufzurufen. In diesem Artikel wird erläutert, wie Regeln zum Überwachen der Telemetriedaten erstellt werden, die von Ihren Geräten gesendet werden.

Telemetriedaten werden verwendet, um numerische Daten vom Gerät zu senden. Eine Regel wird ausgelöst, wenn die ausgewählten Telemetriedaten einen angegebenen Schwellenwert überschreiten.

In diesem Tutorial erstellen Sie eine Regel zum Senden einer E-Mail, wenn die von einem simulierten Sensorgerät gemessene Temperatur 70&deg; F übersteigt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen einer Regel
> * Hinzufügen einer E-Mail-Aktion

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen der Schritte in diesem Tutorial benötigen Sie Folgendes:

[!INCLUDE [iot-central-prerequisites-basic](../../../includes/iot-central-prerequisites-basic.md)]

## <a name="add-and-customize-a-device-template"></a>Hinzufügen und Anpassen einer Gerätevorlage

Hinzufügen einer Gerätevorlage aus dem Gerätekatalog. In diesem Tutorial wird die Gerätevorlage **ESP32-Azure IoT Kit** verwendet:

1. Wählen Sie zum Hinzufügen einer neuen Gerätevorlage auf der Seite **Gerätevorlagen** die Option **+ Neu** aus.

1. Scrollen Sie auf der Seite **Typ auswählen** nach unten, bis im Abschnitt **Vorkonfigurierte Gerätevorlage verwenden** die Kachel **ESP32-Azure IoT-Kit** angezeigt wird.

1. Wählen Sie die Kachel **ESP32-Azure IoT-Kit** und dann die folgende Option aus: **Next: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

Der Name der Vorlage, die Sie erstellt haben, lautet **Sensor Controller**. Das Modell enthält Komponenten, z. B. **Sensor Controller**, **SensorTemp** und **Schnittstelle „Geräteinformationen“** . Mit Komponenten werden die Funktionen eines ESP32-Geräts definiert. Zu den Funktionen gehören die Bereiche Telemetrie, Eigenschaften und Befehle.

Fügen Sie der Gerätevorlage **Sensor Controller** zwei Cloudeigenschaften hinzu:

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen** aus. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage zwei Cloudeigenschaften hinzuzufügen:

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Customer Name     | Keine          | String |

1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.

Fügen Sie der Gerätevorlage ein neues Formular hinzu, um das Gerät zu verwalten:

1. Wählen Sie den Knoten **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.

1. Ändern Sie den Formularnamen in **Manage device**.

1. Wählen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** sowie die Eigenschaft **Zieltemperatur** aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus.

1. Wählen Sie **Speichern** aus, um Ihr neues Formular zu speichern.

Veröffentlichen der Gerätevorlage jetzt.

## <a name="create-a-rule"></a>Erstellen einer Regel

Um eine Telemetrieregel zu erstellen, muss für die Gerätevorlage mindestens ein Telemetriewert definiert sein. In diesem Tutorial wird ein simuliertes Gerät **Sensor Controller** verwendet, das Telemetriedaten zur Temperatur und Luftfeuchtigkeit sendet. Die Regel überwacht die vom Gerät gemeldete Temperatur und sendet eine E-Mail, sobald die Temperatur 70 Grad übersteigt.

> [!NOTE]
> Pro Anwendung gilt ein Grenzwert von 50 Regeln.

1. Klicken Sie im linken Bereich auf **Regeln**.

1. Wenn Sie noch keine Regeln erstellt haben, wird der folgende Bildschirm angezeigt:

    :::image type="content" source="media/tutorial-create-telemetry-rules/rules-landing-page.png" alt-text="Screenshot, der die leere Liste von Regeln zeigt":::

1. Wählen Sie **+ Neu** aus, um eine neue Regel hinzuzufügen.

1. Geben Sie den Namen _Temperature monitor_ ein, um die Regel zu identifizieren, und drücken Sie anschließend die EINGABETASTE.

1. Wählen Sie die Gerätevorlage **Sensor Controller** aus. Die Regel gilt standardmäßig automatisch für alle Geräte, die der Gerätevorlage zugeordnet sind. Wenn Sie nach einer Teilmenge der Geräte filtern möchten, wählen Sie **+ Filter** aus, und verwenden Sie Geräteeigenschaften, um die Geräte anzugeben. Wenn Sie die Regel deaktivieren möchten, verwenden Sie die Umschaltfläche **Aktiviert/Deaktiviert**:

    :::image type="content" source="media/tutorial-create-telemetry-rules/device-filters.png" alt-text="Screenshot, der die Auswahl der Gerätevorlage in der Regeldefinition zeigt":::

### <a name="configure-the-rule-conditions"></a>Konfigurieren der Regelbedingungen

Die von der Regel überwachten Kriterien werden mithilfe von Bedingungen definiert. In diesem Tutorial konfigurieren Sie die Regel so, dass sie ausgelöst wird, wenn die Temperatur den Wert 70&deg; F übersteigt.

1. Wählen Sie in der Dropdownliste **Telemetrie** die Option **Temperatur** aus.

1. Wählen Sie als Nächstes unter **Operator** die Option **Ist größer als** aus, und geben Sie unter **Wert** die Zahl _70_ ein.

    :::image type="content" source="media/tutorial-create-telemetry-rules/condition-filled-out.png" alt-text="Screenshot, der die Temperaturbedingung für die Regel zeigt":::

1. Optional kann auch eine **Zeitaggregation** festgelegt werden. Bei Verwendung einer Zeitaggregation muss im entsprechenden Dropdownmenü auch ein Aggregationstyp (beispielsweise „Durchschnitt“ oder „Summe“) ausgewählt werden.

    * Ohne Aggregation wird diese Regel für jeden Telemetriedatenpunkt ausgelöst, der die Bedingung erfüllt. Wenn Sie die Regel beispielswiese so konfigurieren, dass sie bei einem Temperaturwert über 70 ausgelöst wird, erfolgt die Auslösung nahezu sofort, wenn die Temperatur diesen Wert überschreitet.
    * Mit Aggregation wird die Regel ausgelöst, wenn der Aggregatwert der Telemetriedatenpunkte im Zeitfenster die Bedingung erfüllt. Wenn Sie die Regel beispielsweise so konfigurieren, dass sie ausgelöst wird, wenn der Temperaturwert über 70 liegt, und für den Durchschnittswert der Zeitaggregation zehn Minuten festgelegt ist, wird die Regel ausgelöst, wenn das Gerät eine Durchschnittstemperatur von über 70 meldet (berechnet für ein zehnminütiges Intervall).

    :::image type="content" source="media/tutorial-create-telemetry-rules/aggregate-condition-filled-out.png" alt-text="Screenshot, der die ausgefüllte Aggregatbedingung zeigt":::

Sie können einer Regel mehrere Bedingungen hinzufügen, indem Sie **+ Bedingung** auswählen. Wenn mehrere Bedingungen angegeben werden, müssen alle Bedingungen erfüllt sein, damit die Regel ausgelöst wird. Jede Bedingung wird durch eine implizite `AND`-Klausel verknüpft. Bei Verwendung einer Zeitaggregation mit mehreren Bedingungen müssen alle Telemetriewerte aggregiert werden.

### <a name="configure-actions"></a>Konfigurieren von Aktionen

Nachdem Sie die Bedingung definiert haben, richten Sie die Aktionen ein, die ausgeführt werden sollen, wenn die Regel ausgelöst wird. Aktionen werden aufgerufen, wenn alle in der Regel angegebenen Bedingungen als „true“ ausgewertet werden.

1. Wählen Sie im Abschnitt **Aktionen** die Option **+ E-Mail** aus.

1. Geben Sie _Temperature warning_ als Anzeigenamen für die Aktion, Ihre E-Mail-Adresse in das Feld **An** und den Hinweis _You should check the device!_ als E-Mail-Text ein.

    > [!NOTE]
    > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer.md) in Azure IoT Central.

    :::image type="content" source="media/tutorial-create-telemetry-rules/configure-action.png" alt-text="Screenshot, der die E-Mail-Aktion für die Regel zeigt":::

1. Wählen Sie zum Speichern der Aktion die Option **Fertig** aus. Einer Regel können mehrere Aktionen hinzugefügt werden.

1. Um die neue Regel zu speichern, wählen Sie **Speichern** aus. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Telemetriedaten, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

Nach einer Weile erhalten Sie eine E-Mail, wenn die Regel ausgelöst wird:

:::image type="content" source="media/tutorial-create-telemetry-rules/email.png" alt-text="Screenshot, der das Feld „Benachrichtigungs-E-Mail“ zeigt":::

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen.

## <a name="enable-or-disable-a-rule"></a>Aktivieren oder Deaktivieren einer Regel

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Verwenden Sie in der Regel die Schaltfläche **Aktiviert/Deaktiviert**, um die Regel für alle in der Regel definierten Geräte zu aktivieren oder zu deaktivieren.

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>Aktivieren oder Deaktivieren einer Regel für bestimmte Geräte

Wählen Sie die Regel aus, die Sie anpassen möchten. Verwenden Sie im Abschnitt **Zielgeräte** einen oder mehrere Filter, um den Gültigkeitsbereich der Regel auf die Geräte zu beschränken, die Sie überwachen möchten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen einer telemetriebasierten Regel
* Hinzufügen einer Aktion

Nachdem Sie nun eine schwellenwertbasierte Regel definiert haben, können Sie sich als Nächstes mit Folgendem befassen:

> [!div class="nextstepaction"]
> [Konfigurieren von Regeln](howto-configure-rules.md)
