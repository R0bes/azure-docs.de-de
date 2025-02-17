---
title: 'DCsv2-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für VMs der DCsv2-Serie.
author: mmcrey
ms.service: virtual-machines
ms.subservice: vm-sizes-general
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: jushiman
ms.openlocfilehash: 0d37e8308afd75b310893272f338fb5a3245a8ce
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2021
ms.locfileid: "122696521"
---
# <a name="dcsv2-series"></a>DCsv2-Serie

**Gilt für**: :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen :heavy_check_mark: Einheitliche Skalierungsgruppen

Die virtuellen Computer der DCsv2-Serie können Ihnen beim Schutz der Vertraulichkeit und Integrität Ihrer Daten und Ihres Codes während der Verarbeitung in der öffentlichen Cloud helfen. Die DCsv2-Serie nutzt die Software Guard Extensions von Intel®, die es Kunden ermöglicht, Secure Enclaves für den Schutz zu verwenden.

Diese Computer werden von 3,7 GHz Intel® Xeon E-2288G (Coffee Lake) mit SGX-Technologie unterstützt. Mit der Intel® Turbo Boost Max Technology 3.0 können diese Computer bis zu 5,0 GHz erreichen. 

Beispiele für Anwendungsfälle sind Freigabe von vertraulichen Mehrparteiendaten, Betrugserkennung, vertrauliche Datenbanken, Geldwäschebekämpfung, Blockchain, vertrauliche Nutzungsanalyse, Business Intelligence-Analyse und vertrauliches maschinelles Lernen.

## <a name="configuration"></a>Konfiguration

[Turbo Boost Max 3.0](https://www.intel.com/content/www/us/en/gaming/resources/turbo-boost.html): Unterstützt (Mandanten-VM gibt 3,7 GHz an, erreicht aber Turbo-Geschwindigkeiten)<br>
[Hyperthreading](https://www.intel.com/content/www/us/en/gaming/resources/hyper-threading.html): Nicht unterstützt<br>
[Storage Premium](premium-storage-performance.md): Unterstützt (Für Standard_DC8_v2 nicht unterstützt)<br>
[Storage Premium-Zwischenspeicherung](premium-storage-performance.md): Unterstützt<br>
[Livemigration:](maintenance-and-updates.md) Nicht unterstützt<br>
[Updates mit Speicherbeibehaltung:](maintenance-and-updates.md) Nicht unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 2<br>
[Beschleunigter Netzwerkbetrieb:](../virtual-network/create-vm-accelerated-networking-cli.md) Nicht unterstützt<br>
[Kurzlebige Betriebssystemdatenträger](ephemeral-os-disks.md): Unterstützt <br>

## <a name="technical-specifications"></a>Technische Spezifikationen

| Size             | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) | EPC-Speicher (MiB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2\.000/16                                                                                               | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4\.000/32                                                                                               | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8\.000/64                                                                                               | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16.000/128                                                                                         | 2   | 168                                         |


## <a name="get-started"></a>Erste Schritte

- Erstellen Sie DCsv2-VMs mithilfe von [Azure-Portal](./linux/quick-create-portal.md) oder [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview).
- Virtuelle Computer der DCsv2-Serie sind [VMs der Generation 2](./generation-2.md#creating-a-generation-2-vm) und unterstützen nur `Gen2`-Images.
- Derzeit verfügbar in den Regionen, die unter [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all) aufgeführt sind.
- Nächste Generation von VMs der DC-Serie: [Nehmen Sie am Vorschauprogramm teil](https://aka.ms/intelgen3)

## <a name="more-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)
- [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
- [Weitere Informationen zu Datenträgertypen](./disks-types.md#ultra-disk)

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.