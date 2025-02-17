---
title: Bildbeschriftung und Textbeschriftung
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Projekte erstellen und ausführen, um Bilder oder Text zu beschriften. Verwenden Sie zur Unterstützung bei dieser Aufgabe die ML-gestützte Beschriftung sowie die Human-in-the-Loop-Beschriftung.
author: sdgilley
ms.author: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 04/29/2021
ms.custom: data4ml
ms.openlocfilehash: 54ed2504063cc3a0479d37127888ccb727fbd671
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355380"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Erstellen eines Datenbeschriftungsprojekts und Exportieren der Beschriftungen

Erfahren Sie, wie Sie in Azure Machine Learning Projekte erstellen und ausführen, um Bilder oder Textdaten zu beschriften.  Verwenden Sie zur Unterstützung bei dieser Aufgabe die ML-gestützte Datenbeschriftung sowie die Human-in-the-Loop-Beschriftung.


## <a name="data-labeling-capabilities"></a>Datenbeschriftungsfunktionen

> [!Important]
> Datenbilder oder Text müssen in einem Azure-Blobdatenspeicher verfügbar sein. (Wenn kein Datenspeicher vorhanden ist, können Sie Dateien während der Projekterstellung hochladen.)

Bilddaten können Dateien eines der folgenden Typen sein: JPG, JPEG, PNG, JPE, JFIF, BMP, TIF, TIFF, DCM, DICOM. Jede Datei ist ein zu beschriftendes Element.
 
Textdaten können entweder TXT- oder CSV-Dateien sein.

* Bei TXT-Dateien stellt jede Datei ein zu beschriftendes Element dar.
* Bei CSV-Dateien stellt jede Zeile der Datei ein zu beschriftendes Element dar.

Die Datenbeschriftung von Azure Machine Learning bietet Ihnen einen zentralen Ort zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten:

- Koordinieren von Daten, Beschriftungen und Teammitgliedern, um Beschriftungsaufgaben effizient zu verwalten 
- Nachverfolgen des Fortschritts und Verwalten der Warteschlange mit unvollständigen Beschriftungsaufgaben
- Starten und Beenden des Projekts und Steuern des Beschriftungsfortschritts
- Überprüfen der beschrifteten Daten und Exportieren im COCO-Format oder als Azure Machine Learning-Dataset

## <a name="prerequisites"></a>Voraussetzungen

- Die zu beschriftenden Daten (entweder in lokalen Dateien oder in Azure Blob Storage).
- Der Satz von Beschriftungen, die Sie anwenden möchten.
- Die Anweisungen für die Beschriftung.
- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-a-data-labeling-project"></a>Erstellen eines Datenbeschriftungsprojekts

Beschriftungsprojekte werden in Azure Machine Learning verwaltet. Auf der Seite **Beschriftungsprojekte** verwalten Sie Ihre Projekte.

Wenn sich Ihre Daten bereits in einem Azure-Blobspeicher befinden, sollten Sie diesen als Datenspeicher zur Verfügung stellen, bevor Sie das Beschriftungsprojekt erstellen. Ein Beispiel zum Verwenden eines Datenspeichers finden Sie unter [Tutorial: Erstellen eines Beschriftungsprojekts für mehrklassige Bildklassifizierung](tutorial-labeling.md).

Zum Erstellen eines Projekts wählen Sie **Projekt hinzufügen** aus. Geben Sie dem Projekt einen geeigneten Namen, und wählen Sie den **Beschriftungsaufgabentyp** aus. Der Projektname kann nicht wiederverwendet werden, auch wenn das Projekt in Zukunft gelöscht wird.

### <a name="image-labeling-project"></a>Bildbeschriftungsprojekt

