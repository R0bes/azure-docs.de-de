---
title: Konfigurieren der Authentifizierung in einem Beispiel einer Angular-SPA-Anwendung mit Azure Active Directory B2C
description: Hier finden Sie Informationen zum Verwenden von Azure Active Directory B2C zum Anmelden und Registrieren von Benutzern in einer Angular-Single-Page-Webanwendung.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/29/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 61c771e1e415b88f89b073301952aac2b09aff1f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122338913"
---
# <a name="configure-authentication-in-a-sample-angular-single-page-application-using-azure-active-directory-b2c"></a>Konfigurieren der Authentifizierung in einem Beispiel einer Angular-Single-Page-Webanwendung mit Azure Active Directory B2C

In diesem Artikel wird anhand eines Beispiels einer Angular-Single-Page-Webanwendung veranschaulicht, wie Sie Ihren Angular-Apps die Authentifizierung bei Azure Active Directory B2C (Azure AD B2C) hinzufügen.

## <a name="overview"></a>Übersicht

OpenID Connect (OIDC) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei einer Anwendung angemeldet werden können. In diesem Beispiel für Angular werden [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular) und der [MSAL-Browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) verwendet. Die MSAL ist eine von Microsoft bereitgestellte Bibliothek, die das Hinzufügen von Authentifizierungs- und Autorisierungsunterstützung für Angular-Single-Page-Webanwendungen vereinfacht.

### <a name="sign-in-flow"></a>Anmeldeflow

Der Anmeldeflow umfasst die folgenden Schritte:

1. Der Benutzer navigiert zur App und wählt **Anmelden** aus. 
1. Die App initiiert eine Authentifizierungsanforderung und leitet den Benutzer zu Azure AD B2C um.
1. Der Benutzer [registriert sich oder meldet sich an](add-sign-up-and-sign-in-policy.md), [setzt das Kennwort zurück](add-password-reset-policy.md) oder meldet sich mit einem [Social Media-Konto](add-identity-provider.md) an.
1. Nach erfolgreicher Anmeldung gibt Azure AD B2C einen Autorisierungscode an die App zurück. Die App führt die folgenden Aktionen aus:
  1. Tauscht den Autorisierungscode gegen ein ID-Token, Zugriffstoken und Aktualisierungstoken
  1. Liest die ID-Tokenansprüche
  1. Speichert das Zugriffs- und Aktualisierungstoken zur späteren Verwendung in einem In-Memory-Cache Das Zugriffstoken ermöglicht dem Benutzer den Aufruf geschützter Ressourcen, wie z. B. einer Web-API. Das Aktualisierungstoken dient zum Beziehen eines neuen Zugriffstokens.

### <a name="app-registration-overview"></a>Übersicht über die App-Registrierung

Damit sich Ihre App mit Azure AD B2C anmelden und eine Web-API aufrufen kann, müssen Sie im Azure AD B2C-Verzeichnis zwei Anwendungen registrieren.  

- Durch die Registrierung der **Single-Page-Webanwendung** (Angular) kann sich Ihre App bei Azure AD B2C anmelden. Bei der App-Registrierung geben Sie den *Umleitungs-URI* an. Der Umleitungs-URI ist der Endpunkt, zu dem der Benutzer umgeleitet wird, nachdem er sich bei Azure AD B2C authentifiziert hat. Beim App-Registrierungsprozess wird eine *Anwendungs-ID* generiert, die auch als *Client-ID* bezeichnet wird und Ihre App eindeutig identifiziert. Beispiel: **App-ID: 1**.

- Durch die **Web-API**-Registrierung kann Ihre App eine geschützte Web-API aufrufen. Die Registrierung macht die Web-API-Berechtigungen (Bereiche) verfügbar. Beim App-Registrierungsprozess wird eine *Anwendungs-ID* generiert, die Ihre Web-API eindeutig identifiziert. Beispiel: **App-ID: 2**. Erteilen Sie Ihrer App (App-ID: 1) Berechtigungen für die Web-API-Bereiche (App-ID: 2).  

In den folgenden Diagrammen werden die App-Registrierungen und die Anwendungsarchitektur veranschaulicht.

![Das Diagramm beschreibt eine SPA-App mit Web-API, Registrierungen und Token.](./media/configure-authentication-sample-angular-spa-app/spa-app-with-api-architecture.png) 

