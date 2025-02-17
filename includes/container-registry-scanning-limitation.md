---
title: Datei einfügen
description: Datei einfügen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/19/2021
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 83a42f7a68dc34770106e258b4ed5949feb48c8d
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114471489"
---
> [!IMPORTANT]
> In einer Containerregistrierung, die den Zugriff auf private Endpunkte, ausgewählte Subnetze oder IP-Adressen einschränkt, sind einige Funktionen möglicherweise nicht verfügbar oder erfordern eine umfangreichere Konfiguration. 
> * Wenn der öffentliche Netzwerkzugriff auf eine Registrierung deaktiviert ist, erfordert der Zugriff auf die Registrierung durch bestimmte [vertrauenswürdige Dienste](../articles/container-registry/allow-access-trusted-services.md) einschließlich Azure Security Center die Aktivierung einer Netzwerkeinstellung, um die Netzwerkregeln zu umgehen.
> * Instanzen bestimmter Azure-Dienste, darunter Azure DevOps Services und Azure Container Instances, können derzeit nicht auf die Containerregistrierung zugreifen.
> * Wenn die Registrierung einen genehmigten privaten Endpunkt hat und der öffentliche Netzwerkzugriff deaktiviert ist, können Repositorys und Tags außerhalb des virtuellen Netzwerks nicht über das Azure-Portal, die Azure CLI oder andere Tools aufgelistet werden.
