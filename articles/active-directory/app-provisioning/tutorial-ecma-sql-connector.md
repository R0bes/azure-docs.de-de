---
title: 'Tutorial: Generischer SQL-Connector mit Azure AD-ECMA-Connectorhost'
description: In diesem Tutorial wird beschrieben, wie Sie den generischen SQL-Connector für die lokale Anwendungsbereitstellung verwenden.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/13/2021
ms.subservice: hybrid
ms.author: billmath
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8ef75392676b2c766064eefb3c8113734bfa622
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114437534"
---
# <a name="azure-ad-ecma-connector-host-generic-sql-connector-tutorial"></a>Tutorial: Generischer SQL-Connector mit Azure AD-ECMA-Connectorhost

>[!IMPORTANT]
> Die Vorschauversion zur lokalen Bereitstellung ist derzeit nur mit Einladung verfügbar. Um Zugriff auf die Funktion anzufordern, verwenden Sie das [Zugriffsanforderungsformular](https://aka.ms/onpremprovisioningpublicpreviewaccess). Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

In diesem Tutorial werden die Schritte beschrieben, die Sie ausführen müssen, um Benutzer aus Azure Active Directory (Azure AD) automatisch in einer SQL-Datenbank bereitzustellen und deren Bereitstellungen wieder aufzuheben. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).

In diesem Tutorial wird erläutert, wie Sie den generischen SQL-Connector mit dem Azure AD-ECMA-Connectorhost einrichten und verwenden. 

