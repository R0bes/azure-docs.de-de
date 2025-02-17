---
title: CI/CD mit Azure Pipelines und Bicep-Dateien
description: Beschreibt die Konfiguration von Continuous Integration in Azure Pipelines mithilfe von Bicep-Dateien. Hier erfahren Sie, wie Sie eine Bicep-Datei mithilfe einer Azure CLI-Aufgabe bereitstellen können.
author: mumian
ms.topic: conceptual
ms.author: jgao
ms.date: 06/23/2021
ms.openlocfilehash: 28050d926bf5b4042ceb5b94796550bc517eb977
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114289209"
---
# <a name="integrate-bicep-with-azure-pipelines"></a>Integration von Bicep in Azure Pipelines

Sie können Bicep-Dateien in Azure Pipelines für Continuous Integration und Continuous Deployment (CI/CD) integrieren. In diesem Artikel erfahren Sie, wie Sie eine Bicep-Datei mithilfe einer Azure CLI-Aufgabe bereitstellen können.

## <a name="prepare-your-project"></a>Vorbereiten Ihres Projekts

In diesem Artikel wird vorausgesetzt, dass Ihre Bicep-Datei und Ihre Azure DevOps-Organisation für die Erstellung der Pipeline vorbereitet sind. Die folgenden Schritte zeigen, wie Sie sicherstellen können, dass Sie bereit sind:

* Sie haben eine Azure DevOps-Organisation. Sollten Sie über keine Organisation verfügen, [können Sie kostenlos eine erstellen](/azure/devops/pipelines/get-started/pipelines-sign-up). Wenn Ihr Team bereits über eine Azure DevOps-Organisation verfügt, stellen Sie sicher, dass Sie Administrator des Azure DevOps-Projekts sind, das Sie verwenden möchten.

* Sie haben eine [Dienstverbindung](/azure/devops/pipelines/library/connect-to-azure) zu Ihrem Azure-Abonnement konfiguriert. Die Aufgaben in der Pipeline werden unter der Identität des Dienstprinzipals ausgeführt. Die Schritte zum Erstellen der Verbindung finden Sie unter [Erstellen eines DevOps-Projekts](../templates/deployment-tutorial-pipeline.md#create-a-devops-project).

* Sie haben eine [Bicep-Datei](./quickstart-create-bicep-use-visual-studio-code.md), mit der die Infrastruktur für Ihr Projekt definiert wird.

## <a name="create-pipeline"></a>Erstellen der Pipeline

1. Wenn Sie noch keine Pipeline hinzugefügt haben, müssen Sie eine neue Pipeline erstellen. Wählen Sie aus Ihrer Azure DevOps-Organisation **Pipelines** und **Neue Pipeline** aus.

   ![Hinzufügen einer neuen Pipeline](./media/add-template-to-azure-pipelines/new-pipeline.png)

1. Geben Sie an, wo Ihr Code gespeichert ist. Die folgende Abbildung zeigt die Auswahl von **Azure Repos-Git**.

   ![Auswählen der Codequelle](./media/add-template-to-azure-pipelines/select-source.png)

1. Wählen Sie aus dieser Quelle das Repository aus, das den Code für Ihr Projekt enthält.

   ![Repository auswählen](./media/add-template-to-azure-pipelines/select-repo.png)

1. Wählen Sie den zu erstellenden Pipelinetyp aus. Sie können **Starterpipeline** auswählen.

   ![Pipeline auswählen](./media/add-template-to-azure-pipelines/select-pipeline.png)

Sie können jetzt entweder die Azure PowerShell-Aufgabe oder die Aufgaben zum Kopieren und Bereitstellen der Datei hinzufügen.

## <a name="azure-cli-task"></a>CLI-Task von Azure

Mit der folgenden YAML-Datei wird eine Ressourcengruppe erstellt. Dabei wird mithilfe einer [Azure CLI-Aufgabe](/azure/devops/pipelines/tasks/deploy/azure-cli) eine Bicep-Datei bereitgestellt:

```yml
trigger:
- master

name: Deploy Bicep files

variables:
  vmImageName: 'ubuntu-latest'

  azureServiceConnection: '<your-connection-name>'
  resourceGroupName: '<your-resource-group-name>'
  location: '<your-resource-group-location>'
  templateFile: './azuredeploy.bicep'
pool:
  vmImage: $(vmImageName)

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: $(azureServiceConnection)
    scriptType: bash
    scriptLocation: inlineScript
    inlineScript: |
      az --version
      az group create --name $(resourceGroupName) --location $(location)
      az deployment group create --resource-group $(resourceGroupName) --template-file $(templateFile)
```

Für eine Azure CLI-Aufgabe werden die folgenden Eingaben benötigt:

* `azureSubscription`: Geben Sie den Namen der von Ihnen erstellten Dienstverbindung an.  Informationen hierzu finden Sie unter [Vorbereiten Ihres Projekts](#prepare-your-project).
* `scriptType`: Verwenden Sie **bash**.
* `scriptLocation`: Verwenden Sie **inlineScript** oder **scriptPath**. Wenn Sie **scriptPath** angeben, müssen Sie auch einen `scriptPath`-Parameter angeben.
* `inlineScript`: Geben Sie Ihre Skriptzeilen an.  Das im Beispiel bereitgestellte Skript erstellt eine Bicep-Datei namens *azuredeploy.bicep* und ist im Stammverzeichnis des Repositorys vorhanden.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verwenden des Was-wäre-wenn-Vorgangs in einer Pipeline finden Sie unter [Testen von ARM-Vorlagen mit Was-wäre-wenn-Vorgang in einer Pipeline](https://4bes.nl/2021/03/06/test-arm-templates-with-what-if/).
* Informationen zur Verwendung der Bicep-Datei mit GitHub Actions finden Sie unter [Bereitstellen von Bicep-Dateien mithilfe von GitHub Actions](./deploy-github-actions.md).