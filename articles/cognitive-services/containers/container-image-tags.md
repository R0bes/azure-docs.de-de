---
title: Cognitive Services-Containerimagetags
titleSuffix: Azure Cognitive Services
description: Eine umfassende Liste aller Cognitive Services-Containerimagetags.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 06/25/2021
ms.author: aahi
ms.openlocfilehash: bad25e569077a7503be64fffc85501a2550db96c
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355822"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure Cognitive Services-Containerimagetags und -Versionshinweise

Azure Cognitive Services bietet viele Containerimages. Die Containerregistrierungen und die entsprechenden Repositorys unterscheiden sich je nach Containerimage. Jeder Containerimagename bietet mehrere Tags. Ein Containerimagetag ist ein Mechanismus zur Versionsverwaltung für Containerimages. Dieser Artikel ist als umfassende Referenz für die Auflistung aller Cognitive Services-Containerimages und ihrer verfügbaren Tags gedacht.

> [!TIP]
> Achten Sie bei der Verwendung von [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) besonders auf die Groß-/Kleinschreibung der Containerregistrierung, des Repositorys, des Containerimagenamens und des entsprechenden Tags, da bei ihnen die **Groß-/Kleinschreibung berücksichtigt** wird.

## <a name="anomaly-detector"></a>Anomalieerkennung

Das Containerimage für die [Anomalieerkennung][ad-containers] finden Sie im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/decision` und trägt den Namen `anomaly-detector`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Read OCR (Optical Character Recognition; optische Zeichenerkennung)

Das Containerimage [Maschinelles Sehen][cv-containers] für das Lesen von OCR befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `read`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/vision/read`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `3.2`:

* Der OCR-Container für das Lesen ist jetzt allgemein verfügbar.

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `3.2`                     |       |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)


Versionshinweise für `3.2-preview.2`:

* Version ohne Distribution
* ReadingOrder-Parameter zur Auswahl der Reihenfolge der Textzeilen in der JSON-Antwort
* Verbesserte Protokollierung
* Hotfixes für das CJK-Modell
* 
Versionshinweise für `v2.0.013250001-amd64-preview`:

* Verringern Sie die Speicherauslastung für den Container weiter.
* Für das Einrichten von mehreren Pods ist externer Cache erforderlich. Richten Sie z. B. Redis als Cache ein.
* Fehlender Ergebnisse, wenn Redis-Cache eingerichtet und `ResultExpirationPeriod` auf 0 festgelegt ist, wurden behoben.
* Heben Sie die Größenbeschränkung für den Anforderungstext von 26 MB auf. Der Container kann jetzt Dateien akzeptieren, die größer als 26 MB sind.
* Fügen Sie der Konsolenprotokollierung Zeitstempel und Buildversion hinzu.

Versionshinweise für `1.1.013050001-amd64-preview`

* Die Konfiguration `ReadEngineConfig:ResultExpirationPeriod` für die Containerinitialisierung wurde hinzugefügt, um anzugeben, wann das System Erkennungsergebnisse bereinigen soll. 
    * Die Einstellung wird in Stunden angegeben, und der Standardwert ist 48 Stunden.
    * Durch die Einstellung kann der Speicherverbrauch für die Speicherung der Ergebnisse reduziert werden. Dies gilt insbesondere dann, wenn für den Container ein In-Memory-Speicher verwendet wird.
    * Beispiel 1: ReadEngineConfig:ResultExpirationPeriod=1: Das System löscht das Erkennungsergebnis 1 Stunde nach dem Prozess.
    * Wird diese Konfiguration auf 0 festgelegt, löscht das System das Erkennungsergebnis nach dem Abrufen des Ergebnisses.

* Es wurde ein interner Serverfehler mit dem Code 500 korrigiert, wenn ein ungültiges Imageformat an das System übermittelt wurde. Nun wird ein 400-Fehler zurückgegeben:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `3.2.2.014850001-49e0eac6-amd64-preview` |       |
| `2.0.013250001-amd64-preview` |       |
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---

## <a name="form-recognizer"></a>Formularerkennung

Die Features der Formularerkennung werden von sieben Containern unterstützt:

| Containername | Vollqualifizierter Imagename |
|---|---|
| **Layout** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/layout |
| **Visitenkarte** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/businesscard |
| **Ausweisdokument** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/id-document |
| **Rechnung** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/receipt |
| **Rechnung** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/invoice |
| **Benutzerdefinierte API** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/custom-api |
| **Benutzerdefiniert überwacht** | mcr.microsoft.com/azure-cognitive-services/form-recognizer/custom-supervised |

Containerimages vom Typ [Formularerkennung][fr-containers] befinden sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Sie sind im Repository `azure-cognitive-services/form-recognizer` enthalten.

Für Containerimages sind die folgenden Tags verfügbar:

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `v2.1`:

Container für die Formularerkennung befinden sich derzeit in der geschlossenen Vorschauphase. Wenn Sie die Container verwenden möchten, müssen Sie eine [Onlineanforderung](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUNlpBU1lFSjJUMFhKNzVHUUVLN1NIOEZETiQlQCN0PWcu) einreichen, um eine entsprechende Genehmigung zu erhalten.

| Container | Tags |
|------------|:------|
| **Layout**| &bullet; `latest` </br> &bullet; `2.1-preview` </br> &bullet; `2.1.0.016140001-08108749-amd64-preview`|
| **Visitenkarte** | &bullet; `latest` </br> &bullet; `2.1-preview` </br> &bullet; `2.1.016190001-amd64-preview`  </br> &bullet; `2.1.016320001-amd64-preview`  |
| **Ausweisdokument** | &bullet; `latest` </br> &bullet; `2.1-preview`</br>&bullet; `2.1.016190001-amd64-preview`</br>&bullet; `2.1.016320001-amd64-preview` |
| **Rechnung**| &bullet; `latest` </br> &bullet; `2.1-preview`</br>&bullet; `2.1.016190001-amd64-preview`</br>&bullet; `2.1.016320001-amd64-preview` |
| **Rechnung**| &bullet; `latest` </br> &bullet; `2.1-preview`</br>&bullet; `2.1.016190001-amd64-preview`</br>&bullet; `2.1.016320001-amd64-preview` |
| **Benutzerdefinierte API** | &bullet; `latest` </br> &bullet;`2.1-distroless-20210622013115034-0cc5fcf6`</br>&bullet; `2.1-preview`|
| **Benutzerdefiniert überwacht**| &bullet; `latest` </br> &bullet; `2.1-distroless-20210622013149174-0cc5fcf6`</br>&bullet; `2.1-preview`|

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

> [!IMPORTANT]
> Der Formularerkennungscontainer v1.0 wurde eingestellt.

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

Das Containerimage [LUIS][lu-containers] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/language` und trägt den Namen `luis`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/language/luis`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Benutzerdefinierte Spracherkennung

Das Containerimage [Benutzerdefinierte Spracherkennung][sp-cstt] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `custom-speech-to-text`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `2.13.0-amd64`:

Regelmäßiges monatliches Release

Beachten Sie, dass dieses Containerimage aufgrund des Ausdruckslistenfeatures größer geworden ist.

| Imagetags                    | Notizen | Digest                                                                   |
|-------------------------------|:------|:-------------------------------------------------------------------------|
| `latest`                      |       | `sha256:55ff552d0c593a4ddbed0ae0dede758f93011a165f1afd6738ba906a7e24eeee`|
| `2.13.0-amd64`                |       | `sha256:55ff552d0c593a4ddbed0ae0dede758f93011a165f1afd6738ba906a7e24eeee`|


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `2.12.1-amd64`:

Regelmäßiges monatliches Release

Versionshinweise für `2.11.0-amd64`:

**Fehlerbehebungen**
* Bei Benutzereingaben wird die Groß-/Kleinschreibung beibehalten.

Versionshinweise für `2.10.0-amd64`:

Regelmäßiges monatliches Release

Versionshinweise für `2.9.0-amd64`:

**Feature**
* Weitere Fehlerdetails zu Problemen beim Abrufen von benutzerdefinierten Modellen nach ID.
* Die Hypothese wird in den Konversationsergebnissen standardmäßig unterstützt.

Versionshinweise für `2.7.0-amd64`:

**Funktionen**
* Interpunktion ist standardmäßig aktiviert.

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist.

Versionshinweise für `2.6.0-amd64`:

**Funktionen**
* Unterstützung für PhraseList v2 
* Ausdruckslisten werden in folgenden Gebietsschemas unterstützt:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * de-de
    * zh-cn
* Unterstützung für das Herunterladen benutzerdefinierter Basismodelle. 
    * Verwenden Sie `BaseModelLocale=<locale>` mit dem Befehl `docker run`.
* Vollständig migriert zu .NET 3.1

**Fehlerbehebungen**
* Ein Problem wurde behoben, bei dem der Konfidenzscore im Diarisierungsmodus immer 1 war.
* Migriert zur TextAnalytics 3.0-API

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist.

Versionshinweise für `2.5.0-amd64`:

**Funktionen**
* Unterstützung für benutzerdefinierte Aussprache bei benutzerdefinierten Modellen
* Unterstützung für Azure und Azure US Government Cloud

**Fehlerbehebungen**
* Behebung des Problems mit „Ausführen als Nicht-Root-Benutzer“ im Diarisierungsmodus

| Imagetags                    | Notizen               |
|-------------------------------|:--------------------|
| `2.12.1-amd64`                |                     |
| `2.11.0-amd64`                |                     |
| `2.10.0-amd64`                |                     |
| `2.9.0-amd64`                 |                     |
| `2.7.0-amd64`                 |                     |
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   1\. allgemein verfügbare Version    |

---

## <a name="custom-text-to-speech"></a>Benutzerdefinierte Sprachsynthese

Das Containerimage [Benutzerdefinierte Sprachsynthese][sp-ctts] befindet sich im Containerregistrierungssyndikat der `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `custom-text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `1.14.1-amd64`:

Regelmäßiges monatliches Release

| Imagetags                    | Notizen | Digest                                                                    |
|-------------------------------|:------|:--------------------------------------------------------------------------|
| `latest`                      |       | `sha256:1db1eea50b96fd56cf4e63ff22878a8da1130f8bfa497c9ce70fbe9db40e3d2c` |
| `1.14.1-amd64`                |       | `sha256:1db1eea50b96fd56cf4e63ff22878a8da1130f8bfa497c9ce70fbe9db40e3d2c` |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `1.13.0-amd64`:

**Fehlerbehebungen**
* Bei Benutzereingaben wird die Groß-/Kleinschreibung beibehalten.

Versionshinweise für `1.12.0-amd64`:

Regelmäßiges monatliches Release

Versionshinweise für `1.11.0-amd64`:

**Feature**
* Weitere Fehlerdetails zu Problemen beim Abrufen von benutzerdefinierten Modellen nach ID.

Versionshinweise für `1.9.0-amd64`:

Regelmäßiges monatliches Release

Versionshinweise für `1.8.0-amd64`:

**Funktionen**
* Vollständig migriert zu .NET 3.1

Versionshinweise für `1.7.0-amd64`:

**Feature**
* Teilweise migriert zu .NET 3.1

| Imagetags                    | Notizen               |
|-------------------------------|:--------------------|
| `1.13.0-amd64`                |                     |
| `1.12.0-amd64`                |                     |
| `1.11.0-amd64`                |                     |
| `1.9.0-amd64`                 |                     |
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   1\. allgemein verfügbare Version    |

---

## <a name="speech-to-text"></a>Spracherkennung

Das Containerimage [Spracherkennung][sp-stt] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `speech-to-text`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list).

Seit Spracherkennung v2.5.0 werden Images in der Region *US Government Virginia* unterstützt. Verwenden Sie den Abrechnungsendpunkt sowie die API-Schlüssel der Region *US Government Virginia*, wenn Sie diese Region verwenden.

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `2.13.0-amd64-<locale>`:

Regelmäßiges monatliches Release

Beachten Sie, dass dieses Containerimage aufgrund des Ausdruckslistenfeatures größer geworden ist. 

