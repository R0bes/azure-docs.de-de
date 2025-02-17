---
title: Bedrohungssuchfunktionen in Azure Sentinel | Microsoft-Dokumentation
description: Verwenden Sie die integrierten Suchabfragen von Azure Sentinel, um die richtigen Fragen zu stellen, mit denen Sie Probleme in Ihren Daten finden können.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2021
ms.author: yelevin
ms.openlocfilehash: 78662bf6dbc6d4be4f0ea0890993530f772d2114
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355765"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>Suchen nach Bedrohungen mit Azure Sentinel

> [!IMPORTANT]
>
> Die ressourcenübergreifenden Abfragefunktionen sowie Upgrades für das **Hunting-Dashboard** (markierte Elemente im Anschluss) befinden sich derzeit in der **VORSCHAUPHASE**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Als Sicherheitsanalysten und Prüfer möchten Sie proaktiv nach Sicherheitsbedrohungen suchen, aber Ihre verschiedenen Systeme und Sicherheits-Appliances generieren eine Menge von Daten, deren Analyse und Filterung in aussagekräftige Ereignisse schwierig sein können. Azure Sentinel verfügt über leistungsstarke Such- und Abfragetools zum Aufspüren von Sicherheitsbedrohungen in den Datenquellen Ihrer Organisation. Um Sicherheitsanalysten dabei zu unterstützen, proaktiv nach neuen Anomalien zu suchen, die von Ihren Sicherheits-Apps oder auch Ohren Regeln zur geplanten Analyse nicht erkannt wurden, werden Sie von den in Azure Sentinel integrierten Bedrohungssuchabfragen dazu geführt, die richtigen Fragen zu stellen, um Probleme in den Daten zu finden, die bereits in Ihrem Netzwerk vorhanden sind. 

Zum Beispiel liefert eine integrierte Abfrage Daten über die ungewöhnlichsten Prozesse, die in Ihrer Infrastruktur ausgeführt werden. Sie möchten nicht, dass bei jeder Ausführung eines solchen Prozesse eine Benachrichtigung erfolgt, denn er könnte völlig unproblematisch sein, aber Sie möchten die Abfrage gelegentlich ausführen, um zu sehen, ob es etwas Ungewöhnliches gibt.

## <a name="use-built-in-queries"></a>Verwenden integrierter Abfragen

