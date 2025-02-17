---
title: 'Azure Defender für DNS: Vorteile und Features'
description: Enthält eine Beschreibung der Vorteile und Features von Azure Defender für DNS.
author: memildin
ms.author: memildin
ms.date: 07/25/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 48306c8ec46274bc123ada4f7c8f748a230edf30
ms.sourcegitcommit: 63f3fc5791f9393f8f242e2fb4cce9faf78f4f07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2021
ms.locfileid: "114690820"
---
# <a name="introduction-to-azure-defender-for-dns"></a>Einführung in Azure Defender für DNS

[Azure DNS](../dns/dns-overview.md) ist ein Hostingdienst für DNS-Domänen, der eine Namensauflösung mittels Microsoft Azure-Infrastruktur bietet. Indem Sie Ihre Domänen in Azure hosten, können Sie Ihre DNS-Einträge unter Verwendung der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsabläufe wie bei Ihren anderen Azure-Diensten verwalten.

Azure Defender für DNS bietet durch folgende Maßnahmen eine zusätzliche Schutzebene für Ihre Ressourcen, die mit Azure DNS verbunden sind:

- Kontinuierliche Überwachung aller DNS-Abfragen Ihrer Azure-Ressourcen
- Durchführung erweiterter Sicherheitsanalysen, um Sie auf verdächtige Aktivitäten aufmerksam zu machen

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|**Azure Defender für DNS** wird gemäß [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/) abgerechnet.|
|Clouds:|:::image type="icon" source="./media/icons/yes-icon.png"::: Kommerzielle Clouds<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure China<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure Government|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-dns"></a>Welche Vorteile bietet Azure Defender für DNS?

Azure Defender für DNS schützt Ressourcen, die mit Azure DNS verbunden sind, u. a. gegen folgende Probleme:

- Datenexfiltration aus Ihren Azure-Ressourcen mittels DNS-Tunneling
- Schadsoftware, die mit dem C&C-Server kommuniziert
- Kommunikation mit schädlichen Domänen (beispielsweise Phishing und Kryptografiemining)
- DNS-Angriffe (Kommunikation mit schädlichen DNS-Resolvern) 

Eine vollständige Liste der Warnungen, die von Azure Defender für DNS bereitgestellt werden, finden Sie auf der [Referenzseite zu Warnungen](alerts-reference.md#alerts-dns).

## <a name="dependencies"></a>Abhängigkeiten

Von Azure Defender für DNS werden keine Agents verwendet. 

Aktivieren Sie Azure Defender für DNS zum Schutz Ihrer DNS-Schicht für jedes Ihrer Abonnements, wie unter [Aktivieren von Azure Defender](enable-azure-defender.md) beschrieben.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie sich über Azure Defender für DNS informiert. Verwandte Informationen finden Sie im folgenden Artikel: 

- Sicherheitswarnungen können von Security Center generiert oder von Security Center aus anderen Sicherheitsprodukten empfangen werden. Gehen Sie wie unter [Fortlaufendes Exportieren von Security Center-Daten](continuous-export.md) beschrieben vor, um alle diese Warnungen in Azure Sentinel, in ein beliebiges Drittanbieter-SIEM oder in ein beliebiges anderes externes Tool zu exportieren.

- > [!div class="nextstepaction"]
    > [Aktivieren von Azure Defender](enable-azure-defender.md)