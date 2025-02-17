---
title: Herstellen einer Verbindung mit und Abfragen von Synapse SQL mit Visual Studio und SSDT
description: Verwenden Sie Visual Studio zur Abfrage des dedizierten SQL-Pools mithilfe von Azure Synapse Analytics.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 7d534794737f93cd71ee9820f72e6c5ead08ca2b
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349904"
---
# <a name="connect-to-synapse-sql-with-visual-studio-and-ssdt"></a>Herstellen einer Verbindung mit Synapse SQL mit Visual Studio und SSDT

> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](get-started-visual-studio.md)
> * [sqlcmd](get-started-connect-sqlcmd.md) 
> * [SSMS](get-started-ssms.md)
> 
> 

Verwenden Sie Visual Studio zur Abfrage des dedizierten SQL-Pools mithilfe von Azure Synapse Analytics. Bei dieser Methode wird die SSDT-Erweiterung (SQL Server Data Tools) in Visual Studio 2019 verwendet. 

> [!NOTE]
> Serverloser SQL-Pool wird von SSDT nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen die folgenden Komponenten, um dieses Tutorial verwenden zu können:

- Einen vorhandenen Synapse-Arbeitsbereich. Falls Sie einen Arbeitsbereich erstellen müssen, finden Sie weitere Informationen unter [Erstellen eines Synapse-Arbeitsbereichs](../get-started-create-workspace.md).
- Einen dedizierten SQL-Pool. Falls ein solcher nicht vorhanden ist, finden Sie Informationen zu dessen Erstellung unter [Erstellen eines dedizierten SQL-Pools](../get-started-analyze-sql-pool.md#create-a-dedicated-sql-pool).
- SSDT für Visual Studio. Wenn Sie über Visual Studio verfügen, ist diese Komponente darin wahrscheinlich bereits enthalten. Installationshinweise und -optionen finden Sie unter [Installieren von Visual Studio und SSDT](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md?context=/azure/synapse-analytics/context/context).
- Den vollqualifizierten SQL-Servernamen. Informationen zum Ermitteln dieses Servernamens finden Sie unter [Herstellen einer Verbindung mit einem dedizierten SQL-Pool ](connect-overview.md).

## <a name="1-connect-to-a-dedicated-sql-pool"></a>1. Herstellen einer Verbindung mit einem dedizierten SQL-Pool
1. Öffnen Sie Visual Studio 2019.
2. Öffnen Sie den SQL Server-Objekt-Explorer durch Klicken auf **Anzeigen** > **SQL Server-Objekt-Explorer**.
   
    ![SQL Server-Objekt-Explorer](./media/get-started-visual-studio/open-ssdt.png)
3. Klicken Sie auf das Symbol **SQL Server hinzufügen** .
   
    ![SQL Server hinzufügen](./media/get-started-visual-studio/add-server.png)
4. Füllen Sie die Felder im Fenster zum Herstellen einer Verbindung mit dem Server aus.
   
    ![Verbindung mit Server herstellen](./media/get-started-visual-studio/connection-dialog.png)
   
   * **Servername**: Geben Sie den zuvor ermittelten **Servernamen** ein.
   * **Authentifizierung:** Wählen Sie **SQL Server-Authentifizierung** oder **Integrierte Active Directory-Authentifizierung**:
   * **Benutzername** und **Kennwort**: Geben Sie Benutzername und Kennwort ein, wenn Sie oben „SQL Server-Authentifizierung“ ausgewählt haben.
   * Klicken Sie auf **Verbinden**.
5. Erweitern Sie den Azure SQL-Server. Sie können die dem Server zugeordneten Datenbanken anzeigen. Erweitern Sie „AdventureWorksDW“, um die Tabellen in Ihrer Beispieldatenbank anzuzeigen.
   
    ![AdventureWorksDW erkunden](./media/get-started-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. Ausführen einer Beispielabfrage
Nachdem jetzt eine Verbindung mit Ihrer Datenbank hergestellt wurde, schreiben Sie eine Abfrage.

1. Klicken Sie mit der rechten Maustaste im SQL Server-Objekt-Explorer auf Ihre Datenbank.
2. Wählen Sie **Neue Abfrage** aus. Ein neues Abfragefenster wird geöffnet.
   
    ![Neue Abfrage](./media/get-started-visual-studio/new-query2.png)
3. Kopieren Sie die folgende TSQL-Abfrage in das Abfragefenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Führen Sie die Abfrage aus, indem Sie auf den grünen Pfeil klicken oder die folgende Tastenkombination drücken: `CTRL`+`SHIFT`+`E`.
   
    ![Abfrage ausführen](./media/get-started-visual-studio/run-query.png)
5. Sehen Sie sich die Abfrageergebnisse an. In diesem Beispiel weist die Tabelle „FactInternetSales“ 60398 Zeilen auf.
   
    ![Abfrageergebnisse](./media/get-started-visual-studio/query-results.png)

## <a name="next-steps"></a>Nächste Schritte
Nun da Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [die Daten mit Power BI zu visualisieren](get-started-power-bi-professional.md).
Informationen zum Konfigurieren Ihrer Umgebung für die Azure Active Directory-Authentifizierung finden Sie unter [Authentifizieren beim dedizierten SQL-Pool](sql-authentication.md?tabs=provisioned).
 