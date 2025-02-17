---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit HRworks Single Sign-On | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und HRworks Single Sign-On konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/29/2021
ms.author: jeedes
ms.openlocfilehash: 28127068210199985a280c26a34ce3aa49520a8b
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121746980"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hrworks-single-sign-on"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit HRworks Single Sign-On

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in HRworks Single Sign-On integrieren. Die Integration von HRworks Single Sign-On in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf HRworks Single Sign-On hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei HRworks Single Sign-On anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* HRworks Single Sign-On-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* HRworks Single Sign-On unterstützt **SP**-initiiertes SSO.

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-hrworks-single-sign-on-from-the-gallery"></a>Hinzufügen der HRworks Single Sign-On-App aus dem Katalog

Zum Konfigurieren der Integration von HRworks Single Sign-On in Azure AD müssen Sie HRworks Single Sign-On aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Suchfeld den Suchbegriff **HRworks Single Sign-On** ein.
1. Wählen Sie im Ergebnisbereich **HRworks Single Sign-On** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-hrworks-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für HRworks Single Sign-on

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit HRworks Single Sign-On mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in HRworks Single Sign-On eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für HRworks Single Sign-on die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für HRworks Single Sign-On](#configure-hrworks-single-sign-on-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines HRworks Single Sign-On-Testbenutzers](#create-hrworks-single-sign-on-test-user)** , um eine Entsprechung von B. Simon in HRworks Single Sign-On zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **HRworks Single Sign-On** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** den folgenden Schritt aus:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://login.hrworks.de/?companyId=<COMPANY_ID>&directssologin=true`

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von HRworks Single Sign-On](https://www.hrworks.de/dienstleistungen/support/), um den Wert zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **HRworks Single Sign-On einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf HRworks Single Sign-On gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **HRworks Single Sign-On** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-hrworks-single-sign-on-sso"></a>Konfigurieren des einmaligen Anmeldens für HRworks Single Sign-On

1. Wenn Sie die Konfiguration in HRworks Single Sign-On automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

1. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **HRworks Single Sign-On einrichten**, um zur Anwendung HRworks Single Sign-On weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei HRworks Single Sign-On anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 und 4.

    ![Einrichtungskonfiguration](common/setup-sso.png)

1. Wenn Sie HRworks Single Sign-On manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der HRworks Single Sign-On-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

1. Klicken Sie links in der Menüleiste auf **Administrator** > **Grundlagen** > **Sicherheit** > **Einmaliges Anmelden**, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/hrworks-single-sign-on-tutorial/configure.png)

    a. Aktivieren Sie das Kontrollkästchen **Einmaliges Anmelden verwenden**.

    b. Wählen Sie **XML Metadata** (XML-Metadaten) als **Meta data input method** (Metadaten-Eingabemethode) aus.

    c. Wählen Sie **Individual NameID identifier** (Individueller NameID-Bezeichner) als **Value for NameID** (Wert für NameID) aus.

    d. Öffnen Sie im Editor die Metadaten-XML, die Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **Metadata** (Metadaten) ein.

    e. Klicken Sie auf **Speichern**.

### <a name="create-hrworks-single-sign-on-test-user"></a>Erstellen eines HRworks Single Sign-On-Testbenutzers

Zum Aktivieren von Azure AD-Benutzern müssen Sie sich an HRworks Single Sign-On anmelden, um sie in HRworks Single Sign-On bereitzustellen. In HRworks Single Sign-On ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich an HRworks Single Sign-On als Administrator an.

1. Klicken Sie links in der Menüleiste auf **Administrator** > **Persons** > **Persons** > **New person** (Administrator > Personen > Personen > Neue Person).

     ![Screenshot: HRworks-Seite mit den ausgewählten Optionen „Persons“ und „New person“ (Personen und Neue Person)](./media/hrworks-single-sign-on-tutorial/persons.png)

1. Klicken Sie im Popupfenster auf **Next** (Weiter).

    ![Screenshot: Liste der Länder/Regionen, die Sie für die Person auswählen können](./media/hrworks-single-sign-on-tutorial/new-person.png)

1. Fügen Sie im Popupelement **Create new person with country for legal terms** (Aus rechtlichen Gründen neue Person mit Land erstellen) die erforderlichen Details ein, z. B. **First name** (Vorname) und **Last name** (Nachname), und klicken Sie auf **Create** (Erstellen).

    ![Screenshot: Textfelder, in die Sie den Vor- und Nachnamen der Person eingeben](./media/hrworks-single-sign-on-tutorial/create-person.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für HRworks Single Sign-on weitergeleitet, wo Sie den Anmeldeabfluss initiieren können. 

* Rufen Sie die HRworks Single Sign-on-Anmelde-URL direkt auf und initiieren Sie den Anmeldeablauf.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie in dem Zugriffsbereich von „Meine Apps“ auf die Kachel „HRworks Single Sign-On“ klicken, werden Sie zur Anmelde-URL von HRworks Single Sign-On weitergeleitet. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von HRworks Single Sign-on können Sie die Sitzungssteuerung erzwingen, die Ihre vertraulichen Unternehmensdaten in Echtzeit vor der Exfiltration und Infiltration schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.
