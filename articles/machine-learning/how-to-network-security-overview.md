---
title: Schützen von Arbeitsbereichsressourcen mit virtuellen Netzwerken (VNETs)
titleSuffix: Azure Machine Learning
description: Schützen Sie Arbeitsbereichsressourcen und Compute-Umgebungen von Azure Machine Learning mithilfe eines isolierten Azure Virtual Network (VNet).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: peterlu
author: peterclu
ms.date: 06/11/2021
ms.topic: how-to
ms.custom: devx-track-python, references_regions, contperf-fy21q1,contperf-fy21q4,FY21Q4-aml-seo-hack, security
ms.openlocfilehash: 06dc1a34f35434019d1b992c12502577aa470360
ms.sourcegitcommit: 6f21017b63520da0c9d67ca90896b8a84217d3d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114653489"
---
<!-- # Virtual network isolation and privacy overview -->
# <a name="secure-azure-machine-learning-workspace-resources-using-virtual-networks-vnets"></a>Schützen von Azure Machine Learning-Arbeitsbereichsressourcen mit virtuellen Netzwerken (VNets)

Schützen Sie Arbeitsbereichsressourcen und Compute-Umgebungen von Azure Machine Learning mithilfe virtueller Netzwerke (VNets). Hier wird anhand eines Beispielszenarios veranschaulicht, wie Sie ein vollständiges virtuelles Netzwerk konfigurieren.

