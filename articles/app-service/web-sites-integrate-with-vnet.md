---
title: Integration einer App in Azure Virtual Network
description: Integrieren einer App in Azure App Service mit virtuellen Azure-Netzwerken
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 08/04/2021
ms.author: ccompy
ms.custom: seodec18, devx-track-azurepowershell
ms.openlocfilehash: 444831d1d8e9982ac0837e90fe04941b5ae928a7
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339141"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrieren Ihrer App in ein Azure Virtual Network

In diesem Artikel wird die Azure App Service-Funktion für die VNet-Integration beschrieben, und Sie erfahren, wie Sie die Funktion mit Apps in [Azure App Service](./overview.md) einrichten. Mit [Azure Virtual Network][VNETOverview] (VNET) können Sie viele Ihrer Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann. Die VNet-Integrationsfunktion ermöglicht Ihren Apps den Zugriff auf Ressourcen in einem oder über ein VNet. Die VNet-Integration ermöglicht keinen privaten Zugriff auf Ihre Apps.

Es gibt zwei Varianten der Nutzung von Azure App Service:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>Aktivieren der VNET-Integration

1. Navigieren Sie zur **Netzwerk** benutzeroberfläche im App Service-Portal. Wählen Sie unter **VNET-Integration** die Option **Zum Konfigurieren hier klicken** aus.

1. Wählen Sie **VNET hinzufügen** aus.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-app.png" alt-text="Auswählen der VNET-Integration":::

1. Die Dropdownliste enthält alle virtuellen Azure Resource Manager-Netzwerke in Ihrem Abonnement in derselben Region. Darunter befindet sich eine Liste der virtuellen Resource Manager-Netzwerke in allen anderen Regionen. Wählen Sie das VNET aus, das Sie integrieren möchten.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-add-vnet.png" alt-text="Auswählen des VNETs":::

    * Wenn sich das VNET in derselben Region befindet, erstellen Sie ein neues Subnetz, oder wählen Sie ein leeres bereits vorhandenes Subnetz aus.
    * Wenn Sie ein VNET in einer anderen Region auswählen möchten, müssen Sie über ein VNET-Gateway verfügen, für das Point-to-Site aktiviert ist.
    * Wenn die Integration in ein klassisches VNET erfolgen soll, wählen Sie anstelle der Dropdownliste **Virtuelles Netzwerk** die Option **Klicken Sie hier, um eine Verbindung mit einem klassischen VNET herzustellen** aus. Wählen Sie das gewünschte klassische virtuelle Netzwerk aus. Das Ziel-VNET muss bereits über ein bereitgestelltes Gateway für virtuelle Netzwerke verfügen, für das Point-to-Site aktiviert ist.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-classic.png" alt-text="Auswählen eines klassischen VNETs":::

Während der Integration wird Ihre App neu gestartet. Wenn die Integration abgeschlossen ist, werden Details zu dem VNET angezeigt, in das Sie integriert sind.

## <a name="regional-vnet-integration"></a>Regionale VNET-Integration

Die regionale VNET-Integration unterstützt das Herstellen einer Verbindung mit einem VNET in der gleichen Region und erfordert kein Gateway. Die Verwendung der regionalen VNET-Integration ermöglicht der App Zugriff auf Folgendes:

* Ressourcen in einem VNET in derselben Region wie Ihre App.
* Ressourcen in VNETs, die mit dem VNET, in das Ihre App integriert ist, per Peering verbunden sind.
* Durch Dienstendpunkte geschützte Dienste.
* Ressourcen über Azure ExpressRoute-Verbindungen.
* Ressourcen in dem VNET, in das Sie integriert sind.
* Ressourcen über Verbindungen mit Peering, einschließlich von Azure ExpressRoute-Verbindungen.
* Dienste, von denen private Endpunkte unterstützt werden.

Wenn Sie VNET-Integration mit VNETs in derselben Region verwenden, können Sie die folgenden Azure-Netzwerkfunktionen nutzen:

* **Netzwerksicherheitsgruppen (NSGs)** : Sie können ausgehenden Datenverkehr mit einer NSG blockieren, die in Ihrem Integrationssubnetz platziert ist. Die Regeln für eingehenden Datenverkehr gelten nicht, da Sie die VNET-Integration nicht verwenden können, um eingehenden Zugriff auf Ihre App bereitzustellen.
* **Routingtabellen (UDRs)** : Sie können eine Routingtabelle im Integrationssubnetz platzieren, um ausgehenden Datenverkehr an beliebige gewünschte Ziele zu senden.

