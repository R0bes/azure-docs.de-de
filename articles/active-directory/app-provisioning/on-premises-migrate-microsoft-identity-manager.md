---
title: Exportieren eines Microsoft Identity Manager-Connectors für die Verwendung mit dem Azure AD-ECMA-Connectorhost
description: Dieser Artikel beschreibt, wie ein Connector für die Verwendung mit dem Azure AD-ECMA-Connectorhost erstellt und aus MIM Sync exportiert werden kann.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/01/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0731e35eab3a2872cd3c07e5401577fd51c33323
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114437295"
---
# <a name="export-a-microsoft-identity-manager-connector-for-use-with-the-azure-ad-ecma-connector-host"></a>Exportieren eines Microsoft Identity Manager-Connectors für die Verwendung mit dem Azure AD-ECMA-Connectorhost

>[!IMPORTANT]
> Die Vorschauversion zur lokalen Bereitstellung ist derzeit nur mit Einladung verfügbar. Verwenden Sie das [Zugriffsanforderungsformular](https://aka.ms/onpremprovisioningpublicpreviewaccess), um Zugriff auf die Funktion anzufordern. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

Sie können eine Konfiguration für einen bestimmten Connector aus einer Forefront Identity Manager-Synchronisierungsdienst- oder Microsoft Identity Manager Synchronization Service-Installation (MIM Sync) in den Azure Active Directory (Azure AD) ECMA-Connectorhost importieren. Die MIM Sync-Installation kann nur für die Konfiguration und nicht für die laufende Synchronisierung mit Azure AD verwendet werden.

>[!IMPORTANT]
>Derzeit unterstützt nur der generische SQL-Connector (GSQL) die Verwendung mit dem Azure AD-ECMA-Connectorhost.

## <a name="create-and-export-a-connector-configuration-in-mim-sync"></a>Erstellen und Exportieren einer Connectorkonfiguration in bzw. aus MIM Sync
Wenn Sie MIM Sync bereits mit Ihrem ECMA-Connector konfiguriert haben, fahren Sie mit Schritt 10 fort.

 1. Bereiten Sie einen Server mit Windows Server 2016 vor, der sich von dem Server unterscheidet, der zum Ausführen des Azure AD-ECMA-Connectorhosts verwendet wird. Dieser Hostserver sollte entweder über eine SQL Server 2016-Datenbank in Colocation oder über eine Netzwerkverbindung mit einer SQL Server 2016-Datenbank verfügen. Der Server kann beispielsweise eingerichtet werden, indem eine Azure-VM mit dem Image **SQL Server 2016 SP1 Standard unter Windows Server 2016** bereitgestellt wird. Dieser Server benötigt keine Internetverbindung, sondern ausschließlich einen Remotedesktopzugriff zu Einrichtungszwecken.
 1. Erstellen Sie ein Konto, dass Sie während der MIM Sync-Installation verwenden können. Dabei kann es sich z. B. um ein lokales Konto auf der Windows Server-Instanz handeln. Öffnen Sie zum Erstellen eines lokalen Kontos zunächst **Systemsteuerung** > **Benutzerkonten**, und fügen Sie ein **mimsync**-Benutzerkonto hinzu.
 1. Fügen Sie nun das im vorherigen Schritt erstellte Konto der lokalen Administratorgruppe hinzu.
 1. Erteilen Sie dem gerade erstellten Konto die Berechtigung, einen Dienst auszuführen. Starten Sie **Lokale Sicherheitsrichtlinie**, klicken Sie auf **Lokale Richtlinien** > **Zuweisen von Benutzerrechten** > **Anmelden als Dienst**. Fügen Sie das zuvor erwähnte Konto hinzu.
 1. Installieren Sie MIM Sync auf diesem Host. Wenn Sie nicht über MIM Sync-Binärdateien verfügen, können Sie eine Evaluierungssoftware installieren, indem Sie die ZIP-Datei über [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=48244) herunterladen, das ISO-Image einbinden und den Ordner **Synchronisierungsdienst** auf den Windows Server-Host kopieren. Führen Sie dann das in diesem Ordner enthaltene Setupprogramm aus. Evaluierungssoftware ist zeitlich begrenzt und läuft ab. Sie ist nicht für Produktionszwecke vorgesehen.
 1. Nachdem die Installation von MIM Sync abgeschlossen wurde, müssen Sie sich zunächst abmelden und dann erneut anmelden.
 1. Installieren Sie Ihren Connector auf demselben Server wie MIM Sync. (Diese Testumgebungsanleitung veranschaulicht die Verwendung eines von Microsoft bereitgestellten Connectors, der über [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495) heruntergeladen werden kann).
 1. Starten der Synchronisierungsdienst-Benutzeroberfläche Wählen Sie **Verwaltungs-Agents** aus. Wählen Sie **Erstellen** aus, und geben Sie den Verwaltungs-Agent des entsprechenden Connectors an. Achten Sie darauf, für den Connector einen ECMA-basierten Verwaltungs-Agent auszuwählen.
 1. Geben Sie dem Connector einen Namen und konfigurieren Sie die Parameter, die zum Importieren und Exportieren von Daten in den Connector erforderlich sind. Stellen Sie sicher, dass der Connector einwertige Zeichenfolgenattribute eines Benutzer- oder Personenobjekttyps importieren und exportieren kann.
 1. Starten Sie auf dem MIM Sync-Servercomputer die Synchronisierungsdienst-Benutzeroberfläche, sofern diese noch nicht gestartet wurde. Wählen Sie **Verwaltungs-Agents** aus.
 1. Wählen Sie den Connector aus, und klicken Sie auf **Verwaltungs-Agent exportieren**. Speichern Sie die XML-Datei und die DLL sowie die damit verbundene Software für Ihren Connector auf dem Windows-Server, der den ECMA-Connectorhost enthalten wird.

An diesem Punkt wird der MIM Sync-Server nicht mehr benötigt.

 1. Melden Sie sich bei Windows Server mit dem Konto an, über das der Azure AD-ECMA-Connectorhost ausgeführt wird.
 1. Wechseln Sie zum Verzeichnis C:\Programme\Microsoft ECMA2host\Service\ECMA. Stellen Sie sicher, dass in diesem Verzeichnis bereits eine oder mehrere DLLs vorhanden sind. (Diese DLLs entsprechen den von Microsoft bereitgestellten Connectors.)
 1. Kopieren Sie die MA-DLL für Ihren Connector und alle weiteren erforderlichen DLLs in dasselbe ECMA-Unterverzeichnis des Dienstverzeichnisses.
 1. Wechseln Sie zum Verzeichnis C:\Programme\Microsoft ECMA2Host\Wizard. Führen Sie das Programm Microsoft.ECMA2Host.ConfigWizard.exe aus, um den ECMA-Connectorhost zu konfigurieren.
 1. Es wird Ihnen ein neues Fenster mit einer Connectorliste angezeigt. Standardmäßig sind darin keine Connectors vorhanden. Wählen Sie **Neuer Connector** aus.
 1. Geben Sie die XML-Datei des Verwaltungs-Agents an, die Sie zuvor aus MIM exportiert haben. Fahren Sie mit den Anweisungen für die Konfiguration und die Schemazuordnung aus dem Abschnitt „Konfigurieren eines Connectors“ fort.

## <a name="next-steps"></a>Nächste Schritte

- [App-Bereitstellung](user-provisioning.md)
- [Azure AD-ECMA-Connectorhost: Voraussetzungen](on-premises-ecma-prerequisites.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Azure AD-ECMA-Connectorhost: Konfiguration](on-premises-ecma-configure.md)
- [Generischer SQL-Connector](on-premises-sql-connector-configure.md)
