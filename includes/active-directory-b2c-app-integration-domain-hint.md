---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: bade30eb42235e65170fc59ad77fa2f4ce1114c3
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121801680"
---
## <a name="preselect-an-identity-provider"></a>Vorabauswahl eines Identitätsanbieters

Wenn Sie die User Journey für die Anmeldung bei Ihrer Anwendung so konfiguriert haben, dass Konten für soziale Netzwerke inbegriffen sind, wie z.B. Facebook, LinkedIn oder Google, können Sie den Parameter `domain_hint` angeben. Dieser Abfrageparameter enthält einen Hinweis für Azure AD B2C zu dem sozialen Netzwerk als Identitätsanbieter, das für die Anmeldung verwendet werden sollte. Wenn in der Anwendung beispielsweise `domain_hint=facebook.com` angegeben ist, erfolgt der Anmeldefluss direkt auf der Anmeldeseite von Facebook. 

Gehen Sie wie folgt vor, um Benutzer zu einem externen Identitätsanbieter umzuleiten:
