---
title: Verwenden von Azure Image Builder und Shared Image Gallery für Linux-VMs
description: Erfahren Sie, wie Sie mit Azure Image Builder und der Azure CLI eine Imageversion in einer Shared Image Gallery erstellen und das Image dann global verteilen.
author: kof-f
ms.author: kofiforson
ms.reviewer: cynthn
ms.date: 03/02/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: image-builder
ms.openlocfilehash: 9c3af46cbb3eb669aa864f5d6b1fa95c82cf8e1b
ms.sourcegitcommit: 2cff2a795ff39f7f0f427b5412869c65ca3d8515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2021
ms.locfileid: "113596513"
---
# <a name="create-a-linux-image-and-distribute-it-to-a-shared-image-gallery"></a>Erstellen eines Linux-Images und Verteilen des Images über einen Katalog für freigegebene Images 

In diesem Artikel erfahren Sie, wie Sie mit Azure Image Builder und der Azure CLI eine Imageversion in einem [Katalog für freigegebene Images](../shared-image-galleries.md) erstellen und wie Sie diese dann global verteilen. Sie können zu diesem Zweck auch [Azure PowerShell](../windows/image-builder-gallery.md) verwenden.


Wir verwenden zum Konfigurieren des Images eine JSON-Beispielvorlage. Wir verwenden die JSON-Datei [helloImageTemplateforSIG.json](https://github.com/azure/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json). 

Die Vorlage verwendet [sharedImage](image-builder-json.md#distribute-sharedimage) als Wert für den `distribute`-Abschnitt der Vorlage, um das Image in einem Katalog für freigegebene Images zu verteilen.


## <a name="register-the-features"></a>Registrieren des Features

Um Azure Image Builder zu verwenden, müssen Sie das Feature registrieren.

Überprüfen Sie die Registrierung.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.KeyVault | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Network | grep registrationState
```

Wenn nicht „registered“ ausgegeben wird, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Compute
az provider register -n Microsoft.KeyVault
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Network
```

## <a name="set-variables-and-permissions"></a>Festlegen von Variablen und Berechtigungen 

Einige Angaben verwenden wir wiederholt. Aus diesem Grund erstellen wir einige Variablen, um diese Informationen zu speichern.

Image Builder unterstützt nur das Erstellen von benutzerdefinierten Images in derselben Ressourcengruppe, in der sich auch das verwaltete Quellimage befindet. Aktualisieren Sie den Namen der Ressourcengruppe in diesem Beispiel, sodass es sich um dieselbe Ressourcengruppe handelt, in der sich auch das verwaltete Quellimage befindet.

```azurecli-interactive
# Resource group name - we are using ibLinuxGalleryRG in this example
sigResourceGroup=ibLinuxGalleryRG
# Datacenter location - we are using West US 2 in this example
location=westus2
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=myIbGallery
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibLinuxSIG
```

Erstellen Sie eine Variable für Ihre Abonnement-ID. Diese können Sie mit `az account show | grep id` abrufen.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Erstellen Sie die Ressourcengruppe.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```

## <a name="create-a-user-assigned-identity-and-set-permissions-on-the-resource-group"></a>Erstellen einer vom Benutzer zugewiesene Identität und Festlegen von Berechtigungen für die Ressourcengruppe
Image Builder verwendet die angegebene [Benutzeridentität](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity), um das Image in die Azure Shared Image Gallery (SIG) einzufügen. In diesem Beispiel erstellen Sie eine Azure-Rollendefinition, die über die präzisen Aktionen zur Verteilung des Images an die SIG verfügt. Die Rollendefinition wird dann der Benutzeridentität zugewiesen.

```bash
# create user assigned identity for image builder to access the storage account where the script is located
idenityName=aibBuiUserId$(date +'%s')
az identity create -g $sigResourceGroup -n $idenityName

# get identity id
imgBuilderCliId=$(az identity show -g $sigResourceGroup -n $idenityName | grep "clientId" | cut -c16- | tr -d '",')

# get the user identity URI, needed for the template
imgBuilderId=/subscriptions/$subscriptionID/resourcegroups/$sigResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/$idenityName

# this command will download an Azure role definition template, and update the template with the parameters specified earlier.
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json -o aibRoleImageCreation.json

imageRoleDefName="Azure Image Builder Image Def"$(date +'%s')

# update the definition
sed -i -e "s/<subscriptionID>/$subscriptionID/g" aibRoleImageCreation.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" aibRoleImageCreation.json
sed -i -e "s/Azure Image Builder Service Image Creation Role/$imageRoleDefName/g" aibRoleImageCreation.json

# create role definitions
az role definition create --role-definition ./aibRoleImageCreation.json

# grant role definition to the user assigned identity
az role assignment create \
    --assignee $imgBuilderCliId \
    --role $imageRoleDefName \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="create-an-image-definition-and-gallery"></a>Erstellen einer Imagedefinition und eines Imagekatalogs

Zum Verwenden von Image Builder mit einem Katalog mit freigegebenen Images benötigen Sie einen vorhandenen Imagekatalog und eine Imagedefinition. Imagekatalog und Imagedefinition werden nicht von Image Builder für Sie erstellt.

Wenn Sie noch nicht über einen zu verwendenden Katalog und eine Imagedefinition verfügen, erstellen Sie diese zunächst. Erstellen Sie zuerst einen Imagekatalog.

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

Erstellen Sie anschließend eine Imagedefinition.

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher myIbPublisher \
   --offer myOffer \
   --sku 18.04-LTS \
   --os-type Linux
```


## <a name="download-and-configure-the-json"></a>Herunterladen und Konfigurieren der JSON-Datei

Laden Sie die JSON-Vorlage herunter, und konfigurieren Sie diese mit Ihren Variablen.

```azurecli-interactive
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json -o helloImageTemplateforSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIG.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateforSIG.json
```

## <a name="create-the-image-version"></a>Erstellen der Imageversion

Im folgenden Teil erstellen Sie die Imageversion im Katalog. 

Senden Sie die Imagekonfiguration an den Azure Image Builder-Dienst.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Starten Sie den Imagebuild.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIG01 \
     --action Run 
```

Das Erstellen des Images und Replizieren in beiden Regionen kann einige Zeit in Anspruch nehmen. Warten Sie den Abschluss des Vorgangs ab, bevor Sie eine VM erstellen.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie eine VM aus der Imageversion, die von Azure Image Builder erstellt wurde.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name myAibGalleryVM \
  --admin-username aibuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

Stellen Sie eine SSH-Verbindung mit der VM her.

```azurecli-interactive
ssh aibuser@<publicIpAddress>
```

Sie sollten sehen, dass das Image angepasst wurde. Es wird eine *Nachricht des Tages* ausgegeben, sobald die SSH-Verbindung hergestellt wurde.

```console
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Imageversion erneut anpassen möchten, um eine neue Version desselben Images zu erstellen, überspringen Sie die nächsten Schritte, und [erstellen Sie mit Azure Image Builder eine weitere Imageversion](image-builder-gallery-update-image-version.md).


Das erstellte Image und dessen Ressourcendateien werden gelöscht. Achten Sie darauf, dass Sie mit dieser Bereitstellung fertig sind, bevor Sie die Ressourcen löschen.

Wenn Sie die Imagekatalogressourcen löschen, müssen Sie alle Imageversionen löschen, bevor Sie die Imagedefinition löschen können, die für deren Erstellung verwendet wurde. Sie müssen zunächst alle Imagedefinitionen im Katalog löschen, um den Katalog löschen zu können.

Löschen Sie die Image Builder-Vorlage.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Löschen von Berechtigungszuordnungen, Rollen und Identitäten
```azurecli-interactive
az role assignment delete \
    --assignee $imgBuilderCliId \
    --role "$imageRoleDefName" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup

az role definition delete --name "$imageRoleDefName"

az identity delete --ids $imgBuilderId
```

Rufen Sie die von Image Builder erstellte Imageversion ab (diese beginnt immer mit `0.`), und löschen Sie diese.

```azurecli-interactive
sigDefImgVersion=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'name' -o json | grep 0. | tr -d '"')
az sig image-version delete \
   -g $sigResourceGroup \
   --gallery-image-version $sigDefImgVersion \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```   


Löschen Sie die Imagedefinition.

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

Löschen Sie den Katalog.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Löschen Sie die Ressourcengruppe.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure-Kataloge mit freigegebenen Images](../shared-image-galleries.md).
