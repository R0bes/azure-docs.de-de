---
title: 'Tutorial: Azure Active Directory-Integration mit Origami | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Origami konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/08/2021
ms.author: jeedes
ms.openlocfilehash: cb00f7e5789e7c5ff4a1e3a7fc8265b1ce7edadf
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111888029"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Tutorial: Azure Active Directory-Integration mit Origami

In diesem Tutorial erfahren Sie, wie Sie Origami in Azure Active Directory (Azure AD) integrieren. Die Integration von Origami in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Origami hat.
* Ermöglichen Sie Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Origami anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) bei Origami aktiviert ist.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Origami unterstützt **SP**-initiiertes einmaliges Anmelden.

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-origami-from-the-gallery"></a>Hinzufügen von Origami aus dem Katalog

Zum Konfigurieren der Integration von Origami in Azure AD müssen Sie Origami aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Origami** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Origami** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-origami"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Origami

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Origami mithilfe eines Testbenutzers namens **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Origami eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Origami die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Origami](#configure-origami-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines Origami-Testbenutzers](#create-origami-test-user)** , um in Origami einen Gegenpart von B. Simon zu erhalten, der mit der Darstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Origami** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://live.origamirisk.com/origami/account/login?account=<COMPANY_NAME>`

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam für den Origami-Client](https://wordpress.org/support/theme/origami), um den Wert zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Origami einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Origami gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Origami** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-origami-sso"></a>Konfigurieren des einmaligen Anmeldens für Origami

1. Melden Sie sich mit Administratorrechten beim Origami-Konto an.

2. Klicken Sie oben im Menü auf **Administrator**.
   
    ![Screenshot: Origami-Homepage mit Auswahl von „Administrator“](./media/origami-tutorial/admin.png)

3. Führen Sie auf der Dialogfeldseite „Single Sign On Setup“ die folgenden Schritte aus:
   
    ![Screenshot: Seite „Single Sign On Setup“ (Setup für einmaliges Anmelden) mit Auswahl von „Enable Single Sign On“ (Einmaliges Anmelden aktivieren) und hervorgehobenen Textfeldern](./media/origami-tutorial/configuration.png)

    a. Wählen Sie **Enable Single Sign On** aus.

    b. Fügen Sie im Textfeld **Identity Provider‘s Sign-in Page URL** (URL der Anmeldeseite des Identitätsanbieters) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie im Textfeld **Identity Provider‘s Sign-out Page URL** (URL der Abmeldeseite des Identitätsanbieters) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Klicken Sie auf **Durchsuchen**, um das Zertifikat hochzuladen, das Sie zuvor aus dem Azure-Portal heruntergeladen haben.

    e. Klicken Sie auf **Änderungen speichern**.

### <a name="create-origami-test-user"></a>Erstellen eines Origami-Testbenutzers

In diesem Abschnitt erstellen Sie in Origami einen Benutzer mit dem Namen Britta Simon. 

1. Melden Sie sich mit Administratorrechten beim Origami-Konto an.

2. Klicken Sie oben im Menü auf **Administrator**.
   
    ![Screenshot: Startseite des Origami-Kontos mit Auswahl von „Administrator“](./media/origami-tutorial/admin.png)

3. Klicken Sie im Dialogfeld **Users and Security** (Benutzer und Sicherheit) auf **Users** (Benutzer).
   
    ![Screenshot: Dialogfeld „Users and Security“ (Benutzer und Sicherheit) mit Auswahl von „Users“ (Benutzer)](./media/origami-tutorial/user.png)

4. Klicken Sie auf **Add New User**.
   
    ![Screenshot: Auswahl der Schaltfläche „Add New User“ (Neuen Benutzer hinzufügen)](./media/origami-tutorial/add-user.png)

5. Führen Sie im Dialogfeld „Add New User“ folgende Schritte aus:
   
    ![Screenshot: Dialogfeld „Add New User“ (Neuen Benutzer hinzufügen) mit hervorgehobenen Textfeldern „User Name“ (Benutzername), „First Name“ (Vorname) und „Last Name“ (Nachname)](./media/origami-tutorial/new-user.png)

    a. Geben Sie im Textfeld **Username** (Benutzername) die E-Mail-Adresse des Benutzers ein, z. B. **BrittaSimon\@contoso.com**.

    b. Geben Sie im Textfeld **Kennwort** ein Kennwort ein.

    c. Geben Sie im Textfeld **Confirm Password** das Kennwort erneut ein.

    d. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein (beispielsweise **Britta**).

    e. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein (beispielsweise **Simon**).

    f. Klicken Sie auf **Speichern**.
   
    ![Screenshot: Auswahl der Schaltfläche „Speichern“](./media/origami-tutorial/save.png)

6. Weisen Sie dem Benutzer unter **User Roles** (Benutzerrollen) entsprechende Benutzerrollen und unter **Client Access** (Clientzugriff) einen Benutzerzugriff zu. 
   
    ![Einmaliges Anmelden konfigurieren](./media/origami-tutorial/user-roles.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für Origami weitergeleitet, wo Sie den Anmeldeflow initiieren können. 

* Navigieren Sie direkt zur Anmelde-URL für Origami, und initiieren Sie dort den Anmeldeflow.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie in „Meine Apps“ auf die Kachel „Origami“ klicken, werden Sie zur Anmelde-URL für Origami weitergeleitet. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Origami können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.