### <a name="call-to-a-web-api"></a>Aufrufen einer Web-API

[!INCLUDE [active-directory-b2c-app-integration-call-api](../../includes/active-directory-b2c-app-integration-call-api.md)]

### <a name="sign-out-flow"></a>Abmeldeflow

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, auf dem Folgendes ausgeführt wird:

* [Visual Studio Code](https://code.visualstudio.com/) oder ein anderer Code-Editor
* [Node.js-Runtime](https://nodejs.org/en/download/) und [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [Angular LCI](https://angular.io/cli)

## <a name="step-1-configure-your-user-flow"></a>Schritt 1: Konfigurieren Ihres Benutzerflows

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](../../includes/active-directory-b2c-app-integration-add-user-flow.md)]

## <a name="step-2-register-your-angular-spa-and-api"></a>Schritt 2: Registrieren Ihrer Angular-Single-Page-Webanwendung (SPA) und API

In diesem Schritt erstellen Sie die Angular-SPA-App und Web-API-Anwendungsregistrierungen und geben die Bereiche Ihrer Web-API an.

### <a name="21-register-the-web-api-application"></a>2.1 Registrieren der Web-API-Anwendung

[!INCLUDE [active-directory-b2c-app-integration-register-api](../../includes/active-directory-b2c-app-integration-register-api.md)]

### <a name="22-configure-scopes"></a>2.2 Konfigurieren der Bereiche

[!INCLUDE [active-directory-b2c-app-integration-api-scopes](../../includes/active-directory-b2c-app-integration-api-scopes.md)]

### <a name="23-register-the-angular-app"></a>2.3 Registrieren der Angular-App

Führen Sie die folgenden Schritte aus, um die Registrierung der Angular-App zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispielsweise *MyApp*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Identitätsanbieter oder Organisationsverzeichnis (zum Authentifizieren von Benutzern mit Benutzerflows)** aus. 
1. Wählen Sie unter **Umleitungs-URI** die Option **Single-Page-Anwendung (SPA)** aus, und geben Sie `http://localhost:4200` in das URL-Textfeld ein.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen **Administratoreinwilligung für OpenID- und Offlinezugriffsberechtigungen erteilen**.
1. Wählen Sie **Registrieren**.
1. Notieren Sie sich die **Anwendungs-ID (Client)** , die Sie später beim Konfigurieren der Webanwendung verwenden.
    ![Screenshot: Abrufen der ID der Angular-Anwendung](./media/configure-authentication-sample-angular-spa-app/get-azure-ad-b2c-app-id.png)  

### <a name="25-grant-permissions"></a>2.5 Erteilen von Berechtigungen

[!INCLUDE [active-directory-b2c-app-integration-grant-permissions](../../includes/active-directory-b2c-app-integration-grant-permissions.md)]

## <a name="step-3-get-the-angular-sample-code"></a>Schritt 3: Abrufen des Angular-Beispielcodes

In diesem Beispiel wird veranschaulicht, wie eine Angular-Single-Page-Webanwendung Azure AD B2C für die Benutzerregistrierung und -anmeldung verwenden kann. Anschließend bezieht die App ein Zugriffstoken und ruft eine geschützte Web-API auf. Laden Sie das folgende Beispiel herunter:

  [Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/archive/refs/heads/main.zip), oder klonen Sie das Beispiel aus dem [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/):

  ```
  git clone https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial.git
  ```

### <a name="31-configure-the-angular-sample"></a>3.1 Konfigurieren des Angular-Beispiels

Nachdem Sie nun das Beispiel der SPA-App abgerufen haben, aktualisieren Sie den Code mit Ihren Azure AD B2C- und Web-API-Werten. Öffnen Sie im Ordner des Beispiels unter dem Ordner `src/app` die Datei `auth-config.ts`, und aktualisieren Sie die Schlüsseln mit den entsprechenden Werten:  


