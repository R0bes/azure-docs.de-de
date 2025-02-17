---
title: Ausbildungsmodus – Personalisierung
description: Hier erfahren Sie, wie Sie den Ausbildungsmodus verwenden, um Vertrauen in ein Modell zu gewinnen, ohne dabei Code zu ändern.
author: jeffmend
ms.author: jeffme
ms.manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: aabda26cd2a1dea7205c8432f11af35e3f20b0e1
ms.sourcegitcommit: 16e25fb3a5fa8fc054e16f30dc925a7276f2a4cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2021
ms.locfileid: "122830873"
---
# <a name="use-apprentice-mode-to-train-personalizer-without-affecting-your-existing-application"></a>Verwenden des Ausbildungsmodus zum Trainieren der Personalisierung, ohne Ihre bestehende Anwendung zu beeinträchtigen

Aufgrund der Natur des vertiefenden Lernens in der **Praxis** kann ein Personalisierungsmodell nur in einer Produktionsumgebung trainiert werden. Bei der Bereitstellung eines neuen Anwendungsfalls ist das Personalisierungsmodell nicht effizient, da es Zeit braucht, bis das Modell ausreichend trainiert ist.  Der **Ausbildungsmodus** ist ein Lernverhalten, das diese Situation erleichtert und es Ihnen ermöglicht, Vertrauen in das Modell zu gewinnen, ohne dass der Entwickler Änderungen am Code vornehmen muss.

[!INCLUDE [Important Blue Box - Apprentice mode pricing tier](./includes/important-apprentice-mode.md)]

## <a name="what-is-apprentice-mode"></a>Was ist der Ausbildungsmodus?

Er ähnelt der Art, wie ein Auszubildender eine Technik von einem Experten lernt und sich mit zunehmender Erfahrung verbessern kann. Der Ausbildungsmodus ist ein _Verhalten_, das das Personalisierungsprogramm durch Beobachten der aus der vorhandenen Anwendungslogik erhaltenen Ergebnisse lernen lässt.

Das Personalisierungsprogramm trainiert, indem es dieselbe Ausgabe wie die Anwendung nachahmt. Je mehr Ereignisse fließen, desto eher kann das Personalisierungsprogramm die bestehende Anwendung _einholen_, ohne die bestehende Logik und die Ergebnisse zu beeinträchtigen. Metriken, die über das Azure-Portal und die API verfügbar sind, helfen Ihnen die Leistung zu verstehen, während das Modell lernt.

Nachdem das Personalisierungsprogramm gelernt und einen gewissen Kenntnisstand erreicht hat, kann der Entwickler das Verhalten vom Ausbildungsmodus in den Onlinemodus ändern. Zu diesem Zeitpunkt beginnt das Personalisierungsprogramm, die Aktionen in der Rangfolge-API zu beeinflussen.

## <a name="purpose-of-apprentice-mode"></a>Zweck des Ausbildungsmodus

Der Ausbildungsmodus gibt Ihnen Vertrauen in den Personalisierungsdienst und seine Fähigkeiten zum maschinellen Lernen und gibt Ihnen die Gewissheit, dass der Dienst Informationen erhält, von denen Sie lernen können, ohne den Onlinedatenverkehr zu gefährden.

Die zwei Hauptgründe für die Verwendung des Ausbildungsmodus sind:

* **Kaltstarts** werden minimiert: Der Ausbildungsmodus hilft dabei, die Kosten für die Lernzeit eines „neuen“ Modells zu verwalten und zu bewerten – wenn es nicht die bestmögliche Leistung erbringt und keinen zufriedenstellenden Effektivitätsgrad von etwa 60–80 % erreicht.
* **Überprüfen von Aktions- und Kontextfeatures**: Features, die in Aktionen und Kontext gesendet werden, können unzureichend oder ungenau sein – zu wenig, zu viel, falsch oder zu spezifisch, um die Personalisierung so zu trainieren, dass sie die ideale Effektivitätsrate erreicht. Verwenden Sie [Featureauswertungen](concept-feature-evaluation.md), um Probleme mit Features zu finden und zu beheben.

