---
title: Versionshinweise zu Azure Stack Edge (GA) | Microsoft-Dokumentation
description: In diesem Artikel werden schwerwiegende offene Probleme und Lösungen im allgemein verfügbaren Release von Azure Stack Edge beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/05/2021
ms.author: alkohli
ms.openlocfilehash: 651b67b95a127aeafd8e9f7efbbb167680cc372e
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110460821"
---
# <a name="azure-stack-edge-pro-with-gpu-general-availability-ga-release-notes"></a>Versionshinweise zu Azure Stack Edge Pro mit GPU (GA, allgemeine Verfügbarkeit)

[!INCLUDE [applies-to-Pro-GPU-sku](../../includes/azure-stack-edge-applies-to-gpu-sku.md)]

In den folgenden Versionshinweisen werden die schwerwiegenden offenen und die behobenen Issues für das GA-Release (allgemeine Verfügbarkeit) für Azure Stack Edge Pro-Geräte mit GPU beschrieben.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres Azure Stack Edge Pro-Geräts die Informationen in den Versionshinweisen sorgfältig durch.

Dieser Artikel bezieht sich auf das Release von **Azure Stack Edge Pro 2010**, das der Softwareversionsnummer **2.1.1377.2170** zugeordnet ist.

## <a name="whats-new"></a>Neues

Die folgenden neuen Features sind im Azure Stack Edge 2010-Release verfügbar. 