* Wählen Sie **Bild** aus, um ein Bildbeschriftungsprojekt zu erstellen.

    :::image type="content" source="media/how-to-create-labeling-projects/labeling-creation-wizard.png" alt-text="Erstellen eines Beschriftungsprojekts für die Bildbeschriftung.":::

  * Wählen Sie **Bildklassifizierung mit mehreren Klassen** für Projekte aus, in denen nur eine *einzige Beschriftung* aus einer Reihe von Beschriftungen auf ein Bild angewendet werden soll.
  * Wählen Sie **Bildklassifizierung mit mehreren Beschriftungen** für Projekte aus, in denen *eine oder mehrere* Beschriftungen aus einer Reihe von Beschriftungen auf ein Bild angewendet werden sollen. Ein Foto eines Hunds z. B. kann sowohl mit *Hund* als auch mit *Tag* beschriftet werden.
  * Wählen Sie **Objektidentifikation (umgebendes Rechteck)** für Projekte aus, in denen jedem Objekt innerhalb eines Bilds eine Beschriftung und ein Begrenzungsrahmen zugewiesen werden soll.
  * Wählen Sie für Projekte **Instanzsegmentierung (Polygon)** aus, wenn Sie eine Bezeichnung zuweisen und ein Polygon um jedes Objekt innerhalb eines Bilds zeichnen möchten.

* Wählen Sie **Weiter**, wenn Sie bereit sind, fortzufahren.

### <a name="text-labeling-project-preview"></a>Textbeschriftungsprojekt (Vorschau)

> [!IMPORTANT]
> Die Textbeschriftung ist derzeit als öffentliche Vorschauversion verfügbar.
> Die Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

* Wählen Sie **Text** aus, um ein Textbeschriftungsprojekt zu erstellen.

    :::image type="content" source="media/how-to-create-labeling-projects/text-labeling-creation-wizard.png" alt-text="Erstellen eines Beschriftungsprojekts für die Textbeschriftung.":::

    * Wählen Sie **Textklassifizierung mit mehreren Klassen (Vorschau)** für Projekte aus, in denen nur eine *einzige Beschriftung* aus einer Reihe von Beschriftungen auf das jeweilige Textstück angewendet werden soll.
    * Wählen Sie **Textklassifizierung mit mehreren Beschriftungen (Vorschau)** für Projekte aus, in denen *eine oder mehrere* Beschriftungen aus einer Reihe von Beschriftungen auf das jeweilige Textstück angewendet werden sollen.

* Wählen Sie **Weiter**, wenn Sie bereit sind, fortzufahren.

## <a name="specify-the-data-to-label"></a>Festlegen der zu beschriftenden Daten

Wenn Sie bereits ein Dataset mit Ihren Daten erstellt haben, wählen Sie dieses aus der Dropdownliste **Vorhandenes Dataset auswählen** aus. Oder wählen Sie **Dataset erstellen** aus, um einen vorhandenen Azure-Datenspeicher auszuwählen oder lokale Dateien hochzuladen.

> [!NOTE]
> Ein Projekt kann nicht mehr als 500.000 Dateien enthalten.  Wenn Ihr Dataset mehr umfasst, werden nur die ersten 500.000 Dateien geladen.  

### <a name="create-a-dataset-from-an-azure-datastore"></a>Erstellen eines Datasets aus einem Azure-Datenspeicher

In vielen Fällen ist es in Ordnung, einfach lokale Dateien hochzuladen. [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) bietet jedoch eine schnellere und stabilere Methode zum Übertragen großer Datenmengen. Es wird empfohlen, Storage-Explorer als Standardmethode zum Verschieben von Dateien zu verwenden.

Erstellen eines Datasets aus Dateien, die Sie bereits in einem Azure-Blobspeicher gespeichert haben:

1. Wählen Sie **Dataset erstellen** > **Aus Datenspeicher**.
1. Weisen Sie Ihrem Dataset einen **Namen** zu.
1. Wählen Sie den **Datasettyp** aus.  Für Bilder werden nur Dateien als Datasettypen unterstützt. Für ein Textbeschriftungsprojekt:
    * Wählen Sie **Tabellarisch** aus, wenn Sie eine CSV-Datei verwenden, in der jede Zeile eine Antwort darstellt.
    * Wählen Sie **Datei** aus, wenn Sie für jede Antwort separate TXT-Dateien verwenden.
