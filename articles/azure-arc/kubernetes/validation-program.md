---
title: Validierung für Kubernetes mit Azure Arc-Unterstützung
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Beschreibung des Arc-Validierungsprogramms für Kubernetes-Distributionen
keywords: Kubernetes, Arc, Azure, K8s, Validierung
ms.openlocfilehash: 5d8e48a6e9dc5d8d95dacb649fb0748e4ae4daa5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355631"
---
# <a name="azure-arc-enabled-kubernetes-validation"></a>Validierung für Kubernetes mit Azure Arc-Unterstützung

Kubernetes mit Azure Arc-Aktivierung funktioniert mit jedem Kubernetes-Cluster, der eine CNCF-Zertifizierung (Cloud Native Computing Foundation) besitzt. Das Azure Arc-Team hat auch mit den wichtigsten Anbietern von Kubernetes-Angeboten der Branche zusammengearbeitet, um Kubernetes mit Azure Arc-Aktivierung mit ihren Kubernetes-Distributionen zu überprüfen. Zukünftige Haupt- und Nebenversionen von Kubernetes-Distributionen, die von diesen Anbietern veröffentlicht werden, werden auf Kompatibilität mit Kubernetes mit Azure Arc-Aktivierung überprüft.

## <a name="validated-distributions"></a>Überprüfte Distributionen

Die folgenden von Microsoft bereitgestellten Kubernetes-Distributionen und Infrastrukturanbieter haben die Konformitätstests für Kubernetes mit Azure Arc-Aktivierung erfolgreich bestanden:

| Distributions- und Infrastrukturanbieter | Version |
| ---------------------------------------- | ------- |
| Cluster-API-Anbieter in Azure            | Releaseversion: [0.4.12](https://github.com/kubernetes-sigs/cluster-api-provider-azure/releases/tag/v0.4.12); Kubernetes-Version: [1.18.2](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.2) |
| AKS in Azure Stack HCI                   | Releaseversion: [Update für Dezember 2020](https://github.com/Azure/aks-hci/releases/tag/AKS-HCI-2012); Kubernetes-Version: [1.18.8](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.8) |

Die folgenden Anbieter und ihre entsprechenden Kubernetes-Distributionen haben die Konformitätstests für Kubernetes mit Azure Arc-Aktivierung erfolgreich bestanden:

| Anbietername | Distributionsname | Version |
| ------------ | ----------------- | ------- |
| RedHat       | [OpenShift Container Platform](https://www.openshift.com/products/container-platform) | [4.5.41+](https://docs.openshift.com/container-platform/4.5/release_notes/ocp-4-5-release-notes.html), [4.6.35+](https://docs.openshift.com/container-platform/4.6/release_notes/ocp-4-6-release-notes.html), [4.7.18+](https://docs.openshift.com/container-platform/4.7/release_notes/ocp-4-7-release-notes.html) |
| VMware       | [Tanzu Kubernetes Grid](https://tanzu.vmware.com/kubernetes-grid) | Kubernetes-Version: v1.17.5 |
| Canonical    | [Charmed Kubernetes](https://ubuntu.com/kubernetes) | [1.19](https://ubuntu.com/kubernetes/docs/1.19/components) |
| SUSE Rancher      | [Rancher Kubernetes Engine](https://rancher.com/products/rke/) | RKE CLI-Version: [v1.2.4](https://github.com/rancher/rke/releases/tag/v1.2.4); Kubernetes-Versionen: [1.19.6](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.6)), [1.18.14](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.14)), [1.17.16](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.16))  |
| Nutanix      | [Karbon](https://www.nutanix.com/products/karbon)    | Version 2.2.1 |

Das Azure Arc-Team führte auch die Konformitätstests durch und überprüfte Kubernetes-Szenarien mit Azure Arc-Aktivierung auf den folgenden öffentlichen Cloudanbietern:

| Name des Anbieters der öffentlichen Cloud | Distributionsname | Version |
| -------------------------- | ----------------- | ------- |
| Amazon Web Services        | Elastic Kubernetes Service (EKS) | v1.18.9  |
| Google Cloud Platform      | Google Kubernetes Engine (GKE) | v1.17.15 |

## <a name="scenarios-validated"></a>Überprüfte Szenarien

Die Konformitätstests, die im Rahmen der Kubernetes-Validierung mit Azure Arc-Aktivierung durchgeführt werden, decken die folgenden Szenarien ab:

1. Verbinden von Kubernetes-Clustern mit Azure Arc: 
    * Stellen Sie das Helm-Chart für den Kubernetes-Agent mit Azure Arc-Aktivierung im Cluster bereit.
    * Richten Sie das MSI-Zertifikat (Managed System Identity) auf dem Cluster ein.
    * Agents senden Clustermetadaten an Azure.

2. Konfiguration: 
    * Erstellen Sie die Konfiguration zusätzlich zu einer Kubernetes-Ressource mit Azure Arc-Aktivierung.
    * [Flux](https://docs.fluxcd.io/), das zum Einrichten des GitOps-Workflows erforderlich ist, wird auf dem Cluster bereitgestellt.
    * Flux pullt Manifeste und Helm-Charts aus dem Git-Repository der Demo und stellt sie im Cluster bereit.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie einen Cluster mit Azure Arc verbinden.
> [!div class="nextstepaction"]
> [Verbinden eines Clusters mit Azure Arc](./quickstart-connect-cluster.md)
