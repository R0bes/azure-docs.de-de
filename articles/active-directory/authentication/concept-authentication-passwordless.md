---
title: Kennwortlose Azure Active Directory-Anmeldung
description: Hier finden Sie Informationen zu den Optionen für kennwortlose Anmeldung bei Azure Active Directory mithilfe von FIDO2-Sicherheitsschlüsseln oder der Microsoft Authenticator-App.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/28/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 115f792c82f2e3eea42636c788c3461f7cc55f77
ms.sourcegitcommit: 7f3ed8b29e63dbe7065afa8597347887a3b866b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122350024"
---
# <a name="passwordless-authentication-options-for-azure-active-directory"></a>Optionen für die kennwortlose Authentifizierung für Azure Active Directory

Features wie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) bieten hervorragende Möglichkeiten, Ihre Organisation zu schützen. Allerdings ist es für Benutzer oft lästig, sich nicht nur ihre Kennwörter merken, sondern zusätzlich noch weitere Sicherheitsmaßnahmen durchführen zu müssen. Kennwortlose Authentifizierungsmethoden sind bequemer, weil das Kennwort entfällt und durch etwas ersetzt wird, das Sie haben, plus etwas, das Sie sind oder das Sie wissen.

| Authentifizierung  | Etwas, das Sie haben | Etwas, das Sie wissen |
| --- | --- | --- |
| Kennwortlos | Windows 10-Gerät, Smartphone oder Sicherheitsschlüssel | Biometrische Daten oder PIN |

Jede Organisation hat unterschiedliche Anforderungen in Bezug auf die Authentifizierung. Die globale Microsoft Azure-Plattform und Microsoft Azure Government bieten die folgenden drei Optionen für die kennwortlose Authentifizierung, die in Azure Active Directory (Azure AD) integriert werden können:

- Windows Hello for Business
- Microsoft Authenticator-App
- FIDO2-Sicherheitsschlüssel