1. Wählen Sie den Datenspeicher aus.
1. Wenn Ihre Daten sich in einem Unterordner des Blobspeichers befinden, klicken Sie auf **Durchsuchen**, um den Ordnerpfad auszuwählen.
    * Fügen Sie „/**“ an den Pfad an, um alle Dateien in den Unterordnern des ausgewählten Pfads einzubeziehen.
    * Fügen Sie „* */* .*“ an, um alle Daten im aktuellen Container und den zugehörigen Unterordnern einzubeziehen.
1. Geben Sie eine Beschreibung für das Dataset ein.
1. Wählen Sie **Weiter** aus.
1. Bestätigen Sie die Informationen. Wählen Sie **Zurück**, um Einstellungen zu ändern, oder **Erstellen**, um das Dataset zu erstellen.

### <a name="create-a-dataset-from-uploaded-data"></a>Erstellen eines Datasets aus hochgeladenen Daten

Direktes Hochladen Ihrer Daten:

1. Wählen Sie **Dataset erstellen** > **Aus lokalen Dateien** aus.
1. Weisen Sie Ihrem Dataset einen **Namen** zu.
1. Wählen Sie den **Datasettyp** aus.   Für Bilder werden nur Dateien als Datasettypen unterstützt. Für ein Textbeschriftungsprojekt:
    * Wählen Sie **Tabellarisch** aus, wenn Sie eine CSV-Datei verwenden, in der jede Zeile eine Antwort darstellt.
    * Wählen Sie **Datei** aus, wenn Sie für jede Antwort separate TXT-Dateien verwenden.
1. *Optional:* Wählen Sie **Erweiterte Einstellungen** aus, um den Datenspeicher, den Container und den Pfad zu Ihren Daten anzupassen.
1. Wählen Sie **Durchsuchen** aus, um die lokalen Dateien zum Hochladen auszuwählen.
1. Geben Sie eine Beschreibung des Datasets ein.
1. Wählen Sie **Weiter** aus.
1. Bestätigen Sie die Informationen. Wählen Sie **Zurück**, um Einstellungen zu ändern, oder **Erstellen**, um das Dataset zu erstellen.

Die Daten werden in den Standardblobspeicher („workspaceblobstore“) Ihres Machine Learning-Arbeitsbereichs hochgeladen.

## <a name="configure-incremental-refresh"></a><a name="incremental-refresh"> </a> Konfigurieren der inkrementellen Aktualisierung

Wenn Sie dem Dataset neue Dateien hinzufügen möchten, verwenden Sie die inkrementelle Aktualisierung, um Ihrem Projekt diese neuen Dateien hinzuzufügen.   Wenn die **inkrementelle Aktualisierung** aktiviert ist, wird das Dataset auf der Grundlage der Bezeichnungsabschlussrate regelmäßig auf neue Bilder überprüft, die einem Projekt hinzugefügt werden sollen.   Die Überprüfung auf neue Daten wird beendet, wenn das Projekt die Obergrenze von 500.000 Dateien erreicht.

Wenn Sie Ihrem Projekt weitere Dateien hinzufügen möchten, verwenden Sie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um sie in den entsprechenden Ordner im Blobspeicher hochzuladen. 

Aktivieren Sie das Kontrollkästchen **Enable incremental refresh** (Inkrementelle Aktualisierung aktivieren), wenn das Projekt den Datenspeicher kontinuierlich auf neue Daten überwachen soll. Bei Aktivierung der Option werden diese Daten einmal pro Tag in Ihr Projekt gepullt. Daher müssen Sie nach dem Hinzufügen neuer Daten zum Datenspeicher warten, bevor sie im Projekt angezeigt werden.  Im Abschnitt **Inkrementelle Aktualisierung** auf der Registerkarte **Details** können Sie für Ihr Projekt den Zeitstempel für die letzte Aktualisierung der Daten anzeigen.

Deaktivieren Sie dieses Kontrollkästchen, wenn neue Dateien, die im Datenspeicher angezeigt werden, nicht dem Projekt hinzugefügt werden sollen.

## <a name="specify-label-classes"></a>Angeben von Beschriftungsklassen

