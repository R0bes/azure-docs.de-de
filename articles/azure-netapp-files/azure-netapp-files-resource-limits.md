---
title: Ressourceneinschränkungen für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt Limits für Azure NetApp Files-Ressourcen und wie eine Erhöhung des Ressourcenlimits angefordert wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2021
ms.author: b-juche
ms.openlocfilehash: 06be68fb1de224bbbcad13e71e7f4069e44f8309
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122354876"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ressourcenlimits für Azure NetApp Files

Grundlegende Informationen zu Ressourceneinschränkungen für Azure NetApp Files unterstützen Sie bei der Verwaltung Ihrer Volumes.

## <a name="resource-limits"></a>Ressourceneinschränkungen

In der folgenden Tabelle werden die Ressourcengrenzwerte für Azure NetApp Files beschrieben:

|  Resource  |  Standardlimit  |  Über Supportanfrage anpassbar  |
|----------------|---------------------|--------------------------------------|
|  [Regionales Kapazitätskontingent pro Abonnement](#regional-capacity-quota)   |  25 TiB  |  Ja  |
|  Anzahl von NetApp-Konten pro Azure-Region und Abonnement  |  10    |  Ja   |
|  Anzahl der Kapazitätspools pro NetApp-Konto   |    25     |   Ja   |
|  Anzahl von Volumes pro Abonnement   |    500     |   Ja   |
|  Anzahl der Volumes pro Kapazitätspool     |    500   |    Ja     |
|  Anzahl von Momentaufnahmen pro Volume       |    255     |    Nein        |
|  Anzahl der an Azure NetApp Files (Microsoft.NetApp/volumes) delegierten Subnetze pro virtuellem Azure-Netzwerk    |   1   |    Nein    |
|  Anzahl verwendeter IP-Adressen innerhalb eines VNets (einschließlich VNets mit sofortigem Peering) mit Azure NetApp Files   |    1000   |    Nein   |
|  Mindestgröße eines einzelnen Kapazitätspools   |  4 TiB     |    Nein  |
|  Maximale Größe eines einzelnen Kapazitätspools    |  500 TiB   |   Nein   |
|  Mindestgröße eines einzelnen Volumes    |    100 GB    |    Nein    |
|  Maximale Größe eines einzelnen Volumes     |    ca. 100 TiB    |    Nein    |
|  Maximale Größe einer einzelnen Datei     |    16 TiB    |    Nein    |    
|  Maximale Größe der Verzeichnismetadaten in einem einzelnen Verzeichnis      |    320 MB    |    Nein    |    
|  Maximale Anzahl an Dateien in einem einzelnen Verzeichnis  | *Ca.* 4 Millionen. <br> Siehe [Bestimmen, ob sich ein Verzeichnis der Begrenzungsgröße nähert](#directory-limit).  |    Nein    |   
|  Maximale Anzahl von Dateien ([maxfiles](#maxfiles)) pro Volume     |    100 Mio.    |    Ja    |    
|  Maximale Anzahl von Exportrichtlinienregeln pro Volume     |    5  |    Nein    | 
|  Minimaler zugewiesener Durchsatz für ein Volume für manuelle QoS     |    1 MiB/s   |    Nein    |    
|  Maximaler zugewiesener Durchsatz für ein Volume für manuelle QoS     |    4\.500 MiB/s    |    Nein    |    
|  Anzahl der regionsübergreifenden Datenschutzvolumes für die regionsübergreifende Replikation (Zielvolumes)     |    5    |    Ja    |     

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zur Kapazitätsverwaltung](azure-netapp-files-faqs.md#capacity-management-faqs).

## <a name="determine-if-a-directory-is-approaching-the-limit-size"></a>Feststellen, ob sich ein Verzeichnis der Begrenzungsgröße nähert <a name="directory-limit"></a>  

Sie können den Befehl `stat` von einem Client aus verwenden, um festzustellen, ob sich ein Verzeichnis der maximal zulässigen Größe für Verzeichnismetadaten (320 MB) nähert.   

Bei einem Verzeichnis mit 320 MB beträgt die Anzahl der Blöcke 655.360, wobei jeder Block eine Größe von 512 Bytes hat.  (Berechnung: 320 · 1.024 · 1.024 : 512.) Daraus ergibt sich für ein Verzeichnis mit 320 MB ein Maximum von etwa vier Millionen Dateien. Die tatsächliche maximale Anzahl von Dateien ist jedoch ggf. geringer. Dies hängt von Faktoren wie etwa der Anzahl von Dateien mit ASCII-fremden Zeichen im Verzeichnis ab. Verwenden Sie daher den Befehl `stat` wie folgt, um zu ermitteln, ob der Grenzwert Ihres Verzeichnisses bald erreicht ist.  

Beispiele:

```console
[makam@cycrh6rtp07 ~]$ stat bin
File: 'bin'
Size: 4096            Blocks: 8          IO Block: 65536  directory

[makam@cycrh6rtp07 ~]$ stat tmp
File: 'tmp'
Size: 12288           Blocks: 24         IO Block: 65536  directory
 
[makam@cycrh6rtp07 ~]$ stat tmp1
File: 'tmp1'
Size: 4096            Blocks: 8          IO Block: 65536  directory
```

## <a name="maxfiles-limits"></a>Maxfiles-Limits <a name="maxfiles"></a> 

Azure NetApp Files-Volumes besitzen ein Limit namens *maxfiles*. Das maxfiles-Limit ist die Anzahl von Dateien, die ein Volume enthalten kann. Linux-Dateisysteme beziehen sich auf das Limit als *I-Knoten*. Das maxfiles-Limit für ein Azure NetApp Files-Volume wird basierend auf der Größe (dem Kontingent) des Volumes indiziert. Das maxfiles-Limit für ein Volume erhöht oder verringert sich mit einer Rate von 20 Millionen Dateien pro TiB bereitgestellter Volumegröße. 

Der Dienst passt das maxfiles-Limit für ein Volume basierend auf seiner bereitgestellten Größe dynamisch an. Beispielsweise hätte ein Volume, das anfänglich mit einer Größe von 1 TiB konfiguriert wurde, ein maxfiles-Limit von 20 Millionen. Nachfolgende Änderungen an der Größe des Volumes führten zu einer automatischen Neuanpassung des maxfiles-Limits basierend auf den folgenden Regeln: 

|    Volumegröße (Kontingent)     |  Automatische Neuanpassung des maxfiles-Limits    |
|----------------------------|-------------------|
|    <= 1 TiB                |    20 Mio.     |
|    > 1 TiB, aber <= 2 TiB    |    40 Mio.     |
|    > 2 TiB, aber <= 3 TiB    |    60 Mio.     |
|    > 3 TiB, aber <= 4 TiB    |    80 Mio.     |
|    > 4 TiB                 |    100 Mio.    |

Wenn Sie für ein Volume bereits ein Kontingent von mindestens 4 TiB zugeordnet haben, können Sie eine [Supportanfrage](#limit_increase) initiieren, um das maxfiles-Limit (I-Knoten) auf über 100 Millionen zu erhöhen. Für jede Erhöhung um 100 Millionen Dateien (oder einen Bruchteil davon) müssen Sie das entsprechende Volumekontingent um 4 TiB erhöhen.  Wenn Sie z. B. den Grenzwert „maxfiles“ von 100 Millionen Dateien auf 200 Millionen Dateien (oder eine beliebige Zahl dazwischen) erhöhen, müssen Sie das Volumenkontingent von 4 TiB auf 8 TiB erhöhen.

Sie können das maxfiles-Limit auf 500 Millionen erhöhen, wenn das Volumenkontingent mindestens 20 TiB beträgt. <!-- ANF-11854 --> 

## <a name="regional-capacity-quota"></a>Regionales Kapazitätskontingent

Azure NetApp Files verfügt über ein regionales Limit, basierend auf der Kapazität. Das Standardkapazitätslimit für jedes Abonnement beträgt 25 TiB pro Region auf allen Servicelevels.   

Sie können eine Kapazitätserhöhung anfordern, indem Sie wie folgt ein spezifisches Supportticket für **Dienst- und Abonnementlimits (Kontingente)** übermitteln:

1. Wechseln Sie im Portal zu **Support und Problembehandlung**, um den Supportanfrageprozess zu starten:  

    ![Screenshot mit dem Menü „Support und Problembehandlung“.](../media/azure-netapp-files/support-troubleshoot-menu.png)   

2.  Wählen Sie den Problemtyp **Dienst- und Abonnementlimits (Kontingente)** aus, und geben Sie alle relevanten Details ein:

    ![Screenshot mit dem Menü „Dienst- und Abonnementlimits“.](../media/azure-netapp-files/service-subscription-limits-menu.png)   

3. Klicken Sie auf der Registerkarte „Details“ auf den Link **Details eingeben**, und wählen Sie dann den Kontingenttyp **TiBs pro Abonnement** aus:   

    ![Screenshot mit dem Link „Details eingeben“ auf der Registerkarte „Details“.](../media/azure-netapp-files/support-details.png)   

    ![Screenshot mit dem Fenster „Kontingentdetails“.](../media/azure-netapp-files/support-quota-details.png)   

4.  Wählen Sie auf der Seite „Supportmethode“ die Option **Schweregrad B – Mittlere Auswirkung** aus:  

    ![Screenshot mit dem Fenster „Supportmethode“.](../media/azure-netapp-files/support-method-severity.png)   

5. Schließen Sie den Anforderungsprozess ab, um die Anfrage abzusenden. 
 
Nachdem das Ticket übermittelt wurde, wird die Anforderung zur Verarbeitung an das Azure-Kapazitätsverwaltungsteam gesendet. Sie erhalten in der Regel innerhalb von zwei Arbeitstagen eine Antwort. Bei der Bearbeitung großer Anforderungen wendet sich das Azure-Kapazitätsverwaltungsteam möglicherweise an Sie.
 
Eine Erhöhung des regionalen Kapazitätskontingents führt nicht zu einer Erhöhung der Abrechnung. Die Abrechnung basiert weiterhin auf den bereitgestellten Kapazitätspools.

## <a name="request-limit-increase"></a>Anfordern einer Limiterhöhung <a name="limit_increase"></a> 

Sie können eine Supportanfrage an den Azure-Support stellen, um die anpassbaren Grenzwerte aus der Tabelle [Ressourcenlimits](#resource-limits) zu erhöhen. 

Über den Navigationsbereich im Azure-Portal: 

1. Klicken Sie auf **Hilfe und Support**.
2. Klicken Sie auf **+ Neue Supportanfrage**.
3. Geben Sie die folgenden Informationen auf der Registerkarte „Grundeinstellungen“ an: 
    1. Problemtyp: Wählen Sie **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
    2. Abonnements: Wählen Sie das Abonnement für die Ressource aus, deren Kontingente erhöht werden sollen.
    3. Kontingenttyp: Wählen Sie **Storage: Azure NetApp Files limits** (Speicher: Azure NetApp Files-Grenzwerte) aus.
    4. Klicken Sie auf **Weiter: Lösungen**.
4. Auf der Registerkarte „Details“:
    1. Geben Sie die folgenden Informationen für den entsprechenden Ressourcentyp im Feld „Beschreibung“ an:

        |  Resource  |    Übergeordnete Ressourcen      |    Angeforderte neue Grenzwerte     |    Grund für die Kontingenterhöhung       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Konto |  *Abonnement-ID*   |  *Angeforderte neue maximale Anzahl an **Konten***    |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Pool    |  *Abonnement-ID, NetApp-Konto-URI*  |  *Angeforderte neue maximale Anzahl an **Pools***   |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Volume  |  *Abonnement-ID, NetApp-Konto-URI, Kapazitätspool-URI*   |  *Angeforderte neue maximale Anzahl an **Volumes***     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Maxfiles  |  *Abonnement-ID, NetApp-Konto-URI, Kapazitätspool-URI, Volume-URI*   |  *Angeforderter neuer maximaler **maxfiles**-Wert*     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |    
        |  Datenschutzvolumes für die regionsübergreifende Replikation  |  *Abonnement-ID, NetApp-Zielkonto-URI, Zielkapazitätspool-URI, NetApp-Quellkonto-URI, Quellkapazitätspool-URI, Quellvolume-URI*   |  *Angeforderte neue maximale Anzahl **regionsübergreifender Datenschutzvolumes für die regionsübergreifende Replikation (Zielvolumes)** _     |  _Welches Szenario oder welcher Anwendungsfall hat zur Anforderung geführt?*  |    

    2. Legen Sie die entsprechende Supportmethode fest, und geben Sie Ihre Kontaktinformationen an.

    3. Klicken Sie auf **Weiter: Überprüfen + erstellen**, um die Anforderung zu erstellen. 


## <a name="next-steps"></a>Nächste Schritte  

- [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Cost model for Azure NetApp Files (Kostenmodell für Azure NetApp Files)](azure-netapp-files-cost-model.md)
