---
author: peterclu
ms.service: machine-learning
ms.topic: include
ms.date: 03/08/2021
ms.author: peterlu
ms.openlocfilehash: d6d73754a22a3a0870a6ea141e4a21e7f2ccff09
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2021
ms.locfileid: "122688769"
---
In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen ML Studio (Classic) und Azure Machine Learning zusammengefasst:

| Funktion | ML Studio (klassisch) | Azure Machine Learning |
|---| --- | --- |
| Drag & Drop-Oberfläche | Klassische Umgebung | Aktualisierte Umgebung – [Azure Machine Learning-Designer](../articles/machine-learning/concept-designer.md)| 
| Code SDKs | Nicht unterstützt | Vollständig integriert in [Azure Machine Learning Python](/python/api/overview/azure/ml/) und [R](https://github.com/Azure/azureml-sdk-for-r) SDKs |
| Experiment | Skalierbar (Limit für Trainingsdaten: 10 GB) | Skalieren mit Computeziel |
| Trainieren von Computezielen | Proprietäres Computeziel, nur CPU-Unterstützung | Viele anpassbare [Computeziele für das Training](../articles/machine-learning/concept-compute-target.md#train). GPU- und CPU-Unterstützung | 
| Computeziele für die Bereitstellung | Proprietäres Webdienstformat, nicht anpassbar | Viele anpassbare [Computeziele für die Bereitstellung](../articles/machine-learning/concept-compute-target.md#deploy). GPU- und CPU-Unterstützung |
| ML-Pipeline | Nicht unterstützt | Erstellung von flexiblen modularen [Pipelines](../articles/machine-learning/concept-ml-pipelines.md) zum Automatisieren von Workflows |
| MLOps | Grundlegende Modellverwaltung und Bereitstellung; reine CPU-Bereitstellungen | Entitätsversionierung (Modell, Daten, Workflows), Workflowautomatisierung, Integration in CICD-Tools, CPU- und GPU-Bereitstellungen [und mehr](../articles/machine-learning/concept-model-management-and-deployment.md) |
| Modellformat | Proprietäres Format, nur Studio (klassisch) | Mehrere unterstützte Formate, je nach Typ des Trainingsauftrags |
| Automatisiertes Modelltraining und Optimieren von Hyperparametern |  Nicht unterstützt | [Unterstützt](../articles/machine-learning/concept-automated-ml.md). Code First-Optionen und codefreie Optionen | 
| Datendrifterkennung | Nicht unterstützt | [Unterstützt](../articles/machine-learning/how-to-monitor-datasets.md) |
| Datenbezeichnungsprojekte | Nicht unterstützt | [Unterstützt](../articles/machine-learning/how-to-create-labeling-projects.md) |
| Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) | Nur Rolle „Mitwirkender“ und „Besitzer“ | [Flexible Rollendefinition und RBAC-Steuerung](../articles/machine-learning/how-to-assign-roles.md) |
| KI-Katalog | Unterstützt ([https://gallery.azure.ai/](https://gallery.azure.ai/)) | Nicht unterstützt <br><br> Lernen mit [Python SDK-Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks) |