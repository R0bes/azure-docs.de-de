---
title: Bereitstellen von Bicep-Dateien mithilfe von GitHub Actions
description: Beschreibt, wie Bicep-Dateien mithilfe von GitHub Actions bereitgestellt werden.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.custom: github-actions-azure
ms.openlocfilehash: 808c196ba5ae58e37889ef4b23da64be0eea4d81
ms.sourcegitcommit: b59e0afdd98204d11b7f9b6a3e55f5a85d8afdec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114371441"
---
# <a name="deploy-bicep-files-by-using-github-actions"></a>Bereitstellen von Bicep-Dateien mithilfe von GitHub Actions

Bei [GitHub Actions](https://docs.github.com/en/actions) handelt es sich um eine Featuresammlung in GitHub, mit der sich Ihre Softwareentwicklungsworkflows am selben Ort automatisieren lassen, an dem Sie auch den Code speichern und gemeinsam an Pull Requests und Problemen arbeiten.

Verwenden Sie die [Aktion zum Bereitstellen einer Azure Resource Manager-Vorlage](https://github.com/marketplace/actions/deploy-azure-resource-manager-arm-template), um die Bereitstellung einer Bicep-Datei in Azure zu automatisieren.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Ein GitHub-Konto. Falls Sie noch nicht über ein Konto verfügen, können Sie sich [kostenlos](https://github.com/join) registrieren.

  - Ein GitHub-Repository, in dem Sie Ihre Bicep-Dateien und Ihre Workflowdateien speichern können. Informationen zum Erstellen eines neuen Repositorys finden Sie [in diesem Hilfeartikel](https://docs.github.com/github/creating-cloning-and-archiving-repositories/creating-a-new-repository).

## <a name="workflow-file-overview"></a>Übersicht über die Workflowdatei

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden.

Die Datei besteht aus zwei Abschnitten:

|`Section`  |Aufgaben  |
|---------|---------|
|**Authentifizierung** | 1. Definieren eines Dienstprinzipals. <br /> 2. Erstellen eines GitHub-Geheimnisses. |
|**Bereitstellen** | 1. Stellen Sie die Bicep-Datei bereit. |

## <a name="generate-deployment-credentials"></a>Generieren von Anmeldeinformationen für die Bereitstellung

Sie können mit dem Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) in der [Azure CLI](/cli/azure/) einen [Dienstprinzipal](../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) erstellen. Führen Sie diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com/) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** aus.

Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist:

```azurecli-interactive
    az group create -n {MyResourceGroup} -l {location}
```

Ersetzen Sie den Platzhalter `myApp` durch den Namen Ihrer Anwendung.

```azurecli-interactive
   az ad sp create-for-rbac --name {myApp} --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{MyResourceGroup} --sdk-auth
```

Ersetzen Sie im obigen Beispiel die Platzhalter durch Ihre Abonnement-ID und den Ressourcengruppennamen. Die Ausgabe ist ein JSON-Objekt mit den Anmeldeinformationen für die Rollenzuweisung, die ähnlich wie unten Zugriff auf Ihre App Service-App gewähren. Kopieren Sie dieses JSON-Objekt zur späteren Verwendung. Sie benötigen nur die Abschnitte mit den Werten `clientId`, `clientSecret`, `subscriptionId` und `tenantId`.

```output
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Der Bereich im vorherigen Beispiel ist auf die Ressourcengruppe beschränkt.

## <a name="configure-the-github-secrets"></a>Konfigurieren der GitHub-Geheimnisse

Sie müssen Geheimnisse für Ihre Azure-Anmeldeinformationen, Ressourcengruppe und Abonnements erstellen.

1. Suchen Sie auf [GitHub](https://github.com/) nach Ihrem Repository.

1. Wählen Sie **Settings > Secrets > New secret** (Einstellungen > Geheimnisse > Neues Geheimnis) aus.

1. Fügen Sie die gesamte JSON-Ausgabe aus dem Azure CLI-Befehl in das Wertfeld des Geheimnisses ein. Geben Sie dem Geheimnis den Namen `AZURE_CREDENTIALS`.

1. Erstellen Sie ein weiteres Geheimnis mit dem Namen `AZURE_RG`. Fügen Sie den Namen Ihrer Ressourcengruppe dem Wertfeld des Geheimnisses hinzu (Beispiel: `myResourceGroup`).

1. Erstellen Sie ein zusätzliches Geheimnis namens `AZURE_SUBSCRIPTION`. Fügen Sie Ihre Abonnement-ID dem Wertfeld des Geheimnisses hinzu (Beispiel: `90fd3f9d-4c61-432d-99ba-1273f236afa2`).

## <a name="add-a-bicep-file"></a>Hinzufügen einer Bicep-Datei

Fügen Sie Ihrem GitHub-Repository eine Bicep-Datei hinzu. Die folgende Bicep-Datei erstellt ein Speicherkonto:

```url
https://raw.githubusercontent.com/mumian/azure-docs-json-samples/master/get-started-with-templates/add-variable/azuredeploy.bicep
```

Die Bicep-Datei verwendet einen Parameter namens **storagePrefix** mit 3 bis 11 Zeichen.

Sie können die Datei an einer beliebigen Stelle im Repository ablegen. Im Workflowbeispiel im nächsten Abschnitt wird davon ausgegangen, dass die Bicep-Datei **azuredeploy.bicep** heißt und im Repositorystamm gespeichert ist.

## <a name="create-workflow"></a>Erstellen des Workflows

Die Workflowdatei muss am Repositorystamm im Ordner **.github/workflows** gespeichert werden. Die Erweiterung der Workflowdatei kann entweder **.yml** oder **.yaml** lauten.

1. Klicken Sie in Ihrem GitHub-Repository im oberen Menü auf **Actions** (Aktionen).
1. Klicken Sie auf **New workflow** (Neuer Workflow).
1. Klicken Sie auf **set up a workflow yourself** (Workflow selbst einrichten).
1. Benennen Sie die Workflowdatei um, wenn Sie einen anderen Namen als **main.yml** bevorzugen. Beispiel: **deployBicepFile.yml**.
1. Ersetzen Sie den Inhalt dieser YML-Datei durch den folgenden Code:

    ```yml
    on: [push]
    name: Azure ARM
    jobs:
      build-and-deploy:
        runs-on: ubuntu-latest
        steps:

          # Checkout code
        - uses: actions/checkout@main

          # Log into Azure
        - uses: azure/login@v1
          with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}

          # Deploy Bicep file
        - name: deploy
          uses: azure/arm-deploy@v1
          with:
            subscriptionId: ${{ secrets.AZURE_SUBSCRIPTION }}
            resourceGroupName: ${{ secrets.AZURE_RG }}
            template: ./azuredeploy.bicep
            parameters: storagePrefix=mystore
            failOnStdErr: false
    ```

    Ersetzen Sie **mystore** durch das Namenspräfix Ihres eigenen Speicherkontos.

    > [!NOTE]
    > Sie können in der ARM-Bereitstellungsaktion stattdessen eine Parameterdatei im JSON-Format angeben (Beispiel: `.azuredeploy.parameters.json`).

    Der erste Abschnitt der Workflowdatei enthält Folgendes:

    - **name:** Der Name des Workflows.
    - **on:** Der Name der GitHub-Ereignisse, die den Workflow auslösen. Der Workflow wird ausgelöst, wenn ein Pushereignis für den Hauptbranch auftritt, durch das mindestens eine der beiden angegebenen Dateien geändert wird. Bei den beiden Dateien handelt es sich um die Workflow- und die Bicep-Datei.

1. Klicken Sie auf **Start commit** (Commit starten).
1. Wählen Sie **Direkten Commit zum Hauptbranch ausführen** aus.
1. Klicken Sie auf **Commit new file** (Neue Datei committen) (oder **Commit changes** [Änderungen committen]).

Da der Workflow so konfiguriert ist, dass er entweder von der Workflow- oder der Bicep-Datei ausgelöst wird, die gerade aktualisiert wird, startet der Workflow direkt nach dem Committen der Änderungen.

## <a name="check-workflow-status"></a>Überprüfen des Workflowstatus

1. Klicken Sie auf die Registerkarte **Actions** (Aktionen). Ihnen wird der Workflow **Create deployStorageAccount.yml** (deployStorageAccount.yml erstellen) angezeigt. Die Ausführung des Workflows dauert 1–2 Minuten.
1. Klicken Sie auf den Workflow, um ihn zu öffnen.
1. Wählen Sie im Menü die Option **Run ARM deploy** (ARM-Bereitstellung ausführen) aus, um die Bereitstellung zu überprüfen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ihre Ressourcengruppe und das Repository nicht mehr benötigt werden, bereinigen Sie die bereitgestellten Ressourcen, indem Sie die Ressourcengruppe und Ihr GitHub-Repository löschen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Learn-Modul: Automatisieren der Bereitstellung von ARM-Vorlagen mithilfe von GitHub Actions](/learn/modules/deploy-templates-command-line-github-actions/)