Auf der Seite **Beschriftungsklassen** geben Sie den Satz von Klassen zum Kategorisieren Ihrer Daten an. Die Genauigkeit und Geschwindigkeit Ihrer Beschriftungsersteller wird durch die Möglichkeit der Auswahl zwischen den Klassen beeinflusst. Beispiel: Anstatt Gattung und Art von Pflanzen oder Tieren vollständig anzugeben, wird empfohlen, einen Feldcode zu verwenden oder die Gattung abzukürzen.

Geben Sie eine Beschriftung pro Zeile ein. Verwenden Sie die Schaltfläche **+** , um eine neue Zeile hinzuzufügen. Wenn Sie mehr als 3 oder 4, aber weniger als 10 Beschriftungen haben, können Sie den Namen Zahlen („1: “, „2: “) voranstellen, damit die Beschriftungsersteller die Zifferntasten verwenden können, um ihre Arbeit zu beschleunigen.

## <a name="describe-the-data-labeling-task"></a>Beschreiben der Datenbeschriftungsaufgabe

Es ist wichtig, die Beschriftungsaufgabe genau zu erläutern. Auf der Seite **Beschriftungsanweisungen** können Sie einen Link zu einer externen Website für Beschriftungsanweisungen hinzufügen. Sie können aber auch Anweisungen im Bearbeitungsfeld auf der Seite angeben. Stellen Sie Anweisungen bereit, die aufgabenorientiert und für die Zielgruppe geeignet sind. Stellen Sie sich die folgenden Fragen:

* Welche Beschriftungen sehen die Ersteller, und wie treffen sie ihre Auswahl? Gibt es einen Referenztext, auf den man sich beziehen kann?
* Was sollen sie tun, wenn keine Beschriftung geeignet erscheint?
* Was sollen sie tun, wenn mehrere Beschriftungen geeignet erscheinen?
* Welchen Konfidenzschwellenwert sollen sie einer Beschriftung zuweisen? Sollen sie „nach bestem Wissen und Gewissen“ beschriften, wenn sie nicht sicher sind?
* Was sollen sie tun, wenn interessante Objekte teilweise verdeckt sind oder sich überlappen?
* Was sollen sie tun, wenn ein interessantes Objekt am Bildrand abgeschnitten ist?
* Was sollen sie tun, nachdem sie eine Beschriftung eingereicht haben und glauben, dass sie einen Fehler gemacht haben?

Für Begrenzungsrahmen stellen sich folgende wichtige Fragen:

* Wie wird der Begrenzungsrahmen für diese Aufgabe definiert? Soll er vollständig im Inneren des Objekts befinden, oder soll er sich auf der Außenseite befinden? Soll er so weit wie möglich zugeschnitten werden, oder ist ein gewisser Spielraum akzeptabel?
* Welches Maß an Sorgfalt und Konsistenz erwarten Sie von den Beschriftungserstellern beim Definieren von Begrenzungsrahmen?
* Wie sollen Objekte beschriftet werden, die nur teilweise auf dem Bild zu sehen sind? 
* Wie sollen Objekte beschriftet werden, die teilweise von einem anderen Objekt verdeckt werden?

>[!NOTE]
> Beachten Sie, dass die Beschriftungsersteller die ersten 9 Beschriftungen mit den Zifferntasten 1–9 auswählen können.

## <a name="use-ml-assisted-data-labeling"></a>Verwenden der ML-gestützten Datenbeschriftung

Auf der Seite **Durch ML unterstützte Beschriftung** können Sie automatische Machine Learning-Modelle auslösen, um Beschriftungsaufgaben zu beschleunigen. Sie ist nur für die Bildbeschriftung verfügbar. Medizinische Bilder (DCM) sind nicht in der unterstützten Beschriftung enthalten.

Zu Beginn Ihres Beschriftungsprojekts werden die Elemente in eine zufällige Reihenfolge gebracht, um potenzielle Verzerrungen zu verringern. Im Dataset enthaltene Verzerrungen fließen jedoch in das trainierte Modell ein. Wenn es sich also beispielsweise bei 80 Prozent der Elemente um eine einzelne Klasse handelt, gehören ungefähr 80 Prozent der Daten, die zum Trainieren des Modells verwendet werden, zu dieser Klasse. Dieses Training beinhaltet kein aktives Lernen.

