---
title: Anpassen von Warnungsdetails in Azure Sentinel | Microsoft-Dokumentation
description: Passen Sie basierend auf dem Inhalt der Warnungen den Namen und die Beschreibung von Warnungen sowie ihren Schweregrad und zugewiesene Taktiken an.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2021
ms.author: yelevin
ms.openlocfilehash: 0b6d64167451b3c7363d6bd80c5fafcd67b6fc5c
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339706"
---
# <a name="customize-alert-details-in-azure-sentinel"></a>Anpassen von Warnungsdetails in Azure Sentinel 

> [!IMPORTANT]
>
> - Das Feature für Warnungsdetails befindet sich in der **VORSCHAUPHASE**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="introduction"></a>Einführung

Wenn Sie einen Namen und eine Beschreibung für Ihre geplanten Analyseregeln definieren und ihnen Schweregrade und MITRE ATT&CK-Taktiken zuweisen, werden alle von einer bestimmten Regel generierten Warnungen – und alle daraus erstellten Incidents – mit dem gleichen Namen, der gleichen Beschreibung usw. angezeigt, ohne den jeweiligen Inhalt einer bestimmten Instanz der Warnung zu berücksichtigen.

Mit dem Feature **Warnungsdetails** können Sie die Darstellung einer Warnung an den Inhalt anpassen. Hier können Sie Parameter in Ihrer Warnung auswählen, die im Namen oder in der Beschreibung der einzelnen Warnungsinstanzen dargestellt werden oder die die Taktik und den Schweregrad enthalten können, die bzw. der dieser Warnungsinstanz zugewiesen ist. Wenn der Parameter keinen Wert (oder für Taktik und Schweregrad einen ungültigen Wert) aufweist, werden die Warnungsdetails auf die Standardwerte zurückgesetzt, die auf der ersten Seite des Assistenten angegeben sind.

Das unten beschriebene Verfahren ist Teil des Assistenten zum Erstellen von Analyseregeln. Es wird hier separat beschrieben, um das Szenario für das Hinzufügen oder Ändern von Warnungsdetails in einer vorhandenen Analyseregel zu erläutern.

## <a name="how-to-customize-alert-details"></a>Anpassen von Warnungsdetails

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Analysen** aus.

1. Wählen Sie eine geplante Abfrageregel aus, und klicken Sie auf **Bearbeiten**. Oder erstellen Sie eine neue Regel, indem Sie oben auf dem Bildschirm auf **Erstellen > Geplante Abfrageregel** klicken.

1. Klicken Sie auf die Registerkarte **Regellogik festlegen**.

1. Erweitern Sie im Abschnitt **Warnungsanreicherung (Vorschau)** den Bereich **Warnungsdetails**.

    :::image type="content" source="media/customize-alert-details/alert-enrichment.png" alt-text="Anpassen von Warnungsdetails":::

1. Fügen Sie im nun erweiterten Abschnitt **Warnungsdetails** Freitext mit Parametern hinzu, die den Details entsprechen, die Sie in der Warnung anzeigen möchten:

    1. Geben Sie im Feld **Format des Warnungsnamens** den Text ein, der als Name der Warnung (Warnungstext) angezeigt werden soll, und fügen Sie in doppelten geschweiften Klammern alle Parameter ein, die Teil des Warnungstexts sein sollen.

        Beispiel: `Alert from {{ProviderName}}: {{AccountName}} failed to log on to computer {{ComputerName}} with IP address {{IPAddress}}.`

    1. Gehen Sie beim Feld **Format der Warnungsbeschreibung** genauso vor.
    
    1. Verwenden Sie die Felder **Spalte "Taktik"** und **Spalte "Schweregrad"** nur, wenn die Abfrageergebnisse Spalten mit diesen Informationen enthalten. Wählen Sie für jedes Feld die Spalte aus, die die entsprechenden Informationen enthält.

    Wenn Sie Ihre Meinung ändern oder einen Fehler gemacht haben, können Sie ein Warnungsdetail entfernen, indem Sie auf das Papierkorbsymbol neben den Feldern **Spalte "Taktik"/Spalte "Schweregrad"** klicken oder den freien Text aus den Feldern **Format des Warnungsnamen/Format der Warnungsbeschreibung** löschen.

1. Wenn die Anpassung Ihrer Warnungsdetails abgeschlossen ist, fahren Sie mit der nächsten Registerkarte im Assistenten fort. Wenn Sie eine vorhandene Regel bearbeiten, klicken Sie auf die Registerkarte **Überprüfen und erstellen**. War die Regelüberprüfung erfolgreich, klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Warnungsdetails in Azure Sentinel-Analyseregeln anpassen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md) können Sie sich ein Gesamtbild machen.
- Erfahren Sie mehr über [Entitäten in Azure Sentinel](entities-in-azure-sentinel.md).
