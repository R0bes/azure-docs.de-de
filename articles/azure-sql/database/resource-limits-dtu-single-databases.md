---
title: Einzeldatenbanken mit DTU-Ressourcenlimits
description: Diese Seite beschreibt einige allgemeine DTU-Ressourcenlimits für Einzeldatenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: service-overview
ms.custom: references_regions, seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: reference
author: dimitri-furman
ms.author: dfurman
ms.reviewer: mathoma
ms.date: 04/16/2021
ms.openlocfilehash: aecf872bcac77c94090d374cc18415eba6323b61
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110689815"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-purchasing-model---azure-sql-database"></a>Ressourcengrenzwerte für Einzeldatenbanken, die das DTU-Kaufmodell verwenden: Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Einzeldatenbanken in Azure SQL-Datenbank, die das DTU-Kaufmodell verwenden.

* Weitere Informationen zu Ressourcenlimits für Singletons auf einem Server mit dem DTU-basierten Kaufmodell finden Sie unter [Übersicht über Ressourcenlimits auf einem Server](resource-limits-logical-server.md).
* Informationen zu Ressourcenlimits des DTU-Kaufmodells für Azure SQL-Datenbank finden Sie unter [DTU-Ressourcenlimits für einzelne Datenbanken (Singletons)](resource-limits-dtu-single-databases.md) und [DTU-Ressourcenlimits für Pools für elastische Datenbanken](resource-limits-dtu-elastic-pools.md).
* Informationen zu V-Kern-Ressourcenlimits finden Sie unter [V-Kern-Ressourcenlimits – Azure SQL-Datenbank](resource-limits-vcore-single-databases.md) und [V-Kern-Ressourcenlimits – Pools für elastische Datenbanken](resource-limits-vcore-elastic-pools.md).
* Weitere Informationen zu den verschiedenen Kaufmodellen finden Sie unter [Kaufmodelle und Dienstebenen](purchasing-models.md).

Jedes schreibgeschützte Replikat hat eigene Ressourcen wie DTUs, Worker und Sitzungen. Jedes schreibgeschützte Replikat unterliegt den Ressourcenlimits, die weiter unten in diesem Artikel beschrieben werden. 


## <a name="single-database-storage-sizes-and-compute-sizes"></a>Einzeldatenbank: Speicher- und Computegrößen

Die folgende Tabelle enthält die verfügbaren Ressourcen für Einzeldatenbanken für alle Dienstebenen und Computegrößen. Sie können die Dienstebene, Computegröße und Speichermenge für eine Einzeldatenbank mit folgenden Optionen festlegen:

* [Transact-SQL](single-database-manage.md#transact-sql-t-sql) über [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql#overview-sql-database)
* [Azure portal](single-database-manage.md#the-azure-portal)
* [PowerShell](single-database-manage.md#powershell)
* [Azure-Befehlszeilenschnittstelle](single-database-manage.md#the-azure-cli)
* [REST-API](single-database-manage.md#rest-api)

> [!IMPORTANT]
> Anleitungen und Überlegungen zur Skalierung finden Sie unter [Skalieren einer Einzeldatenbank](single-database-scale.md).

### <a name="basic-service-tier"></a>Basic-Dienstebene

| **Computegröße** | **Grundlegend** |
| :--- | --: |
| Max. DTU-Anzahl | 5 |
| Inbegriffener Speicher (GB) | 2 |
| Max. Speicher (GB) | 2 |
| Max. In-Memory-OLTP-Speicher (GB) |– |
| Max. gleichzeitige Worker (Anforderungen) | 30 |
| Max. gleichzeitige Sitzungen | 300 |
|||

> [!IMPORTANT]
> Die Dienstebene „Basic“ bietet weniger als einen virtuellen Kern (CPU).  Für CPU-intensive Workloads wird eine Dienstebene von S3 oder höher empfohlen.
>
>Im Hinblick auf die Datenspeicherung wird die Dienstebene „Basic“ in Standard-Seitenblobs platziert. Standard-Seitenblobs arbeiten mit HDD-basierten Speichermedien und eignen sich hervorragend für Entwicklungs- und Testaufgaben sowie andere weniger häufig anfallende Workloads, bei denen Leistungsschwankungen keine große Rolle spielen.
>

### <a name="standard-service-tier"></a>Standard-Dienstebene

| **Computegröße** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Max. DTU-Anzahl | 10 | 20 | 50 | 100 |
| Inbegriffener Speicher (GB) <sup>1</sup> | 250 | 250 | 250 | 250 |
| Max. Speicher (GB) | 250 | 250 | 250 | 1024 |
| Max. In-Memory-OLTP-Speicher (GB) | – | – | – | – |
| Max. gleichzeitige Worker (Anforderungen)| 60 | 90 | 120 | 200 |
| Max. gleichzeitige Sitzungen |600 | 900 | 1200 | 2400 |
||||||

<sup>1</sup> Unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/) finden Sie weitere Informationen zu zusätzlichen Kosten, die durch den zusätzlich bereitgestellten Speicherplatz entstehen.

> [!IMPORTANT]
> Die „Standard S0“, „S1“ und „S2“ bieten weniger als einen virtuellen Kern (CPU).  Für CPU-intensive Workloads wird eine Dienstebene von S3 oder höher empfohlen.
>
>Im Hinblick auf die Datenspeicherung werden die Dienstebenen „Standard S0“ und „S1“ in Standard-Seitenblobs platziert. Standard-Seitenblobs arbeiten mit HDD-basierten Speichermedien und eignen sich hervorragend für Entwicklungs- und Testaufgaben sowie andere weniger häufig anfallende Workloads, bei denen Leistungsschwankungen keine große Rolle spielen.
>

### <a name="standard-service-tier-continued"></a>Standard-Dienstebene (Fortsetzung)

| **Computegröße** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Max. DTU-Anzahl | 200 | 400 | 800 | 1600 | 3000 |
| Inbegriffener Speicher (GB) <sup>1</sup> | 250 | 250 | 250 | 250 | 250 |
| Max. Speicher (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
| Max. In-Memory-OLTP-Speicher (GB) | – | – | – | – |– |
| Max. gleichzeitige Worker (Anforderungen)| 400 | 800 | 1600 | 3200 |6000 |
| Max. gleichzeitige Sitzungen |4800 | 9600 | 19200 | 30.000 |30.000 |
|||||||

<sup>1</sup> Unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/) finden Sie weitere Informationen zu zusätzlichen Kosten, die durch den zusätzlich bereitgestellten Speicherplatz entstehen.

### <a name="premium-service-tier"></a>Premium-Dienstebene

| **Computegröße** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Inbegriffener Speicher (GB) <sup>1</sup> | 500 | 500 | 500 | 500 | 4\.096 <sup>2</sup> | 4\.096 <sup>2</sup> |
| Max. Speicher (GB) | 1024 | 1024 | 1024 | 1024 | 4\.096 <sup>2</sup> | 4\.096 <sup>2</sup> |
| Max. In-Memory-OLTP-Speicher (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Max. gleichzeitige Worker (Anforderungen)| 200 | 400 | 800 | 1600 | 2800 | 6400 |
| Max. gleichzeitige Sitzungen | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||

<sup>1</sup> Unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/) finden Sie weitere Informationen zu zusätzlichen Kosten, die durch den zusätzlich bereitgestellten Speicherplatz entstehen.

<sup>2</sup> Von 1.024 GB bis zu 4.096 GB in Inkrementen von 256 GB.

> [!IMPORTANT]
> In allen Regionen außer den folgenden ist im Premium-Tarif derzeit mehr als 1 TB Speicher verfügbar: China, Osten; China, Norden; Deutschland, Mitte; Deutschland, Nordosten. In diesen Regionen ist der Speicher im Tarif „Premium“ auf 1 TB begrenzt.  Weitere Informationen finden Sie unter [Einschränkungen von P11 und P15](single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).

> [!NOTE]
> Weitere Informationen zu `tempdb`-Einschränkungen finden Sie unter [tempdb-Grenzwerte](/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
> 
> Weitere Informationen zu Speichergrenzwerten der Dienstebene „Premium“ finden Sie unter [Speicherplatzgovernance](resource-limits-logical-server.md#storage-space-governance).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu V-Kern-Ressourcenlimits für eine Einzeldatenbank finden Sie unter [Ressourcenlimits für Einzeldatenbanken, die das V-Kern-Kaufmodell verwenden](resource-limits-vcore-single-databases.md).
- Informationen zu V-Kern-Ressourcenlimits für Pools für elastische Datenbanken finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das V-Kern-Kaufmodell verwenden](resource-limits-vcore-elastic-pools.md).
- Informationen zu DTU-Ressourcenlimits für Pools für elastische Datenbanken finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das DTU-Kaufmodell verwenden](resource-limits-dtu-elastic-pools.md).
- Informationen zu den Ressourcengrenzwerten für verwaltete Instanzen in Azure SQL Managed Instance finden Sie unter [Ressourcengrenzwerte für SQL Managed Instance](../managed-instance/resource-limits.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Informationen zu Ressourcengrenzwerten auf Server- und Abonnementebene auf einem logischen SQL-Server finden Sie unter [Übersicht über Ressourcenlimits für einen logischen SQL-Server](resource-limits-logical-server.md).