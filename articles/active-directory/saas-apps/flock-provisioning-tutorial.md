---
title: 'Tutorial: Konfigurieren von Flock für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Flock konfigurieren.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/30/2019
ms.author: thwimmer
ms.openlocfilehash: 57942b7f682f5715239eb4d8a471008b9f475e65
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113767936"
---
# <a name="tutorial-configure-flock-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Flock für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Flock und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Flock zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten.
* [Einen Flock-Mandanten](https://flock.com/pricing/)
* Ein Benutzerkonto in Flock mit Administratorrechten.

## <a name="assigning-users-to-flock"></a>Zuweisen von Benutzern zu Flock 

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Flock benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Flock wie folgt zuweisen:
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-flock"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Flock 

* Es wird empfohlen, Flock einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Flock müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="setup-flock--for-provisioning"></a>Einrichten von Flock für die Bereitstellung

Bevor Sie Flock für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in Flock die SCIM-Bereitstellung aktivieren.

1. Melden Sie sich bei [Flock](https://web.flock.com/?) an. Klicken Sie auf das **Einstellungssymbol** > **Manage your team** (Team verwalten).

    :::image type="content" source="media/flock-provisioning-tutorial/icon.png" alt-text="Screenshot der Flock-Website. Das Symbol für Einstellungen ist hervorgehoben, und das Kontextmenü wird angezeigt. In diesem Menü ist die Option „Team verwalten“ hervorgehoben." border="false":::

2. Wählen Sie **Auth and Provisioning** (Authentifizierung und Bereitstellung) aus.

    :::image type="content" source="media/Flock-provisioning-tutorial/auth.png" alt-text="Screenshot eines Menüs auf der Flock-Website. Das Element „Authentifizierung und Bereitstellung“ ist hervorgehoben." border="false":::

3. Kopieren Sie das **API-Token**. Diese Werte werden im Azure-Portal auf der Registerkarte „Bereitstellung“ Ihrer Flock-Anwendung in das Feld **Geheimes Token** eingegeben.

    :::image type="content" source="media/Flock-provisioning-tutorial/provisioning.png" alt-text="Screenshot der Registerkarte „Bereitstellung“ auf der Flock-Website. Unter „API-Token“ ist ein Wert hervorgehoben. Neben dem Token befindet sich die Schaltfläche „Token kopieren“." border="false":::


## <a name="add-flock--from-the-gallery"></a>Hinzufügen von Flock aus dem Katalog

Bevor Sie Flock für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Flock aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie die folgenden Schritte aus, um Flock aus dem Azure AD-Anwendungskatalog hinzuzufügen:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Flock** ein, wählen Sie im Ergebnisbereich **Flock** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Flock in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-flock"></a>Konfigurieren der automatischen Benutzerbereitstellung in Flock  

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Flock auf der Grundlage von Benutzer- bzw. Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Flock aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Flock](Flock-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-flock--in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Flock in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Flock** aus.

    ![Der Flock-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Screenshot der Optionen zum Verwalten mit aufgerufener Bereitstellungsoption](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Screenshot der Dropdownliste „Bereitstellungsmodus“ mit aufgerufener Option „Automatisch“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt „Administratoranmeldeinformationen“ die zuvor abgerufenen Werte von `https://api.flock-staging.com/v2/scim` und **API-Token** in das Feld **Mandanten-URL** bzw. **Geheimes Token** ein. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Flock herstellen kann. Vergewissern Sie sich im Falle eines Verbindungsfehlers, dass Ihr Flock-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Mandanten-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Flock synchronisieren**.

    ![Benutzerzuordnungen in Flock](media/flock-provisioning-tutorial/usermapping.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Flock synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Flock für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Flock-Benutzerattribute](media/flock-provisioning-tutorial/userattribute.png)

11. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Um den Azure AD-Bereitstellungsdienst für Flock zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

13. Legen Sie die Benutzer und/oder Gruppen fest, die in Flock bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

14. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung nimmt mehr Zeit in Anspruch als die nachfolgenden Synchronisierungen. Unter [Überprüfen des Status der Benutzerbereitstellung](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users) finden Sie weitere Informationen dazu, wie lange das Bereitstellen von Benutzern und/oder Gruppen dauert.

Im Abschnitt **Aktueller Status** können Sie den Fortschritt überwachen und Links zu Ihrem Bereitstellungsaktivitätsbericht folgen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst für Flock ausgeführt werden. Weitere Informationen finden Sie unter [Ermitteln, wann ein bestimmter Benutzer auf eine Anwendung zugreifen kann](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Informationen zum Lesen der Azure AD-Bereitstellungsprotokolle finden Sie unter [Tutorial: Berichterstellung zur automatischen Benutzerkontobereitstellung](../app-provisioning/check-status-user-account-provisioning.md).



## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)