Das [Hunting-Dashboard](#use-the-hunting-dashboard-public-preview) bietet vorgefertigte Abfragebeispiele, um Ihnen den Einstieg zu erleichtern und Sie an die Tabellen und die Abfragesprache heranzuführen. Abfragen werden für in Protokolltabellen gespeicherte Daten ausgeführt – beispielsweise für die Prozesserstellung, für DNS-Ereignisse oder für andere Ereignistypen.

Integrierte Hunting-Abfragen werden von Microsoft-Sicherheitsexperten kontinuierlich weiterentwickelt. Sie fügen neue Abfragen hinzu und optimieren bereits vorhandene Abfragen, um Ihnen einen Einstiegspunkt für die Suche nach neuen Erkennungen zu bieten und Sie bei der Entscheidung zu unterstützen, wo mit der Suche nach den Anfängen neuer Angriffe begonnen werden sollte.

Verwenden Sie Abfragen vor, während und nach einer Kompromittierung, um die folgenden Aktionen durchzuführen:

- **Vor einem Incident:** Es reicht nicht aus, auf Erkennungen zu warten. Ergreifen Sie proaktive Maßnahmen, indem Sie mindestens einmal pro Woche alle Abfragen für die Bedrohungssuche im Zusammenhang mit den Daten ausführen, die Sie in Ihrem Arbeitsbereich erfassen.

    Die Ergebnisse Ihrer proaktiven Suche geben frühzeitig Aufschluss über Ereignisse, die unter Umständen eine aktuelle Kompromittierung bestätigen oder zumindest Schwachpunkte in Ihrer Umgebung aufzeigen, die gefährdet sind und behandelt werden müssen.

- **Während einer Kompromittierung:** Verwenden Sie einen [Livestream](livestream.md), um eine bestimmte Abfrage kontinuierlich auszuführen und eingehende Ergebnisse umgehend anzuzeigen. Livestreams können verwendet werden, wenn Sie Benutzerereignisse aktiv überwachen müssen – beispielsweise, um zu überprüfen, ob eine bestimmte Kompromittierung noch besteht, um die nächste Aktion eines Bedrohungsakteurs zu bestimmen oder um sich gegen Ende einer Untersuchung zu vergewissern, dass die Kompromittierung tatsächlich vorbei ist.

- **Nach einer Kompromittierung:** Verbessern Sie nach einer Kompromittierung oder nach einem Incident Ihre Abdeckung und Erkenntnisse, um ähnliche Incidents in Zukunft zu verhindern.

    - Ändern Sie Ihre vorhandenen Abfragen, oder erstellen Sie neue, die Sie bei der frühzeitigen Erkennung unterstützen. Orientieren Sie sich dabei an den Erkenntnissen, die Sie durch die Kompromittierung oder den Incident gewonnen haben.

    - Wenn Sie eine Hunting-Abfrage entdeckt oder erstellt haben, die wertvolle Erkenntnisse zu möglichen Angriffen liefert, können Sie auf der Grundlage der Abfrage benutzerdefinierte Erkennungsregeln erstellen und diese Erkenntnisse als Warnungen in Ihren Antwortdiensten für Sicherheitsvorfälle verfügbar machen.

        Zeigen Sie die Ergebnisse der Abfrage an, und wählen Sie **Neue Warnungsregel** > **Azure Sentinel-Warnung erstellen** aus. Verwenden Sie den **Analyseregel-Assistenten**, um eine neue, auf Ihrer Abfrage basierende Regel zu erstellen. Weitere Informationen finden Sie unter [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md).


> [!TIP]
> - Während der Public Preview-Phase können nun auch Hunting- und Livestreamabfragen für gespeicherte Daten in Azure Data Explorer erstellt werden. Weitere Informationen finden Sie in der Dokumentation zu Azure Monitor unter [Ressourcenübergreifende Abfrage: Azure Data Explorer mithilfe von Azure Monitor](../azure-monitor/logs/azure-monitor-data-explorer-proxy.md).
>
> - Weitere Abfragen und Datenquellen finden Sie in Communityressourcen wie dem [GitHub-Repository für Azure Sentinel](https://github.com/Azure/Azure-Sentinel/tree/master/Hunting%20Queries).
>

## <a name="use-the-hunting-dashboard-public-preview"></a>Verwenden des Hunting-Dashboards (Public Preview)

Mit dem Hunting-Dashboard können Sie alle Ihre Abfragen oder eine ausgewählte Teilmenge mit einer einzigen Auswahl ausführen. Wählen Sie im Azure Sentinel-Portal die Option **Hunting** aus.

Die daraufhin angezeigte Tabelle enthält alle Abfragen, die von den Sicherheitsanalysten von Microsoft geschrieben wurden, sowie alle weiteren Abfragen, die Sie selbst erstellt oder geändert haben. Jede Abfrage enthält eine Beschreibung dazu, nach welcher Bedrohung mit ihr gesucht wird und für welche Art von Daten sie ausgeführt wird. Diese Vorlagen sind nach ihren verschiedene Taktiken gruppiert: Die Symbole auf der rechten Seite kategorisieren den Typ der Bedrohung, etwa Erstzugriff, Persistenz und Exfiltration.

:::image type="content" source="media/hunting/hunting-start.png" alt-text="Azure Sentinel beginnt mit der Bedrohungssuche" lightbox="media/hunting/hunting-start.png":::

Verwenden Sie das Hunting-Dashboard, um zu ermitteln, wo mit der Suche begonnen werden soll. Betrachten Sie dazu die Ergebnisanzahl, Spitzen oder die Veränderung der Ergebnisanzahl über einen Zeitraum von 24 Stunden. Sortieren und filtern Sie nach Favoriten, Datenquelle, MITRE ATT&CK-Taktik und -Verfahren, Ergebnissen oder Ergebnisdelta. Zeigen Sie Abfragen an, für die noch eine Verbindung mit Datenquellen hergestellt werden muss**, und erhalten Sie Empfehlungen zum Aktivieren dieser Abfragen.

In der folgenden Tabelle werden detaillierte Aktionen beschrieben, die über das Hunting-Dashboard verfügbar sind:

|Aktion  |BESCHREIBUNG  |
|---------|---------|
|**Ermitteln, inwiefern Abfragen für Ihre Umgebung geeignet sind**     |   Wählen Sie die Schaltfläche **Run all queries (Preview)** (Alle Abfragen ausführen (Vorschau)) aus, oder wählen Sie mithilfe der Kontrollkästchen links neben den einzelnen Zeilen einen Teil der Abfragen und anschließend die Schaltfläche **Run selected queries (Preview)** (Ausgewählte Abfragen ausführen (Vorschau)) aus. <br><br>Die Abfrageausführung kann wenige Sekunden, aber auch mehrere Minuten dauern. Dies hängt von der Anzahl ausgewählter Abfragen, vom Zeitbereich und von der abgefragten Datenmenge ab.      |
|**Anzeigen der Abfragen, von denen Ergebnisse zurückgegeben wurden**    |      Nach Abschluss Ihrer Abfragen können Sie mithilfe des Filters **Ergebnisse** die Abfragen anzeigen, von denen Ergebnisse zurückgegeben wurden: <br>- Sortieren Sie sie, um zu sehen, welche Abfragen die meisten oder wenigsten Ergebnisse hatten. <br>- Wählen im Filter **Ergebnisse** die Option *Nicht zutreffend* aus, um die Abfragen anzuzeigen, die in Ihrer Umgebung nicht aktiv sind. <br>- Bewegen Sie den Mauszeiger auf das Infosymbol (**i**) neben *Nicht zutreffend*, um zu sehen, welche Datenquellen erforderlich sind, um diese Abfrage zu aktivieren.  |
|**Identifizieren von Spitzen in Ihren Daten**     |   Sie können Datenspitzen identifizieren, indem Sie nach **Ergebnis Delta** sortieren oder filtern. <br><br>Dadurch werden die Ergebnisse der letzten 24 Stunden mit den Ergebnissen der vorherigen 24 bis 48 Stunden verglichen und alle signifikanten Volumenunterschiede hervorgehoben.     |
|**Anzeigen von Abfragen, die der MITRE Att&CK-Taktik zugeordnet sind**     | Die **MITRE ATT&CK-Taktikleiste** im oberen Bereich der Tabelle gibt an, wie viele Abfragen der jeweiligen MITRE ATT&CK-Taktik zugeordnet sind. Die Taktikleiste wird basierend auf dem aktuellen Satz angewendeter Filter dynamisch aktualisiert. <br><br>Dadurch sehen Sie, welche MITRE ATT&CK-Taktiken erscheinen, wenn Sie nach einer bestimmten Ergebnisanzahl, einem hohen Ergebnisdelta, nach Ergebnissen vom Typ *Nicht zutreffend* oder nach etwas anderem filtern.        |
|**Anzeigen von Abfragen, die MITRE Att&CK-Techniken zugeordnet sind**     | Abfragen können auch MITRE ATT&CK-Techniken zugeordnet werden. Sie können mithilfe des Filters **Technik** nach MITRE ATT&CK-Techniken filtern oder sortieren. Wenn Sie eine Abfrage öffnen, können Sie die Technik auswählen, um die MITRE ATT&CK-Beschreibung der Technik anzuzeigen.        |
|**Speichern einer Abfrage in Ihren Favoriten**     |   In Ihren Favoriten gespeicherte Abfragen werden jedes Mal automatisch ausgeführt, wenn auf die **Bedrohungssuche-Seite** zugegriffen wird. Sie können eine eigene Bedrohungssuchabfrage erstellen oder eine vorhandene Vorlage für Bedrohungssuchabfragen klonen oder anpassen.      |
|**Ausführen von Abfragen**     |   Wählen Sie auf der Seite mit den Details der Hunting-Abfrage die Option **Abfrage ausführen** aus, um die Abfrage direkt über die Seite „Hunting“ auszuführen. Die Anzahl der Übereinstimmungen wird in der Tabelle in der Spalte **Ergebnisse** angezeigt. Überprüfen Sie die Liste der Bedrohungssuchabfragen und deren Übereinstimmungen.     |
|**Überprüfen einer zugrunde liegenden Abfrage**     | Im Bereich mit den Abfragedetails können Sie eine schnelle Überprüfung der zugrunde liegenden Abfrage durchführen. Sie können die Ergebnisse anzeigen, indem Sie auf den Link **Abfrageergebnisse anzeigen** (unterhalb des Abfragefensters) oder auf die Schaltfläche **Ergebnisse anzeigen** (unten im Bereich) klicken. Die Abfrage wird auf dem Blatt **Protokolle** (Log Analytics) geöffnet, und unterhalb der Abfrage können Sie die Übereinstimmungen für die Abfrage überprüfen.         |
|     |         |


## <a name="create-your-own-bookmarks"></a>Erstellen eigener Lesezeichen

Während des Hunting- und Untersuchungsprozesses stoßen Sie möglicherweise auf ungewöhnlich oder verdächtig aussehende Abfrageergebnisse. Versehen Sie diese Elemente mit einem Lesezeichen, um später auf sie zurückzukommen – etwa beim Erstellen oder Anreichern eines Incidents für die Untersuchung.

- Aktivieren Sie in Ihren Ergebnissen die Kontrollkästchen aller Zeilen, die Sie speichern möchten, und wählen Sie **Lesezeichen hinzufügen** aus. Dadurch wird für jede markierte Zeile ein Datensatz (Lesezeichen) erstellt, der die Zeilenergebnisse, die Abfrage, die die Ergebnisse erstellt hat, sowie Entitätszuordnungen zum Extrahieren von Benutzern, Hosts und IP-Adressen enthält. Sie können jedem Lesezeichen Ihre eigenen Tags und Notizen hinzufügen.

- Klicken Sie auf der Hauptseite **Hunting** auf die Registerkarte **Lesezeichen**, um alle mit Lesezeichen versehenen Ergebnisse anzuzeigen. Fügen Sie Lesezeichen Tags hinzu, um sie für die Filterung zu klassifizieren. Wenn Sie beispielsweise eine Angriffskampagne untersuchen, können Sie ein Tag für die Kampagne erstellen, das Tag allen relevanten Lesezeichen hinzufügen und dann alle Lesezeichen auf Basis der Kampagne filtern.

- Untersuchen Sie ein einzelnes, mit Lesezeichen versehenes Ergebnis, indem Sie das Lesezeichen auswählen und anschließend im Detailbereich auf **Untersuchen** klicken, um die Untersuchungsumgebung zu öffnen.

    Sie können auch einen Incident auf der Grundlage eines oder mehrerer Lesezeichen erstellen oder einem bereits vorhandenen Incident ein oder mehrere Lesezeichen hinzufügen. Aktivieren Sie das Kontrollkästchen links neben den Lesezeichen, die Sie verwenden möchten, und wählen Sie anschließend **Incidentaktionen** > **Neuen Incident erstellen** oder **Zu vorhandenem Incident hinzufügen** aus. Selektieren und untersuchen Sie den Incident wie gewohnt.


> [!TIP]
> Lesezeichen stellen beachtenswerte Schlüsselereignisse dar, die zu Incidents eskaliert werden sollten, wenn sie schwerwiegend genug sind, um eine Untersuchung zu erfordern. Ereignisse wie potenzielle Grundursachen, Anzeichen einer Kompromittierung oder andere wichtige Ereignisse sollten mit einem Lesezeichen versehen werden.
>

Weitere Informationen zu Lesezeichen finden Sie unter [Behalten des Überblicks über Daten beim Hunting mit Azure Sentinel](bookmarks.md).

## <a name="use-notebooks-to-power-investigations"></a>Verwenden von Notebooks für Untersuchungen

Notebooks bieten eine Art virtuelle Sandbox mit eigenem Kernel. Sie können Notebooks verwenden, um Ihre Bedrohungssuche und Untersuchungen durch maschinelles Lernen, Visualisierung und Datenanalyse zu verbessern. Sie können eine vollständige Untersuchung in einem Notebook durchführen, die Rohdaten, den Code, den Sie darauf ausführen, die Ergebnisse und ihre Visualisierungen kapseln und das Ganze speichern, damit es für andere Benutzer in Ihrer Organisation freigegeben und wiederverwendet werden kann.

Weitere Informationen finden Sie unter [Aufspüren von Sicherheitsrisiken mit Jupyter Notebook](notebooks.md).


## <a name="useful-operators-and-functions"></a>Praktische Operatoren und Funktionen

Hunting-Abfragen werden in der [Kusto-Abfragesprache (KQL)](/azure/data-explorer/kusto/query/) erstellt. Diese leistungsstarke Abfragesprache mit IntelliSense bietet die Leistungsfähigkeit und Flexibilität, die Sie für die nächste Hunting-Stufe benötigen.

Es ist dieselbe Sprache, die von den Abfragen in Ihren Analyseregeln und an anderer Stelle in Azure Sentinel verwendet wird. Weitere Informationen finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](../azure-monitor/logs/get-started-queries.md).

Die folgenden Operatoren sind bei Hunting-Abfragen für Azure Sentinel besonders hilfreich:

- **where**: Filtert eine Tabelle auf die Teilmenge von Zeilen, die ein Prädikat erfüllen.

- **summarize**: Erzeugt eine Tabelle, in der der Inhalt der Eingabetabelle aggregiert ist.

- **join**: Führt die Zeilen zweier Tabellen zusammen, um eine neue Tabelle zu erzeugen, indem Werte aus den angegebenen Spalten aus beiden Tabellen zugeordnet werden.

- **count**: Gibt die Anzahl von Datensätzen in der Eingabedatensatzgruppe zurück.

- **top**: Gibt die ersten N Datensätze zurück, wobei diese nach den angegebenen Spalten sortiert sind.

- **limit**: Gibt maximal die angegebene Anzahl von Zeilen zurück.

- **project**: Wählt die Spalten aus, die einbezogen, umbenannt oder gelöscht werden sollen, und fügt neue berechnete Spalten ein.

- **extend**: Erstellt berechnete Spalten und fügt diese an das Resultset an.

- **makeset**: Gibt ein dynamisches(JSON) Array der Menge unterschiedlicher Werte zurück, die ein Ausdruck (Expr) in der Gruppe annimmt.

- **find**: Sucht nach Zeilen, die einem Prädikat in einer Gruppe von Tabellen entsprechen.

- **adx() (Vorschauversion)** : Diese Funktion führt ressourcenübergreifende Abfragen für Azure Data Explorer-Datenquellen über die Hunting-Umgebung von Azure Sentinel und über Log Analytics aus. Weitere Informationen finden Sie unter [Ressourcenübergreifende Abfrage: Azure Data Explorer mithilfe von Azure Monitor](../azure-monitor/logs/azure-monitor-data-explorer-proxy.md).

## <a name="save-a-query"></a>Speichern einer Abfrage

Erstellen oder ändern Sie eine Abfrage, und speichern Sie sie als Ihre eigene Abfrage, oder teilen Sie sie mit Benutzern im gleichen Mandanten.

:::image type="content" source="./media/hunting/save-query.png" alt-text="Speichern der Abfrage" lightbox="./media/hunting/save-query.png":::

**So erstellen Sie eine neue Abfrage:**

1. Klicken Sie auf **Neue Abfrage**.

1. Füllen Sie alle leeren Felder aus, und wählen Sie **Erstellen** aus.

    :::image type="content" source="./media/hunting/new-query.png" alt-text="Neue Abfrage" lightbox="./media/hunting/new-query.png":::

**So klonen und ändern Sie eine vorhandene Abfrage:**

1. Wählen Sie in der Tabelle die Bedrohungssuchabfrage aus, die Sie ändern möchten.

1. Wählen Sie in der Zeile der Abfrage, die Sie ändern möchten, die Auslassungspunkte (...) aus, und wählen Sie **Abfrage klonen**.

    :::image type="content" source="./media/hunting/clone-query.png" alt-text="Abfrage klonen" lightbox="./media/hunting/clone-query.png":::

1. Ändern Sie die Abfrage, und wählen Sie **Erstellen** aus.

    :::image type="content" source="./media/hunting/custom-query.png" alt-text="Benutzerdefinierte Abfrage" lightbox="./media/hunting/custom-query.png":::



## <a name="sample-query"></a>Beispielabfrage

Eine typische Abfrage beginnt mit einem Tabellennamen, gefolgt von einer Reihe von Operatoren, die durch \| getrennt sind.

Beginnen Sie im obigen Beispiel mit dem Tabellennamen „SecurityEvent“, und fügen Sie nach Bedarf über Pipes verkettete Elemente hinzu.

1. Definieren Sie einen Zeitfilter, um nur Datensätze aus den letzten sieben Tagen zu überprüfen.

1. Fügen Sie der Abfrage einen Filter hinzu, um nur Ereignisse mit der ID 4688 anzuzeigen.

1. Fügen Sie der Abfrage einen Filter für die Befehlszeile (CommandLine) hinzu, sodass nur Instanzen von „cscript.exe“ enthalten sind.

1. Bilden Sie nur die Spalten ab, die Sie untersuchen möchten, begrenzen Sie die Ergebnisse auf 1.000, und wählen Sie anschließend **Abfrage ausführen** aus.

1. Wählen Sie das grüne Dreieck aus, um die Abfrage auszuführen. Sie können die Abfrage testen, und sie ausführen, um nach anormalem Verhalten zu suchen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Bedrohungsuntersuchung mit Azure Sentinel ausführen. 

Weitere Informationen finden Sie unter:

- [Verwenden von Notebooks zur Ermittlung von Anomalien](notebooks.md)
- [Behalten des Überblicks über Daten bei einer Bedrohungssuche](bookmarks.md)

Erfahren Sie anhand eines Beispiels, wie benutzerdefinierte Analyseregeln bei der [Überwachung von Zoom](https://techcommunity.microsoft.com/t5/azure-sentinel/monitoring-zoom-with-azure-sentinel/ba-p/1341516) mit einem [benutzerdefinierten Connector](create-custom-connector.md) eingesetzt werden.

