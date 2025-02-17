---
title: Verwenden direkter Methoden in Azure Video Analyzer – Azure
description: Azure Video Analyzer stellt mehrere direkte Methoden zur Verfügung. Die direkten Methoden basieren auf den in diesem Thema beschriebenen Konventionen.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 7b56404a371b0e50fe129d0fc5c2edb46b0c0f06
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122338973"
---
# <a name="azure-video-analyzer-direct-methods"></a>Direkte Methoden in Azure Video Analyzer

Live Video Analytics in IoT Edge `avaedge` stellt verschiedene direkte Methoden zur Verfügung, die von IoT Hub aufgerufen werden können. Direkte Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt. Dieser Ansatz eignet sich für Szenarien, in denen die Vorgehensweise bei sofortigen Aktionen unterschiedlich ist, je nachdem, ob das Gerät reagieren konnte oder nicht. Weitere Informationen finden Sie unter [Verstehen und Aufrufen direkter Methoden von IoT Hub](../../iot-hub/iot-hub-devguide-direct-methods.md).

In diesem Artikel werden diese Methoden, Konventionen und das Schema der Methoden beschrieben.

## <a name="conventions"></a>Konventionen

Die direkten Methoden basieren auf den folgenden Konventionen:

1. Direkte Methoden verfügen über eine im Format MAJOR.MINOR (Hauptversion.Nebenversion) angegebene Version (wie im folgenden Beispiel gezeigt). Dies ist die Zahl „@apiVersion“, wobei „1“ die HAUPT-Version und „0“ die NEBEN-Version in diesem Beispiel ist:

```
  {
    "methodName": "pipelineTopologySet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "{TopologyName}",
        "properties": {
            // Desired Topology properties
        }
    }
  }
```

