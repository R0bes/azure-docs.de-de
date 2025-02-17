---
title: Verwalten von Azure-Reservierungen
description: Hier erfahren Sie, wie Sie Azure-Reservierungen verwalten. Informieren Sie sich über Schritte zum Ändern des Reservierungsumfangs, Aufteilen einer Reservierung und Optimieren der Reservierungsnutzung.
ms.service: cost-management-billing
ms.subservice: reservations
author: bandersmsft
ms.reviewer: yashesvi
ms.topic: how-to
ms.date: 06/27/2021
ms.author: banders
ms.openlocfilehash: cee0acf851d82ba09867b8d66c09a17b21e7af45
ms.sourcegitcommit: 1c12bbaba1842214c6578d914fa758f521d7d485
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2021
ms.locfileid: "112989055"
---
# <a name="manage-reservations-for-azure-resources"></a>Verwalten von Reservierungen für Azure-Ressourcen

Nachdem Sie eine Azure-Reservierung erworben haben, müssen Sie möglicherweise die Reservierung auf ein anderes Abonnement anwenden, ändern, wer die Reservierung verwalten kann, oder den Reservierungsumfang ändern. Sie können auch eine Reservierung in zwei Reservierungen aufteilen, um einige der von Ihnen erworbenen Instanzen auf ein anderes Abonnement anzuwenden.

Wenn Sie Azure Reserved Virtual Machine Instances erworben haben, können Sie die Optimierungseinstellung für die Reservierung ändern. Der Reservierungsrabatt kann für virtuelle Computer (Virtual Machines, VMs) in der gleichen Reihe gelten. Sie können aber auch Rechenzentrumskapazität für eine bestimmte VM-Größe reservieren. Sie sollten versuchen, Reservierungen zu optimieren, damit sie vollständig genutzt werden.

*Die zum Verwalten einer Reservierung erforderliche Berechtigung ist von der Abonnementberechtigung getrennt.*

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Reservierungsauftrag und Reservierung

Beim Kauf einer Reservierung werden zwei Objekte erstellt: **Reservierungsauftrag** und **Reservierung**.

Beim Kauf umfasst ein Reservierungsauftrag eine Reservierung. Bei Aktionen wie z. B. Teilung, Zusammenführung, teilweiser Erstattung oder Austausch werden neue Reservierungen unter dem **Reservierungsauftrag** erstellt.

Zum Anzeigen eines Reservierungsauftrags navigieren Sie zu **Reservierungen**, > wählen Sie die Reservierung und dann die **Reservierungsauftrags-ID** aus.

![Beispiel für die Details eines Reservierungsauftrags mit Reservierungsauftrags-ID ](./media/manage-reserved-vm-instance/reservation-order-details.png)

Eine Reservierung erbt die Berechtigungen des entsprechenden Reservierungsauftrags. Zum Tauschen oder Erstatten einer Reservierung muss der Benutzer dem Reservierungsauftrag hinzugefügt werden.

## <a name="change-the-reservation-scope"></a>Ändern des Reservierungsumfangs

 Ihr Reservierungsrabatt gilt für virtuelle Computer, SQL-Datenbank-Instanzen, Azure Cosmos DB oder andere Ressourcen, die mit Ihrer Reservierung übereinstimmen und im Reservierungsumfang ausgeführt werden. Der Abrechnungskontext ist abhängig vom Abonnement, das verwendet wurde, um die Reservierung zu erwerben.

So aktualisieren Sie den Bereich einer Reservierung:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie die Reservierung aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
5. Ändern Sie den Bereich.

Wenn Sie von „Freigegeben“ zu „Einzeln“ wechseln, können Sie nur Abonnements auswählen, deren Besitzer Sie sind. Es können nur Abonnements in demselben Abrechnungskontext wie die Reservierung ausgewählt werden.

Der Bereich gilt nur für einzelne Abonnements mit nutzungsbasierter Zahlung (Angebote MS-AZR-0003P oder MS-AZR-0023P), für das Enterprise-Angebot MS-AZR-0017P oder MS-AZR-0148P oder für CSP-Abonnementtypen.

