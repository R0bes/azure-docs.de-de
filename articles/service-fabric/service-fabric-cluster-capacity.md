---
title: Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster
description: Knotentypen, Dauerhaftigkeit, Zuverlässigkeit und andere Faktoren bei der Planung eines Service Fabric-Clusters
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: pepogors
ms.openlocfilehash: 0b04bc99abc2f9864ed22078f809702390d9f547
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110695447"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster

Die Planung der Clusterkapazität ist für jede Service Fabric-Produktionsumgebung ein wichtiger Schritt. Folgende Aspekte sind besonders wichtig:

* **Anzahl und Eigenschaften der *Clusterknotentypen***

* ***Dauerhaftigkeitsgrad* jedes Knotentyps** – entscheidet über die Service Fabric-VM-Berechtigungen in der Azure-Infrastruktur

* ***Zuverlässigkeitsgrad* des Clusters** – entscheidet über die Stabilität der Service Fabric-Systemdienste und die Clusterfunktionalität insgesamt

In diesem Artikel werden die wesentlichen Entscheidungspunkte für die einzelnen Bereiche erläutert.

## <a name="initial-number-and-properties-of-cluster-node-types"></a>Anzahl und Eigenschaften der Clusterknotentypen

Ein *Knotentyp* definiert die Größe, Anzahl und Eigenschaften der Knoten (virtuelle Computer) im Cluster. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird einer [VM-Skalierungsgruppe](../virtual-machine-scale-sets/overview.md) zugeordnet.

Da jeder Knotentyp eine separate Skalierungsgruppe ist, ist es möglich, diesen einzeln zentral hoch- oder herunterzuskalieren. Bei jedem Typ können unterschiedliche Ports geöffnet sein, und die Typen weisen verschiedene Kapazitätsmetriken auf. Weitere Informationen zur Beziehung zwischen Knotentypen und VM-Skalierungsgruppen finden Sie im Artikel zu [Service Fabric-Clusterknotentypen](service-fabric-cluster-nodetypes.md).

Für jeden Cluster muss ein **primärer Knotentyp** festgelegt werden, der kritische Systemdienste ausführt, die Service Fabric-Plattformfeatures bereitstellen. Obwohl es auch möglich ist, primäre Knotentypen für die Ausführung Ihrer Anwendungen zu verwenden, wird empfohlen, diese nur für die Ausführung von Systemdiensten einzusetzen.

**Nicht primäre Knotentypen** können verwendet werden, um Anwendungsrollen (wie *Front-End-* oder *Back-End-Dienste*) zu definieren und die Dienste in einem Cluster physisch zu isolieren. Service Fabric-Cluster können null oder mehr nicht primäre Knotentypen aufweisen.

Der primäre Knotentyp wird mit dem Attribut `isPrimary` unter der Knotentypdefinition in der Azure Resource Manager-Vorlage konfiguriert. Die vollständige Liste der Knotentypeigenschaften finden Sie unter [NodeTypeDescription-Objekt](/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object). Öffnen Sie eine *AzureDeploy.json*-Datei aus den [Service Fabric-Clusterbeispielen](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/), und suchen Sie mit *Auf Seite suchen* nach dem `nodeTypes`-Objekt, um die Verwendung zu testen.

### <a name="node-type-planning-considerations"></a>Aspekte der Knotentypplanung

Die Anzahl der ursprünglichen Knoten hängt vom Zweck des Clusters und den darauf ausgeführten Anwendungen und Diensten ab. Stellen Sie sich die folgenden Fragen:

* ***Weist Ihre Anwendung mehrere Dienste auf, und müssen einige dieser Dienste öffentlich sein oder über Internetzugriff verfügen?***

    Typische Anwendungen umfassen einen Front-End-Gatewaydienst, der Eingaben von einem Client empfängt, und einen oder mehrere Back-End-Dienste, die mit den Front-End-Diensten kommunizieren. Dabei gibt es ein separates Netzwerk für Front-End- und Back-End-Dienste. In solchen Fällen sind normalerweise drei Knotentypen erforderlich: ein primärer Knotentyp und zwei nicht primäre Knotentypen (einen für jeden Front-End- und Back-End-Dienst).

