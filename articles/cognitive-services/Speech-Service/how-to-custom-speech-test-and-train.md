---
title: Vorbereiten von Daten für Custom Speech – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Wenn Sie die Genauigkeit der Spracherkennung von Microsoft testen oder Ihre benutzerdefinierten Modelle trainieren möchten, benötigen Sie Audio- und Textdaten. Auf dieser Seite werden die einzelnen Datentypen, ihre Verwendung und ihre Verwaltung beschrieben.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: lajanuar
ms.openlocfilehash: e4c5efc165c864576191b6d74030d1dc2f5dc2a5
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122343674"
---
# <a name="prepare-data-for-custom-speech"></a>Vorbereiten von Daten für Custom Speech

Wenn Sie die Genauigkeit der Spracherkennung von Microsoft testen oder Ihre benutzerdefinierten Modelle trainieren möchten, benötigen Sie Audio- und Textdaten. Auf dieser Seite werden die Datentypen behandelt, die für ein benutzerdefiniertes Sprachmodell erforderlich sind.

## <a name="data-diversity"></a>Vielfältigkeit der Daten

Text- und Audiodaten zum Testen und Trainieren eines benutzerdefinierten Modells müssen Beispiele von unterschiedlichen Sprechern und Szenarien enthalten, die Ihr Modell erkennen soll.
Berücksichtigen Sie beim Sammeln von Daten für das Testen und Trainieren benutzerdefinierter Modelle diese Faktoren:

* Ihre Text- und Sprachaudiodaten müssen alle Arten von verbalen Aussagen abdecken, die Ihre Benutzer bei der Interaktion mit Ihrem Modell abgeben. Beispielsweise muss ein Modell, das die Temperatur erhöht und senkt, zu Aussagen trainiert werden, die Personen zum Anfordern solcher Änderungen abgeben können.
* Ihre Daten müssen alle Sprachvarianten enthalten, die das Modell erkennen muss. Viele Faktoren können die Sprache verändern, einschließlich Akzenten, Dialekten, Sprachmischung, Alter, Geschlecht, Sprachwiedergabe, Belastung und Tageszeit.
* Sie müssen Beispiele aus unterschiedlichen Umgebungen (drinnen, draußen, mit Straßenlärm) einschließen, in denen das Modell verwendet wird.
* Die Audiodaten müssen mit Hardwaregeräten gesammelt werden, die auch im Produktionssystem verwendet werden. Wenn Ihr Modell Sprache identifizieren muss, die auf Aufnahmegeräten mit unterschiedlicher Qualität aufgezeichnet wird, müssen die Audiodaten, die Sie zum Trainieren des Modells bereitstellen, auch diese verschiedenen Szenarien widerspiegeln.
* Sie können Ihrem Modell später weitere Daten hinzufügen. Achten Sie jedoch darauf, dass das Dataset vielfältig und repräsentativ für Ihre Projektanforderungen ist.
* Das Einschließen von Daten, die *nicht* zu den Erkennungsanforderungen Ihres benutzerdefinierten Modells gehören, kann die Erkennungsqualität insgesamt beeinträchtigen. Sie sollten daher keine Daten einschließen, die das Modell nicht transkribieren soll.

Ein Modell, das für eine Teilmenge von Szenarien trainiert wurde, wird nur in diesen Szenarien gut funktionieren. Wählen Sie die Daten sorgfältig aus, damit sie sämtliche Szenarien abdecken, die mit Ihrem benutzerdefinierten Modell erkannt werden sollen.

> [!TIP]
> Beginnen Sie mit kleinen Sätzen von Beispieldaten, die der Sprache und der Akustik Ihres Modells entsprechen.
> Zeichnen Sie z. B. ein kleines, aber repräsentatives Audiobeispiel auf derselben Hardware und in derselben akustischen Umgebung auf, in der Ihr Modell in Produktionsszenarien verwendet werden soll.
> Kleine Datasets mit repräsentativen Daten können Probleme offenlegen, bevor Sie mit dem Sammeln von wesentlich größeren Datasets beginnen.
>
> Zum schnellen Einstieg sollten Sie Beispieldaten verwenden. In diesem GitHub-Repository finden Sie <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">Custom Speech-Beispieldaten</a>.

