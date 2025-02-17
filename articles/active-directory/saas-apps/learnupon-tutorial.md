---
title: 'Tutorial: Azure Active Directory-Integration mit LearnUpon | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LearnUpon konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/03/2021
ms.author: jeedes
ms.openlocfilehash: 84da4c02ec4631e9d9f33ac745c3721894cc5f29
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114469476"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Tutorial: Azure Active Directory-Integration mit LearnUpon

In diesem Tutorial erfahren Sie, wie Sie LearnUpon in Azure Active Directory (Azure AD) integrieren. Die Integration von LearnUpon in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf LearnUpon hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihrem Azure AD-Konto automatisch bei LearnUpon anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LearnUpon konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie nicht über eine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* LearnUpon-Abonnement, für das einmaliges Anmelden aktiviert ist.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* LearnUpon unterstützt **IDP-initiiertes** einmaliges Anmelden.

* LearnUpon unterstützt die **Just-in-Time**-Benutzerbereitstellung.

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-learnupon-from-the-gallery"></a>Hinzufügen von LearnUpon aus dem Katalog

Zum Konfigurieren der Integration von LearnUpon in Azure AD müssen Sie LearnUpon aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **LearnUpon** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **LearnUpon** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-learnupon"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für LearnUpon

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit LearnUpon mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LearnUpon eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit LearnUpon die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für LearnUpon](#configure-learnupon-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines LearnUpon-Testbenutzers](#create-learnupon-test-user)** , um eine Entsprechung von B. Simon in LearnUpon zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LearnUpon** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<companyname>.learnupon.com/saml/consumer`.

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Den Wert erhalten Sie vom [Supportteam für den LearnUpon-Client](https://www.learnupon.com/contact/). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den **FINGERABDRUCK**. Dieser Wert wird Ihren SAML-Einstellungen für LearnUpon hinzugefügt.

    ![Downloadlink für das Zertifikat](common/certificateraw.png)

6. Kopieren Sie im Abschnitt **LearnUpon einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LearnUpon gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **LearnUpon** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-learnupon-sso"></a>Konfigurieren des einmaligen Anmeldens für LearnUpon

1. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich mit einem Administratorkonto bei LearnUpon an.

1. Klicken Sie auf die Registerkarte **Einstellungen** .

    ![Screenshot: Registerkarte „Einstellungen“](./media/learnupon-tutorial/settings.png)

1. Klicken Sie auf **Einmaliges Anmelden – SAML** und anschließend auf **Allgemeine Einstellungen**, um SAML-Einstellungen zu konfigurieren.
   
    ![Screenshot: Option „Einmaliges Anmelden – SAML“ mit Auswahl von „Allgemeine Einstellungen“](./media/learnupon-tutorial/general-settings.png) 

1. Führen Sie im Abschnitt **Allgemeine Einstellungen** die folgenden Schritte aus:
   
    ![Screenshot: Abschnitt „Allgemeine Einstellungen“, in dem Sie die beschriebenen Werte eingeben können](./media/learnupon-tutorial/values.png)  
  
    a. Wählen Sie **Aktiviert**.

    b. Wählen Sie für **Version** den Wert **2.0** aus.

    c. Wählen Sie für **Bedingungen überspringen** die Option **Nein** aus.

    d. Geben Sie im Textfeld für den **POST-Parametername für SAML-Token** den Namen des POST-Parameters zur oben angegebenen SAML-Verbraucher-URL mit der zu überprüfenden und authentifizierenden SAML-Assertion ein. Beispiel: **SAMLResponse**.

    e. Geben Sie im Textfeld **Name Identifier Format** (Namensbezeichnerformat) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich die Benutzer-ID (E-Mail-Adresse) befindet. Beispiel: `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`
  
    f. Geben Sie im Textfeld **Identify Provider Location** (Anbieterstandort identifizieren) den Wert ein, der angibt, an welche Adresse die Benutzer weitergeleitet werden, wenn sie auf dem Anmeldebildschirm des Azure-Portals auf das von Ihnen hochgeladene Symbol klicken.
  
    g. Fügen Sie im Textfeld **Sign-Out URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    h. Klicken Sie auf **Manage finger prints**(Fingerabdrücke verwalten), und laden Sie dann den Fingerabdruck des heruntergeladenen Zertifikats hoch.

1. Klicken Sie auf **Benutzereinstellungen**, und führen Sie die folgenden Schritte aus:

     ![Screenshot: Abschnitt „Benutzereinstellungen“, in dem Sie die beschriebenen Werte eingeben können](./media/learnupon-tutorial/user-settings.png)  

    a. Geben Sie im Textfeld **First Name Identifier Format** (Format der Vornamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Vorname des Benutzers befindet, etwa `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
  
    b. Geben Sie im Textfeld **Last Name Identifier Format** (Format der Nachnamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Nachname des Benutzers befindet, z.B. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

### <a name="create-learnupon-test-user"></a>Erstellen eines LearnUpon-Testbenutzers

In diesem Abschnitt wird in LearnUpon ein Benutzer mit dem Namen Britta Simon erstellt. LearnUpon unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in LearnUpon vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt. Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam von LearnUpon](https://www.learnupon.com/contact/) in Verbindung.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen:

* Klicken Sie im Azure-Portal auf „Diese Anwendung testen“. Dadurch sollten Sie automatisch bei der LearnUpon-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie in „Meine Apps“ auf die Kachel „LearnUpon“ klicken, sollten Sie automatisch bei der LearnUpon-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von LearnUpon können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.