## <a name="who-can-manage-a-reservation-by-default"></a>Wer kann eine Reservierung standardmäßig verwalten?

Die folgenden Benutzer können Reservierungen standardmäßig anzeigen und verwalten:

- Die Person, die die Reservierung gekauft hat, und der Kontobesitzer für das Abrechnungsabonnement erhalten Azure RBAC-Zugriff auf den Reservierungsauftrag.
-  Abrechnungsmitwirkende für Enterprise Agreement und Microsoft-Kundenvereinbarung können alle Reservierungen verwalten, wenn sie „Cost Management + Billing“ > „Reservierungstransaktionen“ und dann das blaue Banner auswählen.

Damit andere Personen Reservierungen verwalten können, haben Sie zwei Optionen:

- Delegieren Sie die Zugriffsverwaltung für einen einzelnen Reservierungsauftrag, indem Sie einem Benutzer im Ressourcenbereich des Reservierungsauftrags die Rolle „Besitzer“ zuweisen. Wenn Sie eingeschränkten Zugriff gewähren möchten, wählen Sie eine andere Rolle aus.  
     Ausführliche Informationen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

- Hinzufügen eines Benutzers als Abrechnungsadministrator zu einem Konzernvertrag oder einer Microsoft-Kundenvereinbarung:
    - Fügen Sie bei einem Konzernvertrag Benutzer mit der Rolle _Unternehmensadministrator_ hinzu, um alle für diesen Vertrag geltenden Reservierungsaufträge anzuzeigen und zu verwalten. Benutzer mit der Rolle _Unternehmensadministrator (schreibgeschützt)_ können die Reservierung nur anzeigen. Abteilungsadministratoren und Kontobesitzer können Reservierungen nicht anzeigen, _außer_ wenn sie ihnen mithilfe der Zugriffssteuerung (Access Control, IAM) explizit hinzugefügt werden. Weitere Informationen finden Sie unter [Verwalten von Azure Enterprise-Rollen](../manage/understand-ea-roles.md).

        _Unternehmensadministratoren können den Besitz einer Reservierungsreihenfolge übernehmen und andere Benutzer mithilfe der Zugriffssteuerung (Identity & Access Management, IAM) zu einer Reservierung hinzufügen._
    - Bei einer Microsoft-Kundenvereinbarung können Benutzer mit der Rolle „Besitzer des Abrechnungsprofils“ oder „Mitwirkender am Abrechnungsprofil“ alle mit dem Abrechnungsprofil getätigten Reservierungseinkäufe verwalten. Benutzer mit Leseberechtigung für das Abrechnungsprofil und Rechnungs-Manager können alle Reservierungen anzeigen, die für das Abrechnungsprofil bezahlt werden. Sie können allerdings keine Änderungen an den Reservierungen vornehmen.
    Weitere Informationen finden Sie unter [Rollen und Aufgaben für ein Abrechnungsprofil](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="how-billing-administrators-view-or-manage-reservations"></a>Anzeigen oder Verwalten von Reservierungen durch Abrechnungsadministratoren

Führen Sie als Abrechnungsadministrator die folgenden Schritte aus, um alle Reservierungen und Reservierungstransaktionen anzuzeigen und zu verwalten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Kostenverwaltung + Abrechnung**.
    - Wenn Sie EA-Administrator sind, wählen Sie im Menü auf der linken Seite die Option **Abrechnungsbereiche** und dann in der Liste der Abrechnungsbereiche einen Bereich aus.
    - Wenn Sie Besitzer eines Abrechnungsprofils für eine Microsoft-Kundenvereinbarung sind, wählen Sie im linken Menü **Abrechnungsprofile** aus. Wählen Sie in der Liste der Abrechnungsprofile ein Profil aus.
2. Wählen Sie im Menü auf der linken Seite **Produkte und Dienste** > **Reservierungen** aus.
3. Die vollständige Liste der Reservierungen für Ihr EA-Registrierungsprofil oder -Abrechnungsprofil wird angezeigt.
4. Abrechnungsadministratoren können den Besitz einer Reservierung übernehmen, indem sie sie auswählen und dann im angezeigten Fenster die Option **Zugriff gewähren** auswählen.

## <a name="change-billing-subscription-for-an-azure-reservation"></a>Ändern des Abrechnungsabonnements für eine Azure-Reservierung

Das Abrechnungsabonnement kann nach dem Kauf einer Reservierung nicht mehr geändert werden. Wenn Sie das Abonnement ändern möchten, verwenden Sie den Umtauschprozess, um das richtige Abrechnungsabonnement für die Reservierung festzulegen.

## <a name="split-a-single-reservation-into-two-reservations"></a>Aufteilten einer einzelnen Reservierung in zwei Reservierungen

 Wenn Sie mehrere Ressourceninstanzen innerhalb einer Reservierung erworben haben, können Sie Instanzen innerhalb dieser Reservierung verschiedenen Abonnements zuweisen. Standardmäßig verfügen alle Instanzen über einen Bereich: entweder ein Einzelabonnement, eine Ressourcengruppe oder einen freigegebenen Bereich. Beispiel: Sie haben eine Reservierung für zehn VM-Instanzen erworben und den Bereich auf das Abonnement A festgelegt. Jetzt möchten Sie den Bereich für sieben VM-Instanzen in Abonnement A und für die restlichen drei in Abonnement B ändern. Dies ist durch die Aufteilung einer Reservierung möglich. Nach dem Aufteilen einer Reservierung wird die ursprüngliche Reservierungs-ID storniert, und es werden zwei neue Reservierungen erstellt. Die Aufteilung wirkt sich nicht auf die Reservierungsreihenfolge aus. Es erfolgt dadurch keine neue kommerzielle Transaktion, und die neuen Reservierungen haben dasselbe Enddatum wie die aufgeteilte Reservierung.

 Sie können eine Reservierung mithilfe von PowerShell, der CLI oder über die API in zwei Reservierungen aufteilen.

### <a name="split-a-reservation-by-using-powershell"></a>Aufteilen einer Reservierung mithilfe von PowerShell

1. Rufen Sie die Reservierungsauftrags-ID ab, indem Sie den folgenden Befehl ausführen:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. Abrufen der Details einer Reservierung:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Aufteilen der Reservierung in zwei Reservierungen und Verteilen der Instanzen:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Sie können den Bereich aktualisieren, indem Sie den folgenden Befehl ausführen:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](exchange-and-refund-azure-reservations.md).

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Ändern der Optimierungseinstellung für reservierte VM-Instanzen

 Wenn Sie eine reservierte VM-Instanz erwerben, wählen Sie die Flexibilität bei der Instanzgröße oder die Kapazitätspriorität aus. Bei Auswahl der Flexibilität bei der Instanzgröße wird der Reservierungsrabatt auf weitere VMs in der [Gruppe mit VMs derselben Größe](../../virtual-machines/reserved-vm-instance-size-flexibility.md) angewendet. Die Kapazitätspriorität bestimmt die Rechenzentrumskapazität, die für Ihre Bereitstellungen am wichtigsten ist. Diese Option bietet Ihnen zusätzliche Sicherheit, dass die VM-Instanzen gestartet werden können, wenn sie benötigt werden.

