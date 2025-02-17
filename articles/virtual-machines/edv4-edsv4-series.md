---
title: Edv4- und Edsv4-Serie
description: Hier finden Sie Spezifikationen für VMs der Serie Ev4, Edv4, Esv4 und Edsv4.
author: brbell
ms.author: brbell
ms.reviewer: jushiman
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 02/04/2020
ms.openlocfilehash: 3aca50f175a9925861b8eb59cad0efd78051f6cc
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349748"
---
# <a name="edv4-and-edsv4-series"></a>Edv4- und Edsv4-Serie

Die Edv4- und Edsv4-Serie läuft auf Intel-Prozessoren des Typs &reg; Xeon&reg; Platinum 8272CL (Cascade Lake) in einer Hyperthreadkonfiguration. Sie eignet sich ideal für verschiedene arbeitsspeicherintensive Unternehmensanwendungen und bietet bis zu 504 GiB RAM, [Intel&reg; Turbo Boost Technology 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading Technology](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) and [Intel&reg; Advanced Vector Extensions 512 (Intel&reg; AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). Außerdem unterstützen Sie [Intel&reg; Deep Learning Boost](https://software.intel.com/content/www/us/en/develop/topics/ai/deep-learning-boost.html). Diese neuen VM-Größen bieten 50 Prozent mehr lokalen Speicher sowie bessere IOPS auf lokalen Datenträgern für Lese- und Schreibvorgänge im Vergleich zu den Größen [Ev3/Esv3](./ev3-esv3-series.md) mit [Gen2-VMs](./generation-2.md). Sie verfügen über eine Turbo-Taktfrequenz von 3,4 GHz für alle Kerne. 

## <a name="edv4-series"></a>Edv4-Serie

Die Edv4-Serie läuft auf Intel-Prozessoren des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Edv4-VMs bieten bis zu 504 GiB RAM sowie schnellen und großen lokalen SSD-Speicher (bis zu 2.400 GiB). Diese VMs eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen und Anwendungen, die von lokalem Hochgeschwindigkeitsspeicher mit geringer Latenz profitieren. An Edv4-VMs können Sie SSD Standard- und HDD Standard-Datenträger anfügen. 

[ACU](acu.md): 195–210<br>
[Storage Premium](premium-storage-performance.md): Nicht unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Nicht unterstützt<br>
[Livemigration](maintenance-and-updates.md): Unterstützt<br>
[Updates mit Speicherbeibehaltung](maintenance-and-updates.md): Unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 1 und 2<br>
[Beschleunigter Netzwerkbetrieb:](../virtual-network/create-vm-accelerated-networking-cli.md) Unterstützt<sup>1</sup> <br>
[Kurzlebige Betriebssystemdatenträger:](ephemeral-os-disks.md) Nicht unterstützt <br>
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | <sup>**</sup> Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_E2d_v4<sup>1</sup>  | 2 | 16 | 75 | 4 | 19.000/120 | 2|1000 |
| Standard_E4d_v4  | 4 | 32 | 150 | 8 | 38.500/242 | 2|2000 |
| Standard_E8d_v4 | 8 | 64 | 300 | 16 | 77.000/485 | 4|4000 |
| Standard_E16d_v4 | 16 | 128 | 600 | 32 | 154.000/968 | 8|8.000 |
| Standard_E20d_v4 | 20 | 160 | 750 | 32 | 193.000/1.211  | 8|10000 |
| Standard_E32d_v4 | 32 | 256 | 1200 | 32 | 308.000/1.936 | 8|16000 |
| Standard_E48d_v4 | 48 | 384 | 1800 | 32 | 462.000/2.904 | 8|24.000 |
| Standard_E64d_v4 | 64 | 504 | 2400 | 32 | 615.000/3.872 | 8|30.000 |

<sup>1</sup> Der beschleunigte Netzwerkbetrieb kann nur auf eine einzelne NIC angewendet werden. <br>
<sup>**</sup> Diese IOPS-Werte können mithilfe von [Gen2-VMs](generation-2.md) erzielt werden.

## <a name="edsv4-series"></a>Edsv4-Serie

Die Edsv4-Serie läuft auf Intel-Prozessoren des Typs Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Edsv4-VMs bieten bis zu 504 GiB RAM sowie schnellen und großen lokalen SSD-Speicher (bis zu 2.400 GiB). Diese VMs eignen sich ideal für arbeitsspeicherintensive Unternehmensanwendungen und Anwendungen, die von lokalem Hochgeschwindigkeitsspeicher mit geringer Latenz profitieren.

[ACU](acu.md): 195-210<br>
[Storage Premium](premium-storage-performance.md): Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Livemigration](maintenance-and-updates.md): Unterstützt<br>
[Updates mit Speicherbeibehaltung](maintenance-and-updates.md): Unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 1 und 2<br>
[Beschleunigter Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md): Unterstützt <br>
[Kurzlebige Betriebssystemdatenträger](ephemeral-os-disks.md): Unterstützt <br>
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | <sup>**</sup> Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Durchsatz des Datenträgers mit maximalem Burst ohne Cache: IOPS/MBit/s<sup>1</sup> | Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|---|---|
| Standard_E2ds_v4<sup>4</sup>  | 2 | 16 | 75 | 4 | 19.000/120(50) | 3200/48 | 4000/200 | 2|1000 |
| Standard_E4ds_v4  | 4 | 32 | 150 | 8 | 38.500/242(100) | 6400/96 | 8000/200 | 2|2000 |
| Standard_E8ds_v4 | 8 | 64 | 300 | 16 | 77.000/485(200) | 12800/192 | 16000/400 | 4|4000 |
| Standard_E16ds_v4 | 16 | 128 | 600 | 32 | 154.000/968(400) | 25600/384 | 32000/800 | 8|8.000 |
| Standard_E20ds_v4 | 20 | 160 | 750 | 32 | 193.000/1.211(500)  | 32000/480 | 40000/1000 | 8|10000 |
| Standard_E32ds_v4 | 32 | 256 | 1200 | 32 | 308.000/1.936(800) | 51200/768  | 64000/1600 | 8|16000 |
| Standard_E48ds_v4 | 48 | 384 | 1800 | 32 | 462.000/2.904(1.200) | 76800/1152 | 80.000/2.000 | 8|24.000 |
| Standard_E64ds_v4 <sup>2</sup> | 64 | 504 | 2400 | 32 | 615.000/3.872(1.600) | 80000/1200 | 80.000/2.000 | 8|30.000 |
| Standard_E80ids_v4 <sup>3</sup> | 80 | 504 | 2400 | 32 | 615.000/3.872(1.600) | 80000/1200 | 80.000/2.000 | 8|30.000 |

<sup>**</sup> Diese IOPS-Werte können mithilfe von [Gen2-VMs](generation-2.md) garantiert werden.<br>
<sup>1</sup> VMs der Edsv4-Serie können mit einem [Burst](./disk-bursting.md) ihre Datenträgerleistung für jeweils bis zu 30 Minuten auf das maximale Bursting verbessern.<br>
<sup>2</sup> [Eingeschränkte Kerngrößen verfügbar](./constrained-vcpu.md)<br>
<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.<br>
<sup>4</sup> Der beschleunigte Netzwerkbetrieb kann nur auf eine einzelne NIC angewendet werden. 



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

Preisrechner: [Preisrechner](https://azure.microsoft.com/pricing/calculator/)

Weitere Informationen zu Datenträgertypen: [Datenträgertypen](./disks-types.md#ultra-disk)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
