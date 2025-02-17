---
title: Authentifizieren mit einer verwalteten Identität
description: Ermöglichen Sie Zugriff auf Images in Ihrer privaten Containerregistrierung, indem Sie eine benutzerseitig oder systemseitig zugewiesene verwaltete Azure-Identität verwenden.
ms.topic: article
ms.date: 06/30/2021
ms.openlocfilehash: 84f7d76eb763c8116390501dfbe2a6568849f10f
ms.sourcegitcommit: d90cb315dd90af66a247ac91d982ec50dde1c45f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2021
ms.locfileid: "113286536"
---
# <a name="use-an-azure-managed-identity-to-authenticate-to-an-azure-container-registry"></a>Verwenden einer verwalteten Azure-Identität für die Azure Container Registry-Authentifizierung 

Verwenden Sie eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md), um sich von einer anderen Azure-Ressource aus bei einer Azure Container Registry-Instanz zu authentifizieren, ohne Registrierungsanmeldeinformationen angeben oder verwalten zu müssen. Richten Sie beispielsweise eine benutzerseitig oder systemseitig zugewiesene verwaltete Identität für eine Linux-VM ein, um ebenso einfach wie bei Verwendung einer öffentlichen Registrierung auf Containerimages aus Ihrer Containerregistrierung zuzugreifen. Oder richten Sie einen Azure Kubernetes Service-Cluster ein, um seine [verwaltete Identität](../aks/cluster-container-registry-integration.md) zu verwenden, um Container-Images aus der Azure Container Registry für Pod-Bereitstellungen zu ziehen.

In diesem Artikel erfahren Sie mehr über verwaltete Identitäten und lernen Folgendes:

> [!div class="checklist"]
> * Aktivieren einer benutzerseitig oder systemseitig zugewiesenen Identität für eine Azure-VM
> * Gewähren des Zugriffs auf eine Azure Container Registry-Instanz für die Identität
> * Verwenden der verwalteten Identität, um auf die Registrierung zuzugreifen und ein Containerimage abzurufen 

Um die in diesem Artikel verwendeten Azure-Ressourcen zu erstellen, müssen Sie mindestens Version 2.0.55 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli].

Um eine Containerregistrierung einzurichten und per Push ein Containerimage zu übertragen, muss außerdem Docker lokal installiert sein. Für Docker sind Pakete erhältlich, mit denen Docker auf einem [macOS][docker-mac]-, [Windows][docker-windows]- oder [Linux][docker-linux]-System problemlos konfiguriert werden kann.

## <a name="why-use-a-managed-identity"></a>Gründe für die Verwendung einer verwalteten Identität

Wenn Sie mit der Funktion für verwaltete Identitäten für Azure-Ressourcen nicht vertraut sind, finden Sie hier eine [Übersicht](../active-directory/managed-identities-azure-resources/overview.md).

Nachdem Sie eine Azure-Ressource mit einer verwalteten Identität eingerichtet haben, können Sie der verwalteten Identität genau wie bei jedem anderen Sicherheitsprinzipal Zugriff auf eine andere Ressource erteilen. Weisen Sie beispielsweise einer verwalteten Identität eine Rolle mit Pullberechtigungen, Push- und Pullberechtigungen oder anderen Berechtigungen für eine private Registrierung in Azure zu. (Eine vollständige Liste der Rollen finden Sie unter [Azure Container Registry – Rollen und Berechtigungen](container-registry-roles.md).) Sie können einer Identität Zugriff auf eine oder mehrere Ressourcen gewähren.

Anschließend können die Identität für die Authentifizierung bei [jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). Hierfür müssen keine Anmeldeinformationen in Ihrem Code enthalten sein. Wählen Sie je nach Szenario, wie die Authentifizierung mit der verwalteten Identität erfolgen soll. Um mithilfe der Identität von einer VM aus auf eine Azure Container Registry-Instanz zuzugreifen, authentifizieren Sie sich mit Azure Resource Manager. 