![Authentifizierung: Sicherheits- und Komfortaspekte](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello for Business

Windows Hello for Business eignet sich ideal für Information-Worker, die über einen eigenen Windows-PC verfügen. Die biometrischen und PIN-basierten Anmeldeinformationen sind direkt mit dem PC des Benutzers verknüpft, wodurch verhindert wird, dass jemand anderes als der Eigentümer Zugriff erhält. Mit PKI-Integration (Public Key-Infrastruktur) und integrierter Unterstützung für einmaliges Anmelden (Single Sign-On, SSO) bietet Windows Hello for Business eine praktische Methode für den nahtlosen Zugriff auf Unternehmensressourcen lokal und in der Cloud.

![Beispiel für eine Benutzeranmeldung mit Windows Hello for Business](./media/concept-authentication-passwordless/windows-hellow-sign-in.jpeg)

Die folgenden Schritte zeigen, wie der Anmeldevorgang mit Azure AD funktioniert:

![Diagramm mit Übersicht über die Schritte bei der Benutzeranmeldung mit Windows Hello for Business](./media/concept-authentication-passwordless/windows-hello-flow.png)

1. Ein Benutzer meldet sich bei Windows mit biometrischer Geste oder PIN an. Die Geste entsperrt den privaten Schlüssel von Windows Hello for Business, und die Daten werden an den Security Support Provider für Cloudauthentifizierung gesendet, auch als *Cloudauthentifizierungsanbieter* bezeichnet.
1. Der Cloudauthentifizierungsanbieter fordert eine Nonce (eine zufällige beliebige Zahl, die nur einmal verwendet werden kann) von Azure AD an.
1. Azure AD gibt eine Nonce zurück, die 5 Minuten lang gültig ist.
1. Der Cloudauthentifizierungsanbieter signiert die Nonce mit dem privaten Schlüssel des Benutzers und gibt die signierte Nonce an Azure AD zurück.
1. Azure AD überprüft die signierte Nonce, indem der Dienst den sicher registrierten öffentlichen Schlüssel des Benutzers mit der Nonce-Signatur abgleicht. Nach der Verifizierung der Signatur überprüft Azure AD dann die zurückgegebene signierte Nonce. Nach der Überprüfung der Nonce erstellt Azure AD ein primäres Aktualisierungstoken (Primary Refresh Token, PRT) mit Sitzungsschlüssel, das mit dem Transportschlüssel des Geräts verschlüsselt ist, und gibt es an den Cloudauthentifizierungsanbieter zurück.
1. Der Cloud-AP-Anbieter erhält das verschlüsselte PRT mit Sitzungsschlüssel. Mithilfe des privaten Transportschlüssels des Geräts entschlüsselt der Cloudauthentifizierungsanbieter den Sitzungsschlüssel und schützt den Sitzungsschlüssel mit dem Trusted Platform Module (TPM) des Geräts.
1. Der Cloudauthentifizierungsanbieter gibt als Antwort an Windows zurück, dass die Authentifizierung erfolgreich war. Der Benutzer kann nun sowohl auf Windows-Anwendungen als auch auf Cloudanwendungen und lokale Anwendungen zugreifen, ohne dass eine wiederholte Authentifizierung erforderlich ist (SSO).

Das [Planungshandbuch](/windows/security/identity-protection/hello-for-business/hello-planning-guide) für Windows Hello for Business kann verwendet werden, um die Auswahl der Art der Bereitstellung von Windows Hello for Business und die zu berücksichtigenden Optionen zu erleichtern.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-App

Sie können auch das Smartphone von Mitarbeitern als kennwortlose Authentifizierungsmethode zulassen. Möglicherweise verwenden Sie bereits die Microsoft Authenticator-App als komfortable MFA-Option zusätzlich zu einem Kennwort. Sie können auch die Authenticator-App als kennwortlose Option verwenden.

![Anmelden bei Microsoft Edge mit der Microsoft Authenticator-App](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Die Authenticator-App wandelt jedes iOS- oder Android-Telefon in sichere kennwortlose Anmeldeinformationen um. Benutzer können sich bei jeder beliebigen Plattform oder jedem beliebigen Browser anmelden, indem sie eine Benachrichtigung auf ihrem Telefon erhalten, eine auf dem Bildschirm angezeigte Zahl mit der Zahl auf dem Telefon abgleichen und dann ihre biometrischen Daten (Touch oder Gesicht) oder ihre PIN zur Bestätigung verwenden. Weitere Informationen zur Installation finden Sie unter [Herunterladen und Installieren der Microsoft Authenticator-App](../user-help/user-help-auth-app-download-install.md).

Das Grundprinzip der kennwortlosen Authentifizierung mit der Authenticator-App ist dasselbe wie bei Windows Hello for Business. Sie ist etwas komplizierter, da der Benutzer identifiziert werden muss, damit Azure AD die verwendete Version der Microsoft Authenticator-App herausfinden kann:

![Diagramm mit Übersicht über die Schritte bei der Benutzeranmeldung mit der Microsoft Authenticator-App](./media/concept-authentication-passwordless/authenticator-app-flow.png)

1. Der Benutzer gibt seinen Benutzernamen ein.
1. Azure AD erkennt, dass der Benutzer über sichere Anmeldeinformationen verfügt, und startet den Strong Credential-Flow (Ablauf für sichere Anmeldeinformationen).
1. Über den Apple Push Notification Service (APNS) bei iOS-Geräten bzw. Firebase Cloud Messaging (FCM) bei Android-Geräten wird eine Benachrichtigung an die App gesendet.
1. Der Benutzer erhält die Pushbenachrichtigung und öffnet die App.
1. Die App ruft Azure AD auf und erhält eine Proof-of-Presence-Abfrage sowie eine Nonce.
1. Der Benutzer beantwortet die Abfrage durch Eingabe von biometrischen Daten oder einer PIN, um den privaten Schlüssel zu entsperren.
1. Die Nonce wird mit dem privaten Schlüssel signiert und an Azure AD zurückgesendet.
1. Azure AD überprüft den öffentlichen/privaten Schlüssel und gibt ein Token zurück.

Um mit der kennwortlosen Anmeldung zu beginnen, lesen Sie folgende Anleitung:

> [!div class="nextstepaction"]
> [Aktivieren der kennwortlosen Anmeldung mit der Authenticator-App](howto-authentication-passwordless-phone.md)

## <a name="fido2-security-keys"></a>FIDO2-Sicherheitsschlüssel

Die FIDO-Allianz (Fast IDentity Online) fördert offene Standards für die Authentifizierung und trägt zur Reduzierung der Verwendung von Kennwörtern als Authentifizierungsmethode bei. FIDO2 ist der aktuelle Standard, der den Webauthentifizierungsstandard (WebAuthn) beinhaltet.

FIDO2-Sicherheitsschlüssel sind eine Phishing-resistente, standardbasierte Methode zur kennwortlosen Authentifizierung, die in jedem Formfaktor verfügbar sein kann. Fast Identity Online (FIDO) ist ein offener Standard für die kennwortlose Authentifizierung. Dank FIDO können Benutzer und Organisationen den Standard nutzen, um sich ohne Benutzername oder Kennwort mit einem externen Sicherheitsschlüssel oder einem in ein Gerät integrierten Plattformschlüssel bei ihren Ressourcen anzumelden.

Benutzer können einen FIDO2-Sicherheitsschlüssel registrieren und dann auf dem Anmeldebildschirm als Hauptauthentifizierungsmethode auswählen. Bei diesen FIDO2-Sicherheitsschlüsseln handelt es sich in der Regel um USB-Geräte, es können aber auch Bluetooth- oder NFC-Geräte sein. Mit einem Hardwaregerät, das für die Authentifizierung sorgt, erhöht sich die Sicherheit eines Kontos, da es kein Kennwort gibt, das verfügbar gemacht oder erraten werden kann.

FIDO2-Sicherheitsschlüssel können verwendet werden, um sich bei in Azure AD oder Azure AD Hybrid eingebundenen Windows 10-Geräten anzumelden und das einmalige Anmelden für den Zugriff auf cloudbasierte und lokale Ressourcen zu nutzen. Benutzer können sich auch bei unterstützten Browsern anmelden. FIDO2-Sicherheitsschlüssel sind eine gute Option für Unternehmen, die sehr sicherheitsbewusst sind oder deren Mitarbeiter nicht bereit oder in der Lage sind, ihr Telefon als zweiten Faktor zu nutzen, oder aber bei denen andere entsprechende Szenarien vorliegen.

Es stehen ein Referenzdokument, für das [Browser die FIDO2-Authentifizierung mit Azure AD unterstützen](fido2-compatibility.md), sowie bewährte Methoden für Entwickler zur Verfügung, die [FIDO2-Authentifizierung in den von ihnen entwickelten Anwendungen unterstützen](../develop/support-fido2-authentication.md) möchten.

![Anmelden bei Microsoft Edge mit einem Sicherheitsschlüssel](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Wenn ein Benutzer sich mit einem FIDO2-Sicherheitsschlüssel anmeldet, sieht der Prozess wie folgt aus:

![Diagramm mit Übersicht über die Schritte bei der Benutzeranmeldung mit einem FIDO2-Sicherheitsschlüssel](./media/concept-authentication-passwordless/fido2-security-key-flow.png)

1. Der Benutzer steckt den FIDO2-Sicherheitsschlüssel in seinen Computer.
2. Windows erkennt den FIDO2-Sicherheitsschlüssel.
3. Windows sendet eine Authentifizierungsanforderung.
4. Azure AD sendet eine Nonce zurück.
5. Der Benutzer führt eine Geste aus, um den privaten Schlüssel freizuschalten, der in der sicheren Enclave des FIDO2-Sicherheitsschlüssels gespeichert ist.
6. Der FIDO2-Sicherheitsschlüssel signiert die Nonce mit dem privaten Schlüssel.
7. Die Anforderung des primären Aktualisierungstokens (PRT) mit signierter Nonce wird an Azure AD gesendet.
8. Azure AD überprüft die signierte Nonce mit dem öffentlichen FIDO2-Schlüssel.
9. Azure AD gibt das PRT zurück, um den Zugriff auf lokale Ressourcen zu ermöglichen.

Viele Schlüssel verfügen zwar über eine FIDO2-Zertifizierung der FIDO Alliance, Microsoft verlangt von Anbietern jedoch die Implementierung einiger optionaler Erweiterungen der FIDO2-CTAP-Spezifikation (Client-to-Authenticator Protocol), um maximale Sicherheit und ein optimales Benutzererlebnis zu gewährleisten.

Ein Sicherheitsschlüssel MUSS die folgenden Features und Erweiterungen aus dem FIDO2-CTAP-Protokoll implementieren, damit er Microsoft-kompatibel ist. Der Authenticator-Anbieter muss sowohl die Version FIDO_2_0 als auch die Version FIDO_2_1 der Spezifikation implementieren. Weitere Informationen finden Sie im [Client to Authenticator Protocol](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html) (Client-zu-Authenticator-Protokoll).

| # | Funktion/Erweiterte Vertrauenswürdigkeit | Warum ist diese Funktion oder Erweiterung erforderlich? |
| --- | --- | --- |
| 1 | Residenter/erkennbarer Schlüssel | Dieses Feature ermöglicht, dass der Sicherheitsschlüssel portabel ist, wobei Ihre Anmeldeinformationen darin gespeichert und erkennbar sind und so Flows ohne Benutzernamen möglich werden. |
| 2 | Client-PIN | Diese Funktion ermöglicht Ihnen, Ihre Anmeldeinformationen mit einem zweiten Faktor zu schützen und gilt für Sicherheitsschlüssel, die keine Benutzeroberfläche haben.<br>Sowohl [PIN-Protokoll 1](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#pinProto1) als auch [PIN-Protokoll 2](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#pinProto2) MÜSSEN implementiert werden. |
| 3 | hmac-secret | Diese Erweiterung stellt sicher, dass Sie sich bei Ihrem Gerät anmelden können, wenn es sich im Offline- oder Flugmodus befindet. |
| 4 | Mehrere Konten per RP | Diese Funktion stellt sicher, dass Sie den gleichen Sicherheitsschlüssel für mehrere Dienste wie Microsoft Account und Azure Active Directory verwenden können. |
| 5 | Verwalten von Anmeldeinformationen    | Dieses Feature ermöglicht es Benutzern, ihre Anmeldeinformationen für Sicherheitsschlüssel auf Plattformen zu verwalten. Es gilt für Sicherheitsschlüssel, bei denen diese Funktionalität nicht integriert ist.<br>Authenticator MUSS die Befehle [authenticatorCredentialManagement](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#authenticatorCredentialManagement) und [credentialMgmtPreview](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#prototypeAuthenticatorCredentialManagement) für dieses Feature implementieren.  |
| 6 | Bio-Registrierung           | Dieses Feature ermöglicht es Benutzern, ihre biometrischen Daten für ihre Authentifikatoren zu registrieren. Es gilt für Sicherheitsschlüssel, bei denen diese Funktionalität nicht integriert ist.<br> Authenticator MUSS die Befehle [authenticatorBioEnrollment](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#authenticatorBioEnrollment) und [userVerificationMgmtPreview](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#prototypeAuthenticatorBioEnrollment) für dieses Feature implementieren. |
| 7 | pinUvAuthToken           | Dieses Feature ermöglicht es der Plattform, Authentifizierungstoken per PIN- oder BIO-Übereinstimmung zu verwenden. Das verbessert die Benutzerfreundlichkeit, wenn mehrere Anmeldeinformationen im Authentifikator vorhanden sind.  |
| 8 | forcePinChange           | Dieses Feature ermöglicht es Unternehmen, Benutzer zu einer Änderung ihrer PIN in Remotebereitstellungen aufzufordern.  |
| 9 | setMinPINLength          | Dieses Feature ermöglicht es Unternehmen, eine benutzerdefinierte minimale PIN-Länge für ihre Benutzer zu verwenden. Authenticator MUSS die Erweiterung „MinPinLength“ implementieren und „maxRPIDsForSetMinPINLength“ mindestens den Wert „1“ haben.  |
| 10 | alwaysUV                | Dieses Feature ermöglicht es Unternehmen oder Benutzern, immer eine Benutzerüberprüfung zur Verwendung dieses Sicherheitsschlüssels anzufordern. Authenticator MUSS den Unterbefehl „toggleAlwaysUv“ implementieren. Der Standardwert für „alwaysUV“ muss vom Anbieter festgelegt werden. An diesem Punkt lautet aufgrund der Art von verschiedenen RPs-Einführungs- und Betriebssystemversionen der empfohlene Wert für biometrische Authentifikatoren „true“ (wahr) und für nicht biometrische Authentifikatoren „false“ (falsch).  |
| 11 | credBlob                | Mit dieser Erweiterung können Websites kleine Informationen im Sicherheitsschlüssel speichern. „maxCredBlobLength“ MUSS mindestens 32 Byte umfassen.  |
| 12 | largeBlob               | Mit dieser Erweiterung können Websites umfangreichere Informationen wie Zertifikate im Sicherheitsschlüssel speichern. „maxSerializedLargeBlobArray“ MUSS mindestens 1.024 Byte umfassen.  |


### <a name="fido2-security-key-providers"></a>FIDO2-Sicherheitsschlüsselanbieter

Die folgenden Anbieter bieten FIDO2-Sicherheitsschlüssel in verschiedenen Formfaktoren an, die bekanntermaßen mit der kennwortlosen Benutzererfahrung kompatibel sind. Wir empfehlen Ihnen, die Sicherheitseigenschaften dieser Schlüssel zu bewerten, indem sie sich an den Hersteller und die FIDO Alliance wenden.

| Anbieter                  |     Biometrisch     | USB | NFC | BLE | FIPS-zertifiziert | Kontakt                                                                                             |
|---------------------------|:-----------------:|:---:|:---:|:---:|:--------------:|-----------------------------------------------------------------------------------------------------|
| AuthenTrend               | ![j]              | ![j]| ![j]| ![j]| ![n]           | https://authentrend.com/about-us/#pg-35-3                                                           |
| Ensurity                  | ![j]              | ![j]| ![n]| ![n]| ![n]           | https://www.ensurity.com/contact                                                                    |
| Excelsecu                 | ![j]              | ![j]| ![j]| ![j]| ![n]           | https://www.excelsecu.com/productdetail/esecufido2secu.html                                         |
| Feitian                   | ![j]              | ![j]| ![j]| ![j]| ![j]           | https://shop.ftsafe.us/pages/microsoft                                                              |
| GoTrustID Inc.            | ![n]              | ![j]| ![j]| ![j]| ![n]           | https://www.gotrustid.com/idem-key                                                                  |
| HID                       | ![n]              | ![j]| ![j]| ![n]| ![n]           | https://www.hidglobal.com/contact-us                                                                |
| Hypersecu                 | ![n]              | ![j]| ![n]| ![n]| ![n]           | https://www.hypersecu.com/hyperfido                                                                 |
| IDmelon Technologies Inc. | ![j]              | ![j]| ![j]| ![j]| ![n]           | https://www.idmelon.com/#idmelon                                                                    |
| Kensington                | ![j]              | ![j]| ![n]| ![n]| ![n]           | https://www.kensington.com/solutions/product-category/why-biometrics/                               |
| KONA I                    | ![j]              | ![n]| ![j]| ![j]| ![n]           | https://konai.com/business/security/fido                                                            |
| Nymi                      | ![j]              | ![n]| ![j]| ![n]| ![n]           | https://www.nymi.com/product                                                                      | 
| OneSpan Inc.              | ![j]              | ![n]| ![n]| ![j]| ![n]           | https://www.onespan.com/products/fido                                                               |
| Thales-Gruppe              | ![n]              | ![j]| ![j]| ![n]| ![n]           | https://cpl.thalesgroup.com/access-management/authenticators/fido-devices                           |
| Thetis                    | ![j]              | ![j]| ![j]| ![j]| ![n]           | https://thetis.io/collections/fido2                                                                 |
| Token2, Schweiz        | ![j]              | ![j]| ![j]| ![n]| ![n]           | https://www.token2.swiss/shop/product/token2-t2f2-alu-fido2-u2f-and-totp-security-key               |
| TrustKey-Lösungen        | ![j]              | ![j]| ![n]| ![n]| ![n]           | https://www.trustkeysolutions.com/security-keys/                                                    |
| VinCSS                    | ![n]              | ![j]| ![n]| ![n]| ![n]           | https://passwordless.vincss.net                                                                     |
| Yubico                    | ![n]              | ![j]| ![j]| ![n]| ![j]           | https://www.yubico.com/solutions/passwordless/                                                      |


<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png

> [!NOTE]
> Wenn Sie NFC-basierte Sicherheitsschlüssel erwerben und sie verwenden möchten, benötigen Sie einen unterstützten NFC-Leser für den Sicherheitsschlüssel. Der NFC-Leser ist keine Azure-Anforderung oder -Einschränkung. Eine Liste der unterstützten NFC-Leser finden Sie beim Anbieter für Ihren NFC-basierten Sicherheitsschlüssel.

Wenn Sie Anbieter sind und Ihr Gerät zur Liste der unterstützten Geräte hinzufügen möchten, lesen Sie die Anleitung, wie Sie [Anbieter Microsoft-kompatibler FIDO2-Sicherheitsschlüssel werden](concept-fido2-hardware-vendor.md).

Um mit FIDO2-Sicherheitsschlüsseln zu beginnen, lesen Sie folgende Anleitung:

> [!div class="nextstepaction"]
> [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln](howto-authentication-passwordless-security-key.md)

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Es gelten die folgenden Bedingungen:

- Administratoren können kennwortlose Authentifizierungsmethoden für ihre Mandanten aktivieren.

- Administratoren können die einzelnen Methoden für alle Benutzer oder ausgewählte Benutzer bzw. Gruppen in ihrem Mandanten anwenden.

- Benutzer können diese kennwortlosen Authentifizierungsmethoden in ihrem Kontoportal registrieren und verwalten.

- Benutzer können sich mit diesen kennwortlosen Authentifizierungsmethoden anmelden:
   - Microsoft Authenticator-App: Funktioniert in Szenarien, in denen die Azure AD-Authentifizierung verwendet wird, einschließlich aller Browser, während der Einrichtung von Windows 10 und mit integrierten mobilen Apps unter jedem Betriebssystem.
   - Sicherheitsschlüssel: Funktioniert auf Sperrbildschirm für Windows 10 und im Internet in unterstützten Browsern wie Microsoft Edge (sowohl ältere als auch neue Edge-Version).

- Benutzer können kennwortlose Anmeldeinformationen für den Zugriff auf Ressourcen in Mandanten verwenden, in denen sie nur Gäste sind, müssen aber dennoch die mehrstufige Authentifizierung in diesem Ressourcenmandanten ausführen. Weitere Informationen finden Sie unter [Mögliche doppelte Multi-Factor Authentication](../external-identities/current-limitations.md#possible-double-multi-factor-authentication).  

- Benutzer dürfen keine kennwortlosen Anmeldeinformationen innerhalb eines Mandanten registrieren, in denen sie Gäste sind. Außerdem verfügen sie über kein in diesem Mandanten verwaltetes Kennwort.  


## <a name="choose-a-passwordless-method"></a>Auswählen einer kennwortlosen Methode

Welche der drei kennwortlosen Optionen die richtige ist, hängt von den Sicherheits-, Plattform- und App-Anforderungen Ihres Unternehmens ab.

Folgende Faktoren sollten Sie bei der Auswahl der Microsoft-Technologie zur kennwortlosen Anmeldung berücksichtigen:

||**Windows Hello for Business**|**Anmelden ohne Kennwort mit der Microsoft Authenticator-App**|**FIDO2-Sicherheitsschlüssel**|
|:-|:-|:-|:-|
|**Voraussetzung**| Windows 10 Version 1809 oder höher<br>Azure Active Directory| Microsoft Authenticator-App<br>Mobiltelefon (iOS- und Android-Gerät mit Android 6.0 oder höher)|Windows 10 Version 1903 oder höher<br>Azure Active Directory|
|**Mode**|Plattform|Software|Hardware|
|**Systeme und Geräte**|PC mit integriertem Trusted Platform Module (TPM)<br>PIN und biometrische Erkennung |PIN und biometrische Erkennung per Telefon|FIDO2-Sicherheitsgeräte, die mit Microsoft kompatibel sind|
|**Benutzerfreundlichkeit**|Melden Sie sich mit einer PIN oder einer biometrischen Erkennung (Gesicht, Iris oder Fingerabdruck) bei Windows-Geräten an.<br>Die Windows Hello-Authentifizierung ist gerätegebunden; der Benutzer benötigt sowohl das Gerät als auch eine Anmeldekomponente wie eine PIN oder einen biometrischen Faktor, um auf Unternehmensressourcen zugreifen zu können.|Melden Sie sich mit einem Mobiltelefon per Fingerabdruckscan, Gesichts- oder Iriserkennung oder PIN an.<br>Die Benutzer melden sich von ihrem PC oder Mobiltelefon aus am Geschäfts- oder Privatkonto an.|Anmelden mithilfe eines FIDO2-Sicherheitsgeräts (Biometrie, PIN und NFC)<br>Der Benutzer kann auf das Gerät gemäß den Richtlinien der Organisation zugreifen und sich per PIN oder biometrischem Merkmal mit Geräten wie USB-Sicherheitsschlüsseln und NFC-fähigen Smartcards, Schlüsseln oder Wearables authentifizieren.|
|**Unterstützte Szenarios**| Kennwortfreie Benutzerführung auf Windows-Gerät.<br>Geeignet für dedizierte Arbeits-PCs mit der Möglichkeit zum einmaligen Anmelden (SSO) bei Geräten und Anwendungen.|Kennwortlose, ortsunabhängige Lösung mit Mobiltelefon.<br>Geeignet für den Zugriff auf Arbeits- oder Privatanwendungen im Web von jedem Gerät aus.|Kennwortfreie Benutzerführung für Worker mit Biometrie, PIN und NFC.<br>Geeignet für gemeinsam verwendete PCs, für die die Verwendung eines Mobiltelefons keine geeignete Option ist (z. B. für Helpdeskmitarbeiter, Nutzern von öffentlichen Terminals oder Krankenhauspersonal).|

Verwenden Sie die folgende Tabelle, um herauszufinden, welche Methode Ihre Anforderungen und Benutzer am besten unterstützt.

|Persona|Szenario|Environment|Kennwortlose Technologie|
|:-|:-|:-|:-|
|**Administrator**|Sicherer Zugriff auf ein Gerät für Verwaltungsaufgaben|Zugewiesenes Windows 10-Gerät|Windows Hello for Business und/oder FIDO2-Sicherheitsschlüssel|
|**Administrator**|Verwaltungsaufgaben auf Nicht-Windows-Geräten| Mobilgerät oder Nicht-Windows-Gerät|Anmelden ohne Kennwort mit der Microsoft Authenticator-App|
|**Information-Worker**|Produktive Arbeit|Zugewiesenes Windows 10-Gerät|Windows Hello for Business und/oder FIDO2-Sicherheitsschlüssel|
|**Information-Worker**|Produktive Arbeit| Mobilgerät oder Nicht-Windows-Gerät|Anmelden ohne Kennwort mit der Microsoft Authenticator-App|
|**Mitarbeitern im Kundenkontakt**|Terminals in Fabriken, im Einzelhandel oder zur Dateneingabe|Gemeinsam genutzte Windows 10-Geräte|FIDO2-Sicherheitsschlüssel|

## <a name="next-steps"></a>Nächste Schritte

Um mit der kennwortlosen Anmeldung in Azure AD zu beginnen, lesen Sie eine der folgenden Anleitungen:

* [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln](howto-authentication-passwordless-security-key.md)
* [Aktivieren der kennwortlosen Anmeldung per Telefon mit der Authenticator-App](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Externe Links

* [FIDO Alliance](https://fidoalliance.org/)
* [FIDO2 CTAP-Spezifikation](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
