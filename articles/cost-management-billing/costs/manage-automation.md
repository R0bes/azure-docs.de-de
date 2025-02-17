---
title: Verwalten der Azure-Kosten mithilfe einer Automatisierung
description: In diesem Artikel wird erläutert, wie Sie Azure-Kosten mithilfe einer Automatisierung verwalten können.
author: bandersmsft
ms.author: banders
ms.date: 03/19/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 1c3e7399eedb6c3c4b78c9838f3724d94f20f0e4
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122347068"
---
# <a name="manage-costs-with-automation"></a>Verwalten der Kosten per Automatisierung

Sie können mithilfe der Cost Management-Automatisierung einen benutzerdefinierten Satz von Lösungen erstellen, um Kostendaten abzurufen und zu verwalten. Dieser Artikel behandelt gängige Szenarien für die Cost Management-Automatisierung und die Optionen, die in Ihrer jeweiligen Situation zur Verfügung stehen. Wenn Sie eine Entwicklung mithilfe von APIs anstreben, können die hier bereitgestellten Beispiele gängiger API-Anforderungen Sie dabei unterstützen, Ihren Entwicklungsprozess zu beschleunigen.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Automatisieren des Abrufs von Kostendaten zur Offlineanalyse

Möglicherweise müssen Sie Ihre Azure-Kostendaten herunterladen, um sie mit weiteren Datasets zusammenzuführen. Oder Sie müssen Kostendaten in Ihre eigenen Systeme integrieren. Ja nach Datenmenge stehen verschiedene Optionen zur Verfügung. In jedem Fall müssen Sie über Cost Management-Berechtigungen im geeigneten Umfang verfügen, um APIs und Tools zu nutzen. Weitere Informationen finden Sie unter [Zuweisen des Zugriffs auf Daten](./assign-access-acm-data.md).

## <a name="suggestions-for-handling-large-datasets"></a>Vorschläge für die Verarbeitung großer Datasets

Wenn Ihre Organisation Azure in großem Umfang nutzt und über viele Ressourcen oder Abonnements verfügt, fällt eine große Menge an Nutzungsdaten an. Excel kann derartig große Dateien häufig nicht laden. In dieser Situation empfehlen wir die folgenden Optionen:

**Power BI**

Power BI wird dazu genutzt, große Datenmengen zu erfassen und zu verarbeiten. Als Enterprise Agreement-Kunde können Sie die Power BI-Vorlagen-App verwenden, um Kosten für Ihr Abrechnungskonto zu analysieren. Der Bericht enthält Schlüsselansichten, die von Kunden verwendet werden. Weitere Informationen finden Sie unter [Analysieren der Azure-Kosten mit der Power BI-Vorlagen-App](./analyze-cost-data-azure-cost-management-power-bi-template-app.md).

**Power BI-Datenconnector**

Wenn Sie Ihre Daten täglich analysieren möchten, empfehlen wird die Verwendung des [Power BI-Datenconnectors](/power-bi/connect-data/desktop-connect-azure-cost-management) zum Abrufen von Daten für eine detaillierte Analyse. Alle erstellten Berichte werden durch den Connector auf den neuesten Stand gebracht, wenn weitere Kosten anfallen.

**Cost Management-Exporte**

Vielleicht ist es nicht erforderlich, die Daten täglich zu analysieren. In diesem Fall können Sie das Cost Management-Feature [Exporte](./tutorial-export-acm-data.md) einsetzen, um Datenexporte in ein Azure Storage-Konto zu planen. Anschließend laden Sie die Daten nach Bedarf in Power BI oder analysieren die Daten in Excel, falls die Datei klein genug ist. Exporte stehen im Azure-Portal zur Verfügung oder können mit der [Export-API](/rest/api/cost-management/exports) konfiguriert werden.

**Nutzungsdetails-API**