Wählen Sie *ML-gestützte Beschriftung aktivieren* aus, und geben Sie eine GPU an, um die Beschriftungsunterstützung zu aktivieren. Diese umfasst zwei Phasen:

* Clustering (für Bildbeschriftung)
* Vorabbeschriftung 

Die genaue Anzahl beschrifteter Daten, die zum Starten der Beschriftungsunterstützung erforderlich sind, ist kein fester Wert.  Er kann zwischen Beschriftungsprojekten stark variieren. Bei manchen Projekten werden Clustering- oder Vorabbeschriftungsaufgaben nach der manuellen Beschriftung von 300 Elementen angezeigt. Bei der ML-gestützten Beschriftung kommt ein Verfahren namens *Transferlernen* zum Einsatz. Hierbei wird ein vorab trainiertes Modell verwendet, um den Trainingsprozess zu beschleunigen. Wenn die Klassen Ihres Datasets den Klassen des vorab trainierten Modells ähneln, stehen Vorabbeschriftungen ggf. bereits nach wenigen hundert manuell beschrifteten Elementen zur Verfügung. Wenn sich Ihr Dataset erheblich von den Daten unterscheidet, mit denen das Modell vorab trainiert wurde, dauert es unter Umständen deutlich länger.

Da die abschließenden Beschriftungen weiterhin von den Eingaben des Beschriftungserstellers abhängig sind, wird diese Technologie manchmal auch als *Human-in-the-Loop*-Beschriftung bezeichnet.

> [!NOTE]
> Von der ML-gestützten Datenbeschriftung werden keine Standardspeicherkonten unterstützt, die hinter einem [virtuellen Netzwerk](how-to-network-security-overview.md) geschützt sind. Sie müssen ein nicht standardmäßiges Speicherkonto für die ML-unterstützte Datenbeschriftung verwenden. Das nicht standardmäßige Speicherkonto kann hinter dem virtuellen Netzwerk gesichert werden.

### <a name="clustering"></a>Clustering

Nachdem eine bestimmte Anzahl von Beschriftungen übermittelt wurde, beginnt das Machine Learning-Modell für die Klassifizierung damit, ähnliche Elemente zu gruppieren.  Diese ähnlichen Bilder werden den Beschriftungserstellern im gleichen Bildschirm angezeigt, um das manuelle Taggen zu beschleunigen. Das Clustering ist besonders hilfreich, wenn der Beschriftungsersteller ein Raster mit vier, sechs oder neun Bildern verwendet.

Nachdem ein Machine Learning-Modell mit Ihren manuell beschrifteten Daten trainiert wurde, wird das Modell auf die letzte vollständig verbundene Ebene gekürzt. Nicht beschriftete Bilder durchlaufen dann das gekürzte Modell in einem Prozess, der in der Regel als Einbettung oder Merkmalserstellung bezeichnet wird. Dadurch werden die einzelnen Bilder jeweils in einen hochdimensionalen, durch diese Modellebene definierten Raum eingebettet. Bilder mit Pixelwiederholung in dem Raum werden für Clusteringaufgaben verwendet. 

Die Clusteringphase wird für Objekterkennungsmodelle oder die Textklassifizierung nicht angezeigt.

### <a name="prelabeling"></a>Vorabbeschriftung

Nachdem genügend Beschriftungen übermittelt wurden, wird ein Klassifizierungsmodell verwendet, um Tags vorherzusagen. Oder es wird ein Objekterkennungsmodell verwendet, um Begrenzungsrahmen vorherzusagen. Dem Beschriftungsersteller werden nun Seiten angezeigt, auf denen bereits vorhergesagte Beschriftungen für die einzelnen Elemente vorhanden sind. Bei der Objekterkennung werden auch vorhergesagte Rahmen angezeigt. Diese Vorhersagen müssen dann überprüft und falsch beschriftete Bilder korrigiert werden, bevor die Seite übermittelt wird.  

