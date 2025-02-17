---
title: Firewallregeln – Azure Database for MySQL
description: Erfahren Sie mehr über die Verwendung von Firewallregeln zum Aktivieren von Verbindungen mit Ihrem Azure Database for MySQL-Server.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 83b2d95e875ee25137a69ff310de244e846f1753
ms.sourcegitcommit: 8b38eff08c8743a095635a1765c9c44358340aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "122639702"
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Firewallregeln für Azure-Datenbank für MySQL-Server

[!INCLUDE[applies-to-mysql-single-server](includes/applies-to-mysql-single-server.md)]

Firewalls verhindern jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Serverzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

Zum Konfigurieren einer Firewall erstellen Sie Firewallregeln, die Bereiche zulässiger IP-Adressen festlegen. Sie können Firewallregeln auf Serverebene erstellen.

**Firewallregeln**: Diese Regeln ermöglichen es Clients, auf den gesamten Azure Database for MySQL-Server zuzugreifen, also auf alle Datenbanken im selben logischen Server. Firewallregeln auf Serverebene können über das Azure-Portal oder mithilfe von Azure CLI-Befehlen konfiguriert werden. Zum Erstellen von Firewallregeln auf Serverebene müssen Sie der Besitzer oder ein Mitwirkender des Abonnements sein.

## <a name="firewall-overview"></a>Firewallübersicht
Jeglicher Datenbankzugriff auf Ihren Azure Database for MySQL-Server wird standardmäßig von der Firewall blockiert. Damit Sie Ihren Server über einen anderen Computer verwenden können, müssen Sie eine oder mehrere Firewallregeln auf Serverebene angeben, die Zugriff auf Ihren Server ermöglichen. Legen Sie mithilfe der Firewallregeln fest, welche IP-Adressbereiche aus dem Internet zugelassen werden sollen. Der Zugriff auf die Website des Azure-Portals selbst wird durch die Firewallregeln nicht beeinträchtigt.

Verbindungsversuche über das Internet und Azure müssen zunächst die Firewall passieren, bevor sie Ihren Azure-Datenbank für MySQL-Server erreichen (wie im folgenden Diagramm dargestellt):

:::image type="content" source="./media/concepts-firewall-rules/1-firewall-concept.png" alt-text="Beispielfluss zur Funktionsweise der Firewall":::

## <a name="connecting-from-the-internet"></a>Herstellen einer Verbindung über das Internet
Firewallregeln auf Serverebene wirken sich auf alle Datenbanken auf dem Azure-Datenbank für MySQL-Server aus.

Liegt die IP-Adresse der Anforderung innerhalb eines der in den Firewallregeln auf Serverebene angegebenen Bereiche, wird die Verbindung gewährt.

Liegt die IP-Adresse der Anforderung außerhalb der in den Firewallregeln auf Datenbankebene oder Serverebene angegebenen Bereiche, schlägt die Verbindungsanforderung fehl.

## <a name="connecting-from-azure"></a>Herstellen einer Verbindung über Azure
Es wird empfohlen, die IP-Adresse für ausgehenden Datenverkehr einer beliebigen Anwendung oder eines Diensts zu ermitteln und den Zugriff auf die einzelnen IP-Adressen oder -Bereiche explizit zuzulassen. Sie können z. B. die IP-Adresse für ausgehenden Datenverkehr eines Azure App Service ermitteln oder eine öffentliche IP-Adresse verwenden, die an einen virtuellen Computer oder eine andere Ressource gebunden ist (weitere Informationen zum Herstellen einer Verbindung mit der privaten IP-Adresse eines virtuellen Computers über Dienstendpunkte finden Sie unten). 

Wenn eine feste IP-Adresse für ausgehenden Datenverkehr für Ihren Azure-Dienst nicht verfügbar ist, können Sie die Aktivierung von Verbindungen von allen IP-Adressen im Azure-Rechenzentrum in Erwägung ziehen. Diese Einstellung kann über das Azure-Portal aktiviert werden. Legen Sie dazu die Option **Zugriff auf Azure-Dienste zulassen** auf **EIN** im Bereich **Verbindungssicherheit** fest, und klicken Sie dann auf **Speichern**. Aus der Azure CLI bewirkt eine Firewallregeleinstellung, bei der die Start- und Endadresse gleich 0.0.0.0 ist, das Gleiche. Ist der Verbindungsversuch nicht zulässig, erreicht die Anforderung den Azure Database for MySQL-Server nicht.

