---
title: Zuweisen von Azure-Rollen mit dem Azure-Portal – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über das Azure-Portal mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
author: rolyon
manager: daveba
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 06/25/2021
ms.author: rolyon
ms.custom: contperf-fy21q3-portal,subject-rbac-steps
ms.openlocfilehash: e6a06c2a2a525e89fd6d7ab61f983f2b91c1cc8a
ms.sourcegitcommit: 1c12bbaba1842214c6578d914fa758f521d7d485
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2021
ms.locfileid: "112989001"
---
# <a name="assign-azure-roles-using-the-azure-portal"></a>Zuweisen von Azure-Rollen über das Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Wenn Sie Administratorrollen in Azure Active Directory zuweisen müssen, finden Sie weitere Informationen unter [Zuweisen von Azure AD-Rollen zu Benutzern](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="choose-experience"></a>Auswählen der Benutzeroberfläche

Azure RBAC bietet eine neue Benutzeroberfläche zum Zuweisen von Azure-Rollen im Azure-Portal, die sich derzeit in der öffentlichen Vorschau befindet. Wenn Sie diese neue Benutzeroberfläche ausprobieren möchten, führen Sie die Schritte auf der Registerkarte **(Vorschau)** aus.

#### <a name="current"></a>[Current](#tab/current/)

## <a name="step-1-identify-the-needed-scope"></a>Schritt 1: Ermitteln des erforderlichen Bereichs

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)] Weitere Informationen finden Sie unter [Grundlegendes zu Bereichen](scope-overview.md).

![Abbildung der Bereichsebenen für Azure RBAC.](../../includes/role-based-access-control/media/scope-levels.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im oberen Suchfeld nach dem Bereich, für den Sie Zugriff gewähren möchten. Suchen Sie beispielsweise nach **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine bestimmten Ressource.

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

    Nachfolgend sehen Sie ein Beispiel für eine Ressourcengruppe.

    ![Screenshot: Übersichtsseite einer Ressourcengruppe.](./media/shared/rg-overview.png)

## <a name="step-2-open-the-add-role-assignment-pane"></a>Schritt 2: Öffnen des Bereichs „Rollenzuweisung hinzufügen“

Die Zuweisung von Rollen zum Gewähren von Zugriff auf Azure-Ressourcen erfolgt in der Regel über die Seite **Zugriffssteuerung (IAM)** . Diese wird auch als Identity & Access Management (IAM) bezeichnet und wird an mehreren Stellen im Azure-Portal angezeigt.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.

    ![Screenshot: Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.](./media/shared/rg-access-control.png)

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**.
   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Screenshot: Menü „Hinzufügen > Rollenzuweisung hinzufügen“.](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Screenshot: Seite „Rollenzuweisung hinzufügen“ mit den Optionen „Rolle“, „Zugriff zuweisen zu“ und „Auswählen“.](../../includes/role-based-access-control/media/add-role-assignment-page.png)

## <a name="step-3-select-the-appropriate-role"></a>Schritt 3: Auswählen der geeigneten Rolle

1. Suchen oder scrollen Sie in der Liste **Rollen**, um die Rolle zu finden, die Sie zuweisen möchten.

    Um Ihnen bei der Ermittlung der entsprechenden Rolle zu helfen, können Sie mit dem Mauszeiger auf das Info-Symbol zeigen, um eine Beschreibung der Rolle anzuzeigen. Weitere Informationen finden Sie im Artikel [Integrierte Azure-Rollen](built-in-roles.md).

   ![Screenshot: Liste „Rolle auswählen“ unter „Rollenzuweisung hinzufügen“.](./media/role-assignments-portal/add-role-assignment-role.png)

1. Klicken Sie, um die Rolle auszuwählen.

## <a name="step-4-select-who-needs-access"></a>Schritt 4: Auswahl zugriffsberechtigter Benutzer

1. Wählen Sie in der Liste **Zugriff zuweisen zu** den Typ des Sicherheitsprinzipals aus, dem Sie den Zugriff zuweisen möchten.

    | type | BESCHREIBUNG |
    | --- | --- |
    | **Benutzer, Gruppe oder Dienstprinzipal** | Wenn Sie die Rolle einem Benutzer, einer Gruppe oder einem Dienstprinzipal (Anwendung) zuweisen möchten, wählen Sie diesen Typ aus. |
    | **Benutzerseitig zugewiesene verwaltete Identität** | Wenn Sie die Rolle einer [benutzerseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) zuweisen möchten, wählen Sie diesen Typ aus. |
    | *Systemseitig zugewiesene verwaltete Identität* | Wenn Sie die Rolle einer [systemseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) zuweisen möchten, wählen Sie die Azure-Dienstinstanz aus, in der sich die verwaltete Identität befindet. |

   ![Screenshot: Auswählen eines Sicherheitsprinzipals unter „Rollenzuweisung hinzufügen“.](./media/role-assignments-portal/add-role-assignment-type.png)