| Imagetags                    | Notizen                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Containerimage mit dem Gebietsschema `en-US`.                                                             |
| `2.13.0-amd64-<locale>`       | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.13.0-amd64-en-us`.|

Für diesen Container sind die folgenden Gebietsschemas verfügbar.

| Gebietsschema für v2.13.0          | Notizen                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:9114c6885513cc3ae8d3c9393d3f4f334bb68ff9e444734951f469f8d56fb41c` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:924dc807076633f4e04f1f604c3db63d908a484c69459bf593d72b58d901cd43` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:13387db275daf6375e12ce1da5b858493ab71b249a3759e438345ac32119c6b2` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:2e8bea90f7a106a94e36d9c90e767c58cd8004a61880af53bd4ffb4292a655fe` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:23c8529ee0e91fee549523021711a755da4c249f21493a1864a64941b36e2986` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:9114c6885513cc3ae8d3c9393d3f4f334bb68ff9e444734951f469f8d56fb41c` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:70bbb43641f22e96e70d3b5723b2599dd83533f33d979ff9dfb04a627799f4d1` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:f6fc1c1bcb7d20f2daa30506a039d16ad0537a60c01e41b399159704a001fe42` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:9114c6885513cc3ae8d3c9393d3f4f334bb68ff9e444734951f469f8d56fb41c` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:9114c6885513cc3ae8d3c9393d3f4f334bb68ff9e444734951f469f8d56fb41c` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:218c1f57623b81770c22c7f871bce58a3227ef5fcbe7581e18a69f77107b5c96` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:9537460403216802831fa02a6eb3bf7a3f6e1e6669953ab4ae9c98ea6283799a` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:94f68e496546eb3c33cf07b7f88807fa23c3f9d5022c2e630b589e29951f0538` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:10de908ebf603c6b3a2a937edc870d5fe1c4dc6bc9bb7e1f0eca9b9ed2b19a88` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:cf03effc2a616b8fea8eacf7d45728cd00b9948f4f3e55d692db0125c51881da` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:9c9a51d595253c54811ba8d7502799b638f6332c0524fca2543f20efb76c7337` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:6bb17c45a291f6293970a4de7bfdc9e31fdffedf80e76f66bca3cab118f76252` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:1e58c2e2416208b658d18fc4bf6374d6032710ff29c09f125c6d19a4d6609e92` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:f0c4da3aa11f9eb72adbc7eab0c18047eec5016ec8c2fec2f1132ddceb3b6f3a` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:4d0917974effee44ebf1721e9c0d9a3a2ab957613ce3862fe99062add5d5d08a` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:b72a01b0cfaa97ea6102b48acb0a546501bb63618ee4ec9b892bdbdc6fd7ce8c` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:d26f56f1f4c41b1c035eb47950cb5bc6bd86cbe07ef08c2276275a46ac4c4ad4` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:0ad933b9b3626d21d8ac0320f7fb4c72bcf6767258e39ac57698ce0269ed7750` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:d6f9344f7cf0b827b63fb91c31e490546732e8a6c93080e925cd922458ae3695` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:dbd1fe80e1801b5fa7e468365f469c1b5770b0f27f2e5afb90c25a74702a0a21` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:f234725e54af7bda1c6baa7e9f907b703a85118d65249ca0c050c52109397cc6` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:88dd53d975829707f6ef91ad91aec9ed5fd12df8f4ef33e8c3bdf4701eaaca84` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:502693715b8b666a9c10084c733848f95201e9882f9bfae7df770bd9dc8bb983` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:6aa4f300639f7ee958adced5e7e5867e7f4d4093f2ca953f3ee5da9128bf08f6` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:60f01882b393e00743c61c783e98c1cdcf73097c555999f10e5612b06b5afa90` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:7b58b3a823c0fff1b92e46dd848610f2c9dcae5be0463845292e810d3efa1b1b` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:c51291acc65e1a839477f9bdbd042e4c81d2e638f48a00b6ca423023c9fd6c2c` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:085b3bf2869fcedb56745e6adc98f2a332d57d0b1ac66cc219cec436a884d7d5` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:43e5425cab3f708ed8632152514f4152f45a19953758fb7b5ebe9f4a767bcfdb` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:249f3165e0347b223ff06e34c309a753965a3df55bda2a78e04d86c946205d06` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:624eeed264f25bab59a7723c6e6c3ae760bc63c46ebe3bcd3db171220682c14d` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:6d2d41e3b78ebba9d5d46fc8bddb90d0d69680a904774f5da1fa01eb4efd68e1` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:ce4b4b761d1a2ca2b657b877c46a341a83f0b1a46447007262c051f6785b7312` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:d4ecebce65a18763ac1126bf83706e49ebed80b79255e3820a68e97037d2a501` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:c3088a60818b85cd0f04445837ea0ddcb6e7ac4f77269471717002166195d6d2` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:1d88e66f6fd86ddf6e47596d2e2b9b3fe64ea7e72f6c4c965d3f1c5b98592e1b` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:bb07eb832bcd23f302f0a7b6c4e87bf33186a47ed154ac8b42a1f6dea0f35432` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:b726f92daf85c8aa6b169767efdb2af1691ddb7b21b8af3e9afcb984f41d8539` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:660a5f9e13d62a963c9c92219f8268ad7f7af5ed08890534679e143cff184004` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:cb708bc008a59ac35e292094eba912af741c49eb7e67c2df3c1023ab41a6d454` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:acd788410f8f6f8c269c85e6c70365e751a92976d61b34b7435766c0ae2fd11a` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:f7ef486a64a413f7d69510f25a39ddce9653265852da1b3cc438000f1bbfa368` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:7f6975423cbcf201e318bea9865e93a8e4a6a241b472845d90a877400470338b` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:e2f498c4a19f88779dfae350e0cefb4f0aa1c518c18f43139d4bec6a4f655f45` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:66ec075ea26141d73e07a223f72f10ea8237d0d9675e67d569f026ca6125cd95` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:34b4ee60880d310aa08f1584c2f8d1a9a0236ac0067b9d8ad8bf5057749f2d9b` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:709bc27ebd387cc18d3d16136280234f64c4ba28f05383a52e0bbe066574105a` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:cfd3140a3c7a5234c0273e34b9b124897cff6c2d11403217096616dd34c14e38` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:f03b3407772d4a5be1642ff0f78c64283c2e8fd9b473f8bab90864a59d4f8a4a` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:c67190092fcf7af406406e5906d9de79a8fb37565e84b2dc0786caee0b5b27e2` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:eea6f9608d9802ac43e755de39d87e95e708d5c642f58de09863363051112540` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:3943c40ef4696c44887d08a1cb911f535af451b811737b0101a4fa0ef4284d68` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:52eb41ca6694497356cb23bd02daf4bb2408ffad418696aeb1bdf1f03c2e2845` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:70aa2b907f114278d839a958dea29c74b64cd1f7a5a0406194d2aa3583c12048` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:14e222688387847f51fd858c5575e554046796090e41f072d6200d89f5608e4a` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:8f3ed7b3896b205b5690e5515a5511581715e698cd6fe0704c153d35a4c9af80` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:806572a1ae31575806062301d22233b753c415388184496ee67589ddbc264d49` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:780444acc9be4514072926146c36b7ccce003f27577b339cf431fec2ca6d79f5` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:75460753cba8d45babaf859f94dfd1a1c75b312a841eacded099680dc77c2f89` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:8d92a5f26100d309a11f05ce13e5e5a0f2bbc072df917af158cc251dc75a4d4f` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:d9c75c885591ced0e10cca5594ae5cf92cb1dde73306f8454737b7927aada89a` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:15cc274d238cae2a1d9cabc3e5a71e4ba90ae6318fea63937c8830bd55da0fc2` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:a45730afdc6d15060eff8526e1be08f679b25a2be26156d39266a40e6cd82bc9` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:8f578440ae5c9cd81eee18f68c677bb56ced7c6a6a217d98da60dc856fd2e002` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:99fedeb4acc49fd3185d34532b1a7321931b17f2eda16ab8643312dbf8afcf38` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:7677c49b2426fb26eff59a97a012d5890aa7fdbc09684ef0fb29fdbe63fac333` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:452d269e8e12ae1379d4568bc1b15fefdd3679903365adb3a68bc6669c738615` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:e6fd994a344b5452b4a5b90a499fed0681dd6ef2fab3db161d407cf4f45ff5dd` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:4df5fdc9732c07d479275561522ce34a38c3864098a56e12ec8329e40f4e6f2a` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:49180ac0eccee59a22800f4c1ae870e3a71543e46d2986fc82ec9b77c7de1ea0` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:a0c64efbf2d9d0a111efc79cc7b70e06ac01745de57d9c768f99c54ac5642cee` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:8811c30c10980a3ddf441f1d4e21240bfb8663af6200c2d666fdeb83f48a79c5` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:99860f484f52d9665f33d95659daa8aec5071fa5a97534d40ee4941690ce3e96` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:170b56107ccb22335422c1838e368c0f5cb4518c3309e6259b754ede9e46ff51` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:d8721f303ca0b24705c42e8c0f5d20dcafb3d00b278b7c363d1a4c129f5e2cbd` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:12af9f057acec8231dcdeb1e4037ac53a95957796b5e8dbf48f55db6970a4431` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:b2c1d333b7718c9cc2708287e388c45abcd28a3e8d7fc3c758cc4b73d2697662` |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `2.12.1-amd64-<locale>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

Versionshinweise für `2.11.0-amd64-<locale>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

**Fehlerbehebungen**
* Bei Benutzereingaben wird die Groß-/Kleinschreibung beibehalten.

Versionshinweise für `2.10.0-amd64-<locale>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

Versionshinweise für `2.9.0-amd64-<locale>`:

**Feature**
* Weitere Fehlerdetails zu Problemen beim Abrufen von benutzerdefinierten Modellen nach ID.
* Die Hypothese wird in den Konversationsergebnissen standardmäßig unterstützt.

Versionshinweise für `2.7.0-amd64-<locale>`:

**Funktionen**
* Unterstützung für die folgenden neuen Gebietsschemas:
    * ar-bh, ar-iq, ar-jo, ar-lb, ar-om, ar-sy
    * bg-BG
    * el-GR
    * en-hk, en-ie, en-ph, en-sg, en-za
    * es-ar, es-bo, es-cl, es-co, es-cr, es-cu, es-do, es-ec, es-gt, es-pa, es-pe, es-pr, es-py, es-sv, es-us, es-uy, es-ve
    * et-EE
    * ga-IE
    * hr-HR
    * hu-hu
    * lt-LT
    * lv-LV
    * mt-MT
    * ro-RO
    * sk-SK
    * sl-SL
* Interpunktion ist standardmäßig aktiviert.

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist. 

Versionshinweise für `2.6.0-amd64-<locale>`:

**Funktionen**
* Upgrade auf die neuesten Modelle erfolgt und vollständig migriert zu .NET 3.1
* Unterstützung für PhraseList v2
* Ausdruckslisten werden in folgenden Gebietsschemas unterstützt:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * de-de
    * zh-cn
* Unterstützung für das neue Gebietsschema `cs-CZ` 
    * Groß-/Kleinschreibung und Satzzeichen werden zurzeit nicht unterstützt.

**Fehlerbehebungen**
* Ein Problem wurde behoben, bei dem der Konfidenzscore im Diarisierungsmodus immer 1 war.
* Migriert zur TextAnalytics 3.0-API

Beachten Sie, dass dieses Containerimage aufgrund der enthaltenen Ausdruckslisten größer geworden ist. 

Versionshinweise für `2.5.0-amd64-<locale>`:

**Funktionen**
* Unterstützung für Azure US Government Cloud

**Fehlerbehebungen**
* Behebt ein Problem mit „Ausführen als Nicht-Root-Benutzer“ im Diarisierungsmodus

| Imagetags                  | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `2.12.1-amd64-<locale>`     | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.12.1-amd64-en-us`.|
| `2.11.0-amd64-<locale>`     | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.11.0-amd64-en-us`.|
| `2.10.0-amd64-<locale>`     | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.10.0-amd64-en-us`.|
| `2.9.0-amd64-<locale>`      | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.9.0-amd64-en-us`. |
| `2.7.0-amd64-<locale>`      | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.7.0-amd64-en-us`. |
| `2.6.0-amd64-<locale>`      | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.6.0-amd64-en-us`. |
| `2.5.0-amd64-<locale>`      | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `2.5.0-amd64-en-us`. |


Für diesen Container sind die folgenden Gebietsschemas verfügbar.

| Gebietsschema für v2.12.1          | Notizen                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:070b6f390dbe7b81b72845c1c9c83087979e1e330d84d417f39a371298a4d270` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:2b67e2a2a3ba79e52c5de4b2af7f3d3db565466d9a55d5f9d7501f349f42b49d` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:71cccd4dc4938397ea5b065fb32ab7347350c834edb036805362ca28e7cfec94` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:a9000def8d9c634af244384442c2723ad887c79e7f80a767bf7fcf3638a9deac` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:b8be9222b3e1bc40ba86c41e707f239d9ae23bc87d90b800615c314a443d947f` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:070b6f390dbe7b81b72845c1c9c83087979e1e330d84d417f39a371298a4d270` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:d41dbc9e93ae524abb95d2adde53924a32956ab1ec14a115916e5e531b3f3624` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:3071d896f82d062e126331e3162d5408eb399aeda3041be2336f81bed0634e5e` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:070b6f390dbe7b81b72845c1c9c83087979e1e330d84d417f39a371298a4d270` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:070b6f390dbe7b81b72845c1c9c83087979e1e330d84d417f39a371298a4d270` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:d7207eb391d0455ae112b61bc2c22280618131ad9591324bcde7e5057777fc26` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:c5c9639b9e09e07f6d8733017d30beed3aad54fa91c69c72526d34aa27ead884` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:dc6b7697099cd966aa4c8ba0b192ccb286b4241a76b12dbf494a9de319191334` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:ded8e56b863567e73b92cba4b7abeaf3f8c9ae335280a9645961d683ebfe8f9f` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:d3fc39e0d0454609bde5f6df67d7ade199f5361559ce11f097e97fca312d78b7` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:bbd8ede305ec5b551cdfac857507a1d05c3ca95119e431f0f43fe073d830f8fd` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:e4f39db7de5fb8106237f73adb2fbb229a7b8cb21291e593a346f928af87503f` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:186731d8479923a9dce053aee78f1347cd512471ead33802faef19bfa4e94883` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:04ede5a65eaf6f1d7a36d97056468b024b1577e3cf3a2cdafcd511d1de64f9d8` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:ef48d6889daec88405e7a86b3851df449066da8f0f62404260eabe68081e9b32` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:7d66fb960d55822c648919557d8e921c570dbbe36b165621f2bd5081df3c51c1` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:4285ff1d4b2231bc112a50c22072dabb303240ce18aeeab7183da3a572298a6a` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:b32b94f8a2bf56e0fa2cf63f885e9456b430411038ce2ebef6abd45159787ef6` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:c2162d7524bafd554fea81f2b3d95f3848ff0bf4ec0c4bd9d9bc4f2eae75ca27` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:e55f7d21d3b9d230bba78b41eb2418abacb7e6d832a0ec350ab86f98420260ce` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:d0d3d6d266f05cdedcaf75949ece66492e2e37b15684a80d08de3494381a5d10` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:b708d553eeb22958563c24fef18edc67f89d1b4ea0ff31a66ea34c624fcec878` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:5a5ad9afb9f0935ec9ffd5a1034bed186c46d2f9ea82ab485f949695ca4c2b61` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:c0f4dde13c319b4fd75b6b8615fc68aacd22ac04cf8b605d8d62486a08851d2d` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:af5f1435cd3e58ee9e98d8623a071dd72f30bf9ddbd90e1a61f06677ff34c0d3` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:ba42ed9a8c102b1af53873fc0d9ccd288723be3f5a409bf1480363381f8127fa` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:08292bac0b6d97c5ba3cb2b277c53289235216c124c72ce74c0a2d734860c777` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:e245443a75fdcdd8c10463a45a80d716d36cf336dfb23948f17d50939f65e919` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:d5d853b26104f2b9b7bf48a89dfe8a19f72c5d689eb474d68c8234c8b297dbf0` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:9a503a29fdf52a973c0e9339ac8b4f52442e7130c340ca7e12c8a38df004c8a1` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:661726852daeb5d1d839c05e95c0a683e9722564356089bd4023edfbf83076ae` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:3c55158c8e030fbad2f090b587cbd6501303128af77ff0bddc8819e6a9a88e62` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:31ea64c3cf1d442b5182d664a16afd81ac402ab8a0c2434e642317f20c920be4` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:1ed31bb1cd484fc23b177c355ef65c12dc2b937c113b2b175f8b383e9390ca86` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:0c979930fa983fd76f6d3610b2d9c1018eaefe456b8b5d07f5ff90d605bebc9e` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:7bb685a97e64130caaea382d1b33b57ffb4dbeb16881f421ed212f81f0d46de2` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:4da6a791737e136e494753666c7a40518e147c7bd225461165714510c19a44c6` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:62b41c8003fcc17f5aef9729cfcbbdf81990e1ba2bc4ddefcd947ce3374f5794` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:eb396527bd28bfbd4a5d70ea29775b8352f3490d159b3ceeb32b442058817e12` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:a70f0196b552934d35b165059b28f192f97f83d451ae08ec0d267ab8a3c6adf5` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:361588561ed3ade02926e9db88ae1a9455fd76e6370ad794638d794129aa0036` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:120b28f629f4825e7b7f52f28f535f6c1bf2f8139c8288867a4bf491fc155a4e` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:ebc2b82704cb4d1be4d3dcfad933978ceb3daa8077cf6cadf560d8c33d6f4334` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:33931d7b35f8e7a05822aa7052fb89e8de3124311e70ff567a7f9ca158223f27` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:cd0a9c661b4645763d73a947e933b9d4e817485f4b9d6d0ac173195693a29f33` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:06e90396c307396ef395c23efc3157f75c207f230fb048d73ece407edd24c7b4` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:d9be6bca9c3abf839796d8f89bf43d2646080150057f6eb343c66042bc98ccfc` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:1c3ffb5730c401124edbb7b347569ca3bebd33412a24b32802f4d41401e911dc` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:218d319b2835da7b09ab4536e5d8301ede2bcd3bc023606d05d7294c534982cf` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:dea03196c1ad06cb1bf9914b5c5d1a631aafbaa5bd74a4d53d08dec982f545fe` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:5f33b06d0f77fd3c5d351284b2aff41681927cfa7fbda00ead338f7bd54f6575` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:2b7e558abb94a74e6b5a7f467289ba5cb32970967cd7409db2c150290ed9844d` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:05619049274edcd572d1ac6fabf11e0bdd2e95a9145e99065f46d2f26a2dc960` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:75253c7bb0ef67b50767593e36129dc98c8d9de60a31b2a7069d07a0cb6b6400` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:46bce0ab6a09f0837a4f884e29a69d38591e513e157d334fd39a2c6f1f08bb06` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:747bfeb07d354b848f7ffbd292c16befc00586d62b958fbb42f8b497a0dec87c` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:eab3cf2323ec4d86b923693595e16724dd6090d60a1a93a9d65f73c55b684448` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:1c5085250bcdbde6b619594b2f920c307b3d97672f01f03608618bd52a4374a7` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:b35274995b93587b8957101e8139598011d760df1f4c36f966114a4352b865cf` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:59b5088fef6b8ba41eed98dd738159e914c292ce790a3b8a934aa0ac6c161cca` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:a0074f10622c8ccc7d288cfa131786a02fe2c98e2cbe22caa0d07690c436f8b3` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:a6fc1ad6ea87c5f6282f3d10f724358e30f0f05c91084d52fd665e356bd6119b` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:9fc1363f4466d4e0bba3f2fb74efc54ff24fe43a55fe7703aa75da2b42e563c3` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:e3ec228d0eb76f91cd1fe723607eb0b96b9e1dc8874c40d1307f2b3585ab1912` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:9e6bdf31a80cb8a97b495ce39144d4957d9608e541aae9be6c5c35456476d4af` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:240baf152c419caeee33c7f18285d930af15d14ce784967305accf6541722a22` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:53eff9f8eb08bba90efb30d8fdb2c9760bb0d8ae60cda967b72f0433ae18f524` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:39ff9f4f25ed4953cd5db2d0083339d712ab1ff2adfdcf3e8cd461da94cb1c97` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:a4747493c498b85448de88e4a2b9f967a33886e256c5b7b257c0cebe41963245` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:f49c20ffe5a816f929d0231f7bbd8ddfec37b74b0de992012401b6ff1f0d7b92` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:d56c941c25964d6eca44fa033f12e4bfdc1e34df24bcad03ea35ba687fd91a4a` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:18cec69b7f443140755c55cdc3593a4be7decbf774420e7aeeb38eff92b7b880` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:60f1de16c63c4b1d1450c1b58f06b9ae6f33547d133b07e6f9e57035188a82f6` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:b314044779cd4296cca629d1e5cd01c0c1caebccfb32603b32c07e0374b2832c` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:5819f0f4fb50e4fb8f0485dfdd134ebac74b2376371a0b8f6c915a3e15873d6d` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:c2346a98f8d17ee50da4ced6d4cccf7d36a4e9589c571237b3f4850a411d66e0` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:3accfe8f947359764e92831bdfb5d33ac8add29e8c43ef0af3dfe1c3ff004783` |

