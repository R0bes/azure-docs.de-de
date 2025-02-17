---
title: Verwenden von Entity Behavior Analytics zum Erkennen komplexer Bedrohungen | Microsoft-Dokumentation
description: Aktivieren von User and Entity Behavior Analytics in Azure Sentinel und Konfigurieren von Datenquellen
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2021
ms.author: yelevin
ms.openlocfilehash: 985f1d4edacd81b7567124845836c3d93f976bb2
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340200"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Aktivieren von User and Entity Behavior Analytics (UEBA) in Azure Sentinel 

> [!IMPORTANT]
>
> Die Features „UEBA“ und „Entitätsseiten“ sind nun in **_allen_** Azure Sentinel-Geografien **allgemein verfügbar**. 

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen

So aktivieren oder deaktivieren Sie dieses Feature (diese Voraussetzungen müssen nicht erfüllt sein, um das Feature zu verwenden)

- Der Benutzer muss Mitglied des Azure Active Directory Ihrer Organisation und darf kein Gastbenutzer sein.

- Ihrem Benutzer muss die Rolle **Globaler Administrator** oder **Sicherheitsadministrator** in Azure AD zugewiesen sein.

- Ihrem Benutzer muss mindestens eine der folgenden **Azure-Rollen**  zugewiesen sein ([Weitere Informationen zu Azure RBAC](roles.md)):
    - **Azure Sentinel-Mitwirkender** auf Arbeitsbereichs -oder Ressourcengruppenebene.
    - **Log Analytics-Mitwirkender** auf Ressourcengruppen- oder Abonnementebene.

- In Ihrem Arbeitsbereich dürfen keine Azure-Ressourcensperren aktiviert sein. [Erfahren Sie mehr über Azure-Ressourcensperren](../azure-resource-manager/management/lock-resources.md).

> [!NOTE]
> Zum Hinzufügen von UEBA-Funktionalität zu Azure Sentinel ist keine spezielle Lizenz erforderlich, aber evtl. fallen **zusätzliche Gebühren** an.

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Aktivieren von User and Entity Behavior Analytics

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Entity Behavior** aus.

1. Schalten Sie den Umschalter unter der Überschrift **Aktivieren** auf **Ein** um.

1. Klicken Sie auf die Schaltfläche **Datenquellen auswählen**.

1. Aktivieren Sie im Bereich **Datenquellenauswahl** die Kontrollkästchen neben den Datenquellen, für die Sie UEBA aktivieren möchten, und wählen Sie dann **Anwenden** aus.

    > [!NOTE]
    >
    > In der unteren Hälfte des Bereichs **Datenquellenauswahl** wird eine Liste der von UEBA unterstützten Datenquellen angezeigt, die Sie noch nicht aktiviert haben. 
    >
    > Nachdem Sie UEBA aktiviert haben, können Sie beim Herstellen einer Verbindung mit neuen Datenquellen diese im Bereich des Datenconnectors direkt für UEBA aktivieren (sofern sie UEBA-fähig sind).

1. Wählen Sie **Go to entity search** (Zur Entitätsssuche) aus. Dadurch gelangen Sie zum Bereich für die Entitätssuche, der in Zukunft angezeigt wird, wenn Sie im Hauptmenü von Azure Sentinel **Entity behavior** auswählen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie die User and Entity Behavior Analytics (UEBA) in Azure Sentinel aktivieren und konfigurieren. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
