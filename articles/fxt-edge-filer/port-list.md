---
title: Portliste für Azure FXT Edge Filer
description: Liste der von FXT-Clusterumgebungen verwendeten TCP/UDP-Ports
author: ekpgh
ms.author: v-erkel
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 05/26/2021
ms.openlocfilehash: 28de9732f6a22f730059c08b5be939c23e52ebaa
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111414953"
---
# <a name="required-network-ports"></a>Erforderliche Netzwerkports

Diese Liste enthält TCP/UDP-Ports, die für FXT-Clusterumgebungen erforderlich sind. Konfigurieren Sie Firewalls, mit denen Sie sicherstellen, dass auf diese Ports zugegriffen werden kann.

Die genauen Anforderungen Ihres Systemen variieren abhängig davon, welche Art Back-End-Speicher Sie verwenden.

Weitere Informationen erhalten Sie vom Kundenservice und -support von Microsoft.

## <a name="api-ports"></a>API-Ports

| Direction | Typ | Portnummer | Protocol |
|-----------|------|-------------|----------|
| Eingehend   | TCP  | 22          | SSH      |
| Ausgehend  | TCP  | 80          | HTTP     |
| Eingehend und Ausgehend  | TCP  | 443         | HTTPS    |

## <a name="nfs-ports"></a>NFS-Ports

Eingehende NFS-Ports:

| Typ    | Portnummer | Dienst  |
|---------|-------------|----------|
| TCP/UDP | 111         | RPCBIND  |
| TCP/UDP | 2049        | NFS      |
| TCP/UDP | 4045        | NLOCKMGR |
| TCP/UDP | 4046        | MOUNTD   |
| TCP/UDP | 4047        | STATUS   |

Ausgehende NFS-Ports:

| Typ    | Portnummer | Dienst  |
|---------|-------------|----------|
| TCP/UDP | 111         | RPCBIND  |
| TCP/UDP | 2049        | NFS      |

Der Datenverkehr über ausgehende NFS-Ports variiert abhängig von der Art des Speichers, der als Kernspeichereinheit verwendet wird. (Einige Systeme verwenden Port 2049 nicht, er wird aber häufig genug verwendet, um hier aufgeführt zu werden. Alle Cluster müssen auf Port 111 zugreifen können.) Sehen Sie sich die Dokumentation für Speichersysteme an, oder verwenden Sie die weiter unten unter [Zusätzliche Portanforderungen](#additional-port-requirements) beschriebene Abfragemethodik.

Für bestimmten ausgehenden NFS-Datenverkehr von FXT-Knoten werden kurzlebige Ports verwendet. Ausgehender FXT-Datenverkehr über bekannte Ports sollte nicht auf Transportebene eingeschränkt werden.

## <a name="smb-ports"></a>SMB-Ports

| Direction | Typ | Portnummer | Protocol |
|-----------|------|-------------|----------|
| Eingehend und Ausgehend  | UDP  | 137         | NetBIOS  |
| Eingehend   | UDP  | 138         | NetBIOS  |
| Eingehend und Ausgehend  | TCP  | 139         | SMB      |
| Eingehend und Ausgehend  | TCP  | 445         | SMB      |

<!--| Outbound  | UDP  | 137         | NetBIOS  | 
| Outbound  | TCP  | 139         | SMB      |
| Outbound  | TCP  | 445         | SMB      |
-->

## <a name="general-traffic-ports"></a>Allgemeine Datenverkehrsports

| Direction | Typ    | Portnummer | Protocol |
|-----------|---------|-------------|----------|
| Ausgehend  | TCP/UDP | 53          | DNS      |
| Ausgehend  | TCP/UDP | 88          | Kerberos |
| Ausgehend  | UDP     | 123         | NTP      |
| Ausgehend  | TCP/UDP | 389         | LDAP     |
| Ausgehend  | TCP     | 686         | LDAPS    |

## <a name="additional-port-requirements"></a>Zusätzliche Portanforderungen

Ihre Kernspeichereinheiten benötigen möglicherweise Zugriff auf weitere Ports. Diese Anforderung variiert abhängig von der Art des verwendeten Speichers.

Sie können den Befehl `rpcinfo` verwenden, um zu ermitteln, welche Ports von einem bestimmten Server verwendet werden. Führen Sie diesen Befehl in einem Clientsystem aus, das über keine Firewall verfügt:

`rpcinfo -p <server_IP_address>`

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie dem Azure FXT Edge Filer-Cluster [Speicher hinzufügen](add-storage.md).
* [Wenden Sie sich an den Support](support-ticket.md), um mehr über die Portkonfiguration zu erfahren.
