---
title: Zusammenarbeiten in Microsoft Teams mit einem Azure Sentinel-Incidentteam | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie über Azure Sentinel eine Verbindung zu Microsoft Teams herstellen, um mit anderen Personen in Ihrem Team mithilfe von Azure Sentinel-Daten zusammenzuarbeiten.
services: sentinel
documentationcenter: na
author: batamig
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2021
ms.author: bagol
ms.openlocfilehash: 3aefed370606b80249eabeac35123223b2ad5255
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339218"
---
# <a name="collaborate-in-microsoft-teams-public-preview"></a>Zusammenarbeiten in Microsoft Teams (Public Preview)

Azure Sentinel unterstützt eine direkte Integration in [Microsoft Teams](/microsoftteams/), sodass Sie bei bestimmten Incidents direkt in die Teamarbeit einsteigen können.


> [!IMPORTANT]
> Die Integration in Microsoft Teams befindet sich derzeit in der **PREVIEW**-Phase. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="overview"></a>Übersicht

Die direkte Integration in Microsoft Teams über Azure Sentinel ermöglicht Ihren Teams eine nahtlose Zusammenarbeit innerhalb der gesamten Organisation und mit externen Projektbeteiligten.

Verwenden Sie Microsoft Teams mit einem Azure Sentinel-*Incidentteam*, um Ihre Kommunikation und Koordination zwischen den relevanten Mitarbeitern zu zentralisieren. Incidentteams sind besonders als dedizierte Konferenzbrücke für laufende Incidents mit hohem Schweregrad hilfreich.

Organisationen, die Microsoft Teams bereits für Kommunikation und Zusammenarbeit verwenden, können die Azure Sentinel-Integration nutzen, um Sicherheitsdaten direkt in ihre Konversationen und tägliche Arbeit einzubringen. 

Ein Azure Sentinel-Incidentteam verfügt immer über die aktuellen Daten aus Azure Sentinel. Dadurch wird sichergestellt, dass Ihre Teams stets die relevantesten Daten zur Hand haben.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Erstellen von Teams über Azure Sentinel gilt Folgendes:

