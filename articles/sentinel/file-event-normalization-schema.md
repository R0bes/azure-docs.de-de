---
title: 'Azure Sentinel: Referenz zum Dateiereignis-Normalisierungsschema | Microsoft-Dokumentation'
description: In diesem Artikel wird das Dateiereignis-Normalisierungsschema von Azure Sentinel beschrieben.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/22/2021
ms.author: bagol
ms.openlocfilehash: 80f3779829b54c70fd79251b7c2076deb34dc2ca
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122343323"
---
# <a name="azure-sentinel-file-event-normalization-schema-reference-public-preview"></a>Azure Sentinel: Referenz zum Dateiereignis-Normalisierungsschema (Public Preview)

Das Dateiereignis-Normalisierungsschema wird dazu verwendet, Dateiaktivitäten wie das Erstellen, Ändern oder Löschen von Dateien oder Dokumenten zu beschreiben. Solche Ereignisse werden von Betriebssystemen, Dateispeichersystemen wie Azure Files und Dokumentenverwaltungssystemen wie Microsoft SharePoint gemeldet.

Weitere Informationen zur Normalisierung in Azure Sentinel finden Sie unter [Normalisierung und das Azure Sentinel Information Model (ASIM)](normalization.md).

> [!IMPORTANT]
> Das Dateiereignis-Normalisierungsschema ist derzeit als Public Preview verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="parsers"></a>Parser

Azure Sentinel bietet die folgenden integrierten, produktspezifischen Dateiereignisparser:

- **Sysmon-Dateiaktivitätsereignisse** (Ereignisse 11, 23 und 26), die mit dem Log Analytics-Agent oder dem Azure Monitor-Agent erfasst wurden
- **Microsoft Office 365 SharePoint- und OneDrive-Ereignisse**, die mithilfe des Office-Aktivitätsconnectors gesammelt wurden
- **Microsoft 365 Defender für Endpunkt-Dateiereignisse**
- **Azure Storage**, einschließlich Blob, File, Queue und Table Storage

Verwenden Sie imFileEvent als Tabellennamen in der Abfrage, um den quellunabhängigen Parser zu verwenden, der alle integrierten Parser vereinheitlicht, und um sicherzustellen, dass Ihre Analyse für alle konfigurierten Quellen ausgeführt wird.

