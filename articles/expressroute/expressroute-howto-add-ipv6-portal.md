---
title: 'Azure ExpressRoute: Hinzufügen der IPv6-Unterstützung mithilfe des Azure-Portals'
description: Hier erfahren Sie, wie Sie die IPv6-Unterstützung zum Herstellen einer Verbindung mit Azure-Bereitstellungen mithilfe des Azure-Portals hinzufügen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/09/2021
ms.author: duau
ms.openlocfilehash: 805299da7c74b18cb95c6dc87e1a0462c7f1664b
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2021
ms.locfileid: "123424202"
---
# <a name="add-ipv6-support-for-private-peering-using-the-azure-portal-preview"></a>Hinzufügen der IPv6-Unterstützung für privates Peering mithilfe des Azure-Portal (Vorschau)

In diesem Artikel wird beschrieben, wie Sie die IPv6-Unterstützung zum Herstellen einer Verbindung mit Ihren Ressourcen in Azure über ExpressRoute mithilfe des Azure-Portal hinzufügen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

## <a name="add-ipv6-private-peering-to-your-expressroute-circuit"></a>Hinzufügen von privatem IPv6-Peering zu Ihrer ExpressRoute-Verbindung

1. [Erstellen Sie eine ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md), oder navigieren Sie zur vorhandenen Verbindung, die Sie ändern möchten.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-circuit.png" alt-text="Screenshot einer Liste von ExpressRoute-Verbindungen.":::

1. Wählen Sie als Peeringkonfiguration **Azure, privat** aus.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-peering.png" alt-text="Screenshot der Übersichtsseite von ExpressRoute-Leitungen.":::

1. Fügen Sie ein privates IPv6-Peering zu Ihrer vorhandenen privaten IPv4-Peeringkonfiguration hinzu, indem Sie „Both“ (Beide) für **Subnets** (Subnetze) auswählen oder das private IPv6-Peering für eine neue Verbindung durch Auswählen von „IPv6“ aktivieren. Geben Sie zwei /126 IPv6-Subnetze an, die Sie für Ihre primäre bzw. sekundäre Verknüpfung besitzen. Aus jedem dieser Subnetze weisen Sie die erste verwendbare IP-Adresse für Ihren Router zu, da die zweite verwendbare IP-Adresse von Microsoft für den eigenen Router genutzt wird. **Speichern** Sie Ihre Peeringkonfiguration, sobald Sie alle Parameter angegeben haben.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-peering.png" alt-text="Screenshot: Hinzufügen von IPv6 auf der Seite zum privaten Peering.":::

1. Nachdem die Konfiguration akzeptiert wurde, sieht die Anzeige in etwa wie im folgenden Beispiel aus:

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/view-ipv6-peering.png" alt-text="Screenshot von IPv6, das für privates Peering konfiguriert ist.":::

## <a name="update-your-connection-to-an-existing-virtual-network"></a>Aktualisieren Sie die Verbindung mit einem vorhandenen virtuellen Netzwerk.

Führen Sie die folgenden Schritte aus, wenn Sie über eine vorhandene Umgebung mit Azure-Ressourcen verfügen, für die Sie Ihr privates IPv6-Peering verwenden möchten.

1. Navigieren Sie zu dem virtuellen Netzwerk, mit dem Ihre ExpressRoute-Verbindung verknüpft ist.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-vnet.png" alt-text="Screenshot: Liste der virtuellen Netzwerke.":::

1. Navigieren Sie zur Registerkarte **Adressraum**, und fügen Sie einen IPv6-Adressraum zu Ihrem virtuellen Netzwerk hinzu. **Speichern** Sie Ihren Adressraum.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-space.png" alt-text="Screenshot: Hinzufügen eines IPv6-Adressraums zum virtuellen Netzwerk.":::

1. Navigieren Sie zur Registerkarte **Subnetze**, und wählen Sie das **GatewaySubnet** aus. Aktivieren Sie das Kontrollkästchen **IPv6-Adresse hinzufügen**, und geben Sie einen IPv6-Adressraum für Ihr Subnetz an. Das Gateway-IPv6-Subnetz sollte /64 oder größer sein. **Speichern** Sie Ihre Konfiguration, nachdem Sie alle Parameter angegeben haben.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-gateway-space.png" alt-text="Screenshot: Hinzufügen des Ipv6-Adressraums zum Subnetz.":::
    
1. Wenn Sie über ein vorhandenes zonenredundantes Gateway verfügen, können Sie in PowerShell Folgendes ausführen, um die IPv6-Konnektivität zu aktivieren. (Beachten Sie, dass es bis zu eine Stunde dauern kann, bis Änderungen widergespiegelt werden.) [Erstellen Sie andernfalls das Gateway für virtuelle Netzwerke](./expressroute-howto-add-gateway-portal-resource-manager.md), indem Sie eine beliebige SKU und eine öffentliche IP-Adresse vom Typ „Standard, Statisch“ verwenden. Wenn Sie FastPath verwenden möchten, sollten Sie UltraPerformance oder ErGw3AZ nutzen. (Beachten Sie, dass diese Option nur für Verbindungen mit ExpressRoute Direct verfügbar ist.)

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name "GatewayName" -ResourceGroupName "ExpressRouteResourceGroup"
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
    
>[!NOTE]
> If you have an existing gateway that is not zone-redundant (meaning it is Standard, High Performance, or Ultra Performance SKU), you will need to delete and [recreate the gateway](expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway) using any SKU and a Standard, Static public IP address.

## Create a connection to a new virtual network

Follow the steps below if you plan to connect to a new set of Azure resources using your IPv6 Private Peering.

1. Create a dual-stack virtual network with both IPv4 and IPv6 address space. For more information, see [Create a virtual network](../virtual-network/quick-create-portal.md#create-a-virtual-network).

1. [Create the dual-stack gateway subnet](expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet).

1. [Create the virtual network gateway](expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway) using any SKU and a Standard, Static public IP address. If you plan to use FastPath, use UltraPerformance or ErGw3AZ (note that this option is only available for circuits using ExpressRoute Direct).

1. [Link your virtual network to your ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md).

## Limitations
While IPv6 support is available for connections to deployments in Public Azure regions, it doesn't support the following use cases:

* Connections to existing ExpressRoute gateways that are *not* zone-redundant
* Global Reach connections between ExpressRoute circuits
* Use of ExpressRoute with virtual WAN
* FastPath with non-ExpressRoute Direct circuits
* FastPath with circuits in the following peering locations: Dubai
* Coexistence with VPN Gateway

## Next steps

To troubleshoot ExpressRoute problems, see the following articles:

* [Verifying ExpressRoute connectivity](expressroute-troubleshooting-expressroute-overview.md)
* [Troubleshooting network performance](expressroute-troubleshooting-network-performance.md)