Erwägen Sie die Verwendung der [Nutzungsdetails-API](/rest/api/consumption/usageDetails), wenn Sie über ein kleines Dataset mit Kostendaten verfügen. Bei einer großen Menge an Kostendaten sollten Sie für einen festgelegten Zeitraum möglichst wenig Nutzungsdaten abfragen. Hierzu legen Sie entweder ein kleines Zeitfenster fest oder verwenden in Ihrer Anforderung einen Filter. Beispielsweise zeigt die API in einem Szenario, in dem Sie Kostendaten für drei Jahre benötigen, eine bessere Leistung, wenn Sie anstelle eines einzigen Aufrufs mehrere Aufrufe für verschiedene Zeitbereiche ausführen. Anschließend können Sie die Daten zur weiteren Analyse in Excel laden.

## <a name="automate-retrieval-with-usage-details-api"></a>Automatischer Abruf mit der Nutzungsdetails-API

Die [Nutzungsdetails-API](/rest/api/consumption/usageDetails) bietet eine einfache Möglichkeit, unformatierte, nicht aggregierte Kostendaten abzurufen, die Ihrer Azure-Rechnung entsprechen. Die API ist nützlich, wenn Ihre Organisation eine Lösung für den programmgesteuerten Datenabruf benötigt. Erwägen Sie den Einsatz der API, wenn Sie kleinere Datasets mit Kostendaten analysieren möchten. Wenn Sie hingegen über große Datasets verfügen, sollten Sie sich für eine der anderen Lösungen entscheiden, die zuvor genannt wurden. Die Daten in der Nutzungsdetails-API werden basierend auf Verbrauchseinheiten pro Tag bereitgestellt. Sie werden bei der Berechnung Ihrer monatlichen Rechnung verwendet. Die allgemein verfügbare Version dieser API ist `2019-10-01`. Verwenden Sie `2019-04-01-preview`, um per API auf die Vorschauversion für Reservierungen und Azure Marketplace-Käufe zuzugreifen.

Wenn Sie in regelmäßigen Abständen große Mengen von exportierten Daten abrufen möchten, finden Sie weitere Informationen unter [Regelmäßiges Abrufen großer Datasets mit Kostendaten mithilfe von Exporten](ingest-azure-usage-at-scale.md).

### <a name="usage-details-api-suggestions"></a>Vorschläge zur Nutzungsdetails-API

**Anforderungszeitplan**

Es wird empfohlen, dass Sie pro Tag _nicht mehr als eine Anforderung_ an die Nutzungsdetails-API übermitteln. Weitere Informationen dazu, wie häufig Kostendaten aktualisiert und wie Beträge gerundet werden, finden Sie unter [Grundlegendes zu Cost Management-Daten](./understand-cost-mgt-data.md).

**Verwendung von Bereichen oberster Ebene ohne Filterung**

Verwenden Sie die API zum Abrufen aller benötigten Daten auf der höchstmöglichen Ebene. Führen Sie erst dann eine Filterung, Gruppierung oder aggregierte Analyse durch, wenn alle erforderlichen Daten erfasst wurden. Die API ist dafür optimiert, große Mengen an nicht aggregierten, unformatierten Kostendaten bereitzustellen. Weitere Informationen zu den verfügbaren Bereichen in Cost Management finden Sie unter [Verstehen von und Arbeiten mit Bereichen](./understand-work-scopes.md). Nachdem Sie die erforderlichen Daten für einen Bereich heruntergeladen haben, verwenden Sie Excel, um die Daten mithilfe von Filtern und Pivottabellen zu analysieren.

### <a name="notes-about-pricing"></a>Hinweise zu Preisen

Beachten Sie die folgenden Informationen, wenn Sie Ihre Nutzung und Gebühren mit Ihrem Preisblatt oder Ihrer Rechnung abstimmen möchten.

Preisverhalten auf dem Preisblatt: Das Preisblatt enthält die Preise, die Sie von Azure erhalten haben. Sie wurden anhand einer bestimmten Maßeinheit skaliert. Leider ist die Maßeinheit nicht immer an der Maßeinheit ausgerichtet, auf der die tatsächliche Ressourcennutzung und die Gebühren basieren.