## <a name="when-should-you-use-apprentice-mode"></a>Wann sollten Sie den Ausbildungsmodus verwenden?

Verwenden Sie den Ausbildungsmodus, um die Personalisierung zu trainieren, um die Effektivität der Personalisierung durch die folgenden Szenarien zu verbessern, ohne die Erfahrung Ihrer Benutzer durch die Personalisierung zu beeinträchtigen:

* Sie implementieren die Personalisierung in einem neuen Anwendungsfall.
* Sie haben die Features, die Sie in Kontext oder Aktionen senden, erheblich geändert.
* Sie haben erheblich geändert, wann und wie Sie Relevanzen berechnen.

Der Ausbildungsmodus ist keine effektive Methode zur Messung der Auswirkungen, die die Personalisierung auf die Relevanzbewertungen hat. Zur Messung der Effektivität der Personalisierung bei der Auswahl der bestmöglichen Aktion für jeden Rangfolgeaufruf verwenden Sie [Offlineauswertungen](concepts-offline-evaluation.md).

## <a name="who-should-use-apprentice-mode"></a>Wer sollte den Ausbildungsmodus verwenden?

Der Ausbildungsmodus ist für Entwickler, Data Scientists und Geschäftsentscheidungsträger nützlich:

* **Entwickler** können den Ausbildungsmodus verwenden, um sicherzustellen, dass die Rangfolge- und Relevanz-APIs in der Anwendung ordnungsgemäß verwendet werden und dass Features, die von der Anwendung an die Personalisierung gesendet werden, keine Fehler oder nicht relevante Features wie einen Zeitstempel oder ein UserID-Element enthalten.

* **Data Scientists** können den Ausbildungsmodus verwenden, um zu überprüfen, ob die Features effektiv sind, um die Personalisierungsmodelle zu trainieren, und ob die Relevanzwartezeiten nicht zu lang oder zu kurz sind.

* **Geschäftsentscheidungsträger** können den Ausbildungsmodus verwenden, um das Potenzial der Personalisierung zur Verbesserung der Ergebnisse (d. h. der Relevanzen) im Vergleich zur bestehenden Geschäftslogik zu bewerten. Dadurch können sie eine fundierte Entscheidung treffen, die sich auf die Benutzerfreundlichkeit auswirkt, wo tatsächliche Umsätze und Benutzerzufriedenheit auf dem Spiel stehen.

## <a name="comparing-behaviors---apprentice-mode-and-online-mode"></a>Vergleichen von Verhaltensweisen – Ausbildungsmodus und Onlinemodus

Das Lernen im Ausbildungsmodus unterscheidet sich in den folgenden Punkten vom Onlinemodus.