## <a name="prepare-the-sample-database"></a>Vorbereiten der Beispieldatenbank
Führen Sie auf einem Server, auf dem SQL Server ausgeführt wird, das SQL Skript in [Anhang A](#appendix-a) aus. Dieses Skript erstellt eine Beispieldatenbank mit dem Namen „CONTOSO“. Dies ist die Datenbank, in der Sie Benutzer bereitstellen.


## <a name="create-the-dsn-connection-file"></a>Erstellen der DSN-Verbindungsdatei
Der generische SQL-Connector ist eine DSN-Datei zum Herstellen einer Verbindung mit dem SQL-Server. Zunächst müssen Sie eine Datei mit den ODBC-Verbindungsinformationen erstellen.

1. Starten Sie das ODBC-Verwaltungshilfsprogramm auf Ihrem Server.
  
     ![Screenshot der ODBC-Verwaltung](./media/tutorial-ecma-sql-connector/odbc.png)
1. Wählen Sie die Registerkarte **Datei-DSN** und dann **Hinzufügen** aus. 
 
     ![Screenshot der Registerkarte „Datei-DSN“](./media/tutorial-ecma-sql-connector/dsn-2.png)
1. Wählen Sie **SQL Server Native Client 11.0** und dann **Weiter** aus. 
 
     ![Screenshot der Auswahl eines nativen Clients](./media/tutorial-ecma-sql-connector/dsn-3.png)
1. Geben Sie der Datei einen Namen, z. B. **GenericSQL**, und wählen Sie dann **Weiter** aus. 
 
     ![Screenshot der Benennung des Connectors](./media/tutorial-ecma-sql-connector/dsn-4.png)
1. Wählen Sie **Fertig stellen** aus. 
 
     ![Screenshot der Schaltfläche „Fertig stellen“](./media/tutorial-ecma-sql-connector/dsn-5.png)
1. Konfigurieren Sie nun die Verbindung. Geben Sie als Namen des Servers **APP1** ein, und wählen Sie **Weiter** aus.

     ![Screenshot der Eingabe eines Servernamens](./media/tutorial-ecma-sql-connector/dsn-6.png)
1. Behalten Sie die Windows-Authentifizierung bei, und wählen Sie **Weiter** aus.

     ![Screenshot der Windows-Authentifizierung](./media/tutorial-ecma-sql-connector/dsn-7.png)
1. Geben Sie den Namen der Beispieldatenbank ein, also **CONTOSO**.

     ![Screenshot der Eingabe eines Datenbanknamens](./media/tutorial-ecma-sql-connector/dsn-8.png)
1. Behalten Sie auf diesem Bildschirm die Standardeinstellungen bei, und wählen Sie **Fertig stellen** aus.

     ![Screenshot der Auswahl von „Fertig stellen“](./media/tutorial-ecma-sql-connector/dsn-9.png)
1. Um zu überprüfen, ob alles wie erwartet funktioniert, wählen Sie **Datenquelle testen** aus. 
 
     ![Screenshot von „Datenquelle testen“](./media/tutorial-ecma-sql-connector/dsn-10.png)
1. Überprüfen Sie, ob der Test erfolgreich war.

     ![Screenshot mit Erfolgsmeldung](./media/tutorial-ecma-sql-connector/dsn-11.png)
1. Wählen Sie zweimal **OK**. Schließen Sie den ODBC-Datenquellen-Administrator.

## <a name="download-and-install-the-azure-ad-connect-provisioning-agent-package"></a>Herunterladen und Installieren des Azure AD Connect-Bereitstellungs-Agent-Pakets

 1. Melden Sie sich mit Berechtigungen eines Unternehmensadministrators bei dem Server an, den Sie verwenden werden.
 1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu **Azure Active Directory**.
 1. Wählen Sie im Menü im linken Bereich **Azure AD Connect** aus.
 1. Wählen Sie **Cloudsynchronisierung verwalten** > **Alle Agents überprüfen** aus.
 1. Laden Sie das Paket für den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal herunter.
 1. Akzeptieren Sie die Geschäftsbedingungen, und wählen Sie **Herunterladen** aus.
 1. Führen Sie das Installationsprogramm für den Azure AD Connect-Bereitstellungs-Agent („AADConnectProvisioningAgentSetup.msi“) aus.
 1. Wählen Sie auf dem Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** die Option **Installieren** aus.
 
    ![Screenshot des Bildschirms „Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket“](media/on-premises-ecma-install/install-1.png)</br>
 1. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet. Klicken Sie auf **Weiter**.
 
    ![Screenshot des Begrüßungsbildschirms](media/on-premises-ecma-install/install-2.png)</br>
 1. Wählen Sie auf dem Bildschirm **Erweiterung auswählen** die Option **Lokale Anwendungsbereitstellung (Azure AD in Anwendung)** und dann **Weiter** aus. 
 
    ![Screenshot des Bildschirms „Erweiterung auswählen“](media/on-premises-ecma-install/install-3.png)</br>
 1. Verwenden Sie Ihr globales Administratorkonto, und melden Sie sich bei Azure AD an.
 
     ![Screenshot des Bildschirms für die Azure-Anmeldung](media/on-premises-ecma-install/install-4.png)</br>
 1. Wählen Sie auf dem Bildschirm **Agent-Konfiguration** die Option **Bestätigen** aus.
 
     ![Screenshot der Installationsbestätigung](media/on-premises-ecma-install/install-5.png)</br>
 1. Nach Abschluss der Installation sollte im unteren Bereich des Assistenten eine Meldung angezeigt werden. Wählen Sie **Beenden** aus.
 
     ![Screenshot der Schaltfläche „Beenden“](media/on-premises-ecma-install/install-6.png)</br>
 
## <a name="configure-the-azure-ad-ecma-connector-host"></a>Konfigurieren des Azure AD-ECMA-Connectorhosts
1. Wählen Sie auf dem Desktop die Verknüpfung „ECMA“ aus.
1. Nachdem die Konfiguration des ECMA-Connectorhosts gestartet wurde, behalten Sie den Standardport **8585** bei, und wählen Sie **Generieren** aus, um ein Zertifikat zu generieren. Bei dem automatisch generierten Zertifikat handelt es sich um ein selbstsigniertes Zertifikat der vertrauenswürdigen Stammzertifizierungsstelle. Das SAN stimmt mit dem Hostnamen überein.

     ![Screenshot der Konfiguration der Einstellungen](.\media\on-premises-ecma-configure\configure-1.png)
1. Wählen Sie **Speichern** aus.

## <a name="create-a-generic-sql-connector"></a>Erstellen eines generischen SQL-Connectors
 1. Wählen Sie auf dem Desktop die Verknüpfung „ECMA-Connectorhost“ aus.
 1. Wählen Sie **Neuer Connector** aus.
 
     ![Screenshot der Auswahl von „Neuer Connector“](.\media\on-premises-sql-connector-configure\sql-1.png)

 1. Füllen Sie auf der Seite **Eigenschaften** die Felder mit den Werten aus, die in der Tabelle unter dem Bild angegeben sind, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Eingabe von Eigenschaften](.\media\tutorial-ecma-sql-connector\conn-1.png)

     |Eigenschaft|Wert|
     |-----|-----|
     |Name|SQL|
     |Zeitgeber für automatische Synchronisierung (Minuten)|120|
     |Geheimes Token|Geben Sie hier Ihren eigenen Schlüssel ein. Er sollte mindestens 12 Zeichen lang sein.|
       |Erweiterungs-DLL|Wählen Sie für einen generischen SQL-Connector **Microsoft.IAM.Connector.GenericSql.dll** aus.|
 1. Füllen Sie auf der Seite **Konnektivität** die Felder mit den Werten aus, die in der Tabelle unter dem Bild angegeben sind, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Konnektivität“](.\media\tutorial-ecma-sql-connector\conn-2.png)

     |Eigenschaft|Wert|
     |-----|-----|
     |DSN-Datei|Wechseln Sie zu der Datei, die zu Beginn des Tutorials unter „Erstellen der DSN-Verbindungsdatei“ erstellt wurde.|
     |Benutzername|contoso\administrator|
     |Kennwort|Geben Sie das Kennwort des Administrators ein.|
 1. Füllen Sie auf der Seite **Schema 1** die Felder mit den Werten aus, die in der Tabelle unter dem Bild angegeben sind, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Schema 1“](.\media\tutorial-ecma-sql-connector\conn-3.png)

     |Eigenschaft|Wert|
     |-----|-----|
     |Erkennungsmethode für Objekttyp|Fixed Value (Fester Wert)|
     |Liste mit festem Wert/Tabelle/Sicht/SP (Stored Procedure, gespeicherte Prozedur)|Benutzer|
 1. Füllen Sie auf der Seite **Schema 2** die Felder mit den Werten aus, die in der Tabelle unter dem Bild angegeben sind, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Schema 2“](.\media\tutorial-ecma-sql-connector\conn-4.png)

     |Eigenschaft|Wert|
     |-----|-----|
     |Benutzer:Attributerkennung|Tabelle|
     |Benutzer:Tabelle/Sicht/SP|Employees|
 1. Füllen Sie auf der Seite **Schema 3** die Felder mit den Werten aus, die in der Tabelle unter dem Bild angegeben sind, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Schema 3“](.\media\tutorial-ecma-sql-connector\conn-5.png)

     |Eigenschaft|Beschreibung|
     |-----|-----|
     |Anker für User auswählen|User:ContosoLogin|
     |DN-Attribut für Benutzer auswählen|AzureID|
 1. Übernehmen Sie auf der Seite **Schema 4** die Standardeinstellungen, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Schema 4“](.\media\tutorial-ecma-sql-connector\conn-6.png)
 1. Füllen Sie die Felder auf der Seite **Global** aus, und wählen Sie **Weiter** aus. In der Tabelle unter der Abbildung finden Sie Anweisungen für die einzelnen Felder.
 
     ![Screenshot der Seite „Global“](.\media\tutorial-ecma-sql-connector\conn-7.png)

     |Eigenschaft|Beschreibung|
     |-----|-----|
     |Datum-/Uhrzeitformat der Datenquelle|yyyy-MM-dd HH:mm:ss|
 1. Wählen Sie auf der Seite **Partitionen** die Schaltfläche **Weiter** aus.
 
     ![Screenshot der Seite „Partitionen“](.\media\tutorial-ecma-sql-connector\conn-8.png)

 1. Behalten Sie auf der Seite **Ausführungsprofile** die Aktivierung des Kontrollkästchens **Export** bei. Aktivieren Sie das Kontrollkästchen **Vollständiger Import**, und wählen Sie **Weiter** aus.
 
     ![Screenshot der Seite „Ausführungsprofile“](.\media\tutorial-ecma-sql-connector\conn-9.png)

 1. Füllen Sie die Felder auf der Seite **Export** aus, und wählen Sie **Weiter** aus. In der Tabelle unter der Abbildung finden Sie Anweisungen für die einzelnen Felder. 
 
     ![Screenshot der Seite „Export“](.\media\tutorial-ecma-sql-connector\conn-10.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Vorgangsmethode|Tabelle|
     |Tabelle/Sicht/SP|Employees|
 
 1. Füllen Sie die Felder auf der Seite **Vollständiger Import** aus, und wählen Sie **Weiter** aus. In der Tabelle unter der Abbildung finden Sie Anweisungen für die einzelnen Felder. 
 
     ![Screenshot der Seite „Vollständiger Import“](.\media\tutorial-ecma-sql-connector\conn-11.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Vorgangsmethode|Tabelle|
     |Tabelle/Sicht/SP|Employees|
 
 1. Füllen Sie die Felder auf der Seite **Objekttypen** aus, und wählen Sie **Weiter** aus. In der Tabelle unter der Abbildung finden Sie Anweisungen für die einzelnen Felder.

      - **Anker:** Dieses Attribut muss im Zielsystem eindeutig sein. Der Azure AD-Bereitstellungsdienst fragt nach dem ersten Zyklus mithilfe dieses Attributs den ECMA-Host ab. Dieser Ankerwert muss mit dem Ankerwert in Schema 3 identisch sein.
      - **Abfrageattribut:** wird vom ECMA-Host zum Abfragen des In-Memory-Caches verwendet. Dieses Attribut muss eindeutig sein.
      - **DN:** In den meisten Fällen sollte die Option **Automatisch generiert** aktiviert werden. Falls sie deaktiviert ist, stellen Sie sicher, dass das DN-Attribut einem Attribut in Azure AD zugeordnet ist, das den DN in diesem Format speichert: CN = anchorValue, Object = objectType.

        ![Screenshot der Seite „Objekttypen“](.\media\tutorial-ecma-sql-connector\conn-12.png)

      |Eigenschaft|Beschreibung|
      |-----|-----|
      |Zielobjekt|Benutzer|
      |Anchor|ContosoLogin|
      |Abfrageattribut|AzureID|
      |DN|AzureID|
      |Automatisch generiert|Überprüft|
      

 1. Fügen Sie auf der Seite **Attribute auswählen** alle Attribute in der Dropdownliste hinzu, und wählen Sie **Weiter** aus. 
 
     ![Screenshot der Seite „Attribute auswählen“](.\media\tutorial-ecma-sql-connector\conn-13.png)

      In der Dropdownliste **Attribut** werden alle Attribute angezeigt, die im Zielsystem gefunden wurden und *nicht* auf der vorherigen Seite **Attribute auswählen** ausgewählt wurden. 
 1. Wählen Sie auf der Seite **Bereitstellung aufheben** unter **Flow deaktivieren** die Option **Löschen** aus. Wählen Sie **Fertig stellen** aus.
 
     ![Screenshot der Seite „Bereitstellung aufheben“](.\media\tutorial-ecma-sql-connector\conn-14.png)

## <a name="ensure-ecma2host-service-is-running"></a>Sicherstellen, dass der ECMA2Host-Dienst ausgeführt wird
1. Wählen Sie auf dem Server, auf dem der Azure AD-ECMA-Connectorhost ausgeführt wird, **Starten** aus.
1. Geben Sie **run** (Ausführen) und **services.msc** in das Feld ein.
1. Stellen Sie sicher, dass **Microsoft ECMA2Host** in der in der Liste **Dienste** enthalten ist und ausgeführt wird. Wählen Sie andernfalls **Starten** aus.

   ![Screenshot des ausgeführten Diensts](.\media\on-premises-ecma-configure\configure-2.png)

## <a name="add-an-enterprise-application"></a>Hinzufügen einer Unternehmensanwendung
1. Melden Sie sich als Anwendungsadministrator beim Azure-Portal an.
1. Wechseln Sie im Portal zu **Azure Active Directory** > **Unternehmensanwendungen**.
1. Wählen Sie **Neue Anwendung** aus.

   ![Screenshot des Hinzufügens einer neuen Anwendung](.\media\on-premises-ecma-configure\configure-4.png)
1. Suchen Sie im Katalog nach **Lokale ECMA-App**, und wählen Sie **Erstellen** aus.

## <a name="configure-the-application-and-test"></a>Anwendungskonfiguration und Tests
1. Wählen Sie nach der Erstellung die Seite **Bereitstellung** aus.
1. Klicken Sie auf **Get started** (Los geht‘s).

     ![Screenshot der ersten Schritte](.\media\on-premises-ecma-configure\configure-1.png)
1. Ändern Sie auf der Seite **Bereitstellung** den Modus in **Automatisch**.

     ![Screenshot der Änderung des Modus in „Automatisch“](.\media\on-premises-ecma-configure\configure-7.png)
1. Wählen Sie im Abschnitt **Lokale Konnektivität** den Agent aus, den Sie gerade bereitgestellt haben, und wählen Sie dann **Agent(s) zuweisen** aus.
     >[!NOTE]
     >Warten Sie nach dem Hinzufügen eines Agents 10 Minuten, bis die Registrierung abgeschlossen ist. Der Konnektivitätstest funktioniert erst, wenn die Registrierung abgeschlossen ist.
     >
     >Alternativ können Sie erzwingen, dass die Agent-Registrierung abgeschlossen wird, indem Sie den Bereitstellungs-Agent auf Ihrem Server neu starten. Navigieren Sie zu Ihrem Server, suchen Sie in der Windows-Suchleiste nach **Dienste** und dem **Bereitstellungs-Agent-Dienst von Azure AD Connect**, klicken Sie mit der rechten Maustaste auf den Dienst, und starten Sie ihn neu.
   
     ![Screenshot des Neustarts eines Agents](.\media\on-premises-ecma-configure\configure-8.png)
1.  Geben Sie nach 10 Minuten im Abschnitt **Administratoranmeldeinformationen** die folgende URL ein. Ersetzen Sie den Teil `connectorName` durch den Namen des Connectors auf dem ECMA-Host. Sie können auch `localhost` durch den Hostnamen ersetzen.

     |Eigenschaft|Wert|
     |-----|-----|
     |Mandanten-URL|https://localhost:8585/ecma2host_connectorName/scim|

1. Geben Sie den Wert des **geheimen Tokens** ein, das Sie beim Erstellen des Connectors definiert haben.
1. Wählen Sie **Verbindung testen** aus, und warten Sie eine Minute.

     ![Screenshot der Zuweisung eines Agents](.\media\on-premises-ecma-configure\configure-5.png)
1. Wenn der Verbindungstest erfolgreich war, wählen Sie **Speichern** aus.</br>

     ![Screenshot des Tests eines Agents](.\media\on-premises-ecma-configure\configure-9.png)

## <a name="assign-users-to-an-application"></a>Zuweisen von Benutzern zu einer Anwendung
Da der Azure AD-ECMA-Connectorhost und Azure AD nun miteinander kommunizieren, können Sie mit der Konfiguration des Bereitstellungsbereichs fortfahren. 

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus.
1. Wählen Sie die Anwendung **Lokale Bereitstellung** aus.
1. Wählen Sie links unter **Verwalten** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer/Gruppe hinzufügen** aus.

     ![Screenshot des Hinzufügens eines Benutzers](.\media\tutorial-ecma-sql-connector\app-2.png)
1. Wählen Sie unter **Benutzer** die Option **Keine Elemente ausgewählt** aus.

     ![Screenshot mit „Keine Elemente ausgewählt“](.\media\tutorial-ecma-sql-connector\app-3.png)
1. Wählen Sie auf der rechten Seite Benutzer und dann die Schaltfläche **Auswählen** aus.</br>

     ![Screenshot der Benutzerauswahl](.\media\tutorial-ecma-sql-connector\app-4.png)
1. Wählen Sie nun **Zuweisen** aus.

     ![Screenshot der Benutzerzuweisung](.\media\tutorial-ecma-sql-connector\app-5.png)

## <a name="configure-attribute-mappings"></a>Konfigurieren von Attributzuordnungen
Nun müssen Sie Attribute zwischen der lokalen Anwendung und Ihrem SQL-Server zuordnen.

#### <a name="configure-attribute-mapping"></a>Konfigurieren von Attributzuordnungen
 1. Wählen Sie im Azure AD-Portal unter **Unternehmensanwendungen** die Seite **Bereitstellung** aus.
 1. Klicken Sie auf **Get started** (Los geht‘s).
 1. Erweitern Sie **Zuordnungen**, und wählen Sie **Azure Active Directory-Benutzer bereitstellen** aus.
 
    ![Screenshot der Bereitstellung eines Benutzers](.\media\on-premises-ecma-configure\configure-10.png)
 1. Wählen Sie **Neue Zuordnung hinzufügen** aus.
 
     ![Screenshot von „Neue Zuordnung hinzufügen“](.\media\on-premises-ecma-configure\configure-11.png)
 1. Geben Sie die Quell- und Zielattribute an, und fügen Sie alle Zuordnungen in der folgenden Tabelle hinzu.

      |Zuordnungstyp|Quellattribut|Zielattribut|
      |-----|-----|-----|
      |Direkt|userPrincipalName|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:ContosoLogin|
      |Direkt|objectID|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:AzureID|
      |Direkt|mail|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:Email|
      |Direkt|givenName|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:FirstName|
      |Direkt|surName|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:LastName|
      |Direkt|mailNickname|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:textID|

 1. Wählen Sie **Speichern** aus.
 
     ![Screenshot der Speicherung der Zuordnung](.\media\tutorial-ecma-sql-connector\app-6.png)

## <a name="test-provisioning"></a>Testen der Bereitstellung
Nachdem Ihre Attribute nun zugeordnet sind, können Sie die bedarfsorientierte Bereitstellung mit einem Ihrer Benutzer testen.
 
 1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus.
 1. Wählen Sie die Anwendung **Lokale Bereitstellung** aus.
 1. Wählen Sie links **Bereitstellung** aus.
 1. Klicken Sie auf **Bei Bedarf bereitstellen**.
 1. Suchen Sie nach einem Ihrer Testbenutzer, und wählen Sie **Bereitstellen** aus.
 
     ![Screenshot eines Bereitstellungstests](.\media\on-premises-ecma-configure\configure-13.png)

## <a name="start-provisioning-users"></a>Benutzerbereitstellung starten
 1. Nachdem die bedarfsorientierte Bereitstellung erfolgreich war, wechseln Sie zurück zur Konfigurationsseite der Bereitstellung. Stellen Sie sicher, dass der Bereich nur auf zugewiesene Benutzer und Gruppen festgelegt ist, aktivieren Sie die Bereitstellung (**On**), und wählen Sie **Speichern** aus.
 
    ![Screenshot des Starts der Bereitstellung](.\media\on-premises-ecma-configure\configure-14.png)
 1. Warten Sie einige Minuten, bis die Bereitstellung gestartet wurde. Dies kann bis zu 40 Minuten dauern. Nachdem der Bereitstellungsauftrag wie im nächsten Abschnitt beschrieben abgeschlossen wurde, können Sie den Bereitstellungsstatus wieder deaktivieren (**Off**) und **Speichern** auswählen. Durch diese Aktion wird der Bereitstellungsdienst in Zukunft nicht mehr ausgeführt.

## <a name="check-that-users-were-successfully-provisioned"></a>Überprüfen der erfolgreichen Bereitstellung von Benutzern
Kontrollieren Sie im Anschluss an die Wartezeit die SQL-Datenbank, um zu überprüfen, ob neue Benutzer bereitgestellt werden.

 ![Screenshot der Überprüfung der Bereitstellung von Benutzern](.\media\on-premises-ecma-configure\configure-15.png)

## <a name="appendix-a"></a>Anhang A
Verwenden Sie das folgende SQL-Skript, um die Beispieldatenbank zu erstellen.

```SQL
---Creating the Database---------
Create Database CONTOSO
Go
-------Using the Database-----------
Use [CONTOSO]
Go
-------------------------------------

/****** Object:  Table [dbo].[Employees]    Script Date: 1/6/2020 7:18:19 PM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Employees](
    [ContosoLogin] [nvarchar](128) NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [Email] [nvarchar](128) NULL,
    [InternalGUID] [uniqueidentifier] NULL,
    [AzureID] [uniqueidentifier] NULL,
    [textID] [nvarchar](128) NULL
) ON [PRIMARY]
GO

ALTER TABLE [dbo].[Employees] ADD  CONSTRAINT [DF_Employees_InternalGUID]  DEFAULT (newid()) FOR [InternalGUID]
GO

```




## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei der Bereitstellung lokaler Anwendungen](on-premises-ecma-troubleshoot.md)
- [Überprüfen der bekannten Einschränkungen](known-issues.md)
- [Voraussetzungen für die lokale Bereitstellung](on-premises-ecma-prerequisites.md)
- [Überprüfen der Voraussetzungen für die lokale Bereitstellung](on-premises-ecma-prerequisites.md)
