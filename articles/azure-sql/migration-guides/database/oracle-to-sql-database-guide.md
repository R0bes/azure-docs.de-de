---
title: 'Oracle zu Azure SQL-Datenbank: Migrationsleitfaden'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihr Oracle-Schema über SQL Server Migration Assistant für Oracle zu Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: cawrites
ms.author: chadam
ms.date: 08/25/2020
ms.openlocfilehash: 0a6f7bca2e6a3a1f0ac10f868ddeffe72d082c28
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355780"
---
# <a name="migration-guide-oracle-to-azure-sql-database"></a>Migrationsleitfaden: Oracle zu Azure SQL-Datenbank

[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre Oracle-Schemas mithilfe von [SQL Server Migration Assistant](https://azure.microsoft.com/migration/sql-server/) für Oracle (SSMA für Oracle) zu Azure SQL-Datenbank [migrieren](https://azure.microsoft.com/migration/migration-journey) können.

Weitere Migrationsleitfäden finden Sie in den [Leitfäden zur Azure-Datenbankmigration](/data-migration).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Migration Ihres Oracle-Schemas zu SQL-Datenbank beginnen:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird.
- Laden Sie [SSMA für Oracle](https://www.microsoft.com/download/details.aspx?id=54258) herunter.
- Sie müssen über eine [SQL-Datenbank](../../database/single-database-create-quickstart.md)-Zielinstanz verfügen.
- Fordern Sie die [erforderlichen Berechtigungen für SSMA für Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) und den [Anbieter](/sql/ssma/oracle/connect-to-oracle-oracletosql) an.

## <a name="pre-migration"></a>Vor der Migration

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer [Azure-Cloudmigration](https://azure.microsoft.com/migration) bewerten. Diese Schritte umfassen eine Bestandsaufnahme der Datenbanken, die migriert werden müssen, eine Untersuchung dieser Datenbanken auf potenzielle Migrationsprobleme oder Hindernisse sowie das Behandeln eventuell entdeckter Probleme.

### <a name="assess"></a>Bewerten

Mithilfe von SSMA für Oracle können Sie Datenbankobjekte und Daten überprüfen, Datenbanken für die Migration bewerten, Datenbankobjekte zu SQL-Datenbank migrieren und schließlich die Daten zur Datenbank migrieren.

So erstellen Sie eine Bewertung

1. Öffnen Sie [SSMA für Oracle](https://www.microsoft.com/download/details.aspx?id=54258).
1. Wählen Sie **Datei** und dann **Neues Projekt** aus.
1. Geben Sie den Projektnamen und einen Speicherort für Ihr Projekt ein. Wählen Sie dann in der Dropdown Liste die Option **Azure SQL-Datenbank** als Migrationsziel aus, und klicken Sie auf **OK**.

   ![Screenshot von „Verbindung mit Oracle herstellen“](./media/oracle-to-sql-database-guide/connect-to-oracle.png)

1. Wählen Sie **Verbindung mit Oracle herstellen** aus. Geben Sie im Dialogfeld **Verbindung mit Oracle herstellen** Werte für die Oracle-Verbindungsdetails ein.

1. Wählen Sie die Oracle-Schemas aus, die migriert werden sollen.

   ![Screenshot von der Auswahl des Oracle-Schemas](./media/oracle-to-sql-database-guide/select-schema.png)

1. Klicken Sie im **Metadaten-Explorer von Oracle** mit der rechten Maustaste auf das zu migrierende Oracle-Schema, und wählen Sie dann **Bericht erstellen** aus, um einen HTML-Bericht zu generieren. Stattdessen können Sie eine Datenbank auswählen und dann auf die Registerkarte **Bericht erstellen** klicken.

   ![Screenshot von „Bericht erstellen“](./media/oracle-to-sql-database-guide/create-report.png)

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um ein Inventar der Oracle-Objekte sowie Informationen zum Aufwand für Schemakonvertierungen zu erhalten. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects.

   Ein Beispiel finden Sie unter `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2020_11_12T02_47_55\`.

   ![Screenshot von Bewertungsbericht](./media/oracle-to-sql-database-guide/assessment-report.png)

### <a name="validate-the-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie sie bei Bedarf basierend auf den Anforderungen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie in SSMA für Oracle die Option **Extras** aus, und klicken Sie dann auf **Projekteinstellungen**.
1. Wählen Sie die Registerkarte **Typzuordnung** aus.

   ![Screenshot von „Typzuordnungen“](./media/oracle-to-sql-database-guide/type-mappings.png)

1. Sie können die Typzuordnung für jede Tabelle ändern, indem Sie die Tabelle im **Metadaten-Explorer von Oracle** auswählen.

### <a name="convert-the-schema"></a>Konvertieren des Schemas

So konvertieren Sie das Schema

1. (Optional:) Fügen Sie Anweisungen dynamische Abfragen oder Ad-hoc-Abfragen hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten, und klicken Sie dann auf **Anweisungen hinzufügen**.
1. Klicken Sie auf **Verbindung mit Azure SQL-Datenbank herstellen**.
    1. Geben Sie unter **SQL-Datenbank** Verbindungsdetails zum Verbinden Ihrer Datenbank ein.
    1. Wählen Sie die SQL-Datenbank-Zielinstanz in der Dropdownliste aus, oder geben Sie einen neuen Namen ein. In diesem Fall wird eine Datenbank auf dem Zielserver erstellt.
    1. Geben Sie die Authentifizierungsdetails ein, und klicken Sie auf **Verbinden**.

    ![Screenshot von „Verbindung mit Azure SQL-Datenbank herstellen“](./media/oracle-to-sql-database-guide/connect-to-sql-database.png)

1. Klicken Sie im **Metadaten-Explorer von Oracle** mit der rechten Maustaste auf das Oracle-Schema, und klicken Sie dann auf **Schema konvertieren**. Oder Sie können Ihr Schema auswählen und dann auf die Registerkarte **Schema konvertieren** klicken.

   ![Screenshot von „Schema konvertieren“](./media/oracle-to-sql-database-guide/convert-schema.png)

1. Vergleichen und überprüfen Sie nach Abschluss der Konvertierung die konvertierten Objekte mit den Ursprungsobjekten, um potenzielle Probleme zu identifizieren, und behandeln Sie diese anhand der Empfehlungen.

   ![Screenshot von dem Schema zum Überprüfen von Empfehlungen](./media/oracle-to-sql-database-guide/table-mapping.png)

1. Vergleichen Sie den konvertierten Transact-SQL-Text mit den ursprünglichen gespeicherten Prozeduren, und überprüfen Sie die Empfehlungen.

   ![Screenshot von zu überprüfenden Empfehlungen](./media/oracle-to-sql-database-guide/procedure-comparison.png)

1. Wählen Sie im Ausgabebereich **Ergebnisse überprüfen** aus, und überprüfen Sie die Fehler im Bereich **Fehlerliste**.
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie im Menü **Datei** die Option **Projekt speichern** aus. In diesem Schritt können Sie das Quell- und das Zielschema offline auswerten und Probleme behandeln, bevor Sie das Schema in SQL-Datenbank veröffentlichen.

## <a name="migrate"></a>Migrieren

Nachdem Sie Ihre Datenbanken ausgewertet und Abweichungen gelöst haben, führen Sie als nächsten Schritt den Migrationsprozess aus. Die Migration umfasst zwei Schritte: das Veröffentlichen des Schemas und das Migrieren der Daten.

So veröffentlichen Sie das Schema und migrieren Ihre Daten

1. Veröffentlichen Sie das Schema, indem Sie im **Metadaten-Explorer von Azure SQL-Datenbank** im Knoten **Datenbanken** mit der rechten Maustaste auf die Datenbank klicken und **Mit Datenbank synchronisieren** auswählen.

   ![Screenshot von „Mit Datenbank synchronisieren“](./media/oracle-to-sql-database-guide/synchronize-with-database.png)

1. Überprüfen Sie die Zuordnung zwischen dem Quellprojekt und dem Ziel.

   ![Screenshot von „Mit Datenbank synchronisieren“ zur Überprüfung](./media/oracle-to-sql-database-guide/synchronize-with-database-review.png)

1. Migrieren Sie die Daten, indem Sie im **Metadaten-Explorer für Oracle** mit der rechten Maustaste auf die zu migrierende Datenbank oder das Objekt klicken und **Daten migrieren** auswählen. Oder Sie können die Registerkarte **Daten migrieren** auswählen. Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um Daten für eine gesamte Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der gewünschten Tabelle. Deaktivieren Sie die Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

   ![Screenshot von „Daten migrieren“](./media/oracle-to-sql-database-guide/migrate-data.png)

1. Geben Sie die Verbindungsdetails für die Oracle- und die SQL-Datenbank an.
1. Überprüfen Sie nach Abschluss der Migration den **Bericht zur Datenmigration**.

   ![Screenshot: Bericht zur Datenmigration](./media/oracle-to-sql-database-guide/data-migration-report.png)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) eine Verbindung mit Ihrer SQL-Datenbank-Instanz her, und überprüfen Sie die Migration durch Überprüfen der Daten und des Schemas.

   ![Screenshot der Überprüfung in SQL Server Management Studio](./media/oracle-to-sql-database-guide/validate-data.png)

Oder können Sie die Migration auch über SQL Server Integration Services durchführen. Weitere Informationen finden Sie unter:

- [Erste Schritte mit SQL Server Integration Services](/sql/integration-services/sql-server-integration-services)
- [SQL Server Integration Services für Azure und die Hybriddatenverschiebung](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx)

## <a name="post-migration"></a>Nach der Migration

Nach erfolgreichem Abschluss der *Migrationsphase* müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

Das [Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) ist eine Erweiterung für Visual Studio Code, mit der Sie Ihren Java-Quellcode analysieren und API-Aufrufe und Abfragen mit Datenzugriff erkennen können. Das Toolkit zeigt auf einen Blick, was für die Unterstützung des neuen Datenbank-Back-Ends erforderlich ist. Weitere Informationen finden Sie im Blogbeitrag [Migrieren Ihrer Java-Anwendungen von Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727).

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
1. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
1. **Führen Sie die Validierungstests aus**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.
1. **Ausführen von Leistungstests:** Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

### <a name="validate-migrated-objects"></a>Überprüfen migrierter Objekte

Microsoft SQL Server Migration Assistant for Oracle Tester (SSMA-Tester) ermöglicht Ihnen das Testen migrierter Datenbankobjekte. Der SSMA-Tester wird verwendet, um zu überprüfen, ob sich konvertierte Objekte auf die gleiche Weise verhalten.

#### <a name="create-test-case"></a>Testfall erstellen

1. Öffnen Sie SSMA für Oracle, und wählen Sie erst **Tester** und dann **Neuer Testfall** aus.
   ![Screenshot: Erstellen eines neuen Testfalls.](./media/oracle-to-sql-database-guide/ssma-tester-new.png)

1. Geben Sie die folgenden Informationen für den neuen Testfall an:

   **Name:** Geben Sie den Namen ein, um den Testfall zu bestimmen.

   **Erstellungsdatum:** Das Datum des heutigen Tages, das automatisch bestimmt wird.

   **Datum der letzten Änderung:** Wird automatisch ausgefüllt und darf nicht geändert werden.

   **Beschreibung:** Geben Sie zusätzliche Informationen ein, um den Zweck des Testfalls zu bestimmen.

   ![Screenshot: Schritte zum Initialisieren eines Testfalls.](./media/oracle-to-sql-database-guide/tester-init-test-case.png)

1. Wählen Sie in der Oracle-Objektstruktur auf der linken Seite die Objekte aus, die zum Testfall gehören.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-select-configure-objects.png" alt-text="Screenshot: Schritt zum Auswählen und Konfigurieren des Objekts.":::

   In diesem Beispiel sind die gespeicherte Prozedur `ADD_REGION` und Tabelle `REGION` ausgewählt.

     Weitere Informationen finden Sie unter [Auswählen und Konfigurieren von zu testenden Objekten](/sql/ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql).

1. Wählen Sie als Nächstes im linken Fenster in der Oracle-Objektstruktur die Tabellen, Fremdschlüssel und andere abhängige Objekte aus.

   :::image type="content" source="./media//oracle-to-sql-database-guide/tester-select-configure-affected.png" alt-text="Screenshot: Schritt zum Auswählen und Konfigurieren des betroffenen Objekts.":::

   Weitere Informationen finden Sie unter [Auswählen und Konfigurieren von betroffenen Objekten](/sql/ssma/oracle/selecting-and-configuring-affected-objects-oracletosql).

1. Überprüfen Sie die Auswertungssequenz von Objekten. Ändern Sie die Reihenfolge, indem Sie im Raster auf die Schaltflächen klicken.

   :::image type="content" source="./media/oracle-to-sql-database-guide/test-call-ordering.png" alt-text="Screenshot, der die einzelnen Schritte der Testobjektausführung zeigt.":::

1. Schließen Sie den Testfall ab, indem Sie die in den vorherigen Schritten bereitgestellten Informationen überprüfen. Konfigurieren Sie die Testausführungsoptionen basierend auf dem Testszenario.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-finalize-case.png" alt-text="Screenshot: Schritt zum Abschließen des Objekts.":::

   Weitere Informationen zu Testfalleinstellungen finden Sie unter [Beenden der Vorbereitung von Testfällen](/sql/ssma/oracle/finishing-test-case-preparation-oracletosql).

1. Klicken Sie auf „Fertig stellen“, um den Testfall zu erstellen.

   :::image type="content" source="./media//oracle-to-sql-database-guide/tester-test-repo.png" alt-text="Screenshot: Schritt zum Testen des Repositorys.":::

#### <a name="run-test-case"></a>Ausführen des Testfalls

Wenn der SSMA-Tester einen Testfall ausführt, führt die Test-Engine die zu Testzwecken ausgewählten Objekte aus und generiert einen Überprüfungsbericht.

1. Wählen Sie im Testrepository den Testfall aus, und klicken Sie dann auf „Ausführen“.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-repo-run.png" alt-text="Screenshot: Überprüfen des Testrepositorys.":::

1. Überprüfen Sie den Starttestfall, und klicken Sie auf „Ausführen“.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-run-test-case.png" alt-text="Screenshot: Schritt zum Ausführen des Testfalls.":::

1. Geben Sie als Nächstes Anmeldeinformationen für die Oracle-Quelle an. Klicken Sie nach Eingabe der Anmeldeinformationen auf „Verbinden“.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-oracle-connect.png" alt-text="Screenshot: Schritt zum Herstellen einer Verbindung mit der Oracle-Quelle":::

1. Geben Sie die SQL Server-Anmeldeinformationen an, und klicken Sie auf „Verbinden“.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-sql-connect.png" alt-text="Screenshot: Schritt zum Herstellen einer Verbindung mit dem SQL Server-Ziel.":::

   Bei Erfolg wechselt der Testfall in die Initialisierungsphase.

1. Eine Statusleiste in Echtzeit zeigt den Ausführungsstatus des Testlaufs.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-run-status.png" alt-text="Screenshot: Testfortschritt des Testers":::

1. Überprüfen Sie den Bericht, nachdem der Test abgeschlossen wurde. Der Bericht enthält die Statistiken, alle Fehler während des Testlaufs und einen Detailbericht.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-test-result.png" alt-text="Screenshot: Beispieltestbericht für Tester":::

1. Klicken Sie auf „Details“, um weitere Informationen zu erhalten.

   Beispiel einer erfolgreichen Datenüberprüfung.
   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-test-success.png" alt-text="Screenshot: Erfolgsbericht eines Beispieltests.":::

   Beispiel einer fehlgeschlagenen Datenüberprüfung.

   :::image type="content" source="./media/oracle-to-sql-database-guide/tester-test-failed.png" alt-text="Screenshot: Fehlerbericht des Testers":::

### <a name="optimize"></a>Optimieren

Die Phase nach der Migration ist entscheidend für den Abgleich von Problemen mit der Datengenauigkeit, die Überprüfung der Vollständigkeit und das Beheben von Leistungsproblemen der Workload.

> [!NOTE]
> Weitere Informationen zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Leitfaden zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Migrationsressourcen

Weitere Unterstützung bei der Durchführung dieses Migrationsszenarios finden Sie in den folgenden Ressourcen. Sie wurden für die Unterstützung eines echten Migrationsprojekts entwickelt.

| **Titel/Link**                                                                                                                                          | **Beschreibung**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Data Workload Assessment Model and Tool](https://www.microsoft.com/download/details.aspx?id=103130) | Dieses Tool stellt für eine bestimmte Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen oder Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch einen automatisierten und einheitlichen Entscheidungsprozess für die Zielplattform dabei helfen, Bewertungen von umfangreichen Datenbeständen zu beschleunigen.                                                          |
| [Oracle Inventory Script Artifacts](https://www.microsoft.com/download/details.aspx?id=103121)                 | Diese Ressource umfasst eine PL/SQL-Abfrage für Oracle-Systemtabellen, die die Anzahl von Objekten nach Schematyp, Objekttyp und Status bereitstellt. Darüber hinaus erhalten Sie eine grobe Schätzung zu den „Rohdaten“ sowie zur Größe der Tabellen in jedem Schema. Die Ergebnisse werden im CSV-Format gespeichert.                                                                                                               |
| [Automate SSMA Oracle Assessment Collection & Consolidation](https://www.microsoft.com/download/details.aspx?id=103120)                                             | Diese Ressourcensammlung verwendet eine CSV-Datei als Eingabe („sources.csv“ in den Projektordnern), um die XML-Dateien zu erstellen, die zum Ausführen einer SSMA-Bewertung im Konsolenmodus benötigt werden. Die Datei „source.csv“ wird vom Kunden bereitgestellt und basiert auf einer Inventarisierung vorhandener Oracle-Instanzen. Die Ausgabedateien sind „AssessmentReportGeneration_source_1.xml“, „ServersConnectionFile.xml“ und „VariableValueFile.xml“.|
| [Oracle zu SQL DB – Hilfsprogramm „Database Compare“](https://www.microsoft.com/download/details.aspx?id=103016)|SSMA für Oracle Tester ist das empfohlene Tool, um automatisch die Umwandlung von Datenbankobjekten und Datenmigration zu überprüfen. Außerdem enthält es eine Obermenge der Database Compare-Funktionen.<br /><br />Wenn Sie nach einer alternativen Datenüberprüfungsoption suchen, können Sie Daten mithilfe des Hilfsprogramms „Database Compare“ in ausgewählten oder allen Tabellen, Zeilen und Spalten bis auf Zeilen- oder Spaltenebene vergleichen.|

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Hauptanwendung dieses Teams besteht darin, die komplexe Modernisierung für Datenplattform-Migrationsprojekte auf der Azure-Datenplattform von Microsoft freizugeben und zu beschleunigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarien sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Informationen zu SQL-Datenbank finden Sie unter:
  - [Übersicht über Azure SQL-Datenbank](../../database/sql-database-paas-overview.md)
  - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/)

- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)
   -  [Ressourcen zur Cloudmigration](https://azure.microsoft.com/migration/resources)

- Videoinhalte finden Sie unter:
    - [Übersicht über den Migrationsprozess und die Tools und Dienste, die für die Durchführung von Bewertung und Migration empfohlen werden](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
