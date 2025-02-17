---
title: Erstellen und Bereitstellen von Vorlagenspezifikationen
description: Beschreibt, wie Sie Vorlagenspezifikationen erstellen und diese mit anderen Benutzern in Ihrer Organisation teilen.
ms.topic: conceptual
ms.date: 05/04/2021
ms.author: tomfitz
ms.custom: devx-track-azurepowershell
author: tfitzmac
ms.openlocfilehash: bfbf876f8621beec4d8eddb80c3db285cd6d4c49
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110688291"
---
# <a name="azure-resource-manager-template-specs"></a>Azure Resource Manager-Vorlagenspezifikationen

Eine Vorlagenspezifikation ist ein Ressourcentyp zum Speichern einer Azure Resource Manager-Vorlage (ARM-Vorlage) in Azure für die spätere Bereitstellung. Mit diesem Ressourcentyp können Sie ARM-Vorlagen mit anderen Benutzer in Ihrer Organisation teilen. Wie bei jeder anderen Azure-Ressource können Sie die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC) verwenden, um die Vorlagenspezifikation zu teilen.

**Microsoft.Resources/templateSpecs** ist der Ressourcentyp für Vorlagenspezifikationen. Er besteht aus einer Hauptvorlage und einer beliebigen Anzahl von verknüpften Vorlagen. Azure speichert Vorlagenspezifikationen sicher in Ressourcengruppen. Vorlagenspezifikationen unterstützen [Versionsverwaltung](#versioning).

Zum Bereitstellen der Vorlagenspezifikation verwenden Sie Azure-Standardtools wie PowerShell, Azure CLI, Azure-Portal, REST und andere unterstützte SDKs und Clients. Sie verwenden dieselben Befehle wie für die Vorlage.

> [!NOTE]
> Um Vorlagenspezifikationen mit Azure PowerShell zu verwenden, müssen Sie [mindestens Version 5.0.0](/powershell/azure/install-az-ps) installieren. Zur Verwendung mit der Azure CLI verwenden Sie [mindestens Version 2.14.2](/cli/azure/install-azure-cli).

## <a name="why-use-template-specs"></a>Gründe zur Verwendung von Vorlagenspezifikationen

Vorlagenspezifikationen bieten die folgenden Vorteile:

* Sie verwenden ARM-Standardvorlagen für Ihre Vorlagenspezifikation.
* Sie verwalten den Zugriff über Azure RBAC, statt mit SAS-Token.
* Benutzer können die Vorlagenspezifikation ohne Schreibzugriff auf die Vorlage bereitstellen.
* Sie können die Vorlagenspezifikation in den vorhandenen Bereitstellungsprozess integrieren, z. B. ein PowerShell-Skript oder eine DevOps-Pipeline.

Vorlagenspezifikationen ermöglichen es Ihnen, kanonische Vorlagen zu erstellen und mit Teams in Ihrer Organisation zu teilen. Die Vorlagenspezifikationen sind sicher, weil sie für die Bereitstellung für den Azure Resource Manager verfügbar aber nicht für Benutzer ohne die richtigen Berechtigungen zugänglich sind. Benutzer benötigen nur Lesezugriff auf die Vorlagenspezifikation, um die zugehörige Vorlage bereitzustellen, sodass Sie die Vorlage teilen können, ohne anderen Benutzern das Ändern zu gestatten.

Wenn Sie Ihre Vorlagen derzeit in einem GitHub-Repository oder Speicherkonto aufbewahren, stellen sich mehrere Herausforderungen in Ihren Weg, wenn Sie versuchen, die Vorlagen freizugeben und zu verwenden. Zum Bereitstellen der Vorlage müssen Sie entweder die Vorlage öffentlich zugänglich machen oder den Zugriff mit SAS-Token verwalten. Um diese Einschränkung zu umgehen, können Benutzer lokale Kopien erstellen, die letztendlich von Ihrer ursprünglichen Vorlage abweichen. Vorlagenspezifikationen vereinfachen das Teilen von Vorlagen.

Die Vorlagen, die Sie in eine Vorlagenspezifikation aufnehmen, sollten von Administratoren in Ihrer Organisation überprüft werden, damit sie die Anforderungen und Richtlinien der Organisation einhalten.

## <a name="create-template-spec"></a>Erstellen von Vorlagenspezifikationen

Das folgende Beispiel zeigt eine einfache Vorlage zum Erstellen eines Speicherkonto in Azure.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ]
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[concat('store', uniquestring(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "kind": "StorageV2",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      }
    }
  ]
}
```

Wenn Sie die Vorlagenspezifikation erstellen, werden die PowerShell- oder CLI-Befehle an die Hauptvorlagendatei übergeben. Wenn die Hauptvorlage auf verknüpfte Vorlagen verweist, finden die Befehle diese und verpacken Sie, um die Vorlagenspezifikation zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](#create-a-template-spec-with-linked-templates).

Erstellen einer Vorlagenspezifikation mittels:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzTemplateSpec -Name storageSpec -Version 1.0a -ResourceGroupName templateSpecsRg -Location westus2 -TemplateFile ./mainTemplate.json
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0a" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json"
```

