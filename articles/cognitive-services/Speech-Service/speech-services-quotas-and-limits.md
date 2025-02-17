---
title: Speech-Dienst – Kontingente und Limits
titleSuffix: Azure Cognitive Services
description: Kurzübersicht, ausführliche Beschreibung und bewährte Methoden im Zusammenhang mit Kontingenten und Grenzwerten für Azure Cognitive Speech-Dienste
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2021
ms.author: alexeyo
ms.openlocfilehash: a71a2736533cd2c707d4224db3790e93fe5e2782
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355823"
---
# <a name="speech-service-quotas-and-limits"></a>Speech-Dienst – Kontingente und Limits

Dieser Artikel enthält eine Kurzübersicht und eine **ausführliche Beschreibung** der Kontingente und Grenzwerte für Azure Cognitive Speech-Dienste für alle [Tarife](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/). Außerdem finden Sie hier einige bewährte Methoden zur Vermeidung der Anforderungsdrosselung.

## <a name="quotas-and-limits-quick-reference"></a>Kurzübersicht über Kontingente und Grenzwerte
Zu [Kontingente und Grenzwerte für die Sprachsynthese nach Speech-Ressource](#text-to-speech-quotas-and-limits-per-speech-resource)
### <a name="speech-to-text-quotas-and-limits-per-speech-resource"></a>Kontingente und Grenzwerte für die Spracherkennung nach Speech-Ressource
In den folgenden Tabellen sind Parameter ohne die Zeile „Anpassbar“ **nicht** für alle Tarife anpassbar.

#### <a name="online-transcription"></a>Onlinetranskription
Zur Verwendung mit dem [Speech SDK](speech-sdk.md) und/oder der [Spracherkennungs-REST-API für kurze Audiodaten](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio)

| Kontingent | Free (F0)<sup>1</sup> | Standard (S0) |
|--|--|--|
| **Grenzwert für gleichzeitige Anforderungen – Basismodell-Endpunkt** | 1 | 100 (Standardwert) |
| Anpassbar | Nein<sup>2</sup> | Ja<sup>2</sup> |
| **Grenzwert für gleichzeitige Anforderungen – benutzerdefinierter Endpunkt** | 1 | 100 (Standardwert) |
| Anpassbar | Nein<sup>2</sup> | Ja<sup>2</sup> |

#### <a name="batch-transcription"></a>Batch-Transkription
| Kontingent | Free (F0)<sup>1</sup> | Standard (S0) |
|--|--|--|
| [Sprache-in-Text REST API V2.0 und v3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30) Limit | Batchtranskription ist für F0 nicht verfügbar. | 300 Anforderungen pro Minute |
| Maximale Größe der Audioeingabedatei | Nicht zutreffend | 1 GB |
| Maximale Größe des Eingabeblobs (kann mehrere Dateien enthalten, z. B. in einem ZIP-Archiv; beachten Sie den Grenzwert für die Dateigröße oben) | – | 2,5 GB |
| Maximale Größe des Blob-Containers | – | 5 GB |
| Maximale Anzahl von Blobs pro Container | – | 10000 |
| Maximale Anzahl von Dateien pro Transkriptionsanforderung (bei Verwendung mehrerer Inhalts-URLs als Eingabe) | – | 1000  |
| Maximale Anzahl gleichzeitig ausgeführter Aufträge | – | 2000  |

#### <a name="model-customization"></a>Modellanpassung
| Kontingent | Free (F0)<sup>1</sup> | Standard (S0) |
|--|--|--|
| REST-API-Grenzwert | 300 Anforderungen pro Minute | 300 Anforderungen pro Minute |
| Maximale Anzahl von Sprach-Datasets | 2 | 500 |
| Maximale Dateigröße für das Akustikdataset für den Datenimport | 2 GB | 2 GB |
| Maximale Dateigröße für das Sprachdataset für den Datenimport | 200 MB | 1,5 GB |
| Maximale Dateigröße für das Aussprachedataset für den Datenimport | 1 KB | 1 MB |
| Maximale Textgröße bei Verwendung des `text`-Parameters in der [Create Model](https://westcentralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CreateModel/)-API-Anforderung | 200 KB | 500 KB |

<sup>1</sup> Sehen Sie sich im Zusammenhang mit dem Tarif **Free (F0)** auch die monatlichen Freibeträge auf der [Preisseite](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) an.<br/>
<sup>2</sup> Sehen Sie sich [weitere Erläuterungen](#detailed-description-quota-adjustment-and-best-practices), [bewährte Methoden](#general-best-practices-to-mitigate-throttling-during-autoscaling) und [Anpassungsanweisungen](#speech-to-text-increasing-online-transcription-concurrent-request-limit) an.<br/>

### <a name="text-to-speech-quotas-and-limits-per-speech-resource"></a>Kontingente und Grenzwerte für die Sprachsynthese nach Speech-Ressource
In den folgenden Tabellen sind Parameter ohne die Zeile „Anpassbar“ **nicht** für alle Tarife anpassbar.

#### <a name="general"></a>Allgemein

| Kontingent | Free (F0)<sup>3</sup> | Standard (S0) |
|--|--|--|
| **Maximale Anzahl von Transaktionen pro Sekunde (TPS) pro Speech-Ressource** |  |  |
| Echtzeit-API. Stimmen: Standard, neuronal, benutzerdefiniert und benutzerdefiniert neuronal | 200<sup>4</sup> | 200<sup>4</sup> |
| Anpassbar | Nein<sup>4</sup> | Nein<sup>4</sup> |
| **HTTP-spezifische Kontingente** |  |  |
| Maximal generierte Audiolänge pro Anforderung | 10 Min. | 10 Min. |
| Maximale Gesamtanzahl von unterschiedlichen `<voice>`- und `<audio>`-Tags in SSML | 50 | 50 |
| **WebSocket-spezifische Kontingente** |  |  |
| Maximal generierte Audiolänge pro Turn | 10 Min. | 10 Min. |
| Maximale Gesamtanzahl von unterschiedlichen `<voice>`- und `<audio>`-Tags in SSML | 50 | 50 |
| Maximale SSML-Nachrichtengröße pro Turn | 64 KB | 64 KB |

#### <a name="long-audio-api"></a>API für lange Audioinhalte

| Kontingent | Free (F0)<sup>3</sup> | Standard (S0) |
|--|--|--|
| Minimale Textlänge | – | 400 Zeichen für reinen Text, 400 [abrechenbare Zeichen](text-to-speech.md#pricing-note) für SSML |
| Maximale Textlänge | – | 10.000 Absätze |
| Startzeit | – | 10 Aufgaben oder 10.000 Zeichen, akkumuliert |

#### <a name="custom-neural-voice-and-custom-voicesup6sup"></a>Benutzerdefinierte neuronale Stimme und Custom Voice<sup>6</sup>

| Kontingent | Free (F0)<sup>3</sup> | Standard (S0) |
|--|--|--|
| Maximale Anzahl von Transaktionen pro Sekunde (TPS) pro Speech-Ressource | [Siehe oben](#general) | [Siehe oben](#general) |
| Maximale Anzahl von Datasets pro Speech-Ressource | 10 | 500 |
| Maximale Anzahl simultaner Datasetuploads pro Speech-Ressource | 2 | 5 |
| Maximale Datendateigröße für den Datenimport pro Dataset | 2 GB | 2 GB |
| Upload langer Audiospuren oder Audiospuren ohne Skript | Nein | Ja |
| Maximale Anzahl simultaner Modelltrainings pro Speech-Ressource | 1 (nur Custom Voice<sup>6</sup>) | 3 |
| Maximale Anzahl benutzerdefinierter Endpunkte pro Speech-Ressource | 1 (nur Custom Voice<sup>6</sup>) | 50 |
| **Grenzwert für gleichzeitige Anforderungen für benutzerdefinierte neuronale Stimme** |  |  |
| Standardwert | – | 10 |
| Anpassbar | – | Ja<sup>5</sup> |
| **Grenzwert für gleichzeitige Anforderungen für Custom Voice<sup>6</sup>** |  |  |
| Standardwert | 10 | 10 |
| Anpassbar | Nein<sup>5</sup> | Ja<sup>5</sup> |

<sup>3</sup> Sehen Sie sich im Zusammenhang mit dem Tarif **Free (F0)** auch die monatlichen Freibeträge auf der [Preisseite](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) an.<br/>
<sup>4</sup> Sehen Sie sich [weitere Erläuterungen](#detailed-description-quota-adjustment-and-best-practices) und [bewährte Methoden](#general-best-practices-to-mitigate-throttling-during-autoscaling) an.<br/>
<sup>5</sup> Sehen Sie sich [weitere Erläuterungen](#detailed-description-quota-adjustment-and-best-practices), [bewährte Methoden](#general-best-practices-to-mitigate-throttling-during-autoscaling) und [Anpassungsanweisungen](#text-to-speech-increasing-concurrent-request-limit-for-custom-neural-and-custom-voices) an.<br/>
<sup>6</sup> Custom Voice wird als veraltet markiert und ist für neu erstellte Speech-Ressourcen nicht verfügbar. Weitere Informationen finden Sie [hier](how-to-custom-voice.md#migrate-to-custom-neural-voice).<br/>

## <a name="detailed-description-quota-adjustment-and-best-practices"></a>Ausführliche Beschreibung, Kontingentanpassung und bewährte Methoden
Überprüfen Sie vor dem Anfordern einer Kontingenterhöhung (sofern zutreffend), dass sie wirklich erforderlich ist. Vom Speech-Dienst werden Technologien zur automatischen Skalierung verwendet, um die erforderlichen Rechenressourcen bei Bedarf bereitzustellen und gleichzeitig die Kosten für die Kunden niedrig zu halten, indem keine übermäßige Hardwarekapazität vorgehalten wird. Sollte Ihre Anwendung einen Antwortcode vom Typ 429 (zu viele Anforderungen) erhalten, obwohl sich Ihre Arbeitsauslastung innerhalb der definierten Grenzwerte bewegt (siehe [Kurzübersicht über Kontingente und Grenzwerte](#quotas-and-limits-quick-reference)), liegt das wahrscheinlich daran, dass der Dienst hochskaliert wird, um Ihren Bedarf zu decken, und noch nicht die erforderliche Skalierung erreicht hat, weshalb nicht sofort genügend Ressourcen zur Verfügung stehen, um die Anforderung zu bewältigen. Dieser Zustand ist in der Regel vorübergehend und sollte nicht lange dauern.

### <a name="general-best-practices-to-mitigate-throttling-during-autoscaling"></a>Allgemeine bewährte Methoden zur Behandlung der Drosselung während der automatischen Skalierung
Verwenden Sie die folgenden Techniken, um Probleme im Zusammenhang mit der Drosselung (Antwortcode 429) zu vermeiden:
- Implementieren Sie eine Wiederholungslogik in der Anwendung.
- Vermeiden Sie plötzliche Änderungen bei der Arbeitsauslastung. Erhöhen Sie die Arbeitsauslastung nach und nach. <br/>
*Beispiel:* Ihre Anwendung verwendet Sprachsynthese, und die aktuelle Arbeitsauslastung liegt bei 5 TPS (Transaktionen pro Sekunde). In der nächsten Sekunde erhöhen Sie die Last auf 20 TPS (also um das Vierfache). Der Dienst beginnt sofort mit dem Hochskalieren, um die neue Last bewältigen zu können. Das Hochskalieren dauert jedoch wahrscheinlich länger als eine Sekunde, weshalb für einige Anforderungen der Antwortcode 429 zurückgegeben wird.
- Testen verschiedener Lasterhöhungsmuster
  - Siehe das [Beispiel für Workloadmuster](#example-of-a-workload-pattern-best-practice).
- Erstellen Sie zusätzliche Speech-Ressourcen in den gleichen oder in anderen Regionen, und verteilen Sie die Arbeitsauslastung mithilfe der Roundrobin-Technik darauf. Dies ist besonders wichtig für den Parameter für die **Sprachsynthese-TPS (Transaktionen pro Sekunde)** , der pro Speech-Ressource auf 200 festgelegt ist und nicht angepasst werden kann.

In den nächsten Abschnitten werden bestimmte Kontingentanpassungsfälle beschrieben.<br/>
Fahren Sie mit [Sprachsynthese: Erhöhen des Grenzwerts für gleichzeitige Anforderungen für benutzerdefinierte neuronale und benutzerdefinierte Stimmen](#text-to-speech-increasing-concurrent-request-limit-for-custom-neural-and-custom-voices) fort.

### <a name="speech-to-text-increasing-online-transcription-concurrent-request-limit"></a>Spracherkennung: Erhöhen des Grenzwerts für gleichzeitige Anforderungen bei der Onlinetranskription
Standardmäßig ist die Anzahl gleichzeitiger Anforderungen auf 100 pro Speech-Ressource (Basismodell) und auf 100 pro benutzerdefiniertem Endpunkt (benutzerdefiniertes Modell) beschränkt. Im Tarif „Standard“ kann diese Anzahl erhöht werden. Machen Sie sich vor dem Übermitteln der Anforderung mit dem Material in [diesem Abschnitt](#detailed-description-quota-adjustment-and-best-practices) und mit [diesen bewährten Methoden](#general-best-practices-to-mitigate-throttling-during-autoscaling) vertraut.

>[!NOTE]
> Beachten Sie bei der Verwendung von benutzerdefinierten Modelle, dass eine Speech-Ressource vielen benutzerdefinierten Endpunkten zugeordnet sein kann, die viele benutzerdefinierte Modellimplementierungen hosten. Für jeden benutzerdefinierten Endpunkt wird bei der Erstellung die Standardanzahl der Beschränkung für gleichzeitige Anforderungen (100) festgelegt. Wenn Sie diese anpassen möchten, müssen Sie die Anpassung für jeden benutzerdefinierten Endpunkt **separat** vornehmen. Beachten Sie auch, dass der Wert der Anzahl der Beschränkung für gleichzeitige Anforderungen für das Basismodell einer Speech-Ressource **keine** Auswirkungen auf die benutzerdefinierten Endpunkte hat, die dieser Ressource zugeordnet sind.


Eine Erhöhung des Grenzwerts für gleichzeitige Anforderungen wirkt sich **nicht** direkt auf Ihre Kosten aus. Bei den Speech-Diensten zahlen Sie nur für Ihre tatsächliche Nutzung. Der Grenzwert gibt an, wie hoch der Dienst skaliert werden kann, bevor Ihre Anforderungen gedrosselt werden.

Grenzwerte für gleichzeitige Anforderung in den Modellen **Basis** und **Benutzerdefiniert** müssen **separat** angepasst werden.

Der vorhandene Wert des Parameters für den Grenzwert für gleichzeitige Anforderungen wird im Azure-Portal, in Befehlszeilentools und in API-Anforderungen **nicht** angezeigt. Erstellen Sie eine Azure-Supportanfrage, um den vorhandenen Wert zu überprüfen.

>[!NOTE]
>Für [Speech-Container](speech-container-howto.md) muss der Grenzwert für gleichzeitige Anforderungen nicht erhöht werden, da Container einzig durch die CPUs der Hardware eingeschränkt sind, auf der sie gehostet werden. Speech-Container haben jedoch ihre eigenen Kapazitätsgrenzen, die berücksichtigt werden sollten. Weitere Informationen finden Sie unter der Frage *„Könnten Sie bei der Kapazitätsplanung und Kostenvorkalkulation von lokalen Spracherkennungscontainern helfen?“* in [Häufig gestellte Fragen zu Speech-Containern](./speech-container-howto.md).

#### <a name="have-the-required-information-ready"></a>Halten Sie die erforderlichen Informationen bereit:
- **Basismodell:**
  - ID der Speech-Ressource
  - Region
- **Benutzerdefiniertes Modell:**
  - Region
  - ID des benutzerdefinierten Endpunkts

- **Abrufen der Informationen (Basismodell):**
  - Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).
  - Wählen Sie die Speech-Ressource aus, für die Sie den Grenzwert für gleichzeitige Anforderungen erhöhen möchten.
  - Wählen Sie *Eigenschaften* (Gruppe *Ressourcenverwaltung*) aus.
  - Kopieren und speichern Sie die Werte der folgenden Felder:
    - **Ressourcen-ID**
    - **Standort** (Ihre Endpunktregion)

- **Abrufen der Informationen (benutzerdefiniertes Modell):**
  - Navigieren Sie zum Portal [Speech Studio](https://speech.microsoft.com/).
  - Melden Sie sich ggf. an.
  - Navigieren Sie zu „Custom Speech“.
  - Wählen Sie Ihr Projekt aus.
  - Navigieren Sie zu *Bereitstellung*.
  - Wählen Sie den erforderlichen Endpunkt aus.
  - Kopieren und speichern Sie die Werte der folgenden Felder:
    - **Dienstregion** (Ihre Endpunktregion)
    - **Endpunkt-ID**

#### <a name="create-and-submit-support-request"></a>Erstellen und Übermitteln der Supportanfrage
Übermitteln Sie die Supportanfrage, um für Ihre Ressource die Erhöhung des Grenzwerts für gleichzeitige Anforderungen zu initiieren oder um ggf. den aktuellen Grenzwert zu überprüfen:

- Vergewissern Sie sich, dass Sie über die [erforderlichen Informationen](#have-the-required-information-ready) verfügen.
- Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).
- Wählen Sie die Speech-Ressource aus, für die Sie den Grenzwert für gleichzeitige Anforderungen erhöhen (oder überprüfen) möchten.
- Wählen Sie *Neue Supportanfrage* (Gruppe *Support + Problembehandlung*) aus.
- Daraufhin wird ein neues Fenster mit automatisch ausgefüllten Informationen zum Azure-Abonnement und zur Azure-Ressource angezeigt.
- Geben Sie eine *Zusammenfassung* ein (beispielsweise „Increase STT Concurrency Request limit“ (Grenzwert für gleichzeitige STT-Anforderungen erhöhen)).
- Wählen Sie unter *Problemtyp* die Option für Kontingent- oder Abonnementprobleme aus.
- Wählen Sie unter *Problemuntertyp* Folgendes aus:
  - „Quota or concurrent requests increase“ (Erhöhung eines Kontingents oder der gleichzeitigen Anforderungen) für eine Erhöhungsanforderung
  - „Quota or usage validation“ (Kontingent- oder Nutzungsüberprüfung) zur Überprüfung des vorhandenen Grenzwerts
- Klicken Sie auf *Weiter: Lösungen*.
- Fahren Sie mit der Anforderungserstellung fort.
- Geben Sie auf der Registerkarte *Details* im Feld *Beschreibung* Folgendes ein:
  - Einen Hinweis darauf, dass es bei der Anfrage um ein Kontingent für die **Spracherkennung** geht
  - Das Modell (**Basis** oder **Benutzerdefiniert**)
  - [Zuvor ermittelte](#have-the-required-information-ready) Azure-Ressourceninformationen
  - Geben Sie die übrigen erforderlichen Informationen ein, und klicken Sie auf der Registerkarte *Überprüfen + erstellen* auf die Schaltfläche *Erstellen*.
  - Notieren Sie sich die Nummer der Supportanfrage aus den Benachrichtigungen im Azure-Portal. Sie werden in Kürze zur weiteren Bearbeitung kontaktiert.

### <a name="example-of-a-workload-pattern-best-practice"></a>Beispiel für eine bewährte Methode für Workloadmuster
Dieses Beispiel zeigt die empfohlene Vorgehensweise zur Vermeidung einer möglichen Drosselung [im Rahmen der automatischen Skalierung](#detailed-description-quota-adjustment-and-best-practices). Dabei handelt es sich nicht um eine exakte Anleitung, sondern vielmehr um eine empfohlene Vorlage, die Sie nach Bedarf anpassen können.

Angenommen, der Grenzwert für gleichzeitige Anforderungen einer Speech-Ressource ist auf „300“ festgelegt. Beginnen Sie mit 20 gleichzeitigen Verbindungen, und erhöhen Sie die Last alle 1,5 bis 2 Minuten um 20 gleichzeitige Verbindungen. Kontrollieren Sie die Dienstantworten, und implementieren Sie die Fallbacklogik, durch die im Falle zu vieler Antwortcodes vom Typ 429 die Last verringert wird. Versuchen Sie es dann erneut mit folgendem Minutenmuster: 1, 2, 4, 4. (Sprich: Versuchen Sie, die Last in einer Minute zu erhöhen. Sollte das nicht funktionieren, versuchen Sie es in zwei Minuten. Und so weiter.)

Es empfiehlt sich allgemein, die Arbeitsauslastung und die Arbeitsauslastungsmuster zu testen, bevor sie in der Produktion verwendet werden.

### <a name="text-to-speech-increasing-concurrent-request-limit-for-custom-neural-and-custom-voices"></a>Sprachsynthese: Erhöhen des Grenzwerts für gleichzeitige Anforderungen für benutzerdefinierte neuronale und benutzerdefinierte Stimmen
Standardmäßig ist die Anzahl gleichzeitiger Anforderungen für Endpunkte für benutzerdefinierte neuronale Stimmen und benutzerdefinierte Stimmen auf 10 begrenzt. Im Tarif „Standard“ kann diese Anzahl erhöht werden. Machen Sie sich vor dem Übermitteln der Anforderung mit dem Material in [diesem Abschnitt](#detailed-description-quota-adjustment-and-best-practices) und mit [diesen bewährten Methoden](#general-best-practices-to-mitigate-throttling-during-autoscaling) vertraut.

Eine Erhöhung des Grenzwerts für gleichzeitige Anforderungen wirkt sich **nicht** direkt auf Ihre Kosten aus. Bei den Speech-Diensten zahlen Sie nur für Ihre tatsächliche Nutzung. Der Grenzwert gibt an, wie hoch der Dienst skaliert werden kann, bevor Ihre Anforderungen gedrosselt werden.

Der vorhandene Wert des Parameters für den Grenzwert für gleichzeitige Anforderungen wird im Azure-Portal, in Befehlszeilentools und in API-Anforderungen **nicht** angezeigt. Erstellen Sie eine Azure-Supportanfrage, um den vorhandenen Wert zu überprüfen.

>[!NOTE]
>Für [Speech-Container](speech-container-howto.md) muss der Grenzwert für gleichzeitige Anforderungen nicht erhöht werden, da Container einzig durch die CPUs der Hardware eingeschränkt sind, auf der sie gehostet werden.

#### <a name="prepare-the-required-information"></a>Bereiten Sie die erforderlichen Informationen vor:
Wenn Sie eine Erhöhungsanforderung erstellen möchten, müssen Sie Ihre Bereitstellungsregion und die IDE des benutzerdefinierten Endpunkts angeben. Diese Informationen erhalten Sie wie folgt:

- Navigieren Sie zum Portal [Speech Studio](https://speech.microsoft.com/).
- Melden Sie sich ggf. an.
- Navigieren Sie zu *Custom Voice*.
- Wählen Sie Ihr Projekt aus.
- Navigieren Sie zu *Bereitstellung*.
- Wählen Sie den erforderlichen Endpunkt aus.
- Kopieren und speichern Sie die Werte der folgenden Felder:
    - **Dienstregion** (Ihre Endpunktregion)
    - **Endpunkt-ID**

#### <a name="create-and-submit-support-request"></a>Erstellen und Übermitteln der Supportanfrage
Übermitteln Sie die Supportanfrage, um für Ihre Ressource die Erhöhung des Grenzwerts für gleichzeitige Anforderungen zu initiieren oder um ggf. den aktuellen Grenzwert zu überprüfen:

- Vergewissern Sie sich, dass Sie über die [erforderlichen Informationen](#prepare-the-required-information) verfügen.
- Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).
- Wählen Sie die Speech-Ressource aus, für die Sie den Grenzwert für gleichzeitige Anforderungen erhöhen (oder überprüfen) möchten.
- Wählen Sie *Neue Supportanfrage* (Gruppe *Support + Problembehandlung*) aus.
- Daraufhin wird ein neues Fenster mit automatisch ausgefüllten Informationen zum Azure-Abonnement und zur Azure-Ressource angezeigt.
- Geben Sie eine *Zusammenfassung* ein (beispielsweise „Increase TTS Custom Endpoint Concurrency Request limit“ (Grenzwert für gleichzeitige TTS-Anforderungen für den benutzerdefinierten Endpunkt erhöhen)).
- Wählen Sie unter *Problemtyp* die Option für Kontingent- oder Abonnementprobleme aus.
- Wählen Sie unter *Problemuntertyp* Folgendes aus:
  - „Quota or concurrent requests increase“ (Erhöhung eines Kontingents oder der gleichzeitigen Anforderungen) für eine Erhöhungsanforderung
  - „Quota or usage validation“ (Kontingent- oder Nutzungsüberprüfung) zur Überprüfung des vorhandenen Grenzwerts
- Klicken Sie auf *Weiter: Lösungen*.
- Fahren Sie mit der Anforderungserstellung fort.
- Geben Sie auf der Registerkarte *Details* im Feld *Beschreibung* Folgendes ein:
  - Einen Hinweis darauf, dass es bei der Anfrage um ein Kontingent für die **Sprachsynthese** geht
  - [Zuvor ermittelte](#prepare-the-required-information) Azure-Ressourceninformationen
  - Geben Sie die übrigen erforderlichen Informationen ein, und klicken Sie auf der Registerkarte *Überprüfen + erstellen* auf die Schaltfläche *Erstellen*.
  - Notieren Sie sich die Nummer der Supportanfrage aus den Benachrichtigungen im Azure-Portal. Sie werden in Kürze zur weiteren Bearbeitung kontaktiert.
