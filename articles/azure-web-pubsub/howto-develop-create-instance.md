---
title: Erstellen der Azure Web PubSub-Instanz
description: Eine Übersicht über die Optionen zum Erstellen einer Azure Web PubSub-Instanz und die Vorgehensweise
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: how-to
ms.date: 03/17/2021
ms.openlocfilehash: f3ed823d989abcb32ad7680e902fc5d52200e726
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111951046"
---
# <a name="how-to-create-azure-web-pubsub-instance"></a>Erstellen einer Azure Web PubSub-Instanz

Um eine Anwendung mit dem Azure Web PubSub-Dienst zu erstellen, müssen Sie die Web PubSub-Instanz erstellen und dann Ihre Clients und Server verbinden. In dieser Anleitung werden die Optionen zum Erstellen einer Azure Web PubSub-Instanz erläutert.

## <a name="create-azure-web-pubsub-instance-with-azure-portal"></a>Erstellen einer Azure Web PubSub-Instanz mit Azure-Portal 

Das [Azure-Portal](../azure-portal/index.yml) ist eine webbasierte, zentrale Konsole, die eine Alternative zu Befehlszeilentools darstellt. Sie können Ihr Azure-Abonnement mit dem Azure-Portal verwalten. Erstellen, Verwalten und Überwachen aller Komponenten – von einfachen Web-Apps bis hin zu komplexen Cloudbereitstellungen. Sie können auch eine Azure Web PubSub-Dienstinstanz mit Azure-Portal erstellen.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche „Neu“. Geben Sie im Bildschirm „Neu“ den Begriff *Web PubSub* in das Suchfeld ein, und drücken Sie die EINGABETASTE. (Sie können Azure Web PubSub auch in der Kategorie `Web` durchsuchen.)

:::image type="content" source="media/create-instance-portal/search-web-pubsub-in-portal.png" alt-text="Screenshot: Durchsuchen von Azure Web PubSub im Portal.":::

2. Wählen Sie in den Suchergebnissen **Web PubSub** und dann **Erstellen** aus.

3. Geben Sie folgende Einstellungen ein.

    | Einstellung      | BESCHREIBUNG                                        |
    | ------------ | -------------------------------------------------- |
    | **Ressourcenname** | Der global eindeutige Name, welcher Ihre neue Web PubSub-Dienstinstanz identifiziert. Gültige Zeichen sind `a-z`, `0-9` und `-`.  | 
    | **Abonnement** | Das Azure-Web PubSubAbonnement, unter dem diese neue Web PubSub-Dienstinstanz erstellt wird. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  Name für die neue oder vorhandene Ressourcengruppe, in der Ihre Web PubSub-Dienstinstanz erstellt werden soll. | 
    | **Location** | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus. |
    | **Preisstufe** | Erfahren Sie mehr über die Tarife die [Preisebenen des Azure Web PubSub-Dienstes](https://azure.microsoft.com/pricing/details/web-pubsub/). |
    | **Einheitenanzahl** |  Die Einheitenanzahl gibt an, wie viele Verbindungen Ihre Instanz des Web PubSub-Dienstes akzeptieren kann. Jede Einheit unterstützt höchstens 1.000 gleichzeitige Verbindungen. Dies kann nur im Tarif „Standard“ konfiguriert werden. |

:::image type="content" source="media/howto-develop-create-instance/create-web-pubsub-instance-in-portal.png" alt-text="Screenshot: Erstellen der Azure Web PubSub-Instanz im Portal.":::

4. Wählen Sie **Erstellen** aus, um mit der Bereitstellung der Instanz des Web PubSub-Dienstes zu beginnen.

## <a name="create-azure-web-pubsub-instance-with-azure-cli"></a>Erstellen einer Azure Web PubSub-Instanz mit Azure CLI

Die [Azure-Befehlszeilenschnittstelle (Azure CLI)](/cli/azure) setzt sich aus Befehlen zum Erstellen und Verwalten von Azure-Ressourcen zusammen. Die Azure CLI ist in allen Azure-Diensten verfügbar und soll die schnelle Verwendung von Azure ermöglichen. Der Fokus liegt dabei auf der Automatisierung. Sie können auch eine Azure Web PubSub-Dienstinstanz mit Azure CLI nach GA erstellen.