---

Sie können alle Vorlagenspezifikationen in Ihrem Abonnement anzeigen mithilfe von:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Get-AzTemplateSpec
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az ts list
```

---

Sie können Details einer Vorlagenspezifikation anzeigen, einschließlich ihrer Versionen, mittels:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Get-AzTemplateSpec -ResourceGroupName templateSpecsRG -Name storageSpec
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az ts show \
    --name storageSpec \
    --resource-group templateSpecRG \
    --version "1.0a"
```

---

## <a name="deploy-template-spec"></a>Bereitstellen von Vorlagenspezifikationen

Nachdem Sie die Vorlagenspezifikation erstellt haben, können Benutzer mit **Lese** zugriff auf die Vorlagenspezifikation diese bereitstellen. Informationen zum Erteilen von Zugriffsberechtigungen finden Sie unter [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für eine Gruppe mithilfe von Azure PowerShell](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

Vorlagenspezifikationen können über das Portal, mittels PowerShell, Azure CLI oder als verknüpfte Vorlage in einer größeren Vorlagenbereitstellung bereitgestellt werden. Benutzer in einer Organisation können eine Vorlagenspezifikation in einem beliebigen Bereich in Azure (Ressourcengruppe, Abonnement, Verwaltungsgruppe oder Mandant) bereitstellen.

Anstatt einen Pfad oder URI als Eingabe für eine Vorlage zu übergeben, stellen Sie eine Vorlagenspezifikation bereit, indem Sie deren Ressourcen-ID angeben. Die Ressourcen-ID hat folgendes Format:

**/subscriptions/{Abonnement-ID}/resourceGroups/{Ressourcengruppe}/providers/Microsoft.Resources/templateSpecs/{Vorlagenspezifikation-Name}/versions/{Vorlagenspezifikation-Version}**

Beachten Sie, dass die Ressourcen-ID ein Versionsname für die Vorlagenspezifikation enthält.

Beispielsweise stellen Sie eine Vorlagenspezifikation mit dem folgenden Befehl bereit.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$id = "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/templateSpecsRG/providers/Microsoft.Resources/templateSpecs/storageSpec/versions/1.0a"

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
id = "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/templateSpecsRG/providers/Microsoft.Resources/templateSpecs/storageSpec/versions/1.0a"

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

---

In der Praxis führen Sie in der Regel `Get-AzTemplateSpec` oder `az ts show` aus, um die ID der Vorlagenspezifikation abzurufen, die Sie bereitstellen möchten.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$id = (Get-AzTemplateSpec -Name storageSpec -ResourceGroupName templateSpecsRg -Version 1.0a).Versions.Id

New-AzResourceGroupDeployment `
  -ResourceGroupName demoRG `
  -TemplateSpecId $id
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0a" --query "id")

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

---

Sie können auch eine URL im folgenden Format öffnen, um eine Vorlagenspezifikation bereitzustellen:

```url
https://portal.azure.com/#create/Microsoft.Template/templateSpecVersionId/%2fsubscriptions%2f{subscription-id}%2fresourceGroups%2f{resource-group-name}%2fproviders%2fMicrosoft.Resources%2ftemplateSpecs%2f{template-spec-name}%2fversions%2f{template-spec-version}
```

## <a name="parameters"></a>Parameter

Das Übergeben von Parametern an die Vorlagenspezifikation entspricht genau dem Übergeben von Parametern an eine ARM-Vorlage. Fügen Sie die Parameterwerte entweder inline oder in einer Parameterdatei hinzu.

Verwenden Sie für die Inlineübergabe eines Parameters:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG `
  -StorageAccountType Standard_GRS
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az deployment group create \
  --resource-group demoRG \
  --template-spec $id \
  --parameters storageAccountType='Standard_GRS'
```

---

Verwenden Sie zum Erstellen einer lokalen Parameterdatei:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "StorageAccountType": {
      "value": "Standard_GRS"
    }
  }
}
```

Und übergeben Sie diese Parameterdatei mit:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG `
  -TemplateParameterFile ./mainTemplate.parameters.json
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az deployment group create \
  --resource-group demoRG \
  --template-spec $id \
  --parameters "./mainTemplate.parameters.json"
