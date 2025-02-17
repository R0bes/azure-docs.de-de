---
title: App-Transparenz und -Steuerung mit Microsoft Cloud App Security
description: Hier erfahren Sie, wie Sie App-Risikostufen identifizieren, Sicherheitsverletzungen und Datenlecks in Echtzeit verhindern und App-Connectors verwenden, um Anbieter-APIs für Transparenz und Governance zu nutzen.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 07/29/2021
ms.author: davidmu
ms.collection: M365-identity-device-management
ms.reviewer: bokacevi, dacurwin
ms.openlocfilehash: ffc4a35aef2304a650f5673a26f0e4fd532e23be
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339636"
---
# <a name="cloud-app-visibility-and-control"></a>Sichtbarkeit und Steuerung von Cloud-Apps

Zur optimalen Nutzung von Cloud-Apps und Clouddiensten müssen IT-Teams das richtige Verhältnis zwischen Zugriffsunterstützung und Kontrolle zum Schutz kritischer Daten finden. Microsoft Cloud App Security bietet umfassende Transparenz und Kontrolle über den Datenverkehr sowie ausgeklügelte Analysefunktionen zur Erkennung und Abwehr von Cyberbedrohungen für alle Ihre Clouddienste (sowohl von Microsoft als auch von Drittanbietern).

## <a name="discover-and-manage-shadow-it-in-your-network"></a>Entdecken und Verwalten von Schatten-IT in Ihrem Netzwerk

Wenn IT-Administratoren gefragt werden, wie viele Cloud-Apps von ihren Mitarbeitern verwendet werden, gehen sie im Durchschnitt von 30 bis 40 aus. Tatsächlich werden allerdings durchschnittlich mehr als 1.000 verschiedene Apps von den Mitarbeitern einer Organisation verwendet. Mithilfe von Shadow IT können Sie ermitteln, welche Apps verwendet werden und welche Risikostufe ihnen zugeordnet ist. 80 Prozent der Mitarbeiter verwenden nicht sanktionierte Apps, die von niemandem überprüft wurden und möglicherweise nicht Ihren Sicherheits- und Compliancerichtlinien entsprechen. Da Ihre Mitarbeiter heutzutage die Möglichkeit haben, von außerhalb Ihres Unternehmensnetzwerks auf Ihre Ressourcen und Apps zuzugreifen, reicht es nicht mehr aus, Richtlinien und Regeln für Ihre Firewalls festzulegen.

Mit Microsoft Cloud App Discovery (einem Azure Active Directory Premium P1-Feature) können Sie die verwendeten Apps ermitteln, die Risiken dieser App untersuchen, Richtlinien zur Identifizierung neuer riskanter Apps konfigurieren und die Sanktionierung dieser Apps aufheben, um sie nativ mithilfe Ihrer Proxy- oder Firewallappliance zu blockieren.

- Ermitteln und Identifizieren von Schatten-IT
- Auswertung und Analyse
- Verwalten Ihrer Apps
- Erweiterte Berichte zur Schatten-IT-Ermittlung
- Kontrollieren von sanktionierten Apps

### <a name="learn-more"></a>Weitere Informationen

- [Erkennen und Verwalten von Schatten-IT in Ihrem Netzwerk](/cloud-app-security/tutorial-shadow-it)
- [Mit Cloud App Security entdeckte Apps](/cloud-app-security/discovered-apps)

## <a name="user-session-visibility-and-control"></a>Transparenz und Steuerung von Benutzersitzungen

Heutzutage reicht es häufig nicht aus, wenn Sie erst im Nachhinein von Vorgängen in Ihrer Cloudumgebung erfahren. Sicherheitsverletzungen und Datenlecks sollten nach Möglichkeit verhindert werden, bevor Mitarbeiter absichtlich oder versehentlich Ihre Daten und Ihre Organisation gefährden. Die dazu nötigen Funktionen werden von Microsoft Cloud App Security in Kombination mit Azure Active Directory (Azure AD) in Form einer ganzheitlichen und integrierten Lösung mit App-Steuerung für bedingten Zugriff bereitgestellt.

Die Sitzungssteuerung verwendet eine Reverseproxyarchitektur und ist auf einzigartige Weise in den bedingten Azure AD-Zugriff integriert. Bedingter Azure AD-Zugriff ermöglicht Ihnen das Erzwingen von Zugriffssteuerungen für die Apps Ihrer Organisation auf der Grundlage bestimmter Bedingungen. Die Bedingungen definieren, auf wen (Benutzer oder Benutzergruppe), auf was (welche Cloud-Apps) und wo (welche Orte und Netzwerke) eine Richtlinie für bedingten Zugriff angewendet wird. Nach Bestimmung der Bedingungen können Sie Benutzer an Cloud App Security weiterleiten, wo Ihre Daten in Echtzeit geschützt werden können.  

Diese Steuerung ermöglicht Folgendes:

- Steuern von Dateidownloads
- Überwachen von B2B-Szenarien  
- Steuern des Dateizugriffs  
- Schützen von Dokumenten beim Herunterladen  

### <a name="learn-more"></a>Weitere Informationen

- [Schützen von Apps mithilfe der Sitzungssteuerung in Cloud App Security](/cloud-app-security/proxy-intro-aad)

## <a name="advanced-app-visibility-and-controls"></a>Erweiterte App-Transparenz und -Steuerung

App-Connectors nutzen die APIs von App-Anbietern, um für die Apps, mit denen Sie eine Verbindung herstellen, die Transparenz und Steuerung durch Microsoft Cloud App Security zu verbessern.
Cloud App Security nutzt die vom Cloudanbieter bereitgestellten APIs. Jeder Dienst hat seine eigenen Framework- und API-Einschränkungen wie etwa Drosselung, API-Grenzwerte und dynamische API-Fenster mit Zeitversatz. Das Cloud App Security-Produktteam hat mit diesen Diensten gearbeitet, um die API-Verwendung zu optimieren und die bestmögliche Leistung zu erzielen. Von den Cloud App Security-Engines wird die maximal zulässige Kapazität genutzt (unter Berücksichtigung verschiedener API-Einschränkungen, die von den Diensten erzwungen werden). Einige Vorgänge wie etwa das Scannen aller Dateien im Mandanten erfordern zahlreiche API-Aufrufe und erstrecken sich daher über einen längeren Zeitraum. Bei einigen Richtlinien ist mit einer Ausführungsdauer von mehreren Stunden oder Tagen zu rechnen.

### <a name="learn-more"></a>Weitere Informationen

- [Verbinden von Apps in Cloud App Security](/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)

## <a name="next-steps"></a>Nächste Schritte

- [Erkennen und Verwalten von Schatten-IT in Ihrem Netzwerk](/cloud-app-security/tutorial-shadow-it)
- [Mit Cloud App Security entdeckte Apps](/cloud-app-security/discovered-apps)
- [Schützen von Apps mithilfe der Sitzungssteuerung in Cloud App Security](/cloud-app-security/proxy-intro-aad)
- [Verbinden von Apps in Cloud App Security](/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)