1. Wenn Sie eine benutzerseitig zugewiesene verwaltete Identität oder eine systemseitig zugewiesene verwaltete Identität ausgewählt haben, wählen Sie das **Abonnement** aus, in dem sich die verwaltete Identität befindet.

1. Suchen Sie im Abschnitt **Auswählen** nach dem Sicherheitsprinzipal, indem Sie eine Zeichenfolge eingeben oder durch die Liste scrollen.

   ![Screenshot: Auswählen eines Benutzers unter „Rollenzuweisung hinzufügen“.](./media/role-assignments-portal/add-role-assignment-user.png)

1. Nachdem Sie den Dienstprinzipal gefunden haben, wählen Sie ihn durch Mausklick aus.

## <a name="step-5-assign-role"></a>Schritt 5: Zuweisen einer Rolle

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

1. Überprüfen Sie auf der Registerkarte **Rollenzuweisungen**, ob die Rollenzuweisung in der Liste angezeigt wird.

    ![Screenshot: Rollenzuweisungsliste nach dem Zuweisen einer Rolle.](./media/role-assignments-portal/rg-role-assignments.png)

#### <a name="preview"></a>[(Vorschau)](#tab/preview/)

## <a name="step-1-identify-the-needed-scope-preview"></a>Schritt 1: Ermitteln des erforderlichen Bereichs (Vorschau)

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)] Weitere Informationen finden Sie unter [Grundlegendes zu Bereichen](scope-overview.md).