Das Feature wird für Windows- und Linux-Apps vollständig unterstützt, einschließlich [benutzerdefinierten Containern](./quickstart-custom-container.md). Das Verhalten ist in Windows- und Linux-Apps gleich.

### <a name="how-regional-vnet-integration-works"></a>Funktionsweise der regionalen VNET-Integration

Apps in App Service werden auf Workerrollen gehostet. Der Tarif „Basic“ und höhere Tarife sind dedizierte Hostingpläne, bei denen keine anderen Kundenworkloads auf den gleichen Workern ausgeführt werden. Die regionale VNET-Integration beruht auf der Einbindung virtueller Schnittstellen mit Adressen im delegierten Subnetz. Da sich die Herkunftsadresse in Ihrem VNET befindet, kann sie ähnlich wie eine VM im VNET auf die meisten Ressourcen zugreifen, die im oder über das VNET verfügbar sind. Die Netzwerkimplementierung unterscheidet sich von der Ausführung eines virtuellen Computers in Ihrem VNET. Aus diesem Grund sind einige Netzwerkfunktionen für dieses Feature noch nicht verfügbar.

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-how-regional-works.png" alt-text="Funktionsweise der regionalen VNET-Integration":::

Bei aktivierter regionaler VNET-Integration sendet Ihre App ausgehenden Datenverkehr über das VNet. Ihre App verwendet nach wie vor die ausgehenden Adressen, die im Portal für die App-Eigenschaften aufgeführt sind. Wenn das gesamte Datenverkehrsrouting aktiviert ist, wird der gesamte ausgehende Datenverkehr an Ihr VNet gesendet. Ist das gesamte Datenverkehrsrouting nicht aktiviert, werden nur privater Datenverkehr (RFC1918) und Dienstendpunkte, die im Integrationssubnetz konfiguriert sind, an das VNet gesendet, und ausgehender Datenverkehr für das Internet wird über die üblichen Kanäle abgewickelt.

Die Funktion unterstützt nur eine virtuelle Schnittstelle pro Worker. Eine virtuelle Schnittstelle pro Worker bedeutet eine regionale VNET-Integration pro App Service Plan. Alle Apps im selben App Service-Plan können dieselbe VNet-Integration verwenden. Wenn eine App eine Verbindung mit einem zusätzlichen VNET herstellen muss, müssen Sie einen weiteren App Service-Plan erstellen. Die verwendete virtuelle Schnittstelle ist keine Ressource, auf die Kunden direkten Zugriff haben.

Aufgrund der Funktionsweise dieser Technologie wird der Datenverkehr in Verbindung mit der VNet-Integration nicht in Azure Network Watcher- oder NSG-Flussprotokollen (Netzwerksicherheitsgruppe) aufgeführt.

### <a name="subnet-requirements"></a>Subnetzanforderungen

VNET-Integration hängt von der Verwendung eines dedizierten Subnetzes ab. Wenn Sie ein Subnetz bereitstellen, verliert das Azure-Subnetz von Anfang an fünf IP-Adressen. Für jede Instanz des Plans wird eine Adresse aus dem Integrationssubnetz verwendet. Wenn Sie Ihre App auf vier Instanzen skalieren, werden vier Adressen verwendet. 

Wenn Sie die Größe hoch- oder herunterskaliert haben, wird der erforderliche Adressraum für einen kurzen Zeitraum verdoppelt. Dies wirkt sich auf die tatsächlichen, verfügbaren unterstützten Instanzen für eine bestimmte Subnetzgröße aus. Die folgende Tabelle zeigt sowohl die maximal verfügbaren Adressen pro CIDR-Block als auch die Auswirkungen auf die horizontale Skalierung:

| CIDR-Blockgröße | Maximal verfügbare Adressen | Maximale horizontale Skalierung (Instanzen)<sup>*</sup> |
|-----------------|-------------------------|---------------------------------|
| /28             | 11                      | 5                               |
| /27             | 27                      | 13                              |
| /26             | 59                      | 29                              |

<sup>*</sup>Geht davon aus, dass irgendwann ein Hoch- oder Herunterskalieren der Größe oder SKU erforderlich ist. 

Da die Subnetzgröße nach der Zuweisung nicht mehr geändert werden kann, verwenden Sie ein Subnetz, das für die Aufnahme jedweder Skalierung Ihrer App groß genug ist. Um jegliche Probleme mit der Subnetzkapazität zu vermeiden, ist die empfohlene Größe /26 mit 64 Adressen.

