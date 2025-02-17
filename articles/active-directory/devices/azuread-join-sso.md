---
title: Funktionsweise von SSO für lokale Ressourcen auf in Azure AD eingebundenen Geräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Umgebung für einmaliges Anmelden (SSO) durch die Konfiguration von hybrid in Azure Active Directory eingebundenen Geräten erweitern.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 05df955e85bf5de12e3cc8892b1a2323947dd66f
ms.sourcegitcommit: 92dd25772f209d7d3f34582ccb8985e1a099fe62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114227860"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Funktionsweise von SSO für lokale Ressourcen auf in Azure AD eingebundenen Geräten

Es wird Sie wahrscheinlich nicht überraschen, dass für ein in Azure Active Directory (Azure AD) eingebundenes Gerät einmaliges Anmelden (Single Sign-On, SSO) für die Cloud-Apps Ihres Mandanten möglich ist. Falls Ihre Umgebung über eine lokale Active Directory-Instanz verfügt, können Sie die SSO-Umgebung auch auf in Azure AD eingebundenen Geräten auf Ressourcen und Anwendungen ausdehnen, die die lokale AD-Instanz verwenden. 

In diesem Artikel wird die entsprechende Vorgehensweise beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Für das lokale einmalige Anmelden ist eine Kommunikation in Sichtverbindung mit Ihren lokalen AD DS-Domänencontrollern erforderlich. Wenn in Azure AD eingebundene Geräte nicht mit dem Netzwerk Ihrer Organisation verbunden sind, ist ein VPN oder eine andere Netzwerkinfrastruktur erforderlich. 

## <a name="how-it-works"></a>Funktionsweise 

Mit einem in Azure AD eingebundenen Gerät verfügen Ihre Benutzer bereits über eine SSO-Umgebung für die Cloud-Apps in Ihrer Umgebung. Falls Ihre Umgebung über eine Azure AD-Instanz und eine lokale AD-Instanz verfügt, möchten Sie wahrscheinlich die SSO-Umgebung um lokale Branchen-Apps, Dateifreigaben und Drucker erweitern.

In Azure AD eingebundene Geräte haben keine Informationen zu Ihrer lokalen AD-Umgebung, da sie nicht darin eingebunden sind. Sie können aber mit Azure AD Connect zusätzliche Informationen zu Ihrer lokalen AD-Umgebung auf diesen Geräten bereitstellen.

Bei Verwendung einer sowohl aus Azure AD als auch lokalem AD bestehenden Hybridumgebung haben Sie Azure AD Connect wahrscheinlich bereits bereitgestellt, um Ihre lokalen Identitätsinformationen mit der Cloud zu synchronisieren. Im Rahmen des Synchronisierungsprozesses synchronisiert Azure AD Connect Informationen über lokale Benutzer und Domänen mit Azure AD. Wenn sich ein Benutzer in einer Hybridumgebung an einem in Azure AD eingebundenen Gerät anmeldet, ist der Ablauf wie folgt:

1. Azure AD sendet die Details der lokalen Domäne des Benutzers zusammen mit dem [primären Aktualisierungstoken](concept-primary-refresh-token.md) wieder an das Gerät zurück.
1. Der LSA-Dienst (lokale Sicherheitsautorität) ermöglicht die Kerberos- und NTLM-Authentifizierung auf dem Gerät.

>[!NOTE]
> Für Windows Hello for Business ist eine zusätzliche Konfiguration erforderlich, um lokales einmaliges Anmelden über ein in Azure AD eingebundenes Gerät zu ermöglichen. Weitere Informationen finden Sie unter [Configure Azure AD joined devices for On-premises Single-Sign On using Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base) (Konfigurieren von in Azure AD eingebundenen Geräten für lokales einmaliges Anmelden mit Windows Hello for Business). 
>
> Die kennwortlose Authentifizierung mit FIDO2-Sicherheitsschlüsseln bei Windows 10 erfordert eine zusätzliche Konfiguration zum Aktivieren von lokalem einmaligem Anmelden über ein mit Azure AD verbundenes Gerät. Weitere Informationen finden Sie unter [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei lokalen Ressourcen mit Azure Active Directory](../authentication/howto-authentication-passwordless-security-key-on-premises.md). 

Bei einem Zugriffsversuch auf eine Ressource, die Kerberos oder NTLM in der lokalen Umgebung des Benutzers anfordert, geschieht auf dem Gerät Folgendes:

1. Die Informationen der lokalen Domäne und die Benutzeranmeldeinformationen werden an den ermittelten DC gesendet, um den Benutzer zu authentifizieren.
1. Das Gerät empfängt von Kerberos ein [Ticket Granting Ticket (TGT)](/windows/desktop/secauthn/ticket-granting-tickets) oder ein NTLM-Token, das auf dem von der lokalen Ressource oder Anwendung unterstützten Protokoll basiert. Wenn beim Versuch, das Kerberos-TGT oder das NTLM-Token für die Domäne abzurufen, ein Fehler auftritt (das zugehörige DCLocator-Timeout kann zu einer Verzögerung führen), werden Wiederholungsversuche mithilfe von Einträgen der Anmeldeinformationsverwaltung unternommen, oder der Benutzer wird ein Popupfenster zur Authentifizierung angezeigt, in dem Anmeldeinformationen für die Zielressource angefordert werden.

Alle Apps, die für die **integrierte Windows-Authentifizierung** konfiguriert sind, erhalten SSO auf nahtlose Weise, wenn ein Benutzer darauf zugreift.

## <a name="what-you-get"></a>Ergebnis

Mit SSO haben Sie auf einem in Azure AD eingebundenen Gerät folgende Möglichkeiten: 

- Zugreifen auf einen UNC-Pfad auf einem AD-Mitgliedsserver
- Zugreifen auf einen AD-Mitgliedswebserver, der für integrierte Windows-Sicherheit konfiguriert ist 

Falls Sie Ihre lokale AD-Instanz über ein Windows-Gerät verwalten möchten, können Sie die [Remoteserver-Verwaltungstools für Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) installieren.

Verwenden Sie Folgendes:

- Snap-In „Active Directory-Benutzer und -Computer“ (ADUC) zum Verwalten aller AD-Objekte. Allerdings müssen Sie die Domäne, mit der eine Verbindung hergestellt werden soll, manuell angeben.
- DHCP-Snap-In zum Verwalten eines in AD eingebundenen DHCP-Servers. Allerdings müssen Sie ggf. den DHCP-Servernamen oder die -Adresse angeben.
 
## <a name="what-you-should-know"></a>Wichtige Informationen

Unter Umständen müssen Sie Ihre [domänenbasierte Filterung](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) in Azure AD Connect anpassen, um sicherzustellen, dass die Daten zu den erforderlichen Domänen synchronisiert werden.

Apps und Ressourcen, die von der Active Directory-Computerauthentifizierung abhängig sind, funktionieren nicht, da in Azure AD eingebundene Geräte nicht über ein Computerobjekt in AD verfügen. 

Es ist nicht möglich, Dateien auf einem in Azure AD eingebundenen Gerät für andere Benutzer freizugeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Worum handelt es sich bei der Geräteverwaltung in Azure Active Directory?](overview.md).