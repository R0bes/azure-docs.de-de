---
title: Verwaltungshub
description: Verwalten Sie Ihre Verbindungen, Ihre Konfiguration für die Quellcodeverwaltung und Ihre globalen Erstellungseigenschaften im Azure Data Factory-Verwaltungshub.
ms.service: data-factory
ms.subservice: authoring
ms.topic: conceptual
author: minhe-msft
ms.author: hemin
ms.date: 04/27/2021
ms.openlocfilehash: 91b19a33df19fd474c9db282fd4467be54f8dd15
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355622"
---
# <a name="management-hub-in-azure-data-factory"></a>Verwaltungshub in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Der Verwaltungshub, den Sie in der Azure Data Factory-Benutzeroberfläche über die Registerkarte *Verwalten* aufrufen, ist ein Portal mit globalen Verwaltungsaktionen für Ihre Data Factory. Hier können Sie die Verbindungen mit Datenspeichern und externen Compute-Umgebungen, die Konfiguration der Quellcodeverwaltung und die Triggereinstellungen verwalten.

## <a name="manage-connections"></a>Verwalten von Verbindungen

### <a name="linked-services"></a>Verknüpfte Dienste

Verknüpfte Dienste definieren die Verbindungsinformationen für Azure Data Factory für das Herstellen einer Verbindung mit externen Datenspeichern und Compute-Umgebungen. Weitere Informationen finden Sie unter [Verknüpfte Dienste in Azure Data Factory](concepts-linked-services.md). Die Erstellung, Bearbeitung und Löschung von verknüpften Diensten erfolgt im Verwaltungshub.

![Verwalten von verknüpften Diensten](media/author-management-hub/management-hub-linked-services.png)

### <a name="integration-runtimes"></a>Integration Runtimes

Eine Integration Runtime ist eine Computeinfrastruktur, mit der Azure Data Factory Datenintegrationsfunktionen in verschiedenen Netzwerkumgebungen bereitstellt. Weitere Informationen finden Sie unter [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md). Im Verwaltungshub können Sie Integration Runtimes erstellen, löschen und überwachen.

![Verwalten von Integration Runtimes](media/author-management-hub/management-hub-integration-runtime.png)

## <a name="manage-source-control"></a>Verwalten der Quellcodeverwaltung

### <a name="git-configuration"></a>Git-Konfiguration

Sie können alle Git-bezogenen Informationen unter den Git-Konfigurationseinstellungen im Verwaltungshub anzeigen/bearbeiten. 

Die Informationen zum zuletzt veröffentlichten Commit sind ebenfalls aufgeführt und können zum besseren Verständnis des genauen Commits beitragen, der zuletzt in Umgebungen veröffentlicht/bereitgestellt wurde. Außerdem können sie bei Hotfixes in der Produktionsumgebung hilfreich sein.

Weitere Informationen finden Sie unter [Quellcodeverwaltung in Azure Data Factory](source-control.md).

![Verwalten des Git-Repositorys](media/author-management-hub/management-hub-git.png)

### <a name="parameterization-template"></a>Parametrisierungsvorlage

Sie können eine Datei mit benutzerdefinierten Parametern generieren oder bearbeiten, um bei Veröffentlichungen für einen Kollaborationsbranch die generierten Resource Manager-Vorlagenparameter außer Kraft zu setzen. Weitere Informationen finden Sie unter [Verwenden benutzerdefinierter Parameter mit der Resource Manager-Vorlage](continuous-integration-deployment.md#use-custom-parameters-with-the-resource-manager-template). Die Parametrisierungsvorlage ist nur bei der Arbeit in einem Git-Repository verfügbar. Wenn die Datei *arm-template-parameters-definition.json* im Arbeitsbranch nicht vorhanden ist, wird sie durch das Bearbeiten der Standardvorlage generiert.

![Verwalten von benutzerdefinierten Parametern](media/author-management-hub/management-hub-custom-parameters.png)

## <a name="manage-authoring"></a>Verwalten der Erstellung

### <a name="triggers"></a>Trigger

Trigger legen fest, wann mit dem Ausführen einer Pipeline begonnen werden soll. Derzeit können Trigger zu einer bestimmten Uhrzeit ausgelöst werden, in regelmäßigen Abständen oder abhängig von einem Ereignis. Weitere Informationen finden Sie unter [Triggerausführung](concepts-pipeline-execution-triggers.md#trigger-execution). Im Verwaltungshub können Sie Trigger erstellen, bearbeiten, löschen oder deren aktuellen Status anzeigen.

![Screenshot: Hier können Sie den aktuellen Status eines Trigger erstellen, bearbeiten, löschen und anzeigen.](media/author-management-hub/management-hub-triggers.png)

### <a name="global-parameters"></a>Globale Parameter

Globale Parameter sind Konstanten in einer Data Factory, die von einer Pipeline in beliebigen Ausdrücken verbraucht werden können. Weitere Informationen finden Sie unter [Globale Parameter](author-global-parameters.md).

![Erstellen globaler Parameter](media/author-global-parameters/create-global-parameter-3.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie ein [Git-Repository](source-control.md) für Ihre Azure Data Factory-Instanz konfigurieren.


