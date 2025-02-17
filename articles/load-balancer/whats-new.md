---
title: Neues in Azure Load Balancer
description: Hier finden Sie Informationen zu den Neuerungen in Azure Load Balancer, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: a132b97822b9c4ca8f6f14b0b45cebc2775fdc86
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114291090"
---
# <a name="whats-new-in-azure-load-balancer"></a>Neues in Azure Load Balancer

Azure Load Balancer wird regelmäßig aktualisiert. Bleiben Sie bei aktuellen Ankündigungen auf dem neuesten Stand. Dieser Artikel bietet Folgendes:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen (sofern zutreffend)

Die aktuellen Updates für Azure Load Balancer finden Sie [hier](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer). Unter diesem Link können Sie auch den RSS-Feed abonnieren.

## <a name="recent-releases"></a>Aktuelle Releases

| type |Name |BESCHREIBUNG  |Hinzufügedatum  |
| ------ |---------|---------|---------|
| Funktion | [Unterstützung für IP-basierte Back-End-Pools (allgemeine Verfügbarkeit)](https://azure.microsoft.com/updates/iplbga/) | Azure Load Balancer unterstützt das Hinzufügen und Entfernen von Ressourcen zu bzw. aus einem Back-End-Pool über IPv4- oder IPv6-Adressen. Dies ermöglicht die einfache Verwaltung von Containern, virtuellen Computern und VM-Skalierungsgruppen, die Load Balancer zugeordnet sind. Darüber hinaus können IP-Adressen als Teil eines Back-End-Pools reserviert werden, bevor die zugehörigen Ressourcen erstellt werden. [Hier](backend-pool-management.md) erhalten Sie weitere Informationen.|März 2021 |
| Funktion | [Unterstützung von Instanzmetadaten für Lastenausgleichsmodule und öffentliche IP-Adressen der Standard-SKU](https://azure.microsoft.com/updates/standard-load-balancer-and-ip-addresses-metadata-now-available-through-azure-instance-metadata-service-imds/)|Metadaten von öffentlichen IP-Adressen und Lastenausgleichsmodulen der Standard-SKU können jetzt über Azure Instance Metadata Service (IMDS) abgerufen werden. Die Metadaten sind in den ausgeführten Instanzen von VMs und in VMSS-Instanzen (VM-Skalierungsgruppen) verfügbar. Sie können die Metadaten für die Verwaltung Ihrer VMs nutzen. Weitere Informationen finden Sie [hier](instance-metadata-service-load-balancer.md).| Februar 2021 |
| Funktion | [Upgrade der SKU für öffentliche IP-Adressen von Basic auf Standard ohne Verlust der IP-Adresse](https://azure.microsoft.com/updates/public-ip-sku-upgrade-generally-available/) | Wenn Sie von Basic zu Load Balancer Standard wechseln, behalten Sie Ihre öffentliche IP-Adresse bei. [Hier](../virtual-network/public-ip-upgrade-portal.md) erhalten Sie weitere Informationen.| Januar 2021|
| Funktion | Unterstützung für Verschiebungen über Ressourcengruppen hinweg | Unterstützung von Load Balancer Standard und öffentlichen IP-Standardadressen für [Verschiebungen von Ressourcengruppen](https://azure.microsoft.com/updates/standard-resource-group-move/). | Oktober 2020 |
| Funktion| Azure Load Balancer Insights using Azure Monitor (Azure Load Balancer-Erkenntnisse mithilfe von Azure Monitor) | Kunden können jetzt für alle ihre Load Balancer-Konfigurationen und Integritätsdashboards topologische Karten nutzen, die als Teil von Azure Monitor für Netzwerke erstellt wurden und sich für Load Balancer Standard-Instanzen eignen, die mit entsprechenden Metriken im Azure-Portal vorkonfiguriert wurden. [Erste Schritte und weitere Informationen](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Juni 2020 |
| Überprüfen | Hinzufügung der Überprüfung für Hochverfügbarkeitsports | Es wurde eine Überprüfung hinzugefügt, um sicherzustellen, dass Hochverfügbarkeitsportregeln und Nicht-Hochverfügbarkeitsportregeln nur konfigurierbar sind, wenn Floating IP aktiviert ist. Bisher wurde diese Konfiguration akzeptiert, funktionierte jedoch nicht wie vorgesehen. Es wurde keine Änderung an der Funktionalität vorgenommen. Weitere Informationen finden Sie [hier](load-balancer-ha-ports-overview.md#limitations).| Juni 2020 |
| Funktion| IPv6 support for Azure Load Balancer (generally available) (IPv6-Unterstützung für Azure Load Balancer (allgemein verfügbar)) | Sie können IPv6-Adressen als Front-End für Ihre Azure Load Balancer-Instanzen verwenden. [Hier](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) erfahren Sie, wie Sie eine Anwendung mit dualem Stapel erstellen. |April 2020|
| Funktion| TCP Resets on Idle Timeout (generally available) (TCP-Zurücksetzungen bei Leerlauftimeout (allgemein verfügbar))| Verwenden Sie TCP-Zurücksetzungen, um ein besser vorhersagbares Anwendungsverhalten zu ermöglichen. [Weitere Informationen](load-balancer-tcp-reset.md)| Februar 2020 |

## <a name="known-issues"></a>Bekannte Probleme

Die Produktgruppe arbeitet aktiv an Lösungen für die folgenden bekannten Probleme:

|Problem |BESCHREIBUNG  |Minderung  |
| ---------- |---------|---------|
| Ausgehende IP-Adresse beim IP-basierten Lastenausgleich | Beim IP-basierten Lastenausgleich wird die standardmäßige Zugriffs-IP-Adresse von Azure für ausgehenden Datenverkehr verwendet, sofern keine Ausgangsregeln konfiguriert sind. | Um ausgehenden Zugriff von dieser IP-Adresse zu verhindern, verwenden Sie Ausgangsregeln oder eine NAT Gateway-Instanz für eine vorhersagbare IP-Adresse und zur Verhinderung der SNAT-Portauslastung. |

  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Load Balancer finden Sie unter [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md) und [Häufig gestellte Fragen zu Load Balancer](load-balancer-faqs.yml).