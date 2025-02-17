---
title: 'Tutorial: Erstellen von Site-to-Site-Verbindungen mithilfe von Azure Virtual WAN'
description: Erfahren Sie, wie Sie Azure Virtual WAN verwenden, um eine Site-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/15/2021
ms.author: cherylmc
ms.openlocfilehash: 8fb3734e1975254442fa2aff57ba60847bbfac04
ms.sourcegitcommit: 47ac63339ca645096bd3a1ac96b5192852fc7fb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114362251"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Tutorial: Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE-VPN-Verbindung (IKEv1 und IKEv2) nutzen. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen WAN
> * Erstellen eines Hubs
> * Erstellen eines Site-to-Site-VPN-Gateways
> * Erstellen einer Site
> * Herstellen einer Verbindung zwischen einer Site und einem Hub
> * Herstellen einer VPN-Verbindung zwischen einer Site und einem Hub
> * Verbinden eines VNET mit einem Hub
> * Herunterladen einer Konfigurationsdatei
> * Konfigurieren Ihres VPN-Gateways

> [!NOTE]
> Falls Sie über viele Sites verfügen, verwenden Sie normalerweise einen [Virtual WAN-Partner](https://aka.ms/virtualwan), um diese Konfiguration zu erstellen. Sie können diese Konfiguration aber auch selbst erstellen, wenn Sie mit Netzwerken vertraut sind und sich mit der Konfiguration Ihres eigenen VPN-Geräts auskennen.
>

:::image type="content" source="./media/virtual-wan-about/virtualwan.png" alt-text="Screenshot: Netzwerkdiagramm für Virtual WAN":::

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

[!INCLUDE [Before you begin](../../includes/virtual-wan-before-include.md)]

## <a name="create-a-virtual-wan"></a><a name="openvwan"></a>Erstellen eines Virtual WAN

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-hub"></a><a name="hub"></a>Erstellen eines Hubs

Ein Hub ist ein virtuelles Netzwerk, das Gateways für Verbindungen vom Typ „Site-to-Site“, „ExpressRoute“ oder „Point-to-Site“ enthalten kann. Nachdem der Hub erstellt wurde, werden Ihnen für den Hub auch dann Kosten berechnet, wenn Sie keine Websites zuordnen.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-s2s-hub-include.md)]

## <a name="create-a-site-to-site-vpn-gateway"></a><a name="gateway"></a>Erstellen eines Site-to-Site-VPN-Gateways

[!INCLUDE [Create a gateway](../../includes/virtual-wan-tutorial-s2s-gateway-include.md)]

## <a name="create-a-site"></a><a name="site"></a>Erstellen einer Site

In diesem Abschnitt wird eine Site erstellt. Sites entsprechen Ihren physischen Standorten. Sie können beliebig viele davon erstellen. Erstellen Sie beispielsweise drei separate Sites, wenn Sie jeweils über eine Filiale in New York, London und Los Angeles verfügen. Diese Sites enthalten Ihre lokalen VPN-Geräteendpunkte. In einem virtuellen WAN können bis zu 1.000 Sites pro virtuellem Hub erstellt werden. Bei mehreren Hubs ist die Erstellung von 1.000 Sites pro Hub möglich. Falls Sie über ein CPE-Gerät eines Virtual WAN-Partners verfügen, können Sie sich bei dem Partner über die Automatisierungsmöglichkeiten in Azure informieren. Automatisierung impliziert in der Regel einen einfachen Klickvorgang, um umfassende Branchinformationen nach Azure zu exportieren und die Konnektivität vom CPE zum Azure Virtual WAN-VPN-Gateway einzurichten. Weitere Informationen finden Sie unter [Virtual WAN-Partner](virtual-wan-configure-automation-providers.md).

[!INCLUDE [Create a site](../../includes/virtual-wan-tutorial-s2s-site-include.md)]

## <a name="connect-the-vpn-site-to-the-hub"></a><a name="connectsites"></a>Herstellen einer Verbindung von der VPN-Site mit dem Hub

In diesem Schritt stellen Sie für Ihre VPN-Site eine Verbindung mit dem Hub her.

[!INCLUDE [Connect VPN sites](../../includes/virtual-wan-tutorial-s2s-connect-vpn-site-include.md)]

## <a name="connect-the-vnet-to-the-hub"></a><a name="vnet"></a>Herstellen einer Verbindung zwischen VNET und Hub

[!INCLUDE [Connect](../../includes/virtual-wan-connect-vnet-hub-include.md)]

## <a name="download-vpn-configuration"></a><a name="device"></a>Herunterladen der VPN-Konfiguration

Verwenden Sie die VPN-Gerätekonfiguration, um Ihr lokales VPN-Gerät zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Übersicht**.
2. Klicken Sie oben auf der Seite **Hub > VPNSite** auf **VPN-Konfiguration herunterladen**. Azure erstellt ein Speicherkonto in der Ressourcengruppe „microsoft-network-[location]“, wobei „location“ für den WAN-Standort steht. Nachdem Sie die Konfiguration auf Ihre VPN-Geräte angewendet haben, können Sie dieses Speicherkonto löschen.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Wenden Sie die Konfiguration auf Ihr lokales VPN-Gerät an.

### <a name="about-the-vpn-device-configuration-file"></a>Informationen zur VPN-Gerätekonfigurationsdatei