```

---

## <a name="versioning"></a>Versionsverwaltung

Wenn Sie eine Vorlagenspezifikation erstellen, geben Sie einen Versionsnamen dafür an. Wenn Sie den Vorlagencode durchlaufen, können Sie entweder eine vorhandene Version aktualisieren (für Hotfixes) oder eine neue Version veröffentlichen. Die Version ist eine Textzeichenfolge. Sie können sich für ein beliebiges Versionskontrollsystem entscheiden, einschließlich einer semantischen Versionsverwaltung. Benutzer der Vorlagenspezifikation können den Versionsnamen angeben, die sie bei der Bereitstellung verwenden möchten.

## <a name="use-tags"></a>Verwenden von Tags

[Tags](../management/tag-resources.md) helfen Ihnen dabei, Ihre Ressourcen logisch zu organisieren. Mithilfe von Azure PowerShell und der Azure CLI können Sie Tags zu Vorlagenspezifikationen hinzufügen:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzTemplateSpec `
  -Name storageSpec `
  -Version 1.0a `
  -ResourceGroupName templateSpecsRg `
  -Location westus2 `
  -TemplateFile ./mainTemplate.json `
  -Tag @{Dept="Finance";Environment="Production"}
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0a" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json" \
  --tags Dept=Finance Environment=Production
```

---

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Set-AzTemplateSpec `
  -Name storageSpec `
  -Version 1.0a `
  -ResourceGroupName templateSpecsRg `
  -Location westus2 `
  -TemplateFile ./mainTemplate.json `
  -Tag @{Dept="Finance";Environment="Production"}
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-cli)

```azurecli
az ts update \
  --name storageSpec \
  --version "1.0a" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json" \
  --tags Dept=Finance Environment=Production
```

---

Beim Erstellen oder Ändern einer Vorlagenspezifikation mit angegebenem Parameter „Version“, aber ohne den Parameter „Tag/Tags“, gilt Folgendes:

- Wenn die Vorlagenspezifikation vorliegt und Tags aufweist, aber die Version nicht vorhanden ist, erbt die neue Version dieselben Tags wie die vorhandene Vorlagenspezifikation.

Beim Erstellen oder Ändern einer Vorlagenspezifikation, bei der sowohl die Parameter „Tag/Tags“ als auch „Version“ angegeben sind, gilt Folgendes:

- Wenn sowohl die Vorlagenspezifikation als auch die Version nicht vorhanden sind, werden die Tags zur neuen Vorlagenspezifikation und zur neuen Version hinzugefügt.
- Wenn die Vorlagenspezifikation vorhanden ist, aber die Version nicht vorliegt, werden die Tags nur zur neuen Version hinzugefügt.
- Wenn sowohl die Vorlagenspezifikation als auch die Version vorhanden sind, werden die Tags nur auf die Version angewendet.

Beim Ändern einer Vorlage mit festgelegtem Parameter „Tag/Tags“, aber ohne den Parameter „Version“, werden die Tags nur zur Vorlagenspezifikation hinzugefügt.