Stellen Sie die [quellunabhängigen und quellspezifischen Parser](normalization.md#parsers) aus dem [Azure Sentinel GitHub-Repository](https://aka.ms/AzSentinelFileEvent) bereit.

## <a name="add-your-own-normalized-parsers"></a>Hinzufügen eigener normalisierter Parser


Wenn Sie benutzerdefinierte Parser für das Dateiereignis-Informationsmodell implementieren, benennen Sie Ihre KQL-Funktionen mit der folgenden Syntax: `imFileEvent<vendor><Product`.

Fügen Sie Ihre KQL-Funktion dem quellunabhängigen Parser `imFileEvent` hinzu, um sicherzustellen, dass alle Inhalte, die das Modell Dateiereignis verwenden, auch den neuen Parser verwenden.

## <a name="normalized-content-for-process-activity-data"></a>Normalisierter Inhalt für Prozessaktivitätsdaten

Die folgenden Azure Sentinel-**Analyseregeln** funktionieren mit jeder Dateiaktivität, die mithilfe des Informationsmodells von Azure Sentinel normalisiert wird:

- SUNBURST- und SUPERNOVA-Hintertürhashes (normalisierte Dateiereignisse)
- Exchange Server: im März 2021 offengelegte Sicherheitsrisiken – IoC-Übereinstimmung
- Schreiben verdächtiger Dateien durch den HAFNIUM UM-Dienst
- NOBELIUM – Domänen-, Hash- und IP-IOCs – März 2021
- SUNSPOT-Protokolldateierstellung 

Weitere Informationen finden Sie unter [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md).


## <a name="schema-details"></a>Schemadetails

Das Dateiereignis-Informationsmodell ist auf das [OSSEM-Prozessentitätsschema](https://github.com/OTRF/OSSEM/blob/master/docs/cdm/entities/file.md) ausgerichtet.

### <a name="log-analytics-fields"></a>Log Analytics-Felder

Die folgenden Felder werden von Log Analytics für jeden Datensatz generiert und können beim Erstellen eines benutzerdefinierten Connectors überschrieben werden.

| Feld         | type     | Diskussion (Discussion)      |
| ------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| <a name="timegenerated"></a>**TimeGenerated** | datetime | Der Zeitpunkt, zu dem das Ereignis vom meldenden Gerät generiert wurde.|
| **_ResourceId**   | guid     | Die Azure-Ressourcen-ID des meldenden Geräts oder Diensts oder die Ressourcen-ID des Protokollforwarders für Ereignisse, die mit Syslog, CEF oder WEF weitergeleitet werden. |
| | | |

> [!NOTE]
> Log Analytics fügt auch andere Felder hinzu, die für Sicherheitsanwendungsfälle weniger relevant sind. Weitere Informationen finden Sie unter [Standardspalten in Azure Monitor-Protokollen](../azure-monitor/logs/log-standard-columns.md).
>

## <a name="event-fields"></a>Ereignisfelder

Ereignisfelder sind allen Schemas gemeinsam und beschreiben die Aktivität selbst und das meldende Gerät.

| Feld               | Klasse       | type       |  BESCHREIBUNG       |
|---------------------|-------------|------------|--------------------|
| **EventMessage**        | Optional    | String     |     Eine allgemeine Nachricht oder Beschreibung, entweder im Datensatz enthalten oder aus ihm generiert.   |
| **EventCount**          | Obligatorisch.   | Integer    |     Die Anzahl der Ereignisse, die vom Datensatz beschrieben werden. <br><br>Dieser Wert wird verwendet, wenn die Quelle Aggregation unterstützt. Ein einzelner Datensatz kann mehrere Ereignisse darstellen. <br><br>Legen Sie den Wert für andere Quellen auf `1` fest.   |
| **EventStartTime**      | Obligatorisch.   | Datum/Uhrzeit  |      Wenn die Quelle Aggregation unterstützt und der Datensatz mehrere Ereignisse darstellt, gibt dieses Feld die Zeit an, zu der das erste Ereignis generiert wurde. Andernfalls wird in diesem Feld ein Alias für das Feld [TimeGenerated](#timegenerated) verwendet. |
| **EventEndTime**        | Obligatorisch.   | Alias      |      Alias für das Feld [TimeGenerated](#timegenerated).    |
| **EventType**           | Obligatorisch.   | Enumerated |    Beschreibt den vom Datensatz gemeldeten Vorgang. <br><br>Für Dateidatensätze werden folgende Werte unterstützt: <br><br>- `FileCreated`<br>- `FileModified`<br>- `FileDeleted`<br>- `FileRenamed`<br>- `FileCopied`<br>- `FileMoved`<br>- `FolderCreated`<br>- `FolderDeleted` |
| **EventResult**         | Obligatorisch.   | Enumerated |  Beschreibt das Ergebnis des Ereignisses, das auf einen der folgenden unterstützten Werte normalisiert wird: <br><br>- `Success`<br>- `Partial`<br>- `Failure`<br>- `NA` (nicht zutreffend) <br><br>Die Quelle stellt möglicherweise nur einen Wert für das Feld [EventOriginalResultDetails](#eventoriginalresultdetails) bereit, das analysiert werden muss, um den Wert **EventResult** zu erhalten. |
| <a name="eventoriginalresultdetails"></a>**EventOriginalResultDetails**  | Optional    | String     | Beschreibt das Ergebnis des Ereignisses. Beachten Sie, dass dieser Wert nicht normalisiert ist und für den ursprünglichen Wert vorgesehen ist, wie er von der Quelle bereitgestellt wird. Zu diesem Zeitpunkt gibt es kein verwandtes normalisiertes Feld, d. h. EventResultDetails, für dieses Schema. |
| **EventOriginalUid**    | Optional    | String     | Eine eindeutige ID des ursprünglichen Datensatzes, sofern von der Quelle bereitgestellt.<br><br>Beispiel: `69f37748-ddcd-4331-bf0f-b137f1ea83b`|
| **EventOriginalType**   | Optional    | String     | Der ursprüngliche Ereignistyp oder die ursprüngliche Ereignis-ID, sofern von der Quelle bereitgestellt.<br><br>Beispiel: `4663`|
| <a name ="eventproduct"></a>**EventProduct**       | Obligatorisch.   | String     | Das Produkt, das das Ereignis erzeugt. <br><br>Beispiel: `Sysmon`<br><br>**Hinweis**: Dieses Feld ist möglicherweise nicht im Quelldatensatz verfügbar. In solchen Fällen muss dieses Feld vom Parser festgelegt werden.           |
| **EventProductVersion** | Optional    | String     | Die Version des Produkts, das das Ereignis erzeugt. <br><br>Beispiel: `12.1`<br><br>**Hinweis**: Dieses Feld ist möglicherweise nicht im Quelldatensatz verfügbar. In solchen Fällen muss dieses Feld vom Parser festgelegt werden.           |
| **EventVendor**         | Obligatorisch.   | String     | Der Hersteller des Produkts, das das Ereignis erzeugt. <br><br>Beispiel: `Microsoft`  <br><br>**Hinweis**: Dieses Feld ist möglicherweise nicht im Quelldatensatz verfügbar. In solchen Fällen muss dieses Feld vom Parser festgelegt werden.  |
| **EventSchemaVersion**  | Obligatorisch.   | String     | Die Version des Schemas. Die hier dokumentierte Version des Schemas ist `0.1`.         |
| **EventReportUrl**      | Optional    | String     | Eine URL, die im Ereignis für eine Ressource bereitgestellt wird, die zusätzliche Informationen zum Ereignis enthält.|
| **Dvc**                 | Alias       | String     | Ein eindeutiger Bezeichner des Geräts, auf dem das Ereignis aufgetreten ist. <br><br>Beispiel: `ContosoDc.Contoso.Azure`<br><br>Dieses Feld kann ein Alias für die Felder [DvcId](#dvcid), [DvcHostname](#dvchostname) oder [DvcIpAddr](#dvcipaddr) sein. Verwenden Sie für Cloudquellen, für die es kein offensichtliches Gerät gibt, denselben Wert wie das Feld [EventProduct](#eventproduct).            |
| <a name ="dvcipaddr"></a>**DvcIpAddr**   | Empfohlen | IP-Adresse | Die IP-Adresse des Geräts, auf dem das Dateiereignis eingetreten ist.  <br><br>Beispiel: `45.21.42.12`    |
| <a name ="dvchostname"></a>**DvcHostname**    | Empfohlen | Hostname   | Der Hostname des Geräts, auf dem das Dateiereignis eingetreten ist. <br><br>Beispiel: `ContosoDc.Contoso.Azure` |
| <a name ="dvcid"></a>**DvcId**  | Optional    | String     |  Die eindeutige ID des Geräts, auf dem das Dateiereignis eingetreten ist. <br><br>Beispiel: `41502da5-21b7-48ec-81c9-baeea8d7d669`   |
| **DvcMacAddr**          | Optional    | MAC        |   Die MAC-Adresse des Geräts, auf dem das Dateiereignis aufgetreten ist.  <br><br>Beispiel: `00:1B:44:11:3A:B7`       |
| **DvcOs**               | Optional    | String     |         Das Betriebssystem, das auf dem Gerät ausgeführt wird, auf dem das Dateiereignis eingetreten ist.    <br><br>Beispiel: `Windows`    |
| **DvcOsVersion**        | Optional    | String     |   Die Version des Betriebssystems auf dem Gerät, auf dem das Dateiereignis eingetreten ist. <br><br>Beispiel: `10` |
| **AdditionalFields**    | Optional    | Dynamisch    | Wenn Ihre Quelle zusätzliche Informationen enthält, die erhalten bleiben sollten, behalten Sie sie entweder mit den ursprünglichen Feldnamen bei, oder erstellen Sie das dynamische Feld **AdditionalFields**, und fügen Sie ihm die zusätzlichen Informationen als Schlüssel-Wert-Paare hinzu.    |
| | | | |



### <a name="file-event-specific-fields"></a>Dateiereignisspezifische Felder

Die in der folgenden Tabelle aufgeführten Felder sind spezifisch für Dateiereignisse. Sie ähneln jedoch Feldern in anderen Schemas und folgen ähnlichen Benennungskonventionen.

Das Dateiereignisschema verweist auf die folgenden Entitäten, die für Dateiaktivitäten von zentraler Bedeutung sind:

- **Actor**. Der Benutzer, der die Dateiaktivität initiiert hat
- **ActingProcess**. Der Prozess, der vom Actor zum Initiieren der Dateiaktivität verwendet wird
- **TargetFile**. Die Datei, für die der Vorgang ausgeführt wurde
- **Quelldatei (SrcFile)** . Speichert Dateiinformationen vor dem Vorgang

Die Beziehung zwischen diesen Entitäten wird am besten wie folgt demonstriert: Ein **Actor** führt einen Dateivorgang mithilfe eines **handelnden Prozesses** aus, der die **Quelldatei** in die **Zieldatei** ändert. 

Beispiel: `JohnDoe` (**Actor**) verwendet `Windows File Explorer` (**handelnder Prozess**), um `new.doc` (**Quelldatei**) in `old.doc` (**Zieldatei**) umzubenennen.

| Feld          | Klasse        | type       | BESCHREIBUNG   |
|---------------|--------------|------------|-----------------|
| **ActingProcessCommandLine** |Optional  |String  | Die Befehlszeile zum Ausführen des Programms. <br><br>Beispiel: `"choco.exe" -v` |
|**ActingProcessGuid** |Optional     | String     |  Ein generierter eindeutiger Bezeichner (GUID) des handelnden Prozesses. Ermöglicht die systemübergreifende Identifizierung des Prozesses.  <br><br> Beispiel: `EF3BD0BD-2B74-60C5-AF5C-010000001E00`            |
| **ActingProcessId**| Obligatorisch.    | Integer        | Die Prozess-ID (PID) des handelnden Prozesses.<br><br>Beispiel: `48610176`           <br><br>**Hinweis**: Der Typ ist als *Zeichenfolge* definiert, um unterschiedliche Systeme zu unterstützen, aber unter Windows und Linux muss dieser Wert numerisch sein. <br><br>Wenn Sie einen Windows- oder Linux-Computer verwenden und einen anderen Typ verwendet haben, stellen Sie sicher, dass Sie die Werte konvertieren. Wenn Sie beispielsweise einen Hexadezimalwert verwendet haben, konvertieren Sie diesen in einen Dezimalwert.    |
| <a name="actingprocessname"></a>**ActingProcessName**              | Optional     | String     |   Der Name des handelnden Prozesses. Dieser Name wird häufig vom Image oder von der ausführbaren Datei abgeleitet, die zum Definieren des ursprünglichen Codes und der Daten verwendet wird, die dem virtuellen Adressraum des Prozesses zugeordnet sind.<br><br>Beispiel: `C:\Windows\explorer.exe`  |
|**Process**| Alias| | Alias für [ActingProcessName](#actingprocessname)|
| <a name="actoruserid"></a>**ActorUserId**    | Empfohlen  | String     |   Eine eindeutige ID des **Akteurs**. Die jeweilige ID hängt vom System ab, das das Ereignis generiert. Weitere Informationen finden Sie unter [Benutzerentität](normalization.md#the-user-entity).  <br><br>Beispiel: `S-1-5-18`    |
| **ActorUserIdType**| Empfohlen  | String     |  Der Typ der ID, die im Feld [ActorUserId](#actoruserid) gespeichert ist. Weitere Informationen finden Sie unter [Benutzerentität](normalization.md#the-user-entity). <br><br>Beispiel: `SID`         |
| <a name="actorusername"></a>**ActorUsername**  | Obligatorisch.    | String     | Der Benutzername des Benutzers, der das Ereignis initiiert hat. <br><br>Beispiel: `CONTOSO\WIN-GG82ULGC9GO$`     |
| **ActorUsernameType**              | Obligatorisch.    | Enumerated |   Gibt den Typ des Benutzernamens an, der im Feld [ActorUsername](#actorusername) gespeichert ist. Weitere Informationen finden Sie unter [Benutzerentität](normalization.md#the-user-entity). <br><br>Beispiel: `Windows`       |
|**Benutzer** | Alias| | Alias für das Feld [ActorUsername](#actorusername). <br><br>Beispiel: `CONTOSO\dadmin`|
| **ActorUserType**|Optional | Enumerated| Der Typ des **Akteurs**. Unterstützte Werte sind: <br><br>- `Regular`<br>- `Machine`<br>- `Admin`<br>- `System`<br>- `Application`<br>- `Service Principal`<br>- `Other` <br><br>**Hinweis**: Die Quelle stellt möglicherweise nur einen Wert für das Feld [ActorOriginalUserType](#actororiginalusertype) bereit, das analysiert werden muss, um den Wert **ActorUserType** zu erhalten.|
|<a name="actororiginalusertype"></a>**ActorOriginalUserType** |Optional |String | Der Benutzertyp des **Akteurs**, wie vom meldenden Gerät bereitgestellt. |
|**HttpUserAgent** |Optional | String |Der verwendete Benutzer-Agent, wenn der Vorgang von einem Remotesystem über HTTP oder HTTPS initiiert wird.<br><br>Zum Beispiel:<br>`Mozilla/5.0 (Windows NT 10.0; Win64; x64)`<br>`AppleWebKit/537.36 (KHTML, like Gecko)`<br>` Chrome/42.0.2311.135`<br>`Safari/537.36 Edge/12.246`|
| **NetworkApplicationProtocol**| Optional|String | Wenn der Vorgang von einem Remotesystem initiiert wird, ist dieser Wert das Anwendungsschichtprotokoll, das im OSI-Modell verwendet wird. <br><br>Obwohl dies kein aufzählbares Feld ist und jeder Wert akzeptiert wird, sind folgende Werte zu bevorzugen: `HTTP`, `HTTPS`, `SMB`, `FTP` und `SSH`.<br><br>Beispiel: `SMB`|
|**SrcIpAddr** |Empfohlen |IP-Adresse | Die IP-Adresse dieses Systems, wenn der Vorgang von einem Remotesystem initiiert wird.<br><br>Beispiel: `185.175.35.214`|
| **SrcFileCreationTime**|Optional |Datum/Uhrzeit |Der Zeitpunkt, zu dem die Quelldatei erstellt wurde. |
|**SrcFileDirectory** | Optional| String| Der Quelldateiordner oder -speicherort. Dieses Feld sollte dem Feld [SrcFilePath](#srcfilepath) unter Ausschluss des letzten Elements gleichen. <br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
| **SrcFileExtension**|Optional | String|Die Quelldateierweiterung. <br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
|**SrcFileMimeType** |Optional |Enumerated |    Der MIME- oder Medientyp der Quelldatei. Die unterstützten Werte sind im [IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml)-Repository aufgeführt. |
|**SrcFileName** |Optional |String | Der Name der Quelldatei, ohne Pfad oder Speicherort, aber ggf. mit einer Erweiterung. Dieses Feld sollte dem letzten Element im Feld [SrcFilePath](#srcfilepath) gleichen. <br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
| <a name="srcfilepath"></a>**SrcFilePath**| Obligatorisch.|String |Der vollständige normalisierte Pfad der Quelldatei, einschließlich des Ordners oder Speicherorts, des Dateinamens und der Erweiterung. <br><br>Weitere Informationen finden Sie unter [Pfadstruktur](#path-structure).<br><br>Beispiel: `/etc/init.d/networking` |
|**SrcFilePathType** |Obligatorisch. | Enumerated| Der [SrcFilePath](#srcfilepath)-Typ. Weitere Informationen finden Sie unter [Pfadstruktur](#path-structure).|
|**SrcFileMD5**|Optional |MD5 | Der MD5-Hash der Quelldatei. <br><br>Beispiel:           `75a599802f1fa166cdadb360960b1dd0` |
|**SrcFileSHA1**|Optional |SHA1 |Der SHA-1-Hash der Quelldatei.<br><br>Beispiel:<br>`d55c5a4df19b46db8c54`<br>`c801c4665d3338acdab0` |
|**SrcFileSHA256** | Optional|SHA256 |Der SHA-256-Hash der Quelldatei. <br><br>Beispiel:<br> `e81bb824c4a09a811af17deae22f22dd`<br>`2e1ec8cbb00b22629d2899f7c68da274`|
|**SrcFileSHA512** |Optional | SHA512|Der SHA-512-Hash der Quelldatei. |
|**SrcFileSize**| Optional|Integer | Die Größe der Quelldatei in Byte.|
|**TargetFileCreationTime** | Optional|Datum/Uhrzeit |Der Zeitpunkt, zu dem die Zieldatei erstellt wurde. |
|**TargetFileDirectory** | Optional|String |Der Zieldateiordner oder -speicherort. Dieses Feld sollte dem Feld [TargetFilePath](#targetfilepath) unter Ausschluss des letzten Elements gleichen. <br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
|**TargetFileExtension** |Optional |String | Die Zieldateierweiterung<br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
| **TargetFileMimeType**|Optional | Enumerated| Der MIME- oder Medientyp der Zieldatei. Die zulässigen Werte sind im [IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml)-Repository aufgeführt.|
| **TargetFileName**|Optional |String |Der Name der Zieldatei, ohne Pfad oder Speicherort, aber ggf. mit einer Erweiterung. Dieses Feld sollte dem letzten Element im Feld [TargetFilePath](#targetfilepath) gleichen.<br><br>**Hinweis**: Ein Parser kann diesen Wert bereitstellen, wenn der Wert in der Protokollquelle verfügbar ist und nicht aus dem vollständigen Pfad extrahiert werden muss.|
|<a name="targetfilepath"></a>**TargetFilePath** | Obligatorisch.| String| Der vollständige normalisierte Pfad der Zieldatei, einschließlich des Ordners oder Speicherorts, des Dateinamens und der Erweiterung. Weitere Informationen finden Sie unter [Pfadstruktur](#path-structure). <br><br>**Hinweis**: Wenn der Datensatz keine Ordner- oder Speicherortinformationen enthält, speichern Sie den Dateinamen nur hier. <br><br>Beispiel: `C:\Windows\System32\notepad.exe`|
|**TargetFilePathType** | Obligatorisch.|Enumerated | Der Typ von [TargetFilePath](#targetfilepath). Weitere Informationen finden Sie unter [Pfadstruktur](#path-structure).    |
|**FilePath** |Alias | | Alias für das Feld [TargetFilePath](#targetfilepath).|
| **TargetFileMD5**| Optional| MD5|Der MD5-Hash der Zieldatei. <br><br>Beispiel: `75a599802f1fa166cdadb360960b1dd0` |
|**TargetFileSHA1** |Optional |SHA1 |Der SHA-1-Hash der Zieldatei. <br><br>Beispiel:<br> `d55c5a4df19b46db8c54`<br>`c801c4665d3338acdab0`|
|**TargetFileSHA256** | Optional|SHA256 |Der SHA-256-Hash der Zieldatei. <br><br>Beispiel:<br> `e81bb824c4a09a811af17deae22f22dd`<br>`2e1ec8cbb00b22629d2899f7c68da274`  |
| **TargetFileSHA512**| Optional| SHA512|Der SHA-512-Hash der Quelldatei. |
|**Hash**|Alias | |Alias für den besten verfügbaren Zieldateihash. |
|**TargetFileSize** |Optional | Integer|Die Größe der Zieldatei in Byte. |
| **TargetUrl**|Optional | String|Wenn der Vorgang über HTTP oder HTTPS initiiert wird, wird die URL verwendet. <br><br>Beispiel: `https://onedrive.live.com/?authkey=...` |
| | | | |



## <a name="path-structure"></a>Pfadstruktur

Der Pfad muss so normalisiert werden, dass er einem der folgenden Formate entspricht. Das Format, in das der Wert normalisiert wird, wird im entsprechenden Feld **FilePathType** widergespiegelt.

|type  |Beispiel  |Notizen  |
|---------|---------|---------|
|**Windows Lokal**     |   `C:\Windows\System32\notepad.exe`      |      Da bei Windows-Pfadnamen die Groß-/Kleinschreibung nicht beachtet wird, impliziert dieser Typ, dass bei dem Wert die Groß-/Kleinschreibung nicht beachtet wird.   |
|**Windows-Freigabe**     |      `\\Documents\My Shapes\Favorites.vssx`   | Da bei Windows-Pfadnamen die Groß-/Kleinschreibung nicht beachtet wird, impliziert dieser Typ, dass bei dem Wert die Groß-/Kleinschreibung nicht beachtet wird.        |
|**Unix**     |  `/etc/init.d/networking`       |     Da bei Unix-Pfadnamen die Groß-/Kleinschreibung beachtet wird, impliziert dieser Typ, dass bei dem Wert die Groß-/Kleinschreibung beachtet wird.  <br><br>– Verwenden Sie diesen Typ für AWS S3. Verketten Sie den Bucket und die Schlüsselnamen, um den Pfad zu erstellen. <br><br>– Verwenden Sie diesen Typ für Azure Blob Storage-Objektschlüssel.   |
|**URL**     |  `https://1drv.ms/p/s!Av04S_*********we`       | Verwenden Sie diesen Typ, wenn der Dateipfad als URL verfügbar ist. URLs sind nicht auf HTTP oder HTTPS als Schema beschränkt. Jeder gültige Wert, einschließlich eines FTP-Werts, ist gültig.         |
|     |         |         |



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter:

- [Normalisierung in Azure Sentinel](normalization.md)
- [Azure Sentinel: Referenz zum Authentifizierungsnormalisierungsschema (Public Preview)](authentication-normalization-schema.md)
- [Azure Sentinel: Referenz zum DNS-Normalisierungsschema](dns-normalization-schema.md)
- [Azure Sentinel: Referenz zum Netzwerknormalisierungsschema](normalization-schema.md)
- [Azure Sentinel: Referenz zum Prozessereignis-Normalisierungsschema (Public Preview)](process-events-normalization-schema.md)
- [Azure Sentinel: Referenz zum Registrierungsereignis-Normalisierungsschema (Public Preview)](registry-event-normalization-schema.md)