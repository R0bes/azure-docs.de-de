---
title: Grundlegendes zur Abrechnung für Azure Files | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Abrechnungsmodelle „Bereitgestellt“ und „Nutzungsbasierte Zahlung“ für Azure-Dateifreigaben interpretieren können.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 08/17/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 7f133600f800881f462583ca5bee2972a5c914fa
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122343610"
---
# <a name="understand-azure-files-billing"></a>Grundlegendes zur Abrechnung für Azure Files
Für Azure Files gibt es zwei Abrechnungsmodelle: „Bereitgestellt“ und „Nutzungsbasierte Zahlung“. Das Modell „Bereitgestellt“ ist nur für Premium-Dateifreigaben verfügbar, d. h. für Dateifreigaben, die in einem Speicherkonto des Typs **FileStorage** bereitgestellt werden. Das Modell „Nutzungsbasierte Zahlung“ ist nur für Standarddateifreigaben verfügbar, d. h. für Dateifreigaben, die in einem Speicherkonto des Typs **Universell, Version 2** bereitgestellt werden. In diesem Artikel wird die Funktionsweise beider Modelle erklärt, um Ihnen zu helfen, Ihre monatliche Azure Files-Rechnung zu verstehen.

:::row:::
    :::column:::
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/m5_-GsKv4-o" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    :::column-end:::
    :::column:::
        Bei diesem Video handelt es sich um ein Interview, in dem die Grundlagen des Azure Files-Abrechnungsmodells besprochen werden, einschließlich der Optimierung von Azure-Dateifreigaben, um die geringstmöglichen Kosten zu erzielen, und des Vergleichs von Azure Files mit anderen Dateispeicherangeboten vor Ort und in der Cloud.
   :::column-end:::
:::row-end:::

Preisinformationen zu Azure Files finden Sie auf der [Seite „Azure Files – Preise“](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Ja](../media/icons/yes-icon.png) |

## <a name="storage-units"></a>Speichereinheiten    
In Azure Files werden Basis 2-Maßeinheiten zur Darstellung der Speicherkapazität verwendet: KiB, MiB, GiB und TiB. Bei Ihrem Betriebssystem wird möglicherweise dieselbe Maßeinheit oder dasselbe Zählsystem verwendet.

### <a name="windows"></a>Windows
Sowohl im Windows-Betriebssystem als auch in Azure Files wird die Speicherkapazität mithilfe des Basis 2-Zählsystems gemessen, doch es gibt einen Unterschied bei der Bezeichnung von Einheiten. In Azure Files wird die Speicherkapazität mit Basis 2-Maßeinheiten bezeichnet, während sie in Windows mit Basis 10-Maßeinheiten bezeichnet wird. Beim Melden der Speicherkapazität gibt es in Windows keine Umwandlung von „Basis 2“ in „Basis 10“.

| Akronym | Definition                         | Einheit     | Windows wird angezeigt als |
|---------|------------------------------------|----------|---------------------|
| KiB     | 1\.024 Bytes                        | Kibibyte | KB (Kilobyte)       |
| MiB     | 1\.024 KiB (1.048.576 Bytes)        | Mebibyte | MB (Megabyte)       |
| GiB     | 1\.024 MiB (1.073.741.824 Bytes)     | Gibibyte | GB (Gigabyte)       |
| TiB     | 1\.024 GiB (1.099.511.627.776 Bytes) | Tebibyte | TB (Terabyte)       |