Preisverhalten für Nutzungsdetails: In Nutzungsdateien werden skalierte Informationen angezeigt, die ggf. nicht genau mit den Angaben im Preisblatt übereinstimmen. Dies gilt insbesondere in folgenden Fällen:

- Einzelpreis: Der Preis wird anhand der Maßeinheit skaliert, auf deren Grundlage die Gebühren von den Azure-Ressourcen tatsächlich ausgegeben werden. Wenn die Skalierung durchgeführt wird, stimmt der Preis nicht mit dem Preis im Preisblatt überein.
- Maßeinheit: Die Maßeinheit, über die die Gebühren von den Azure-Ressourcen tatsächlich ausgegeben werden.
- Tatsächlicher Preis/Ressourcensatz: Der Preis steht für den tatsächlichen Satz, den Sie pro Einheit zahlen, nachdem die Rabatte berücksichtigt wurden. Dieser Preis sollte für die Menge verwendet werden, um die Berechnungen der Art „Preis mal Menge“ zum Abstimmen der Gebühren durchzuführen. Bei diesem Preis werden die folgenden Szenarien und der Preis für die skalierte Einheit genutzt, der auch in den Dateien enthalten ist. Aus diesem Grund kann sich dieser Preis vom Preis für die skalierte Einheit unterscheiden.
  - Mehrstufige Preise: Ein Beispiel für mehrstufige Preise ist die Berechnung von 10 USD für die ersten 100 Einheiten und von 8 USD für die nächsten 100 Einheiten.
  - Enthaltene Menge – Beispiel: Die ersten 100 Einheiten sind kostenlos, und dann fallen Kosten von 10 USD pro Einheit an.
  - Reservations
  - Rundung während der Berechnung: Bei der Rundung werden die verbrauchte Menge, die mehrstufigen Preise bzw. die Preise für die enthaltene Menge und der Preis für die skalierte Einheit berücksichtigt.

### <a name="a-single-resource-might-have-multiple-records-for-a-single-day"></a>Eine einzelne Ressource kann über mehrere Datensätze für einen einzelnen Tag verfügen.

Azure-Ressourcenanbieter geben Nutzung und Gebühren an das Abrechnungssystem aus und füllen das Feld `Additional Info` der Nutzungsdatensätze auf. Gelegentlich geben Ressourcenanbieter die Nutzung für einen bestimmten Tag aus und stempeln die Datensätze mit unterschiedlichen Rechenzentren im Feld `Additional Info` der Nutzungsdatensätze. Dies kann dazu führen, dass in Ihrer Nutzungsdatei mehrere Datensätze für eine Verbrauchseinheit/Ressource für einen einzelnen Tag vorhanden sind. In diesem Fall wird Ihnen kein zu hoher Betrag in Rechnung gestellt. Die verschiedenen Datensätze stellen die vollständigen Kosten der Verbrauchseinheit für die Ressource an diesem Tag dar.

## <a name="example-usage-details-api-requests"></a>Nutzungsdetails-API – Beispielanforderungen

Die folgenden Beispielanforderungen werden von Microsoft-Kunden verwendet, um gängige Szenarien abzudecken.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Abrufen von Nutzungsdetails für einen Bereich in einem bestimmten Datumsbereich

Die über die Anforderung zurückgegebenen Daten entsprechen dem Datum, an dem die Nutzung vom Abrechnungssystem erfasst wurde. Die Daten können Kosten mehrerer Rechnungen umfassen. Der zu verwendende Aufruf ist je nach Abonnementtyp verschieden.

Verwenden Sie für ältere Kunden mit einem Enterprise Agreement (EA) oder einem Abonnement mit nutzungsbasierter Bezahlung den folgenden Aufruf:

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01
```

Verwenden Sie für zeitgemäße Kunden mit einer Microsoft-Kundenvereinbarung den folgenden Aufruf:

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?startDate=2020-08-01&endDate=2020-08-05&$top=1000&api-version=2019-10-01
```

### <a name="get-amortized-cost-details"></a>Abrufen amortisierter Kostendaten

