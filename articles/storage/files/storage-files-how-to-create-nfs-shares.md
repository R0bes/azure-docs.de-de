---
title: Erstellen einer NFS-Freigabe (Vorschau) – Azure Files
description: In diesem Artikel erfahren Sie, wie Sie eine Azure-Dateifreigabe erstellen, die mithilfe des NFS-Protokolls (Network File System) eingebunden werden kann.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 07/01/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 2b1e7f17445fe2b24b19acf4669637ef4c47c196
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339151"
---
# <a name="how-to-create-an-nfs-share-preview"></a>Erstellen einer NFS-Freigabe (Vorschau)
Azure-Dateifreigaben sind vollständig verwaltete Dateifreigaben, die in der Cloud gespeichert werden. In diesem Artikel erfahren Sie, wie Sie eine Dateifreigabe erstellen, für die das NFS-Protokoll verwendet wird (Vorschau).

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Nein](../media/icons/no-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Nein](../media/icons/no-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Nein](../media/icons/no-icon.png) | ![Ja](../media/icons/yes-icon.png) |

## <a name="limitations"></a>Einschränkungen
[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]


### <a name="regional-availability"></a>Regionale Verfügbarkeit
[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen
- NFS-Freigaben akzeptieren nur numerische UID/GID. Deaktivieren Sie die ID-Zuordnung, um zu vermeiden, dass Ihre Clients eine alphanumerische UID/GID senden.
- Der Zugriff auf NFS-Freigaben ist nur über vertrauenswürdige Netzwerke möglich. Verbindungen zu Ihrer NFS-Freigabe müssen einer der folgenden Quellen entstammen:
    - Entweder [Erstellen eines privaten Endpunkts](storage-files-networking-endpoints.md#create-a-private-endpoint) (empfohlen) oder [Einschränken des Zugriffs auf den öffentlichen Endpunkt](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
    - [Konfigurieren eines Site-to-Site-VPN zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
    - Konfigurieren von [ExpressRoute](../../expressroute/expressroute-introduction.md)

- Falls Sie die Azure CLI verwenden möchten, [installieren Sie die neueste Version](/cli/azure/install-azure-cli).

## <a name="register-the-nfs-41-protocol"></a>Registrieren des NFS 4.1-Protokolls

Sie müssen sich erst für das Feature registrieren, um NFS-basierte Azure-Dateifreigaben erstellen zu können. In Speicherkonten, die vor der Registrierung erstellt wurden, können keine NFS-Freigaben erstellt werden.

Wenn Sie das Azure PowerShell-Modul oder die Azure CLI nutzen, registrieren Sie Ihr Feature mithilfe der folgenden Befehle:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die NFS 4.1-Funktionen für Azure Files zu registrieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
# Connect your PowerShell session to your Azure account, if you have not already done so.
Connect-AzAccount
# Set the actively selected subscription, if you have not already done so.
$subscriptionId = "<yourSubscriptionIDHere>"
$context = Get-AzSubscription -SubscriptionId $subscriptionId
Set-AzContext $context
# Register the NFS 4.1 feature with Azure Files to enable the preview.
Register-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares 
    
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
# Connect your Azure CLI to your Azure account, if you have not already done so.
az login
# Provide the subscription ID for the subscription where you would like to 
# register the feature
subscriptionId="<yourSubscriptionIDHere>"
az feature register \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId
az provider register \
    --namespace Microsoft.Storage
```

---

Die Registrierungsgenehmigung kann bis zu einer Stunde dauern. Verwenden Sie die folgenden Befehle, um zu überprüfen, ob die Registrierung abgeschlossen wurde:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die Registrierung der NFS 4.1-Funktionen für Azure Files zu überprüfen. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
Get-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az feature show \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId
```
---

## <a name="create-a-filestorage-storage-account"></a>Erstellen eines FileStorage-Speicherkontos
Derzeit sind NFS 4.1-Freigaben nur als Premium-Dateifreigaben verfügbar. Wenn Sie eine Premium-Dateifreigabe mit Unterstützung für das Protokoll NFS 4.1 bereitstellen möchten, müssen Sie zunächst ein FileStorage-Speicherkonto erstellen. Ein Speicherkonto ist ein Objekt auf oberster Ebene in Azure, das einen freigegebenen Speicherpool darstellt, der zum Bereitstellen mehrerer Azure-Dateifreigaben verwendet werden kann.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Navigieren Sie zum Erstellen eines FileStorage-Speicherkontos zum Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Menü **Speicherkonten** aus.

    ![Hauptseite im Azure-Portal: Auswählen eines Speicherkontos](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. Klicken Sie im angezeigten Fenster **Speicherkonten** auf **Hinzufügen**.
1. Wählen Sie das Abonnement aus, in dem das Speicherkonto erstellt werden soll.
1. Wählen Sie die Ressourcengruppe aus, in der das Speicherkonto erstellt werden soll.
1. Geben Sie als Nächstes einen Namen für Ihr Speicherkonto ein. Der gewählte Name muss innerhalb von Azure eindeutig sein. Der Name muss ebenfalls zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.
1. Wählen Sie einen Standort für Ihr Speicherkonto aus, oder verwenden Sie den Standardstandort.
1. Wählen Sie für **Leistung** den Wert **Premium** aus.

    Sie müssen **Premium** auswählen, damit **Dateifreigaben** in der Dropdownliste **Kontoart** verfügbar ist.

1. Wählen Sie unter **Premium account type** (Premium-Kontotyp) die Option **Dateifreigaben** aus.

    :::image type="content" source="media/storage-how-to-create-file-share/files-create-smb-share-performance-premium.png" alt-text="Screenshot: Leistung „Premium“ ausgewählt":::

1. Behalten Sie für **Replikation** den Standardwert **Lokal redundanter Speicher (LRS)** bei.
1. Wählen Sie **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.
1. Klicken Sie auf **Erstellen**.

Sobald Ihre Speicherkontoressource erstellt wurde, navigieren Sie dorthin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Um ein FileStorage-Speicherkonto zu erstellen, öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die folgenden Befehle aus. Ersetzen Sie dabei `<resource-group>` und `<storage-account>` durch die entsprechenden Werte für Ihre Umgebung.

```powershell
$resourceGroupName = "<resource-group>"
$storageAccountName = "<storage-account>"
$location = "westus2"

$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $location `
    -Kind FileStorage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um ein FileStorage-Speicherkonto zu erstellen, öffnen Sie Ihr Terminal, und führen Sie die folgenden Befehle aus. Ersetzen Sie dabei `<resource-group>` und `<storage-account>` durch die entsprechenden Werte für Ihre Umgebung.

```azurecli-interactive
resourceGroup="<resource-group>"
storageAccount="<storage-account>"
location="westus2"

az storage account create \
    --resource-group $resourceGroup \
    --name $storageAccount \
    --location $location \
    --sku Premium_LRS \
    --kind FileStorage
```
---

## <a name="create-an-nfs-share"></a>Erstellen einer NFS-Freigabe

# <a name="portal"></a>[Portal](#tab/azure-portal)

Nachdem Sie nun ein FileStorage-Konto erstellt und das Netzwerk konfiguriert haben, können Sie eine NFS-Dateifreigabe erstellen. Der Prozess ähnelt dem Erstellen einer SMB-Freigabe, Sie wählen jedoch anstelle von **SMB** beim Erstellen der Freigabe **NFS** aus.

1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie **Dateifreigaben** aus.
1. Wählen Sie **+ Dateifreigabe** aus, um eine neue Dateifreigabe zu erstellen.
1. Benennen Sie Ihre Dateifreigabe, und wählen Sie eine bereitgestellte Kapazität aus.
1. Wählen Sie unter **Protokoll** die Option **NFS** aus.
1. Treffen Sie für **Root-Squash** eine Auswahl.

    - Root-Squash (Standardwert): Zugriff für den Remotesuperuser (Stamm) wird UID (65534) und GID (65534) zugeordnet.
    - Kein Root-Squash: Remotesuperuser (Stamm) erhält Zugriff als Stamm.
    - All Squash: Der gesamte Benutzerzugriff wird UID (65534) und GID (65534) zugeordnet.
    
1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/files-nfs-create-share.png" alt-text="Screenshots: Blatt für die Erstellung einer Dateifreigabe":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Im Azure PowerShell-Modul verwenden Sie das Cmdlet [New-AzRmStorageShare](/powershell/module/az.storage/new-azrmstorageshare) zum Erstellen einer Premium-Dateifreigabe.

    > [!NOTE]
    > Premium-Dateifreigaben werden über ein bereitgestelltes Modell abgerechnet. Die bereitgestellte Größe der Freigabe wird unten durch `QuotaGiB` angegeben. Weitere Informationen finden Sie unter [Grundlegendes zu dem bereitgestellten Modell](understanding-billing.md#provisioned-model) und auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/).

    ```powershell
    New-AzRmStorageShare `
        -StorageAccount $storageAccount `
        -Name myshare `
        -EnabledProtocol NFS `
        -RootSquash RootSquash `
        -QuotaGiB 1024
    ```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
In der Azure CLI verwenden Sie den Befehl [az storage share create](/cli/azure/storage/share-rm) zum Erstellen einer Premium-Dateifreigabe.

> [!NOTE]
> Premium-Dateifreigaben werden über ein bereitgestelltes Modell abgerechnet. Die bereitgestellte Größe der Freigabe wird unten durch `quota` angegeben. Weitere Informationen finden Sie unter [Grundlegendes zu dem bereitgestellten Modell](understanding-billing.md#provisioned-model) und auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share-rm create \
    --resource-group $resourceGroup \
    --storage-account $storageAccount \
    --name "myshare" \
    --enabled-protocol NFS \
    --root-squash RootSquash \
    --quota 1024
```
---

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun eine NFS-Freigabe erstellt haben, müssen Sie sie in Ihren Linux-Client einbinden, um sie verwenden zu können. Weitere Informationen dazu finden Sie unter [Einbinden einer NFS-Freigabe](storage-files-how-to-mount-nfs-shares.md).

Wenn Probleme auftreten, finden Sie unter [Fehlerbehebung für NFS-Dateifreigaben in Azure](storage-troubleshooting-files-nfs.md) weitere Informationen.