- Der Benutzer, der das Team erstellt, muss in Azure Sentinel über Schreibberechtigungen für Incidents verfügen. Die Rolle [Azure Sentinel-Antwortender](../role-based-access-control/built-in-roles.md#azure-sentinel-responder) ist beispielsweise die ideale mindestens erforderliche Rolle für diese Berechtigung.

- Der Benutzer, der das Team erstellt, muss darüber hinaus auch über Berechtigungen zum Erstellen von Teams in Microsoft Teams verfügen.

- Jeder Azure Sentinel-Benutzer, also Benutzer mit der Rolle [Leser](../role-based-access-control/built-in-roles.md#azure-sentinel-reader), [Antwortender](../role-based-access-control/built-in-roles.md#azure-sentinel-responder) oder [Mitwirkender](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor), kann Zugriff auf das erstellte Team erhalten, indem er den Zugriff anfordert.

## <a name="use-an-incident-team-to-investigate"></a>Untersuchungen mithilfe eines Incidentteams

Führen Sie Untersuchungen zusammen mit einem *Incidentteam* durch, indem Sie Microsoft Teams direkt aus dem betreffenden Incident integrieren.

**So erstellen Sie Ihr Incidentteam:**

1. Wählen Sie in Azure Sentinel im Raster **Threat Management** > **Incidents** den Incident aus, den Sie gerade untersuchen.

1. Wählen Sie am unteren Rand des Incidentbereichs, der auf der rechten Seite angezeigt wird, **Aktionen** > **Team erstellen** aus.

    [ ![Erstellen Sie ein Team für die Zusammenarbeit in einem Incidentteam.](media/collaborate-in-microsoft-teams/create-team.png) ](media/collaborate-in-microsoft-teams/create-team.png#lightbox)

    Der Bereich **Neues Team** wird auf der rechten Seite geöffnet. Definieren Sie die folgenden Einstellungen für Ihr Incidentteam:

    - **Teamname**: Wird automatisch als der Name des betreffenden Incidents definiert. Ändern Sie den Namen nach Bedarf, damit er für Sie leicht identifizierbar ist.
    - **Beschreibung**: Geben Sie eine aussagekräftige Beschreibung für Ihr Incidentteam ein.
    - **Gruppen hinzufügen**: Wählen Sie eine oder mehrere Azure AD-Gruppen aus, die dem Incidentteam hinzugefügt werden sollen. Einzelne Benutzer werden auf dieser Seite nicht unterstützt. Wenn Sie einzelne Benutzer hinzufügen müssen, [tun Sie dies in Microsoft Teams](#more-users), nachdem Sie das Team erstellt haben.

        > [!TIP]
        > Wenn Sie regelmäßig mit denselben Teams zusammenarbeiten, sollten Sie den Stern :::image type="icon" source="media/collaborate-in-microsoft-teams/save-as-favorite.png" border="false"::: auswählen, um sie als Favoriten zu speichern.
        >
        > Favoriten werden automatisch ausgewählt, wenn Sie das nächste Mal ein Team erstellen. Wenn Sie die Favoritenoption von dem nächsten Team, das Sie erstellen, entfernen möchten, klicken Sie entweder auf **Löschen** :::image type="icon" source="media/collaborate-in-microsoft-teams/delete-user-group.png" border="false":::oder wählen den Stern erneut :::image type="icon" source="media/collaborate-in-microsoft-teams/save-as-favorite.png" border="false"::: aus, um das Team vollständig aus den Favoriten zu entfernen.
        >

1. Wenn Sie mit dem Hinzufügen von Gruppen fertig sind, wählen Sie **Erstellen** aus, um das Incidentteam zu erstellen.

    Der Incidentbereich wird aktualisiert und zeigt nun einen Link zu Ihrem neuen Incidentteam unter dem Titel **Teamname** an.

    [ ![Klicken Sie auf den Link „Teams integration“, der dem Incident hinzugefügt wurde.](media/collaborate-in-microsoft-teams/teams-link-added-to-incident.jpg) ](media/collaborate-in-microsoft-teams/teams-link-added-to-incident.jpg#lightbox)


1. Wählen Sie den Link **Teams integration** aus, um zu Microsoft Teams zu wechseln. Dort werden alle Daten zu dem betreffenden Incident auf der Registerkarte **Incident page** aufgeführt.

    [ ![Incidentseite in Microsoft Teams.](media/collaborate-in-microsoft-teams/incident-in-teams.jpg) ](media/collaborate-in-microsoft-teams/incident-in-teams.jpg#lightbox)

Setzen Sie die Konversation über die Untersuchung in Teams so lange wie nötig fort. Ihnen stehen sämtliche Details zum Incident in Teams zur Verfügung.

> [!TIP]
> - <a name="more-users"></a>Wenn Sie Ihrem Team einzelne Benutzer hinzufügen müssen, können Sie dies in Microsoft Teams über die Schaltfläche **Add more people** (Weitere Personen hinzufügen) auf der Registerkarte **Posts** (Beiträge) tun.
>
> - Wenn Sie [einen Incident schließen](investigate-cases.md#closing-an-incident), wird das zugehörige Incidentteam, das Sie in Microsoft Teams erstellt haben, archiviert. Wenn der Incident jemals erneut geöffnet wird, wird auch das zugehörige Incidentteam erneut in Microsoft Teams geöffnet, sodass Sie Ihre Konversation direkt an der Stelle, an der Sie aufgehört haben, fortsetzen können.
>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter:

- [Tutorial: Untersuchen von Vorfällen mit Azure Sentinel](investigate-cases.md)
- [Übersicht über Teams und Kanäle in Microsoft Teams](/microsoftteams/teams-channels-overview/)
