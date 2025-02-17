---
title: Erstellen einer VM mit einer statischen öffentlichen IP-Adresse – PowerShell | Microsoft-Dokumentation
description: Erstellen einer VM mit einer statischen öffentlichen IP-Adresse mithilfe von PowerShell Statische öffentliche IP-Adressen sind Adressen, die sich nie ändern.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
ms.subservice: ip-services
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: allensu
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6054d600ef7619cedfc0360eed1c8d78f1afbaf8
ms.sourcegitcommit: beff1803eeb28b60482560eee8967122653bc19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113433596"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse mithilfe von PowerShell


Sie können einen virtuellen Computer (VM) mit einer statischen öffentlichen IP-Adresse erstellen. Mit einer öffentlichen IP-Adresse können Sie über das Internet mit einer VM kommunizieren. Um sicherzustellen, dass sich die Adresse niemals ändert, weisen Sie anstelle einer dynamischen Adresse eine statische öffentliche IP-Adresse zu. Lesen Sie im folgenden Artikel mehr über [statische öffentliche IP-Adressen](./public-ip-addresses.md#ip-address-assignment). Um eine öffentliche dynamische IP-Adresse, die einem vorhandenen virtuellen Computer zugewiesen ist, in eine statische Adresse zu ändern oder mit privaten IP-Adressen zu arbeiten, lesen Sie [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md). Für öffentliche IP-Adressen fällt eine [Schutzgebühr](https://azure.microsoft.com/pricing/details/ip-addresses) an, und für die Anzahl der öffentlichen IP-Adressen, die Sie pro Abonnement verwenden können, ist ein [Grenzwert](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) festgelegt.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Auf dem lokalen Computer oder mithilfe von Azure Cloud Shell können Sie die folgenden Schritte durchführen. Um den lokalen Computer zu verwenden, stellen Sie sicher, dass [Azure PowerShell installiert ist](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Wählen Sie oben rechts in einem beliebigen Befehlsfelds **Testen** aus, um Azure Cloud Shell zu verwenden. Cloud Shell meldet Sie in Azure an.

1. Wenn Sie Cloud Shell bereits verwenden, fahren Sie mit Schritt 2 fort. Öffnen Sie eine Befehlssitzung, und melden Sie sich mit `Connect-AzAccount` in Azure an.
2. Erstellen Sie mit dem Befehl [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe in der Azure-Region „USA, Osten“ erstellt:

   ```azurepowershell-interactive
   New-AzResourceGroup -Name myResourceGroup -Location EastUS
   ```

3. Erstellen Sie mit dem Befehl [New-AzVM](/powershell/module/az.Compute/New-azVM) einen virtuellen Computer. Die Option `-AllocationMethod "Static"` weist dem virtuellen Computer eine statische öffentliche IP-Adresse zu. Das folgende Beispiel erstellt einen virtuellen Windows Server-Computer mit einer statischen öffentlichen IP-Adresse mit der SKU „Basic“ mit dem Namen *myPublicIpAddress*. Geben Sie bei entsprechender Aufforderung einen Benutzernamen und ein Kennwort als Anmeldeinformationen für den virtuellen Computer ein:

   ```azurepowershell-interactive
   New-AzVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
   ```

   Wenn die öffentliche IP-Adresse die SKU „Standard“ aufweisen muss, müssen Sie in Einzelschritten [eine öffentliche IP-Adresse erstellen](virtual-network-public-ip-address.md#create-a-public-ip-address), [eine Netzwerkschnittstelle erstellen](virtual-network-network-interface.md#create-a-network-interface), [der Netzwerkschnittstelle die öffentliche IP-Adresse zuweisen](virtual-network-network-interface-addresses.md#add-ip-addresses) und dann [einen virtuellen Computer mit der Netzwerkschnittstelle erstellen](virtual-network-network-interface-vm.md#add-existing-network-interfaces-to-a-new-vm). Weitere Informationen zu SKUs für öffentliche IP-Adressen finden Sie [hier](./public-ip-addresses.md#sku). Wenn die VM dem Back-End-Pool einer öffentlichen Azure Load Balancer-Instanz hinzugefügt wird, muss die SKU der öffentlichen IP-Adresse der VM der SKU der öffentlichen IP-Adresse des Lastenausgleichsmoduls entsprechen. Weitere Informationen finden Sie unter [Azure Load Balancer](../load-balancer/skus.md).

4. Zeigen Sie die zugewiesene öffentliche IP-Adresse an, und bestätigen Sie mit [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress), dass sie als statische Adresse erstellt wurde:

   ```azurepowershell-interactive
   Get-AzPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
   ```

   Azure hat eine öffentliche IP-Adresse über Adressen in zugewiesen, die in der Region, in der der virtuelle Computer erstellt wurde, verwendet werden. Sie können die Liste von Bereichen (Präfixen) für die [öffentliche Azure-Cloud](https://www.microsoft.com/download/details.aspx?id=56519), die [Azure US Government-Cloud](https://www.microsoft.com/download/details.aspx?id=57063) sowie für die Azure-Cloud in [China](https://www.microsoft.com/download/details.aspx?id=57062) und [Deutschland](https://www.microsoft.com/download/details.aspx?id=57064) herunterladen.

> [!WARNING]
> Ändern Sie die IP-Adresseinstellungen nicht innerhalb des Betriebssystems des virtuellen Computers. Öffentliche Azure-IP-Adressen sind dem Betriebssystem nicht bekannt. Auch wenn Sie dem Betriebssystem Einstellungen für private IP-Adressen hinzufügen können, wird empfohlen, dies erst nach Lesen des Artikels [Hinzufügen einer privaten IP-Adresse zu einem Betriebssystem](virtual-network-network-interface-addresses.md#private) und bei absoluter Notwendigkeit zu tun.

[!INCLUDE [ephemeral-ip-note.md](../../includes/ephemeral-ip-note.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie mehr über [öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses) in Azure.
- Erfahren Sie mehr über [Einstellungen für öffentliche IP-Adressen](virtual-network-public-ip-address.md#create-a-public-ip-address).
- Lesen Sie mehr über [private IP-Adressen](./private-ip-addresses.md) und das Zuweisen einer [statischen privaten IP-Adresse](virtual-network-network-interface-addresses.md#add-ip-addresses) zu einer Azure-VM.
- Erfahren Sie mehr über das Erstellen von VMs unter [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