* ***Haben die Dienste, aus denen sich Ihre Anwendung zusammensetzt, unterschiedliche Infrastrukturanforderungen, z. B. höhere RAM-Anforderungen oder längere CPU-Zyklen?***

    Der Front-End-Dienst kann häufig auf kleineren virtuellen Computern (VM-Größen wie D2) ausgeführt werden, die über für das Internet geöffnete Ports verfügen.  Rechenintensive Back-End-Dienste hingegen müssen auf größeren virtuellen Computern (mit VM-Größen wie D4, D6, D15) ausgeführt werden, die nicht mit dem Internet verbunden sind. Wenn Sie unterschiedliche Knotentypen für diese Dienste definieren, können Sie die zugrunde liegenden Service Fabric-VMs effizienter und sicherer nutzen und diese unabhängig voneinander skalieren. Weitere Informationen zur Einschätzung der benötigten Ressourcenmenge finden Sie unter [Kapazitätsplanung für Service Fabric-Anwendungen](service-fabric-capacity-planning.md).

* ***Muss einer der Anwendungsdienste auf über 100 Knoten aufskaliert werden?***

    Ein einzelner Knotentyp kann für Service Fabric-Anwendungen nicht zuverlässig über 100 Knoten pro VM-Skalierungsgruppe skaliert werden. Wenn Sie mehr als 100 Knoten ausführen, benötigen Sie eine weitere VM-Skalierungsgruppe (und somit zusätzliche Knotentypen).