| Gebietsschema für v2.11.0          | Notizen                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:32c26ed8370d1f30098811fda382e68aceccabc671570365f15ead37c3d84304` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:a6af48cdaf9f7562bfaced449016106dbde5c678fdd4c69985d166959a38b146` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:43cec166dcde9dc7cd535228440d11d396518fcfb14d9fa617e6e26f5156dc84` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:b55095b27e8eef60dfe9657735a425b9ca1fe3c29ce4ff1f3d67bf7b2ac77bb1` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:7cc4ad997a76844414a982982251653525f27dc396db44f23b7f012d20f53677` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:32c26ed8370d1f30098811fda382e68aceccabc671570365f15ead37c3d84304` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:5d3b402f41f616ee792a5e7e3f41b4ec5638dc8ad60a3c133ec588e07b09d581` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:c4f88fdaec73ebe241d6c94695b20eb2c792a9fd77dbb51f24fc7807dfd0dc61` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:32c26ed8370d1f30098811fda382e68aceccabc671570365f15ead37c3d84304` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:32c26ed8370d1f30098811fda382e68aceccabc671570365f15ead37c3d84304` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:a42b6f63a16313f280088bd47978e177bc2f1bf2d392a070cf5c6a06d9f7a62c` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:21425557e62d71326e9eb614c535878f981a914bf66d9dd883221656ca891858` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:682e8a8ad5f2582f25a18b0518f9fba9b3849b72eb5dab5454586724272c52de` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:1d0661ae5920f82e607c72ae7d6eee917c190d80c3d13403d770947c67a4294e` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:8d5257d6c326e4d96ba395faa0c717f48c4d437866f8dc1e1252c5e983b3008f` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:086a4e33f746868fc1865322f1d7dfb5c1c3af64bdbd369804155f18710ad96e` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:0e2c7d5337f953d45fc7594317e6eab5eecec44a1c15fba51a128fc510519c3f` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:dcfe3fc95b895d0205a7b72368595e98dfdcb4b6398522e7daa2fbbe2b087ef6` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:f04cedb6b50560f0584cb3634cbfee5e9c147d60fc044cbd0df10fc28f04ed98` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:9692c45c6b5b8716f99852a2ddf4b7fd1e2c00ea29f9a20da68e899cf3064fa1` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:97106aa991b4ef5b0f1859ae7a7df3c6e22dd009123281a7458d336a78ebd854` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:da2bc14cd86f200a439b3ce708c6643d507d482daabae87c351bee4c10efa60b` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:f8fc43e5d20afe8108b6f35c3e09d403557f150413672d45322421be1fddff20` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:abb8ca669c806a71af88d3643694252e1833ca99aacbd739a3962ec00c3cdb61` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:13bc7717dd73f4323956a3f7441b24dd2f86c13d41adc709e3f6f26266cacd91` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:b7f44d7cf4bbe4d89729207a38e91726c321ea03a66c5e5624b27ae9913fdafa` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:d81ee15821646607aec9fa46223c9197f74675a89070912ca892ad5adfcab6f9` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:2e2f9102c9f6fba0736fb01d745d35b677bf92750eed5cad245ee089998f66f2` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:dd962ec3f32b8fdeb15f7ab18ea9d19e7c93baf4c801fac59d44f5cf845e9935` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:f89c0e513f43800e1d19177384b815c1a04f5b07ccba8fd9c80aa5ebf5c71648` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:3ebc64dceb1b7fbef716de3736a020b23e8fb4e9aceb183524863681e0b278fe` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:ba05465c312acf6b9a1a1866c81c795027470e8bda8389dd0fcb641c9f1af592` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:51d49d90f600ae971019974a6a38c71b3bf01a84301ee6e8604c3f424bc6773f` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:a19f0ab805d0268c06a0e83aad2dcab458638e8c2f7869f5b2315695ae2ea4d8` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:a9539f091ec3feef34511ce9d337436151980eda69c7f8c8f2493e8d1be81e66` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:a0f5c19a683b92566747db79e30ac7ad09cde07bcb15451166b5257d036a86bc` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:2aa5e82c726a8771c706a2de38bed09ca9c8298bb166c49fa227b8966011efa4` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:60361c1a305d0fef3deb0e4886c4044aebcf41878a748bc0615b94fcf9489cf9` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:d628b894966988880bb11f1ec1380702077bd45c2a83b912ae3e7451d8fd90cb` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:2bd901c320237e041ecca1ea34c359cf847cf8dacecfcb0e1ed8fd1794463fe5` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:099d21e5e5816d5d7e0965cda5878bfe78f5447e4994957dcc45ae40223b14b1` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:af6c258b7e984ee17d32b9dfc49969cfc1d7ee33aa2485017fab191d8d574e92` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:7d0e03c7f44f61b4632b730c2cf8e3d7c584a869bb5d53b9e5021549d1d500a8` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:ad580c1ac73d919434387869803d9fabec24e19afd6b4cc5aa7e809fb93dc908` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:2e85df2af0003c0a41752c6e989ed8b724a22958e7ed3cbf67e54ca621bb5975` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:bae49ae543878096c1dd0c77a8f83a30ba1416605efa58dad59ca3577f7006ea` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:fd9deebe4e5a4466af439a8e40a1a39261a7b0228a4ed979b8086e1c65c60e26` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:0e69fc4689dafad97e00bed7c4eb7ca44b94e3a3d9357d6d36bed8135963e9e4` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:37ebac38fac4306668858140736d83e008ae0756f8e1fe5ed6386780bc9796ba` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:223d494cf64cdceaabe6e9bae82d378d7ea53eb8c01d58bdbd2e1ed360aaa34b` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:378e5735198e38d6bed8c87a59ed69f8c3bd57ac8a462332d74dd8495cb07ed2` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:d92f672c2a61a67db43d9884bc2692c304b3c2c5446bed2d315892876270366b` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:11dc172c7ae91b6cba7fb4ab1a61e48b27b193bf434a68827eb197c0ba05d6fb` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:3057eaaf8e0403690c0223c0db3a392b05f2ec45e53511327b8447912e32b8b4` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:37062edf6805dce30309e4615c2947dded730b5b5be7e3bcd85bb93e38b08f31` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:9f1bf1901a6b0e2caf4c9ff30e0b6bb3f1f4f814ad86fc62a471d4fe1fe4c101` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:095b40ad1afeebd932c299410a4732fd64da2251230aa044ca2c43b4d0bb6791` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:60e9257735cee7dc6cde1b5725588b1c1ea84f852220f1f4f3e873177a24fc5c` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:71c5e3a9196155678a6ad9cd62b812386579521ac410b40e3526dee153d749e1` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:fce7d215575d2a94cdb4818bb1525f6448f5f881fc3e7f04274c64978bd6aaa7` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:d71d8e1e3692bb0781e98b984dea79950a8009a6fa03e729325c338ca5c09a98` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:dc2e35e158c09fd793b180050a0100df4a3716da4d0a7a528dc3ea65b6ecf21b` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:e6ab373eb9477d90d44175fffb646298d403405633e0a61ccf20f9e7381243b8` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:0ce15c2d14bba49639adea30c91df1ac47e7b2a7796be551276bad8ec8312ed4` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:bbe958ff9c7c51efc6521866173b26ac2cfe682d114ce3ed6b1f6b8e9b3a7327` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:4e4d890605e09717ef88982f586611c605342465a8ef81f2280f665ad1378522` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:60bd2d1f817019e6626876b15f5697be07c3b2b368e4cc7e3c3871c3e9181052` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:c8520e7155ef176fb9fea48c541acae995a6a80ba6913ac4289786ee55062ce6` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:c8440308a5cb77791f33ae458c49abc084a1be8c418df9feeda9a4aa917a59bc` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:a66739b36a410c181ccd2205c59fee2726b3905d1c5ba4531909be96cf85a55c` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:c4ba7ff5c11d4243a3e128aca1f8110e62df82d956706c97c237016a94cb485f` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:c3fc4117598c0dcea0fd5e6f19adf7763e42732e32e3ac93ff74795fdc167e67` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:78bcfa610f645c113134cc24c8af8dd3c630065c1b009fb5e36dfab4999c16fb` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:134eb68c900787bae3a98a2bdf192f2a5460fb96b92590d65765d982245a7ccf` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:d194aaefe82a5f91df9e01beec271ad9565c4d36cb0539421e947b5c8e67228d` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:cf272b112b10587c034f00f7df2bfcdefbf542859fa089c15581040db99ed383` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:7364a1068f9940e9bb6ea5476b0a007a37d42b899dc4ba56be833e4d2b8d359d` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:21ce33714fa37bfede60560a7a24c17c88566c767b76c58c877a48c51811c9ac` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:b97035a4f0334f890ff3630a2de249b72a879de3c7d4fcc849c3d76aa97f4d2e` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:ae4a89a26768c978d91ed797e9ecb8035fdb61f12c1b1124c86939c79ddcb38e` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:41bc980abe79cd69034a8ade2be203478b531a00f5e74b1f7b8f9c5267700261` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:51a50a7fcd5a9db6422235a2df0e8fba360efcd3cefee9abe44ab2cdce62088f` |