Die Gerätekonfigurationsdatei enthält die Einstellungen, die beim Konfigurieren Ihrer lokalen VPN-Geräte verwendet werden. Beachten Sie beim Anzeigen dieser Datei die folgenden Informationen:

* **vpnSiteConfiguration**: In diesem Abschnitt sind die Gerätedetails für die Einrichtung einer Site angegeben, für die eine Verbindung mit dem virtuellen WAN hergestellt wird. Sie enthält den Namen und die öffentliche IP-Adresse des Zweigstellengeräts.
* **vpnSiteConnections**: Dieser Abschnitt enthält die folgenden Einstellungen:

    * **Adressraum** des virtuellen Hub-VNET.<br>Beispiel:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adressraum** der VNETs, die mit dem Hub verbunden sind.<br>Beispiel:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.3.0.0/16"]
         ```
    * **IP-Adressen** des vpngateway für den virtuellen Hub. Da jede Verbindung des vpngateway aus zwei Tunneln mit Aktiv-Aktiv-Konfiguration besteht, werden in dieser Datei beide IP-Adressen aufgelistet. In diesem Beispiel werden für jede Site „Instance0“ und „Instance1“ angezeigt.<br>Beispiel:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Details zur Konfiguration der vpngateway-Verbindung**, z.B. BGP, vorinstallierter Schlüssel usw. Der vorinstallierte Schlüssel (Pre-Shared Key, PSK) wird automatisch für Sie erstellt. Sie können die Verbindung für einen benutzerdefinierten PSK auf der Seite „Übersicht“ jederzeit bearbeiten.
  
### <a name="example-device-configuration-file"></a>Beispiel für Gerätekonfigurationsdatei

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.3.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Konfigurieren Ihres VPN-Geräts

>[!NOTE]
> Wenn Sie mit einer Virtual WAN-Partnerlösung arbeiten, wird die VPN-Gerätekonfiguration automatisch durchgeführt. Der Gerätecontroller ruft die Konfigurationsdatei aus Azure ab und wendet sie auf das Gerät an, um die Verbindung mit Azure einzurichten. Dies bedeutet, dass Sie nicht wissen müssen, wie Sie Ihr VPN-Gerät manuell konfigurieren.
>

Falls Sie eine Anleitung für die Konfiguration Ihres Geräts benötigen, können Sie die Anleitung auf der [Seite mit den Schritten für die VPN-Gerätekonfiguration](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) verwenden. Beachten Sie hierbei aber die folgenden Einschränkungen:

* Die Anleitung auf der Seite für die VPN-Geräte wurde nicht für Virtual WAN geschrieben, aber Sie können die Virtual WAN-Werte aus der Konfigurationsdatei verwenden, um Ihr VPN-Gerät manuell zu konfigurieren. 
* Die herunterladbaren Skripts für die Gerätekonfiguration, die für VPN Gateway bestimmt sind, funktionieren nicht für Virtual WAN, da sich die Konfiguration unterscheidet.
* Für eine neue Virtual WAN-Instanz können IKEv1 und IKEv2 unterstützt werden.
* Für Virtual WAN können sowohl richtlinienbasierte als auch routenbasierte VPN-Geräte und die entsprechenden Geräteanweisungen verwendet werden.

## <a name="configure-your-vpn-gateway"></a><a name="gateway-config"></a>Konfigurieren Ihres VPN-Gateways

Sie können Ihre VPN Gateway-Einstellungen jederzeit anzeigen und konfigurieren, indem Sie **Anzeigen/Konfigurieren** auswählen.

:::image type="content" source="media/virtual-wan-site-to-site-portal/view-configuration-1.png" alt-text="Screenshot: Seite „VPN (Site-to-Site)“ mit einem Pfeil, der auf die Aktion „Anzeigen/Konfigurieren“ zeigt" lightbox="media/virtual-wan-site-to-site-portal/view-configuration-1-expand.png":::

Auf der Seite **VPN-Gateway bearbeiten** werden die folgenden Einstellungen angezeigt:

* Öffentliche IP-Adresse für VPN Gateway (von Azure zugewiesen)
* Private IP-Adresse für VPN Gateway (von Azure zugewiesen)
* BGP-IP-Standardadresse für VPN Gateway (von Azure zugewiesen)
* Konfigurationsoption für benutzerdefinierte BGP-IP-Adresse: Dieses Feld ist für APIPA (Automatic Private IP Addressing) reserviert. Azure unterstützt BGP-IP-Adressen in den Bereichen „169.254.21.*“ und „169.254.22.*“. Azure akzeptiert BGP-Verbindungen in diesen Bereichen, wählt jedoch die Verbindung mit der standardmäßigen BGP-IP-Adresse.

   :::image type="content" source="media/virtual-wan-site-to-site-portal/view-configuration-2.png" alt-text="Screenshot: Seite „VPN Gateway bearbeiten“ mit hervorgehobener Schaltfläche „Bearbeiten“" lightbox="media/virtual-wan-site-to-site-portal/view-configuration-2-expand.png":::

## <a name="clean-up-resources"></a><a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie die von Ihnen erstellten Ressourcen nicht mehr benötigen, können Sie sie löschen. Einige Virtual WAN-Ressourcen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge gelöscht werden. Das Löschen kann bis zu 30 Minuten dauern.

[!INCLUDE [Delete resources](../../includes/virtual-wan-resource-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> * [Virtual WAN – Häufig gestellte Fragen](virtual-wan-faq.md)
