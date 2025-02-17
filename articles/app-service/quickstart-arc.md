---
title: 'Schnellstart: Erstellen einer Web-App in Azure Arc'
description: Erste Schritte mit App Service in Azure Arc zum Bereitstellen Ihrer ersten Web-App.
ms.topic: quickstart
ms.date: 06/02/2021
ms.openlocfilehash: 4002ef3d66eaae05881da0dd8d95cc82ffeb916d
ms.sourcegitcommit: 351279883100285f935d3ca9562e9a99d3744cbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2021
ms.locfileid: "112377006"
---
# <a name="create-an-app-service-app-on-azure-arc-preview"></a>Erstellen einer App Service-App in Azure Arc (Vorschau)

In dieser Schnellstartanleitung erstellen Sie eine [App Service-App in einem Kubernetes-Cluster mit Azure Arc-Unterstützung](overview-arc-integration.md) (Vorschau). Dieses Szenario unterstützt nur Linux-Apps, und Sie können einen integrierten Sprachstapel oder einen benutzerdefinierten Container verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- [Richten Sie Ihr Kubernetes mit Azure Arc-Unterstützung für die Ausführung von App Service ein](manage-create-arc-environment.md).

[!INCLUDE [app-service-arc-cli-install-extensions](../../includes/app-service-arc-cli-install-extensions.md)]

## <a name="1-create-a-resource-group"></a>1. Erstellen einer Ressourcengruppe

Führen Sie den folgenden Befehl aus.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="2-get-the-custom-location"></a>2. Abrufen des benutzerdefinierten Speicherorts

[!INCLUDE [app-service-arc-get-custom-location](../../includes/app-service-arc-get-custom-location.md)]


## <a name="3-create-an-app-service-plan"></a>3. Erstellen eines App Service-Plans

Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `$customLocationId` aus dem vorherigen Schritt.

```azurecli-interactive
az appservice plan create -g myResourceGroup -n myPlan \
    --custom-location $customLocationId \
    --per-site-scaling --is-linux --sku K1
``` 

## <a name="4-create-an-app"></a>4. Erstellen einer App

Im folgenden Beispiel wird eine Node.js-App erstellt. Ersetzen Sie `<app-name>` durch einen Namen, der in Ihrem Cluster eindeutig ist (gültige Zeichen: `a-z`, `0-9` und `-`). Führen Sie [`az webapp list-runtimes --linux`](/cli/azure/webapp) aus, um alle unterstützten Laufzeiten anzuzeigen.

```azurecli-interactive
 az webapp create \
    --plan myPlan \
    --resource-group myResourceGroup \
    --name <app-name> \
    --custom-location $customLocationId \
    --runtime 'NODE|12-lts'
```

## <a name="5-deploy-some-code"></a>5. Bereitstellen von Code

> [!NOTE]
> `az webapp up` wird während der öffentlichen Vorschauphase nicht unterstützt.

Rufen Sie eine Node.js-Beispiel-App mit Git ab, und stellen Sie sie mithilfe von [ZIP-Bereitstellung](deploy-zip.md) bereit. Ersetzen Sie `<app-name>` durch den Namen Ihrer Web-App.

```azurecli-interactive
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
cd nodejs-docs-hello-world
zip -r package.zip .
az webapp deployment source config-zip --resource-group myResourceGroup --name <app-name> --src package.zip
```

## <a name="6-get-diagnostic-logs-using-log-analytics"></a>6. Abrufen von Diagnoseprotokollen mit Log Analytics

> [!NOTE]
> Um Log Analytics zu verwenden, sollten Sie es zuvor bei der [Installation der App Service-Erweiterung](manage-create-arc-environment.md#install-the-app-service-extension) aktiviert haben. Wenn Sie die Erweiterung ohne Log Analytics installiert haben, überspringen Sie diesen Schritt.

Navigieren Sie zu dem [Log Analytics-Arbeitsbereich, der mit Ihrer App Service-Erweiterung konfiguriert ist](manage-create-arc-environment.md#install-the-app-service-extension), und klicken Sie dann im linken Navigationsbereich auf „Protokolle“. Führen Sie die folgende Beispielabfrage aus, um Protokolle der letzten 72 Stunden anzuzeigen. Ersetzen Sie `<app-name>` durch den Namen Ihrer Web-App. Wenn beim Ausführen einer Abfrage ein Fehler auftritt, versuchen Sie es nach 10 bis 15 Minuten erneut (es kann zu eine Verzögerung kommen, bis Log Analytics beginnt Protokolle von Ihrer Anwendung zu empfangen). 

```kusto
let StartTime = ago(72h);
let EndTime = now();
AppServiceConsoleLogs_CL
| where TimeGenerated between (StartTime .. EndTime)
| where AppName_s =~ "<app-name>"
```

Die Anwendungsprotokolle für alle Apps, die in Ihrem Kubernetes-Cluster gehostet werden, werden im Log Analytics-Arbeitsbereich in der benutzerdefinierten Protokolltabelle namens `AppServiceConsoleLogs_CL` protokolliert. 

**Log_s** enthält Anwendungsprotokolle für einen bestimmten App Service, und **AppName_s** den Namen der App Service-App. Zusätzlich zu Protokollen, die Sie über Ihren Anwendungscode schreiben, enthält die Spalte „Log_s“ auch Protokolle zum Starten und Herunterfahren von Containern sowie zu Funktions-Apps.

Weitere Informationen zu Protokollabfragen finden Sie unter [Erste Schritte mit Kusto](../azure-monitor/logs/get-started-queries.md).

## <a name="optional-deploy-a-custom-container"></a>(Optional) Bereitstellen eines benutzerdefinierten Containers

Um eine benutzerdefinierte Container-App zu erstellen, führen Sie [az webapp create](/cli/azure/webapp#az_webapp_create) mit `--deployment-container-image-name` aus. Fügen Sie für ein privates Repository `--docker-registry-server-user` und `--docker-registry-server-password` hinzu.

Versuchen Sie zum Beispiel:

```azurecli-interactive
az webapp create \
    --plan myPlan \
    --resource-group myResourceGroup \
    --name <app-name> \
    --custom-location $customLocationId \
    --deployment-container-image-name mcr.microsoft.com/appsvc/node:12-lts
```

<!-- `TODO: currently gets an error but the app is successfully created: "Error occurred in request., RetryError: HTTPSConnectionPool(host='management.azure.com', port=443): Max retries exceeded with url: /subscriptions/62f3ac8c-ca8d-407b-abd8-04c5496b2221/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/cephalin-arctest4/config/appsettings?api-version=2020-12-01 (Caused by ResponseError('too many 500 error responses',))"` -->

Informationen zum Aktualisieren des Images nach dem Erstellen der App finden Sie unter [Ändern des Docker-Images eines benutzerdefinierten Containers](configure-custom-container.md?pivots=container-linux#change-the-docker-image-of-a-custom-container).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren einer ASP.NET Core-App](configure-language-dotnetcore.md?pivots=platform-linux)
- [Konfigurieren einer Node.js-App](configure-language-nodejs.md?pivots=platform-linux)
- [Konfigurieren einer PHP-App](configure-language-php.md?pivots=platform-linux)
- [Konfigurieren einer Linux-Python-App](configure-language-python.md)
- [Konfigurieren einer Java-App](configure-language-java.md?pivots=platform-linux)
- [Konfigurieren einer Linux-Ruby-App](configure-language-ruby.md)
- [Konfigurieren eines benutzerdefinierten Containers](configure-custom-container.md?pivots=container-linux)
