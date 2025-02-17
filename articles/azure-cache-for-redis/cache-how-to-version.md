---
title: Festlegen der Version für Azure Cache for Redis (Vorschau)
description: Erfahren Sie mehr über das Konfigurieren der Redis-Version.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: d1dda6c4c414c79daf8223794c7d89846fad418f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346734"
---
# <a name="set-redis-version-for-azure-cache-for-redis-preview"></a>Festlegen der Version für Azure Cache for Redis (Vorschau)
In diesem Artikel erfahren Sie, wie Sie die Version der Redis-Software konfigurieren, die mit Ihrer Cache-Instanz verwendet werden soll. Azure Cache for Redis bietet die neueste Hauptversion von Redis und mindestens eine frühere Version. Diese Versionen werden regelmäßig aktualisiert, sobald neuere Redis-Software veröffentlicht wird. Sie können zwischen den beiden verfügbaren Versionen wählen. Beachten Sie, dass automatisch ein Upgrade Ihres Caches auf die nächste Version erfolgt, wenn die derzeit verwendete Version nicht mehr unterstützt wird.

> [!NOTE]
> Redis 6 befindet sich derzeit in der Vorschauphase. Derzeit unterstützt Redis 6 keine Clustering-, Zonenredundanz-, ACL-, PowerShell-, Azure CLI-, Terraform- und Georeplikationsfunktionen zwischen einem Redis 4.0- und 6.0-Cache. Die Redis-Version kann auch nicht mehr geändert werden, nachdem ein Cache erstellt wurde. 
>

> [!IMPORTANT]
> Sobald Redis 6.0 allgemein verfügbar (GA) ist, wird Redis 6.0 die Redis-Standardversion für neue Caches. Sie haben weiterhin die Möglichkeit, Redis 4.0-Caches zu erstellen. 
>

## <a name="prerequisites"></a>Voraussetzungen
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)

## <a name="create-a-cache"></a>Erstellen eines Caches
Führen Sie die folgenden Schritte aus, um einen Cache zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann **Ressource erstellen** aus.
  
1. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann **Azure Cache for Redis** aus.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Auswählen von „Azure Cache for Redis“.":::
   
1. Konfigurieren Sie auf der Seite **Grundlagen** die Einstellungen für den neuen Cache.
   
    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Abonnement** | Wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem diese neue Azure Cache for Redis-Instanz erstellt wird. | 
    | **Ressourcengruppe** | Wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Der Name der Ressourcengruppe, in der Ihr Cache und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
    | **DNS-Name** | Geben Sie einen global eindeutigen Namen ein. | Der Cachename muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und Bindestriche enthalten. Der Name muss mit einer Zahl oder einem Buchstaben beginnen und enden und darf keine aufeinanderfolgenden Bindestriche enthalten. Der *Hostname* Ihrer Cache-Instanz lautet *\<DNS name>.redis.cache.windows.net*. | 
    | **Location** | Wählen Sie einen Standort aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihren Cache verwenden. |
    | **Cachetyp** | Wählen Sie einen [Tarif und eine Größe für den Cache](https://azure.microsoft.com/pricing/details/cache/) aus. |  Der Tarif bestimmt Größe, Leistung und verfügbare Features für den Cache. Weitere Informationen finden Sie unter [What is Azure Cache for Redis](cache-overview.md) (Was ist Azure Cache for Redis?). |
   
1. Wählen Sie auf der Seite **Erweitert** eine zu verwendende Redis-Version aus.
   
    :::image type="content" source="media/cache-how-to-version/select-redis-version.png" alt-text="Redis-Version.":::

1. Klicken Sie auf **Erstellen**. 
   
    Es dauert eine Weile, bis der Cache erstellt wird. Sie können den Fortschritt auf der Seite **Übersicht** von Azure Cache for Redis überwachen. Wenn **Wird ausgeführt** als **Status** angezeigt wird, ist der Cache einsatzbereit.

    > [!NOTE]
    > Zu diesem Zeitpunkt kann die Redis-Version nicht mehr geändert werden, sobald ein Cache erstellt wurde.
    >

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-features-arent-supported-with-redis-6"></a>Welche Features werden mit Redis 6 nicht unterstützt?

Derzeit unterstützt Redis 6 keine Clustering-, Zonenredundanz-, ACL-, PowerShell-, Azure CLI-, Terraform- und Georeplikationsfunktionen zwischen einem Redis 4.0- und 6.0-Cache. 

### <a name="can-i-change-the-version-of-my-cache-after-its-created"></a>Kann ich die Version meines Caches ändern, nachdem er erstellt wurde?

Derzeit können Sie die Version Ihres Caches nicht mehr ändern, nachdem er erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Azure Cache for Redis-Features.

> [!div class="nextstepaction"]
> [Premium-Dienstebenen für Azure Cache for Redis](cache-overview.md#service-tiers)