2. Eine bestimmte Version des Video Analyzer-Moduls unterstützt alle Nebenversionen eines direkten Methodenaufrufs bis zu seiner aktuellen Version. Die Unterstützung für Hauptversionen ist nicht garantiert.
3. Alle direkten Methoden sind synchron.
4. Die Fehlerergebnisse basieren auf dem [OData-Fehlerschema](http://docs.oasis-open.org/odata/odata-json-format/v4.01/odata-json-format-v4.01.html#sec_ErrorResponse).
5. Bei Namen sollten die folgenden Einschränkungen beachtet werden:
    
    * Nur alphanumerische Zeichen und Bindestriche, sofern ein Name nicht mit einem Bindestrich beginnt oder endet
    * Keine Leerzeichen
    * Maximal 32 Zeichen

### <a name="example-of-response-from-a-direct-method"></a>Beispiel für eine Antwort von einer direkten Methode

```
-----------------------  Request: livePipelineList  --------------------------------------------------

{
  "@apiVersion": "1.0"
}

---------------  Response: livePipelineList - Status: 200  ---------------

{
  "value": []
}

--------------------------------------------------------------------------
```    
### <a name="error-codes"></a>Fehlercodes
Wie im folgenden Beispiel gezeigt, gibt es beim Erhalten einer Fehlerantwort von einer direkten Methode einen Fehlercode der obersten Ebene, und weitere Informationen finden Sie unter `details`.


```json
{
  "status": 400,
  "payload": {
    "error": {
      "code": "BadRequest",
      "message": "The request is invalid",
      "details": [
        {
          "code": "ApiVersionNotSupported",
          "message": "The API version '1.1' is not supported. Supported version(s): 1.0."
        }
      ]
    }
  }
}
```

Im Folgenden finden Sie die Fehlercodes, die auf der obersten Ebene verwendet werden.

|Status |Code   |`Message`|
|---|---|---|
|400|   BadRequest| Request is not valid (Anforderung ist ungültig)|
|400|   InvalidResource|    Die Ressource ist ungültig.|
|400|   InvalidVersion| Die API-Version ist ungültig.|
|402|   ConnectivityRequired    |Das Edge-Modul erfordert Cloudkonnektivität für die ordnungsgemäße Funktion.|
|404|   NotFound    |Ressource wurde nicht gefunden.|
|409|   Konflikt:|   Vorgangskonflikt|
|500|   InternalServerError|    Interner Serverfehler|
|503|   ServerBusy| Der Server ist ausgelastet.|

Im Folgenden finden Sie einige Fehlercodes, die auf Detailebene verwendet werden.

|Status|    Detailcode   |BESCHREIBUNG|
|---|---|---|
|400|   PipelineValidationError|    Allgemeine Pipeline-Fehler, wie etwa Zyklen oder Partitionierung usw.|
|400|   ModuleValidationError|  Modulspezifische Überprüfungsfehler.|
|409|   PipelineTopologyInUse|  Auf die Pipelinetopologie wird immer noch von einer oder mehreren Livepipelines verwiesen.|
|409|   OperationNotAllowedInState| Der angeforderte Vorgang kann im aktuellen Zustand nicht ausgeführt werden.|
|409|   ResourceValidationError|    Die referenzierte Ressource (Beispiel: Videoressource) befindet sich nicht in einem gültigen Zustand.|

## <a name="supported-direct-methods"></a>Unterstützte direkte Methoden  
Im Folgenden finden Sie die direkten Methoden, die vom Video Analyzer-Edgemodul verfügbar gemacht werden. Das Schema für die direkten Methoden finden Sie [hier](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/videoanalyzer/data-plane/VideoAnalyzer.Edge/preview/1.0.0/AzureVideoAnalyzerSdkDefinitions.json).

### <a name="pipelinetopologylist"></a>pipelineTopologyList

Diese direkte Methode listet alle Pipelinetopologien auf, die erstellt wurden.

#### <a name="request"></a>Anforderung

```
{
  "@apiVersion": "1.0"
}
```
#### <a name="response"></a>Antwort

```
{
  "status": 200,
  "value": [
    {
      "systemData": {
        "createdAt": "2021-05-05T14:19:22.16Z",
        "lastModifiedAt": "2021-05-05T16:20:41.505Z"
      },
      
      // first pipeline topology payload
      
    },
      "systemData": {
        "createdAt": "2021-05-06T14:19:22.16Z",
        "lastModifiedAt": "2021-05-06T16:20:41.505Z"
      },
      
      // next pipeline topology payload
      
    }    
  ]
}
```

#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode |
|--|--|
| Entität gefunden | 200 |
| Allgemeine Benutzerfehler | 400er Bereich |
| Entität nicht gefunden | 404 |
| Allgemeine Serverfehler | 500er Bereich |

### <a name="pipelinetopologyset"></a>pipelineTopologySet

Erstellt eine Pipelinetopologie mit dem angegebenen Namen, wenn keine solche Topologie vorhanden ist, oder aktualisiert eine vorhandene Topologie mit diesem Namen.

Schlüsselaspekte:

* Eine Pipelinetopologie kann kostenlos aktualisiert werden, wenn keine Livepipelines darauf verweisen.
* Eine Pipeline-Topologie kann frei aktualisiert werden, wenn alle auf sie verweisenden Livepipelines deaktiviert sind, sofern:

    * Neu hinzugefügte Parameter Standardwerte aufweisen
    * Auf entfernte Parameter von keiner Pipeline verwiesen wird
* Nur einige Pipelinetopologieupdates sind zulässig, während eine Livepipeline aktiv ist.

#### <a name="request"></a>Anforderung

```
  {
    "methodName": "pipelineTopologySet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "{TopologyName}",
        "properties": {
            // Desired pipeline topology properties
        }
    }
  }
```

#### <a name="response"></a>Antwort

```
  {
    "status": 201,
    "payload": {
        "systemData": {
           "createdAt": "2021-05-11T18:16:46.491Z",
           "lastModifiedAt": "2021-05-11T18:16:46.491Z"
        },
        "name": "{TopologyName}",
        "properties": {
            // Complete pipeline topology
        }
    }
  }
```
#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
Vorhandene Entität aktualisiert |200|   –|
Neue Entität erstellt  |201|   –|
Allgemeine Benutzerfehler |400er Bereich  ||
Pipelineüberprüfungsfehler  |400    |PipelineValidationError|
Modulüberprüfungsfehler|   400 |ModuleValidationError|
Allgemeine Serverfehler   |500er Bereich  ||

### <a name="pipelinetopologyget"></a>pipelineTopologyGet

Ruft eine Pipelinetopologie mit dem angegebenen Namen ab, sofern vorhanden.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0",
        "name": "{TopologyName}"       
  }
