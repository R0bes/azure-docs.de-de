---
title: Azure Backup-Unterstützungsmatrix für die SQL Server-Sicherung auf Azure-VMs
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für die Sicherung von SQL Server auf Azure-VMs mit dem Azure Backup-Dienst.
ms.topic: conceptual
ms.date: 06/07/2021
ms.custom: references_regions
ms.openlocfilehash: 678a3e63205d986681016fe64971e9bd874f9c71
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119926"
---
# <a name="support-matrix-for-sql-server-backup-in-azure-vms"></a>Unterstützungsmatrix für die SQL Server-Sicherung auf Azure-VMs

Mit Azure Backup können Sie SQL Server-Datenbanken auf Azure-VMs sichern, die auf der Microsoft Azure-Cloudplattform gehostet werden. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Unterstützungseinstellungen und Einschränkungen für Szenarien und Bereitstellungen der SQL Server-Sicherung auf Azure-VMs.

## <a name="scenario-support"></a>Unterstützung von Szenarien

**Unterstützung** | **Details**
--- | ---
**Unterstützte Bereitstellungen** | SQL-Marketplace-Azure-VMs und Nicht-Marketplace-VMs (manuelle SQL Server-Installation) werden unterstützt.
**Unterstützte Regionen** | Azure Backup für SQL Server-Datenbanken ist in allen Regionen verfügbar, mit Ausnahme von „Frankreich, Süden“ (FRS), „Vereinigtes Königreich, Norden“ (UKN), „Vereinigtes Königreich, Süden 2“ (UKS2), „UG IOWA“ (UGI) und „Deutschland (Black Forest)“.
**Unterstützte Betriebssysteme** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux wird derzeit nicht unterstützt.
**Unterstützte SQL Server-Versionen** | SQL Server 2019, SQL Server 2017 wie auf der Seite [Lebenszyklus für Produkt suchen](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017) beschrieben, SQL Server 2016 und SPs wie auf der Seite [Lebenszyklus für Produkt suchen](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack) beschrieben, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, Developer, Express<br><br>Lokale Express-Datenbankversionen werden nicht unterstützt.
**Unterstützte .NET-Versionen** | .NET Framework 4.5.2 oder höher auf dem virtuellen Computer installiert

## <a name="feature-considerations-and-limitations"></a>Überlegungen und Einschränkungen in Bezug auf Features

|Einstellung  |Maximales Limit |
|---------|---------|
|Anzahl von Datenbanken, die auf einem Server (und in einem Tresor) geschützt werden können    |   2000      |
|Unterstützte Datenbankgröße (bei größeren Datenbanken können Leistungsprobleme auftreten)   |   6 TB*      |
|Anzahl von in einer Datenbank unterstützten Dateien    |   1000      |