## <a name="create-a-template-spec-with-linked-templates"></a>Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen

Wenn die Hauptvorlage für Ihre Vorlagenspezifikation auf verknüpfte Vorlagen verweist, können die PowerShell- und CLI-Befehle die verknüpften Vorlagen auf Ihrem lokalen Laufwerk automatisch suchen und verpacken. Sie müssen keine Speicherkonten oder Repositorys manuell konfigurieren, um die Vorlagenspezifikationen zu hosten – alles befindet sich eigenständig in der Vorlagenspezifikationsressource.

Das folgende Beispiel besteht aus einer Hauptvorlage mit zwei verknüpften Vorlagen. Das Beispiel ist nur ein Auszug aus der Vorlage. Beachten Sie, dass es eine Eigenschaft namens `relativePath` verwendet, um die Verknüpfung mit den anderen Vorlagen herzustellen. Sie müssen `apiVersion` von `2020-06-01` oder höher für die Bereitstellungsressource verwenden.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  ...
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      ...
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "relativePath": "artifacts/webapp.json"
        }
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      ...
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "relativePath": "artifacts/database.json"
        }
      }
    }
  ],
  "outputs": {}
}
```

Wenn der PowerShell- oder CLI-Befehl zum Erstellen der Vorlagenspezifikation für das vorherige Beispiel ausgeführt wird, findet der Befehl drei Dateien: die Hauptvorlage, die Web-App-Vorlage (`webapp.json`) und die Datenbankvorlage (`database.json`) und verpackt diese in die Vorlagenspezifikation.

Weitere Informationen finden Sie im [Tutorial: Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](template-specs-create-linked.md).

## <a name="deploy-template-spec-as-a-linked-template"></a>Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage

Nachdem Sie eine Vorlagenspezifikation erstellt haben, ist es einfach, sie aus einer ARM-Vorlage oder einer anderen Vorlagenspezifikation wiederzuverwenden. Sie stellen eine Verknüpfung mit einer Vorlagenspezifikation her, indem Sie deren Ressourcen-ID zu Ihrer Vorlage hinzufügen. Die verknüpfte Vorlagenspezifikation wird automatisch bereitgestellt, wenn Sie die Hauptvorlage bereitstellen. Mit diesem Verhalten können Sie modulare Vorlagenspezifikationen entwickeln und diese ganz nach Bedarf wiederverwenden.

Beispielsweise können Sie eine Vorlagenspezifikation erstellen, die Netzwerkressourcen bereitstellt, sowie eine weitere Vorlagenspezifikation, die Speicherressourcen bereitstellt. In ARM-Vorlagen stellen Sie jederzeit eine Verknüpfung mit diesen beiden Vorlagenspezifikationen her, wenn Sie Netzwerk- oder Speicherressourcen konfigurieren müssen.

Das folgende Beispiel ähnelt dem vorherigen Beispiel, aber Sie verwenden die `id`-Eigenschaft, um eine Verknüpfung mit einer Vorlagenspezifikation herzustellen, anstatt mit der `relativePath`-Eigenschaft eine Verknüpfung miteiner lokalen Vorlage herzustellen. Verwenden Sie `2020-06-01` als API-Version für die Bereitstellungsressource. In dem Beispiel befinden sich die Vorlagenspezifikationen in einer Ressourcengruppe namens **templateSpecsRG**.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  ...
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "networkingDeployment",
      ...
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'networkingSpec', '1.0a')]"
        }
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "storageDeployment",
      ...
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0a')]"
        }
      }
    }
  ],
  "outputs": {}
}
```

Weitere Informationen zum Verknüpfen von Vorlagenspezifikationen finden Sie unter [Tutorial: Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage](template-specs-deploy-linked-template.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen und Bereitstellen einer Vorlagenspezifikation finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Vorlagenspezifikationen](quickstart-create-template-specs.md).

* Weitere Informationen zum Verknüpfen von Vorlagen in Vorlagenspezifikationen finden Sie unter [Tutorial: Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](template-specs-create-linked.md).

* Weitere Informationen zum Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage finden Sie unter [Tutorial: Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage](template-specs-deploy-linked-template.md).
