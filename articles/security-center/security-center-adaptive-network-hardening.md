---
title: Adaptive Netzwerkhärtung in Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie anhand echter Datenverkehrsmuster Ihre NSG-Regeln (Netzwerksicherheitsgruppen) härten und den Sicherheitsstatus weiter verbessern.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 05/26/2021
ms.author: memildin
ms.openlocfilehash: 7bb65cf7e63dcb6f31eef73ccb3ad3186a56f0c5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346441"
---
# <a name="improve-your-network-security-posture-with-adaptive-network-hardening"></a>Verbessern des Netzwerksicherheitsstatus mit adaptiver Netzwerkhärtung

Die adaptive Netzwerkhärtung ist ein Azure Security Center-Feature ohne Agent. Es muss nichts auf Ihren Computern installiert werden, um von diesem Netzwerkhärtungstool zu profitieren.

Auf dieser Seite wird das Konfigurieren und Verwalten der adaptiven Netzwerkhärtung in Security Center beschrieben.

## <a name="availability"></a>Verfügbarkeit
|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|Erfordert [Azure Defender für Server](defender-for-servers-introduction.md)|
|Erforderliche Rollen und Berechtigungen:|Schreibberechtigungen für die NSGs des Computers|
|Clouds:|:::image type="icon" source="./media/icons/yes-icon.png"::: Kommerzielle Clouds<br>:::image type="icon" source="./media/icons/no-icon.png"::: National/Sovereign (Azure Government, Azure China 21Vianet)|
|||

## <a name="what-is-adaptive-network-hardening"></a>Was ist die adaptive Netzwerkhärtung?
Der Einsatz von [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/network-security-groups-overview.md) zur Filterung von ein- und ausgehendem Datenverkehr für Ressourcen verbessert den Sicherheitsstatus Ihres Netzwerks. Es gibt jedoch Situationen, in denen es sich bei dem Datenverkehr, der die NSG durchläuft, um eine Teilmenge der definierten NSG-Regeln handelt. In diesen Fällen lässt sich der Sicherheitsstatus durch eine Härtung der NSG-Regeln auf der Grundlage tatsächlicher Datenverkehrsmuster noch weiter verbessern.

Die adaptive Netzwerkhärtung liefert Empfehlungen zur weiteren Härtung der NSG-Regeln. Dabei kommt ein Machine Learning-Algorithmus zum Einsatz, der Faktoren wie tatsächlichen Datenverkehr, bekannte vertrauenswürdige Konfiguration und Bedrohungsinformationen sowie weitere Anzeichen einer Kompromittierung berücksichtigt und auf der Grundlage dieser Faktoren Empfehlungen abgibt, um nur Datenverkehr von bestimmten IP-/Port-Tupeln zuzulassen.

