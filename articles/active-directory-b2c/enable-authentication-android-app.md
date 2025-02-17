---
title: Aktivieren der Authentifizierung in einer Android-App – Azure AD B2C
description: Aktivieren Sie die Authentifizierung in einer Android-Anwendung mithilfe der Bausteine von Azure Active Directory B2C. Erfahren Sie, wie Sie Azure AD B2C für die Anmeldung und Registrierung von Benutzern in einer Android-Anwendung verwenden.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/06/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support, has-adal-ref
ms.openlocfilehash: b9341909f6dd1dd9d01408a7b7af46e47a0ff339
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2021
ms.locfileid: "122428712"
---
# <a name="enable-authentication-in-your-own-android-application-using-azure-active-directory-b2c"></a>Aktivieren der Authentifizierung in einer eigenen Android-Anwendung mithilfe von Azure Active Directory B2C

In diesem Artikel wird beschrieben, wie Sie einer eigenen mobilen Android-Anwendung die Authentifizierung von Azure Active Directory B2C (Azure AD B2C) hinzufügen. 

Verwenden Sie diesen Artikel mit den Informationen unter [Konfigurieren der Authentifizierung in einer Android-Beispielanwendung](./configure-authentication-sample-android-app.md), und ersetzen Sie dabei die Android-Beispiel-App durch Ihre eigene Android-App. Nachdem Sie die Schritte in diesem Artikel ausgeführt haben, akzeptiert Ihre Anwendung Anmeldungen über Azure AD B2C.

## <a name="prerequisites"></a>Voraussetzungen

Berücksichtigen Sie die Voraussetzungen und Integrationsschritte unter [Konfigurieren der Authentifizierung in einer Android-Beispielanwendung](configure-authentication-sample-android-app.md).

## <a name="create-an-android-app-project"></a>Erstellen eines Android-App-Projekts

Sofern Sie noch nicht über eine Android-Anwendung verfügen, gehen Sie wie folgt vor, um ein neues Projekt einzurichten.

1. Öffnen Sie Android Studio, und wählen Sie **Start a new Android Studio project** (Neues Android Studio-Projekt starten) aus.
2. Wählen Sie **Basic Activity** (Standardaktivität) aus, und klicken Sie dann auf **Next** (Weiter).
3. Benennen Sie Ihre Anwendung.
4. Speichern Sie den Paketnamen. Sie geben diesen später im Azure-Portal ein.
5. Ändern Sie die Sprache von **Kotlin** in **Java**.
6. Legen Sie **Minimum API level** (Mindestebene für API) auf **API 19** oder höher fest, und klicken Sie auf **Finish** (Fertig stellen).
7. Wählen Sie in der Projektansicht in der Dropdownliste die Option **Project** aus, um die Projektdateien mit und ohne Quelle anzuzeigen, öffnen Sie **app/build.gradle**, und legen Sie `targetSdkVersion` auf `28` fest.

## <a name="install-the-dependencies"></a>Installieren der Abhängigkeiten

Navigieren Sie im Android Studio-Projektfenster zu **app** > **build.gradle**, und fügen Sie Folgendes hinzu:

```gradle
apply plugin: 'com.android.application'

allprojects {
    repositories {
    mavenCentral()
    google()
    mavenLocal()
    maven {
        url 'https://pkgs.dev.azure.com/MicrosoftDeviceSDK/DuoSDK-Public/_packaging/Duo-SDK-Feed/maven/v1'
    }
    maven {
        name "vsts-maven-adal-android"
        url "https://identitydivision.pkgs.visualstudio.com/_packaging/AndroidADAL/maven/v1"
        credentials {
            username System.getenv("ENV_VSTS_MVN_ANDROIDADAL_USERNAME") != null ? System.getenv("ENV_VSTS_MVN_ANDROIDADAL_USERNAME") : project.findProperty("vstsUsername")
            password System.getenv("ENV_VSTS_MVN_ANDROIDADAL_ACCESSTOKEN") != null ? System.getenv("ENV_VSTS_MVN_ANDROIDADAL_ACCESSTOKEN") : project.findProperty("vstsMavenAccessToken")
        }
    }
    jcenter()
    }
}
dependencies{
    implementation 'com.microsoft.identity.client:msal:2.+'
    }
packagingOptions{
    exclude("META-INF/jersey-module-version")
}
```