> [!NOTE]
> Aktuell kann keine verwaltete Identität in Azure Container Instances verwendet werden, um beim Erstellen einer Containergruppe ein Image aus Azure Container Registry zu pullen. Die Identität ist nur in einem ausgeführten Container verfügbar. Wenn Sie eine Containergruppe in Azure Container Instances mithilfe von Images aus Azure Container Registry bereitstellen möchten, empfiehlt sich die Verwendung einer anderen Authentifizierungsmethode (beispielsweise ein [Dienstprinzipal](container-registry-auth-service-principal.md)).

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Wenn Sie nicht bereits über eine Azure Container Registry-Instanz verfügen, erstellen Sie eine Registrierung, und übertragen Sie per Push ein Beispielcontainerimage in die Registrierung. Die hierzu erforderlichen Schritte finden Sie unter [Schnellstart: Erstellen einer privaten Containerregistrierung mit der Azure CLI](container-registry-get-started-azure-cli.md).

In diesem Artikel wird vorausgesetzt, dass Sie das Containerimage `aci-helloworld:v1` in Ihrer Registrierung gespeichert haben. Die Beispiele verwenden eine Registrierung namens *myContainerRegistry*. Ersetzen Sie in den weiteren Schritten diesen Namen durch Ihre eigenen Registrierungs- und Imagenamen.

## <a name="create-a-docker-enabled-vm"></a>Erstellen einer Docker-fähigen VM

Erstellen Sie eine Docker-fähige Ubuntu-VM. Sie müssen außerdem die [Azure CLI](/cli/azure/install-azure-cli) auf dem virtuellen Computer installieren. Wenn Sie bereits über eine Azure-VM verfügen, überspringen Sie diesen Schritt zum Erstellen des virtuellen Computers.

Stellen Sie mithilfe von [az vm create][az-vm-create] eine Azure-Standard-VM mit Ubuntu bereit. Im folgenden Beispiel wird eine VM mit dem Namen *myDockerVM* in der vorhandenen Ressourcengruppe *myResourceGroup* erstellt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Notieren Sie nach Abschluss der Befehlsausführung die von der Azure CLI angezeigte `publicIpAddress`. Verwenden Sie diese Adresse, um SSH-Verbindungen mit der VM herzustellen.

### <a name="install-docker-on-the-vm"></a>Installieren von Docker auf der VM

Wenn die VM ausgeführt wird, stellen Sie eine SSH-Verbindung mit der VM her. Ersetzen Sie *publicIpAddress* durch die öffentliche IP-Adresse Ihrer VM.

```bash
ssh azureuser@publicIpAddress
```

Führen Sie den folgenden Befehl aus, um Docker auf der VM zu installieren:

```bash
sudo apt update
sudo apt install docker.io -y
```

Führen Sie nach der Installation den folgenden Befehl aus, um sicherzustellen, dass Docker ordnungsgemäß auf der VM ausgeführt wird:

```bash
sudo docker run -it mcr.microsoft.com/hello-world
```

Ausgabe:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Führen Sie die in [Installieren der Azure CLI mit apt](/cli/azure/install-azure-cli-apt) beschriebenen Schritte aus, um die Azure CLI auf Ihrer Ubuntu-VM zu installieren. Für diesen Artikel müssen Sie sicherstellen, dass Sie Version 2.0.55 oder höher installieren.

Beenden Sie die SSH-Sitzung.

## <a name="example-1-access-with-a-user-assigned-identity"></a>Beispiel 1: Zugriff mit einer benutzerseitig zugewiesenen Identität

### <a name="create-an-identity"></a>Erstellen einer Identität