Nachdem ein Machine Learning-Modell mit Ihren manuell beschrifteten Daten trainiert wurde, wird es anhand eines Testsatzes manuell beschrifteter Elemente ausgewertet, um seine Genauigkeit mit verschiedensten Konfidenzschwellenwerten zu bestimmen. Diese Auswertung dient zur Ermittlung eines Zuverlässigkeitsschwellenwerts, über dem das Modell genau genug ist, um Vorabbeschriftungen anzuzeigen. Anschließend wird das Modell anhand von nicht beschrifteten Daten ausgewertet. Elemente, bei denen die Vorhersagezuverlässigkeit über dem Schwellenwert liegt, werden für die Vorabbeschriftung verwendet.

## <a name="initialize-the-data-labeling-project"></a>Initialisieren des Datenbeschriftungsprojekts

Nach dem Initialisieren das Beschriftungsprojekts sind einige Aspekte des Projekts unveränderlich. Sie können den Aufgabentyp oder das Dataset nicht ändern. Beschriftungen sowie die URL für die Aufgabenbeschreibung *können* dagegen geändert werden. Überprüfen Sie die Einstellungen sorgfältig, bevor Sie das Projekt erstellen. Nachdem Sie das Projekt übermittelt haben, gelangen Sie wieder zur Startseite von **Datenbeschriftung**, wo das Projekt mit dem Status **Initialisierung** angezeigt wird.

> [!NOTE]
> Diese Seite wird möglicherweise nicht automatisch aktualisiert. Aktualisieren Sie die Seite nach einer Pause manuell, um den Status des Projekts als **Erstellt** anzuzeigen.

## <a name="run-and-monitor-the-project"></a>Ausführen und Überwachen des Projekts
Sobald das Projekt initialisiert wurde, startet Azure die Ausführung. Wählen Sie das Projekt auf der Hauptseite von **Datenbeschriftung** aus, um die Details des Projekts anzuzeigen.

Um das Projekt anzuhalten oder neu zu starten, schalten Sie den Status **Wird ausgeführt** oben rechts um. Sie können Daten nur dann beschriften, wenn das Projekt ausgeführt wird.

### <a name="dashboard"></a>Dashboard

Auf der Registerkarte **Dashboard** wird der Fortschritt der Beschriftungsaufgabe angezeigt.

:::image type="content" source="media/how-to-create-labeling-projects/labeling-dashboard.png" alt-text="Dashboard für Datenbeschriftung":::

Das Fortschrittsdiagramm zeigt an, wie viele Elemente beschriftet wurden und wie viele noch übrig sind.  Für ausstehende Elemente kann Folgendes gelten:

* Noch nicht zu einer Aufgabe hinzugefügt
* In einer Aufgabe enthalten, die einem Beschriftungsersteller zugewiesen aber noch nicht abgeschlossen ist 
* In der Warteschlange von Aufgaben, die noch zugewiesen werden müssen

Der mittlere Abschnitt zeigt die Warteschlange der Aufgaben, die noch zugewiesen werden müssen. Wenn die ML-gestützte Beschriftung deaktiviert ist, wird in diesem Abschnitt die Anzahl der manuellen Aufgaben angezeigt, die zugewiesen werden müssen. Wenn die ML-gestützte Beschriftung aktiviert ist, wird auch Folgendes angezeigt:

* Aufgaben, die gruppierte Elemente in der Warteschlange enthalten
* Aufgaben, die vorbeschriftete Elemente in der Warteschlange enthalten

Wenn die ML-gestützte Beschriftung aktiviert ist, wird außerdem in einer kleinen Statusleiste angezeigt, wann das nächste Training ausgeführt wird.  In den Abschnitten zu Experimenten werden Links zu den einzelnen Machine Learning-Ausführungen angezeigt.

* Training – trainiert ein Modell zum Vorhersagen der Beschriftungen.
* Validierung – bestimmt, ob die Vorhersage dieses Modells für das Vorbeschriften der Elemente verwendet wird. 
* Rückschluss – Vorhersageausführung für neue Elemente
* Featurisierung – Clusterelemente (nur für Bildklassifizierungsprojekte)

