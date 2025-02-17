---
title: Zweistufige Überprüfung für Azure AD MFA und AD FS – Azure Active Directory
description: Auf dieser Seite zur Azure AD Multi-Factor Authentication werden die ersten Schritte mit Azure AD MFA und AD FS beschrieben.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/27/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9abc9e2e2ca2b9305cccf5cec554994c3703ef1
ms.sourcegitcommit: f53f0b98031cd936b2cd509e2322b9ee1acba5d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2021
ms.locfileid: "123214508"
---
# <a name="getting-started-with-azure-ad-multi-factor-authentication-and-active-directory-federation-services"></a>Erste Schritte mit Azure AD Multi-Factor Authentication und Active Directory-Verbunddienste (AD FS)

<center>

![Erste Schritte mit Azure AD MFA und AD FS](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Wenn Ihre Organisation über einen Verbund Ihres lokalen Active Directory mit Azure Active Directory über AD FS verfügt, gibt es zwei Optionen für die Verwendung von Azure AD Multi-Factor Authentication.

* Schützen von Cloudressourcen mit Azure AD Multi-Factor Authentication oder Active Directory-Verbunddienste (AD FS)
* Sichern von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server

In der folgenden Tabelle sind die Überprüfungsverfahren beim Schützen von Ressourcen mit Azure AD Multi-Factor Authentication und AD FS zusammengefasst.

| Überprüfungsvorgang bei browserbasierten Apps | Überprüfungsvorgang bei Nicht-Browser-Apps |
|:--- |:--- |
| Schützen von Azure AD-Ressourcen mit Azure AD Multi-Factor Authentication |<li>Der erste Überprüfungsschritt erfolgt lokal über AD FS.</li> <li>Der zweite Schritt ist eine telefonbasierte Methode mithilfe der Cloudauthentifizierung.</li> |
| Sichern von Azure AD-Ressourcen mit Active Directory-Verbunddiensten |<li>Der erste Überprüfungsschritt erfolgt lokal über AD FS.</li><li>Der zweite Schritt wird lokal unter Berücksichtigung des Anspruchs ausgeführt.</li> |

Sicherheitshinweise zu App-Kennwörtern für Verbundbenutzer:

* App-Kennwörter werden über die Cloudauthentifizierung überprüft, daher werden Verbunde umgangen. Der Verbund wird nur beim Einrichten eines App-Kennworts aktiv verwendet.
* Lokale Einstellungen für die Clientzugriffssteuerung werden von App-Kennwörtern nicht berücksichtigt.
* Wenn Sie App-Kennwörter verwenden, geht die Möglichkeit zum lokalen Protokollieren der Authentifizierung verloren.
* Das Deaktivieren oder Löschen von Konten kann für die Verzeichnissynchronisierung bis zu drei Stunden dauern, sodass das Deaktivieren bzw. Löschen von App-Kennwörtern in der Cloudidentität verzögert wird.

Weitere Informationen zum Einrichten von Azure AD Multi-Factor Authentication oder des Azure Multi-Factor Authentication-Servers mit AD FS finden Sie in den folgenden Artikeln:

* [Schützen von Cloudressourcen mit Azure AD Multi-Factor Authentication und AD FS](howto-mfa-adfs.md)
* [Sichern von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit einem Windows Server](howto-mfaserver-adfs-windows-server.md)
* [Sichern von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit AD FS 2.0](howto-mfaserver-adfs-2.md)