Erstellen Sie mit dem Befehl [az identity create](/cli/azure/identity#az_identity_create) eine Identität in Ihrem Abonnement. Sie können entweder dieselbe Ressourcengruppe verwenden, die Sie zuvor zum Erstellen der Containerregistrierung oder VM verwendet haben, oder Sie verwenden eine andere Ressourcengruppe.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRId
```

Um die Identität in den folgenden Schritten zu konfigurieren, verwenden Sie den Befehl [az identity show][az_identity_show], um die Ressourcen-ID und die Dienstprinzipal-ID der Identität in Variablen zu speichern.

```azurecli
# Get resource ID of the user-assigned identity
userID=$(az identity show --resource-group myResourceGroup --name myACRId --query id --output tsv)

# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACRId --query principalId --output tsv)
```

Da Sie die ID der Identität in einem späteren Schritt benötigen, wenn Sie sich von Ihrer VM aus bei der CLI anmelden, zeigen Sie den Wert an:

```bash
echo $userID
```

Die ID hat das folgende Format:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRId
```

### <a name="configure-the-vm-with-the-identity"></a>Konfigurieren der VM mit der Identität

Über den folgenden Befehl [az vm identity assign][az-vm-identity-assign] wird Ihre Docker-VM mit der benutzerseitig zugewiesenen Identität konfiguriert:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM --identities $userID
```

### <a name="grant-identity-access-to-the-container-registry"></a>Gewähren des Zugriffs auf die Containerregistrierung für die Identität

Jetzt konfigurieren Sie die Identität für den Zugriff auf Ihre Containerregistrierung. Verwenden Sie zunächst den Befehl [az acr show][az-acr-show], um die Ressourcen-ID der Registrierung abzurufen:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Verwenden Sie den Befehl [az role assignment create][az-role-assignment-create], um der Registrierung die Rolle „AcrPull“ zuzuweisen. Diese Rolle gewährt der Registrierung [Pullberechtigungen](container-registry-roles.md). Um sowohl Pull- als auch Pushberechtigungen zu gewähren, weisen Sie die Rolle „ACRPush“ zu.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Verwenden der Identität zum Zugriff auf die Registrierung

Stellen Sie eine SSH-Verbindung mit der Docker-VM her, die mit der Identität konfiguriert ist. Führen Sie über die Azure CLI-Installation auf der VM die folgenden Azure CLI-Befehle aus.

Authentifizieren Sie sich zunächst mithilfe von [az login][az-login] bei der Azure-Befehlszeilenschnittstelle, und verwenden Sie hierbei die Identität, die Sie für die VM konfiguriert haben. Ersetzen Sie `<userID>` durch die ID der Identität, die Sie in einem der vorherigen Schritte abgerufen haben. 

```azurecli
az login --identity --username <userID>
```

Authentifizieren Sie sich anschließend mit dem Befehl [az acr login][az-acr-login] bei der Registrierung. Bei Verwendung dieses Befehls verwendet die CLI das bei Ausführung von `az login` erstellte Active Directory-Token für die nahtlose Authentifizierung Ihrer Sitzung mit der Containerregistrierung. (Je nach Einrichtung Ihrer VM müssen Sie diesen Befehl und docker-Befehle mit `sudo` ausführen.)

```azurecli
az acr login --name myContainerRegistry
```

Es sollte die Meldung `Login succeeded` angezeigt werden. Anschließend können Sie `docker`-Befehle ohne Angabe von Anmeldeinformationen ausführen. Führen Sie beispielsweise [docker pull][docker-pull] aus, um das Image `aci-helloworld:v1` zu pullen, und geben Sie dabei den Namen des Anmeldeservers für Ihre Registrierung an. Der Name des Anmeldeservers setzt sich aus dem Namen Ihrer Containerregistrierung (nur Kleinbuchstaben) gefolgt von `.azurecr.io` zusammen. Beispiel: `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="example-2-access-with-a-system-assigned-identity"></a>Beispiel 2: Zugriff mit einer systemseitig zugewiesenen Identität

### <a name="configure-the-vm-with-a-system-managed-identity"></a>Konfigurieren der VM mit einer systemseitig verwalteten Identität

Über den folgenden Befehl vom Typ [az vm identity assign][az-vm-identity-assign] wird Ihre Docker-VM mit einer systemseitig zugewiesenen Identität konfiguriert:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM 
```

Legen Sie mithilfe des Befehls [az vm show][az-vm-show] eine Variable auf den Wert von `principalId` (die Dienstprinzipal-ID) der VM-Identität fest, um sie in späteren Schritten zu verwenden.

```azurecli-interactive
spID=$(az vm show --resource-group myResourceGroup --name myDockerVM --query identity.principalId --out tsv)
```

### <a name="grant-identity-access-to-the-container-registry"></a>Gewähren des Zugriffs auf die Containerregistrierung für die Identität

Jetzt konfigurieren Sie die Identität für den Zugriff auf Ihre Containerregistrierung. Verwenden Sie zunächst den Befehl [az acr show][az-acr-show], um die Ressourcen-ID der Registrierung abzurufen:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Verwenden Sie den Befehl [az role assignment create][az-role-assignment-create], um der Identität die Rolle „AcrPull“ zuzuweisen. Diese Rolle gewährt der Registrierung [Pullberechtigungen](container-registry-roles.md). Um sowohl Pull- als auch Pushberechtigungen zu gewähren, weisen Sie die Rolle „ACRPush“ zu.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Verwenden der Identität zum Zugriff auf die Registrierung

Stellen Sie eine SSH-Verbindung mit der Docker-VM her, die mit der Identität konfiguriert ist. Führen Sie über die Azure CLI-Installation auf der VM die folgenden Azure CLI-Befehle aus.

Authentifizieren Sie sich zunächst mithilfe von [az login][az-login] bei der Azure-Befehlszeilenschnittstelle, und verwenden Sie dabei die systemseitig zugewiesene Identität für die VM.

```azurecli
az login --identity
```

Authentifizieren Sie sich anschließend mit dem Befehl [az acr login][az-acr-login] bei der Registrierung. Bei Verwendung dieses Befehls verwendet die CLI das bei Ausführung von `az login` erstellte Active Directory-Token für die nahtlose Authentifizierung Ihrer Sitzung mit der Containerregistrierung. (Je nach Einrichtung Ihrer VM müssen Sie diesen Befehl und docker-Befehle mit `sudo` ausführen.)

```azurecli
az acr login --name myContainerRegistry
```

Es sollte die Meldung `Login succeeded` angezeigt werden. Anschließend können Sie `docker`-Befehle ohne Angabe von Anmeldeinformationen ausführen. Führen Sie beispielsweise [docker pull][docker-pull] aus, um das Image `aci-helloworld:v1` zu pullen, und geben Sie dabei den Namen des Anmeldeservers für Ihre Registrierung an. Der Name des Anmeldeservers setzt sich aus dem Namen Ihrer Containerregistrierung (nur Kleinbuchstaben) gefolgt von `.azurecr.io` zusammen. Beispiel: `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehr über die Verwendung verwalteter Identitäten mit Azure Container Registry erfahren und Folgendes gelernt:

> [!div class="checklist"]
> * Aktivieren einer benutzerseitig oder systemseitig zugewiesenen Identität in einer Azure-VM
> * Gewähren des Zugriffs auf eine Azure Container Registry-Instanz für die Identität
> * Verwenden der verwalteten Identität, um auf die Registrierung zuzugreifen und ein Containerimage abzurufen

* Erfahren Sie mehr über [verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/index.yml).
* Informieren Sie sich über die Verwendung einer [systemseitig](https://github.com/Azure/app-service-linux-docs/blob/master/HowTo/use_system-assigned_managed_identities.md) oder [benutzerseitig](https://github.com/Azure/app-service-linux-docs/blob/master/HowTo/use_user-assigned_managed_identities.md) zugewiesenen verwalteten Identität mit App Service und Azure Container Registry.


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az_login
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-vm-create]: /cli/azure/vm#az_vm_create
[az-vm-show]: /cli/azure/vm#az_vm_show
[az-vm-identity-assign]: /cli/azure/vm/identity#az_vm_identity_assign
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-identity-show]: /cli/azure/identity#az_identity_show
[azure-cli]: /cli/azure/install-azure-cli