![Abbildung, die die Bereichsebenen für Azure RBAC in der Benutzeroberfläche der Vorschauversion zeigt.](../../includes/role-based-access-control/media/scope-levels.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im oberen Suchfeld nach dem Bereich, für den Sie Zugriff gewähren möchten. Suchen Sie beispielsweise nach **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine bestimmten Ressource.

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

    Nachfolgend sehen Sie ein Beispiel für eine Ressourcengruppe.

    ![Screenshot: Übersichtsseite einer Ressourcengruppe in der Benutzeroberfläche der Vorschauversion.](./media/shared/rg-overview.png)

## <a name="step-2-open-the-add-role-assignment-page-preview"></a>Schritt 2: Öffnen der Seite „Rollenzuweisung hinzufügen (Vorschau)“

Die Zuweisung von Rollen zum Gewähren von Zugriff auf Azure-Ressourcen erfolgt in der Regel über die Seite **Zugriffssteuerung (IAM)** . Diese wird auch als Identity & Access Management (IAM) bezeichnet und wird an mehreren Stellen im Azure-Portal angezeigt.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.

    ![Screenshot: Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe in der Benutzeroberfläche der Vorschauversion.](./media/shared/rg-access-control.png)

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen (Vorschau)** .

    Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

    ![Screenshot: Menü „Hinzufügen > Rollenzuweisung hinzufügen“ in der Benutzeroberfläche der Vorschauversion.](./media/shared/add-role-assignment-menu-preview.png)

    Die Seite „Rollenzuweisung hinzufügen“ wird geöffnet.

## <a name="step-3-select-the-appropriate-role-preview"></a>Schritt 3: Auswählen der geeigneten Rolle (Vorschau)

1. Wählen Sie auf der Registerkarte **Rollen** eine Rolle aus, die Sie verwenden möchten.

    Sie können nach einer Rolle anhand des Namens oder der Beschreibung suchen. Sie können Rollen auch nach Typ und Kategorie filtern.

   ![Screenshot: Seite „Rollenzuweisung hinzufügen“ mit der Registerkarte „Rollen“ auf der Benutzeroberfläche der Vorschauversion](./media/shared/roles.png)

1. Klicken Sie in der Spalte **Details** auf **Anzeigen**, um weitere Details zu einer Rolle abzurufen.

   ![Screenshot: Bereich „Rollendetails anzeigen“ mit der Registerkarte „Berechtigungen“ in der Benutzeroberfläche der Vorschauversion.](./media/role-assignments-portal/select-role-permissions.png)

1. Klicken Sie auf **Weiter**.

## <a name="step-4-select-who-needs-access-preview"></a>Schritt 4: Auswahl zugriffsberechtigter Benutzer (Vorschau)

1. Wählen Sie auf der Registerkarte **Mitglieder** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus, um die ausgewählte Rolle mindestens einem Azure AD-Benutzer, einer Gruppe oder einem Dienstprinzipal (Anwendung) zuzuweisen.

   ![Screenshot: Seite „Rollenzuweisung hinzufügen“ mit der Registerkarte „Mitglieder“ auf der Benutzeroberfläche der Vorschauversion](./media/shared/members.png)

1. Klicken Sie auf **Mitglieder auswählen**.

1. Suchen Sie nach den Benutzern, Gruppen oder Dienstprinzipalen, und wählen Sie sie aus.

    Sie können im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigename oder E-Mail-Adresse zu durchsuchen.

   ![Screenshot: Bereich „Mitglieder auswählen“ auf der Benutzeroberfläche der Vorschauversion](./media/shared/select-members.png)

1. Klicken Sie auf **Speichern**, um die Benutzer, Gruppen oder Dienstprinzipale der Liste „Mitglieder“ hinzuzufügen.

1. Um die ausgewählte Rolle mindestens einer verwalteten Identität zuzuweisen, wählen Sie **Verwaltete Identität** aus.

1. Klicken Sie auf **Mitglieder auswählen**.

1. Wählen Sie im Bereich **Verwaltete Identitäten auswählen** aus, ob der Typ [Systemseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) oder [Benutzerseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) ist.

1. Suchen Sie nach den verwalteten Identitäten, und wählen Sie sie aus.

    Wenn Sie eine systemseitig zugewiesene verwaltete Identität ausgewählt haben, müssen Sie die Azure-Dienstinstanz auswählen, in der sich die verwaltete Identität befindet.

   ![Screenshot: Hinzufügen systemseitig zugewiesener verwalteter Identitäten mithilfe des Bereichs „Prinzipal auswählen“ in der Benutzeroberfläche der Vorschauversion.](./media/role-assignments-portal/select-managed-identity-system.png)

   ![Screenshot: Hinzufügen benutzerseitig zugewiesener verwalteter Identitäten mithilfe des Bereichs „Prinzipal auswählen“ in der Benutzeroberfläche der Vorschauversion.](./media/role-assignments-portal/select-managed-identity-user.png)

1. Klicken Sie auf **Speichern**, um die verwalteten Identitäten der Liste „Mitglieder“ hinzuzufügen.

1. Geben Sie im Feld **Beschreibung** eine optionale Beschreibung für diese Rollenzuweisung ein.

    Sie können diese Beschreibung später in der Liste der Rollenzuweisungen anzeigen.

1. Klicken Sie auf **Weiter**.

## <a name="step-5-optional-add-condition-preview"></a>Schritt 5: (Optional) Hinzufügen einer Bedingung (Vorschau)

Wenn Sie eine Rolle ausgewählt haben, die Bedingungen unterstützt, wird eine Registerkarte **Bedingung** angezeigt, und Sie haben die Möglichkeit, Ihrer Rollenzuweisung eine Bedingung hinzuzufügen. Eine [Bedingung](conditions-overview.md) ist eine zusätzliche Überprüfung, die Sie ihrer Rollenzuweisung optional hinzufügen können, um eine genauere Zugriffssteuerung zu ermöglichen.

Derzeit können Bedingungen zu integrierten oder benutzerdefinierten Rollenzuweisungen hinzugefügt werden, die über [Aktionen für Speicherblobdaten](conditions-format.md#actions) verfügen. Dies umfasst die folgenden integrierten Rollen:


- [Mitwirkender an Speicherblobdaten](built-in-roles.md#storage-blob-data-contributor)
- [Besitzer von Speicherblobdaten](built-in-roles.md#storage-blob-data-owner)
- [Leser von Speicherblobdaten](built-in-roles.md#storage-blob-data-reader)

1. Klicken Sie auf **Bedingung hinzufügen**, wenn Sie die Rollenzuweisungen basierend auf Speicherblobattributen weiter optimieren möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten von Bedingungen für Azure-Rollenzuweisungen](conditions-role-assignments-portal.md).

   ![Screenshot: Seite „Rollenzuweisung hinzufügen“ mit der Registerkarte „Bedingung hinzufügen“ in der Benutzeroberfläche der Vorschauversion.](./media/shared/condition.png)

1. Klicken Sie auf **Weiter**.

## <a name="step-6-assign-role-preview"></a>Schritt 6: Zuweisen einer Rolle (Vorschau)

1. Überprüfen Sie auf der Registerkarte **Überprüfen und zuweisen** die Einstellungen für die Rollenzuweisung.

   ![Screenshot: Seite „Rolle zuweisen“ mit der Registerkarte „Überprüfen und zuweisen“ in der Benutzeroberfläche der Vorschauversion.](./media/role-assignments-portal/review-assign.png)

1. Klicken Sie auf **Überprüfen und zuweisen**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

    ![Screenshot: Liste der Rollenzuweisungen nach dem Zuweisen einer Rolle in der Benutzeroberfläche der Vorschauversion.](./media/role-assignments-portal/rg-role-assignments.png)

1. Wenn die Beschreibung für die Rollenzuweisung nicht angezeigt wird, klicken Sie auf **Spalten bearbeiten**, um die Spalte **Beschreibung** hinzuzufügen.

---

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Administratorzugriff für ein Azure-Abonnement](role-assignments-portal-subscription-admin.md)
- [Entfernen von Azure-Rollenzuweisungen](role-assignments-remove.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
