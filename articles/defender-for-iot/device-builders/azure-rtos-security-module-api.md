---
title: API für den Defender-IoT-Micro-Agent für Azure RTOS
description: Referenz-API für den Defender-IoT-Micro-Agent für Azure RTOS
ms.topic: reference
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: e7000a7e6d8ba332432f1ececa12bd9543e9e4a7
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113016149"
---
# <a name="defender-iot-micro-agent-for-azure-rtos-api-preview"></a>API für den Defender-IoT-Micro-Agent für Azure RTOS (Vorschau)

Diese API ist ausschließlich für die Verwendung mit dem Defender-IoT-Micro-Agent für Azure RTOS vorgesehen. Zusätzliche Ressourcen finden Sie in der [GitHub-Ressource zum Defender-IoT-Micro-Agent für Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases). 

## <a name="enable-defender-iot-micro-agent-for-azure-rtos"></a>Aktivieren des Defender-IoT-Micro-Agents für Azure RTOS

**nx_azure_iot_security_module_enable**

### <a name="prototype"></a>Prototyp

```c
UINT nx_azure_iot_security_module_enable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschreibung

Diese Routine aktiviert das Defender-IoT-Micro-Agent-Subsystem von Azure IoT. Ein interner Zustandsautomat verwaltet die Sammlung von Sicherheitsereignissen und sendet sie an Azure IoT Hub. Nur eine NX_AZURE_IOT_SECURITY_MODULE-Instanz ist erforderlich, um die Datensammlung zu verwalten.

### <a name="parameters"></a>Parameter

| Name | Beschreibung |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Ein Zeiger auf ein `NX_AZURE_IOT`.  |

### <a name="return-values"></a>Rückgabewerte

|Rückgabewerte  |Beschreibung |
|---------|---------|
|NX_AZURE_IOT_SUCCESS|   Azure IoT-Sicherheitsmodul erfolgreich aktiviert.     |
|NX_AZURE_IOT_FAILURE   |  Fehler beim Aktivieren des Azure IoT-Sicherheitsmoduls wegen eines internen Fehlers.    |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Das Sicherheitsmodul erfordert eine gültige #NX_AZURE_IOT-Instanz.      |

### <a name="allowed-from"></a>Zulässig von

Threads

## <a name="disable-azure-iot-defender-iot-micro-agent"></a>Deaktivieren des Azure IoT Defender-IoT-Micro-Agents

**nx_azure_iot_security_module_disable**


### <a name="prototype"></a>Prototyp

```c
UINT nx_azure_iot_security_module_disable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschreibung

Diese Routine deaktiviert das Defender-IoT-Micro-Agent-Subsystem von Azure IoT.

### <a name="parameters"></a>Parameter

| Name | Beschreibung |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Ein Zeiger auf `NX_AZURE_IOT`. Wenn NULL, wird die Singletoninstanz deaktiviert. |

### <a name="return-values"></a>Rückgabewerte

|Rückgabewerte  |Beschreibung |
|---------|---------|
|NX_AZURE_IOT_SUCCESS     |   Erfolgreich, wenn das Azure IoT-Sicherheitsmodul erfolgreich deaktiviert wurde.      |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Azure IoT Hub-Instanz unterscheidet sich von der zusammengesetzten Singletoninstanz.       |
|NX_AZURE_IOT_FAILURE    |  Fehler beim Deaktivieren des Azure IoT-Sicherheitsmoduls wegen eines internen Fehlers.       |

### <a name="allowed-from"></a>Zulässig von

Threads


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den ersten Schritten mit dem Defender-IoT-Micro-Agent für Azure RTOS finden Sie in den folgenden Artikeln:

- Lesen Sie die [Übersicht](iot-security-azure-rtos.md) zum Defender-IoT-Micro-Agent für Azure RTOS.