Wenn Ihre Apps in Ihrem Plan ein VNet erreichen sollen, mit dem bereits Apps in einem anderen Plan eine Verbindung herstellen, wählen Sie ein Subnetz aus, das nicht von der bereits vorhandenen VNET-Integration verwendet wird.

### <a name="routes"></a>Routen

Beim Konfigurieren der regionalen VNET-Integration stehen zwei Routingtypen zur Verfügung: Mit Anwendungsrouting wird definiert, welcher Datenverkehr von Ihrer Anwendung an das VNet weitergeleitet wird. Mit Netzwerkrouting können Sie steuern, wie Datenverkehr von Ihrem VNet und nach draußen weitergeleitet wird.

#### <a name="application-routing"></a>Anwendungsrouting

Beim Konfigurieren des Anwendungsroutings können Sie entweder den gesamten Datenverkehr oder nur privaten Datenverkehr (auch [RFC1918-Datenverkehr](https://datatracker.ietf.org/doc/html/rfc1918#section-3) genannt) an Ihr VNet weiterleiten. Dies wird über die Einstellung „Route All“ (Gesamten Datenverkehr weiterleiten) konfiguriert. Ist „Route All“ (Gesamten Datenverkehr weiterleiten) deaktiviert, wird von Ihrer App nur privater Datenverkehr an Ihr VNet weitergeleitet. Wenn Sie den gesamten ausgehenden Datenverkehr an Ihr VNet weiterleiten möchten, muss „Route All“ (Gesamten Datenverkehr weiterleiten) aktiviert sein.

> [!NOTE]
> * Ist „Route All“ (Gesamten Datenverkehr weiterleiten) aktiviert, unterliegt der gesamte Datenverkehr den NSGs und UDRs, die auf Ihr Integrationssubnetz angewendet werden. Wenn das gesamte Datenverkehrsrouting aktiviert ist, wird ausgehender Datenverkehr weiterhin von den Adressen gesendet, die in Ihren App-Eigenschaften aufgeführt sind – es sei denn, Sie stellen Routen bereit, durch die der Datenverkehr an einen anderen Ort weitergeleitet wird.
> 
> * „Route All“ (Gesamten Datenverkehr weiterleiten) wird momentan in Windows-Containern nicht unterstützt.
>
> * Von der regionalen VNET-Integration kann der Port 25 nicht verwendet werden.

„Route All“ (Gesamten Datenverkehr weiterleiten) kann in Ihrer App über das Portal deaktiviert werden. Gehen Sie dazu wie folgt vor: 

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-route-all-enabled.png" alt-text="„Route All“ (Gesamten Datenverkehr weiterleiten) aktiviert":::

1. Navigieren Sie in Ihrem App-Portal zur Benutzeroberfläche für die **VNET-Integration**.
1. Legen Sie **Route All** (Gesamten Datenverkehr weiterleiten) auf „Deaktiviert“ fest.
    
    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-route-all-disabling.png" alt-text="Deaktivieren von „Route All“ (Gesamten Datenverkehr weiterleiten)":::

1. Wählen Sie **Ja**.

Sie können „Route All“ (Gesamten Datenverkehr weiterleiten) auch mithilfe der CLI konfigurieren (*Hinweis:* ab `az version` 2.27.0):

```azurecli-interactive
az webapp config set --resource-group myRG --name myWebApp --vnet-route-all-enabled [true|false]
```

Die Konfigurationseinstellung „Route All“ (Gesamten Datenverkehr weiterleiten) ersetzt die Legacy-App-Einstellung `WEBSITE_VNET_ROUTE_ALL` und hat Vorrang.

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-route-all-appsetting.png" alt-text="App-Einstellung „Route All“ (Gesamten Datenverkehr weiterleiten)":::

#### <a name="network-routing"></a>Netzwerkrouting

Sie können mithilfe von Routingtabellen ausgehenden Datenverkehr von Ihrer App an beliebige Ziele weiterleiten. Routingtabellen wirken sich auf Ihren Zieldatenverkehr aus. Wenn „Route All“ (Gesamten Datenverkehr weiterleiten) im [Anwendungsrouting](#application-routing) deaktiviert ist, ist nur privater Datenverkehr (RFC1918) von Ihren Routingtabellen betroffen. Gängige Ziele können Firewallgeräte oder Gateways beinhalten. Routen, die für Ihr Integrationssubnetz festgelegt werden, wirken sich nicht auf Antworten auf eingehende App-Anforderungen aus. 

Wenn Sie den gesamten ausgehenden Datenverkehr lokal weiterleiten möchten, können Sie eine Routingtabelle verwenden, um den gesamten ausgehenden Datenverkehr an das ExpressRoute-Gateway zu senden. Wenn Sie Datenverkehr nicht an ein Gateway weiterleiten, müssen Sie unbedingt Routen im externen Netzwerk festlegen, über die Antworten zurückgesendet werden.

BGP-Routen (Border Gateway Protocol) wirken sich ebenfalls auf den App-Datenverkehr aus. Wenn Sie über BGP-Routen von einem ExpressRoute-Gateway verfügen, ist der ausgehende Datenverkehr Ihrer App betroffen. Ähnlich wie benutzerdefinierte Routen wirken sich BGP-Routen gemäß Ihrer Routingbereichseinstellung auf den Datenverkehr aus.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Eine App mit regionaler VNET-Integration kann eine [Netzwerksicherheitsgruppe][VNETnsg] verwenden, um ausgehenden Datenverkehr an Ressourcen in Ihrem VNet oder im Internet zu blockieren. Wenn Sie Datenverkehr an öffentliche Adressen blockieren möchten, muss [Route All](#application-routing) (Gesamten Datenverkehr weiterleiten) für das VNet aktiviert werden. Ist „Route All“ (Gesamten Datenverkehr weiterleiten) nicht aktiviert, werden NSGs nur auf RFC1918-Datenverkehr angewendet.

Eine NSG, die auf Ihr Integrationssubnetz angewendet wird, ist unabhängig von den Routingtabellen wirksam, die ggf. auf Ihr Integrationssubnetz angewendet werden. 

Die Regeln für eingehenden Datenverkehr in einer NSG gelten nicht für Ihre App, da sich die VNET-Integration nur auf ausgehenden Datenverkehr Ihrer App auswirkt. Um den eingehenden Datenverkehr für Ihre App zu steuern, verwenden Sie das Feature für Zugriffsbeschränkungen.

### <a name="service-endpoints"></a>Dienstendpunkte

Mit der regionalen VNET-Integration können Sie Azure-Dienste erreichen, die mit Dienstendpunkten geschützt sind. Gehen Sie wie folgt vor, um auf einen durch einen Dienstendpunkt gesicherten Dienst zuzugreifen:

* Konfigurieren Sie die Integration des regionalen VNET in Ihrer Web-App, um eine Verbindung mit einem bestimmten Subnetz für die Integration herzustellen.
* Wechseln Sie zum Zieldienst, und konfigurieren Sie Dienstendpunkte für das Integrationssubnetz.

### <a name="private-endpoints"></a>Private Endpunkte

Wenn Sie Aufrufe an [private Endpunkte][privateendpoints] senden möchten, müssen Sie sicherstellen, dass Ihre DNS-Suchen zu dem privaten Endpunkt aufgelöst werden. Sie können dieses Verhalten auf eine der folgenden Arten erzwingen: 

* Integrieren mit private Azure DNS-Zonen. Falls Ihr VNet nicht über einen benutzerdefinierten DNS-Server verfügt, erfolgt dies automatisch, wenn die Zonen mit dem VNet verknüpft werden.
* Verwalten des privaten Endpunkts im DNS-Server, der von Ihrer App verwendet wird. Hierzu müssen Sie die Adresse des privaten Endpunkts kennen und dann den Endpunkt, den Sie erreichen möchten, mit einem A-Eintrag auf diese Adresse verweisen lassen.
* Konfigurieren Ihres eigenen DNS-Servers zur Weiterleitung an private Azure DNS-Zonen.

### <a name="azure-dns-private-zones"></a>Azure DNS Private Zones 

Nachdem Ihre App in Ihr VNet integriert wurde, verwendet sie den DNS-Server, mit dem Ihr VNet konfiguriert ist. Ist kein benutzerdefiniertes DNS angegeben, verwendet sie das standardmäßige Azure-DNS sowie alle privaten Zonen, die ggf. mit dem VNet verknüpft sind.

> [!NOTE]
> Für Linux-Apps funktioniert Azure DNS Private Zones nur, wenn „Route All“ (Gesamten Datenverkehr weiterleiten) aktiviert ist.

### <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen bei der Verwendung der VNET-Integration in VNETs in derselben Region:

* Sie können nicht über Verbindungen mit globalem Peering auf Ressourcen zugreifen.
* Sie können keine Ressourcen über Peeringverbindungen mit klassischen virtuellen Netzwerken erreichen.
* Das Feature ist in allen App Service-Skalierungseinheiten in den Tarifen „Premium V2“ und „Premium V3“ verfügbar. Es ist außerdem im Tarif „Standard“ verfügbar, jedoch nur in neueren App Service-Skalierungseinheiten. Wenn Sie sich auf einer älteren Skalierungseinheit befinden, können Sie das Feature nur aus einem „Premium V2“-App Service-Plan verwenden. Wenn Sie sicherstellen möchten, dass Sie das Feature in einem „Standard“-App Service-Plan verwenden können, erstellen Sie Ihre App in einem „Premium V3“-App Service-Plan. Diese Pläne werden nur für auf unseren neuesten Skalierungseinheiten unterstützt. Sie können anschließend nach Bedarf herunterskalieren.  
* Das Integrationssubnetz kann nur von einem App Service-Plan verwendet werden.
* Die Funktion kann nicht für Apps im Plan „App Service (isoliert)“ in einer App Service-Umgebung verwendet werden.
* Für das Feature ist ein nicht genutztes Subnetz vom Typ /28 oder größer in einem Azure Resource Manager-VNet erforderlich.
* Die App und das VNET müssen sich in derselben Region befinden.
* Es ist nicht möglich, ein VNET mit einer integrierten App zu löschen. Entfernen Sie die Integration, bevor Sie das VNET löschen.
* Sie können nur eine regionale VNET-Integration pro App Service-Plan verwenden. Mehrere Apps im gleichen App Service-Plan können das gleiche VNET verwenden.
* Sie können das Abonnement einer App oder eines Plans nicht ändern, solange eine App mit regionaler VNET-Integration vorhanden ist.
* Ihre App kann ohne Konfigurationsänderungen keine Adressen in Azure DNS Private Zones unter Linux-Plänen auflösen.

## <a name="gateway-required-vnet-integration"></a>Von einem Gateway abhängige VNet-Integration

Die von einem Gateway abhängige VNET-Integration unterstützt das Herstellen einer Verbindung mit einem VNET in einer anderen Region oder mit einem klassischen VNET. Von einem Gateway abhängige VNet-Integration:

* Eine App kann nur jeweils mit einem VNET eine Verbindung herstellen.
* Bis zu fünf VNETs können in einen App Service-Plan integriert werden.
* Ermöglicht die Verwendung desselben VNET durch mehrere Apps, ohne dass sich dies auf die für einen App Service-Plan zulässige Gesamtanzahl von VNETs auswirkt. Wenn Sie sechs Apps verwenden, die dasselbe VNET im selben App Service-Plan nutzen, zählt dies als ein verwendetes VNET.
* Unterstützt dank der SLA auf dem Gateway eine SLA mit einer Verfügbarkeit von 99,9 %.
* Ermöglicht Ihren Apps die Verwendung des DNS, mit dem das VNET konfiguriert ist.
* Erfordert ein routenbasiertes Virtual Network-Gateway, das mit einem SSTP-Point-to-Site-VPN konfiguriert ist, bevor es mit einer App verbunden werden kann.

Sie können keine von einem Gateway abhängige VNet-Integration verwenden:

* Mit einem VNET, das mit Azure ExpressRoute verbunden ist.
* Aus einer Linux-App.
* Aus einem [Windows-Container](quickstart-custom-container.md).
* Für den Zugriff auf durch Dienstendpunkte geschützte Ressourcen.
* Mit einem Koexistenzgateway, das sowohl ExpressRoute als auch Point-to-Site- oder Site-to-Site-VPNs unterstützt.

### <a name="set-up-a-gateway-in-your-azure-virtual-network"></a>Einrichten eines Gateways in Ihrem virtuellen Azure-Netzwerk

So erstellen Sie ein Gateway

1. [Erstellen Sie ein Gatewaysubnetz][creategatewaysubnet] in Ihrem VNET.

1. [Erstellen Sie das VPN-Gateway][creategateway]. Wählen Sie einen routenbasierten VPN-Typ aus.

1. [Legen Sie die Point-to-Site-Adressen fest][setp2saddresses]. Wenn sich das Gateway nicht in der Basic-SKU befindet, muss IKEV2 in der Point-to-Site-Konfiguration deaktiviert und SSTP ausgewählt werden. Der Point-to Site-Adressraum muss in den RFC 1918-Adressblöcken 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 liegen.

Wenn Sie das Gateway für die Verwendung mit der App Service-VNet-Integration erstellen, müssen Sie kein Zertifikat hochladen. Das Erstellen des Gateways kann 30 Minuten dauern. Sie können Ihre App erst mit Ihrem VNET integrieren, nachdem das Gateway bereitgestellt wurde.

### <a name="how-gateway-required-vnet-integration-works"></a>Funktionsweise der von einem Gateway abhängigen VNet-Integration

Die von einem Gateway abhängige VNet-Integration basiert auf der Point-to-Site-VPN-Technologie. Point-to-Site-VPNs beschränken den Netzwerkzugriff auf den virtuellen Computer, auf dem die App gehostet wird. Apps sind darauf beschränkt, Datenverkehr an das Internet nur über Hybridverbindungen oder VNet-Integration zu senden. Wenn Ihre App über das Portal so konfiguriert wurde, dass sie die von einem Gateway abhängige VNet-Integration verwendet, wird eine komplexe Aushandlung in Ihrem Namen verwaltet, um Zertifikate auf der Gateway- und der Anwendungsseite zu erstellen und zuzuweisen. Als Ergebnis können die Worker, die Ihre Anwendungen hosten, direkte Verbindungen mit dem Gateway des virtuellen Netzwerks im ausgewählten VNET herstellen.

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-how-gateway-works.png" alt-text="Funktionsweise der von einem Gateway abhängigen VNet-Integration":::

### <a name="access-on-premises-resources"></a>Zugriff auf lokale Ressourcen

Apps können durch Integration mit VNETs, die Site-to-Site-Verbindungen aufweisen, auf lokale Ressourcen zugreifen. Wenn Sie die von einem Gateway abhängige VNet-Integration verwenden, aktualisieren Sie Ihre lokalen VPN-Gatewayrouten mit Ihren Point-to-Site-Adressblöcken. Verwenden Sie beim ersten Einrichten des Site-to-Site-VPN die Skripts für die Konfiguration. Die Routen sollten damit richtig eingerichtet werden. Wenn Sie die Point-to-Site-Adressen nach dem Erstellen des Site-to-Site-VPN hinzufügen, müssen Sie die Routen manuell aktualisieren. Die Details zur Vorgehensweise variieren je nach Gateway und sind hier nicht beschrieben. Das Border Gateway Protocol (BGP) kann nicht für eine Site-to-Site-VPN-Verbindung konfiguriert werden.

Für die regionale VNET-Integration ist keine zusätzliche Konfiguration erforderlich, um über Ihr VNET lokale Ressourcen zu erreichen. Sie müssen Ihr VNET lediglich über ExpressRoute oder ein Site-to-Site-VPN mit lokalen Ressourcen verbinden.

> [!NOTE]
> Bei der von einem Gateway abhängigen VNET-Integration wird eine App nicht in ein VNET integriert, das ein ExpressRoute-Gateway enthält. Selbst wenn das ExpressRoute-Gateway im [Koexistenzmodus][VPNERCoex] konfiguriert ist, funktioniert die VNet-Integration nicht. Wenn Sie über eine ExpressRoute-Verbindung auf Ressourcen zugreifen müssen, verwenden Sie die regionale VNET-Integrationsfunktion oder eine in Ihrem VNET ausgeführte [App Service-Umgebung][ASE].
>
>

### <a name="peering"></a>Peering

Wenn Sie Peering mit der regionalen VNet-Integration verwenden, ist keine zusätzliche Konfiguration erforderlich.

Bei der von einem Gateway abhängigen VNet-Integration müssen Sie für das Peering einige zusätzliche Elemente konfigurieren. So konfigurieren Sie Peering für Ihre App

1. Fügen Sie eine Peeringverbindung mit dem VNET hinzu, mit dem Ihre App eine Verbindung herstellt. Aktivieren Sie beim Hinzufügen der Peeringverbindung **Zugriff auf virtuelles Netzwerk zulassen** sowie **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen**.
1. Fügen Sie in dem VNET, das mit dem VNET, mit dem Sie verbunden sind, mittels Peering verknüpft ist, eine Peeringverbindung hinzu. Aktivieren Sie beim Hinzufügen der Peeringverbindung im Ziel-VNET **Zugriff auf virtuelles Netzwerk zulassen**, und wählen Sie **Weitergeleiteten Datenverkehr zulassen** und **Allow remote gateways** (Remotegateways zulassen) aus.
1. Wechseln Sie im Portal zu **App Service-Plan** > **Netzwerk** > **VNet-Integration**-Benutzeroberfläche. Wählen Sie das VNET aus, mit dem Ihre App eine Verbindung herstellt. Fügen Sie den Adressbereich des VNET, das mit dem VNET, mit dem Ihre App verbunden ist, mittels Peering verknüpft ist, im Abschnitt „Routing“ hinzu.

## <a name="manage-vnet-integration"></a>Verwalten der VNet-Integration

Das Verbinden und Trennen der Verbindung mit einem VNET erfolgt auf App-Ebene. Vorgänge, die sich auf die VNet-Integration über mehrere Apps auswirken können, werden auf Ebene des App Service-Plans ausgeführt. Sie können im Portal unter der App über **Netzwerk** > **VNET-Integration** Details zu Ihrem VNET abrufen. Ähnliche Informationen finden Sie auf der App Service-Planebene im Portal unter **App Service-Plan** > **Netzwerk** > **VNET-Integration**.

Der einzige Vorgang, den Sie in der App-Ansicht Ihrer VNET-Integration durchführen können, ist das Trennen der App von dem VNET, mit dem sie derzeit verbunden ist. Um Ihre App von einem VNET zu trennen, wählen Sie **Verbindung trennen** aus. Wenn Sie die Verbindung mit einem VNET trennen, wird die App neu gestartet. Das Trennen der Verbindung führt nicht zu Änderungen in Ihrem VNET. Das Subnetz oder Gateway wird nicht entfernt. Wenn Sie Ihr VNET löschen möchten, trennen Sie zuerst Ihre App vom VNET, und löschen Sie die darin enthaltenen Ressourcen, z. B. Gateways.

Auf der Benutzeroberfläche der VNET-Integration des App Service-Plans werden alle VNET-Integrationen angezeigt, die von den Apps in Ihrem App Service-Plan verwendet werden. Wählen Sie das gewünschte VNET aus, um Informationen dazu anzuzeigen. Es gibt zwei Aktionen, die Sie hier für die VNet-Integration mit erforderlichem Gateway durchführen können:

* **Netzwerk synchronisieren**: Der Vorgang zum Synchronisieren des Netzwerks wird nur für die von einem Gateway abhängige VNet-Integration verwendet. Durch die Netzwerksynchronisierung wird sichergestellt, dass Ihre Zertifikate und Netzwerkinformationen synchronisiert sind. Wenn Sie dem VNET ein DNS hinzufügen oder dieses ändern, führen Sie eine Netzwerksynchronisierung durch. Mit diesem Vorgang werden alle Apps, die dieses VNET verwenden, neu gestartet. Dieser Vorgang funktioniert nicht, wenn Sie eine App und ein VNet verwenden, die zu verschiedenen Abonnements gehören.
* **Routen hinzufügen**: Durch das Hinzufügen von Routen wird ausgehender Datenverkehr in Ihr VNET geleitet.

Die private IP-Adresse, die der Instanz zugewiesen ist, wird über die Umgebungsvariable **WEBSITE_PRIVATE_IP** verfügbar gemacht. Auf der Benutzeroberfläche der Kudu-Konsole wird auch die Liste der Umgebungsvariablen angezeigt, die für die Web-App verfügbar sind. Diese IP-Adresse wird aus dem Adressbereich des integrierten Subnetzes zugewiesen. Bei der regionalen VNET-Integration ist der Wert von „WEBSITE_PRIVATE_IP“ eine IP-Adresse aus dem Adressbereich des delegierten Subnetzes, und bei der für das Gateway erforderlichen VNET-Integration ist der Wert eine IP-Adresse aus dem Adressbereich des Point-to-Site-Adresspools, der für das VNET-Gateway konfiguriert ist. Diese IP-Adresse wird von der Web-App zum Herstellen einer Verbindung mit den Ressourcen über das virtuelle Netzwerk verwendet. 

> [!NOTE]
> Der Wert von WEBSITE_PRIVATE_IP ist veränderlich. Es handelt sich aber immer um eine IP-Adresse im Adressbereich des Integrationssubnetzes oder im Point-to-Site-Adressbereich. Sie müssen daher den Zugriff vom gesamten Adressbereich aus zulassen.
>

### <a name="gateway-required-vnet-integration-routing"></a>Routing der von einem Gateway abhängige VNet-Integration
Die in Ihrem VNET definierten Routen werden zum Weiterleiten des Datenverkehrs aus der App in Ihr VNET verwendet. Wenn Sie weiteren ausgehenden Datenverkehr in das VNET senden möchten, fügen Sie diese Adressblöcke hier hinzu. Diese Funktion kann nur mit der von einem Gateway abhängigen VNet-Integration. Routingtabellen wirken sich nicht auf den App-Datenverkehr aus, wenn von einem Gateway abhängige VNet-Integration so verwendet wird wie regionale VNet-Integration.

### <a name="gateway-required-vnet-integration-certificates"></a>Zertifikate der von einem Gateway abhängigen VNet-Integration
Wenn die von einem Gateway abhängige VNet-Integration aktiviert ist, müssen Zertifikate ausgetauscht werden, um die Sicherheit der Verbindung zu gewährleisten. Zusammen mit den Zertifikaten werden die DNS-Konfiguration, Routen und anderen Elemente, mit denen das Netzwerk beschrieben wird, ausgetauscht.

Wenn Zertifikate oder Netzwerkinformationen geändert werden, wählen Sie **Netzwerk synchronisieren** aus. Wenn Sie **Netzwerk synchronisieren** auswählen, bewirkt dies einen kurzen Ausfall der Verbindung zwischen der App und dem VNET. Die App wird nicht neu gestartet, aber der Konnektivitätsverlust kann dazu führen, dass Ihre Website nicht richtig funktioniert.

## <a name="pricing-details"></a>Preisübersicht
Bei der Funktion für die regionale VNet-Integration fallen neben den Gebühren für den App Service-Plantarif keine zusätzlichen Gebühren an.

Bei der Verwendung der von einem Gateway abhängigen VNet-Integrationsfunktion fallen drei Gebühren an:

* **Gebühren laut App Service-Plantarif**: Ihre Apps müssen sich in einem der App Service-Pläne „Standard“, „Premium“, „PremiumV2“ oder „PremiumV3“ befinden. Weitere Informationen zu diesen Kosten finden Sie unter [App Service-Preise][ASPricing].
* **Datenübertragungskosten**: Es gibt eine Gebühr für ausgehende Daten, auch wenn sich das VNET im selben Rechenzentrum befindet. Diese Gebühren werden unter [Datenübertragung – Preisübersicht][DataPricing] beschrieben.
* **Kosten für VPN Gateway**: Es gibt Kosten für das virtuelle Netzwerkgateway, das für das Point-to-Site-VPN erforderlich ist. Weitere Informationen finden Sie unter [VPN-Gateway: Preise][VNETPricing].

## <a name="troubleshooting"></a>Problembehandlung

> [!NOTE]
> Die VNET-Integration wird für Docker Compose-Szenarien in App Service nicht unterstützt.
> Die Zugriffsbeschränkungen von Azure Functions werden ignoriert, wenn ein privater Endpunkt vorliegt.
>

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Automation

CLI-Unterstützung für die regionale VNet-Integration ist verfügbar. Für den Zugriff auf die folgenden Befehle [installieren Sie die Azure CLI][installCLI].

```azurecli
az webapp vnet-integration --help

Group
    az webapp vnet-integration : Methods that list, add, and remove virtual network
    integrations from a webapp.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a regional virtual network integration to a webapp.
    list   : List the virtual network integrations on a webapp.
    remove : Remove a regional virtual network integration from webapp.
```

PowerShell-Unterstützung für die regionale VNET-Integration ist ebenfalls verfügbar. Sie müssen allerdings generische Ressourcen mit einem Eigenschaftsarray der Subnetzressourcen-ID (resourceID) erstellen.

```azurepowershell
# Parameters
$sitename = 'myWebApp'
$resourcegroupname = 'myRG'
$VNetname = 'myVNet'
$location = 'myRegion'
$integrationsubnetname = 'myIntegrationSubnet'
$subscriptionID = 'aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee'

# Property array with the SubnetID
$properties = @{
  subnetResourceId = "/subscriptions/$subscriptionID/resourceGroups/$resourcegroupname/providers/Microsoft.Network/virtualNetworks/$VNetname/subnets/$integrationsubnetname"
}

# Creation of the VNet Integration
$vNetParams = @{
  ResourceName = "$sitename/VirtualNetwork"
  Location = $location
  ResourceGroupName = $resourcegroupname
  ResourceType = 'Microsoft.Web/sites/networkConfig'
  PropertyObject = $properties
}
New-AzResource @vNetParams
```

<!--Links-->
[VNETOverview]: ../virtual-network/virtual-networks-overview.md
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: ../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[setp2saddresses]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#addresspool
[VNETRouteTables]: ../virtual-network/manage-route-table.md
[installCLI]: /cli/azure/install-azure-cli
[privateendpoints]: networking/private-endpoint.md
[VNETnsg]: /azure/virtual-network/security-overview/
