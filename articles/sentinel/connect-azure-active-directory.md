---
title: Verknüpfen von Azure Active Directory-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten aus Azure Active Directory sammeln und Anmelde-, Überwachungs- und Bereitstellungsprotokolle von Azure AD an Azure Sentinel streamen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/21/2021
ms.author: yelevin
ms.openlocfilehash: 5faf675d4e5d188840b3e9ab457ec28829f52dc0
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339008"
---
# <a name="connect-azure-active-directory-azure-ad-data-to-azure-sentinel"></a>Verknüpfen von Azure AD-Daten (Azure Active Directory) mit Azure Sentinel

> [!IMPORTANT]
> Wie weiter unten angegeben, befinden sich einige der verfügbaren Protokolltypen derzeit in der **VORSCHAUPHASE**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Sie können den in Azure Sentinel integrierten Connector verwenden, um Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) zu erfassen und in Azure Sentinel zu streamen. Der Connector ermöglicht das Streamen folgender Protokolltypen:

- [**Anmeldeprotokolle**](../active-directory/reports-monitoring/concept-all-sign-ins.md): Diese Protokolle enthalten Informationen zu interaktiven Benutzeranmeldungen, bei denen ein Benutzer einen Authentifizierungsfaktor bereitstellt.

    Der Azure AD-Connector beinhaltet nun drei zusätzliche Kategorien von Anmeldeprotokollen (aktuell in der **VORSCHAUPHASE**):
    
    - [**Protokolle für nicht interaktive Benutzeranmeldungen**](../active-directory/reports-monitoring/concept-all-sign-ins.md#non-interactive-user-sign-ins): Diese Protokolle enthalten Informationen zu Anmeldungen, die von einem Client im Namen eines Benutzers und ohne Interaktion oder Authentifizierungsfaktor des Benutzers vorgenommen werden.
    
    - [**Protokolle für Dienstprinzipalanmeldungen**](../active-directory/reports-monitoring/concept-all-sign-ins.md#service-principal-sign-ins): Diese Protokolle enthalten Informationen zu Anmeldungen durch Apps und Dienstprinzipale ohne Benutzerinteraktion. Bei diesen Anmeldungen stellt die App oder der Dienst eigene Anmeldeinformationen für die Authentifizierung oder den Zugriff auf Ressourcen bereit.
    
    - [**Protokolle für Anmeldungen mit verwalteter Identität**](../active-directory/reports-monitoring/concept-all-sign-ins.md#managed-identity-for-azure-resources-sign-ins): Diese Protokolle enthalten Informationen zu Anmeldungen durch Azure-Ressourcen mit Geheimnissen, die von Azure verwaltet werden. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

- [**Überwachungsprotokolle**](../active-directory/reports-monitoring/concept-audit-logs.md): Diese Protokolle enthalten Informationen zu Systemaktivitäten im Zusammenhang mit der Benutzer- und Gruppenverwaltung, mit verwalteten Anwendungen und mit Verzeichnisaktivitäten.

- [**Bereitstellungsprotokolle**](../active-directory/reports-monitoring/concept-provisioning-logs.md) (ebenfalls in der **VORSCHAUPHASE**): Diese Protokolle enthalten Informationen zu Systemaktivitäten im Zusammenhang mit Benutzern, Gruppen und Rollen, die durch den Azure AD-Bereitstellungsdienst bereitgestellt werden. 


## <a name="prerequisites"></a>Voraussetzungen

- Für die Aufnahme von Anmeldeprotokollen in Azure Sentinel ist eine Azure Active Directory P1- oder P2-Lizenz erforderlich. Eine beliebige Azure AD-Lizenz (Free/O365/P1/P2) reicht aus, um die anderen Protokolltypen zu ingestieren. Für Azure Monitor (Log Analytics) und Azure Sentinel können zusätzliche Gebühren auf Gigabytebasis anfallen.

- Ihrem Benutzer muss die Rolle „Azure Sentinel-Mitwirkender“ für den Arbeitsbereich zugewiesen sein.

- Ihrem Benutzer muss die Rolle „Globaler Administrator“ oder „Sicherheitsadministrator“ für den Mandanten zugewiesen sein, von dem aus Sie die Protokolle streamen möchten.

- Ihr Benutzer muss über Lese- und Schreibberechtigungen für die Azure AD-Diagnoseeinstellungen verfügen, um den Verbindungsstatus sehen zu können. 

## <a name="connect-to-azure-active-directory"></a>Herstellen einer Verbindung mit Azure Active Directory

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Katalog für Datenconnectors **Azure Active Directory** aus, und klicken Sie anschließend auf **Open connector page** (Connectorseite öffnen).

1. Aktivieren Sie die Kontrollkästchen neben den Protokolltypen, die Sie an Azure Sentinel streamen möchten (wie weiter oben beschrieben), und klicken Sie auf **Verbinden**.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter **LogManagement** in den folgenden Tabellen angezeigt:

- `SigninLogs`
- `AuditLogs`
- `AADNonInteractiveUserSignInLogs`
- `AADServicePrincipalSignInLogs`
- `AADManagedIdentitySignInLogs`
- `AADProvisioningLogs`

Geben Sie zum Abfragen der Azure AD-Protokolle im oberen Bereich des Abfragefensters den Tabellennamen ein.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Azure Active Directory mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
