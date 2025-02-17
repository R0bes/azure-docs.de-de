---
title: Hinzufügen eines NSX-T-Netzwerksegments mithilfe von Azure VMware Solution
description: Hier finden Sie die Schritte zum Hinzufügen eines NSX-T-Netzwerksegments für Azure VMware Solution im Azure-Portal.
ms.topic: include
ms.date: 07/16/2021
ms.openlocfilehash: 7744884da7e1e6d46428bb00b52f88772c866760
ms.sourcegitcommit: 92dd25772f209d7d3f34582ccb8985e1a099fe62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114229309"
---
<!-- Used in configure-dhcp-azure-vmware-solution.md and tutorial-nsx-t-network-segment.md -->


>[!NOTE]
>Wenn Sie DHCP verwenden möchten, müssen Sie [einen DHCP-Server oder ein DHCP-Relay konfigurieren](../configure-dhcp-azure-vmware-solution.md), bevor Sie ein NSX-T-Netzwerksegment konfigurieren können. 

1. Wählen Sie in Ihrer privaten Azure VMware Solution-Cloud unter **Workloadnetzwerk** die Option **Segmente** > **Hinzufügen** aus. 

2. Geben Sie die Details für das neue logische Segment an, und klicken Sie auf **OK**.

   :::image type="content" source="../media/networking/add-new-nsxt-segment.png" alt-text="Screenshot: Hinzufügen eines neuen NSX-T-Segments im Azure-Portal":::

   - **Segmentname**: Name des Segments, das in vCenter sichtbar ist

   - **Subnetzgateway**: Gateway-IP-Adresse für das Subnetz des Segments mit einer Subnetzmaske. Virtuelle Computer werden einem logischen Segment angefügt, und alle VMs, die eine Verbindung mit diesem Segment herstellen, gehören zum selben Subnetz.  Außerdem müssen alle VMs, die diesem logischen Segment angefügt sind, eine IP-Adresse aus dem selben Segment enthalten.

   - **DHCP** (optional): DHCP-Bereiche für ein logisches Segment. Ein [DHCP-Server oder DHCP-Relay](../configure-dhcp-azure-vmware-solution.md) muss für die Nutzung von DHCP in Segmenten konfiguriert werden.  

   >[!NOTE]
   >Das **verbundene Gateway** ist standardmäßig ausgewählt und schreibgeschützt.  Es zeigt das Tier-1-Gateway und den Typ der Segmentinformationen. 
   >
   >- **T1**: Name des Tier-1-Gateways im NSX-T Manager. Eine private Cloud wird mit einem NSX-T Tier-0-Gateway im Aktiv/Aktiv-Modus und einem standardmäßigen NSX-T Tier-1-Gateway im Aktiv/Standby-Modus bereitgestellt.  Segmente, die über die Azure VMware Solution-Konsole erstellt werden, stellen nur eine Verbindung mit dem standardmäßigen NSX-T Tier-1-Gateway her, und die Workloads dieser Segmente erhalten Ost-West- und Nord-Süd-Konnektivität. Sie können nur über den NSX-T Manager weitere Tier-1-Gateways erstellen. Von der NSX-T Manager-Konsole erstellte Tier-1-Gateways sind in der Azure VMware Solution-Konsole nicht sichtbar. 
   >
   >- **Typ**: Überlagerungssegment, das von Azure VMware Solution unterstützt wird.

Das Segment ist nun in Azure VMware Solution, NSX-T Manager und vCenter sichtbar.