|Bereich|Ausbildungsmodus|Onlinemodus|
|--|--|--|
|Auswirkungen auf die Benutzerfreundlichkeit|Sie können vorhandenes Benutzerverhalten verwenden, um die Personalisierung zu trainieren, indem Sie sie beobachten (nicht beeinflussen) lassen, was Ihre **Standardaktion** gewesen wäre und welche Relevanz sie erhalten hätte. Dies bedeutet, dass die Erfahrungen Ihrer Benutzer und die Geschäftsergebnisse von ihnen nicht beeinträchtigt werden.|Zeigen Sie die Top-Aktion an, die vom Rangfolgeaufruf zurückgegeben wird, um das Benutzerverhalten zu beeinflussen.|
|Lerngeschwindigkeit|Die Personalisierung lernt im Ausbildungsmodus langsamer als im Onlinemodus. Der Ausbildungsmodus kann nur durch Beobachtung der durch Ihre **Standardaktion** erzielten Relevanzen lernen, was die Lerngeschwindigkeit einschränkt, da keine Auswertung durchgeführt werden kann.|Lernt schneller, da es sowohl das aktuelle Modell nutzen als auch nach neuen Trends suchen kann.|
|Lerneffektivität: Obergrenze|Die Personalisierung kann die Leistung Ihrer grundlegenden Geschäftslogik (die Relevanzsumme, die durch die **Standardaktion** jedes Rangfolgeaufrufs erreicht wird) annähern, sehr selten erreichen und niemals überschreiten. Diese Näherungsgrenze wird durch Untersuchung reduziert. Bei einer Untersuchung von 20 % ist es beispielsweise sehr unwahrscheinlich, dass die Leistung des Ausbildungsmodus 80 % überschreitet. 60 % stellt ein sinnvolles Ziel dar, um zum Onlinemodus aufzusteigen.|Die Personalisierung sollte über die Baseline der Anwendungen hinausgehen, und im Laufe der Zeit sollten Sie dort, wo sie ins Stocken gerät, eine Offline- und Featureauswertung durchführen, um das Modell weiter zu verbessern. |
|Rangfolge-API-Wert für „rewardActionId“|Die Erfahrung der Benutzer wird nicht beeinflusst, da _rewardActionId_ immer die erste Aktion ist, die Sie in der Rangfolgenanforderung senden. Mit anderen Worten, die Rangfolge-API führt im Ausbildungsmodus keine sichtbaren Aktionen für Ihre Anwendung durch. Die Relevanz-APIs in Ihrer Anwendung sollten die Art und Weise, wie die Relevanz-API beim Moduswechsel verwendet wird, nicht ändern.|Die Erfahrung der Benutzer wird durch die _rewardActionId_ verändert, die die Personalisierung für Ihre Anwendung auswählt. |
|Auswertungen|Die Personalisierung führt einen Vergleich der Relevanzsummen, die Ihre Standardgeschäftslogik erhält, mit den Relevanzsummen, die die Personalisierung erhalten würde, wenn sie sich zu diesem Zeitpunkt im OnlinemModus befände. Ein Vergleich ist im Azure-Portal für diese Ressource verfügbar|Werten Sie die Effektivität der Personalisierung aus, indem Sie [Offlineauswertungen](concepts-offline-evaluation.md) durchführen, die es Ihnen ermöglichen, die von der Personalisierung insgesamt erzielten Relevanzen mit den potenziellen Relevanzen der Baseline der Anwendung zu vergleichen.|

Hinweis zur Effektivität des Ausbildungsmodus:

* Die Effektivität der Personalisierung im Ausbildungsmodus erreicht selten annähernd 100 % der Baseline der Anwendung und überschreitet diese nie.
* Die bewährten Methoden bestünden darin, nicht zu versuchen, 100 % zu erreichen. Je nach Anwendungsfall sollte jedoch ein Bereich von 60 bis 80 % angestrebt werden.

## <a name="limitations-of-apprentice-mode"></a>Einschränkungen des Ausbildungsmodus
Der Ausbildungsmodus versucht, das Personalisierungsmodell zu trainieren, indem er versucht, Ihren bestehenden Algorithmus für die Auswahl von Basisobjekten zu imitieren, und zwar mithilfe der in Ihrem Kontext vorhandenen Features und der in Rangfolgeaufrufen verwendeten Aktionen sowie des Feedbacks von Relevanzaufrufen. Die folgenden Faktoren wirken sich darauf aus, ob oder wann der Ausbildungsmodus der Personalisierung genügend übereinstimmende Relevanzen erlernt.

### <a name="scenarios-where-apprentice-mode-may-not-be-appropriate"></a>Szenarien, in denen der Ausbildungsmodus möglicherweise nicht geeignet ist:

#### <a name="editorially-chosen-content"></a>Redaktionell gewählter Inhalt:
In einigen Szenarien wie Nachrichten oder Unterhaltung könnte das Baselineelement manuell von einem Redaktionsteam zugewiesen werden. Dies bedeutet, dass Menschen ihr breitgefächertes Wissen und ihr Verständnis dafür, was ansprechende Inhalte sein könnten, verwenden, um bestimmte Artikel oder Medien aus einem Pool auszuwählen und diese als „bevorzugte“ oder „Helden“-Artikel zu kennzeichnen. Da es sich bei diesen Redakteuren nicht um einen Algorithmus handelt und die vom Redaktionsteam berücksichtigten Faktoren differenzierter sein können und nicht als Features des Kontexts und der Aktionen enthalten sind, ist es unwahrscheinlich, dass der Ausbildungsmodus in der Lage ist, die nächste Baselineaktion vorherzusagen. In diesen Situationen haben Sie folgende Möglichkeiten:

* Testen der Personalisierung im Onlinemodus: Dass der Ausbildungsmodus keine Baselines vorhersagt, bedeutet nicht, dass die Personalisierung keine gleichwertigen oder sogar besseren Ergebnisse erzielen kann. Ziehen Sie in Erwägung, die Personalisierung für eine gewisse Zeit in den Onlinemodus zu versetzen oder einen A/B-Test durchzuführen, wenn Sie über die entsprechende Infrastruktur verfügen, und führen Sie dann eine Offlineauswertung durch, um den Unterschied zu bewerten.
* Fügen Sie redaktionelle Überlegungen und Empfehlungen als Features hinzu: Fragen Sie Ihre Redakteure, welche Faktoren ihre Entscheidungen beeinflussen, und schauen Sie, ob Sie diese als Features in Ihren Kontext und Ihre Aktion aufnehmen können. Beispielsweise können Redakteure in einem Medienunternehmen Inhalte hervorheben, während ein bestimmter Prominenter in den Nachrichten ist: Dieses Wissen könnte als Kontextfeature hinzugefügt werden.

### <a name="factors-that-will-improve-and-accelerate-apprentice-mode"></a>Faktoren, die den Ausbildungsmodus verbessern und beschleunigen werden
Wenn der Ausbildungsmodus lernt und übereinstimmende Relevanzen über Null erreicht, aber langsam zu wachsen scheint (gelangt nicht auf 60 %..80 % übereinstimmende Relevanzen innerhalb von zwei Wochen), ist es möglich, dass die Herausforderung zu wenig Daten aufweist. Die folgenden Schritte können das Lernen beschleunigen. 

1. Hinzufügen weiterer Ereignisse mit positiven Relevanzen im Laufe der Zeit: Der Ausbildungsmodus wird in Anwendungsfällen besser funktionieren, in denen Ihre Anwendung mehr als 100 positive Relevanzen pro Tag erhält. Wenn z. B. eine Website, die einen Klick belohnt, eine Durchklickrate von 2 % aufweist, sollte sie mindestens über 5.000 Besuche pro Tag verfügen, um spürbare Lerneffekte zu erzielen. 
2. Probieren Sie eine Nutzenbewertung aus, die einfacher ist und häufiger vorkommt. Beispiel: Der Wechsel von „Haben Benutzer den Artikel zu Ende gelesen?“ zu „Haben Benutzer mit dem Lesen des Artikels begonnen?“.
3. Hinzufügen von differenzierenden Features: Sie können eine visuelle Überprüfung der Aktionen in einem Rangaufruf und in ihren Features durchführen. Verfügt die Baselineaktion über Features, die sich von anderen Aktionen unterscheiden? Wenn sie weitgehend gleich aussehen, fügen Sie weitere Features hinzu, die sie weniger ähnlich machen.
4. Reduzieren von Aktionen pro Ereignis: Die Personalisierung verwendet die Einstellung „Explore %“ (Erkunden in Prozent), um Vorlieben und Trends zu erkennen. Wenn ein Rangfolgeaufruf über mehr Aktionen verfügt, ist die Wahrscheinlichkeit geringer, dass eine Aktion für die Untersuchung gewählt wird. Reduzieren Sie die Anzahl der Aktionen, die in jedem Rangfolgeaufruf gesendet werden, auf eine kleinere Anzahl (auf weniger als 10). Dies kann eine vorübergehende Anpassung sein, um zu zeigen, dass der Ausbildungsmodus die richtigen Daten für die Relevanzen aufweist.