## <a name="add-the-authentication-components"></a>Hinzufügen der Authentifizierungskomponenten

Der [Beispielcode](configure-authentication-sample-android-app.md#step-3-get-the-android-mobile-app-sample) setzt sich aus den folgenden Komponenten zusammen. Fügen Sie diese Komponenten aus der Android-Beispiel-App Ihrer eigenen App hinzu. 

|Komponente  |type  | `Source` |BESCHREIBUNG  |
|---------|---------|---------|---------|
| B2CUser| Klasse| [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/blob/master/app/src/main/java/com/azuresamples/msalandroidkotlinapp/B2CUser.kt) [Java](https://github.com/Azure-Samples/ms-identity-android-java/blob/master/app/src/main/java/com/azuresamples/msalandroidapp/B2CUser.java)| Repräsentiert einen B2C-Benutzer. Diese Klasse ermöglicht es Benutzern, sich mit mehreren Richtlinien anzumelden. | 
| B2CModeFragment | Fragment-Klasse| [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/blob/master/app/src/main/java/com/azuresamples/msalandroidkotlinapp/B2CModeFragment.kt) [Java](https://github.com/Azure-Samples/ms-identity-android-java/blob/master/app/src/main/java/com/azuresamples/msalandroidapp/B2CModeFragment.java)| Ein Fragment repräsentiert einen modularen Teil der Anmeldung über die Azure AD B2C-Benutzerschnittstelle in Ihrer Hauptaktivität. Dieses Fragment enthält den Großteil des Authentifizierungscodes. |
| fragment_b2c_mode.xml | Fragment-Layout| [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/blob/master/app/src/main/res/layout/fragment_b2c_mode.xml) [Java](https://github.com/Azure-Samples/ms-identity-android-java/blob/master/app/src/main/res/layout/fragment_b2c_mode.xml) | Definiert die Struktur für eine Benutzerschnittstelle für die B2CModeFragment-Fragmentkomponente. |
| B2CConfiguration| Klasse| [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/blob/master/app/src/main/java/com/azuresamples/msalandroidkotlinapp/B2CConfiguration.kt) [Java](https://github.com/Azure-Samples/ms-identity-android-java/blob/master/app/src/main/java/com/azuresamples/msalandroidapp/B2CConfiguration.java)| Eine Konfigurationsdatei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Die mobile App nutzt diese Informationen, um eine Vertrauensstellung mit Azure AD B2C herzustellen, den Benutzer an- und abzumelden sowie Token zu beziehen und zu überprüfen. Weitere Konfigurationseinstellungen finden Sie in der Datei „auth_config_b2c.json“.  | 
|auth_config_b2c.json | JSON-Datei| [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/blob/master/app/src/main/res/raw/auth_config_b2c.json) [Java](https://github.com/Azure-Samples/ms-identity-android-java/blob/master/app/src/main/res/raw/auth_config_b2c.json)| Eine Konfigurationsdatei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Die mobile App nutzt diese Informationen, um eine Vertrauensstellung mit Azure AD B2C herzustellen, den Benutzer an- und abzumelden sowie Token zu beziehen und zu überprüfen. Weitere Konfigurationseinstellungen finden Sie in der B2CConfiguration-Klasse. | 

## <a name="configure-your-android-app"></a>Konfigurieren der Android-App

Nachdem Sie die [Authentifizierungskomponenten hinzugefügt](#add-the-authentication-components) haben, konfigurieren Sie Ihre Android-App mit Ihren Azure AD B2C-Einstellungen. Einstellungen für den Azure AD B2C-Identitätsanbieter werden in der Datei „auth_config_b2c.json“ und in der B2CConfiguration-Klasse konfiguriert. 

Folgen Sie den Anweisungen zum [Konfigurieren der mobilen Beispiel-App](configure-authentication-sample-android-app.md#step-5-configure-the-sample-mobile-app).

## <a name="set-the-redirect-uri"></a>Festlegen des Umleitungs-URI

In diesem Abschnitt legen Sie fest, wo Ihre Anwendung auf die Azure AD B2C-Tokenantwort lauscht. 


1. Generieren Sie einen neuen Signaturhash für die Entwicklung. Dieser lautet für jede Entwicklungsumgebung anders.
    
    Unter einem Windows-Betriebssystem:
    
    ```
    keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
    ```
    
    Für ein iOS-Betriebssystem:
    
    ```dotnetcli
    keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64
    ```
    
    Verwenden Sie für eine Produktionsumgebung den folgenden Befehl:
    
    ```
    keytool -exportcert -alias SIGNATURE_ALIAS -keystore PATH_TO_KEYSTORE | openssl sha1 -binary | openssl base64
    ```

    Weitere Hilfe beim Signieren Ihrer Apps finden Sie unter [Signieren Ihrer Android-App](https://developer.android.com/studio/publish/app-signing). 

1. Fügen Sie in **app** > **src** > **main** > **AndroidManifest.xml** die folgende `BrowserTabActivity`-Aktivität zum Hauptteil der Anwendung hinzu:
    
    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Package_Name"
                android:path="/Signature_Hash" />
        </intent-filter>
    </activity>
    ```
1. Ersetzen Sie `Signature_Hash` durch den von Ihnen generierten Hash.
1. Ersetzen Sie `Package_Name` durch den Namen Ihres Android-Pakets.
 
Führen Sie die folgenden Schritte aus, um die Registrierung der mobilen App mit Ihrem App-Umleitungs-URI zu aktualisieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **App-Registrierungen** und dann die Anwendung aus, die Sie in [Schritt 2.3: Registrieren der mobilen App](configure-authentication-sample-android-app.md#23-register-the-mobile-app) registriert haben.
1. Siehe **Authentifizierung**.
1. Wählen Sie unter **Android** die Option **URI hinzufügen** aus.
1. Geben Sie **Paketname** und **Signaturhash** ein.
1. Wählen Sie **Speichern** aus.

Ihr Umleitungs-URI und die `BrowserTabActivity`-Aktivität sollten in etwa wie im folgenden Beispiel aussehen: 

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

Die Umleitungs-URL für das Android-Beispiel:

```
msauth://com.azuresamples.msalandroidkotlinapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D
```

Anschließend verwendet der Absichtsfilter das gleiche Muster wie im folgenden XML-Codeausschnitt:

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host="com.azuresamples.msalandroidkotlinapp"
            android:path="/1wIqXSqBj7w+h11ZifsnqwgyKrY="
            android:scheme="msauth" />
    </intent-filter>
</activity>
```



#### <a name="java"></a>[Java](#tab/java)

Die Umleitungs-URL für das Android-Beispiel:

```
msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D
```

Anschließend verwendet der Absichtsfilter das gleiche Muster wie im folgenden XML-Codeausschnitt:

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
<intent-filter>
    <action android:name="android.intent.action.VIEW" />

    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />

    <data
        android:host="com.azuresamples.msalandroidapp"
        android:path="/1wIqXSqBj7w+h11ZifsnqwgyKrY="
        android:scheme="msauth" />
</intent-filter>
</activity>
```

--- 

## <a name="code-building-blocks"></a>Codebausteine

In diesem Abschnitt werden die Codebausteine beschrieben, die Ihrer Android-App die Authentifizierung ermöglichen. In der folgenden Tabelle werden die B2CModeFragment-Methoden aufgeführt, und es wird angegeben, wie Sie Ihren Code anpassen. 

### <a name="instantiate-a-public-client-application"></a>Instanziieren einer öffentlichen Clientanwendung

Öffentlichen Clientanwendungen sind nicht vertrauenswürdig genug, um Anwendungsgeheimnisse sicher aufzubewahren, deshalb enthalten sie keine geheimen Clientschlüssel. Instanziieren Sie in [onCreate](https://developer.android.com/reference/android/app/Fragment#onCreate(android.os.Bundle)) oder [onCreateView](https://developer.android.com/reference/android/app/Fragment#onCreateView(android.view.LayoutInflater,%20android.view.ViewGroup,%20android.os.Bundle)) MSAL mithilfe des Objekts für öffentliche Clientanwendungen, das mehrere Konten umfasst.

Die `MultipleAccountPublicClientApplication`-Klasse wird verwendet, um MSAL-basierte Apps zu erstellen, die ein gleichzeitiges Anmelden mehrerer Konten zulassen. Dies ermöglicht die Anmeldung mit mehreren Azure AD B2C-Benutzerflows oder benutzerdefinierten Richtlinien. Beispielsweise könnte sich ein Benutzer mit einem Benutzerflow für [Anmeldung oder Registrierung](add-sign-up-and-sign-in-policy.md) anmelden und später einen Benutzerflow für die [Profilbearbeitung](add-profile-editing-policy.md) ausführen. 

Der folgende Codeausschnitt veranschaulicht die Initialisierung der MSAL-Bibliothek mit der JSON-Konfigurationsdatei `auth_config_b2c.json`.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


```kotlin
PublicClientApplication.createMultipleAccountPublicClientApplication(context!!,
    R.raw.auth_config_b2c,
    object : IMultipleAccountApplicationCreatedListener {
        override fun onCreated(application: IMultipleAccountPublicClientApplication) {
            // Set the MultipleAccountPublicClientApplication to the class member b2cApp
            b2cApp = application
            // Load the account (if there is any)
            loadAccounts()
        }

        override fun onError(exception: MsalException) {
            // Error handling
            displayError(exception)
        }
    })
```

#### <a name="java"></a>[Java](#tab/java)

```java
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
    R.raw.auth_config_b2c,
    new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
        @Override
        public void onCreated(IMultipleAccountPublicClientApplication application) {
            // Set the MultipleAccountPublicClientApplication to the class member b2cApp
            b2cApp = application;

            // Load the account (if there is any)
            loadAccounts();
        }

        @Override
        public void onError(MsalException exception) {
            // Error handling
            displayError(exception);
        }
    });
```

--- 

### <a name="load-accounts"></a>Laden von Konten

Wenn die App in den Vordergrund wechselt, lädt sie das vorhandene Konto, um festzustellen, ob der Benutzer angemeldet ist oder nicht.  Verwenden Sie diese Methode, um die Benutzeroberfläche mit dem Authentifizierungsstatus zu aktualisieren. Aktivieren oder deaktivieren Sie beispielsweise die Abmeldeschaltfläche.

Im folgenden Codeausschnitt wird das Laden der Konten veranschaulicht:

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


```kotlin
private fun loadAccounts() {
    if (b2cApp == null) {
        return
    }
    b2cApp!!.getAccounts(object : LoadAccountsCallback {
        override fun onTaskCompleted(result: List<IAccount>) {
            users = B2CUser.getB2CUsersFromAccountList(result)
            updateUI(users)
        }
    
        override fun onError(exception: MsalException) {
            displayError(exception)
        }
    })
    }
```

#### <a name="java"></a>[Java](#tab/java)

```java
private void loadAccounts() {
    if (b2cApp == null) {
        return;
    }

    b2cApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            users = B2CUser.getB2CUsersFromAccountList(result);
            updateUI(users);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

--- 

### <a name="interactive-authorization-request"></a>Interaktive Autorisierungsanforderung

Eine interaktive Autorisierungsanforderung ist ein Flow, bei dem der Benutzer zur Registrierung oder Anmeldung aufgefordert wird. Die `initializeUI`-Methode konfiguriert das `runUserFlowButton`-Klickereignis. Wenn der Benutzer auf die Schaltfläche **BENUTZERFLOW AUSFÜHREN** klickt, leitet die App den Benutzer an Azure AD B2C weiter, um den Anmeldeflow abzuschließen. 

Die `runUserFlowButton.setOnClickListener`-Methode bereitet das Objekt `AcquireTokenParameters` mit relevanten Daten zur Autorisierungsanforderung vor. Anschließend fordert die `acquireToken`-Methode den Benutzer auf, den Registrierungs- oder Anmeldeflow abzuschließen. 

Der folgende Codeausschnitt veranschaulicht, wie die interaktive Autorisierungsanforderung gestartet wird. 

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


```kotlin
val parameters = AcquireTokenParameters.Builder()
        .startAuthorizationFromActivity(activity)
        .fromAuthority(getAuthorityFromPolicyName(policy_list.getSelectedItem().toString()))
        .withScopes(B2CConfiguration.scopes)
        .withPrompt(Prompt.LOGIN)
        .withCallback(authInteractiveCallback)
        .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
        .startAuthorizationFromActivity(getActivity())
        .fromAuthority(B2CConfiguration.getAuthorityFromPolicyName(policyListSpinner.getSelectedItem().toString()))
        .withScopes(B2CConfiguration.getScopes())
        .withPrompt(Prompt.LOGIN)
        .withCallback(getAuthInteractiveCallback())
        .build();

b2cApp.acquireToken(parameters);
```

--- 

 
### <a name="interactive-authorization-request-callback"></a>Rückruf für interaktive Autorisierungsanforderung

Sobald der Benutzer den Autorisierungsflow (erfolgreich oder nicht erfolgreich) abgeschlossen hat, wird das Ergebnis an die `getAuthInteractiveCallback()`-Rückrufmethode zurückgegeben. 

Die Rückrufmethode übergibt das `AuthenticationResult`-Objekt oder eine Fehlermeldung im `MsalException`-Objekt. Verwenden Sie diese Methode für folgende Aufgaben:

- Aktualisieren der Benutzeroberfläche der mobilen App mit Informationen nach Abschluss der Anmeldung
- Erneutes Laden der Kontenobjekts
- Aufruf eines Web-API-Diensts mit einem Zugriffstoken
- Verarbeiten von Authentifizierungsfehlern

Der folgende Codeausschnitt veranschaulicht die Verwendung des interaktiven Authentifizierungsrückrufs.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


```kotlin
private val authInteractiveCallback: AuthenticationCallback
    private get() = object : AuthenticationCallback {
        override fun onSuccess(authenticationResult: IAuthenticationResult) {
            /* Successfully got a token, use it to call a protected resource; web API  */
            Log.d(TAG, "Successfully authenticated")

            /* display result info */
            displayResult(authenticationResult)

            /* Reload account asynchronously to get the up-to-date list. */
            loadAccounts()
        }

        override fun onError(exception: MsalException) {
            val B2C_PASSWORD_CHANGE = "AADB2C90118"
            if (exception.message!!.contains(B2C_PASSWORD_CHANGE)) {
                txt_log!!.text = """
                    The user clicks the 'Forgot Password' link in a sign-up or sign-in user flow.
                    Your application needs to handle this error code by running a specific user flow that resets the password.
                    """.trimIndent()
                return
            }

            /* Failed to acquireToken */Log.d(TAG, "Authentication failed: $exception")
            displayError(exception)
            if (exception is MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
            } else if (exception is MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        override fun onCancel() {
            /* User canceled the authentication */
            Log.d(TAG, "User cancelled login.")
        }
    }
```

#### <a name="java"></a>[Java](#tab/java)

```java
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, use it to call a protected resource - MSGraph */
            Log.d(TAG, "Successfully authenticated");

            /* display result info */
            displayResult(authenticationResult);

            /* Reload account asynchronously to get the up-to-date list. */
            loadAccounts();
        }

        @Override
        public void onError(MsalException exception) {
            final String B2C_PASSWORD_CHANGE = "AADB2C90118";
            if (exception.getMessage().contains(B2C_PASSWORD_CHANGE)) {
                logTextView.setText("The user clicks the 'Forgot Password' link in a sign-up or sign-in user flow.\n" +
                        "Your application needs to handle this error code by running a specific user flow that resets the password.");
                return;
            }

            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());
            displayError(exception);

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        @Override
        public void onCancel() {
            /* User canceled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

--- 

## <a name="call-a-web-api"></a>Aufrufen einer Web-API

Zum Aufruf einer [tokenbasierten Autorisierungs-Web-API](enable-authentication-web-api.md) benötigt die App ein gültiges Zugriffstoken. Die App führt die folgenden Schritte aus:


1. Beziehen eines Zugriffstokens mit den erforderlichen Berechtigungen (Bereichen) für den Web-API-Endpunkt
1. Übergeben des Zugriffstokens als Bearertoken im Autorisierungsheader der HTTP-Anforderung in folgendem Format:

```http
Authorization: Bearer <access-token>
```

Wenn sich Benutzer [interaktiv anmelden](#interactive-authorization-request), empfängt die App ein Zugriffstoken in der `getAuthInteractiveCallback`-Rückrufmethode. Verwenden Sie für nachfolgende Web-API-Aufrufe das Verfahren zum automatischen Tokenabruf, wie in diesem Abschnitt beschrieben. 

Rufen Sie vor dem Aufruf einer Web-API die `acquireTokenSilentAsync`-Methode mit den geeigneten Bereichen für Ihren Web-API-Endpunkt auf. Die MSAL-Bibliothek führt die folgenden Schritte aus:

1. Es wird versucht, ein Zugriffstoken mit den angeforderten Bereichen aus dem Tokencache abzurufen. Falls das Token vorhanden ist, wird es zurückgegeben. 
1. Wenn das Token nicht im Tokencache vorhanden ist, versucht die MSAL-Bibliothek, mithilfe des Aktualisierungstokens ein neues Zugriffstoken zu beziehen. 
1. Wenn das Aktualisierungstoken nicht vorhanden oder abgelaufen ist, wird eine Ausnahme zurückgegeben. In diesem Fall sollte der Benutzer dazu aufgefordert werden, [sich interaktiv anzumelden](#interactive-authorization-request).  

Der folgende Codeausschnitt veranschaulicht das Beziehen eines Zugriffstokens:

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

Durch das Klickereignis für die `acquireTokenSilentButton`-Schaltfläche wird ein Zugriffstoken mit den angegebenen Bereichen erworben. 

```kotlin
btn_acquireTokenSilently.setOnClickListener(View.OnClickListener {
    if (b2cApp == null) {
        return@OnClickListener
    }
    val selectedUser = users!![user_list.getSelectedItemPosition()]
    selectedUser.acquireTokenSilentAsync(b2cApp!!,
            policy_list.getSelectedItem().toString(),
            B2CConfiguration.scopes,
            authSilentCallback)
})
```

Die `authSilentCallback`-Rückrufmethode gibt ein Zugriffstoken zurück und ruft eine Web-API auf:

```kotlin
private val authSilentCallback: SilentAuthenticationCallback
    private get() = object : SilentAuthenticationCallback {
        override fun onSuccess(authenticationResult: IAuthenticationResult) {
            Log.d(TAG, "Successfully authenticated")

            /* Call your web API here*/
            callWebAPI(authenticationResult)
        }

        override fun onError(exception: MsalException) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: $exception")
            displayError(exception)
            if (exception is MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
            } else if (exception is MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception is MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }
    }
```

Das folgende Beispiel veranschaulicht, wie eine geschützte Web-API mit einem Bearertoken aufgerufen wird:

```kotlin
@Throws(java.lang.Exception::class)
private fun callWebAPI(authenticationResult: IAuthenticationResult) {
    val accessToken = authenticationResult.accessToken
    val thread = Thread {
        try {
            val url = URL("https://your-app-service.azurewebsites.net/helo")
            val conn = url.openConnection() as HttpsURLConnection
            conn.setRequestProperty("Accept", "application/json")
            
            // Set the bearer token
            conn.setRequestProperty("Authorization", "Bearer $accessToken")
            if (conn.responseCode == HttpURLConnection.HTTP_OK) {
                val br = BufferedReader(InputStreamReader(conn.inputStream))
                var strCurrentLine: String?
                while (br.readLine().also { strCurrentLine = it } != null) {
                    Log.d(TAG, strCurrentLine)
                }
            }
            conn.disconnect()
        } catch (e: IOException) {
            e.printStackTrace()
        } catch (e: Exception) {
            e.printStackTrace()
        }
    }
    thread.start()
}
```

#### <a name="java"></a>[Java](#tab/java)

Durch das Klickereignis für die `acquireTokenSilentButton`-Schaltfläche wird ein Zugriffstoken mit den angegebenen Bereichen erworben. 

```java
acquireTokenSilentButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        if (b2cApp == null) {
            return;
        }

        final B2CUser selectedUser = users.get(b2cUserList.getSelectedItemPosition());
        selectedUser.acquireTokenSilentAsync(b2cApp,
                policyListSpinner.getSelectedItem().toString(),
                B2CConfiguration.getScopes(),
                getAuthSilentCallback());
    }
});
```

Die `authSilentCallback`-Rückrufmethode gibt ein Zugriffstoken zurück und ruft eine Web-API auf:

```java
private SilentAuthenticationCallback getAuthSilentCallback() {
    return new SilentAuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            Log.d(TAG, "Successfully authenticated");

            /* Call your web API here*/
            callWebAPI(authenticationResult);
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());
            displayError(exception);

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }
    };
}
```

Das folgende Beispiel veranschaulicht, wie eine geschützte Web-API mit einem Bearertoken aufgerufen wird:

```java
private void callWebAPI(IAuthenticationResult authenticationResult) throws Exception {
    final String accessToken = authenticationResult.getAccessToken();
    
    
    Thread thread = new Thread(new Runnable() {
    
        @Override
        public void run() {
            try {
                URL url = new URL("https://your-app-service.azurewebsites.net/helo");
                HttpsURLConnection conn = (HttpsURLConnection)url.openConnection();
                conn.setRequestProperty("Accept", "application/json");
                
                // Set the bearer token
                conn.setRequestProperty("Authorization", "Bearer " +  accessToken);
    
                if (conn.getResponseCode() == HttpURLConnection.HTTP_OK) {
                    BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                    String strCurrentLine;
                    while ((strCurrentLine = br.readLine()) != null) {
                        Log.d(TAG, strCurrentLine);
                    }
                }
                conn.disconnect();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    });
    
    thread.start();
    
    }
```

--- 

### <a name="add-permission-to-perform-network-operations"></a>Hinzufügen der Berechtigung zum Ausführen von Netzwerkvorgängen

Um Netzwerkvorgänge in Ihrer Anwendung auszuführen, schließen Sie die folgende Berechtigung in Ihr Manifest ein. Weitere Informationen finden Sie unter [Verbindungsherstellung mit dem Netzwerk](https://developer.android.com/training/basics/network-ops/connecting).

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren von Authentifizierungsoptionen in einer Android-Anwendung](enable-authentication-android-app-options.md)
* [Aktivieren der Authentifizierung in Ihrer eigenen Web-API](enable-authentication-web-api.md)