* ***Erstreckt sich Ihr Cluster über Verfügbarkeitszonen?***

    Service Fabric unterstützt Cluster, die sich über [Verfügbarkeitszonen](../availability-zones/az-overview.md) erstrecken, indem Knotentypen bereitgestellt werden, die an bestimmte Zonen angeheftet sind. So wird die Hochverfügbarkeit Ihrer Anwendungen sichergestellt. Für Verfügbarkeitszonen müssen Sie die Knotentypen anders planen, und es gelten höhere Mindestanforderungen. Weitere Informationen finden Sie unter [Die empfohlene Topologie für das Verteilen eines primären Knotentyps über Verfügbarkeitszonen](service-fabric-cross-availability-zones.md#recommended-topology-for-spanning-a-primary-node-type-across-availability-zones).

Wenn Sie die Anzahl und die Eigenschaften von Knotentypen für die Erstellung Ihres Clusters bestimmen, müssen Sie bedenken, dass Sie auch nach der Bereitstellung Ihres Clusters jederzeit nicht primäre Knotentypen hinzufügen, bearbeiten oder entfernen können. [Primäre Knotentypen können auch in ausgeführten Clustern hoch- oder herunterskaliert werden.](service-fabric-scale-up-primary-node-type.md) Dazu müssen Sie jedoch einen neuen Knotentyp erstellen, die Workload dorthin verschieben und dann den ursprünglichen primären Knotentyp entfernen.

Ein weiterer zu berücksichtigender Aspekt für Ihre Knotentypeigenschaften ist der Dauerhaftigkeitsgrad, der über die Berechtigungen der VMs eines Knotentyps in der Azure-Infrastruktur entscheidet. Verwenden Sie die VM-Größen, die Sie für Ihre Cluster auswählen, und die Anzahl der Instanzen, die Sie einzelnen Knotentypen zuweisen, um den geeigneten Dauerhaftigkeitsgrad für die jeweiligen Knotentypen festzulegen. Dies wird im Folgenden erläutert.

## <a name="durability-characteristics-of-the-cluster"></a>Dauerhaftigkeitsmerkmale des Clusters

Der *Dauerhaftigkeitsgrad* entscheidet über die Berechtigungen Ihrer Service Fabric-VMs in der zugrunde liegenden Azure-Infrastruktur. Mit dieser Berechtigung kann Service Fabric Infrastrukturanforderungen auf VM-Ebene anhalten (z. B. einen Neustart, ein Reimaging oder eine Migration), die sich auf die Quorumanforderungen für Service Fabric-Systemdienste und Ihre zustandsbehafteten Dienste auswirken.

> [!IMPORTANT]
> Der Dauerhaftigkeitsgrad wird pro Knotentyp festgelegt. Wenn kein Wert angegeben ist, wird *Bronze* verwendet, es werden jedoch keine automatischen Betriebssystemupgrades durchgeführt. Die Dauerhaftigkeit *Silber* oder *Gold* wird für Produktionsworkloads empfohlen.

In der folgenden Tabelle werden die Dauerhaftigkeitsgrade für Service Fabric sowie die jeweiligen Anforderungen aufgeführt.

| Dauerhaftigkeitsstufe  | Erforderliche Mindestanzahl von VMs | Unterstützte VM-Größen                                                                  | Aktualisierungen, die Sie an Ihrer VM-Skalierungsgruppe vornehmen                               | Updates und Wartung, initiiert von Azure                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Gold             | 5                              | Vollknotengrößen speziell für einen einzelnen Kunden (z. B. L32s, GS5, G5, DS15_v2, D15_v2) | Kann bis zur Genehmigung durch den Service Fabric-Cluster verzögert werden | Kann für 2 Stunden pro Upgradedomäne angehalten werden, damit zusätzliche Zeit für die Wiederherstellung von Replikaten nach früheren Fehlern verfügbar ist |
| Silber           | 5                              | Virtuelle Computer mit einem Kern oder höher mit mindestens 50 GB lokalem SSD-Speicher                      | Kann bis zur Genehmigung durch den Service Fabric-Cluster verzögert werden | Kann nicht für eine längere Zeit verzögert werden                                                    |
| Bronze          | 1                              | Virtuelle Computer mit mindestens 50 GB lokaler SSD-Kapazität                                              | Wird nicht durch den Service Fabric-Cluster verzögert           | Kann nicht für eine längere Zeit verzögert werden                                                    |

> [!NOTE]
> Die oben genannte Mindestanzahl von VMs ist für jeden Dauerhaftigkeitsgrad eine notwendige Anforderung. Wir führen Überprüfungen durch, die das Erstellen von VM-Skalierungsgruppen, die diese Anforderungen nicht erfüllen, oder dementsprechendes Ändern vorhandener VM-Skalierungsgruppen verhindern.

> [!WARNING]
> Beim Dauerhaftigkeitsgrad „Bronze“ sind keine automatischen Betriebssystemupgrades verfügbar. Während die [Anwendung zur Patchorchestrierung](service-fabric-patch-orchestration-application.md) (nur für nicht in Azure gehostete Cluster) für den Dauerhaftigkeitsgrad Silber oder höher *nicht empfohlen* wird, stellt diese die einzige Möglichkeit dar, Windows-Updates unter Einbeziehung von Service Fabric-Upgradedomänen zu automatisieren.

> [!IMPORTANT]
> Unabhängig vom Dauerhaftigkeitsgrad wird der Cluster durch die Ausführung einer [Belegungsfreigabe](/rest/api/compute/virtualmachinescalesets/deallocate) in einer VM-Skalierungsgruppe zerstört.

### <a name="bronze"></a>Bronze

Knotentypen, die mit dem Dauerhaftigkeitsgrad „Bronze“ ausgeführt werden, erhalten keine Berechtigungen. Das bedeutet, dass die Infrastrukturaufträge, die sich auf die zustandsbehafteten Workloads auswirken, nicht angehalten oder verzögert werden. Verwenden Sie den Dauerhaftigkeitsgrad „Bronze“ nur für Knotentypen, die zustandslose Workloads ausführen. Für Produktionsworkloads wird die Ausführung unter „Silber“ oder höher empfohlen.

### <a name="silver-and-gold"></a>Silber und Gold

Verwenden Sie die Dauerhaftigkeitsgrade „Silber“ oder „Gold“ für alle Knotentypen, die zustandsbehaftete Dienste hosten, für die Sie eine häufige horizontale Herunterskalierung erwarten und für die Bereitstellungsvorgänge verzögert und Kapazitäten reduziert werden sollen, um den Vorgang zu vereinfachen. Szenarios mit horizontaler Skalierung sollten die Wahl des Dauerhaftigkeitsgrads jedoch nicht beeinflussen.

#### <a name="advantages"></a>Vorteile

* Reduziert die Anzahl der erforderlichen Schritte in einem Vorgang zur horizontalen Herunterskalierung (Knotendeaktivierung und Remove-ServiceFabricNodeState werden automatisch aufgerufen)
* Reduziert das Risiko eines Datenverlusts aufgrund von direkten Änderungen der VM-Größe oder von Azure-Infrastrukturvorgängen

#### <a name="disadvantages"></a>Nachteile

* Für Bereitstellungen in Ihren VM-Skalierungsgruppen (und anderen zugehörigen Azure-Ressourcen) kann eine Verzögerung bzw. ein Timeout auftreten, oder sie können durch Probleme in Ihrem Cluster oder auf Infrastrukturebene vollständig blockiert werden.
* Erhöht die Anzahl der [Lebenszyklusereignisse für Replikate](service-fabric-reliable-services-lifecycle.md) (z.B. Tausch des primären Replikats) aufgrund von automatischen Knotendeaktivierungen während Azure-Infrastrukturvorgängen.
* Stellt Knoten für bestimmte Zeiträume außer Dienst, während Updates der Azure-Plattformsoftware oder Aktivitäten zur Hardwarewartung durchgeführt werden. Es kann sein, dass Knoten während dieser Aktivitäten den Status „Wird deaktiviert“ oder „Deaktiviert“ aufweisen. Hierdurch reduziert sich die Kapazität Ihres Clusters vorübergehend, aber es sollten sich keine Auswirkungen auf die Verfügbarkeit Ihres Clusters oder Ihrer Anwendungen ergeben.

#### <a name="best-practices-for-silver-and-gold-durability-node-types"></a>Bewährte Methoden für die Dauerhaftigkeitsgrade „Silber“ und „Gold“

Berücksichtigen Sie die folgenden Empfehlungen für die Verwaltung von Knotentypen mit der Dauerhaftigkeit „Silber“ oder „Gold“:

* Halten Sie Ihren Cluster und Anwendungen jederzeit fehlerfrei, und stellen Sie sicher, dass Anwendungen rechtzeitig auf alle [Lebenszyklus-Dienstereignisse für Replikate](service-fabric-reliable-services-lifecycle.md) (z.B. Unterbrechung der Replikaterstellung) reagieren.
* Führen Sie sicherere Methoden für die Änderung von VM-Größen (Hoch-/Herunterskalieren) ein. Das Ändern der VM-Größe einer VM-Skalierungsgruppe muss sorgfältig geplant und durchgeführt werden. Weitere Informationen finden Sie unter [Hochskalieren des primären Knotentyps eines Service Fabric-Clusters](service-fabric-scale-up-primary-node-type.md).
* Verwalten Sie mindestens fünf Knoten für alle VM-Skalierungsgruppen, für die die Dauerhaftigkeitsstufen „Gold“ oder „Silber“ aktiviert wurden. Der Cluster wechselt in den Fehlerzustand, wenn Sie unter diesem Schwellenwert abskalieren, und Sie müssen den Status (`Remove-ServiceFabricNodeState`) für die entfernten Knoten manuell bereinigen.
* Jede VM-Skalierungsgruppe mit der Dauerhaftigkeitsstufe „Silber“ oder „Gold“ muss einem eigenen Knotentyp im Service Fabric-Cluster zugeordnet werden. Das Zuordnen mehrerer VM-Skalierungsgruppen zu einem einzelnen Knotentyp verhindert die ordnungsgemäße Koordinierung zwischen dem Service Fabric-Cluster und der Azure-Infrastruktur.
* Löschen Sie keine zufälligen VM-Instanzen, sondern verwenden Sie immer die Funktion zum Abskalieren für VM-Skalierungsgruppen. Das Löschen von zufälligen VM-Instanzen kann zu Ungleichheiten in der auf [Upgradedomänen](service-fabric-cluster-resource-manager-cluster-description.md#upgrade-domains) und [Fehlerdomänen](service-fabric-cluster-resource-manager-cluster-description.md#fault-domains) verteilten VM-Instanz führen. Durch eine solche Ungleichheit kann das System ggf. keinen ordnungsgemäßen Lastenausgleich zwischen den Dienstinstanzen und Dienstreplikaten mehr durchführen.
* Wenn Sie die Autoskalierung verwenden, legen Sie die Regeln so fest, dass die Abskalierung (Entfernung von VM-Instanzen) jeweils nur für einen Knoten ausgeführt wird. Es ist nicht sicher, mehrere Instanzen gleichzeitig zu skalieren.
* Beim Löschen oder Freigeben von VMs auf dem primären Knotentyp sollten Sie niemals die Anzahl der zugeordneten virtuellen Computer unter die Anforderungen des Zuverlässigkeitsgrads reduzieren. Diese Vorgänge werden in einer Skalierungsgruppe mit der Dauerhaftigkeitsstufe „Silber“ oder „Gold“ auf unbestimmte Zeit blockiert.

### <a name="changing-durability-levels"></a>Ändern von Dauerhaftigkeitsstufen

Innerhalb bestimmter Einschränkungen kann die der Dauerhaftigkeitsgrad für Knotentypen angepasst werden:

* Knotentypen mit dem Dauerhaftigkeitsgrad „Silber“ oder „Gold“ können nicht auf „Bronze“ herabgestuft werden.
* Das Upgrade von „Bronze“ auf „Silber“ oder „Gold“ kann einige Stunden dauern.
* Stellen Sie beim Ändern des Dauerhaftigkeitsgrads sicher, dass diese sowohl in der Service Fabric-Erweiterungskonfiguration der VM-Skalierungsgruppenressource als auch in der Knotentypdefinition der Service Fabric-Clusterressource aktualisiert wird. Diese Werte müssen übereinstimmen.

Ein weiterer Aspekt bei der Kapazitätsplanung ist der Zuverlässigkeitsgrad für Ihren Cluster, der über die Stabilität der Systemdienste und Ihres gesamten Clusters entscheidet. Dies wird im nächsten Abschnitt beschrieben.

## <a name="reliability-characteristics-of-the-cluster"></a>Zuverlässigkeitsmerkmale des Clusters

Der *Zuverlässigkeitsgrad* des Clusters entscheidet über die Anzahl der Systemdienstreplikate, die auf dem primären Knotentyp des Clusters ausgeführt werden. Je mehr Replikate, desto zuverlässiger sind die Systemdienste (und somit der gesamte Cluster).

> [!IMPORTANT]
> Der Zuverlässigkeitsgrad wird auf Clusterebene festgelegt und entscheidet über die Mindestanzahl der Knoten des primären Knotentyps. Für Produktionsworkloads muss mindestens der Zuverlässigkeitsgrad „Silber“ (größer oder gleich fünf Knoten) festgelegt werden.  

Für die Zuverlässigkeitsstufe können folgende Werte festgelegt werden:

* **Platin:** Systemdienste werden mit neun Zielreplikatgruppen ausgeführt.
* **Gold:** Systemdienste werden mit sieben Zielreplikatgruppen ausgeführt.
* **Silber:** Systemdienste werden mit fünf Zielreplikatgruppen ausgeführt.
* **Bronze:** Systemdienste werden mit drei Zielreplikatgruppen ausgeführt.

Dies ist die Empfehlung für die Auswahl der Zuverlässigkeitsstufe. Die Anzahl von Seedknoten wird ebenfalls auf die Mindestanzahl von Knoten für eine Zuverlässigkeitsstufe festgelegt.


| **Anzahl von Knoten** | **Zuverlässigkeitsstufe** |
| --- | --- |
| 1 | *Geben Sie den Parameter `reliabilityLevel` nicht an, denn er wird vom System berechnet.* |
| 3 | Bronze |
| 5 oder 6| Silber |
| 7 oder 8 | Gold |
| 9 und höher | Platin |

Wenn Sie den Cluster vergrößern oder verkleinern (die Summe der VM-Instanzen in allen Knotentypen), sollten Sie ggf. den Zuverlässigkeitgrad des Clusters ändern. Durch diesen Vorgang werden die erforderlichen Clusterupgrades ausgelöst, um die Replikatgruppenanzahl der Systemdienste zu ändern. Warten Sie, bis das laufende Upgrade abgeschlossen ist, ehe Sie Änderungen am Cluster vornehmen, beispielsweise Knoten hinzufügen.  Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen von [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade) verfolgen.

### <a name="capacity-planning-for-reliability"></a>Kapazitätsplanung für Zuverlässigkeit

Die Kapazitätsanforderungen Ihres Clusters richten sich nach Ihren Anforderungen an Workloads und Zuverlässigkeit. Dieser Abschnitt enthält allgemeine Anleitungen für den Einstieg in die Kapazitätsplanung.

#### <a name="virtual-machine-sizing"></a>VM-Größen

**Für Produktionsworkloads wird die VM-Größe (SKU) [Standard D2_V2](../virtual-machines/dv2-dsv2-series.md) (oder gleichwertig) mit mindestens 50 GB lokalem SSD-Speicher, 2 Kernen und 4 GiB Arbeitsspeicher empfohlen.** Es wird mindestens 50 GB lokaler SSD-Speicher empfohlen. Für einige Workloads (z. B. solche, die Windows-Container ausführen) werden jedoch größere Datenträger benötigt. Wenn Sie [andere VM-Größen](../virtual-machines/sizes-general.md) für Produktionsworkloads auswählen, müssen Sie die folgenden Einschränkungen beachten:

- VM-Größen mit partiellem Kern wie Standard A0 werden nicht unterstützt.
- VM-Größen der *A-Serie* werden aus Leistungsgründen nicht unterstützt.
- Virtuelle Computer mit niedriger Priorität werden nicht unterstützt.

#### <a name="primary-node-type"></a>Primärer Knotentyp

Für **Produktionsworkloads** in Azure sind mindestens fünf primäre Knoten (VM-Instanzen) und der Zuverlässigkeitsgrad „Silber“ erforderlich. Es wird empfohlen, den primären Knotentyp für Systemdienste zu reservieren und Platzierungseinschränkungen zu verwenden, um Ihre Anwendung auf den sekundären Knotentypen bereitzustellen.

**Testworkloads** in Azure können eine Mindestanzahl von einem oder drei Knoten ausführen. Stellen Sie bei der Konfiguration eines Knotenclusters sicher, dass die Einstellung `reliabilityLevel` in Ihrer Resource Manager-Vorlage nicht vorhanden ist (die Angabe eines leeren Zeichenfolgenwerts für `reliabilityLevel` ist nicht ausreichend). Wenn Sie über das Azure-Portal einen Cluster mit einem Knoten eingerichtet haben, erfolgt diese Konfiguration automatisch.

> [!WARNING]
> Cluster mit einem Knoten werden ohne Zuverlässigkeit und mit einer speziellen Konfiguration ausgeführt, wenn das Aufskalieren nicht unterstützt wird.

#### <a name="non-primary-node-types"></a>Nicht primäre Knotentypen

Die Mindestanzahl der Knoten für einen nicht primären Knotentyp hängt vom jeweiligen [Dauerhaftigkeitsgrad](#durability-characteristics-of-the-cluster) des Knotentyps ab. Sie sollten die Anzahl der Knoten (und den Dauerhaftigkeitsgrad) anhand der Anzahl der Replikate für Anwendungen oder Dienste planen, die für den Knotentyp ausgeführt werden sollen. Zudem spielt eine Rolle, ob die Workload zustandsbehaftet oder zustandslos ist. Denken Sie daran, dass Sie die Anzahl der VMs in einem Knotentyp nach der Bereitstellung des Clusters jederzeit erhöhen oder verringern können.

##### <a name="stateful-workloads"></a>Zustandsbehaftete Workloads

Für zustandsbehaftete Workloads, die [zuverlässige Sammlungen oder Reliable Actors](service-fabric-choose-framework.md) von Service Fabric verwenden, werden mindestens fünf Zielreplikate empfohlen. So ist im stabilen Zustand in jeder Fehlerdomäne und jeder Upgradedomäne ein Replikat (aus einer Replikatgruppe) vorhanden. Im Allgemeinen verwenden Sie den Zuverlässigkeitsgrad, den Sie für Systemdienste festlegen, als Richtwert für die Replikatanzahl, die Sie für Ihre zustandsbehafteten Dienste verwenden.

##### <a name="stateless-workloads"></a>Zustandslose Workloads

Für zustandslose Produktionsworkloads müssen mindestens drei nicht primäre Knotentypen unterstützt werden, um das Quorum aufrechtzuerhalten. Es werden jedoch fünf Knotentypen empfohlen.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Ihren Cluster konfigurieren, sollten Sie die [Clusterupgraderichtlinien `Not Allowed`](service-fabric-cluster-fabric-settings.md) lesen, damit Sie Ihre Cluster später nicht neu erstellen müssen, weil andernfalls nicht änderbare Systemkonfigurationseinstellungen vorliegen.

Weitere Informationen zur Clusterplanung:

* [Computeplanung und -skalierung](service-fabric-best-practices-capacity-scaling.md)
* [Kapazitätsplanung für Service Fabric-Anwendungen](service-fabric-capacity-planning.md)
* [Planen der Notfallwiederherstellung](service-fabric-disaster-recovery.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