| Gebietsschema für v2.10.0          | Notizen                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:da276dc1b481c002a9b3d2944e190af799175b5a2eabafab87153e22529bdab1` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:c2ae166526cb0c5d481b537daa3accd379c4b1bf51fce6d85ac20591e7e0b4c0` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:7d4a6cb1d9d66f6bd62f90b82000ef811f8a3dd58b03641b6c51ad6f0f4fd4dc` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:7489a0ed06fdf1da1d25e3211f5a66abe420babee148961a2ffe8cdbd82564a7` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:478e4575073660e9153811f58e74815f62395ee2ebd868d448fbc3a5e16442be` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:025dcbd6a7d1912812b2556ffd7a16ad2158be6c3746e2822f2b97f460aa685b` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:5af93722e70e445b3a4102bf621e6d5bb5854bcc99f60d4590e23fc24e50297e` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:a9402f03b02150288d51e03ec97b8efb98ad6c444df3ab50a3b4ce1129d02d86` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:122df16df46a84a14b28e4ff406a047947fdc10a65b40482438beee55579f687` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:7b7d7ef798a0210b8c33a3a201ba149e1264cc7ac6ddaf986721d86e91e5e444` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:ba8dd6564939eda7b81b1a4c13ad31672927528dd146698fce10c12d21f647a9` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:d0fa9bc409238ebdab0a15174b3169c99cbad42323087ea589bb7812a0550149` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:4c4a115ae8daf53e344c1c4f838ebc68c3de2dae4d1f1aceb021425807d96ac0` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:f18c31f2bc9e655b93f71049b40dae2213c7417169f7a4e42f603d5891857b2a` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:67f02cdb2285c2891aff8ff8d35ee20bad11f2d1cc1d67c461185466edefa5d6` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:ed606155b5f9b6c6dd68c0c1f5e48a0735bc4a5ded872655c0ef7de2bf084312` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:2fb6a64aaea5efdb2cac8bda2c7d437638fca93aa24268a45f2a395285e022df` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:9ddb64e481cec6449dfc48091092247fa401fcd48ab1d955c5186565f903bd34` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:060a87ae817a82486966a4f10d1e872d30370ea58e297ca4c2018d0e034bfbe5` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:ece4299bd7f02fe4403b53320cf55bb2e3ab65da3d94bfea09124c14955a3de3` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:6b47286a882122de8114942d426cbb8b4f1aded318032317b03a6b68237372e0` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:41fa2caec6a732736f75b682e0410b89ba5e12307cd6e2652986a2676a5dd560` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:80ae57602d8e66c6ed0366327a87c0ed5717b44c596b981a2b5be09c7f5a4c8a` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:705c125e5105b6eed37d745e2092d55ca8b6ccff22f4eeac9c2df958f36c72e9` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:67f794f16fdac457f0e0a84192e588611adb43777635b14706754c19fd90b130` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:94f755e70043dbe011424a0f756970f1d01ec51cb95a469531e3a6b0aa84aed1` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:c42eb56cbb48e0957f73793f83435c705ed0f857579acb020394025abdd760e2` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:7cfacb01fdb80bd1b5e68d16f9e2741237ae4ec1a41a9121aed1be2622fc9f3f` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:7dbe5becdf4f3264764eb596d61781a2b2ee54bf9552bbb8f4db5e7fcf75d8f8` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:a1064b4498b7c5972a8a79ea84b78c2e1e7698c039eab49fd08963d11798ac61` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:03cd0f0bae11df645dff52b15746e31493522db5399a18878df765b6aace0a80` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:3dc8d3f0842089edde4703abe8df3a219fb177afd5ac370c5b04c85abae4ca15` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:7b0927c3b60bf38e995c57a27843680d9062d88611c49378dda8f71a4602f7a4` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:72e51683124c76255ec9280cd0641d6e44633199bda769ddb31336362f6e641d` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:a948970cd11e2597ba150291b2dcc72f2d59ad4f693933ef1f72c210f19fb663` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:b773cd7bbb5eba548bc468c2f6d50732e2553c5f8ba4b955404140def4c3f3fd` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:db73492bd83597c1fa47e7c4ab5eedbc1afa7662088fb03df2aaa5b737b5f837` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:d3a86e840438eb2278d0bbfdf1fc98a48fd744fb8c92118f6d3d6298c45a2b96` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:b60dae65bf1fe20e698ce32811373473d811bc363d4db093b643238f71461d4c` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:2a81a9d1b32c546ec03caeeaeddb1b26e5e00747c691f5be62f9d23c5ba84377` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:c4b91cd5e017060a82a34f83d3f62a16b856313c02fea048d300abf149aadf67` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:2a5bddc5355d6eb0b101423c733d6cf067bafd0e152b63bf6c4dcd943ff561f3` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:f60037ad8dd2b40f588608a5eace8b0b9f3171d05d39a02c2dd1afe98ea7e18d` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:e302da84ee0264221f0e663470f579348664ddef37050bc0fe57c620264bae06` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:b6a79de315c73ec3301aa0cfa7ed920abbf8b6f80fd3d42637b785ee97a85584` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:2de931f1e6f38cdc2f54a08bc1e64a13876326d57784f0ad1c50384381790b05` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:47c1b3cceb8a6f0b2ea16160ba8c503d39ac77f44c254dc880b5e17d2aba4a4c` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:bf40fbfce8241e14656df47178d7b57f19022cc6b2598de5b337c6710eba99b6` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:93e0d58ed07d637c3e394ce80ee93524697063cb693da2aed9013660b2543702` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:1d239549ecf7f6bef5f9d258f5fd34f81fb0e5fff89c66dfec769e912b1cbf7b` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:596f42a366a61d1cf05dedb81a4f373cfae2dc04e8bec3479bfec121417dd4fb` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:fcdad9382db8fc7ff0a7ad59fa9fd4cd319ca258edff869b66d76031bcfee640` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:533a6420a4a98d4a2c947d26511e90651fc341c96b90a02615b38ce2a799f058` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:ec6b95c03d9d5030457c4a9e1fd8e07fbae24ec50b0bb3b2a95eadcd81a1d136` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:67cc80b8159122c530913505fed0f7bc4edfd3d77b25bc34b6c6157d57178728` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:2b1f3b4220f8a7a44c8339e4c6a4b9a55f7583b5540f045997c9cab8364facb2` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:4703fd5e1c5020d5c58b1adde30e5209b1e6f21d0636bac11013dcf8da9340d3` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:58c2bb9cf2ead05fc77b3962ee7cef0e0eec33e32697757f65ae8925d55f87b8` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:dcdeed91559fb7e1b7d2ea70215ec373a59afa6b67468d13316af109314ca384` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:06745d241654571428c219c38cd43b56e92b97eeb5aa6656ac726da79460afc1` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:3c85f1057b5942c5d2094055e7b9ecc6ef995905bbdabfad48bfefb805f436cf` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:313f2fb20b8c2a18bd6ce5e7877899310575d390a2c3c54cd2519d0538393201` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:7c897fdb38661eb60f576c0a1a9d69bab9e44e7a70e8136fa3d12531cde0e4d7` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:9bf17aa5d4c577a440c770b6a63b66037a201cbea0202af4856257fde0548f0e` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:08f0bb7f1454c5d6c740d218013f47c54bed17701e05c239364f5b2eba07692e` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:0643e1c342cf6d526620a46b3435c130702b9320a6075ede1351810956ed6ae9` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:139f83900395a0d1af99dc90e661238ca2fa0bc06c74cbac28631ba0399345bf` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:1c38423ccd1b8042d43eabe013f5b6989556610ada803b4367848b58c4832a76` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:2b33e5d5ae0cc46bd9a4ae860fe22f088903d4978b287df4eff6ae63b91566f3` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:40a667412882bfe8073abf376fe94378d7c364e7b22aee410d7b6e99d65e55be` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:d55464b46585fcfd86c420a30d11b10f3b5c9c0d70390b75f40fe9dbbeeefa99` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:06f3f986ff92f16e963771da485695ec9e1da482b10f35babb2d54e260da23e7` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:0566062d116cb06c3eb365dce6e86d9c46ce37293b11ef71c4e219c3a11ca559` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:0fa6da985d839919fedb503625383dcda04de6bd39558f2f72b64410675b8f85` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:88418775c8a8df79aa52de03091b938b7a4efc708907556dfbe3e1d686050e81` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:9087a08cc455772515f5775a788cdde35d7f5bbe3aa3ba34ae99573fd87b29a1` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:372e1c256520e9ee84c4c400eae935c1d6b1d59adb2be4c4dbc56439db069ba0` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:8406a3be34530c7d654d1dfa1c593dd51b8946b480fe80a100e599e86385dc2b` |

| Gebietsschema für v2.9.0           | Notizen                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:41e7942e4026beaad93e50f199a6a2d855f77c74e60bc9636bf2bf2c7d3bd482` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:d27f383435770aa01bb4117ba2d50a05ec172a1da35c4920ab43cd0fb74f44c2` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:ca2734a6bfc562c4c07981358051d281fb5e089815b9eac14c66a0e6f92e9858` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:57429ee8e95a76ec953f1b1f94b39a20507626cd7fe5431df826912e5b959e41` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:4c5fb6fdc08343e8640222583373effae3d03907cf1262a4fad3303df9385797` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:5ffd280908e3ee65fcb7bea0b532844f9d8510044ab4c2c612dc3c235938ad0a` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:00f3d1fd6ccb857ccef8a72322336e7a097d04027411f0dcc5499b44229fb470` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:aa6ae12f786dcaa028e5867abba198effed875b6bc4cbafd4be37349e95dceef` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:515a940ccd76ef1926bab3ad259e1cc7ac2bd90bb3860d28f83d0f6324b3f0fe` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:03f6242d73de64c3eb3347400ea6e7408a8816bd96f3d6368ea2a8193accd457` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:ed6714e804ff2d1bbd41512c78906ad9b8827dfdfed0076a271817e075c2ec40` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:386f2bb4c4b6ba797919ddcb5bbc9942bf8a03e774f9b01438f9bae0928414ef` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:28696d10c78404fec033794e6e6ae0bfd92b0dab5cf7eb1d24cc2cdfbfcb646d` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:dd9ce70f83767a5bdc52fd62b96e09ce6f79ecc1903ed8e116753099b06b03cd` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:70095cf952565256f3a0927358d0fd802d28fe1c3b89b26ead31ba1127cd0b06` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:836bc38328636799ec9c8717618d51ab8b50ea2f0dc9663f342c4454938c9b23` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:eda3702d95d4ae3b64ceb93bda42e8522776e141a18b2a3dde3bc3fcf0e9a2b8` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:bfc2126fffb947bf10ac379efb70db3d2c7ee2c16dd541a5b86e03e73d7d477c` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:5660d02eabf4e1e9f58e7993ed7e5917b1990b41ed35a484a715d7265400cd0b` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:891c1805fd8011865de7371ffd4bde85d879341f2100e8053bbbc722d7c792bc` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:21d6d46398f940a769241fdfffec5658356e54b4127b44efe5e061724f7a7681` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:6f473b8ba56bad098c21a0c0496cb312dafcfb83dc1a2e1aff21011f6b39321d` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:20aa22d24e35f7d92ceac96d2cbab8ce46ee0ed7bb601f18fa867f1bd0bcf5ab` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:5e5ad2b016a1ceac500813e0a68ff4108ddf5a4ca98cb0aed4930b6d1e8920dd` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:b372d9e32e7b518bb9949d8db459bd4e300304e53aed1342aba65a054d4a4c25` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:d3538f3834c554ebebbdfe75e261a06f104dfa27143353601c3a6a3d41025129` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:0bb100ef5313b182a59c08949e4baf1086bde2c1a6bca3324c4e052f465f7632` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:cdab27080ef3ded55dcf89cf85bc2ae16de1372f84a42d836ff5f20612b68a61` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:e4ea51ffa38f347adc7c0642d50237cfa045683f52b5e3e726e4c28688231d35` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:f81c0b7f774d64e673a1311d00604f5e4837fdba4d8fb4a2ab0c8bb8b7fde87d` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:78035c54e649e34cd8276a402f9c9845e13bc40503da6c2f631698a16a049c67` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:e4e4d9c123e452f8ae89bf6cc1292a406f7b482668e36b48ef2fbb29f14c4360` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:10a4ddd279633cc8696b00be77f6e9309494a560244a325982522aaa805806e7` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:a603a8f9c1778808df5d14e3fa1c7e993ef9cca3e0b515a4d4586c2c3a1d14b6` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:4f539f8019c489623868bf02f3c61ed4b66d3a85e89250a9b484717a91e9489e` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:20fc3806f08ad4e6fd5fb1f71318f1f5b591e2085ee4cbba2f25ea06135e5f6a` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:d65520a4f628f6a416171ac58341579fdffba97ddd2941a910bda385d31c735d` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:d38ea88613f5db6d6d9f879ef92a204c524bb27766848b825d1e6ce2a9b13cf7` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:02205d1ecc29feed3ac8442dbdc1855c419749d9dcbd98028a5d1619166f0328` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:c9c3e1ac800120a14f472c8be62730a489e00f29df29fe770a56429ea1c09ef5` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:859c24c40e65bc19a866218466eb7678f71205bedfcb6ee3180b6cb721194b9a` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:036f13d34005f5d6634387c9d13c3535724795b0d6cad832fc46363609fc2f11` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:b8eb300d0a11dc397d0bab02e1f6b26de6091595fd052ebb607f196c28d16f1c` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:0ffba124ecd79777ca08055689a1d853916ccd8c8f2806d0001edf5eb4aa42fa` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:4d7caf48264eaf18bb2d07b0258d6f64b7c26815fdbdf812718dd8e88f1a6d1e` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:310abdc1a8490990a99ce061f04c9d49cafb7a452fbfdc2790de6f60e1505c6c` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:8f209d30b2d148224b296c2d2c204b5970fbe7aaf5eb3289cf8b6644bfd78373` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:11b718d4b86d606b198e47deaa25f6ce164cfc53267048e3d2dbe1bc8500cc5a` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:7a4264a0e9560e6aa3fdee80c3e3f55a0e26cddce8ebbeb7a9c87693ab451a25` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:bbc764ac08b2ef10ac58a8f9534d4d375109fdf16ab75c8cdbf2d57aa692d3e2` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:2d0a83b7bcf1cfc50cf013c95442519e5236a146b7968e75e129b3a5c33ad3a1` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:f0ee8f259035ac5dd9ef38807495d0f8d989ddbb8eacf83893f1fea22265e6b4` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:6101ecac9f5f35c1ea1b8cd8e52fdbbc1be2582e4f3e385c16509fd95a002217` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:9e94c4d6fff73058ce4eef609b8404430a429c6961648655c915cb2fac10656f` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:44986ad44bb53eaf350e0865e62ea5ba7f37d1f5b52e388f61f56fd7afe8ff32` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:6b7aaa828d1b2d2fce1831e540e08ba60307088b90ca32e96fd002a67aff926b` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:1abeda544a7579daac7f8b8f8d34a2cc63b4bd3631e474315d424973ae024ab0` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:455da50a7db591df7be69d7cd361a77734b9249101d8cf86b807f0350b5167ef` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:676e17b6223e35d1897b46536e6f523e1d18b78f834b62ec00bb126ad3a2e71a` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:dbfb97e52dc4b4c71dec1a9e622714f004b1e59d7900260e09a85bf15912fccd` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:19f7f644ae3a0639fdcc53acc065d0e534b74c07f8c095418d4d4d444c566bf1` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:d3a13ab6fa2eb5d5ca0e3281b1092452650e9ede8749f6edcab990e3bbb8d198` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:7ad5e61f9a72c600bdc79e4c04ac63c239951ac4c0d44e02fe0607a6aff356cc` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:fe6a4812534d704b145b84fd8857fb3d9052f67fcbbd5d490c5902082e295195` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:adcd34941d4ace7db01bd476d61c9bbafe071419932b4cfae5231cf202af3a14` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:0534a7e4b391f1ee666b248a274879c081496ed4939b0ad33154d8a96fd67f94` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:091ea4a31652ff9dbc6259636f6c12b0ceb79a269e2cf3cdec677a1914b6a64e` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:5eef3ae8afb445e60bb913edd6eed1415abb0bfbc439978f69f4cba7b61c8e6e` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:98709e9349d889b57933317005af42770e47ce8178a7d9c737d9fbdd81148478` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:3a9139334c4780dc6f6a9b0f15fba5292e16ecf1f5d45fe49a9c8ef3b0e110b3` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:b29b2a65d83c20d65ba4e4fbca66f9fc07e536e161f90448c2bb360eb8de1e55` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:4302e1d979b24a23595ee2b1fd074a57ee36166ce9ac400a3deb397341ae52b2` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:69be11a63199d9a6f63ac346e689051ba9cd5214894b110da2879aaa0f4a8e88` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:2e4167dacdcb2c9d91930356ebae311b6b33ceb3e85f908422e880edbd42da64` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:d46289ee9ba71c9c1dbbefa5da439e71310af74633c9d6d6d448d2ebee60da02` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:49eeee500e07ffd3056ba8aab314d6c8458399a8c0d6d44ce1d9aebf50ddca06` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:5a3251ad6df9565d44dd422de4fa0d83a9b50c8a80ec15213403482940d2b2fc` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:2c45dd90b0c19d7f12b1be44d3e85fe2603cea2389c2877b79d6de351839cf6a` |