### <a name="macos"></a>macOS
Wenn Sie ermitteln möchten, welches Zählsystem verwendet wird, lesen Sie auf der Website von Apple [How iOS and macOS report storage capacity](https://support.apple.com/HT201402) (Wie iOS und MacOS die Speicherkapazität melden).

### <a name="linux"></a>Linux
Ein anderes Zählsystem könnte von jedem Betriebssystem oder jeder individuellen Software verwendet werden. Wenn Sie erfahren möchten, wie die Speicherkapazität jeweils gemeldet wird, lesen Sie die zugehörige Dokumentation.

## <a name="reserve-capacity"></a>Reservekapazität
Azure Files unterstützt Speicherkapazitätsreservierungen, mit denen Sie einen Preisnachlass für Speicherplatz erzielen können, indem Sie sich im Voraus auf die Speichernutzung festlegen. Sie sollten den Kauf von reservierten Instanzen für alle Produktions-Workloads oder Dev/Test-Workloads mit konsistenten Footprints in Betracht ziehen. Wenn Sie reservierte Kapazität erwerben, muss Ihre Reservierung die folgenden Abmessungen angeben:

- **Kapazitätsgröße**: Kapazitätsreservierungen können für entweder 10 TiB oder 100 TiB mit größeren Rabatten für den Erwerb einer Reservierung mit höherer Kapazität erfolgen. Sie können mehrere Reservierungen erwerben, einschließlich Reservierungen mit unterschiedlichen Kapazitätsgrößen, um Ihre Workload-Anforderungen zu erfüllen. Wenn Ihre Produktionsbereitstellung beispielsweise über 120 TiB Dateifreigaben verfügt, können Sie eine 100 TiB-Reservierung und zwei 10 TiB-Reservierungen erwerben, um die Gesamtkapazitätsanforderungen zu erfüllen.
- **Laufzeit:** Reservierungen können für eine Laufzeit von einem Jahr oder drei Jahren erworben werden, mit erheblichen Rabatten für den Erwerb einer längeren Reservierungslaufzeit. 
- **Ebene**: Die Ebene der Azure-Files für die Kapazitätsreservierung. Reservierungen für Azure Files sind derzeit für die Tarife „Premium“, „Heiß“ und „Kalt“ verfügbar.
- **Standort**: Die Azure-Region der Kapazitätsreservierung. Kapazitätsreservierungen sind in einer Teilmenge der Azure-Regionen verfügbar.
- **Redundanz**: Die Speicherredundanz der Kapazitätsreservierung. Reservierungen werden für alle von Azure Files unterstützten Redundanzen unterstützt, einschließlich LRS, ZRS, GRS und GZRS.

Sobald Sie eine Kapazitätsreservierung erwerben, wird sie automatisch von Ihrer vorhandenen Speicherauslastung verbraucht. Wenn Sie mehr Speicher als reserviert verwenden, zahlen Sie den Listenpreis für den Saldo, der nicht von der Kapazitätsreservierung abgedeckt ist. Gebühren für Transaktionen, Bandbreite und Datenübertragung sind nicht in der Reservierung enthalten.

Weitere Informationen zum Erwerb von Speicherreservierungen finden Sie unter [Optimieren der Kosten für Azure Files mit reservierter Kapazität](files-reserve-capacity.md).

## <a name="provisioned-model"></a>Bereitgestelltes Modell
Azure Files verwendet für Premium-Dateifreigaben das Modell „Bereitgestellt“. Beim diesem Geschäftsmodell geben Sie dem Azure Files-Dienst aktiv an, wie hoch Ihr Speicherbedarf ist, anstatt nach Inanspruchnahme abgerechnet zu werden. Dies ist vergleichbar mit dem Kauf lokaler Hardware. Wenn Sie eine Azure-Dateifreigabe mit einer bestimmten Menge Speicherplatz bereitstellen, zahlen Sie für diesen Speicher unabhängig davon, ob Sie ihn nutzen oder nicht, genauso wie Sie nicht anfangen, Kosten für lokale physische Medien zu zahlen, wenn Sie beginnen, Speicherplatz zu belegen. Im Gegensatz zum Kauf lokaler physischer Medien können bereitgestellte Dateifreigaben je nach Speicher- und E/A-Leistungsmerkmalen dynamisch hoch- oder herunterskaliert werden.

Die bereitgestellte Größe der Dateifreigabe kann jederzeit heraufgesetzt, jedoch erst 24 Stunden nach der letzten Heraufsetzung herabgesetzt werden. Wenn in einer 24-stündigen Wartezeit keine Heraufsetzung aufgetreten ist, können Sie das Freigabekontingent beliebig oft herabsetzen, bis Sie es erneut heraufsetzen. Änderungen an IOPS und Durchsatz werden innerhalb weniger Minuten nach Änderung der bereitgestellten Größe wirksam.

Es ist möglich, die Größe Ihrer bereitgestellten Freigabe unter Ihre verbrauchten GiB zu reduzieren. Wenn Sie dies tun, gehen Ihnen keine Daten verloren, sondern es wird Ihnen weiterhin die verwendete Größe in Rechnung gestellt. Sie erhalten die Leistung (IOPS-Grundwert, Durchsatz und Burst-IOPS) der bereitgestellten Freigabe, nicht die der verwendeten Größe.

### <a name="provisioning-method"></a>Bereitstellungsmethode
Wenn Sie eine Premium-Dateifreigabe bereitstellen, geben Sie an, wie viele GiBs Ihre Workload benötigt. Jedes GiB, das Sie bereitstellen, berechtigt Sie zu mehr IOPS und Durchsatz in einem festen Verhältnis. Zusätzlich zum garantierten IOPS-Grundwert unterstützt jede Premium-Dateifreigabe Bursting nach dem Prinzip der bestmöglichen Leistung. Die Formeln für IOPS und Durchsatz sind wie folgt:

| Element | Wert |
|-|-|
| Mindestgröße einer Dateifreigabe | 100 GB |
| Bereitstellungseinheit | 1 GiB |
| Baseline-IOPS-Formel | `MIN(400 + 1 * ProvisionedGiB, 100000)` |
| Burstgrenzwert | `MIN(MAX(4000, 3 * BaselineIOPS), 100000)` |
| Eingangsrate | `40 MiB/sec + 0.04 * ProvisionedGiB` |
| Ausgangsrate | `60 MiB/sec + 0.06 * ProvisionedGiB` |

Die folgende Tabelle zeigt einige Beispiele dieser Formeln für die bereitgestellten Freigabengrößen:

| Kapazität (GiB) | IOPS-Grundwert | Burst-IOPS | Ausgehend (MiB/s) | Eingehend (MiB/s) |
|-|-|-|-|-|
| 100 | 500 | Bis zu 4.000 | 66 | 44 |
| 500 | 900 | Bis zu 4.000 | 90 | 60 |
| 1\.024 | 1\.424 | Bis zu 4.000 | 122 | 81 |
| 5\.120 | 5\.520 | Bis zu 15.360 | 368 | 245 |
| 10.240 | 10.640 | Bis zu 30.720 | 675 | 450 |
| 33.792 | 34.192 | Bis zu 100.000 | 2\.088 | 1\.392 |
| 51.200 | 51.600 | Bis zu 100.000 | 3\.132 | 2\.088 |
| 102.400 | 100.000 | Bis zu 100.000 | 6\.204 | 4\.136 |

Die effektive Leistung der Dateifreigabe hängt u. a. von den Grenzwerten des Computernetzwerks, der verfügbaren Netzwerkbandbreite, den E/A-Größen und der Parallelität ab. Beispielsweise kann ein einzelner virtueller Windows-Computer ohne aktivierte Funktion SMB Multichannel namens *Standard F16s_v2*, der mit einer Premium-Dateifreigabe über SMB verbunden ist, laut internen Tests mit Lese-/Schreibvorgängen mit einer E/A-Größe von 8 KiB 20 K Lese-IOPS und 15 K Schreib-IOPS erzielen. Bei Lese-/Schreibvorgängen mit einer E/A-Größe von 512 MiB kann derselbe virtuelle Computer einen Durchsatz von 1,1 GiB/s ausgehend und 370 MiB/s eingehend erzielen. Der gleiche Client kann eine bis zu \~dreifache Leistung erzielen, wenn SMB Multichannel für die Premium-Freigaben aktiviert ist. Um eine maximale Leistung zu erreichen, [aktivieren Sie SMB Multichannel](storage-files-enable-smb-multichannel.md), und verteilen Sie die Last auf mehrere VMs. Weitere Informationen zur [Leistung von SMB Multichannel](storage-troubleshooting-files-performance.md) und zu gängigen Leistungsproblemen sowie deren Lösungen finden Sie im [Leitfaden zur Problembehandlung](storage-files-smb-multichannel-performance.md).

### <a name="bursting"></a>Bursting
Wenn Ihre Workload die zusätzliche Leistung benötigt, um den Spitzenbedarf zu erfüllen, kann Ihre Freigabe Burstgutschriften verwenden, um den IOPS-Grundwert für die Freigabe zu überschreiten und die erforderliche Freigabeleistung bereitzustellen. Premium-Dateifreigaben können ihren IOPS-Wert bis auf 4.000 oder um den Faktor drei erhöhen (je nachdem, welcher Wert höher ist). Bursting wird automatisiert und funktioniert auf Basis eines Guthabensystems. Die Burstübertragung funktioniert auf Best-Effort-Basis, und der Burstgrenzwert ist keine Garantie. Bei Dateifreigaben ist eine Burstübertragung *bis zum* Grenzwert für eine maximale Dauer von 60 Minuten möglich.

Guthaben sammeln sich in einem Burstbucket an, wenn Datenverkehr für Ihre Dateifreigabe unterhalb des IOPS-Grundwerts liegt. Beispielsweise weist eine Freigabe mit 100 GiB 500 IOPS-Grundwerte auf. Wenn der tatsächliche Datenverkehr auf der Freigabe 100 IOPS für ein bestimmtes 1-Sekunden-Intervall betrug, werden die 400 nicht verwendeten IOPS einem Burstbucket gutgeschrieben. Auf ähnliche Weise fällt eine Burstgutschrift in Höhe von 1.424 IOPS für eine 1-TiB-Leerlauffreigabe an. Diese Guthaben werden dann später verwendet, wenn Vorgänge die IOPS-Grundwerte überschreiten.

Wenn eine Freigabe den IOPS-Grundwert überschreitet und Guthaben in einem Burstbucket hat, führt sie Burstübertragungen mit der maximal zulässigen Spitzenburstrate durch. Freigaben können bis zu einer Dauer von maximal 60 Minuten weiterhin Bursts ausführen, solange Guthaben verbleibt. Dies hängt aber von der Höhe des aufgelaufenen Burstguthabens ab. Jede E/A über dem IOPS-Grundwert verbraucht ein Guthaben, und wenn alle Guthaben verbraucht sind, kehrt die Freigabe zum IOPS-Grundwert zurück.

Freigabeguthaben können drei Zustände aufweisen:

- Anwachsend, wenn die Dateifreigabe weniger als den IOPS-Grundwert verwendet.
- Abnehmend, wenn die Dateifreigabe mehr als den IOPS-Grundwert verwendet und sich im Burstmodus befindet.
- Konstant, wenn die Dateifreigabe genau den IOPS-Grundwert verwendet. Guthaben sind weder aufgelaufen noch werden sie in Anspruch genommen.

Neue Dateifreigaben beginnen mit der vollen Anzahl von Guthaben im Burstbucket. Burstguthaben werden nicht angesammelt, wenn der Freigabe-IOPS aufgrund einer Einschränkung durch den Server unter den IOPS-Grundwert fällt.

## <a name="pay-as-you-go-model"></a>Modell für die nutzungsbasierte Bezahlung
Azure Files verwendet für Standarddateifreigaben das Geschäftsmodell „Nutzungsbasierte Zahlung“. Bei diesem Geschäftsmodell wird der zu zahlende Betrag dadurch bestimmt, wie viel Sie tatsächlich in Anspruch nehmen, und nicht auf Grundlage einer bereitgestellten Menge. Grundsätzlich zahlen Sie die Kosten einer auf Datenträgern gespeicherte Datenmenge und dann für zusätzliche Transaktionen, die auf der Nutzung dieser Daten basieren. Ein Modell mit nutzungsbasierter Zahlung kann wirtschaftlich sein, da Sie keine Überdimensionierung vornehmen müssen, um künftiges Wachstum oder Leistungsanforderungen zu berücksichtigen. Es muss auch keine Aufhebung von Bereitstellung erfolgen, wenn sich die Workload oder der Datenspeicherbedarf im Laufe der Zeit ändert. Andererseits kann ein Modell mit nutzungsbasierter Zahlung im Rahmen eines Budgetierungsprozesses schwierig zu planen sein, da das Abrechnungsmodell mit nutzungsbasierter Zahlung der Inanspruchnahme durch Endbenutzer unterliegt.

### <a name="differences-in-standard-tiers"></a>Unterschiede bei den Standardtarifen
Wenn Sie eine Standarddateifreigabe erstellen, haben Sie die Wahl zwischen den Speicherebenen „Transaktionsoptimiert“, „Heiß“ und „Kalt“. Alle drei Ebenen werden auf exakt derselben Standardspeicherhardware gespeichert. Der Hauptunterschied dieser drei Ebenen besteht im Preis für die Speicherung ruhender Daten (bei kälteren Ebenen niedriger) und im Transaktionspreis (bei kälteren Ebenen höher). Dies bedeutet Folgendes:

- Wie der Name bereits vermuten lässt, ist der Preis der transaktionsoptimierten Ebene für Workloads mit hohem Transaktionsaufkommen optimiert. Die transaktionsoptimierte Ebene hat zwar den höchsten Preis für die Speicherung ruhender Daten, bietet aber die niedrigsten Transaktionspreise.
- Die heiße Ebene ist für aktive Workloads ohne hohes Transaktionsaufkommen vorgesehen. Verglichen mit der transaktionsoptimierten Ebene hat sie einen etwas niedrigeren Preis für die Speicherung ruhender Daten und etwas höhere Transaktionspreise. Stellen Sie sie sich als Mittelweg zwischen den Ebenen „Transaktionsoptimiert“ und „Kalt“ vor.
- Bei der kalten Speicherebene sind die Kosten für Workloads mit geringer Aktivität optimiert. Diese Ebene weist die niedrigsten Kosten für die Speicherung ruhender Daten, aber die höchsten Kosten für Transaktionen auf.

Wenn Sie für eine Workload, auf die nur selten zugegriffen wird, die Speicherebene „Transaktionsoptimiert“ wählen, zahlen Sie für die wenigen Transaktionen in der Freigabe in einem Monat fast nichts, aber einen hohen Betrag für die Datenspeicherung. Wenn Sie dieselbe Freigabe auf die kalte Speicherebene verlagern, würden Sie immer noch fast keine Transaktionskosten zahlen, einfach weil für diese Workload nur sehr selten Transaktionen erfolgen, aber die kalte Speicherebene bietet viel günstigere Datenspeicherungskosten. Bei Wahl der optimalen Speicherebene für Ihren Anwendungsfall können Sie erheblich Kosten sparen.

Ähnlich verhält es sich, wenn Sie eine Workload mit häufigem Zugriff auf die kalte Speicherebene verlagern. Dann zahlen Sie viel mehr für Transaktionen, aber weniger für Datenspeicherung. Dies kann dazu führen, dass die gestiegenen Kosten für Transaktionen die Einsparungen aufgrund niedrigerer Kosten für Datenspeicherung überwiegen. Dies kann bewirken, dass Sie für die kalte Speicherebene mehr zahlen als bei Wahl von „transaktionsoptimiert“. Es ist möglich, dass bei bestimmten Nutzungsgraden die heiße Speicherebene am wirtschaftlichsten ist, während die kalte Speicherebene teurer als „transaktionsoptimiert“ ist.

Welche Ebene für Ihre Standarddateifreigabe am kostengünstigsten ist, hängt von Ihrer Workload und von der Aktivitätsebene ab. In der Praxis lässt sich die wirtschaftlichste Speicherebene am besten ermitteln, indem der tatsächliche Ressourcenverbrauch der Freigabe (gespeicherte Daten, Schreibtransaktionen usw.) untersucht wird.

### <a name="what-are-transactions"></a>Was sind Transaktionen?
Transaktionen sind Vorgänge oder Anforderungen an Azure Files, den Inhalt der Dateifreigabe hoch- oder herunterzuladen oder anderweitig zu ändern. Jede Aktion in einer Dateifreigabe führt zu einer oder mehreren Transaktionen. Bei Standardfreigaben mit dem Abrechnungsmodell „Nutzungsbasierte Zahlung“ führt dies zu Transaktionskosten.

Es gibt fünf grundlegende Transaktionskategorien: „Schreiben“, „Auflisten“, „Lesen“, „Sonstige“ und „Löschen“. Alle Vorgänge, die über die REST-API oder SMB erfolgen, werden wie folgt einer dieser vier Kategorien zugeordnet:

| Vorgangsart | Schreibtransaktionen | Listentransaktionen | Lesetransaktionen | Sonstige Transaktionen | Löschtransaktionen |
|-|-|-|-|-|-|
| Verwaltungsvorgänge | <ul><li>`CreateShare`</li><li>`SetFileServiceProperties`</li><li>`SetShareMetadata`</li><li>`SetShareProperties`</li><li>`SetShareACL`</li></ul> | <ul><li>`ListShares`</li></ul> | <ul><li>`GetFileServiceProperties`</li><li>`GetShareAcl`</li><li>`GetShareMetadata`</li><li>`GetShareProperties`</li><li>`GetShareStats`</li></ul> | | <ul><li>`DeleteShare`</li></ul> |
| Datenvorgänge | <ul><li>`CopyFile`</li><li>`Create`</li><li>`CreateDirectory`</li><li>`CreateFile`</li><li>`PutRange`</li><li>`PutRangeFromURL`</li><li>`SetDirectoryMetadata`</li><li>`SetFileMetadata`</li><li>`SetFileProperties`</li><li>`SetInfo`</li><li>`Write`</li><li>`PutFilePermission`</li></ul> | <ul><li>`ListFileRanges`</li><li>`ListFiles`</li><li>`ListHandles`</li></ul>  | <ul><li>`FilePreflightRequest`</li><li>`GetDirectoryMetadata`</li><li>`GetDirectoryProperties`</li><li>`GetFile`</li><li>`GetFileCopyInformation`</li><li>`GetFileMetadata`</li><li>`GetFileProperties`</li><li>`QueryDirectory`</li><li>`QueryInfo`</li><li>`Read`</li><li>`GetFilePermission`</li></ul> | <ul><li>`AbortCopyFile`</li><li>`Cancel`</li><li>`ChangeNotify`</li><li>`Close`</li><li>`Echo`</li><li>`Ioctl`</li><li>`Lock`</li><li>`Logoff`</li><li>`Negotiate`</li><li>`OplockBreak`</li><li>`SessionSetup`</li><li>`TreeConnect`</li><li>`TreeDisconnect`</li><li>`CloseHandles`</li><li>`AcquireFileLease`</li><li>`BreakFileLease`</li><li>`ChangeFileLease`</li><li>`ReleaseFileLease`</li></ul> | <ul><li>`ClearRange`</li><li>`DeleteDirectory`</li></li>`DeleteFile`</li></ul> |

> [!Note]  
> NFS 4.1 ist nur für Premium-Dateifreigaben mit dem Abrechnungsmodell „Bereitgestellt“ verfügbar. Transaktionen haben keinen Einfluss auf die Abrechnung für Premium-Dateifreigaben.

## <a name="file-storage-comparison-checklist"></a>Prüfliste für den Dateispeichervergleich
Um die Kosten von Azure Files im Vergleich zu anderen Dateispeicheroptionen richtig zu bewerten, beachten Sie bitte die folgenden Fragen:

- **Wie bezahlen Sie für Speicher, IOPS und Bandbreite?**  
    Bei Azure Files hängt das verwendete Abrechnungsmodell davon ab, ob Sie [Premium](#provisioned-model)- oder [Standard](#pay-as-you-go-model)-Dateifreigaben bereitstellen. Die meisten Cloud-Lösungen verfügen über Modelle, die sich entweder an den Prinzipien des bereitgestellten Speichers ( Preisdefinition, Einfachheit) oder des Pay-as-you-go-Speichers (Sie zahlen nur für die tatsächliche Nutzung) orientieren. Von besonderem Interesse für bereitgestellte Modelle sind die Mindestgröße der bereitgestellten Freigabe, die Bereitstellungseinheit und die Möglichkeit, die Bereitstellung zu erhöhen und zu verringern. 

- **Wie erzielen Sie die Speicher-Ausfallsicherheit und Redundanz?**  
    Mit Azure Files werden Speicher-Ausfallsicherheit und Redundanz in das Produktangebot integriert. Alle Ebenen und Redundanzebenen stellen sicher, dass Daten hochverfügbar sind und auf mindestens drei Kopien Ihrer Daten zugegriffen werden kann. Wenn Sie andere Dateispeicheroptionen in Betracht ziehen, sollten Sie berücksichtigen, ob Speicher-Ausfallsicherheit und Redundanz integriert sind oder es sich um etwas handelt, das Sie selbst zusammenstellen müssen. 

- **Was müssen Sie verwalten?**  
    Bei Azure Files ist die grundlegende Verwaltungseinheit ein Speicherkonto. Andere Lösungen erfordern möglicherweise eine zusätzliche Verwaltung, z. B. Betriebssystem-Aktualisierungen oder die Verwaltung virtueller Ressourcen (VMs, Festplatten, Netzwerk-IP-Adressen usw.).

- **Welche Sicherungskosten fallen an?**  
    Mit Azure Files ist die Integration von Azure Backup einfach zu aktivieren und der Sicherungsspeicher wird als Teil des Kostenanteils abgerechnet (Sicherungen werden als differenzielle Snapshots gespeichert). Andere Lösungen erfordern möglicherweise die Lizenzierung von Sicherungssoftware und zusätzliche Kosten für Sicherungsspeicher.

## <a name="see-also"></a>Weitere Informationen
- [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/)
- [Planung einer Azure Files-Bereitstellung](storage-files-planning.md) und [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](../file-sync/file-sync-planning.md)
- [Erstellen einer Dateifreigabe](storage-how-to-create-file-share.md) und [Bereitstellen der Azure-Dateisynchronisierung](../file-sync/file-sync-deployment-guide.md)