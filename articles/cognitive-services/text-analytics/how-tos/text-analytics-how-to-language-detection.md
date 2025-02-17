---
title: Sprachenerkennung mit der Textanalyse-REST-API
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API von Azure Cognitive Services Sprachen erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 07/02/2021
ms.author: aahi
ms.openlocfilehash: 33dbcfb3faa046eeeec2dc19a45ece9bcf270b76
ms.sourcegitcommit: cc099517b76bf4b5421944bd1bfdaa54153458a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113549776"
---
# <a name="example-detect-language-with-text-analytics"></a>Beispiel: Sprachenerkennung mithilfe der Textanalyse

Das Feature für die [Sprachenerkennung](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/Languages) der Azure-Textanalyse-REST-API wertet Texteingaben für jedes Dokument aus und gibt Sprachen-IDs mit einer Punktzahl zurück, die die Stärke der Analyse angibt.

Diese Funktion ist hilfreich für Inhaltsspeicher, die willkürliche Texte mit unbekannter Sprache sammeln. Sie können die Analyseergebnisse analysieren, um die Sprache des Eingabedokuments zu bestimmen. Die Antwort gibt außerdem eine Punktzahl zurück, die die Zuverlässigkeit des Modells widerspiegelt. Der Wert liegt zwischen 0 und 1.

Mit dem Feature für die Sprachenerkennung können eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen erkannt werden. Für dieses Feature wird keine genaue Liste mit Sprachen veröffentlicht.

Bei Inhalten in einer seltener verwendeten Sprache können Sie das Feature für die Sprachenerkennung ausprobieren, um zu sehen, ob es einen Code zurückgibt. Die Antwort bei Sprachen, die nicht erkannt werden können, lautet `unknown`.

> [!TIP]
> Die Textanalyse bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Spracherkennung, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

## <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID und Text.

Ein Dokument darf maximal 5.119 Zeichen enthalten. Pro Sammlung können bis zu 1.000 Elemente (IDs) vorhanden sein. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel zeigt Inhalte, die Sie für die Sprachenerkennung übermitteln können:

```json
{
    "documents": [
        {
            "id": "1",
            "text": "This document is in English."
        },
        {
            "id": "2",
            "text": "Este documento está en inglés."
        },
        {
            "id": "3",
            "text": "Ce document est en anglais."
        },
        {
            "id": "4",
            "text": "本文件为英文"
        },
        {
            "id": "5",
            "text": "Этот документ на английском языке."
        }
    ]
}
```

## <a name="step-1-structure-the-request"></a>Schritt 1: Strukturieren der Anforderung

Weitere Informationen zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine POST-Anforderung. Die API-Dokumentation für diese Anforderung finden Sie [hier](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/Languages).

+ Legen Sie den HTTP-Endpunkt für die Sprachenerkennung fest. Verwenden Sie entweder eine Textanalyseressource in Azure oder einen instanziierten [Textanalysecontainer](text-analytics-how-to-install-containers.md). Sie müssen `/text/analytics/v3.1/languages` in die URL einschließen. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/languages`.

+ Legen Sie einen Anforderungsheader fest, der den [Zugriffsschlüssel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) für Textanalysevorgänge enthält.

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/Languages), um eine Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Senden der Anforderung per POST

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie im Artikel [Datengrenzwerte](../concepts/data-limits.md).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.


## <a name="step-3-view-the-results"></a>Schritt 3: Zeigen Sie die Ergebnisse an

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Eigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei im lokalen System speichern. Importieren Sie dann die Ausgabe in eine Anwendung, mit der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Ergebnisse für die Beispielanforderung sollten wie im folgenden JSON-Dokument aussehen. Beachten Sie, dass es sich um ein JSON-Dokument mit mehreren Elementen handelt, bei dem jedes Element das Erkennungsergebnis für jedes Dokument darstellt, das Sie übermittelt haben. Die Ausgabe erfolgt in englischer Sprache. 

Die Spracherkennung gibt eine vorherrschende Sprache für ein Dokument sowie den [ISO 639-1-Namen](https://www.iso.org/standard/22109.html), den Anzeigenamen und die Konfidenzpunktzahl zurück. Das positive Ergebnis 1,0 steht für die höchstmögliche Zuverlässigkeit der Analyse.

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "English",
                "iso6391Name": "en",
                "confidenceScore": 0.99
            },
            "warnings": []
        },
        {
            "id": "2",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 0.91
            },
            "warnings": []
        },
        {
            "id": "3",
            "detectedLanguage": {
                "name": "French",
                "iso6391Name": "fr",
                "confidenceScore": 0.78
            },
            "warnings": []
        },
        {
            "id": "4",
            "detectedLanguage": {
                "name": "Chinese_Simplified",
                "iso6391Name": "zh_chs",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "5",
            "detectedLanguage": {
                "name": "Russian",
                "iso6391Name": "ru",
                "confidenceScore": 1.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-01-05"
}
```