|`Section`  |Key  |Wert  |
|---------|---------|---------|
| b2cPolicies | Namen |Der Benutzerflow oder die benutzerdefinierte Richtlinie, den/die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben. |
| b2cPolicies | authorities | Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-[Mandanten](tenant-management.md#get-your-tenant-name). Beispiel: `contoso.onmicrosoft.com`. Ersetzen Sie dann den Richtliniennamen durch den Benutzerflow oder die benutzerdefinierte Richtlinie, den/die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben. Beispiel: `https://<your-tenant-name>.b2clogin.com/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>`. |
| b2cPolicies | authorityDomain|Der Name Ihres Azure AD B2C-[Mandanten](tenant-management.md#get-your-tenant-name). Beispiel: `contoso.onmicrosoft.com`. |
| Konfiguration | clientId | Die Angular-Anwendungs-ID aus [Schritt 2.3](#23-register-the-angular-app). |
| protectedResources| endpoint| Die URL der Web-API: `http://localhost:5000/api/todolist`. |
| protectedResources| Bereiche| Die Web-API-Bereiche, die Sie in [Schritt 2.2](#22-configure-scopes) erstellt haben. Beispiel: `b2cScopes: ["https://<your-tenant-namee>.onmicrosoft.com/tasks-api/tasks.read"]`. |

Der sich ergebende Code in *src/app/auth-config.ts* sollte in etwa wie das folgende Beispiel aussehen:

```typescript
export const b2cPolicies = {
     names: {
         signUpSignIn: "b2c_1_susi_reset_v2",
         editProfile: "b2c_1_edit_profile_v2"
     },
     authorities: {
         signUpSignIn: {
             authority: "https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/b2c_1_susi_reset_v2",
         },
         editProfile: {
             authority: "https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/b2c_1_edit_profile_v2"
         }
     },
     authorityDomain: "your-tenant-name.b2clogin.com"
 };
 
 
export const msalConfig: Configuration = {
     auth: {
         clientId: '<your-MyApp-application-ID>',
         authority: b2cPolicies.authorities.signUpSignIn.
         knownAuthorities: [b2cPolicies.authorityDomain],
         redirectUri: '/', 
     },
    // More configuration here
 }

export const protectedResources = {
  todoListApi: {
    endpoint: "http://localhost:5000/api/todolist",
    scopes: ["https://your-tenant-namee.onmicrosoft.com/api/tasks.read"],
  },
}
```

## <a name="step-4-get-the-web-api-sample-code"></a>Schritt 4: Abrufen des Web-API-Beispielcodes

Nachdem Sie die Web-API registriert und die zugehörigen Bereiche definiert haben, können Sie den Web-API-Code für die Verwendung mit Ihrem Azure AD B2C-Mandanten konfigurieren. Laden Sie das folgende Beispiel herunter:

[Laden Sie ein \*ZIP-Archiv](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) herunter, oder klonen Sie das Web-API-Beispielprojekt von GitHub. Sie können auch direkt zum Projekt [Azure-Samples/active-directory-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) auf GitHub navigieren.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="41-configure-the-web-api"></a>4.1 Konfigurieren der Web-API

Öffnen Sie im Ordner des Beispiels die Datei *config.json*. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Die Web-API-App verwendet diese Informationen, um das Zugriffstoken zu überprüfen, das die Web-App als Bearertoken übergibt. Aktualisieren Sie die folgenden Eigenschaften der App-Einstellungen:

|`Section`  |Key  |Wert  |
|---------|---------|---------|
|Anmeldeinformationen|tenantName| Der erste Teil Ihres Azure AD B2C-[Mandantennamens](tenant-management.md#get-your-tenant-name). Beispielsweise `contoso`.|
|Anmeldeinformationen|clientID| Die Web-API-Anwendungs-ID aus [Schritt 2.1](#21-register-the-web-api-application). Im [obigen Diagramm](#app-registration-overview)ist dies die Anwendung mit der *App-ID: 2*.|
|Anmeldeinformationen| Issuer (Aussteller)| (Optional) Der Anspruchswert des Tokenausstellers `iss`. Azure AD B2C gibt das Token standardmäßig im folgenden Format zurück: `https://<your-tenant-name>.b2clogin.com/<your-tenant-ID>/v2.0/`. Ersetzen Sie `<your-tenant-name>` durch den ersten Teil des Namens Ihres Azure AD B2C-[Mandanten](tenant-management.md#get-your-tenant-name). Ersetzen Sie durch `<your-tenant-ID>` die [ID Ihres Azure AD B2C-Mandanten](tenant-management.md#get-your-tenant-id). |
|Richtlinien|policyName|Der Benutzerflow oder die benutzerdefinierte Richtlinie, den/die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben. Wenn Ihre Anwendung mehrere Benutzerflows oder benutzerdefinierte Richtlinien verwendet, geben Sie nur eine(n) an. Beispielsweise den Benutzerflow für Registrierung oder Anmeldung.|
| resource| scope | Die Bereiche ihrer Web-API-Anwendungsregistrierung aus Schritt [2.5])(#25-grant-permissions). |

Die endgültige Konfigurationsdatei sollte wie die folgende JSON aussehen:

```json
{
    "credentials": {
        "tenantName": "<your-tenant-namee>",
        "clientID": "<your-webapi-application-ID>",
        "issuer": "https://<your-tenant-name>.b2clogin.com/<your-tenant-ID>/v2.0/"
    },
    "policies": {
        "policyName": "b2c_1_susi"
    },
    "resource": {
        "scope": ["tasks.read"] 
    },
    // More settings here
} 
```

## <a name="step-5-run-the-angular-spa-and-web-api"></a>Schritt 5: Ausführen der Angular-SPA und Web-API

Nun können Sie den bereichsbezogenen API-Zugriff der Angular-SPA testen. In diesem Schritt führen Sie sowohl die Web-API als auch die Angular-Beispielanwendung auf Ihrem lokalen Computer aus. Melden Sie sich dann bei der Angular-Anwendung an, und wählen Sie die Schaltfläche **TodoList** aus, um eine Anforderung an die geschützte API zu senden.

### <a name="run-the-web-api"></a>Ausführen der Web-API

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie zum Verzeichnis mit dem Web-API-Beispiel. Beispiel:

    ```console
    cd active-directory-b2c-javascript-nodejs-webapi
    ```

1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    node index.js
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die Anwendung gehostet wird.

    ```console
    Listening on port 5000...
    ```

### <a name="run-the-angular-application"></a>Ausführen der Angular-Anwendung

1. Öffnen Sie ein weiteres Konsolenfenster, und wechseln Sie zum Verzeichnis mit dem Angular-Beispiel. Beispiel:

    ```console
    cd ms-identity-javascript-angular-tutorial-main/3-Authorization-II/2-call-api-b2c/SPA
    ```

1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    npm start
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die Anwendung gehostet wird.

    ```console
    Listening on port 4200...
    ```

1. Navigieren Sie in Ihrem Browser zu `http://localhost:4200`, um die Anwendung anzuzeigen.
1. Wählen Sie **Anmelden**.

    ![Screenshot, der die Angular-Beispiel-App mit dem Anmeldelink zeigt.](./media/configure-authentication-sample-angular-spa-app/sample-app-sign-in.png)

1. Schließen Sie den Registrierungs- oder Anmeldevorgang ab.
1. Nach erfolgreicher Anmeldung sollte Ihr Profil angezeigt werden. Wählen Sie im Menü **ToDoList** aus.

    ![Screenshot, der die Angular-Beispiel-App mit dem Benutzerprofil und dem Aufruf der To-Do-Liste zeigt.](./media/configure-authentication-sample-angular-spa-app/sample-app-result.png)

1. Sie können der Liste  **neue Einträge hinzufügen** oder Einträge **löschen** oder **bearbeiten**.

    ![Screenshot, der den Aufruf der To-Do-Liste in der Angular-Beispiel App zeigt.](./media/configure-authentication-sample-angular-spa-app/sample-app-calls-web-api.png)

## <a name="deploy-your-application"></a>Bereitstellen der Anwendung 

In einer Produktionsanwendung handelt es sich bei dem Umleitungs-URI der App-Registrierung in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, wie beispielsweise `https://contoso.com`. 

Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern. Für Umleitungs-URIs gelten die folgenden Einschränkungen:

* Die Antwort-URL muss mit dem Schema `https` beginnen.
* Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. 

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Codebeispiel](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/)
* [Aktivieren der Authentifizierung in Ihrer eigenen Angular-Anwendung](enable-authentication-angular-spa-app.md)
* [Konfigurieren von Authentifizierungsoptionen in Ihrer Angular-Anwendung](enable-authentication-angular-spa-app-options.md)
* [Aktivieren der Authentifizierung in Ihrer Web-API](enable-authentication-web-api.md)