Auf der rechten Seite befindet sich eine Verteilung der Beschriftungen für abgeschlossene Aufgaben.  Beachten Sie, dass ein Element in einigen Projekttypen mehrere Beschriftungen aufweisen kann. In diesem Fall kann die Gesamtanzahl der Beschriftungen größer als die Gesamtanzahl der Elemente sein.

### <a name="data-tab"></a>Registerkarte „Daten“

Auf der Registerkarte **Daten** können Sie Ihr Dataset anzeigen und beschriftete Daten überprüfen. Wenn Sie falsch beschriftete Daten sehen, können Sie diese auswählen und auf **Ablehnen** klicken. Dadurch werden die Beschriftungen entfernt und die Daten wieder in die Warteschlange der unbeschrifteten Daten eingereiht.

### <a name="details-tab"></a>Registerkarte „Details“

Zeigen Sie Details zu Ihrem Projekt an.  Auf dieser Registerkarte haben Sie folgende Optionen:

* Anzeigen von Projektdetails und Eingabedatasets
* Aktivieren der inkrementellen Aktualisierung
* Anzeigen von Details zum Speichercontainer, in dem beschriftete Ausgaben in Ihrem Projekt gespeichert werden
* Hinzufügen von Beschriftungserstellern zu Ihrem Projekt
* Bearbeiten von Anweisungen zu Ihren Beschriftungen
* Bearbeiten der Details der ML-gestützten Beschriftung einschließlich Aktivieren/Deaktivieren

### <a name="access-for-labelers"></a>Zugriff für Beschriftungsersteller