> [!TIP]
> Dieser Artikel ist Teil einer Reihe zum Schützen eines Azure Machine Learning-Workflows. Sehen Sie sich auch die anderen Artikel in dieser Reihe an:
>
> * [Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md)
> * [Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
> * [Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md)
> * [Aktivieren von Studio-Funktionalität](how-to-enable-studio-virtual-network.md)
> * [Verwenden von benutzerdefiniertem DNS](how-to-custom-dns.md)
> * [Verwenden einer Firewall](how-to-access-azureml-behind-firewall.md)

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie mit den folgenden Themen vertraut sind:
+ [Virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md)
+ [IP-Netzwerke](../virtual-network/public-ip-addresses.md)
+ [Azure Machine Learning-Arbeitsbereich mit privatem Endpunkt](how-to-configure-private-link.md)
+ [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/network-security-groups-overview.md):
+ [Netzwerkfirewalls](../firewall/overview.md)
## <a name="example-scenario"></a>Beispielszenario

In diesem Abschnitt erfahren Sie, wie ein Netzwerkszenario grundsätzlich eingerichtet wird, um die Azure Machine Learning-Kommunikation mit privaten IP-Adressen zu schützen.

In der folgenden Tabelle sehen Sie in einer Gegenüberstellung, wie Dienste mit und ohne VNet auf verschiedene Teile eines Azure Machine Learning-Netzwerks zugreifen:

| Szenario | Arbeitsbereich | Zugeordnete Ressourcen | Trainingscompute-Umgebung | Rückschlusscompute-Umgebung |
|-|-|-|-|-|-|
|**Kein virtuelles Netzwerk**| Öffentliche IP-Adresse | Öffentliche IP-Adresse | Öffentliche IP-Adresse | Öffentliche IP-Adresse |
|**Sichere Ressourcen in einem virtuellen Netzwerk**| Private IP-Adresse (privater Endpunkt) | Öffentliche IP-Adresse (Dienstendpunkt) <br> **oder** <br> Private IP-Adresse (privater Endpunkt) | Private IP-Adresse | Private IP-Adresse  | 

* **Arbeitsbereich**: Erstellen Sie einen privaten Endpunkt für Ihren Arbeitsbereich. Der private Endpunkt verbindet den Arbeitsbereich über mehrere private IP-Adressen mit dem VNET.
* **Zugehörige Ressourcen:** Stellen Sie mithilfe von Dienstendpunkten oder privaten Endpunkten eine Verbindung mit Arbeitsbereichsressourcen wie Azure Storage, Azure Key Vault oder Azure Container Services her.
    * **Dienstendpunkte** geben die Identität Ihres virtuellen Netzwerks gegenüber dem Azure-Dienst an. Nachdem Sie Dienstendpunkte in Ihrem virtuellen Netzwerk aktiviert haben, können Sie eine virtuelle Netzwerkregel hinzufügen, um die Azure-Dienstressourcen in Ihrem virtuellen Netzwerk zu schützen. Dienstendpunkte nutzen öffentliche IP-Adressen.
    * **Private Endpunkte** sind Netzwerkschnittstellen, die das Herstellen einer sicheren Verbindung mit einem Dienst gestatten, der über Private Link betrieben wird. Ein privater Endpunkt verwendet eine private IP-Adresse in Ihrem VNET und bindet den Dienst so effektiv in das VNET ein.
* **Trainingscomputezugriff:** Hiermit greifen Sie sicher auf Computeziele zu, die zu Trainingszwecken verwendet werden, wie beispielsweise Azure Machine Learning-Compute-Instanz oder Azure Machine Learning-Computecluster. 
* **Rückschlusscomputezugriff:** Hiermit greifen Sie mit privaten IP-Adressen auf AKS-Computecluster (Azure Kubernetes Services) zu.


In den nächsten Abschnitten wird gezeigt, wie Sie das oben beschriebene Netzwerkszenario absichern. Zum Schützen Ihres Netzwerks müssen Sie folgende Schritte ausführen:

1. [**Arbeitsbereich und zugehörige Ressourcen schützen**](#secure-the-workspace-and-associated-resources)
1. [**Trainingsumgebung schützen**](#secure-the-training-environment)
1. [**Rückschlussumgebung schützen**](#secure-the-inferencing-environment)
1. [**Studio-Funktionalität aktivieren**](#optional-enable-studio-functionality) (optional)
1. [**Firewalleinstellungen**](#configure-firewall-settings) konfigurieren
1. [DNS-Namensauflösung](#custom-dns) konfigurieren
## <a name="secure-the-workspace-and-associated-resources"></a>Schützen des Arbeitsbereichs und zugehöriger Ressourcen

Führen Sie die folgenden Schritte aus, um Ihren Arbeitsbereich und zugehörige Ressourcen zu schützen. Danach können Ihre Dienste im virtuellen Netzwerk kommunizieren.

1. Erstellen Sie einen [Arbeitsbereich mit Private Link-Unterstützung](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint), um die Kommunikation zwischen Ihrem VNET und dem Arbeitsbereich zu ermöglichen.
1. Fügen Sie dem virtuellen Netzwerk die folgenden Dienste hinzu. Verwenden Sie dazu _entweder_ einen __Dienstendpunkt__ oder einen __privaten Endpunkt__. Gewähren Sie außerdem vertrauenswürdigen Microsoft-Diensten Zugriff auf diese Dienste:
    
    | Dienst | Endpunktinformationen | Zulassen vertrauenswürdiger Informationen |
    | ----- | ----- | ----- |
    | __Azure Key Vault__| [Dienstendpunkt](../key-vault/general/overview-vnet-service-endpoints.md)</br>[Privater Endpunkt](../key-vault/general/private-link-service.md) | [Erlauben der Umgehung dieser Firewall für vertrauenswürdige Microsoft-Dienste](how-to-secure-workspace-vnet.md#secure-azure-key-vault) |
    | __Azure Storage-Konto__ | [Dienstendpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints)</br>[Privater Endpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints) | [Gewähren von Zugriff für vertrauenswürdige Azure-Dienste](../storage/common/storage-network-security.md#grant-access-to-trusted-azure-services) |
    | __Azure Container Registry__ | [Dienstendpunkt](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr)</br>[Privater Endpunkt](../container-registry/container-registry-private-link.md) | [Zulassen vertrauenswürdiger Dienste](../container-registry/allow-access-trusted-services.md) |


![Architekturdiagramm, das darstellt, wie der Arbeitsbereich und die zugehörigen Ressourcen über Dienstendpunkte oder private Endpunkte in einem VNET miteinander kommunizieren](./media/how-to-network-security-overview/secure-workspace-resources.png)

Ausführliche Anweisungen zum Ausführen dieser Schritte finden Sie unter [Secure an Azure Machine Learning workspace](how-to-secure-workspace-vnet.md) (Schützen eines Azure Machine Learning-Arbeitsbereichs). 

### <a name="limitations"></a>Einschränkungen

Für das Schützen Ihres Arbeitsbereichs und zugehöriger Ressourcen in einem virtuellen Netzwerk gelten die folgenden Einschränkungen:
- Ein Azure Machine Learning-Arbeitsbereich mit privatem Endpunkt ist in den Regionen Azure Government und Azure China 21Vianet nicht verfügbar.
- Alle Ressourcen müssen sich innerhalb desselben VNET befinden. Sie können allerdings Subnetze innerhalb eines VNET einsetzen.

## <a name="secure-the-training-environment"></a>Schützen der Trainingsumgebung

In diesem Abschnitt erfahren Sie, wie Sie die Trainingsumgebung in Azure Machine Learning schützen. Außerdem wird beschrieben, wie Azure Machine Learning einen Trainingsauftrag durchführt. So können Sie besser verstehen, wie die Netzwerkkonfigurationen kooperieren.

Führen Sie die folgenden Schritte aus, um die Trainingsumgebung zu schützen:

1. [Erstellen Sie im virtuellen Netzwerk eine Azure Machine Learning-Compute-Instanz und einen Azure Machine Learning-Computercluster](how-to-secure-training-vnet.md#compute-instance), um den Trainingsauftrag auszuführen.
1. [Lassen Sie eingehende Kommunikation zu](how-to-secure-training-vnet.md#required-public-internet-access), damit Verwaltungsdienste Aufträge an Ihre Computeressourcen übermitteln können. 

![Architekturdiagramm, das die Absicherung verwalteter Computecluster und -instanzen zeigt](./media/how-to-network-security-overview/secure-training-environment.png)

Eine ausführliche Anleitung zum Ausführen dieser Schritte finden Sie unter [Secure an Azure Machine Learning training environment with virtual networks](how-to-secure-training-vnet.md) (Schützen einer Trainingsumgebung mit VNETs). 

### <a name="example-training-job-submission"></a>Beispiel für die Übermittlung eines Trainingsauftrags 

In diesem Abschnitt erfahren Sie, wie Azure Machine Learning für eine sichere Kommunikation zwischen Diensten sorgt, um einen Trainingsauftrag zu übermitteln. Sie können daraus ersehen, wie die von Ihnen vorgenommenen Konfigurationen in der Summe eine sichere Kommunikation gewährleisten.

1. Der Client lädt Trainingsskripts und Trainingsdaten in Speicherkonten hoch, die mit einem Dienst- oder einem privaten Endpunkt geschützt sind.

1. Der Client sendet einen Trainingsauftrag über den privaten Endpunkt an den Azure Machine Learning-Arbeitsbereich.

1. Der Azure Batch-Dienst empfängt den Auftrag aus dem Arbeitsbereich. Er übergibt dann den Trainingsauftrag über den öffentlichen Lastenausgleich für die Computeressource an die Compute-Umgebung. 

1. Die Computeressource erhält den Auftrag und startet das Training. Die Computeressource greift auf sichere Speicherkonten zu, um Trainingsdateien herunter- und die Ausgabe hochzuladen.

### <a name="limitations"></a>Einschränkungen

- Die Azure-Compute-Instanz und die Azure-Computecluster müssen sich im selben VNET, in derselben Region und im gleichen Abonnement wie der Arbeitsbereich und die zugehörigen Ressourcen befinden. 

## <a name="secure-the-inferencing-environment"></a>Schützen der Rückschlussumgebung

In diesem Abschnitt erfahren Sie, welche Möglichkeiten Ihnen zum Schützen einer Rückschlussumgebung zur Verfügung stehen. Es wird empfohlen, für umfangreiche Produktionsbereitstellungen AKS-Cluster (Azure Kubernetes Services) zu verwenden.

Für AKS-Cluster in einem virtuellen Netzwerk stehen Ihnen zwei Optionen bereit:

- Sie können einen AKS-Standardcluster bereitstellen oder ihn Ihrem VNET hinzufügen.
- Sie können Ihrem VNET einen privaten AKS-Cluster hinzufügen.

**AKS-Standardcluster** weisen eine Steuerungsebene mit öffentlichen IP-Adressen auf. Sie können entweder Ihrem VNET bei der Bereitstellung einen AKS-Standardcluster hinzufügen oder nach der Erstellung einen Cluster anfügen.

**Private AKS-Cluster** weisen eine Steuerungsebene auf, auf die nur über private IP-Adressen zugegriffen werden kann. Private AKS-Cluster müssen nach der Erstellung des Clusters angefügt werden.

Eine ausführliche Anleitung zum Hinzufügen von Standard-und privaten Clustern finden Sie unter [Secure an inferencing environment](how-to-secure-inferencing-vnet.md) (Schützen einer Rückschlussumgebung). 

Das folgende Netzwerkdiagramm zeigt einen geschützten Azure Machine Learning-Arbeitsbereich mit einem privaten AKS-Cluster, der an das virtuelle Netzwerk angefügt wurde.

![Architekturdiagramm, das Anfügen eines privaten AKS-Clusters an das virtuelle Netzwerk zeigt. Die AKS-Steuerungsebene wird außerhalb des Kunden-VNET platziert.](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>Einschränkungen
- AKS-Cluster müssen demselben VNET angehören wie der Arbeitsbereich und die zugehörigen Ressourcen. 

## <a name="optional-enable-public-access"></a>Optional: Aktivieren des öffentlichen Zugriffs

Sie können den Arbeitsbereich hinter einem VNet mithilfe eines privaten Endpunkts schützen und weiterhin den Zugriff über das öffentliche Internet zulassen. Die anfängliche Konfiguration entspricht der zum [Schützen von Arbeitsbereich und zugehörigen Ressourcen](#secure-the-workspace-and-associated-resources). 

Nachdem Sie den Arbeitsbereich mit einem privaten Endpunkt geschützt haben, [aktivieren Sie öffentlichen Zugriff](how-to-configure-private-link.md#enable-public-access). Anschließend können Sie über das öffentliche Internet und das VNet auf den Arbeitsbereich zugreifen.

### <a name="limitations"></a>Einschränkungen

- Wenn Sie Azure Machine Learning Studio über das öffentliche Internet verwenden, können einige Features wie der Designer möglicherweise nicht auf Ihre Daten zugreifen. Dieses Problem tritt auf, wenn die Daten in einem Dienst gespeichert werden, der hinter dem VNet geschützt ist. Dies gilt z. B. für ein Azure Storage-Konto.
## <a name="optional-enable-studio-functionality"></a>Aktivieren der Studio-Funktionalität (optional)

[Schützen des Arbeitsbereichs](#secure-the-workspace-and-associated-resources) > [Schützen der Trainingsumgebung](#secure-the-training-environment) > [Schützen der Rückschlussumgebung](#secure-the-inferencing-environment) > **Aktivieren der Studio-Funktionalität** > [Konfigurieren der Firewalleinstellungen](#configure-firewall-settings)

Wenn sich Ihr Speicher in einem VNet befindet, müssen Sie die volle Funktionalität in Studio durch zusätzliche Konfigurationsschritte aktivieren. Standardmäßig sind die folgenden Features deaktiviert:

* Vorschau der Daten im Studio.
* Visualisieren von Daten im Designer.
* Bereitstellen eines Modells im Designer.
* Senden eines AutoML-Experiments.
* Starten eines Beschriftungsprojekts.

Informationen zur Aktivierung der vollen Studio-Funktionalität finden Sie unter [Verwenden von Azure Machine Learning Studio in einem virtuellen Azure-Netzwerk](how-to-enable-studio-virtual-network.md).

### <a name="limitations"></a>Einschränkungen

Von der [ML-gestützten Datenbeschriftung](how-to-create-labeling-projects.md#use-ml-assisted-data-labeling) werden keine Standardspeicherkonten hinter einem virtuellen Netzwerk unterstützt. Verwenden Sie daher für die ML-gestützte Datenbeschriftung ein anderes Speicherkonto als das Standardkonto. 

> [!TIP]
> Solange es sich nicht um das Standardspeicherkonto handelt, kann das Konto für die Datenbeschriftung hinter dem virtuellen Netzwerk abgesichert werden. 

## <a name="configure-firewall-settings"></a>Konfigurieren der Firewalleinstellungen

Konfigurieren Sie Ihre Firewall für die Steuerung des Datenverkehrs zwischen Ihrem Azure Machine Learning-Arbeitsbereich und dem öffentlichen Internet. Zwar empfehlen wir Azure Firewall, aber Sie können auch andere Firewallprodukte einsetzen. 

Weitere Informationen zu Firewalleinstellungen finden Sie unter [Verwenden des Arbeitsbereichs hinter einer Firewall für Azure Machine Learning](how-to-access-azureml-behind-firewall.md).

## <a name="custom-dns"></a>Benutzerdefinierter DNS

Wenn Sie eine benutzerdefinierte DNS-Lösung für Ihr virtuelles Netzwerk verwenden möchten, müssen Sie Hosteinträge für Ihren Arbeitsbereich hinzufügen.

Weitere Informationen zu den erforderlichen Domänennamen und IP-Adressen finden Sie unter [Verwenden eines Arbeitsbereichs mit einem benutzerdefinierten DNS-Server](how-to-custom-dns.md).

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe zum Schützen eines Azure Machine Learning-Workflows. Sehen Sie sich auch die anderen Artikel in dieser Reihe an:

* [Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md)
* [Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
* [Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md)
* [Aktivieren von Studio-Funktionalität](how-to-enable-studio-virtual-network.md)
* [Verwenden von benutzerdefiniertem DNS](how-to-custom-dns.md)
* [Verwenden einer Firewall](how-to-access-azureml-behind-firewall.md)