_*Der Grenzwert für die Datenbankgröße hängt von der unterstützten Datenübertragungsrate und der Konfiguration des Sicherungszeitlimits ab. Dies ist nicht die harte Grenze. [Erfahren Sie mehr](#backup-throughput-performance) über die Leistung des Sicherungsdurchsatzes._

* SQL Server-Sicherung kann im Azure-Portal oder mit **PowerShell** konfiguriert werden. Die Befehlszeilenschnittstelle wird nicht unterstützt.
* Die Lösung wird für beide Arten von [Bereitstellungen](../azure-resource-manager/management/deployment-models.md) unterstützt: Azure Resource Manager-VMs und klassische VMs.
* Alle Sicherungstypen (vollständig/differenziell/Protokoll) und Wiederherstellungsmodelle (einfach/vollständig/massenprotokolliert) werden unterstützt.
* Bei **schreibgeschützten** Datenbanken: Es werden nur die Sicherungstypen „Vollständig“ und „Nur vollständig kopieren“ unterstützt.
* Die native SQL-Komprimierung wird unterstützt, wenn sie vom Benutzer explizit in der Sicherungsrichtlinie aktiviert wurde. Je nachdem, welcher Wert vom Benutzer für dieses Steuerelement festgelegt wurde, überschreibt Azure Backup die Standardwerte auf Instanzebene mit der COMPRESSION- bzw. NO_COMPRESSION-Klausel.
* Für TDE aktivierte Datenbanksicherungen werden unterstützt. Wenn Sie eine mit TDE verschlüsselte Datenbank in einer anderen SQL Server-Instanz wiederherstellen möchten, müssen Sie zuerst [das Zertifikat auf dem Zielserver wiederherstellen](/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server). Eine Komprimierung der Sicherungen für TDE-fähige Datenbanken für SQL Server 2016 und höhere Versionen ist verfügbar, allerdings für geringere Übertragungsgrößen, wie [hier](https://techcommunity.microsoft.com/t5/sql-server/backup-compression-for-tde-enabled-databases-important-fixes-in/ba-p/385593) erläutert.
* Sicherungs- und Wiederherstellungsvorgänge für Spiegeldatenbanken und Datenbankmomentaufnahmen werden nicht unterstützt.
* Eine SQL Server-**Failoverclusterinstanz (FCI)** wird nicht unterstützt.
* Die Verwendung mehrerer Sicherungslösungen zum Sichern Ihrer eigenständigen SQL Server-Instanz oder SQL-Always On-Verfügbarkeitsgruppe kann zu Fehlern bei der Sicherung führen. Es ist daher ratsam, von dieser Vorgehensweise abzusehen. Das Sichern von zwei Knoten einer Verfügbarkeitsgruppe mit derselben Lösung oder anderen Lösungen kann auch zu Fehlern bei der Sicherung führen.
* Wenn Verfügbarkeitsgruppen konfiguriert sind, beruht die Erstellung von Sicherungen der verschiedenen Knoten auf mehreren Faktoren. Das Sicherungsverhalten für eine Verfügbarkeitsgruppe ist unten zusammengefasst.

### <a name="back-up-behavior-with-always-on-availability-groups"></a>Sicherungsverhalten bei Verwendung von Always On-Verfügbarkeitsgruppen

Wir empfehlen, die Sicherung nur auf einem Knoten der Verfügbarkeitsgruppe zu konfigurieren. Konfigurieren Sie die Sicherung immer in derselben Region wie den primären Knoten. Anders ausgedrückt: Der primäre Knoten muss sich immer in der Region befinden, in der Sie die Sicherung konfigurieren. Wenn sich alle Knoten der Verfügbarkeitsgruppe in der Region befinden, in der die Sicherung konfiguriert ist, ist alles in Ordnung.

#### <a name="for-cross-region-ag"></a>Für regionsübergreifende Verfügbarkeitsgruppe

* Sicherungen werden unabhängig von der Sicherungseinstellung nur für die Knoten ausgeführt, die sich in der Region befinden, in der die Sicherung konfiguriert ist. Dies liegt daran, dass regionsübergreifende Sicherungen nicht unterstützt werden. Wenn Sie nur zwei Knoten haben und sich der sekundäre Knoten in der anderen Region befindet, werden die Sicherungen weiterhin über den primären Knoten ausgeführt (sofern Ihre Sicherungseinstellung nicht „Nur sekundär“ lautet).
* Wenn für einen Knoten ein Failover auf eine Region erfolgt, bei der es sich nicht um die Region handelt, in der die Sicherung konfiguriert ist, schlagen Sicherungen auf den Knoten in der Failoverregion fehl.

Abhängig von der Sicherungseinstellung und den Sicherungstypen (Vollständig/Differenziell/Protokoll/Nur vollständig kopieren) werden die Sicherungen von einem bestimmten Knoten (Primär/Sekundär) erstellt.

#### <a name="backup-preference-primary"></a>Sicherungseinstellung: Primär

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Primär
Nur vollständig kopieren |  Primär

#### <a name="backup-preference-secondary-only"></a>Sicherungseinstellung: Nur sekundär

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

#### <a name="backup-preference-secondary"></a>Sicherungseinstellung: Secondary

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

#### <a name="no-backup-preference"></a>Keine Sicherungseinstellung

**Sicherungstyp** | **Node**
--- | ---
Vollständig | Primär
Differenziell | Primär
Log |  Secondary
Nur vollständig kopieren |  Secondary

## <a name="backup-throughput-performance"></a>Durchsatzleistung bei der Sicherung

Azure Backup unterstützt eine konsistente Datenübertragungsrate von 200 MBit/s für vollständige und differenzielle Sicherungen großer SQL-Datenbanken (von 500 GB). Um die optimale Leistung zu nutzen, stellen Sie Folgendes sicher:

- Die zugrunde liegende VM (mit der SQL Server-Instanz, die die Datenbank hostet) ist mit dem erforderlichen Netzwerkdurchsatz konfiguriert. Wenn der maximale Durchsatz der VM weniger als 200 MBit/s beträgt, kann Azure Backup Daten nicht mit der optimalen Geschwindigkeit übertragen.<br>Außerdem muss für den Datenträger, der die Datenbankdateien enthält, ausreichend Durchsatz bereitgestellt werden. [Erfahren Sie mehr](../virtual-machines/disks-performance.md) über Datenträgerdurchsatz und -leistung auf Azure-VMs. 
- Prozesse, die auf der VM ausgeführt werden, verbrauchen keine VM-Bandbreite. 
- Die Sicherungszeitpläne sind auf eine Teilmenge von Datenbanken verteilt. Mehrere Sicherungen, die gleichzeitig auf einer VM ausgeführt werden, teilen sich die Netzwerknutzungsrate zwischen den Sicherungen. [Erfahren Sie mehr](faq-backup-sql-server.yml#can-i-control-how-many-concurrent-backups-run-on-the-sql-server-) über das Steuern der Anzahl gleichzeitiger Sicherungen.

>[!NOTE]
> [Laden Sie den detaillierten Ressourcenplaner herunter](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx). Mit diesem können Sie die ungefähre Anzahl von geschützten Datenbanken berechnen, die basierend auf den VM-Ressourcen, der Bandbreite und der Sicherungsrichtlinie pro Server empfohlen werden.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie eine [SQL Server-Datenbank sichern](backup-azure-sql-database.md), die auf einer Azure-VM ausgeführt wird.