Jeder Benutzer, der Zugriff auf Ihren Arbeitsbereich hat, kann Daten in Ihrem Projekt beschriften.  Sie können auch die Berechtigungen für Ihre Beschriftungsersteller anpassen, sodass sie zwar auf die Beschriftung, aber nicht auf andere Teile des Arbeitsbereichs oder das Beschriftungsprojekt zugreifen können.  Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md). Dort erfahren Sie auch, wie Sie die [benutzerdefinierte Rolle „Beschriftungsersteller“](how-to-assign-roles.md#labeler) erstellen.

## <a name="add-new-label-class-to-a-project"></a>Hinzufügen einer neuen Beschriftungsklasse zu einem Projekt

Im Rahmen des Datenbeschriftungsprozesses stellen Sie möglicherweise fest, dass Sie weitere Beschriftungen benötigen, um Ihre Bilder zu klassifizieren.  So kann es beispielsweise sein, dass Sie für verwirrende Bilder Beschriftungen wie „Unbekannt“ oder „Sonstiges“ hinzufügen möchten.

Gehen Sie wie folgt vor, um einem Projekt Beschriftungen hinzuzufügen:

1. Wählen Sie das Projekt auf der Hauptseite von **Datenbeschriftung** aus.
1. Schalten Sie auf der Seite rechts oben den Status **Wird ausgeführt** auf **Angehalten**, um Beschriftungsersteller von ihrer Tätigkeit abzuhalten.
1. Wählen Sie die Registerkarte **Details** aus.
1. Wählen Sie in der Liste auf der linken Seite die Option **Beschriftungsklassen** aus.
1. Wählen Sie im oberen Bereich der Liste **+ Beschriftungen hinzufügen** aus. ![Hinzufügen einer Beschriftung](media/how-to-create-labeling-projects/add-label.png)
1. Fügen Sie im Formular Ihre neue Beschriftung hinzu, und wählen Sie aus, wie Sie fortfahren möchten.  Da Sie die verfügbaren Beschriftungen geändert haben, wählen Sie aus, wie mit den bereits beschrifteten Daten verfahren werden soll:
    * „Von vorn beginnen und alle vorhandenen Beschriftungen entfernen.“  Wählen Sie diese Option aus, wenn Sie mit der Beschriftung von vorn beginnen und dabei den neuen vollständigen Beschriftungssatz verwenden möchten. 
    * „Von vorn beginnen und alle vorhandenen Beschriftungen beibehalten.“  Wählen Sie diese Option aus, um alle Daten als nicht beschriftet zu markieren, die vorhandenen Beschriftungen aber als Standardtag für Bilder beizubehalten, die bereits beschriftet wurden.
    * „Fortfahren und alle vorhandenen Beschriftungen beibehalten.“ Wählen Sie diese Option aus, um die bereits beschrifteten Daten unverändert zu lassen und von nun an die neue Beschriftung für noch nicht beschriftete Daten verwenden zu können.
1. Passen Sie Ihre Anweisungsseite nach Bedarf für die neuen Beschriftungen an.
1. Schalten Sie nach dem Hinzufügen aller neuen Beschriftungen oben rechts auf der Seite den Status **Angehalten** auf **Wird ausgeführt**, um das Projekt wieder zu starten.  

## <a name="export-the-labels"></a>Exportieren der Beschriftungen
 
Verwenden Sie die Schaltfläche **Exportieren** auf der Seite **Projektdetails** Ihres Beschriftungsprojekts. Sie können die Beschriftungsdaten für Machine Learning-Experimente jederzeit exportieren. 

* Textbeschriftungen können wie folgt exportiert werden:
    * Eine CSV-Datei. Die CSV-Datei wird im Standardblobspeicher des Azure Machine Learning-Arbeitsbereichs in einem Ordner unter *Labeling/export/csv* erstellt. 
    * Ein [Azure Machine Learning-Dataset mit Beschriftungen](how-to-use-labeled-dataset.md). 

* Bildbeschriftungen können wie folgt exportiert werden:
    * [COCO-Format](http://cocodataset.org/#format-data). Die COCO-Datei wird im Standardblobspeicher des Azure Machine Learning-Arbeitsbereichs in einem Ordner unter *Labeling/export/coco* erstellt. 
    * Ein [Azure Machine Learning-Dataset mit Beschriftungen](how-to-use-labeled-dataset.md). 

Sie können im Abschnitt **Datasets** von Machine Learning auf das exportierte Azure Machine Learning-Dataset zugreifen. Die Seite mit Datasetdetails bietet auch Beispielcode für den Zugriff auf Ihre Beschriftungen aus Python.

![Exportiertes Dataset](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="troubleshooting"></a>Problembehandlung

Verwenden Sie diese Tipps, wenn Sie eines der folgenden Probleme feststellen.

|Problem  |Lösung  |
|---------|---------|
|Es können nur Datasets verwendet werden, die für Blobdatenspeicher erstellt wurden.     |  Dies ist eine bekannte Einschränkung der aktuellen Version.       |
|Nach der Erstellung wird für das Projekt lange der Status „Wird initialisiert“ angezeigt.     | Aktualisieren Sie die Seite manuell. Die Initialisierung sollte mit ungefähr 20 Datenpunkten pro Sekunde durchgeführt werden. Die fehlende automatische Aktualisierung ist ein bekanntes Problem.         |
|Beim Überprüfen von Bildern werden neu bezeichnete Bilder nicht angezeigt.     |   Wählen Sie die Schaltfläche **Erste** aus, um alle bezeichneten Bilder zu laden. Mit der Schaltfläche **Erste** gelangen Sie zurück an den Anfang der Liste, aber es werden alle bezeichneten Daten geladen.      |
|Wird während der Erstellung von Bezeichnungen für die Objekterkennung ESC gedrückt, wird in der linken oberen Ecke eine Bezeichnung mit der Größe null erstellt. In diesem Fall ist die Übermittlung von Bezeichnungen nicht erfolgreich.     |   Löschen Sie die Bezeichnung, indem Sie auf das daneben angezeigte Kreuzsymbol klicken.  |
|Das Zuweisen einer Gruppe von Aufgaben zu einem bestimmten Beschriftungsersteller ist nicht möglich.     |   Dies ist eine bekannte Einschränkung der aktuellen Version.  |

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Erstellen eines Beschriftungsprojekts für mehrklassige Bildklassifizierung](tutorial-labeling.md).
* Beschriften von Bildern für die [Bildklassifizierung oder Objekterkennung](how-to-label-data.md)
* Weitere Informationen zu [Azure Machine Learning und Machine Learning Studio (klassisch)](./overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)
