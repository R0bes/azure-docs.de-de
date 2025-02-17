---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: ed17f43eadcc6771ade990e5f023773898ad459f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121802560"
---
## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Vorgehensweise der Antwort des APIM-Proxyservers mit SSL-Zertifikaten beim TLS-Handshake

### <a name="clients-calling-with-sni-header"></a>Aufrufen von Clients mit dem SNI-Header
Wenn der Kunde eine oder mehrere benutzerdefinierte für den Proxy konfigurierte Domänen besitzt, kann APIM auf HTTPS-Anforderungen von benutzerdefinierten Domänen (z.B. „contoso.com“) sowie der Standarddomäne (z.B. „apim-service-name.azure-api.net“) reagieren. Anhand der Informationen im SNI-Header (Server Name Indication, Servernamensanzeige), antwortet APIM mit dem entsprechenden Serverzertifikat.

### <a name="clients-calling-without-sni-header"></a>Aufrufen von Clients ohne den SNI-Header
Wenn der Kunde einen Client verwendet, der den [SNI](https://tools.ietf.org/html/rfc6066#section-3)-Header nicht sendet, antwortet APIM gemäß der folgenden Logik:

* Wenn der Dienst nur eine für den Proxy konfigurierte benutzerdefinierte Domäne enthält, ist das Standardzertifikat das Zertifikat, das für die benutzerdefinierte Proxydomäne ausgestellt wurde.
* Wenn der Dienst mehrere benutzerdefinierte Domänen für den Proxy konfiguriert hat (Unterstützung in den Tarifen **Developer** und **Premium**), kann der Kunde festlegen, welches Zertifikat als Standardzertifikat verwendet werden soll. Zum Festlegen des Standardzertifikats sollte die Eigenschaft [defaultSslBinding](/rest/api/apimanagement/2020-12-01/api-management-service/create-or-update#hostnameconfiguration) auf „true“ („defaultSslBinding“:„true“) festgelegt werden. Wenn der Kunde die Eigenschaft nicht festgelegt, ist das Standardzertifikat das Zertifikat, das für die unter „*.azure-api.net“ gehostete Proxystandarddomäne ausgestellt wird.

## <a name="support-for-putpost-request-with-large-payload"></a>Unterstützung für PUT/POST-Anforderung mit großer Nutzlast

Der APIM-Proxyserver unterstützt Anforderungen mit großer Nutzlast (z.B. Nutzlast > 40 KB) bei der Verwendung von clientseitigen Zertifikaten in HTTPS. Um zu verhindern, dass die Serveranforderung blockiert wird, können Kunden die Eigenschaft [„negotiateClientCertificate“: „true“](/rest/api/apimanagement/2020-12-01/api-management-service/create-or-update#hostnameconfiguration) für den Proxyhostnamen festlegen. Wenn die Eigenschaft auf „true“ festgelegt ist, wird vor jedem Austausch von HTTP-Anforderungen zum Zeitpunkt der SSL/TLS-Verbindung das Clientzertifikat angefordert. Da die Einstellung für die Ebene **Proxyhostname** gilt, wird bei allen Verbindungsanforderungen das Clientzertifikat angefordert. Kunden können bis zu 20 benutzerdefinierte Domänen für den Proxy konfigurieren (nur im **Premium**-Tarif unterstützt) und diese Einschränkung umgehen.