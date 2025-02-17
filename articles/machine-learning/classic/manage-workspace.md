---
title: 'ML Studio (Classic): Verwalten von Arbeitsbereichen in Azure'
description: Verwalten des Zugriffs auf Machine Learning Studio (Classic)-Arbeitsbereiche und Bereitstellen sowie Verwalten von Machine Learning-API-Webdiensten
services: machine-learning
ms.service: machine-learning
ms.subservice: studio-classic
ms.topic: how-to
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: cf9b2134dc3a043ad12b109a6e0b0e46fde92219
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2021
ms.locfileid: "122693788"
---
# <a name="manage-a-machine-learning-studio-classic-workspace"></a>Verwalten eines Machine Learning Studio-Arbeitsbereichs (klassisch)

**GILT FÜR:**  ![Gilt für ](../../../includes/media/aml-applies-to-skus/yes.png)Machine Learning Studio (Classic) ![Gilt nicht für ](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)

[!INCLUDE [ML Studio (classic) retirement](../../../includes/machine-learning-studio-classic-deprecation.md)]

> [!NOTE]
> Informationen zum Verwalten von Webdiensten im Portal für Machine Learning-Webdienste finden Sie unter [Verwalten eines Webdiensts mithilfe des Portals für Machine Learning Studio (Classic)-Webdienste](manage-new-webservice.md).
> 
> 

Sie können Machine Learning Studio-Arbeitsbereiche (klassisch) im Azure-Portal verwalten.



## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwalten Sie einen Studio-Arbeitsbereich (klassisch) im Azure-Portal

1. Melden Sie sich über das Administratorkonto eines Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie oben auf der Seite im Suchfeld „Machine Learning Studio-Arbeitsbereiche (klassisch)“ ein, und wählen Sie dann **Machine Learning Studio-Arbeitsbereiche (klassisch)** aus.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Neben den Standardinformationen und -optionen zur Ressourcenverwaltung stehen folgende Optionen zur Verfügung:

- **Eigenschaften anzeigen:** Auf dieser Seite werden Informationen zum Arbeitsbereich und der Ressource angezeigt. Zudem können Sie hier das Abonnement und die Ressourcengruppe ändern, mit denen der Arbeitsbereich verbunden ist.
- **Speicherschlüssel neu synchronisieren:** Im Arbeitsbereich werden die Schlüssel für das Speicherkonto verwaltet. Wenn sich die Schlüssel für das Speicherkonto ändern, können Sie auf **Schlüssel neu synchronisieren** klicken, um die Schlüssel mit dem Arbeitsbereich zu synchronisieren.

Verwenden Sie zum Verwalten der diesem Studio-Arbeitsbereich (klassisch) zugeordneten Webdienste das Machine Learning Web Services-Portal. Ausführliche Informationen finden Sie unter [Verwalten eines Webdiensts mithilfe des Portals für Machine Learning Studio (Classic)-Webdienste](manage-new-webservice.md).

> [!NOTE]
> Zum Bereitstellen oder Verwalten neuer Webdienste muss Ihnen in dem Abonnement, für das der Webdienst bereitgestellt wird, eine Teilnehmer- oder Administratorrolle zugewiesen sein. Wenn Sie andere Benutzer in einen Machine Learning Studio-Arbeitsbereich (klassisch) einladen, müssen Sie ihnen eine Teilnehmer- oder Administratorrolle im Abonnement zuweisen, damit sie Webdienste bereitstellen oder verwalten können. 
> 
>Weitere Informationen zum Festlegen von Zugriffsberechtigungen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Bereitstellen von Machine Learning mithilfe von Azure Resource Manager-Vorlagen](deploy-with-resource-manager-template.md).