> [!IMPORTANT]
> Diese Option **Zugriff auf Azure-Dienste zulassen** konfiguriert die Firewall so, dass alle von Azure ausgehenden Verbindungen zugelassen werden (einschließlich Verbindungen von den Abonnements anderer Kunden). Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.
> 

:::image type="content" source="./media/concepts-firewall-rules/allow-azure-services.png" alt-text="Konfigurieren von „Zugriff auf Azure-Dienste erlauben“ im Portal":::

### <a name="connecting-from-a-vnet"></a>Herstellen einer Verbindung über ein VNet
Wenn Sie von einem VNet aus eine sichere Verbindung mit Ihrem Azure Database for MySQL-Server herstellen möchten, ziehen Sie die Verwendung von [VNet-Dienstendpunkten](./concepts-data-access-and-security-vnet.md) in Betracht. 

## <a name="programmatically-managing-firewall-rules"></a>Programmgesteuertes Verwalten von Firewallregeln
Außer im Azure-Portal können Firewallregeln programmgesteuert mithilfe der Azure CLI verwaltet werden. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md).

## <a name="troubleshooting-firewall-issues"></a>Behandeln von Firewallproblemen
Wenn der Zugriff auf den Microsoft Azure Database for MySQL-Serverdienst nicht das erwartete Verhalten aufweist, sind folgende Punkte zu beachten:

* **Änderungen an der Zulassungsliste sind noch nicht wirksam**: Änderungen der Firewallkonfiguration für den Azure Database for MySQL-Server werden möglicherweise erst nach fünf Minuten wirksam.

* **Die Anmeldung ist nicht autorisiert, oder ein falsches Kennwort wurde verwendet**: Wenn eine Anmeldung nicht über Berechtigungen für den Azure Database for MySQL-Server verfügt oder das verwendete Kennwort falsch ist, wird die Verbindung mit dem Azure Database for MySQL-Server verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, eine Verbindung mit dem Server herzustellen. Jeder Client muss die erforderlichen Sicherheitsanmeldeinformationen bereitstellen.

* **Dynamische IP-Adresse**: Wenn Sie über eine Internetverbindung mit dynamischer IP-Adresszuweisung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

   * Fragen Sie Ihren Internetdienstanbieter (ISP) nach dem IP-Adressbereich, der den Clientcomputern zugewiesen ist, mit denen auf den Azure-Datenbank für MySQL-Server zugegriffen wird, und fügen Sie dann den IP-Adressbereich als Firewallregel hinzu.

   * Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die IP-Adressen als Firewallregeln hinzu.

* **Die IP-Adresse des Servers scheint öffentlich zu sein:** Verbindungen mit dem Azure Database for MySQL-Server werden über ein öffentlich zugängliches Azure-Gateway weitergeleitet. Die tatsächliche Server-IP-Adresse wird jedoch durch die Firewall geschützt. Weitere Informationen finden Sie im Artikel zur [Verbindungsarchitektur](concepts-connectivity-architecture.md). 

* **Verbindungsherstellung von Azure-Ressource mit zulässiger IP-Adresse nicht möglich:** Überprüfen Sie, ob der Dienstendpunkt **Microsoft.Sql** für das Subnetz aktiviert ist, von dem aus Sie die Verbindung herstellen möchten. Wenn **Microsoft.Sql** aktiviert ist, ist dies ein Hinweis darauf, dass Sie in diesem Subnetz nur [VNET-Dienstendpunkt-Regeln](concepts-data-access-and-security-vnet.md) nutzen möchten.

   Beispielsweise wird ggf. der folgende Fehler angezeigt, wenn Sie eine Verbindung von einer Azure-VM in einem Subnetz herstellen, für das **Microsoft.Sql** aktiviert, aber keine entsprechende VNET-Regel vorhanden ist: `FATAL: Client from Azure Virtual Networks is not allowed to access the server`

* **Die Firewallregel ist für das IPv6-Format nicht verfügbar:** Firewallregeln müssen das IPv4-Format aufweisen. Wenn Sie Firewallregeln im IPv6-Format angeben, wird ein Validierungsfehler angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
* [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md)
* [VNET-Dienstendpunkte in Azure Database for MySQL](./concepts-data-access-and-security-vnet.md)
