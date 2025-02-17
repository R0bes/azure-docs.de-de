---
title: 'Visual Studio Code: Erstellen von Bicep-Dateien'
description: Hier erfahren Sie, wie Sie Visual Studio Code und die Bicep-Erweiterung für Bicep-Dateien zum Bereitstellen von Azure-Ressourcen verwenden.
ms.date: 06/25/2021
ms.topic: quickstart
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 8167516a41943ea17fdeb47bce84767178593e85
ms.sourcegitcommit: cd8e78a9e64736e1a03fb1861d19b51c540444ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2021
ms.locfileid: "112970101"
---
# <a name="quickstart-create-bicep-files-with-visual-studio-code"></a>Schnellstart: Erstellen von Bicep-Dateien mit Visual Studio Code

Dieser Schnellstart führt Sie durch die Schritte zum Erstellen einer [Bicep-Datei](overview.md) mit Visual Studio Code. Sie erstellen ein Speicherkonto und ein virtuelles Netzwerk. Außerdem erfahren Sie, wie die Bicep-Erweiterung die Entwicklung vereinfacht, indem Sie die Typsicherheit, Syntaxvalidierung und das Autovervollständigen bereitstellt.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Informationen zum Einrichten Ihrer Umgebung für die Bicep-Entwicklung finden Sie unter [Installieren von Bicep-Tools](install.md). Nachdem Sie diese Schritte abgeschlossen haben, verfügen Sie über [Visual Studio Code](https://code.visualstudio.com/) und die [Bicep-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep). Außerdem müssen Sie die neueste Version der [Azure CLI](/cli/azure/) oder das neueste [Azure PowerShell-Modul](/powershell/azure/new-azureps-module-az) besitzen.

## <a name="add-resource-snippet"></a>Hinzufügen eines Ressourcenausschnitts

Starten Sie Visual Studio Code und erstellen Sie eine neue Datei mit dem Namen *main.bicep*.

Der VS Code mit der Bicep-Erweiterung vereinfacht die Entwicklung durch die Bereitstellung vordefinierter Codeausschnitte. In dieser Schnellstartanleitung fügen Sie einen Codeausschnitt hinzu, mit dem ein virtuelles Netzwerk erstellt wird.

Geben Sie **vnet** in *main.bicep* ein. Wählen Sie **res-vnet** aus der Liste aus, und bestätigen Sie dann mit der Tabulator oder Eingabetaste.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/add-snippet.png" alt-text="Hinzufügen eines Ausschnitts für ein virtuelles Netzwerk":::

Ihre Bicep-Datei enthält nun den folgenden Code:

```bicep
resource virtualNetwork 'Microsoft.Network/virtualNetworks@2019-11-01' = {
  name: 'name'
  location: resourceGroup().location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'Subnet-1'
        properties: {
          addressPrefix: '10.0.0.0/24'
        }
      }
      {
        name: 'Subnet-2'
        properties: {
          addressPrefix: '10.0.1.0/24'
        }
      }
    ]
  }
}
```

Dieser Codeausschnitt enthält alle Werte, die Sie zum Definieren des virtuellen Netzwerks benötigen. Sie können diesen Code jedoch an ihre Anforderungen anpassen. Beispielsweise ist `name` kein hervorragender Name für das virtuelle Netzwerk. Ändern Sie die `name` Eigenschaft in `examplevnet`.

```bicep
name: 'examplevnet'
```

Sie können diese Bicep-Datei bereitstellen, aber wir fügen vor der Bereitstellung einen Parameter und ein Speicherkonto hinzu.

## <a name="add-parameter"></a>Hinzufügen eines Parameters

Nun fügen wir einen Parameter hinzu, der für den Speicherkontonamen verwendet werden soll. Fügen Sie über dem virtuellen Netzwerk einige Zeilen hinzu, und geben Sie Folgendes ein:

```bicep
param storageName
```

Wenn Sie nach **storageName** einen Leerzeichen hinzufügen, beachten Sie, dass IntelliSense die Datentypen anbietet, die für den Parameter verfügbar sind.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/add-param.png" alt-text="Hinzufügen des Zeichenfolgentyps zum Parameter":::

Wählen Sie **Zeichenfolge** aus.

Sie haben den folgenden Parameter:

```bicep
param storageName string
```

Dieser Parameter funktioniert einwandfrei, aber für die Speicherkonten gelten Grenzwerte für die Länge des Namens. Der Name muss mindesten 3 und darf höchsten 24 Zeichen lang sein. Sie können diese Anforderungen angeben, indem Sie dem Parameter Decorator-Elemente hinzufügen.

Fügen Sie eine Zeile über dem Parameter hinzu, und geben Sie **@** ein. Die verfügbaren Decorators werden ihnen angezeigt. Beachten Sie, dass Decorator-Elemente sowohl für **minLength** als auch für **maxLength**  verfügbar sind.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/add-decorators.png" alt-text="Hinzufügen von Decorator-Elementen zum Parameter":::

Fügen Sie beide Decorator-Elemente hinzu und geben Sie die Zeichengrenzwerte an, wie unten gezeigt:

```bicep
@minLength(3)
@maxLength(24)
param storageName string
```

Sie können auch eine Beschreibung für den Parameter hinzufügen. Fügen Sie Informationen ein, die es Personen, die die Bicep-Datei bereitstellen, helfen, den bereitzustellenden Wert zu verstehen.

```bicep
@minLength(3)
@maxLength(24)
@description('Provide a name for the storage account. Use only lower case letters and numbers. The name must be unique across Azure.')
param storageName string
```

Ihr Parameter kann nun verwendet werden.

## <a name="add-resource"></a>Ressource hinzufügen

Anstatt einen Codeausschnitt zum Definieren des Speicherkontos zu verwenden, verwenden wir IntelliSense, um die Werte festzulegen. IntelliSense vereinfacht diesen Schritt erheblich, anstatt die Werte manuell eingeben zu müssen.

Sie definieren eine Ressource mit dem `resource` Schlüsselwort.  Geben Sie unterhalb Ihres virtuellen Netzwerks **resource exampleStorage** ein:

```bicep
resource exampleStorage
```

**exampleStorage** ist ein symbolischer Name für die Ressource, die Sie bereitstellen. Sie erleichtert damit das Verweisen auf die Ressource in anderen Teilen Ihrer Bicep-Datei.

Wenn Sie nach dem symbolischen Namen ein Leerzeichen hinzufügen, wird eine Liste der Ressourcentypen angezeigt. Fahren Sie mit der Eingabe des Worts **Speicher** fort, bis Sie ihn aus den verfügbaren Optionen auswählen können.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/select-resource-type.png" alt-text="Auswählen von Speicherkonten als Ressourcentyp":::

Nachdem Sie **Microsoft.Storage/storageAccounts** ausgewählt haben, werden Ihnen die verfügbaren API-Versionen angezeigt. Wählen Sie die neueste Version aus.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/select-api-version.png" alt-text="Auswählen der API-Version als Ressourcentyp":::

Fügen Sie nach dem einfachen Anführungszeichen für den Ressourcentyp **=** und ein Leerzeichen hinzu. Ihnen werden Optionen zum Hinzufügen von Eigenschaften zu der Ressource angezeigt. Wählen Sie die **erforderlichen Eigenschaften** aus.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/select-required-properties.png" alt-text="Hinzufügen der erforderlichen Eigenschaften":::

Diese Option fügt alle Eigenschaften für den Ressourcentyp hinzu, die für die Bereitstellung erforderlich sind. Nachdem Sie diese Option ausgewählt haben, verfügt Ihr Speicherkonto über die folgenden Eigenschaften:

```bicep
resource exampleStorage 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name:
  location:
  sku: {
    name:
  }
  kind:

}
```

Sie haben es fast geschafft! Geben Sie einfach Werte für diese Eigenschaften an.

Auch hier hilft Ihnen IntelliSense weiter. Geben Sie `name` für den Parameter an, der einen Namen für das Speicherkonto enthält. Legen Sie hierfür `location` auf `eastus` fest. Beim Hinzufügen von einem SKU-Name und einer SKU-Art stellt IntelliSense die gültigen Optionen zur Verfügung.

Wenn Sie fertig sind, haben Sie:

```bicep
@minLength(3)
@maxLength(24)
param storageName string

resource virtualNetwork 'Microsoft.Network/virtualNetworks@2019-11-01' = {
  name: 'examplevnet'
  location: resourceGroup().location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'Subnet-1'
        properties: {
          addressPrefix: '10.0.0.0/24'
        }
      }
      {
        name: 'Subnet-2'
        properties: {
          addressPrefix: '10.0.1.0/24'
        }
      }
    ]
  }
}

resource exampleStorage 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: storageName
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

Weitere Informationen zur Bicep-Syntax finden Sie unter [Bicep-Struktur](./file.md).

## <a name="visualize-resources"></a>Visualisieren von Ressourcen

Sie können eine Darstellung der Ressourcen in Ihrer Datei anzeigen.

Wählen Sie in der linken oberen Ecke die Schnellansichtsschaltfläche aus, um die Bicep-Schnellansicht zu öffnen.

:::image type="content" source="./media/quickstart-create-bicep-use-visual-studio-code/bicep-visualizer.png" alt-text="Bicep-Schnellansicht":::

Die Schnellansicht zeigt die in der Bicep-Datei definierten Ressourcen mit den Informationen zur Ressourcenabhängigkeit an. Da die beiden in dieser Schnellstartanleitung definierten Ressourcen keine Abhängigkeitsbeziehung haben, wird kein Connector zwischen den beiden Ressourcen angezeigt.

## <a name="deploy-the-bicep-file"></a>Bereitstellen der Bicep-Datei

Um die erstellte Datei bereitzustellen, öffnen Sie die PowerShell oder Azure CLI. Wenn Sie das integrierte Visual Studio Code-Terminal verwenden möchten, wählen Sie die Tastenkombination `ctrl` + ```` ` ```` aus. Ändern Sie das aktuelle Verzeichnis in das Verzeichnis, in dem sich die Bicep-Datei befindet.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli
az group create --name exampleRG --location eastus

az deployment group create --resource-group exampleRG --template-file main.bicep --parameters storageName={your-unique-name}
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
New-AzResourceGroup -Name exampleRG -Location eastus

New-AzResourceGroupDeployment -ResourceGroupName exampleRG -TemplateFile ./main.bicep -storageName "{your-unique-name}"
```

---

> [!NOTE]
> Ersetzen Sie **{Ihren-eindeutigen-Namen}** (einschließlich geschweifte Klammern) durch einen eindeutigen Speicherkontonamen.

Nach Abschluss der Bereitstellung sollte eine Meldung mit dem Hinweis angezeigt werden, dass die Bereitstellung erfolgreich war. Wenn Sie eine Fehlermeldung erhalten, die angibt, dass das Speicherkonto bereits verwendet wurde, wird der von Ihnen bereitgestellte Speichername bereits verwendet. Geben Sie einen Namen an, der wahrscheinlich eindeutiger ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe der Schnellstartanleitung mithilfe der Azure-Befehlszeilenschnittstelle oder des Azure PowerShell-Moduls.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli
az group delete --name exampleRG
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
Remove-AzResourceGroup -Name exampleRG
```

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bicep in Microsoft Learn](learn-bicep.md)
