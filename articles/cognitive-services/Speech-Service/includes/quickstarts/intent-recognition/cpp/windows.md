---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: pafarley
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 7fed5159398604d95da9c2dca6cee0555897386f
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2021
ms.locfileid: "123543248"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp" target="_blank">Installieren Sie das Speech SDK für Ihre Entwicklungsumgebung, und erstellen Sie ein leeres Beispielprojekt.</a>

## <a name="create-a-luis-app-for-intent-recognition"></a>Erstellen einer LUIS-App für die Absichtserkennung

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Öffnen Sie als Nächstes Ihr Projekt in Visual Studio.

1. Starten Sie Visual Studio 2019.
2. Laden Sie das Projekt, und öffnen Sie `helloworld.cpp`.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert. Beachten Sie, dass Sie eine asynchrone Methode mit dem Namen `recognizeIntent()` erstellt haben.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,72-80)]

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Bevor Sie ein `IntentRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, die den Schlüssel und Speicherort Ihrer LUIS-Vorhersageressource verwendet.

> [!IMPORTANT]
> Der Starterschlüssel und die Erstellungsschlüssel funktionieren nicht. Sie müssen den Vorhersageschlüssel und den Vorhersagespeicherort verwenden, die Sie zuvor erstellt haben. Weitere Informationen finden Sie unter [Erstellen einer LUIS-App für die Absichtserkennung](#create-a-luis-app-for-intent-recognition).

Fügen Sie diesen Code in die Methode `recognizeIntent()` ein. Stellen Sie sicher, dass Sie folgende Werte aktualisieren:

* Ersetzen Sie `"YourLanguageUnderstandingSubscriptionKey"` durch Ihren LUIS-Vorhersageschlüssel.
* Ersetzen Sie `"YourLanguageUnderstandingServiceRegion"` durch Ihren LUIS-Speicherort.  Verwenden Sie den **Regionsbezeichner** der [Region](../../../../regions.md).

>[!TIP]
> Informationen dazu, wie Sie diese Werte ermitteln, finden Sie unter [Erstellen einer LUIS-App für die Absichtserkennung](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](/cpp/cognitive-services/speech/speechconfig).

Das Speech SDK verwendet für die Erkennung standardmäßig amerikanisches Englisch (en-us). Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../../how-to-specify-source-language.md).

## <a name="initialize-an-intentrecognizer"></a>Initialisieren eines IntentRecognizer-Objekts

Als Nächstes erstellen wir ein Objekt vom Typ `IntentRecognizer`. Fügen Sie diesen Code in die Methode `recognizeIntent()` direkt unterhalb der Speech-Konfiguration ein.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Hinzufügen von LanguageUnderstandingModel und Absichten

Sie müssen der Absichtserkennung ein `LanguageUnderstandingModel` zuordnen und die zu erkennenden Absichten hinzufügen. Wir verwenden Absichten aus der vordefinierten Domäne für die Gebäudeautomatisierung.

Fügen Sie diesen Code unterhalb von `IntentRecognizer` ein. Stellen Sie sicher, dass Sie `"YourLanguageUnderstandingAppId"` durch die ID Ihrer LUIS-App ersetzen.

>[!TIP]
> Informationen dazu, wie Sie diesen Wert ermitteln, finden Sie unter [Erstellen einer LUIS-App für die Absichtserkennung](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-33)]

In diesem Beispiel wird die Funktion `AddIntent()` verwendet, um Absichten individuell hinzuzufügen. Wenn Sie alle Absichten aus einem Modell hinzufügen möchten, verwenden Sie `AddAllIntents(model)`, und übergeben Sie das Modell.

> [!NOTE]
> Vom Speech SDK werden nur LUIS-v2.0-Endpunkte unterstützt.
> Die v3.0-Endpunkt-URL im Beispielabfragefeld muss manuell geändert werden, um ein v2.0-URL-Muster zu verwenden.
> Von LUIS-v2.0-Endpunkten wird immer eines der beiden folgenden Muster verwendet:
> * `https://{AzureResourceName}.cognitiveservices.azure.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
> * `https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`

## <a name="recognize-an-intent"></a>Erkennen einer Absicht

Rufen Sie die Methode `RecognizeOnceAsync()` über das Objekt `IntentRecognizer` auf. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde. Der Einfachheit halber warten wir auf die kommende Rückgabe, um fortzufahren.

Fügen Sie diesen Code unterhalb Ihres Modells ein:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=43)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Speech-Dienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach in der Konsole aus.

Fügen Sie diesen Code unterhalb von `auto result = recognizer->RecognizeOnceAsync().get();` ein:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=46-71)]

## <a name="check-your-code"></a>Überprüfen des Codes

Ihr Code sollte nun wie folgt aussehen:

> [!NOTE]
> Wir haben dieser Version einige Kommentare hinzugefügt.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-79)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

1. **Kompilieren des Codes**: Wählen Sie auf der Menüleiste von Visual Studio **Build** > **Projektmappe erstellen** aus.
2. **Starten der App**: Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie <kbd>F5</kbd>.
3. **Starten der Erkennung**: Sie werden aufgefordert, etwas auf Englisch zu sagen. Ihre Spracheingabe wird an den Speech-Dienst gesendet, in Text transkribiert und in der Konsole ausgegeben.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]