```
#### <a name="response"></a>Antwort

```
{
  "status": 200,
  "payload": {
    "value": [
      {
        "systemData": {
          "createdAt": "2021-05-06T10:28:04.560Z",
          "lastModifiedAt": "2021-05-06T10:28:04.560Z"
        },
        "name": "{TopologyName}",
        // Complete pipeline topology
      }
    ]
  }
}
```

#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode |
|--|--|
| Erfolg | 200 |
| Allgemeine Benutzerfehler | 400er Bereich |
| Allgemeine Serverfehler | 500er Bereich |

### <a name="pipelinetopologydelete"></a>pipelineTopologyDelete

Löscht eine einzelne Pipelinetopologie.

* Beachten Sie, dass keine Livepipelines vorhanden sind, die [auf eine gelöschte Pipelinetopologie verweisen](pipeline.md#pipeline-states). Wenn solche Livepipelines vorhanden sind, erhalten Sie einen `TopologyInUse` Fehler.

#### <a name="request"></a>Anforderung

```
  {
    "methodName": "pipelineTopologyDelete",
    "payload": {
        "@apiVersion": "1.0",
        "name": "{TopologyName}"
    }
  }
```
#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```

#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode | Detaillierter Fehlercode |
|--|--|--|
| Entität gelöscht | 200 | – |
| Entität nicht gefunden | 204 | – |
| Allgemeine Benutzerfehler | 400er Bereich |  |
| Auf die Pipelinetopologie wird immer noch von einer oder mehreren Pipelines verwiesen | 409 | PipelineTopologyInUse |
| Allgemeine Serverfehler | 500er Bereich |  |

### <a name="livepipelinelist"></a>livePipelineList

Listet alle Livepipelines auf.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0"
  }
```
#### <a name="response"></a>Antwort

```
{
  "status": 200,
  "value": [
    {
      "systemData": {
        "createdAt": "2021-05-05T14:19:22.16Z",
        "lastModifiedAt": "2021-05-05T16:20:41.505Z"
      },      
      // first live pipeline payload  
    },
      "systemData": {
        "createdAt": "2021-05-06T14:19:22.16Z",
        "lastModifiedAt": "2021-05-06T16:20:41.505Z"
      },
      // next live pipeline payload
    }    
  ]
}
```
#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode |
|--|--|
| Entität gefunden | 200 |
| Allgemeine Benutzerfehler | 400er Bereich |
| Entität nicht gefunden | 404 |
| Allgemeine Serverfehler | 500er Bereich |

### <a name="livepipelineset"></a>livePipelineSet

Erstellt eine Livepipeline mit dem angegebenen Namen, wenn keine solche Livepipeline vorhanden ist, oder aktualisiert eine vorhandene Livepipeline mit diesem Namen.

Schlüsselaspekte:

* Eine Livepipeline kann frei aktualisiert werden, während sie sich im Zustand „Deaktiviert“ befindet. In anderen Zuständen sind nur einige Updates zulässig.
* Eine Livepipeline wird bei jedem Update erneut validiert.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0",
        "name": "{livePipelineName}",
        "properties": {
            // Desired live pipeline properties
        }
  }
```
#### <a name="response"></a>Antwort

````
  {
    "status": 201,
    "payload": {
        "systemData": {
           "createdAt": "2021-05-11T18:16:46.491Z",
           "lastModifiedAt": "2021-05-11T18:16:46.491Z"
        },
        "name": "{livePipelineName}",
        "properties": {
            // Complete live pipeline
        }
    }
  }
````
#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode | Detaillierter Fehlercode |
|--|--|--|
| Vorhandene Entität aktualisiert | 200 | – |
| Neue Entität erstellt | 201 | – |
| Allgemeine Benutzerfehler | 400er Bereich | – |
| Livepipelineüberprüfungsfehler | 400 | PipelineValidationError |
| Modulüberprüfungsfehler | 400 | ModuleValidationError |
| Ressourcenüberprüfungsfehler | 409 | ResourceValidationError |
| Allgemeine Serverfehler | 500er Bereich | – |

### <a name="livepipelinedelete"></a>livePipelineDelete

Löscht eine einzelne Livepipeline.

Schlüsselaspekte:

* Nur deaktivierte Livepipelines können gelöscht werden.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0",
        "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Antwort

```
  {
    "status": 200,
    "payload": null
  }
```
#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode | Detaillierter Fehlercode |
|--|--|--|
| Livepipeline erfolgreich gelöscht | 200 | – |
| Livepipeline nicht gefunden | 204 | – |
| Allgemeine Benutzerfehler | 400er Bereich |  |
| Pipeline befindet sich nicht im Status „Deaktiviert“ | 409 | OperationNotAllowedInState |
| Allgemeine Serverfehler | 500er Bereich |  |

### <a name="livepipelineget"></a>livePipelineGet

Dies ist ähnlich wie liveTopologyGet. Ruft eine Livepipeline mit dem angegebenen Namen ab, sofern vorhanden.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0",
        "name": "{livePipelineName}"       
  }
