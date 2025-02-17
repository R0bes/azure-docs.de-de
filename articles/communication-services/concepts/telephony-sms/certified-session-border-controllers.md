---
title: 'Für Azure Direct Routing zertifizierte Session Border Controller: Azure Communication Services'
description: Liste der für das Direct Routing von Azure Communication Services zertifizierten Session Border Controller und bekannte Einschränkungen
author: boris-bazilevskiy
manager: nmurav
services: azure-communication-services
ms.author: bobazile
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 7251e97209e9cca1b4c86a6ab8c829c8f0346525
ms.sourcegitcommit: 98e126b0948e6971bd1d0ace1b31c3a4d6e71703
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2021
ms.locfileid: "114674897"
---
# <a name="list-of-session-border-controllers-certified-for-azure-communication-services-direct-routing"></a>Liste der für das Direct Routing von Azure Communication Services zertifizierten Session Border Controller
Dieses Dokument enthält eine Liste der für das Direct Routing von Azure Communication Services zertifizierten Session Border Controller. Es enthält außerdem bekannte Einschränkungen.

Microsoft arbeitet mit den ausgewählten SBC-Anbietern (Session Border Controller) zusammen, die für Teams Direct Routing zertifiziert sind, um Azure Direct Routing nutzen zu können. Den Fortschritt können Sie auf dieser Seite verfolgen. Der für Teams Direct Routing zertifizierte SBC kann zwar mit Azure Direct Routing genutzt werden, es wird jedoch empfohlen, keine Workload auf dem SBC zu verwenden, bis er auf dieser Seite angezeigt wird. Außerdem wird der nicht zertifizierte SBC nicht unterstützt. Obwohl Azure Direct Routing auf dem gleichen Back-End wie Teams Direct Routing basiert, gibt es einige Unterschiede. Die Zertifizierung beinhaltet eine umfassende Überprüfung des SBC für Azure Direct Routing.

Microsoft arbeitet mit jedem Anbieter an folgenden Punkten:
- Gemeinsame Arbeit an den SIP-Verbindungsprotokollen
- Durchführen intensiver Tests mithilfe eines Drittanbieterlabs. Nur Geräte, die die Tests bestehen, werden zertifiziert.
- Durchführen täglicher Tests mit allen zertifizierten Geräten in Produktions- und Präproduktionsumgebungen. Die Überprüfung der Geräte in Präproduktionsumgebungen garantiert, dass neue Versionen des Azure Communication Services-Codes in der Cloud mit zertifizierten SBCs funktionieren.
- Einrichten eines gemeinsamen Unterstützungsprozesses mit den SBC-Anbietern

[!INCLUDE [Public Preview](../../includes/public-preview-include-document.md)]

Die Medienumgehung wird in Azure Communication Services noch nicht unterstützt. Early Media werden auf einem webbasierten Client nicht unterstützt.
In der folgenden Tabelle sind die Geräte aufgeführt, die für das Direct Routing von Azure Communication Services zertifiziert sind.

Wenn Sie Fragen zum SBC-Zertifizierungsprogramm für das Direct Routing von Communication Services haben, wenden Sie sich an acsdrcertification@microsoft.com.

## <a name="certified-sbc-vendors"></a>Zertifizierte SBC-Anbieter

|Hersteller|Produkt|Softwareversion|
|:--- |:--- |:--- 
|Metaswitch|Perimeta SBC|4,9|


Beachten Sie die Zertifizierung für eine Hauptversion. Das bedeutet, dass Firmware mit einer beliebigen Zahl in der SBC-Firmware nach der Hauptversion unterstützt wird.

## <a name="next-steps"></a>Nächste Schritte

### <a name="conceptual-documentation"></a>Dokumentation

- [Telefonnummerntypen in Azure Communication Services](./plan-solution.md)
- [Planen des direkten Azure-Routings](./sip-interface-infrastructure.md)
- [Koppeln des Session Border Controllers und Konfigurieren des Sprachroutings](./direct-routing-provisioning.md)
- [Preise](../pricing.md)

### <a name="quickstarts"></a>Schnellstarts

- [Schnellstart: Auf Telefon anrufen](../../quickstarts/voice-video-calling/pstn-call.md)