## <a name="using-apprentice-mode-to-train-with-historical-data"></a>Verwenden des Ausbildungsmodus zum Trainieren mit Verlaufsdaten

Wenn Sie über eine beträchtliche Menge an Verlaufsdaten verfügen, mit denen Sie die Personalisierung trainieren möchten, können Sie den Ausbildungsmodus verwenden, um die Daten über die Personalisierung wiederzugeben.

Richten Sie die Personalisierung im Ausbildungsmodus ein, und erstellen Sie ein Skript, das die Rangfolge mit den Aktionen und Kontextfeatures aus den Verlaufsdaten aufruft. Rufen Sie die Relevanz-API auf der Grundlage Ihrer Berechnungen der Datensätze in diesen Daten auf. Sie benötigen ungefähr 50.000 Verlaufsereignisse, um einige Ergebnisse zu erhalten, aber 500.000 werden für ein größeres Vertrauen in die Ergebnisse empfohlen.

Beim Training anhand von Verlaufsdaten wird empfohlen, dass die gesendeten Daten (Features für Kontext und Aktionen, ihr Layout im JSON-Code, das für Rangfolgeanforderungen verwendet wird, und die Berechnung der Relevanz in diesem Trainingsdataset) mit den Daten (Features und Berechnung der Relevanz) aus der vorhandenen Anwendung übereinstimmen.

Offline- und Post-Facto-Daten neigen dazu, unvollständiger und unübersichtlicher zu sein und sie unterscheiden sich im Format. Ein Training anhand von Verlaufsdaten ist zwar möglich, die Ergebnisse daraus sind jedoch möglicherweise nicht schlüssig und kein guter Indikator dafür, wie gut die Personalisierung lernt, insbesondere wenn sich die Features zwischen früheren Daten und der vorhandenen Anwendung unterscheiden.

Typischerweise ist für die Personalisierung im Vergleich zum Training mit Verlaufsdaten der Wechsel des Verhaltens in den Ausbildungsmodus und das Lernen aus einer vorhandenen Anwendung ein effektiverer Weg zu einem effektiven Modell, mit weniger Arbeits-, Datentechnik- und Bereinigungsaufwand.

## <a name="using-apprentice-mode-versus-ab-tests"></a>Verwenden des Ausbildungsmodus im Vergleich zu A/B-Tests

Es ist nur dann sinnvoll, A/B-Tests von Personalisierungsbehandlungen durchzuführen, wenn diese überprüft worden sind und im Onlinemodus gelernt wird. Im Ausbildungsmodus wird nur die **Standardaktion** verwendet, was bedeutet, dass alle Benutzer effektiv die Steuerungserfahrung sehen würden.

Auch wenn die Personalisierung nur die _Behandlung_ darstellt, besteht dieselbe Herausforderung, wenn die Überprüfung der Daten für das Training der Personalisierung gut ist. Stattdessen könnte der Ausbildungsmodus mit 100 % des Datenverkehrs verwendet werden, bei dem alle Benutzer die (unbeeinflusste) Steuerungserfahrung erhalten.

Sobald Sie einen Anwendungsfall mit Personalisierung und Onlinelernen haben, können Sie mit A/B-Experimenten kontrollierte Kohorten und wissenschaftliche Vergleiche von Ergebnissen durchführen, die komplexer sein können als die für Relevanzen verwendeten Signale. Eine Beispielfrage, die ein A/B-Test beantworten könnte, lautet: `In a retail website, Personalizer optimizes a layout and gets more users to _check out_ earlier, but does this reduce total revenue per transaction?`

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu [aktiven und inaktiven Ereignissen](concept-active-inactive-events.md)