- **Speicherklassen:** In diesem Release stehen Speicherklassen zur Verfügung, mit denen Sie Speicher dynamisch bereitstellen können. Weitere Informationen finden Sie unter [Kubernetes-Speicherverwaltung auf dem Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-gpu-kubernetes-storage.md#dynamicprovisioning). 
- **Kubernetes-Dashboard mit Metrics Server:** In diesem Release wird ein Kubernetes-Dashboard mit dem Metrics Server-Add-On hinzugefügt. Sie können das Dashboard verwenden, um eine Übersicht über die auf dem Azure Stack Edge Pro-Gerät ausgeführten Anwendungen, den Status von Kubernetes-Clusterressourcen und eventuelle auf dem Gerät aufgetretene Fehler anzuzeigen. Mit dem Metrics Server-Add-On wird die CPU- und Speicherauslastung der Kubernetes-Ressourcen auf dem Gerät aggregiert. Weitere Informationen finden Sie unter [Überwachen des Azure Stack Edge Pro-GPU-Geräts über das Kubernetes-Dashboard](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md).
- **Azure Arc-fähiges Kubernetes für Azure Stack Edge Pro:** Ab diesem Release können Sie Anwendungsworkloads auf dem Azure Stack Edge Pro-Gerät über Azure Arc-fähiges Kubernetes bereitstellen. Azure Arc ist ein Hybridverwaltungstool, mit dem Sie Anwendungen in Ihren Kubernetes-Clustern bereitstellen können. Weitere Informationen finden Sie unter [Bereitstellen von Workloads über Azure Arc auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md).  

## <a name="known-issues"></a>Bekannte Probleme 

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Issues für das Azure Stack Edge Pro-Gerät.

| Nein. | Funktion | Problem | Problemumgehung/Kommentare |
| --- | --- | --- | --- |
|**1.**|Previewfunktionen |In diesem GA-Release sind die folgenden Features in der Vorschau für Ihr Azure Stack Edge Pro-Gerät verfügbar: lokaler Azure Resource Manager, VMs, Kubernetes, Azure Arc-fähiges Kubernetes, MPS (Multi-Process Service, Multiprozessdienst) für GPU.  |Diese Features werden in einem späteren Release allgemein verfügbar sein. |
| **2.** |Azure Stack Edge Pro + Azure SQL | Zum Erstellen einer SQL-Datenbank ist Administratorzugriff erforderlich.   |Führen Sie statt der Schritte 1 und 2 unter [https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database](../iot-edge/tutorial-store-data-sql-server.md#create-the-sql-database) die folgenden Schritte aus. <ul><li>Aktivieren Sie in der lokalen Benutzeroberfläche des Geräts die Computeschnittstelle. Wählen Sie **Compute > Portnr. > Für Compute aktivieren > Anwenden** aus.</li><li>Laden Sie das [Hilfsprogramm sqlcmd](/sql/tools/sqlcmd-utility) auf Ihren Clientcomputer herunter.</li><li>Stellen Sie eine Verbindung mit der IP-Adresse der Computeschnittstelle (dem aktivierten Port) her, und fügen Sie am Ende der Adresse „,1401“ hinzu.</li><li>Der endgültige Befehl sieht wie folgt aus: sqlcmd -S {IP-Adresse-der-Schnittstelle},1401 -U SA -P "Starkes!Kennw0rt".</li>Danach sollten die Schritte 3 und 4 aus der aktuellen Dokumentation identisch sein. </li></ul> |
| **3.** |Aktualisieren| Inkrementelle Änderungen an Blobs, die über **Aktualisieren** wiederhergestellt werden, sind NICHT möglich. |Bei Blobendpunkten können Teilaktualisierungen von Blobs nach dem erneuten Laden dazu führen, dass die Aktualisierungen nicht in die Cloud hochgeladen werden. Dies betrifft beispielsweise eine Sequenz folgender Aktionen:<ul><li>Erstellen eines Blobs in der Cloud. Oder Löschen eines zuvor hochgeladenen Blobs auf dem Gerät.</li><li>Aktualisieren des Blobs mithilfe der Aktualisierungsfunktion aus der Cloud auf der Appliance.</li><li>Aktualisieren nur eines Teils des Blobs mithilfe von Azure SDK-REST-APIs.</li></ul>Diese Aktionen können dazu führen, dass die aktualisierten Abschnitte des Blobs nicht in der Cloud aktualisiert werden. <br>**Problemumgehung**: Verwenden Sie Tools wie Robocopy, oder führen Sie reguläre Dateikopiervorgänge über Explorer oder die Befehlszeile durch, um ganze Blobs zu ersetzen.|
|**4.**|Drosselung|Wenn bei der Drosselung neue Schreibvorgänge auf dem Gerät nicht zulässig sind, tritt bei über den NFS-Client durchgeführten Schreibvorgängen der Fehler „Zugriff verweigert“ auf.| Der Fehler wird wie folgt angezeigt:<br>`hcsuser@ubuntu-vm:~/nfstest$ mkdir test`<br>mkdir: Verzeichnis „test“ kann nicht erstellt werden: Zugriff verweigert|
|**5.**|Blob Storage-Erfassung|Wenn Sie AzCopy Version 10 für die Erfassung von Blob Storage verwenden, führen Sie AzCopy mit folgendem Argument aus: `Azcopy <other arguments> --cap-mbps 2000`| Wenn diese Grenzwerte für AzCopy nicht angegeben werden, wird möglicherweise eine große Anzahl von Anforderungen an das Gerät gesendet. Dies kann zu Problemen mit dem Dienst führen.|
|**6.**|Mehrstufige Speicherkonten|Bei der Verwendung von mehrstufigen Speicherkonten gilt Folgendes:<ul><li> Es werden nur Blockblobs unterstützt. Seitenblobs werden nicht unterstützt.</li><li>Momentaufnahmen- oder Kopieren-APIs werden nicht unterstützt.</li><li> Die Erfassung von Hadoop-Workloads über `distcp` wird nicht unterstützt, da dabei der Kopiervorgang intensiv genutzt wird.</li></ul>||
|**7.**|Verbindung mit NFS-Freigabe|Wenn mehrere Prozesse in dieselbe Freigabe kopiert werden und das Attribut `nolock` nicht verwendet wird, werden beim Kopiervorgang möglicherweise Fehler angezeigt.|Zum Kopieren von Dateien in die NFS-Freigabe muss das Attribut `nolock` an den mount-Befehl übergeben werden. Beispiel: `C:\Users\aseuser mount -o anon \\10.1.1.211\mnt\vms Z:`.|
|**8.**|Kubernetes-Cluster|Wenn Sie auf Ihrem Gerät, auf dem ein Kubernetes-Cluster ausgeführt wird, ein Update anwenden, werden die virtuellen Kubernetes-Computer neu gestartet. In diesem Fall werden nur Pods, die mit angegebenen Replikaten bereitgestellt werden, nach einem Update automatisch wiederhergestellt.  |Wenn Sie einzelne Pods außerhalb eines Replikationscontrollers ohne Angabe einer Replikatgruppe erstellt haben, werden diese Pods nach dem Geräteupdate nicht automatisch wiederhergestellt. Diese Pods müssen wiederhergestellt werden.<br>Eine Replikatgruppe ersetzt Pods, die aus einem bestimmten Grund gelöscht oder beendet werden, z. B. Knotenfehler oder Aktualisierung eines unterbrochenen Knotens. Aus diesem Grund empfiehlt es sich, eine Replikatgruppe auch dann zu verwenden, wenn in der Anwendung nur ein einziger Pod erforderlich ist.|
|**9.**|Kubernetes-Cluster|Kubernetes auf Azure Stack Edge Pro wird nur mit Helm v3 oder höher unterstützt. Weitere Informationen finden Sie unter [Frequently asked questions: Removal of Tiller](https://v3.helm.sh/docs/faq/) (Häufig gestellte Fragen: Entfernen von Tiller).|
|**10.**|Kubernetes mit Azure Arc-Aktivierung |Im allgemein verfügbaren Release wird Azure Arc-fähiges Kubernetes von Version 0.1.18 auf 0.2.9. Da das Update für Azure Arc-fähiges Kubernetes nicht auf Azure Stack Edge-Geräten unterstützt wird, müssen Sie Azure Arc-fähiges Kubernetes erneut bereitstellen.|Führen Sie die folgenden Schritte durch:<ol><li>[Führen Sie Updates für Gerätesoftware und Kubernetes durch](azure-stack-edge-gpu-install-update.md).</li><li>Stellen Sie eine Verbindung mit der [PowerShell-Schnittstelle des Geräts](azure-stack-edge-gpu-connect-powershell-interface.md) her.</li><li>Entfernen Sie den vorhandenen Azure Arc-Agent. Geben Sie Folgendes ein: `Remove-HcsKubernetesAzureArcAgent`.</li><li>Stellen Sie [Azure Arc auf einer neuen Ressource](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md) bereit. Verwenden Sie keine vorhandene Azure Arc-Ressource.</li></ol>|
|**11.**|Kubernetes mit Azure Arc-Aktivierung|Azure Arc-Bereitstellungen werden nicht unterstützt, wenn auf dem Azure Stack Edge Pro-Gerät ein Webproxy konfiguriert ist.||
|**12.**|Kubernetes |Port 31000 ist für das Kubernetes-Dashboard reserviert. Entsprechend sind die IP-Adressen 172.28.0.1 und 172.28.0.10 in der Standardkonfiguration jeweils für den Kubernetes-Dienst bzw. den Core DNS-Dienst reserviert.|Reservierte IP-Adressen dürfen nicht verwendet werden.|
|**13.**|Kubernetes |In Kubernetes sind derzeit keine Lastenausgleichsdienste für mehrere Protokolle zulässig, beispielsweise ein DNS-Dienst, der auf TCP sowie auf UDP lauschen soll. |Zum Umgehen dieser Einschränkung von Kubernetes mit MetalLB können für die gleiche Podauswahl zwei Dienste (einer für TCP, einer für UDP) erstellt werden. Diese Dienste verwenden denselben Freigabeschlüssel und denselben Wert spec.loadBalancerIP und nutzen so die gleiche IP-Adresse gemeinsam. IP-Adressen können auch gemeinsam genutzt werden, wenn Sie über mehr Dienste als verfügbare IP-Adressen verfügen. <br> Weitere Informationen finden Sie unter [IP Address Sharing](https://metallb.universe.tf/usage/#ip-address-sharing) (Freigeben von IP-Adressen).|
|**14.**|Kubernetes-Cluster|Vorhandene Marketplace-Module für Azure IoT Edge können Änderungen für die Ausführung in IoT Edge auf Azure Stack Edge-Geräten erfordern.|Weitere Informationen finden Sie unter „Ändern von Azure IoT Edge-Modulen aus dem Marketplace zum Ausführen auf dem Azure Stack Edge-Gerät“.<!-- insert link-->|
|**15.**|Kubernetes |Dateibasierte Bindungseinbindungen werden mit Azure IoT Edge in Kubernetes auf dem Azure Stack Edge-Gerät nicht unterstützt.|IoT Edge verwendet eine Übersetzungsebene, um `ContainerCreate`-Optionen in Kubernetes-Konstrukte zu übersetzen. Bei Erstellung von `Binds` wird das Verzeichnis `hostpath` zugeordnet. Daher können dateibasierte Bindungseinbindungen nicht an Pfade in IoT Edge-Containern gebunden werden. Ordnen Sie nach Möglichkeit das übergeordnete Verzeichnis zu.|
|**16.**|Kubernetes |Wenn Sie eigene Zertifikate für IoT Edge verwenden und diese zu Ihrem Azure Stack Edge-Gerät hinzufügen möchten, nachdem die Computeressource auf dem Gerät konfiguriert wurde, werden die neuen Zertifikate nicht übernommen.|Zum Umgehen dieses Problems sollten Sie die Zertifikate hochladen, bevor Sie die Computeressource auf dem Gerät konfigurieren. Wenn die Computeressource bereits konfiguriert ist, [stellen Sie eine Verbindung mit der PowerShell-Schnittstelle des Geräts her, und führen Sie IoT Edge-Befehle aus](azure-stack-edge-gpu-connect-powershell-interface.md#use-iotedge-commands). Starten Sie die Pods `iotedged` und `edgehub` neu.|
|**17.**|Zertifikate |In bestimmten Fällen kann es einige Sekunden dauern, bis der Zertifikatsstatus in der lokalen Benutzeroberfläche aktualisiert ist. |Dies kann sich in den folgenden Szenarios auf die lokale Benutzeroberfläche auswirken:<ul><li>Spalte **Status** auf der Seite **Zertifikate**</li><li>Kachel **Sicherheit** auf der Seite **Erste Schritte**</li><li>Kachel **Konfiguration** auf der Seite **Übersicht**</li></ul>  |
|**17.**|IoT Edge |Über IoT Edge bereitgestellte Module können das Hostnetzwerk nicht verwenden. | |
|**18.**|Compute + Kubernetes |Der NTLM-Webproxy wird nicht von Compute/Kubernetes unterstützt. ||
|**19.**|Compute + Webproxy + Update |Wenn Sie die Computeressource mit einem Webproxy konfiguriert haben, können Updates für die Computeressource fehlschlagen. |Es wird empfohlen, dass Sie die Computeressource vor dem Update deaktivieren. |

<!--|**18.**|Azure Private Edge Zone (Preview) |There is a known issue with Virtual Network Function VM if the VM was created on Azure Stack Edge device running earlier preview builds such as 2006/2007b and then the device was updated to 2009 GA release. The issue is that the VNF information can't be retrieved or any new VNFs can't be created unless the VNF VMs are deleted before the device is updated.  |Before you update Azure Stack Edge device to 2009 release, use the PowerShell command `get-mecvnf` followed by `remove-mecvnf <VNF guid>` to remove all Virtual Network Function VMs one at a time. After the upgrade, you will need to redeploy the same VNFs.|-->




## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Stack Edge Pro-Geräten mit GPU](azure-stack-edge-gpu-deploy-prep.md)