| Gebietsschema für v2.7.0           | Notizen                                    | Digest                                                                   |
|-----------------------------|:-----------------------------------------|:-------------------------------------------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | Containerimage mit dem Gebietsschema `ar-BH`. | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | Containerimage mit dem Gebietsschema `ar-IQ`. | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | Containerimage mit dem Gebietsschema `ar-JO`. | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | Containerimage mit dem Gebietsschema `ar-LB`. | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | Containerimage mit dem Gebietsschema `ar-OM`. | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | Containerimage mit dem Gebietsschema `ar-SY`. | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | Containerimage mit dem Gebietsschema `bg-BG`. | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | Containerimage mit dem Gebietsschema `el-GR`. | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | Containerimage mit dem Gebietsschema `en-HK`. | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | Containerimage mit dem Gebietsschema `en-IE`. | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | Containerimage mit dem Gebietsschema `en-PH`. | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | Containerimage mit dem Gebietsschema `en-SG`. | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | Containerimage mit dem Gebietsschema `en-ZA`. | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | Containerimage mit dem Gebietsschema `es-AR`. | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | Containerimage mit dem Gebietsschema `es-BO`. | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | Containerimage mit dem Gebietsschema `es-CL`. | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | Containerimage mit dem Gebietsschema `es-CO`. | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | Containerimage mit dem Gebietsschema `es-CR`. | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | Containerimage mit dem Gebietsschema `es-CU`. | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | Containerimage mit dem Gebietsschema `es-DO`. | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | Containerimage mit dem Gebietsschema `es-EC`. | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | Containerimage mit dem Gebietsschema `es-GT`. | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | Containerimage mit dem Gebietsschema `es-HN`. | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | Containerimage mit dem Gebietsschema `es-NI`. | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | Containerimage mit dem Gebietsschema `es-PA`. | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | Containerimage mit dem Gebietsschema `es-PE`. | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | Containerimage mit dem Gebietsschema `es-PR`. | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | Containerimage mit dem Gebietsschema `es-PY`. | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | Containerimage mit dem Gebietsschema `es-SV`. | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | Containerimage mit dem Gebietsschema `es-US`. | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | Containerimage mit dem Gebietsschema `es-UY`. | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | Containerimage mit dem Gebietsschema `es-VE`. | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | Containerimage mit dem Gebietsschema `et-EE`. | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | Containerimage mit dem Gebietsschema `ga-IE`. | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | Containerimage mit dem Gebietsschema `hr-HR`. | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | Containerimage mit dem Gebietsschema `hu-HU`. | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | Containerimage mit dem Gebietsschema `lt-LT`. | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | Containerimage mit dem Gebietsschema `lv-LV`. | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | Containerimage mit dem Gebietsschema `mt-MT`. | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | Containerimage mit dem Gebietsschema `ro-RO`. | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | Containerimage mit dem Gebietsschema `sk-SK`. | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | Containerimage mit dem Gebietsschema `sl-SI`. | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |

| Gebietsschema für v2.6.0           | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. |
| `cs-cz`                     | Containerimage mit dem Gebietsschema `cs-CZ`. |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. |

| Gebietsschema für v2.5.0           | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Containerimage mit dem Gebietsschema `ar-AE`. |
| `ar-eg`                     | Containerimage mit dem Gebietsschema `ar-EG`. |
| `ar-kw`                     | Containerimage mit dem Gebietsschema `ar-KW`. |
| `ar-qa`                     | Containerimage mit dem Gebietsschema `ar-QA`. |
| `ar-sa`                     | Containerimage mit dem Gebietsschema `ar-SA`. |
| `ca-es`                     | Containerimage mit dem Gebietsschema `ca-ES`. |
| `da-dk`                     | Containerimage mit dem Gebietsschema `da-DK`. |
| `de-de`                     | Containerimage mit dem Gebietsschema `de-DE`. |
| `en-au`                     | Containerimage mit dem Gebietsschema `en-AU`. |
| `en-ca`                     | Containerimage mit dem Gebietsschema `en-CA`. |
| `en-gb`                     | Containerimage mit dem Gebietsschema `en-GB`. |
| `en-in`                     | Containerimage mit dem Gebietsschema `en-IN`. |
| `en-nz`                     | Containerimage mit dem Gebietsschema `en-NZ`. |
| `en-us`                     | Containerimage mit dem Gebietsschema `en-US`. |
| `es-es`                     | Containerimage mit dem Gebietsschema `es-ES`. |
| `es-mx`                     | Containerimage mit dem Gebietsschema `es-MX`. |
| `fi-fi`                     | Containerimage mit dem Gebietsschema `fi-FI`. |
| `fr-ca`                     | Containerimage mit dem Gebietsschema `fr-CA`. |
| `fr-fr`                     | Containerimage mit dem Gebietsschema `fr-FR`. |
| `gu-in`                     | Containerimage mit dem Gebietsschema `gu-IN`. |
| `hi-in`                     | Containerimage mit dem Gebietsschema `hi-IN`. |
| `it-it`                     | Containerimage mit dem Gebietsschema `it-IT`. |
| `ja-jp`                     | Containerimage mit dem Gebietsschema `ja-JP`. |
| `ko-kr`                     | Containerimage mit dem Gebietsschema `ko-KR`. |
| `mr-in`                     | Containerimage mit dem Gebietsschema `mr-IN`. |
| `nb-no`                     | Containerimage mit dem Gebietsschema `nb-NO`. |
| `nl-nl`                     | Containerimage mit dem Gebietsschema `nl-NL`. |
| `pl-pl`                     | Containerimage mit dem Gebietsschema `pl-PL`. |
| `pt-br`                     | Containerimage mit dem Gebietsschema `pt-BR`. |
| `pt-pt`                     | Containerimage mit dem Gebietsschema `pt-PT`. |
| `ru-ru`                     | Containerimage mit dem Gebietsschema `ru-RU`. |
| `sv-se`                     | Containerimage mit dem Gebietsschema `sv-SE`. |
| `ta-in`                     | Containerimage mit dem Gebietsschema `ta-IN`. |
| `te-in`                     | Containerimage mit dem Gebietsschema `te-IN`. |
| `th-th`                     | Containerimage mit dem Gebietsschema `th-TH`. |
| `tr-tr`                     | Containerimage mit dem Gebietsschema `tr-TR`. |
| `zh-cn`                     | Containerimage mit dem Gebietsschema `zh-CN`. |
| `zh-hk`                     | Containerimage mit dem Gebietsschema `zh-HK`. |
| `zh-tw`                     | Containerimage mit dem Gebietsschema `zh-TW`. |

---

## <a name="text-to-speech"></a>Text-zu-Sprache

Das Containerimage [Sprachsynthese][sp-tts] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `1.14.1-amd64-<locale-and-voice>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

| Imagetags                                  | Notizen                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.                                            | 
| `1.14.1-amd64-<locale-and-voice>`           | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.14.1-amd64-en-us-ariarus`. |

| Gebietsschemas für v1.14.1                         | Notizen                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            | `sha256:506c4694cb4628aab870d81b53885c4b63f7d167fcc3407dd7a203ab3da6bd9b` |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           | `sha256:ec6963d01458464eff3ed2be965cbe782c11bd751022ead9d4dad39caa7db4a1` |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            | `sha256:d296080e707bb20eba7db2473c8caa76c17ded594b8a82e0932a71694ee0f2a9` |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       | `sha256:80545662ec2dce6949c902351dd29be9778749ee980efc0c78be5074a9e126a8` |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           | `sha256:206773547eadde8e5e396ebac9f7a17e0e20ba6c8a453f7c03c8723689224384` |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        | `sha256:b5636a23d0d0a9c6f5c93885a1033730bf1f0c12335769fc544bb23f1697ae21` |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         | `sha256:df6d494145125b1945626834084f8f8d91d7b996edf417e33ec8d9441665cc16` |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         | `sha256:65a088fa6dc97d60c2d35214af0c90a6e9a33ae2f4082270dcc7961a64e38bfd` |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:acd5c459d0447aa39e4bf5ed74c7f4fdfa275c3ca0cabc24ee4f110f6500e743` |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:acd5c459d0447aa39e4bf5ed74c7f4fdfa275c3ca0cabc24ee4f110f6500e743` |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   | `sha256:a879c3dff58420b8af5fb955e8cb5727c76f7acddfe89dde298ca0934d72f1aa` |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        | `sha256:50422aa0cd5b58a5e1c4e334e7098f7590f02fbfb392a5d08fde2018577a6cac` |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       | `sha256:68ee93b7e541836fb4df93a6925edc9734a8390765fd10b9541eddb94788128d` |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       | `sha256:b4c6a1580faf6466238060c9e26b2c9bf17da2ee8492f856fceb96e927722c70` |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      | `sha256:1ada3a373ae2e3475c8e1ee9b2a5966ae126376bb5ac0c01e07591b53de5c2e4` |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           | `sha256:4989ac096aa8923ef16c823cd3767730dcbea633827d269a1e5dc9206325edcc` |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   | `sha256:1fc5a152d99e61823a8d0253ba1c04a79c1a846b5c135e1638695f47d21b936c` |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        | `sha256:8814ea674f531e12e0d502cc542afbabf5123107f05792215c81f68a259cd5e8` |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    | `sha256:3dd9b566fb592009693159d2c1eeebb034e22124746ee4d20f7b904a04e90a5b` |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            | `sha256:a1cddb74a6f14c3f9e3514dbcd64d05406f36e79089ef8217fcb724f8126a3e9` |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    | `sha256:1f5e27a078dc61d558864b29e060e963fe1cd4e56d5a5c33e943088803f3b3fd` |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        | `sha256:0f2873c0a80159624960b1d7c3dafa1e60be69f94aa1939bac37bdb941240ba1` |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     | `sha256:338a4c2b0923d44895ebba1d3aed13eef8ec775c911e39ee9acd33b304831db0` |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      | `sha256:ab856028f3ab7c7af881b4e53fe957bc89d3f8bb1daf7b3376593f845cac1fad` |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         | `sha256:ab856028f3ab7c7af881b4e53fe957bc89d3f8bb1daf7b3376593f845cac1fad` |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     | `sha256:0e4862eb77acb3b3f5c08984ce3605d06e12876b72d5c48dcd86e05461aecff7` |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       | `sha256:bde0c632722de7093c787c076e73cfcc84ce6afa282fc269a7fb5e3edc5e986a` |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         | `sha256:feebe5f990e6713c2a8e3759059553c9b9ec59505449686896bd7ef25d2d4bd8` |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       | `sha256:84b9517218281c7660f2851e819dc79a003cd2c06adf50341a46293dab3754db` |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    | `sha256:fbcdd314a1c94b60a338c9a3b352fdb19bc0d64d1e698ae8ca9b30eeb0cc89b0` |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    | `sha256:4d0a3a6f789acbee3cf52e26ce4f2bc7f15a1d51bd4a4187262fbd432a7a0512` |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        | `sha256:232730b6b1732a6169b024f9513527a01f515b5534ffbe5e6b0ec816c452333b` |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     | `sha256:a24417b4e2d2f22c17a6a2ea6ae8acd67386881c1c10e7cb4988a4fc93e06b72` |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        | `sha256:24178c994f15ef135453b6417c3866e5cc6e0db4767a0ed70a446fe67d2124de` |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        | `sha256:3e9b860513a1f0ebfe4280fa7994348305c78fccf00906e1983e1e557b44d455` |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     | `sha256:3b5a7a1e8a01782e12a1b39f9f2981a3f1798751351251e6d477f4df1b5f4997` |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       | `sha256:b2cbd6b417b42e11d6d64d8a1f26b2f00f398ec2225207dd89043b859712b261` |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     | `sha256:dc2b98bb93526bc95bff551a3dc3869afff041a904022bc3bd2d30b0b7ce1993` |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    | `sha256:1af6a1807b4d4d48a1f7229e6e03360d9bb979113bbe4f4590975f9e98f09af1` |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     | `sha256:0b5ed83a9a48cba741b5e491926bb5a1e3022eda8660b573e3abb231f3f81b73` |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            | `sha256:5f2307252f16876be05545581f1698c8a8834c4b462db76c151400c538f1aff4` |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          | `sha256:a86d04e0ae19a1ca30ba14a4951e8f8d78c4c27a78378f07e5f37a753e282ea9` |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  | `sha256:1e56c468fae9c07c76581a7c7430d9bcc02eeaee5e4657830a2c59649cdfd80c` |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         | `sha256:1e56c468fae9c07c76581a7c7430d9bcc02eeaee5e4657830a2c59649cdfd80c` |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           | `sha256:7445bc7d1d73c5bb4775de73253b4733fbe53caae93a7bd5093f2cf61dc7f7cd` |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        | `sha256:96050684a66cede45f5a757dc6faa45663efcae1739abc820a77a7e171b7733a` |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          | `sha256:28065b6532a04912cb59104e7d6d1904be3b71b8f45427082825c752c3f1737e` |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   | `sha256:ee465ab38a0b9331fdf7a1baeda62b6a368b2aceb10754158e3f14a45b473dfd` |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        | `sha256:b15a06df122dac510aa9327aa623147435ce2e576ebbe0be1c28ecf19b4f9717` |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    | `sha256:cbced8cfbd556c8a169bfd2da35446787c5f5acd1607083155cf2f8e7ad8b2a2` |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       | `sha256:1dda74d78c7227c45720e6aac912053160a65957b43b0b528376dc3f7a8570f6` |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   | `sha256:ffa25c2702b5156e97eb9457085341d035add070d43638e78b0ae9f2f23fe76b` |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        | `sha256:f4955991abb31d5814913e49c17535f79b618f3376de75af1feac74ff9430cd5` |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          | `sha256:4c4fdfc2c70ae624d69c1435433068efacccd96809e9112a4fcb1f4e52802d00` |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        | `sha256:080902d1f8f67d018746d3099d2739fc203cf87959912e45352a7525c7b95bb9` |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        | `sha256:b3c808f060b29485c8a18f5b717f96f4f1d5c724811012cf9ad4654b658b08f6` |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      | `sha256:f95ded0a8f5dc9bf53f469fcd8c9608fa53ab45b5fdc915f132fff3cb6fcb8e0` |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   | `sha256:da85762763f2a4cf6de112244138aee57235bbfab807e5dd80b76e9fc6703e44` |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      | `sha256:085dd402f070660f2a0a9139b2b09ec7699191533e4b442260364715fd83ff38` |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        | `sha256:4cf8270fb836dda947580886891c79d07ccd9cca7cfb19d328fafba9f61d5303` |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          | `sha256:a11f8da57c87b49145293b1c91e2073f96a70301b839e9d9848fdd1a2a164aed` |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    | `sha256:e6619b9518029ba9e19d6b98dbe1b79c676c135248c32c9a3c3c2e3edb56efc7` |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    | `sha256:04ecb7975978c004fbe2960e74d71b9d1fdfbaea904f1104f519f43351dc77e5` |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    | `sha256:c7fe3fc2fd40891e51fe00c3bbbf5386b7400cee6091956ad08fa974fe7518d7` |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           | `sha256:e7624a3f3521a663bfd96f30904f722b16c6b2523fa2d150a578311c2abfe7b1` |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            | `sha256:898ab51ca3e6697b39391fdc34d76f79cea6a40dc53f9fb16ae9241e09eeaec1` |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       | `sha256:7aba595a1b4994dfb2002bc7c56e1dc94d92bb3e49ba9024ef2ebd8614deb24d` |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        | `sha256:850f8b7e23434c01fd3c901549bf00e541f0e86f96e75ed22531036acc899418` |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          | `sha256:cc155a9aba2e1f4786702b570608c4aa344fddaba9bd6f3d705a2cc8d5990b37` |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         | `sha256:3c0c5b6ea14b697219420730f195553ac691ff69cb65a7aecb3df2e35de2f3b8` |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         | `sha256:ee98a8a4e5ccd68ca0fe7c485a7595f4b62930ee2a13cc85e3c5486954a18c4c` |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              | `sha256:2bfa898d787863b7ec55421b8d21db7b2ba89c904a95705573a02bb43b2226de` |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       | `sha256:f5afefbd54a45418fbffa6f272e2dc8651fbd06276ce7d4ecf2e50ea1b947b12` |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. | `sha256:fc314d3e4729ec77b2cfdb1408d3aeed7f6d17b7e3c353e4cfc31fc9712eccd3` |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   | `sha256:102c47ff3b91c7106cf116f86dad5814a2d893672fa833d082d30ae500df8112` |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    | `sha256:75892d547cc35964fe079efd077e83825c38f43179bee4486e672113ff56d612` |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    | `sha256:e7cf6d4d0d7509c829a39cceac03f1f97e2f0f496bc1193d2291cac6ce08a007` |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        | `sha256:e7cf6d4d0d7509c829a39cceac03f1f97e2f0f496bc1193d2291cac6ce08a007` |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       | `sha256:6d9c790d7a322dd6dc56512d008055e72863b9fa5c01a5bd074de79227d45093` |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   | `sha256:acf24aca14e04a4120f9fd71c5eadd9e1f61e61c835e5482249dae2a1546ee02` |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   | `sha256:90767a1712dc74a9a3d1c73d5613c088d2d28034a2d8430e4cfd7062478dbd29` |


# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `1.13.0-amd64-<locale-and-voice>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

Versionshinweise für `1.12.0-amd64-<locale-and-voice>`:

**Feature**
* Führen Sie ein Upgrade auf die aktuellen Modelle durch.

Versionshinweise für `1.11.0-amd64-<locale-and-voice>`:

**Feature**
* Weitere Fehlerdetails zu Problemen beim Abrufen von benutzerdefinierten Modellen nach ID.

Versionshinweise für `1.9.0-amd64-<locale-and-voice>`:

* Regelmäßiges monatliches Release

Versionshinweise für `1.8.0-amd64-<locale-and-voice>`:

**Feature**

* Vollständig migriert zu .NET 3.1

Versionshinweise für `1.7.0-amd64-<locale-and-voice>`:

**Feature**

* Upgrade von Komponenten auf .NET 3.1

| Imagetags                                  | Notizen                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.13.0-amd64-<locale-and-voice>`           | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.13.0-amd64-en-us-ariarus`. |
| `1.12.0-amd64-<locale-and-voice>`           | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.12.0-amd64-en-us-ariarus`. |
| `1.11.0-amd64-<locale-and-voice>`           | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.11.0-amd64-en-us-ariarus`. |
| `1.9.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.9.0-amd64-en-us-ariarus`.  |
| `1.8.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.8.0-amd64-en-us-ariarus`.  |
| `1.7.0-amd64-<locale-and-voice>`            | 1\. allgemein verfügbare Version. Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.7.0-amd64-en-us-ariarus`.  |

| Gebietsschemas für v1.13.0                         | Notizen                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            | `sha256:8ff6360ba584d81b987582ce1c2cb6bb624cf68e4d71544805b9afc0401542dd` |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           | `sha256:da5037de95c00362cb1871374735778c3eb68640ae4cb6a260659e7e0a67c37e` |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            | `sha256:871140e57c126ac79c92c69572b86587150d1f14447c91152de3d4b10b3ef9f6` |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       | `sha256:7291ca9c579b1967cca941ce11321daa06ed6a9a1f0922d425d39f70a4aa8acd` |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           | `sha256:c8f34c3a7fc5af5141da5439b520614e039d133b6180e8157f12ec7279e9163a` |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        | `sha256:694eb294595700266355f8d57530ec3cccd4e04aa74dd630b96558bf2b481e71` |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         | `sha256:f875435d8fadb56df2123d5aa1ceca34990d00f4c75678eb2526b83058972717` |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         | `sha256:c58359bd6e6676e23dda181a86caee1771366b0329a44fae0f363bbd381058ad` |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:c8e615d40c6e96216b90e329bf7185060de646db1e92fd1fdcd344a52bd86b55` |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:c8e615d40c6e96216b90e329bf7185060de646db1e92fd1fdcd344a52bd86b55` |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   | `sha256:e8e3f04f0ee74d4247ffb7c69e54559f0cc6db66a121406e06ceb9dcdc3c4379` |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        | `sha256:15112a55bc7ccb6c29ee0a1de464fa6352a0e9953399032e5c8a0d29ec064af0` |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       | `sha256:9a77bb5451889f62b8a146bfcc4a412c1cef95fd2102650528ccee84a08b25b8` |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       | `sha256:90ee1094fbb8e739788545b3b9f4fabad5b4dffb5b7087cfd01c3b21ba1b2473` |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      | `sha256:43b7d3c87162129253fd5c150307a5d9dc6ea28b8fa19776b66f4aa7a546f43b` |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           | `sha256:75a4423d5b24136efdc5de28a7a5b50a3a09b65b3824f86dd50a95eefea7ead6` |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   | `sha256:87e926f7db4a27870c735c80ad801bc5480fb2665594727ae760c8c287677088` |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        | `sha256:3fbd6a824831f158762036aa41c0397f7c1148150a4dc045db5f19ba840e74b6` |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    | `sha256:646810c4129f8919ff56d91701b488e229bd12b3dd9c89a1635868f9340e00b8` |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            | `sha256:641abfa96380f142d4b2f9145cd02886d44f01bce68614094b48c1e01b50ed59` |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    | `sha256:c0acfffceae9c1ff5ad305d8b98929d9c65eca25f49ddcb8999d7de6118392d2` |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        | `sha256:fbdc9ef0b4308ffce87d6ff6854814804b3cafacad6c4dc5cdac6a47c6de7975` |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     | `sha256:f31c40c9db2f1e826686649e748d0b2be0c00abcac62c2aae5b8981b0d8c681d` |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      | `sha256:1232b798aae3ce68d1e555a5b35142bde5b4c871488f8c82c3d7c0767925afd8` |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         | `sha256:1232b798aae3ce68d1e555a5b35142bde5b4c871488f8c82c3d7c0767925afd8` |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     | `sha256:5fd7e9fbcc84ab467d04e95b18f5411579ce2d9a153b7f6e396f2412d08898dc` |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       | `sha256:5fbbd16ab58b7f2440778b258bb0cd966286de0dbb3ce7f5e54d0f244f63dd3f` |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         | `sha256:806b92916b2fe1e7855023a009742033a48cb7eddde84ddf7c93be93b9621026` |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       | `sha256:507d9f40dcb846a5d1511a5e9e1cf94b360b1d9922f4b1143c3146d1b3bc69a2` |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    | `sha256:594add691d03d02fa5925f817e6a25c091fac1a924e0ea4b626e0fce858a78cb` |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    | `sha256:09d288b58fea080689471618227d1cb3ccc467f2edc9477eaaffffb09b3d6d8b` |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        | `sha256:7019c80c88444a60bf1016eb66284745dc8184b051685df4a1b3c40d32c8ad7f` |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     | `sha256:eed46588733b884c330fff1ff7f4e3e3fd6416cb340ebd80e44c4b3d1e085e55` |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        | `sha256:00f7a854c4a01bdbef88e0b138c97f732f1c6008a8b2c1722fc8da3a91fa79a4` |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        | `sha256:5f32e838a0925c560d2961a42487b99dd7e79e04661a7711f905d36c55973fd6` |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     | `sha256:6f3d3237c990f8f04d4c8f488746f74fa94edd2c5f1def758af90b2be251900e` |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       | `sha256:282e2e48c1147b74d927e801534be52b1301a081ff881994e85bb9d85b6e85fb` |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     | `sha256:16370c22530c93fc6c5ebeaf10663de7c3d45db58eccc716abd5274b5bee56d3` |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    | `sha256:e6541e82b8555f748f1feb5eef1c0ebf884245c5448f0ced46e6f25dabb925a2` |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     | `sha256:a4cf0bab208a31da3e796bf353969dfd98184b30e0cf713df49cb4fb07ff568b` |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            | `sha256:4417d0a14098b564eb4ba91772eb7ad5976ac52b0b59ae484fc3a88017e0776b` |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          | `sha256:da086a3e2bc3e17f4e44165055fc61679e9356688d3735ee8cfd81e6265b8622` |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  | `sha256:0c9915bf34e3045e39aa245c597aa7223fbf6100d7e20cbcc1bf131f89ee785e` |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         | `sha256:0c9915bf34e3045e39aa245c597aa7223fbf6100d7e20cbcc1bf131f89ee785e` |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           | `sha256:fc08c968efe882ed11ad0ee0755a9d43eff88b96da8ec19e7a5c071810c84d8c` |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        | `sha256:b6ad73f07efd1576e166b4d7e54a4ff419bfedc513a175fbb968389eb289a4ee` |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          | `sha256:3aad5ccf0c155593934c29a3e50502bc80b0370fa29626e67cda141d4bf5ac89` |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   | `sha256:01502f274bad378e6e99bed5f80fdb476880ce04e8775ca56d338de2f2d43e8c` |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        | `sha256:fdc20724194612d99e8339d25c72c7fe937ad741abe46d86def6c62880913c2a` |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    | `sha256:abf0e442ec972e25743a8af55da49a6fd5bf2ffd6ca09619d68e4dc9f9db779a` |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       | `sha256:9eff152cd4bea6f9de3b101c0704f37c8a061e060287e3f9f8fc2eb28d7dcec7` |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   | `sha256:83aa3c569f7598843d4957f075915ac2635d3aaf577ac1158c12a1238dd7e148` |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        | `sha256:ea404c7857f9df0a23cbf3fac12ae00f11c32a6822d91078a321302f09f01082` |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          | `sha256:d4c15f7da8e03650395489b6cb6975d59322b1bbd2c59957617f0c0a297409ee` |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        | `sha256:cb2c0fb57513c66e00bd6b8cbb44882d5bb7d483c19784d2b1e09511d58842bc` |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        | `sha256:7b9a92ab8a9856f422e65b428b845571a059c0923dc1c348134f271ed7a4abe0` |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      | `sha256:cface74973368a78d75a2a079214aa748574c5f037b0c4189888269b6016f230` |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   | `sha256:cc3e74228002b8d4e7dc487ff6f930316ac5d7a93f97937942a23f41b484ba8c` |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      | `sha256:dca613867e2f559d9485f9ba553ecea3de6d4b2779d4eed0ce1e53e7f7939773` |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        | `sha256:791ac2b3100725f909cfeceb17fc0d5fd1022242db45ba455d7ea088d76ac033` |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          | `sha256:3b93df188bcbdf9416d203a7e30ade8908728316666cd3451a5f0320cdf219a9` |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    | `sha256:d2f636e35e67be196a4ad79f168e4df74d2f00d5b5c6123bd61f9aec72bfd1a7` |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    | `sha256:247a4c6025faced1be1738d816c1bb74b23bbc5d49458f9afe95dc32ab3ea71c` |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    | `sha256:355c3a0f64f003d0a041a757b8ddcdea8130b6a56a7c4003a68ba0412400c446` |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           | `sha256:55fff1cde012a7791c756104ba68a360e609a765bd776024a9f5f00199f568e5` |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            | `sha256:7f80965dde85e3a5aae9f69561c296d073289f0b6aa37e95ff0aa5192a5b7f90` |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       | `sha256:1bd43f513a5b2752c44a107e1898459cdda5d7267ec21f379679d411700e5189` |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        | `sha256:8062e2479a6a3dc17b8342c07a94a39dd1e1f788c1def0a1ab55a885b491bbab` |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          | `sha256:6ce345df654bd1db213c16c866b608037dcefb1d056fc14727db3b9e21437762` |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         | `sha256:9b9c8ad7f8621f887f3e9fda26f43995855dba76831fdf2598ef383cf3d20f39` |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         | `sha256:2e45f019df702d8788c1d9c20ff75cfd94aecaaf6facb9f41b642ef1bfe7d318` |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              | `sha256:3b142a414ff9f30ebef144e22bf979589600f226442d2f882384695795739178` |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       | `sha256:23b76501492c9b60e8888eda2f6b0258859f68ed6ff7fb49bacbb18cd5f542ed` |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. | `sha256:e9acc58168f6800d9dd11cbc569c9d279ecf28f3d17c702528d25f67edd447c9` |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   | `sha256:85e7d7ae77d41195de5102b772621ef34564d40fad224a0ed21a8fe8daf98b0f` |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    | `sha256:1fcba05138c0e5bf36447530311800e2d4044824b5d893439a12f3ebc6380135` |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    | `sha256:d02bd8759e085abbc95725aa4f70f124c4505aa0856a17696a1555b2cf64512e` |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        | `sha256:d02bd8759e085abbc95725aa4f70f124c4505aa0856a17696a1555b2cf64512e` |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       | `sha256:a3f68538088b5b07f4dc27239fa3a6308d949c2643638634c74f3ee132bca911` |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   | `sha256:bb0696685f3a90fe6898ff1487cb0c5957e02f3c63cdb7d02394b5c061339bf3` |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   | `sha256:1772b3bc8b166f429356b00d07ca438202c75d578b6d1655351b9c1e06ae1424` |

| Gebietsschemas für v1.12.0                         | Notizen                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            | `sha256:987e6b3e9e13570eb29117e87829a4905b35c712a0f36429dd6404793af31627` |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           | `sha256:7d1d3c337b7e3bdc6ae2b3e074828ffc3c64ffc0ab94abcb89896e623148d963` |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            | `sha256:cf01bea4f1f6b7112871da84fd82fb7e6de106c11cc933f21131385173f1da09` |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       | `sha256:d6060a1e16cbe40990677b3c46f14dc619ee6887d39a4af1cac51fba2baca9a9` |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           | `sha256:5033185bd60257033989fc4ff124c69b1dd02d5b99b79ff5c52ae84572095693` |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        | `sha256:ac9655166f8181db2d0e6684cc3a5b6e089da788f17c78067af2cf061c8db660` |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         | `sha256:90d222aa43c3efac04b9bc3e746b9ebea446cc16c3bdbb471b81065edfbc3023` |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         | `sha256:0c08c10f559c97eda9a0a3f8527f8b05810a53e8a3fd2b8e9f2ab35f587d6c46` |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:bf54713a1691f2378cf701a1f68ed0f4d32adeab25b2cbd9493f753d56d13e39` |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:bf54713a1691f2378cf701a1f68ed0f4d32adeab25b2cbd9493f753d56d13e39` |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   | `sha256:b94c79ace4b33bad944f88259da4dab5f52da7e78af85a8b6eee0e99ed05a387` |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        | `sha256:3b331be0a6eb32b12d5c6244691bd51ee1d6b218bd3dc066c0f9cb5b78864e14` |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       | `sha256:1bbbd1214119d2e02539f7bef8eeba48e86f17b968f2532a7d96e96ef40ecbe3` |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       | `sha256:aa0a38fd20cabcf33baa97b3a88f354d01055f57ed9376bf98b7ea0993333ffa` |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      | `sha256:57966c65522862572e07ba474fba7e2c6038091cc1b8a35861645dffc2fc5f5b` |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           | `sha256:57c6ff08057f199a8eb75668f8ddce26b92c87a7e01e9003b74339b98ea438c4` |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   | `sha256:89a8b8b8e900e6dbda665d245fd8a911d6e3286ee16a92e46f1993dc3667b631` |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        | `sha256:18347ce1c4e4e21180f64c27bb4bcbebbf52597e25db7e24dbeb57edcea56109` |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    | `sha256:015905bd42f8fb4ec575d971ff2d710ac5f904da2b84909270d3a7e51f5e3029` |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            | `sha256:4a490dcc6be935178761f14edbdd0c6e4036626046dbfeda002336d871c36fdc` |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    | `sha256:f26fb9b32ca82aa00c40f8824ed5d3d95ba1be5a10343e8649946d9468f9f74f` |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        | `sha256:43f5fffad77d3446bc08922df36e244115ecf6090e7c48c42281c2fa62d23b90` |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     | `sha256:0ca4a07585a61a6e15c7fd951b77bab6b5cf8934ecff65fe4ca6cfe8e47f351b` |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      | `sha256:00857cb570528dee93f7c9c7f96bb2e11763ff6aa9cc7405a05bcbad3d85b08d` |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         | `sha256:00857cb570528dee93f7c9c7f96bb2e11763ff6aa9cc7405a05bcbad3d85b08d` |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     | `sha256:3d7c911788bda58225a7100ba1a9afbb61e0a9f8b7633b383fe6e9faa48471d0` |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       | `sha256:251841a8399bd168644460e3ebf6d92f093dc8ea60f9defdc663a7e1f60515fa` |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         | `sha256:dbc6bb44b283902755907d9cee5694f880c95c6cf939f328059d826fefe53dfa` |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       | `sha256:9f11111e24b554d907d36516d130324d64a477b512cbd7faffa0b7d3895aa538` |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    | `sha256:04add8f669539cb2522237a1b01d263b30ed609332cd2ff6dcf2c88fcd24764a` |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    | `sha256:d375f7eea3592e041943a56ba18bec9ebc4bba1c99dea4d583f2012aee31cff7` |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        | `sha256:437e38d9cb97d2cee27890529eccc1d0b96622749c83844b89c50dc119176b61` |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     | `sha256:b6c0937fddd2e4d39a7cd96628a3d7d6004936f356cb553942e4f7dd48824b52` |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        | `sha256:5a359ab047d811996cccb9f3f95a59a7e023ee5be72ff0f509e7ebfeb0d3a07a` |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        | `sha256:439bab9f2933c73e52e78f1683a027e81a251c32fb8aa49b6cd8e7c9b2451f15` |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     | `sha256:ca798c5d25454b60cafca44f7f7e32896146966a8de94d00cced06235e38bf00` |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       | `sha256:e696a65a7c40209a8dd8d9ff59ca5334811e993f5b454f6d741ce0fc59258e07` |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     | `sha256:ab6e7c023ee6cef95f8dc4eeb3c804ea1b8af937cadb17efcc12e5b18adcfc69` |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    | `sha256:cb8f51f75a0b93baf6efb1624d7d01cd736926769922d61a63773eb3a1097399` |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     | `sha256:482aa2eb44f41294780cf299e6105a1a3105a2d8065233b34ef1837879f95b7f` |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            | `sha256:2f24ef0e620eeec3ea14262302d22cbb539a8afa85d356ffa446ca9cfd723b31` |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          | `sha256:0338f8e24eddb819c45909ec3a92c430b1d5ec1567a71495cc19c9a74382b224` |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  | `sha256:5d7e10ab0fd18d1d163c31341765b6f65bb198048424aa622b854172e845726d` |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         | `sha256:5d7e10ab0fd18d1d163c31341765b6f65bb198048424aa622b854172e845726d` |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           | `sha256:08d606969abd0165a798a8e0061e6439d4a33ad6af71aa58a1228e98018e7da9` |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        | `sha256:9613dbf91878054e2ab79d5d9c8f3686d5fe80b16c40d38db9aec3a2c3816555` |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          | `sha256:037ca355d8dcf9bff5fda9b9a4a9c2a54a03f3a48c378693c11437a36a245836` |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   | `sha256:647b92d1591501ed032d67cf2cfd719e95c24ffb624143d301c2b6dc5eed7397` |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        | `sha256:c35e40ffe1352870b9f177dcf70c1cd9eec9f22f92d35080fb5baa1fa65eac8d` |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    | `sha256:4fa1436d83439cc9672fe82e35f57a366d2c1a6eb1df1f9f9175d3a588b09610` |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       | `sha256:82f13a16e7812857143d311b5443cecfd7c199a88235728f437ba03e7cd92342` |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   | `sha256:565bfa8bab3a11608fd5fecae1a0cd655b4508404c354d5574af0e88ff1aec76` |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        | `sha256:2b9ab2e9d946e152b46a634ae291fedd220c76a7ba133346e80b4b19bcaa1422` |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          | `sha256:3a05e09241b43c149132b42079f486f0a076d493d4e4c7e4a56b8a030c5b55c7` |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        | `sha256:bb018c3c7d65c825c1755c510aca7f73f058ac4dce236dc114131c5699a1cb61` |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        | `sha256:eb2f7dc4db0981717b5fdd16c290ecb8135bd5ae409e0b569e3de34a9fb9f071` |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      | `sha256:098fabd9284caabafd4af526d52d5fa70ccbd0dc0e0c658753d7c644ab3bf813` |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   | `sha256:c7c033ef39c3da6c82ed1870e6796f501654403605268bcc8136cedd37c5ad1f` |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      | `sha256:2da1e4c972b47efd82a28b4a8324637d878b100bc730f90e9c9d16a6ccec75e9` |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        | `sha256:2f0ba437a2f7fbce9923a4da986aec53ec0ad3d52858e6aa12a7464cfa190240` |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          | `sha256:847e60ea915697dd038319a071757e095229ca0001bf05f1d922d4c52ff4b22a` |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    | `sha256:37914d4ed1a12d3999385592d5dc0c0ed11148f71f09e11a1bb4c9394691e3b7` |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    | `sha256:3a800cee6d1520a1c0502d9b682a7e0f98ef01de58bb39ea31573a9711ef1271` |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    | `sha256:70ad01c5cf6da459e0938c1da17348624e38d94b3ce4f22e181b9516262e961c` |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           | `sha256:8920e7acd70d6d550b66eb3c23878d070dc98219bd59fa8fce1abaf622da4c2f` |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            | `sha256:c17313ddab7e7d9c2777d4a19df65b34da4e30e52b4a21f81e5c59bacdfce979` |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       | `sha256:91315b4a62bbf69e117cdb4ef88facb02d3ee3d436a1e313af94ba6cb0b8608d` |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        | `sha256:d04761de48003062617397de4c4c5f448cd9b4bf57262587d245277d4e408431` |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          | `sha256:e41002cf7f56d948d2737adc23c0750b430d553d78abb2ac53c42427de971299` |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         | `sha256:5f556a0c113750d8780c09be8af7db28bc29784056d22389aec61c256ab9cbcb` |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         | `sha256:c893b27edd98c0760b7e510c365018e333aa0976ef742f7714ad59c92950a8e2` |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              | `sha256:bc34adc094183bbbc461e0350d7aa8e5140ece5e89cd9e77c60f2c96276037b2` |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       | `sha256:20b23d368f83d4b2926b6d8529d23c4dd84727bb063593d549fb959ce3ace8d2` |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. | `sha256:cb638e72c8966204ab9142810b94cf4c2da54f3fd5917ae0e12a11d28a4253bb` |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   | `sha256:041a22b054b0acf494ff3085cdb2cd2eb4faeb7b692027f1723d27c341a8ee33` |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    | `sha256:7d9d2766713507b04c0bf3332367e867524ff392b693f4eb8a8c003a4dfc3bac` |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    | `sha256:b6dfbdbc5ef0d91812d96c88393c0ae4835eea42dbba4c3d36ab9c5e806bb772` |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        | `sha256:b6dfbdbc5ef0d91812d96c88393c0ae4835eea42dbba4c3d36ab9c5e806bb772` |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       | `sha256:9802fc4a9656063cb9f215ca757db5289960d323244272ce280db0395ddd46ac` |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   | `sha256:05f50dffbeb17e4215a5a53cc0791d825b63bc1e2b007b00797e5d0e1b1d6d1e` |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   | `sha256:e96f4aecba6e3c0741218f3e1aec35e53147b12543be9fdcd76ff98d4c34cf84` |

| Gebietsschemas für v1.11.0                         | Notizen                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            | `sha256:7ba558f444ea482eca87b3e850e9b416c71391282b26a590d1ee3d9a81350188` |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           | `sha256:7f0afcc205340dea7ffd959812dcba6a11448f6c5c1ab55c1422a360bd876137` |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            | `sha256:fde80af0e2e8e49b49ddec5f1502a246cf308328738d6f572f0043e625673782` |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       | `sha256:fb2b50b128aa84ad0cd05db2462337d316ff2d2d78f393c5a9dece588a80654e` |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           | `sha256:9dde22e5e2164bee77aaf9fe4e8fc141d9dfbe3c92c4b07da969d34aa14f7fd0` |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        | `sha256:4a756cd10ad21dcc2b1c7006ec961f7e267f6d2204d9ad4efd6d4730d67a4ccc` |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         | `sha256:9d531c162c4279830f99ef0d44a506a023a0137723aab3adff7a663043a1c576` |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         | `sha256:353d07168b4a44fcc12a0239f5bf20e2d29365b9abe26b9b844fb6194e7c9bcc` |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   | `sha256:8e22964dc4b77c05f602f72b0e706a534a89a271c4d17b5117af122c34df9a18` |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        | `sha256:fcd6288d5fd4ddfe3d3e65e860895f6f7a7e81216c7113f71e7b1b01eb501150` |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       | `sha256:e49a5ec17b696a3a73d10383d369a2ff88ccddb812898a2eedefe6e6a009ce5a` |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       | `sha256:b7fb06bd992982c7e2e71da217898da45b742aab08e901bfcef9c43acf546bc0` |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      | `sha256:efd7d85845ca597937b8cbea7724cf31797855e0de5f30d66984ab9bac688152` |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           | `sha256:8211077d55b440dbb26e42db6322b35ef6ec88e8c2ec6647831e0046668ed8a4` |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   | `sha256:f6e924720b71d8f9a1edd4f5f2280e9054263eb79ce5364e03c9b802ad92f2dd` |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        | `sha256:de702f70c53e4c1647e5fdd3432d37dc8972e069fcc103a1fc2b0be70f0d6d71` |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    | `sha256:5077cb575ffeb64e3d70184a68259438821891f6c9865350d2f887ea43ee99c1` |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            | `sha256:c6f734cc12f04697a4d9b2003c46c5a4efd8c68da90838debb5628d9f8e70104` |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    | `sha256:f5a78e857bc1563cbcd74f7b856bc2e4bd981675b397aeccfa134137f1cd3392` |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        | `sha256:667729cafd6bf5afe071a0a2989f836943e3bb6d3d1ebe35b7fab9bb311bfebc` |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     | `sha256:e46533f972235f297dd31fd338638f5117e3f04fa4a434d678d1cecc76db023b` |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     | `sha256:53ee105977b6440f1a7fe5088255a9c6e437c39b7c66e5cd4aba984a1667b25c` |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       | `sha256:537d2018f414b825aa9995d2e15e0bdb0119e45f2c6fc10d326e3df6f49ef713` |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         | `sha256:05da3347d457ca040cbe9b3e3d586d298a844f906b34ef7b6d768c247274ff1f` |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       | `sha256:481cc43ba896a0d3291903af84120fa618130e2a2c8dce9b0ef23172b66858a8` |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    | `sha256:8cb9d071a1e01dc3e63d5f1b1c040aa6fee94488a5bbd60f2c91704abfd921cc` |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    | `sha256:da293ff5c49435c020044614962382040f41b6339ec83677301921a6dabbafb7` |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        | `sha256:9677d5bbbbe0c73df93948d4ecf3f367830ef9e7cfb3b42557cf94ec514b6c68` |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     | `sha256:a5109a6a659aa321892d4c6844e102ac72990fc2d58f32e45a072b291849fee8` |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        | `sha256:f8f1aa8168660ee1c21dfa4a92530bcba6f1aeb765cee9087a6cc29d7c332a8a` |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        | `sha256:450f0f75f26299a89a80efc3ce93b42d6447a32022aaf4f88edc935e56100191` |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     | `sha256:7b18adf90e6db8f8e2c5955f38aa0adfbdbd10a9a95e2cf13035b9c5416000e8` |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       | `sha256:ec3c238d0bfc3d26f20349ade1c4e19805b796f4bb3d5bf1fe4a9801b1ea1471` |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     | `sha256:7b13613a9c5260e03ed831c79e5538633b4201867068ca0e1624b2c39fa8cf39` |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    | `sha256:162c777447e3077438865332ac34df956be43c0429ce9962bcf5df9b210dbf01` |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     | `sha256:8cdf28dc31d40a69eb6720fd42b8c19792f973c4e58760abbb6573c6129c81c1` |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            | `sha256:3f9ec9201deca21f5e3e561d6dd673ee6fb2a7f13b4cae2985ffb69622994b99` |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          | `sha256:c6de645816587116384ada93c02257f257a13a4b696e1bd8aeecebb9a9668f15` |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           | `sha256:6ac24252194f91cd815736bd8be03fb95e0b965fabed5de4c631e99cd917da97` |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        | `sha256:bf20ea91d922beb682e321a31cabb11ebec474f47edcf4e3787882e2a204b3b5` |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          | `sha256:859bef31e5d882b508154ec00632e5e1e95bc8ea2dde6198f157703d759746c7` |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   | `sha256:b6c81ab4bd0aba217977b0bd83a8a65f7c09b5954cda0870dea15aec0dbbe1ed` |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        | `sha256:e216a1390a0d4d9f111c56c1d655f36614947eea18d6ec91a9f6d050048b1ad4` |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    | `sha256:ba2042523ea1fff9d2c8b805ac36075169c3aecce0c965d09e326c06eab5a36f` |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       | `sha256:fdbc8f59fc1c4b52c11d248ee9a5d7fe4e58343f036e558fbb33282e24d5b71f` |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   | `sha256:08ea0ed61ac152dc5caea2d4cacc81175c272cb4a835eecaa7f8e7c5485740b7` |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        | `sha256:40ff95e5fb92278e369b4f37d7dbb109431ecb115b1b9516aa887e6bb4fd030b` |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          | `sha256:70cfe68a81ee860136cfaed35909f522c28c20ef5514c2d9d96c283892f8b7f5` |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        | `sha256:9941cda0e65884900532e6a0ba68e475f373277105594bf09e67225450192d3c` |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        | `sha256:c71d980dfc70575421d1589c74e8b3e7cc036551412d0ad0f89dbc543252a405` |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      | `sha256:e5fbd98a70eb1dcf80c446b48b8f17e47ac12853bb255f0aed174c78196de257` |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   | `sha256:9f57f9847f2372fa341cf037410ac68ada1c3075ab9b77cffbcf01d199f7c1f5` |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      | `sha256:ef546532c582392e6ed47df55c0fbfa6dca6d3e523547089263b57354a4efb1a` |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        | `sha256:116aefb76ddf39bed379c023c8260d2607314ad1b31ddef83ec2818ad9805a0b` |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          | `sha256:6968fdefdd798adab48faeb40857c8cdca55712dbf4806703e11ccdfab874051` |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    | `sha256:48add20e3c147fb4be26c948841a12736c8b10d053aa7d25984df8e4016e939f` |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    | `sha256:ce5c055aedb3f9323f41a9de8d8f3dd23fb2ad0621d499f914f5cb3856e995f3` |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    | `sha256:badc02f9ccdee13ab7dbd4e178bd5c57d332cc3acd2d4a9a3f889d317e0517be` |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           | `sha256:763d4fe74b6f04a976482880eed76175854f659bb5bfcb315dce8ef69acead2e` |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            | `sha256:73374363f9b69e03b8b9de34b319d7797876a3dae40bdce0830a67cf4bb4d4f2` |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       | `sha256:317d6b5d69f56c9087cd1e8004e60a48841b997937dcdccc97e7c0b2e2ffb631` |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        | `sha256:d1aaad1d5f32a910e245e6c117178c0703d39035e4053fe2dd2bb646fc02f7b8` |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          | `sha256:0224ac3b2de11c4f6ef65ce0bdcd1b9c4112ea472b3bd5626fdff47a5185f54c` |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         | `sha256:16c7384bfe210f30e09eae3542a58ff9bdbfa9253fdf4d380a53b37809f82c7d` |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         | `sha256:5c7786c00a66346438ee4065e3eaa03ef9f8323ba839068344492b8a3b6d997a` |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              | `sha256:6925744597c45eed8761a9597f3525f435dd420b67ff775a73211fdef9cd9cb2` |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       | `sha256:b38a3f465062853b171d2bce6c6d8afa14d223e24bfd5ea0827e34c26a09a2c8` |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. | `sha256:fa9555e2f520340457d5cebe469af40516237fb9398a5f90046565655b2862f8` |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   | `sha256:d7eeca43e45d09a1c22611f865fb1f8b42673688a11a2acffd37a4e08a7fd8c4` |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    | `sha256:ee7257c0179fbe015324b4d29f16fe93964e5f1901906240477fb1d820a500f2` |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       | `sha256:263153fd6e05970e04af9a9bd95fb13591f0138ac030a632a6a78d95936afa4b` |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   | `sha256:b8289bb550b9328d83d6a7ec93bdf9524087222f537a55db0b2eb5402c2bf663` |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   | `sha256:af4bc0ef2211f69a92541bb14596341375e1003aef541aefcea7843192046b4c` |

| Gebietsschemas für v1.9.0                          | Notizen                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |

| Gebietsschemas für v.1.8.0                          | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |

| Gebietsschemas für v.1.7.0                          | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `de-ch-karsten`                             | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `de-de-hedda`                               | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `sl-si-lado`                                | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `th-th-pattara`                             | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |

---

## <a name="neural-text-to-speech"></a>Text-zu-Sprache (neuronal)

Das Containerimage [neuronale Sprachsynthese][sp-ntts] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `neural-text-to-speech`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list).


# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

Versionshinweise für `v1.8.0`: Regelmäßiges monatliches Release

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `1.8.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.8.0-amd64-en-us-arianeural`. |

| Gebietsschemas und Stimmen für v1.8.0           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-conradneural`                | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-ConradNeural`.    |
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-au-williamneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-WilliamNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-ca-liamneural`                  | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-LiamNeural`.      |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-gb-ryanneural`                  | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-RyanNeural`.      |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-alvaroneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-AlvaroNeural`.    |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `es-mx-jorgeneural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-JorgeNeural`.     |
| `fr-ca-antoineneural`               | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-AntoineNeural`.   |
| `fr-ca-jeanneural`                  | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-JeanNeural`.      |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `fr-fr-henrineural`                 | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HenriNeural`.     |
| `hi-in-madhurneural`                | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-MadhurNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-diegoneural`                 | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-DiegoNeural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `it-it-isabellaneural`              | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-IsabellaNeural`.  |
| `ja-jp-keitaneural`                 | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-KeitaNeural`.     |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-injoonneural`                | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-InJoonNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-antonioneural`               | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-AntonioNeural`.   |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `tr-tr-ahmetneural`                 | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-AhmetNeural`.     |
| `tr-tr-emelneural`                  | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-EmelNeural`.      |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |
| `zh-cn-xiaoyouneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoYouNeural`.   |
| `zh-cn-yunyangneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYangNeural`.   |
| `zh-cn-yunyeneural`                 | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYeNeural`.     |

# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

Versionshinweise für `v1.7.0`:
* Upgrade auf die neuesten Modelle mit Qualitätsverbesserungen und Fehlerbehebungen

Versionshinweise für `v1.6.0`:
* Upgrade auf die neuesten Modelle mit Qualitätsverbesserungen und Fehlerbehebungen

Versionshinweise für `v1.5.0`:
* Upgrade auf die neuesten Modelle mit Qualitätsverbesserungen und Fehlerbehebungen
* Unterstützung von bis zu 38 neuronalen Stimmen

Versionshinweise für `v1.4.0`:
* Führen Sie ein Upgrade auf die aktuellen Modelle durch. 
* Die CPU-Kosten und die Wartezeit wurden reduziert.
* Bessere Unterstützung der Satzrhythmusoptimierung mit SSML-Tag (beispielsweise Tonhöhenkontur).

Versionshinweise für `v1.3.0`:
* Der „Text-zu-Sprache (neuronal)“-Container ist jetzt allgemein verfügbar. 

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `1.5.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.5.0-amd64-en-us-arianeural`. |
| `1.4.0-amd64-<locale-and-voice>`            | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.4.0-amd64-en-us-arianeural`. |
| `1.3.0-amd64-<locale-and-voice>-preview`    | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.3.0-amd64-en-us-arianeural-preview`. |
| `1.2.0-amd64-<locale-and-voice>-preview`    | Ersetzen Sie `<locale>` durch eins der unten aufgeführten, verfügbaren Gebietsschemas. Beispiel: `1.2.0-amd64-en-us-arianeural-preview`. |