```
#### <a name="response"></a>Antwort

```
{
  "status": 200,
  "payload": {
    "value": [
      {
        "systemData": {
          "createdAt": "2021-05-06T10:28:04.560Z",
          "lastModifiedAt": "2021-05-06T10:28:04.560Z"
        },
        "name": "{livePipelineName}",
        // Complete live pipeline
      }
    ]
  }
}
```

#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode |
|--|--|
| Erfolg | 200 |
| Allgemeine Benutzerfehler | 400er Bereich |
| Allgemeine Serverfehler | 500er Bereich |

### <a name="livepipelineactivate"></a>livePipelineActivate

Aktiviert eine Livepipeline. 

Schlüsselaspekte

* Die Methode gibt zurück, wenn sich die Livepipeline im Status „Wird aktiviert“ oder „Aktiviert“ befindet. 
* Ein List/Set-Vorgang für die Livepipeline würde den aktuellen Zustand zurückgeben.
* Diese Methode kann aufgerufen werden, solange sich die Livepipeline nicht im (vorübergehenden) „Wird deaktiviert“-Zustand befindet.
* Idempotenz:
    * Das Aufrufen von für eine Livepipeline, die sich bereits im Zustand „Wird aktiviert“ befindet, verhält sich genauso wie wenn die `livePipelineActivate` Livepipeline deaktiviert wurde.
    * Wenn Sie eine Pipeline aktivieren, die sich im Status „Aktiv“ befindet, wird sofort ein Erfolgscode zurückgegeben.

#### <a name="request"></a>Anforderung

```
  {
        "@apiVersion": "1.0",
        "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```
#### <a name="status-codes"></a>Statuscodes

| Bedingung | Statuscode | Detaillierter Fehlercode |
|--|--|--|
| Pipeline erfolgreich aktiviert | 200 | – |
| Neue Entität erstellt | 201 | – |
| Allgemeine Benutzerfehler | 400er Bereich |  |
| Modulüberprüfungsfehler | 400 | ModuleValidationError |
| Ressourcenüberprüfungsfehler | 409 | ResourceValidationError |
| Die Livepipeline wird deaktiviert | 409 | OperationNotAllowedInState |
| Allgemeine Serverfehler | 500er Bereich |  |

### <a name="livepipelinedeactivate"></a>livePipelineDeactivate

Deaktiviert eine Livepipeline. Durch das Deaktivieren einer Pipeline wird die Videoverarbeitung deaktiviert und sichergestellt, dass alle Ereignisse und Videos, die am Edge zwischengespeichert werden, in die Cloud übertragen werden, sofern dies möglich ist.

Schlüsselaspekte:

* Die Methode gibt nur zurück, wenn die Pipeline deaktiviert ist.
* Diese Methode kann aufgerufen werden, solange sich die Livepipeline nicht im (vorübergehenden) „Wird aktiviert“-Zustand befindet.
* Die Pipeline nimmt den Zustand „Wird deaktiviert“ an, während die Deaktivierung erfolgt.
    * Ein List/Set-Vorgang für die Livepipeline würde den aktuellen Zustand zurückgeben.
    * Die Deaktivierung wird abgeschlossen, wenn alle Medien in die Cloud hochgeladen wurden (wenn die Pipeline über einen Knoten der [Videosenke](pipeline.md#video-sink) verfügt).
* Idempotenz:
    * Das Aufrufen von `livePipelineDeactivate` für eine Meerpipeline, die sich bereits im Status „Wird deaktiviert“ befindet, verhält sich genauso wie wenn die im „Aktiv“-Zustand wäre.
    * Wenn Sie eine Pipeline deaktivieren, die sich im Status „Aktiv“ befindet, wird sofort ein Erfolgscode zurückgegeben.


#### <a name="request"></a>Anforderung

```
  {
    "@apiVersion": "1.0",
    "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Antwort

```
{
 "status": 200,
 "payload": null
}
```
| Bedingung | Statuscode | Detaillierter Fehlercode |
|--|--|--|
| Pipeline erfolgreich aktiviert | 200 | – |
| Neue Entität erstellt | 201 | – |
| Allgemeine Benutzerfehler | 400er Bereich |  |
| Der Pipeline befindet sich im Zustand „Wird aktiviert“ | 409 | OperationNotAllowedInState |
| Allgemeine Serverfehler | 500er Bereich |  |

## <a name="next-steps"></a>Nächste Schritte

[Konfigurationsschema für Modulzwillinge](module-twin-configuration-schema.md)