### <a name="ambiguous-content"></a>Mehrdeutige Inhalte

In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen Länder-/Regionscode gemäß [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen.

„Impossible“ ist beispielsweise sowohl im Englischen als auch im Französischen gebräuchlich, und bei unzureichendem Kontext basiert die Antwort auf dem Länder-/Regionshinweis „US“. Sollte bekannt sein, dass der Text aus Frankreich stammt, kann ein entsprechender Hinweis angegeben werden.

**Input** (Eingabe)

```json
{
    "documents": [
        {
            "id": "1",
            "text": "impossible"
        },
        {
            "id": "2",
            "text": "impossible",
            "countryHint": "fr"
        }
    ]
}
```

Der Dienst verfügt nun über zusätzlichen Kontext und kann eine bessere Beurteilung abgeben: 

**Ausgabe**

```json
{
    "documents":[
        {
            "detectedLanguage":{
                "confidenceScore":0.62,
                "iso6391Name":"en",
                "name":"English"
            },
            "id":"1",
            "warnings":[
                
            ]
        },
        {
            "detectedLanguage":{
                "confidenceScore":1.0,
                "iso6391Name":"fr",
                "name":"French"
            },
            "id":"2",
            "warnings":[
                
            ]
        }
    ],
    "errors":[
        
    ],
    "modelVersion":"2020-09-01"
}
```

Wenn das Analysetool die Eingabe nicht analysieren kann, wird `(Unknown)` zurückgegeben. Beispiel: Sie übermitteln einen Textblock, der ausschließlich aus arabischen Zahlen besteht.

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "(Unknown)",
                "iso6391Name": "(Unknown)",
                "confidenceScore": 0.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-01-05"
}
```

### <a name="mixed-language-content"></a>Inhalt in verschiedenen Sprachen

Wenn in einem Dokument Inhalte in verschiedenen Sprachen enthalten sind, wird die Sprache zurückgegeben, die in den Inhalten überwiegt – allerdings mit einer niedrigeren positiven Bewertung. Die Bewertung spiegelt die marginale Stärke dieser Bewertung wider. Im folgenden Beispiel enthält die Eingabe eine Mischung aus Englisch, Spanisch und Französisch. Das Analysetool zählt die Zeichen der einzelnen Segmente, um die vorherrschende Sprache zu bestimmen.

**Input** (Eingabe)

```json
{
    "documents": [
        {
            "id": "1",
            "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
        }
    ]
}
```

**Ausgabe**

Die resultierende Ausgabe enthält die vorherrschende Sprache mit einer Punktzahl von weniger als 1,0, wodurch eine niedrigere Zuverlässigkeit angegeben wird.

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 0.88
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-01-05"
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Sprachenerkennung unter Verwendung der Textanalyse in Azure Cognitive Services vertraut gemacht. Die folgenden Punkte wurden erläutert und veranschaulicht:

+ Die [Sprachenerkennung](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/Languages) ist für eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID und Text.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/languages` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Die Antwortausgabe besteht aus Sprachen-IDs für die jeweilige Dokument-ID. Die Ausgabe kann an eine beliebige JSON-fähige App gestreamt werden. Zu den Beispiel-Apps zählen Excel und Power BI, um nur einige zu nennen.

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über die Textanalyse](../overview.md)
* [Verwenden der Textanalyse-Clientbibliothek](../quickstarts/client-libraries-rest-api.md)
* [Neuigkeiten](../whats-new.md)
* [Modellversionen](../concepts/model-versioning.md)
