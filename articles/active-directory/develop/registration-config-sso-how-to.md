---
title: Konfigurieren des einmaliges Anmeldens für Anwendungen
description: Konfigurieren des einmaliges Anmeldens für eine mit Azure AD entwickelte und registrierte benutzerdefinierte Anwendung
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, has-adal-ref
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 5fa84215c6393fe1bddee3d72dd93951dee06f31
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2021
ms.locfileid: "122428992"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Konfigurieren des einmaliges Anmeldens für eine Anwendung

Die Aktivierung der einmaligen Verbundanmeldung in Ihrer App wird beim Verbund mit Azure AD für OpenID Connect, SAML 2.0 oder WS-Fed automatisch aktiviert. Wenn sich Ihre Endbenutzer anmelden müssen, anstatt bereits über eine vorhandene Sitzung mit Azure AD zu verfügen, ist es wahrscheinlich, dass Ihre App möglicherweise falsch konfiguriert ist.

* Wenn Sie ADAL/MSAL verwenden, stellen Sie sicher, dass für **PromptBehavior** die Option **Auto** statt **Immer** festgelegt ist.

* Wenn Sie eine mobile App erstellen, sind möglicherweise zusätzliche Konfigurationen erforderlich, um Broker-SSO oder Nicht-Broker-SSO zu aktivieren.

Informationen für Android-Geräte finden Sie unter [Aktivieren von App-übergreifendem SSO in Android](../azuread-dev/howto-v1-enable-sso-android.md).<br>

Informationen für iOS-Geräte finden Sie unter [Aktivieren von App-übergreifendem SSO in iOS](../azuread-dev/howto-v1-enable-sso-ios.md).

## <a name="next-steps"></a>Nächste Schritte

[Azure AD-SSO](../manage-apps/what-is-single-sign-on.md)<br>

[Aktivieren von App-übergreifendem SSO in Android](../azuread-dev/howto-v1-enable-sso-android.md)<br>

[Aktivieren von App-übergreifendem SSO in iOS](../azuread-dev/howto-v1-enable-sso-ios.md)<br>

[Integrieren von Apps in Azure AD](./quickstart-register-app.md)<br>

[Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](./v2-permissions-and-consent.md)<br>

[AzureAD Microsoft Q&A](/answers/topics/azure-active-directory.html)