---
title: Datei einfügen
description: include file
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 52082c22053a5a54a4462fb01012bdceff848a96
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113364933"
---
## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA-Treiber

NVIDIA CUDA-Treiber für VMs der Serien NC, NCv2, NCv3, ND und NDv2 (optional für NV-Serie) werden nur unter den in der folgenden Tabelle aufgeführten Linux-Distributionen unterstützt. Die CUDA-Treiberinformationen sind zum Zeitpunkt der Veröffentlichung aktuell. Informationen zu den neuesten CUDA-Treibern und unterstützten Betriebssystemen finden Sie auf der [NVIDIA](https://developer.nvidia.com/cuda-zone)-Website. Stellen Sie sicher, dass Sie eine Installation der bzw. ein Upgrade auf die neuesten CUDA-Treiber für Ihre Distribution ausgeführt haben. 

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Linux-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Ubuntu 16.04 LTS oder CentOS 7.4 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.


### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle Computer der NV- und NVv3-Serie, die als virtuelle Arbeitsstationen oder für virtuelle Anwendungen verwendet werden. Installieren Sie nur diese GRID-Treiber auf Azure NV-VMs, und zwar nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten. Sie müssen keinen NVIDIA vGPU-Softwarelizenzserver einrichten.

Die von Azure neu verteilten GRID-Treiber funktionieren nicht auf Nicht-NV-Serien-VMs wie NC, NCv2, NCv3, ND sowie NDv2-Serien-VMs.

|Distribution|Treiber|
| --- | -- |
|Ubuntu 18.04 LTS<br/><br/>Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.7 bis 7.9, 8.0, 8.1<br/><br/>SUSE Linux Enterprise Server 12 SP2 <br/><br/>SUSE Linux Enterprise Server 15 SP2 | NVIDIA GRID 12.2, Treiberbranch [R460](https://go.microsoft.com/fwlink/?linkid=874272) (EXE-Datei)|

Eine vollständige Liste aller vorherigen NVIDIA GRID-Treiber finden Sie auf [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json).

> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>