Wenn der Reservierungsumfang „Freigegeben“ lautet, ist die Flexibilität bei der Instanzgröße standardmäßig aktiviert. Die Rechenzentrumskapazität wird bei VM-Bereitstellungen nicht priorisiert.

Für Reservierungen, bei denen der Umfang auf Einzelabonnements festgelegt ist, können Sie die Reservierung anstelle von Flexibilität bei der VM-Instanzgröße im Hinblick auf die Kapazitätspriorität optimieren.

Gehen Sie wie folgt vor, um die Optimierungseinstellung für die Reservierung zu aktualisieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie die Reservierung aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
  ![Beispiel für das Konfigurationselement](./media/manage-reserved-vm-instance/add-product03.png)
5. Ändern Sie die Einstellung **Optimiert für**.
  ![Beispiel für die Einstellung „Optimiert für“](./media/manage-reserved-vm-instance/instance-size-flexibility-option.png)

## <a name="optimize-reservation-use"></a>Optimieren der Reservierungsnutzung

Einsparungen durch Azure-Reservierungen ergeben sich nur bei anhaltender Ressourcennutzung. Wenn Sie eine Reservierung erwerben, bezahlen Sie für die voraussichtliche Ressourcennutzung in einem Zeitraum von einem Jahr oder drei Jahren. Versuchen Sie, Ihre Reservierung zu maximieren, um den größtmöglichen Nutzen und die höchstmöglichen Einsparungen zu erzielen. In den folgenden Abschnitten erfahren Sie, wie Sie eine Reservierung überwachen und deren Nutzung optimieren.