Falls Sie für die Ist-Kosten Einkäufe anzeigen möchten, sobald diese anfallen, können Sie den Wert für *metric* in der folgenden Anforderung in `ActualCost` ändern. Für amortisierte Kosten und Ist-Kosten müssen Sie die Version `2019-04-01-preview` verwenden. Die aktuelle API-Version funktioniert – abgesehen vom neuen type/metric-Attribut und geänderten Eigenschaftennamen – genauso wie Version `2019-10-01`. Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, lauten Ihre Filter im folgenden Beispiel `startDate` und `endDate`.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="automate-alerts-and-actions-with-budgets"></a>Automatisieren von Warnungen und Aktionen mit Budgets

Die Maximierung des Nutzens Ihrer Cloudinvestition umfasst zwei wichtige Komponenten: Die eine ist die automatische Budgeterstellung. Die andere ist die Konfiguration einer kostenbasierten Orchestrierung, um auf Budgetwarnungen zu reagieren. Die Azure-Budgeterstellung kann auf verschiedene Arten automatisiert werden. Die Überschreitung Ihrer konfigurierten Warnungsschwellenwerte hat verschiedene Reaktionen zur Folge.

In den folgenden Abschnitten werden die verfügbaren Optionen behandelt und exemplarische API-Anforderungen bereitgestellt, um Ihnen den Einstieg in die Budgetautomatisierung zu erleichtern.

### <a name="how-costs-are-evaluated-against-your-budget-threshold"></a>Bewerten von Kosten auf der Grundlage Ihres Budgetschwellenwerts

Ihre Kosten werden einmal pro Tag auf der Grundlage Ihres Budgetschwellenwerts bewertet. Bei der Erstellung eines neuen Budgets oder am Tag für die Budgetzurücksetzung sind die Kosten, die mit dem Schwellenwert verglichen werden, Null bzw. NULL, da die Bewertung möglicherweise nicht stattgefunden hat.

Wenn Azure erkennt, dass der Schwellenwert für Ihre Kosten überschritten wurde, wird spätestens eine Stunde nach der Erkennung eine Benachrichtigung ausgelöst.

### <a name="view-your-current-cost"></a>Anzeigen Ihrer aktuellen Kosten

Wenn Sie Ihre aktuellen Kosten anzeigen möchten, müssen Sie mithilfe der [Abfrage-API](/rest/api/cost-management/query) einen GET-Aufruf ausführen.

Bei einem GET-Aufruf für die API für Budgets werden nicht die aktuellen, in der Kostenanalyse angezeigten Kosten zurückgegeben. Stattdessen werden Ihre zuletzt ausgewerteten Kosten zurückgegeben.

### <a name="automate-budget-creation"></a>Automatisieren der Budgeterstellung

Die Budgeterstellung kann mithilfe der [API für Budgets](/rest/api/consumption/budgets) automatisiert werden. Alternativ können Sie für die Budgeterstellung auch eine [Budgetvorlage](quick-create-budget-template.md) verwenden. Vorlagen sind eine einfache Methode, um Azure-Bereitstellungen zu standardisieren und gleichzeitig sicherzustellen, dass die Kostenkontrolle ordnungsgemäß konfiguriert ist und erzwungen wird.

### <a name="supported-locales-for-budget-alert-emails"></a>Unterstützte Gebietsschemas für Budgetwarnungs-E-Mails

Mit Budgets werden Sie benachrichtigt, wenn die Kosten einen festgelegten Schwellenwert überschreiten. Sie können bis zu fünf E-Mail-Empfänger pro Budget festlegen. Empfänger erhalten die E-Mail-Warnungen innerhalb von 24 Stunden nach der Überschreitung des Budgetschwellenwerts. Der Empfänger soll jedoch möglicherweise eine E-Mail in einer anderen Sprache erhalten. Sie können die folgenden Sprachkulturcodes mit der Budgets-API verwenden. Legen Sie den Kulturcode mit dem Parameter `locale` ähnlich wie im folgenden Beispiel fest.