Ein Beispiel: Angenommen, die vorhandene NSG-Regel lässt Datenverkehr von 140.20.30.10/24 an Port 22 zu. Basierend auf der Analyse des Datenverkehrs empfiehlt die Netzwerkhärtung möglicherweise eine Einschränkung des Bereichs, um den Datenverkehr von 140.23.30.10/29 zuzulassen und sämtlichen weiteren Datenverkehr an diesem Port zu verweigern. Die vollständige Liste der unterstützten Ports finden Sie in den häufig gestellten Fragen unter [Welche Ports werden unterstützt?](#which-ports-are-supported)

## <a name="view-hardening-alerts-and-recommended-rules"></a>Anzeigen von Härtungswarnungen und empfohlenen Regeln

1. Öffnen Sie im Menü von Security Center das Dashboard **Azure Defender**, und wählen Sie die Kachel für die adaptive Netzwerkhärtung (1) oder das Insights-Panelelement für die adaptive Netzwerkhärtung (2) aus. 

    :::image type="content" source="./media/security-center-adaptive-network-hardening/traffic-hardening.png" alt-text="Zugriff auf die Tools für die adaptive Netzwerkhärtung." lightbox="./media/security-center-adaptive-network-hardening/traffic-hardening.png":::

    > [!TIP]
    > Das Insights-Panel zeigt den Prozentsatz Ihrer VMs, die derzeit mit adaptiver Netzwerkhärtung geschützt sind. 

1. Die Detailseite für die Empfehlung **Auf VMs mit Internetausrichtung müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden** wird geöffnet. Ihre Netzwerk-VMs sind auf drei Registerkarten gruppiert:
   * **Fehlerhafte Ressourcen:** Virtuelle Computer, für die Empfehlungen und Warnungen vorliegen, die durch Ausführen des Algorithmus der adaptiven Netzwerkhärtung ausgelöst wurden. 
   * **Fehlerfreie Ressourcen:** Virtuelle Computer ohne Warnungen oder Empfehlungen.
   * **Nicht überprüfte Ressourcen:** Virtuelle Computer, auf denen der Algorithmus der adaptiven Netzwerkhärtung aus einem der folgenden Gründe nicht ausgeführt werden kann:
      * **Virtuelle Computer sind klassische virtuelle Computer**: Es werden nur virtuelle Azure Resource Manager-Computer unterstützt.
      * **Nicht genügend Daten verfügbar:** Security Center benötigt mindestens Datenverkehrsdaten eines Zeitraums von 30 Tagen, um fundierte Härtungsempfehlungen abgeben zu können.
      * **Virtueller Computer nicht durch Azure Defender geschützt:** Nur virtuelle Computer, die mit [Azure Defender für Server](defender-for-servers-introduction.md) geschützt sind, können dieses Feature nutzen.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/recommendation-details-page.png" alt-text="Detailseite der Empfehlung „Auf VMs mit Internetausrichtung müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden“.":::

1. Wählen Sie auf der Registerkarte **Fehlerhafte Ressourcen** einen virtuellen Computer aus, um dessen Warnungen und die empfohlenen Härtungsregeln anzuzeigen.

    - Auf der Registerkarte **Regeln** werden die Regeln aufgeführt, die laut adaptiver Netzwerkhärtung hinzugefügt werden sollten.
    - Auf der Registerkarte **Warnungen** werden die Warnungen aufgeführt, die aufgrund von eingehendem Datenverkehr für die Ressource generiert wurden, der nicht innerhalb des durch die empfohlenen Regeln zugelassenen IP-Adressbereichs liegt.

1. Optional können Sie die Regeln bearbeiten:

    - [Ändern einer Regel](#modify-rule)
    - [Löschen einer Regel](#delete-rule) 
    - [Regel hinzufügen](#add-rule)

3. Wählen Sie die Regeln, die Sie auf die NSG anwenden möchten, und dann **Erzwingen** aus.

    > [!TIP]
    > Wenn die zulässigen Quell-IP-Adressbereiche als „Keine“ angezeigt werden, bedeutet dies, dass die empfohlene Regel eine *Ablehnungsregel* ist. Andernfalls handelt es sich um eine *Zulassungsregel*.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/hardening-alerts.png" alt-text="Verwalten der Regeln der adaptiven Netzwerkhärtung.":::

      > [!NOTE]
      > Die erzwungenen Regeln werden den Netzwerksicherheitsgruppen hinzugefügt, die den virtuellen Computer schützen. (Ein virtueller Computer kann durch eine Netzwerksicherheitsgruppe geschützt werden, die der Schnittstellenkarte zugeordnet ist, oder das Subnetz, in dem der virtuelle Computer befindet, oder beides.)

## <a name="modify-a-rule"></a>Ändern einer Regel <a name ="modify-rule"> </a>

Die Parameter einer empfohlenen Regel können bei Bedarf geändert werden. So können Sie beispielsweise die empfohlenen IP-Adressbereiche ändern.

Im Folgenden finden Sie einige wichtige Richtlinien, die Sie beim Ändern einer Regel der adaptiven Netzwerkhärtung berücksichtigen sollten:

- Regeln können nicht von **Zulassen** in **Verweigern** geändert werden. 

- Es können nur die Parameter von Regeln des Typs **Zulassen** geändert werden. 

    Das Erstellen und Ändern von Verweigerungsregeln erfolgt direkt in der NSG. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md).

- Die einzige Regel vom Typ „Verweigern“, die hier ggf. angezeigt wird, ist eine Regel zum **Verweigern des gesamten Datenverkehrs**, und diese Regel kann nicht geändert werden. Sie kann allerdings gelöscht werden (siehe [Löschen einer Regel](#delete-rule)). Informationen zu dieser Art von Regel finden Sie in den häufig gestellten Fragen unter [Wann sollte ich eine Regel zum Verweigern des gesamten Datenverkehrs verwenden?](#when-should-i-use-a-deny-all-traffic-rule)

So ändern Sie eine Regel der adaptiven Netzwerkhärtung:

1. Um Parameter einer Regel zu ändern, wählen Sie auf der Registerkarte **Regeln** die drei Punkte (...) am Ende der Zeile der gewünschten Regel und anschließend **Bearbeiten** aus.

   ![Regel bearbeiten.](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. Aktualisieren Sie im Fenster **Regel bearbeiten** die Details, die Sie ändern möchten, und wählen Sie anschließend **Speichern** aus.

   > [!NOTE]
   > Nach dem Auswählen von **Speichern** haben Sie die Regel erfolgreich geändert. *Allerdings haben Sie sie noch nicht auf die NSG angewendet.* Um sie anzuwenden, müssen Sie die Regel in der Liste auswählen und **Erzwingen** auswählen (wie im nächsten Schritt beschrieben).

   ![„Speichern“ auswählen.](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Wählen Sie in der Liste die aktualisierte Regel und dann **Erzwingen** aus, um die aktualisierte Regel anzuwenden.

    ![Regel erzwingen.](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

## <a name="add-a-new-rule"></a>Hinzufügen einer neuen Rolle <a name ="add-rule"> </a>

Sie können eine Regel vom Typ „Zulassen“ hinzufügen, die nicht von Security Center empfohlen wurde.

> [!NOTE]
> Nur Regeln vom Typ „Zulassen“ können hier hinzugefügt werden. Regeln vom Typ „Verweigern“ können direkt in der Netzwerksicherheitsgruppe hinzugefügt werden. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md).

So fügen Sie eine Regel der adaptiven Netzwerkhärtung hinzu:

1. Wählen Sie in der Symbolleiste oben die Option **Regel hinzufügen** aus.

   ![Regel hinzufügen.](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. Geben Sie im Fenster **Neue Regel** die Details ein, und wählen Sie **Hinzufügen** aus.

   > [!NOTE]
   > Nach dem Auswählen von **Hinzufügen** haben Sie die Regel erfolgreich hinzugefügt, und sie wird zusammen mit den anderen empfohlenen Regeln aufgeführt. Allerdings haben Sie sie noch nicht auf die NSG *angewendet*. Um sie zu aktivieren, müssen Sie die Regel in der Liste auswählen und dann **Erzwingen** auswählen (wie im nächsten Schritt beschrieben).

3. Wählen Sie in der Liste die neue Regel und dann **Erzwingen** aus, um die neue Regel anzuwenden.

    ![Regel erzwingen.](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


## <a name="delete-a-rule"></a>Löschen einer Regel <a name ="delete-rule"> </a>

Bei Bedarf können Sie eine empfohlene Regel für die aktuelle Sitzung löschen. Dies kann beispielsweise erforderlich sein, wenn Sie feststellen, dass eine vorgeschlagene Regel die Blockierung von zulässigem Datenverkehr zur Folge hätte.

So löschen Sie eine Regel der adaptiven Netzwerkhärtung für Ihre aktuelle Sitzung:

- Wählen Sie auf der Registerkarte **Regeln** die drei Punkte (...) am Ende der Zeile der Regel und anschließend **Löschen** aus.  

    ![Regel löschen.](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)


## <a name="faq---adaptive-network-hardening"></a>Häufig gestellte Fragen – Adaptive Netzwerkhärtung

- [Welche Ports werden unterstützt?](#which-ports-are-supported)
- [Welche Voraussetzungen oder VM-Erweiterungen sind für die adaptive Netzwerkhärtung erforderlich?](#are-there-any-prerequisites-or-vm-extensions-required-for-adaptive-network-hardening)

### <a name="which-ports-are-supported"></a>Welche Ports werden unterstützt?

Empfehlungen der adaptiven Netzwerkhärtung werden nur an den folgenden Ports unterstützt (sowohl für UDP als auch für TCP): 

13, 17, 19, 22, 23, 53, 69, 81, 111, 119, 123, 135, 137, 138, 139, 161, 162, 389, 445, 512, 514, 593, 636, 873, 1433, 1434, 1900, 2049, 2301, 2323, 2381, 3268, 3306, 3389, 4333, 5353, 5432, 5555, 5800, 5900, 5900, 5985, 5986, 6379, 6379, 7000, 7001, 7199, 8081, 8089, 8545, 9042, 9160, 9300, 11211, 16379, 26379, 27017, 37215

### <a name="are-there-any-prerequisites-or-vm-extensions-required-for-adaptive-network-hardening"></a>Welche Voraussetzungen oder VM-Erweiterungen sind für die adaptive Netzwerkhärtung erforderlich?

Die adaptive Netzwerkhärtung ist ein Azure Security Center-Feature ohne Agent. Es muss nichts auf Ihren Computern installiert werden, um von diesem Netzwerkhärtungstool zu profitieren.

### <a name="when-should-i-use-a-deny-all-traffic-rule"></a>Wann sollte ich eine Regel zum Verweigern des gesamten Datenverkehrs verwenden?

Eine Regel zum **Verweigern des gesamten Datenverkehrs** wird empfohlen, wenn Security Center nach Ausführung des Algorithmus keinen Datenverkehr erkennt, der zugelassen werden soll (basierend auf der vorhandenen NSG-Konfiguration). Aus diesem Grund ist die empfohlene Regel, jeglichen Datenverkehr für den angegebenen Port zu verweigern. Der Name dieser Art von Regel wird als *Vom System generiert* angezeigt. Nach Erzwingung dieser Regel ist ihr tatsächlicher Name in der NSG eine Zeichenfolge, die sich aus dem Protokoll, der Datenverkehrsrichtung, „DENY“ und einer Zufallszahl zusammensetzt.