### <a name="view-reservation-use-in-the-azure-portal"></a>Anzeigen der Reservierungsnutzung im Azure-Portal

Die Reservierungsnutzung kann unter anderem im Azure-Portal angezeigt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Alle Dienste** > [**Reservierungen**](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) aus, und sehen Sie sich den Wert unter **Nutzung (%)** für eine Reservierung an.  
  ![Abbildung: Reservierungsliste](./media/manage-reserved-vm-instance/reservation-list.png)
3. Wählen Sie eine Reservierung aus.
4. Überprüfen Sie den Nutzungstrend für die Reservierung im Zeitverlauf.
  ![Abbildung der Reservierungsnutzung](./media/manage-reserved-vm-instance/reservation-utilization-trend.png)

### <a name="view-reservation-use-with-api"></a>Anzeigen der Reservierungsnutzung per API

EA-Kunden (Enterprise Agreement) können programmgesteuert anzeigen, wie die Reservierungen in ihrer Organisation genutzt werden. Die ungenutzte Reservierung erhalten Sie über Nutzungsdaten. Beachten Sie bei der Betrachtung der Reservierungsgebühren, dass die Daten in Ist-Kosten und amortisierte Kosten aufgeteilt werden. Ist-Kosten liefern Daten für die Abstimmung Ihrer monatlichen Rechnung. Diese Daten enthalten auch Details zu den Kosten für den Reservierungserwerb sowie zur Reservierungsanwendung. Amortisierte Kosten ähneln Ist-Kosten, mit dem Unterschied, dass der effektive Preis für die Reservierungsnutzung anteilig berechnet wird. Ungenutzte Reservierungsstunden werden in amortisierten Kostendaten angezeigt. Weitere Informationen zu Nutzungsdaten für EA-Kunden finden Sie unter [Abrufen von Reservierungskosten und Nutzung laut Enterprise Agreement](understand-reserved-instance-usage-ea.md).

Verwenden Sie für andere Arten von Abonnements die API [Reservierungszusammenfassungen – Liste nach Reservierungsauftrag und Reservierung](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation).

### <a name="optimize-your-reservation"></a>Optimieren Ihrer Reservierung

Sollten Sie feststellen, dass die Reservierungen Ihrer Organisation zu wenig genutzt werden, haben Sie folgende Möglichkeiten:

- Vergewissern Sie sich, dass die virtuellen Computer, die für Ihre Organisation erstellt werden, der VM-Größe für die Reservierung entsprechen.
- Vergewissern Sie sich, dass Instanzgrößenflexibilität aktiviert ist. Weitere Informationen finden Sie unter [Verwalten von Reservierungen – Ändern der Optimierungseinstellung für reservierte VM-Instanzen](#change-optimize-setting-for-reserved-vm-instances).
- Ändern Sie den Umfang der Reservierung in _Freigegeben_, damit sie umfassender gilt. Weitere Informationen finden Sie unter [Ändern des Reservierungsumfangs](#change-the-reservation-scope).
- Tauschen Sie die ungenutzte Menge ggf. um. Weitere Informationen finden Sie unter [Stornieren, Umtauschen oder Rückerstatten von Reservierungen](#cancel-exchange-or-refund-reservations).


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
 - [Anzeigen der Reservierungsnutzung](reservation-utilization.md)
 - [Umtausch und Rückerstattung](exchange-and-refund-azure-reservations.md)
 - [Verlängern von Reservierungen](reservation-renew.md)
 - [Übertragungen zwischen Mandanten](troubleshoot-reservation-transfers-between-tenants.md)
 - [Ermitteln eines Reservierungskäufers über Azure-Protokolle](find-reservation-purchaser-from-logs.md)