```json
{
  "eTag": "\"1d681a8fc67f77a\"",
  "properties": {
    "timePeriod": {
      "startDate": "2020-07-24T00:00:00Z",
      "endDate": "2022-07-23T00:00:00Z"
    },
    "timeGrain": "BillingMonth",
    "amount": 1,
    "currentSpend": {
      "amount": 0,
      "unit": "USD"
    },
    "category": "Cost",
    "notifications": {
      "actual_GreaterThan_10_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 20,
        "locale": "en-us",
        "contactEmails": [
          "user@contoso.com"
        ],
        "contactRoles": [],
        "contactGroups": [],
        "thresholdType": "Actual"
      }
    }
  }
}

```

Von einem Kulturcode unterstützte Sprachen:

| Kulturcode| Sprache |
| --- | --- |
| de-de | Englisch (USA) |
| ja-jp | Japanisch (Japan) |
| zh-cn | Chinesisch (vereinfacht, China) |
| de-DE | Deutsch (Deutschland) |
| es-ES | Spanisch (Spanien, international) |
| fr-FR | Französisch (Frankreich) |
| it-it | Italienisch (Italien) |
| ko-KR | Koreanisch (Korea) |
| pt-br | Portugiesisch (Brasilien) |
| ru-RU | Russisch (Russische Föderation) |
| zh-tw | Chinesisch (traditionell, Taiwan) |
| cs-CZ | Tschechisch (Tschechische Republik) |
| pl-PL | Polnisch (Polen) |
| tr-TR | Türkisch (Türkei) |
| da-DK | Dänisch (Dänemark) |
| en-gb | Walisisch (Großbritannien) |
| hu-hu | Ungarisch (Ungarn) |
| nb-NO | Norwegisch Bokmål (Norwegen) |
| nl-NL | Niederländisch (Niederlande) |
| pt-pt | Portugiesisch (Portugal) |
| sv-SE | Schwedisch (Schweden) |

### <a name="common-budgets-api-configurations"></a>Gängige Konfigurationen für die API für Budgets

Ein Budget in Ihrer Azure-Umgebung kann auf verschiedenste Arten konfiguriert werden. Machen Sie sich zunächst Gedanken zu Ihrem Szenario, und ermitteln Sie anschließend die dafür erforderlichen Konfigurationsoptionen. Folgende Optionen stehen zur Verfügung:

- **Aggregationsintervall**: Gibt den wiederkehrenden Zeitraum für die Kostenerfassung und -auswertung Ihres Budgets an. Die gängigsten Optionen sind „Monatlich“, „Vierteljährlich“ und „Jährlich“.
- **Zeitraum**: Gibt die Gültigkeitsdauer Ihres Budgets an. Die Budgetüberwachung ist nur während des Gültigkeitszeitraums aktiv, und Warnungen werden nur innerhalb dieses Zeitraums ausgegeben.
- **Benachrichtigungen**
  - Kontakt-E-Mail-Adressen: An diese E-Mail-Adressen werden Warnungen gesendet, wenn für ein Budget Kosten anfallen und die definierten Schwellenwerte überschritten werden.
  - Kontaktrollen: Bei dieser Option werden E-Mail-Warnungen an alle Benutzer mit einer entsprechenden Azure-Rolle im angegebenen Bereich gesendet. So können beispielsweise Abonnementbesitzer eine Warnung für ein Budget erhalten, das im Abonnementbereich erstellt wurde.
  - Kontaktgruppen: Bei Überschreitung eines Warnungsschwellenwerts werden vom Budget die konfigurierten Aktionsgruppen aufgerufen.
- **Kostendimensionsfilter**: Die Filtermöglichkeiten, die in der Kostenanalyse oder über die Abfrage-API zur Verfügung stehen, sind auch für Ihr Budget verfügbar. Verwenden Sie diesen Filter, um den Kostenbereich einzuschränken, der durch das Budget überwacht wird.