| Gebietsschemas und Stimmen für v1.7.0           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-conradneural`                | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-ConradNeural`.    |
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-au-williamneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-WilliamNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-ca-liamneural`                  | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-LiamNeural`.      |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-gb-ryanneural`                  | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-RyanNeural`.      |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-alvaroneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-AlvaroNeural`.    |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `es-mx-jorgeneural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-JorgeNeural`.     |
| `fr-ca-antoineneural`               | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-AntoineNeural`.   |
| `fr-ca-jeanneural`                  | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-JeanNeural`.      |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `fr-fr-henrineural`                 | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HenriNeural`.     |
| `hi-in-madhurneural`                | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-MadhurNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-diegoneural`                 | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-DiegoNeural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `it-it-isabellaneural`              | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-IsabellaNeural`.  |
| `ja-jp-keitaneural`                 | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-KeitaNeural`.     |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-injoonneural`                | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-InJoonNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-antonioneural`               | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-AntonioNeural`.   |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `tr-tr-ahmetneural`                 | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-AhmetNeural`.     |
| `tr-tr-emelneural`                  | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-EmelNeural`.      |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |
| `zh-cn-xiaoyouneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoYouNeural`.   |
| `zh-cn-yunyangneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYangNeural`.   |
| `zh-cn-yunyeneural`                 | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYeNeural`.     |

| Gebietsschemas und Stimmen für v1.6.0           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-conradneural`                | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-ConradNeural`.    |
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-au-williamneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-WilliamNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-ca-liamneural`                  | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-LiamNeural`.      |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-gb-ryanneural`                  | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-RyanNeural`.      |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-alvaroneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-AlvaroNeural`.    |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `es-mx-jorgeneural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-JorgeNeural`.     |
| `fr-ca-antoineneural`               | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-AntoineNeural`.   |
| `fr-ca-jeanneural`                  | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-JeanNeural`.      |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `fr-fr-henrineural`                 | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HenriNeural`.     |
| `hi-in-madhurneural`                | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-MadhurNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-diegoneural`                 | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-DiegoNeural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `it-it-isabellaneural`              | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-IsabellaNeural`.  |
| `ja-jp-keitaneural`                 | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-KeitaNeural`.     |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-injoonneural`                | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-InJoonNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-antonioneural`               | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-AntonioNeural`.   |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `tr-tr-ahmetneural`                 | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-AhmetNeural`.     |
| `tr-tr-emelneural`                  | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-EmelNeural`.      |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |
| `zh-cn-xiaoyouneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoYouNeural`.   |
| `zh-cn-yunyangneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYangNeural`.   |
| `zh-cn-yunyeneural`                 | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYeNeural`.     |

| Gebietsschemas und Stimmen für v1.5.0           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-conradneural`                | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-ConradNeural`.    |
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-au-williamneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-WilliamNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-ca-liamneural`                  | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-LiamNeural`.      |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-gb-ryanneural`                  | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-RyanNeural`.      |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-alvaroneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-AlvaroNeural`.    |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `es-mx-jorgeneural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-JorgeNeural`.     |
| `fr-ca-antoineneural`               | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-AntoineNeural`.   |
| `fr-ca-jeanneural`                  | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-JeanNeural`.      |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `fr-fr-henrineural`                 | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HenriNeural`.     |
| `hi-in-madhurneural`                | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-MadhurNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-diegoneural`                 | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-DiegoNeural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `it-it-isabellaneural`              | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-IsabellaNeural`.  |
| `ja-jp-keitaneural`                 | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-KeitaNeural`.     |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-injoonneural`                | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-InJoonNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-antonioneural`               | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-AntonioNeural`.   |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `tr-tr-ahmetneural`                 | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-AhmetNeural`.     |
| `tr-tr-emelneural`                  | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-EmelNeural`.      |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |
| `zh-cn-xiaoyouneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoYouNeural`.   |
| `zh-cn-yunyangneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYangNeural`.   |
| `zh-cn-yunyeneural`                 | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYeNeural`.     |

| Gebietsschemas und Stimmen für v1.4.0           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |
| `zh-cn-xiaoyouneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoYouNeural`.   |
| `zh-cn-yunyangneural`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYangNeural`.   |
| `zh-cn-yunyeneural`                 | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-YunYeNeural`.     |

| v1.3.0 Gebietsschemas und Stimmen           | Notizen                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-ca-claraneural`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-us-arianeural`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `en-us-jennyneural`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JennyNeural`.     |
| `es-es-elviraneural`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `hi-in-swaraneural`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Swaraneural`.     |
| `it-it-elsaneural`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |

| v1.2.0 Vorschau Gebietsschemas und Stimmen           | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `de-de-katjaneural-preview`                 | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `en-au-natashaneural-preview`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `en-ca-claraneural-preview`                 | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural-preview`                 | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `en-gb-mianeural-preview`                   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `en-us-arianeural-preview`                  | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `en-us-guyneural-preview`                   | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `es-es-elviraneural-preview`                | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural-preview`                 | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural-preview`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural-preview`                | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `it-it-elsaneural-preview`                  | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural-preview`                | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural-preview`                 | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural-preview`             | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural-preview`              | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |

---

## <a name="speech-language-detection"></a>Speech-Sprachenerkennung

Das Containerimage [Speech-Sprachenerkennung][sp-lid] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/speechservices/` und trägt den Namen `language-detection`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)

* Versionshinweise für Version `1.3.0`:
    * Unterstützung für eigenständige Sprach-IDs mit `SingleLanguage` und fortlaufendem Modus.

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.3.0-amd64-preview`                       |      |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `1.2.0-amd64-preview`                       |      |
| `1.1.0-amd64-preview`                       |      |

---

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Das Containerimage befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `keyphrase`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list).

# <a name="latest-version"></a>[Aktuelle Version](#tab/current)


| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Textsprachenerkennung

Das Containerimage [Sprachenerkennung][ta-la] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `language`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`.


Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list).

# <a name="latest-versions"></a>[Neueste Versionen](#tab/current)

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Vorherige Versionen](#tab/previous)


| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>Standpunktanalyse

Das Containerimage [Standpunktanalyse][ta-se] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `sentiment`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list).

| Imagetags | Notizen                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Standpunktanalyse v3 (Englisch)               |
| `3.0-es`   | Standpunktanalyse v3 (Spanisch)               |
| `3.0-fr`   | Standpunktanalyse v3 (Französisch)                |
| `3.0-it`   | Standpunktanalyse v3 (Italienisch)               |
| `3.0-de`   | Standpunktanalyse v3 (Deutsch)                |
| `3.0-zh`   | Standpunktanalyse v3 (Chinesisch, vereinfacht)  |
| `3.0-zht`  | Standpunktanalyse v3 (Chinesisch, traditionell) |
| `3.0-ja`   | Standpunktanalyse v3 (Japanisch)              |
| `3.0-pt`   | Standpunktanalyse v3 (Portugiesisch)            |
| `3.0-nl`   | Standpunktanalyse v3 (Niederländisch)                 |
| `2.1`    | Standpunktanalyse v2      |


## <a name="text-analytics-for-health"></a>Textanalyse für Gesundheit

Das Containerimage [Text Analytics for Health][ta-he] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/textanalytics/` und trägt den Namen `healthcare`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/textanalytics/healthcare`.

Dieses Containerimage verfügt über die folgenden Tags. Eine vollständige Liste der [Tags finden Sie auch in der MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/healthcare/tags/list).


Versionshinweise für `3.0.015490002-onprem-amd64`:

* neue Modellversion `2021-03-01`
* Für MCR freigegebener Container.

| Imagetags | Notizen                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0.015490002-onprem-amd64`   |               |


## <a name="translator"></a>Translator

Das Containerimage [Translator][tr-containers] befindet sich im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services/translator` und trägt den Namen `text-translation`. Der vollqualifizierte Name des Containerimages lautet `mcr.microsoft.com/azure-cognitive-services/translator/text-translation`.

Dieses Containerimage verfügt über die folgenden Tags.

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |


[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/containers/form-recognizer-container-install-run.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
[ta-he]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=healthcare
[tr-containers]: ../translator/containers/translator-how-to-install-container.md