## <a name="data-types"></a>Datentypen

Die folgende Tabelle enthält die zulässigen Datentypen, gibt an, wann der jeweilige Datentyp zu verwenden ist, und nennt die empfohlene Datenmenge. Nicht jeder Datentyp ist zum Erstellen eines Modells erforderlich. Die Datenanforderungen sind unterschiedlich, je nachdem, ob Sie einen Test erstellen oder ein Modell trainieren.

| Datentyp | Zum Testen verwendet | Empfohlene Menge | Für Training verwendet | Empfohlene Menge |
|-----------|-----------------|----------|-------------------|----------|
| [Audio](#audio-data-for-testing) | Ja<br>Zur visuellen Prüfung verwendet | Mindestens 5 Audiodateien | Nein | – |
| [Nur-Text](#plain-text-data-for-training) | Nein | Nicht zutreffend | Ja | 1 – 200 MB zugehöriger Text |
| [Aussprache](#pronunciation-data-for-training) | Nein | Nicht zutreffend | Ja | 1 KB – 1 MB Aussprachetext |
| [Audio + menschenmarkierte Transkripte](#audio-and-human-labeled-transcript-data) | Ja<br>Zur Bewertung der Genauigkeit verwendet | 0,5 – 5 Stunden Audio | Ja | 1–20 Stunden Audiodaten |

Dateien sollten nach Typ in einem Dataset gruppiert und als ZIP-Datei hochgeladen werden. Jedes Dataset darf nur einen einzelnen Datentyp enthalten.

> [!TIP]
> Wenn Sie ein neues Modell trainieren, beginnen Sie mit [Text](#plain-text-data-for-training). Diese Daten verbessern das Erkennen von speziellen Begriffen und Ausdrücken bereits. Training mit Textdaten ist deutlich schneller als das Training mit Audiodaten (Minuten vs. Tage).

> [!NOTE]
> Nicht alle Basismodelle unterstützen das Training mit Audiodaten. Wenn es von einem Basismodell nicht unterstützt wird, verwendet der Speech-Dienst nur den Text aus den Transkriptionen und ignoriert die Audiodaten. Eine Liste mit Basismodellen, die das Training mit Audiodaten unterstützen, finden Sie unter [Sprachunterstützung](language-support.md#speech-to-text). Selbst wenn ein Basismodell das Training mit Audiodaten unterstützt, verwendet der Dienst möglicherweise nur einen Teil der Audiodaten. Dennoch werden alle Transkriptionen verwendet.
>
> Wenn Sie das für das Training verwendete Basismodell ändern und das Trainingsdataset Audiodaten enthält, überprüfen Sie *immer*, ob das neue ausgewählte Basismodell [das Training mit Audiodaten unterstützt](language-support.md#speech-to-text). Wenn das zuvor verwendete Basismodell kein Training mit Audiodaten unterstützt hat und das Trainingsdataset Audiodaten enthält, verlängert sich die Trainingsdauer mit dem neuen Basismodell **deutlich** und kann schnell einige Stunden oder sogar mehrere Tage und mehr betragen. Dies ist insbesondere dann der Fall, wenn es sich bei der Region Ihres Abonnements für den Speech-Dienst **nicht** um eine [Region mit dedizierter Hardware](custom-speech-overview.md#set-up-your-azure-account) für Training handelt.
>
> Wenn das im obigen Abschnitt beschriebene Problem auftritt, können Sie die Trainingsdauer schnell verkürzen, indem Sie die Menge der Audiodaten im Dataset verringern oder die Audiodaten ganz daraus entfernen, sodass das Dataset nur noch Text enthält. Letzteres ist sehr empfehlenswert, wenn es sich bei der Region Ihres Abonnements für den Speech-Dienst **nicht** um eine [Region mit dedizierter Hardware](custom-speech-overview.md#set-up-your-azure-account) für Training handelt.
>
> In Regionen mit dedizierter Hardware für das Training nutzt der Sprachdienst bis zu 20 Stunden Audiodaten für das Training. In anderen Regionen verwendet er nur bis zu 8 Stunden Audiodaten.

## <a name="upload-data"></a>Hochladen von Daten

Navigieren Sie zum Hochladen der Daten zum <a href="https://speech.microsoft.com/customspeech" target="_blank">Custom Speech-Portal</a>. Navigieren Sie nach dem Erstellen eines Projekts zur Registerkarte **Speech-Datasets**, und klicken Sie auf **Daten hochladen**, um den Assistenten zu starten und Ihr erstes Dataset zu erstellen. Sie werden aufgefordert, einen Sprachdatentyp für das Dataset auszuwählen, bevor Sie die Daten hochladen können.

Zunächst müssen Sie angeben, ob das Dataset für **Training** oder **Testen** verwendet werden soll. Und es gibt mehrere Arten von Daten, die hochgeladen und zum **Trainieren** oder **Testen** verwendet werden können. Jedes hochgeladene Dataset muss den Anforderungen für den ausgewählten Datentyp entsprechen. Ihre Daten müssen ordnungsgemäß formatiert sein, bevor sie hochgeladen werden. Durch die richtige Formatierung wird sichergestellt, dass die Daten vom Custom Speech-Dienst korrekt verarbeitet werden. Die Anforderungen sind in den folgenden Abschnitten aufgelistet.

Nach dem Hochladen des Datasets haben Sie verschiedene Möglichkeiten:

* Sie können zur Registerkarte **Benutzerdefinierte Modelle trainieren** navigieren, um ein benutzerdefiniertes Modell zu trainieren.
* Sie können zur Registerkarte **Testmodelle** navigieren, um die Qualität nur mit Audiodaten visuell zu überprüfen oder die Genauigkeit mit Audio- und Humantranskriptionsdaten auszuwerten.


## <a name="plain-text-data-for-training"></a>Nur-Text-Daten für das Training

Domänenbezogene Sätze können verwendet werden, um die Genauigkeit bei der Erkennung von Produktnamen oder branchenspezifischem Fachjargon zu verbessern. Sätze können als einzelne Textdatei bereitgestellt werden. Um die Genauigkeit zu erhöhen, verwenden Sie Textdaten, die näher an den erwarteten gesprochenen Äußerungen liegen. 

Das Training mit Nur-Text kann in der Regel innerhalb weniger Minuten abgeschlossen werden.

Um ein benutzerdefiniertes Modell mithilfe von Sätzen zu erstellen, müssen Sie eine Liste von Beispieläußerungen bereitstellen. Diese Äußerungen müssen _keine_ vollständigen oder grammatikalisch korrekten Sätze sein, doch sie müssen der gesprochenen Eingabe, die Sie in der Produktionsumgebung erwarten, genau entsprechen. Wenn bestimmte Begriffe eine höhere Gewichtung haben sollen, fügen Sie mehrere Sätze hinzu, die diese Begriffe enthalten.

Als allgemeine Richtlinie gilt: Die Modellanpassung ist am effektivsten, wenn der Trainingstext so nah wie möglich an dem in der Produktion zu erwartenden Text liegt. Themenspezifischer Jargon und Ausdrücke, die Sie ergänzen möchten, sollten im Trainingstext enthalten sein. Versuchen Sie nach Möglichkeit, einen Satz oder ein Schlüsselwort auf einer separaten Zeile einzufügen. Schlüsselwörter und Ausdrücke, die für Sie wichtig sind (z. B. Produktnamen), können Sie mehrmals kopieren. Achten Sie jedoch darauf, nicht zu viel zu kopieren, da sich dies auf die Gesamterkennungsrate auswirken kann.

Stellen Sie anhand der folgenden Tabelle sicher, dass die zugehörige Datendatei für Äußerungen richtig formatiert ist:

| Eigenschaft | Wert |
|----------|-------|
| Textcodierung | UTF-8 BOM |
| Anzahl von Äußerungen pro Zeile | 1 |
| Maximale Dateigröße | 200 MB |

Darüber hinaus sollten Sie die folgenden Einschränkungen beachten:

* Vermeiden Sie es, mehr als dreimal Zeichen, Wörter oder Wortgruppen zu wiederholen. Zum Beispiel: „aaaa“, „yeah yeah yeah yeah“ oder „that's it that's it that's it that's it“. Der Sprachdienst kann Zeilen mit zu vielen Wiederholungen löschen.
* Verwenden Sie keine Sonderzeichen oder UTF-8-Zeichen über `U+00A1`.
* URIs werden zurückgewiesen.

## <a name="pronunciation-data-for-training"></a>Aussprachedaten für das Training

Wenn die Benutzer auf selten vorkommende Begriffe ohne Standardaussprache treffen oder diese verwenden werden, können Sie eine benutzerdefinierte Aussprachedatei bereitstellen, um die Erkennung zu verbessern. 
> [!IMPORTANT]
> Es wird davon abgeraten, benutzerdefinierte Aussprachedateien zu verwenden, um die Aussprache gebräuchlicher Wörter zu ändern.

Aussprache sollte als eine einzige Textdatei bereitgestellt werden. Hier sehen Sie Beispiele für eine gesprochene Äußerung und die jeweilige benutzerdefinierte Aussprache:

| Erkannte/angezeigte Form | Gesprochene Form |
|--------------|--------------------------|
| 3CPO | drei c p o |
| CNTK | c n t k |
| IEEE | i doppel e |

Die gesprochene Form ist die jeweilige Lautfolge. Sie kann aus Buchstaben, Wörtern, Silben oder einer Kombination aus allen drei bestehen.

Eine benutzerdefinierte Aussprache ist für Englisch (`en-US`) und Deutsch (`de-DE`) verfügbar. Die folgende Tabelle enthält die unterstützten Zeichen nach Sprache:

| Sprache | Gebietsschema | Zeichen |
|----------|--------|------------|
| Englisch | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| Deutsch | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

Stellen Sie anhand der folgenden Tabelle sicher, dass die zugehörige Datendatei für Aussprache richtig formatiert ist. Aussprachedateien sind klein und sollten einige KB nicht übersteigen.

| Eigenschaft | Wert |
|----------|-------|
| Textcodierung | UTF-8 BOM (ANSI wird für Englisch ebenfalls unterstützt) |
| Anzahl der Aussprachen pro Zeile | 1 |
| Maximale Dateigröße | 1 MB (1 KB für Free-Tarif) |

## <a name="audio-and-human-labeled-transcript-data"></a>Audio- und Humantranskriptionsdaten

Audio- und Humantranskriptionsdaten können sowohl zu Trainings- als auch zu Testzwecken verwendet werden. Um die akustischen Aspekte wie geringfügige Akzente, Sprechweisen, Hintergrundgeräusche oder die Genauigkeit der Spracherkennung von Microsoft bei der Verarbeitung Ihrer Audiodateien zu verbessern, müssen Sie Humantranskriptionen (Wort für Wort) für den Vergleich bereitstellen. Zwar ist das Erstellen einer menschenmarkierten Transkription oft sehr zeitaufwendig, doch wird sie benötigt, um die Genauigkeit zu bewerten und das Modell für Ihre Anwendungsfälle zu trainieren. Denken Sie daran, dass die Verbesserung der Erkennung nur so gut ist wie die bereitgestellten Daten. Aus diesem Grund ist es wichtig, dass nur qualitativ hochwertige Transkripte hochgeladen werden.

Audiodateien können am Anfang und am Ende der Aufzeichnung Stille aufweisen. Schließen Sie nach Möglichkeit in jeder Beispieldatei mindestens eine halbe Sekunde Stille vor und nach Sprache ein. Auch wenn Audiodaten mit geringer Aufzeichnungslautstärke oder störenden Hintergrundgeräuschen nicht hilfreich sind, sollte dies keine negativen Auswirkungen auf Ihr benutzerdefiniertes Modell haben. Sie sollten immer ein Upgrade Ihrer Mikrofone und Signalverarbeitungshardware in Erwägung ziehen, bevor Sie Audiostichproben sammeln.

| Eigenschaft                 | Wert                               |
|--------------------------|-------------------------------------|
| Dateiformat              | RIFF (WAV)                          |
| Samplingrate              | 8\.000 Hz oder 16.000 Hz               |
| Kanäle                 | 1 (Mono)                            |
| Maximale Länge pro Audioaufnahme | 2 Stunden (Tests)/60 s (Training) |
| Beispielformat            | PCM, 16 Bit                         |
| Archivierungsformat           | .zip                                |
| Maximale ZIP-Dateigröße         | 2 GB                                |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Haben Sie nicht einmal echte Audiodaten? Sie können auch eine Textdatei (TXT) hochladen, indem Sie den Typ **Transkript (automatische Audiosynthese)** als **Testdaten** auswählen, um ein ungefähres Gefühl für die aktuellen Genauigkeitsgrade zu bekommen. Die Audiopaare für jede gesprochene Äußerung werden automatisch mithilfe der [Sprachsynthese](text-to-speech.md) synthetisiert. 
> 
> Beachten Sie, dass die synthetisierten Audiodaten in der Regel **NICHT** zur Verwendung als **Trainingsdaten** empfohlen werden.
> 
> Die maximale Dateigröße beträgt 500 KB. Für jede Zeile wird ein Ton synthetisiert, und die maximale Größe jeder Zeile beträgt 65535 Byte.

> [!NOTE]
> Beim Hochladen von Trainings- und Testdaten darf die ZIP-Datei maximal 2 GB groß sein. Sie können nur aus *einem* Dataset testen. Achten Sie darauf, dass dieses die geeignete Dateigröße einhält. Darüber hinaus dürfen die einzelnen Trainingsdateien 60 Sekunden nicht überschreiten, andernfalls tritt ein Fehler auf.

In Hinsicht auf Probleme wie die Löschung oder Ersetzung von Wörtern sind erhebliche Datenmengen erforderlich, um die Erkennung zu verbessern. Im Allgemeinen wird empfohlen, wortwörtliche Transkriptionen für 1 bis 20 Stunden Audiodaten bereitzustellen. Aber auch schon 30 Minuten können helfen, die Erkennungsergebnisse zu verbessern. Die Transkriptionen für sämtliche WAV-Dateien sollten in einer einzelnen Textdatei enthalten sein. Jede Zeile der Transkriptionsdatei muss den Namen einer der Audiodateien enthalten – gefolgt von der jeweiligen Transkription. Der Dateiname und die Transkription sollten durch ein Tabulatorzeichen (\t) getrennt werden.

Beispiel:

<!-- The following example contains tabs. Don't accidentally convert these into spaces. -->

```input
speech01.wav    speech recognition is awesome
speech02.wav    the quick brown fox jumped all over the place
speech03.wav    the lazy dog was not amused
```

> [!IMPORTANT]
> Die Transkription muss als UTF-8-Bytereihenfolge-Marke (byte order mark, BOM) codiert sein.

Der Text der Transkriptionen wird normalisiert, damit diese vom System verarbeitet werden können. Einige wichtige Normalisierungen müssen jedoch noch vor dem Hochladen der Daten in Speech Studio vorgenommen werden. Informationen dazu, welche Sprache bei der Vorbereitung Ihrer Transkriptionen verwendet werden muss, finden Sie unter [Erstellen einer menschenmarkierten Transkription](how-to-custom-speech-human-labeled-transcriptions.md).

Wenn Sie die Audiodateien und die entsprechenden Transkriptionen zusammengetragen haben, packen Sie sie in eine gemeinsame ZIP-Datei, bevor Sie sie in <a href="https://speech.microsoft.com/customspeech" target="_blank">Speech Studio</a> hochladen. Im Folgenden sehen Sie ein Beispieldataset mit drei Audiodateien und einer von einer Person markierten Transkriptionsdatei:

> [!div class="mx-imgBorder"]
> ![Auswählen von Audio im Speech-Portal](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

Unter [Einrichten Ihres Azure-Kontos](custom-speech-overview.md#set-up-your-azure-account) finden Sie eine Liste der empfohlenen Regionen für Ihre Speech-Dienstabonnements. Durch das Einrichten des Speech-Abonnements in einer dieser Regionen verringert sich die für das Trainieren des Modells erforderliche Zeit. In diesen Regionen können beim Training zehn Stunden an Audiodaten pro Tag im Vergleich zu gerade einmal einer Stunde pro Tag in anderen Regionen verarbeitet werden. Wenn das Modelltraining nicht innerhalb einer Woche abgeschlossen werden kann, wird für das Modell ein Fehler angezeigt.

Nicht alle Basismodelle unterstützen das Training mit Audiodaten. Wenn das Basismodell das Training mit Audiodaten nicht unterstützt, ignoriert der Dienst die Audiodaten und nutzt nur den Text der Transkriptionen für das Training. In diesem Fall ist das Training identisch mit dem Training mit zugehörigem Text. Eine Liste mit Basismodellen, die das Training mit Audiodaten unterstützen, finden Sie unter [Sprachunterstützung](language-support.md#speech-to-text).

## <a name="audio-data-for-testing"></a>Audiodaten für Tests

Audiodaten eignen sich optimal, um die Genauigkeit des Microsoft-Basismodells für die Spracherkennung oder eines benutzerdefinierten Modells zu testen. Denken Sie daran, dass mit Audiodaten die Genauigkeit der Spracherkennung im Hinblick auf die Leistung eines bestimmten Modells überprüft wird. Wenn Sie die Genauigkeit eines Modells bemessen möchten, verwenden Sie [Audio- und menschenmarkierte Transkriptionsdaten](#audio-and-human-labeled-transcript-data).

Anhand der folgenden Tabelle können Sie sicherstellen, dass Ihre Audiodateien für die Verwendung mit Custom Speech richtig formatiert sind:

| Eigenschaft                 | Wert                 |
|--------------------------|-----------------------|
| Dateiformat              | RIFF (WAV)            |
| Samplingrate              | 8\.000 Hz oder 16.000 Hz |
| Kanäle                 | 1 (Mono)              |
| Maximale Länge pro Audioaufnahme | 2 Stunden               |
| Beispielformat            | PCM, 16 Bit           |
| Archivierungsformat           | .zip                  |
| Maximale Archivgröße     | 2 GB                  |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Beim Hochladen von Trainings- und Testdaten darf die ZIP-Datei maximal 2 GB groß sein. Sollten Sie mehr Daten zum Trainieren benötigen, teilen Sie sie auf mehrere ZIP-Dateien auf, und laden Sie sie separat hoch. Später können Sie auswählen, dass Sie *mehrere* Datasets zum Trainieren verwenden möchten. Sie können jedoch nur aus *einem* Dataset testen.

Verwenden Sie <a href="http://sox.sourceforge.net" target="_blank" rel="noopener">SoX</a>, um Audioeigenschaften zu überprüfen oder vorhandene Audiodaten in die entsprechenden Formate zu konvertieren. Im Folgenden finden Sie einige Beispiele für das Ausführen der einzelnen Aktivitäten über die SoX-Befehlszeile:

| Aktivität | BESCHREIBUNG | SoX-Befehl |
|----------|-------------|-------------|
| Audioformat überprüfen | Verwenden Sie diesen Befehl zum Überprüfen<br>des Dateiformats der Audiodaten. | `sox --i <filename>` |
| Audioformat konvertieren | Verwenden Sie diesen Befehl zum Konvertieren<br>der Audiodatei für einen einzelnen Kanal mit 16 Bit und 16 kHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="next-steps"></a>Nächste Schritte

* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
* [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)
* [Trainieren eines benutzerdefinierten Modells](how-to-custom-speech-train-model.md)
* [Bereitstellen des Modells](./how-to-custom-speech-train-model.md)