Nachdem Sie die passenden Budgeterstellungsoptionen für Ihre Anforderungen ermittelt haben, können Sie das Budget mithilfe der API erstellen. Das folgende Beispiel hilft Ihnen bei den ersten Schritten mit einer allgemeinen Budgetkonfiguration.

**Erstellen eines nach mehreren Ressourcen und Tags gefilterten Budgets**

Anforderungs-URL: `PUT https://management.azure.com/subscriptions/{SubscriptionId} /providers/Microsoft.Consumption/budgets/{BudgetName}/?api-version=2019-10-01`

```json
{
  "eTag": "\"1d34d016a593709\"",
  "properties": {
    "category": "Cost",
    "amount": 100.65,
    "timeGrain": "Monthly",
    "timePeriod": {
      "startDate": "2017-10-01T00:00:00Z",
      "endDate": "2018-10-31T00:00:00Z"
    },
    "filter": {
      "and": [
        {
          "dimensions": {
            "name": "ResourceId",
            "operator": "In",
            "values": [
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}",
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}"
            ]
          }
        },
        {
          "tags": {
            "name": "category",
            "operator": "In",
            "values": [
              "Dev",
              "Prod"
            ]
          }
        },
        {
          "tags": {
            "name": "department",
            "operator": "In",
            "values": [
              "engineering",
              "sales"
            ]
          }
        }
      ]
    },
    "notifications": {
      "Actual_GreaterThan_80_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 80,
        "contactEmails": [
          "user1@contoso.com",
          "user2@contoso.com"
        ],
        "contactRoles": [
          "Contributor",
          "Reader"
        ],
        "contactGroups": [
          "/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/microsoft.insights/actionGroups/{actionGroupName}
        ],
        "thresholdType": "Actual"
      }
    }
  }
}
```

### <a name="configure-cost-based-orchestration-for-budget-alerts"></a>Konfigurieren einer kostenbasierten Orchestrierung für Budgetwarnungen

Mithilfe von Azure-Aktionsgruppen können Sie Budgets so konfigurieren, dass durch sie automatisierte Aktionen gestartet werden. Weitere Informationen zur Automatisierung von Aktionen mithilfe von Budgets finden Sie unter [Verwalten von Kosten mit Azure-Budgets](../manage/cost-management-budget-scenario.md).

## <a name="data-latency-and-rate-limits"></a>Datenlatenz und Ratenlimits

Es wird empfohlen, die APIs nicht mehr als einmal pro Tag aufzurufen. Cost Management-Daten werden alle vier Stunden aktualisiert, wenn neue Nutzungsdaten von Azure-Ressourcenanbietern eingehen. Wenn die Aufrufe häufiger durchgeführt werden, wird keine größere Datenmenge bereitgestellt. Stattdessen führt dies zu einer höheren Auslastung. Weitere Informationen dazu, wie oft sich Daten ändern und wie Datenlatenz gehandhabt wird, finden Sie unter [Grundlegendes zu Cost Management-Daten](understand-cost-mgt-data.md).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Fehlercode 429: Aufrufanzahl hat Ratenlimits überschritten

Um allen Cost Management-Abonnenten ein einheitliches Benutzererlebnis zu ermöglichen, gelten für die Cost Management-APIs Ratenlimits. Wenn Sie den Grenzwert erreichen, erhalten Sie den HTTP-Statuscode `429: Too many requests`. Aktuell gelten für unsere APIs die folgenden Durchsatzlimits:

- 30 Aufrufe pro Minute – pro Bereich, pro Benutzer oder Anwendung.
- 200 Aufrufe pro Minute – pro Mandant, pro Benutzer oder Anwendung.

## <a name="next-steps"></a>Nächste Schritte

- [Analysieren der Azure-Kosten mit der Power BI-Vorlagen-App](./analyze-cost-data-azure-cost-management-power-bi-template-app.md)
- [Erstellen und Verwalten von exportierten Daten](./tutorial-export-acm-data.md) mithilfe von Exporten
- Weitere Informationen zur [Nutzungsdetails-API](/rest/api/consumption/usageDetails)
