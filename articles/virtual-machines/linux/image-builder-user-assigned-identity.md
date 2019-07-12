---
title: De installatiekopie van een virtuele Machine maken en gebruiken van een gebruiker toegewezen beheerde identiteit voor toegang tot bestanden in Azure Storage (preview)
description: Maak VM-installatiekopie met behulp van Azure Image Builder, die toegang hebben tot bestanden die zijn opgeslagen in Azure Storage met behulp van de gebruiker toegewezen beheerde identiteit.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: b6347765f8d2e21c352834dc8d28b65c28f99758
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671450"
---
# <a name="create-an-image-and-use-a-user-assigned-managed-identity-to-access-files-in-azure-storage"></a>Een installatiekopie maken en gebruiken van een gebruiker toegewezen beheerde identiteit voor toegang tot bestanden in Azure Storage 

Azure ondersteunt het Image Builder met behulp van scripts of kopiëren van bestanden vanaf meerdere locaties, zoals GitHub en Azure storage enzovoort. Als u wilt gebruiken, moet zijn extern toegankelijk is voor Azure Image Builder, maar u kunt Azure Storage-blobs met behulp van SAS-Tokens kan beveiligen.

In dit artikel laat zien hoe het maken van een aangepaste installatiekopie met behulp van de Azure VM Image Builder, waar de service wordt gebruikt een [beheerde identiteit gebruiker toegewezen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) voor toegang tot bestanden in Azure storage voor de aanpassing van de installatiekopie, zonder u hoeft te maken de bestanden die openbaar toegankelijk is, of instellen van de SAS-tokens.

In het volgende voorbeeld maakt u twee resourcegroepen, één zal worden gebruikt voor de aangepaste installatiekopie en de andere fungeert als host voor een Azure Storage-Account, met een scriptbestand. Dit simuleert een echte leven-scenario, waarin u build-artefacten of afbeeldingsbestanden in verschillende opslagaccounts, buiten Image Builder kan hebben. U maakt een gebruiker toegewezen identiteit en klik vervolgens op verlenen die-machtigingen voor het scriptbestand lezen, maar u openbare toegang niet wordt ingesteld op dat bestand. U gebruikt vervolgens de Shell-systeemaanpasser downloaden en uitvoeren van script van de storage-account.


> [!IMPORTANT]
> Azure Image Builder is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="register-the-features"></a>Registreren van de functies
Voor het gebruik van Azure Image Builder tijdens de Preview-versie, moet u de nieuwe functie te registreren.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Controleer de status van de functieregistratie van de.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Controleer uw registratie.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState

az provider show -n Microsoft.Storage | grep registrationState
```

Als ze niet geregistreerd zijn doen, voert u het volgende uit:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```


## <a name="create-a-resource-group"></a>Een resourcegroep maken

We gebruiken sommige stukjes informatie herhaaldelijk, dus we gaan enkele variabelen die voor het opslaan van die informatie maken.


```azurecli-interactive
# Image resource group name 
imageResourceGroup=aibmdimsi
# storage resource group
strResourceGroup=aibmdimsistor
# Location 
location=WestUS2
# name of the image to be created
imageName=aibCustLinuxImgMsi01
# image distribution metadata reference name
runOutputName=u1804ManImgMsiro
```

Maak een variabele voor uw abonnements-ID. U krijgt deze met `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Maak de resourcegroepen voor de installatiekopie en de script-opslag.

```azurecli-interactive
# create resource group for image template
az group create -n $imageResourceGroup -l $location
# create resource group for the script storage
az group create -n $strResourceGroup -l $location
```


De opslag maken en het voorbeeldscript naartoe kopiëren vanuit GitHub.

```azurecli-interactive
# script storage account
scriptStorageAcc=aibstorscript$(date +'%s')

# script container
scriptStorageAccContainer=scriptscont$(date +'%s')

# script url
scriptUrl=https://$scriptStorageAcc.blob.core.windows.net/$scriptStorageAccContainer/customizeScript.sh

# create storage account and blob in resource group
az storage account create -n $scriptStorageAcc -g $strResourceGroup -l $location --sku Standard_LRS

az storage container create -n $scriptStorageAccContainer --fail-on-exist --account-name $scriptStorageAcc

# copy in an example script from the GitHub repo 
az storage blob copy start \
    --destination-blob customizeScript.sh \
    --destination-container $scriptStorageAccContainer \
    --account-name $scriptStorageAcc \
    --source-uri https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh
```



Image Builder machtigen om resources te maken in de resourcegroep van de installatiekopie. De `--assignee` waarde is de app-registratie-ID voor de Image Builder-service. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="create-user-assigned-managed-identity"></a>Maken van de gebruiker toegewezen beheerde identiteit

De identiteit maken en toewijzen van machtigingen voor het opslagaccount van het script. Zie voor meer informatie, [User-Assigned beheerde identiteit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

```azurecli-interactive
# Create the user assigned identity 
identityName=aibBuiUserId$(date +'%s')
az identity create -g $imageResourceGroup -n $identityName
# assign the identity permissions to the storage account, so it can read the script blob
imgBuilderCliId=$(az identity show -g $imageResourceGroup -n $identityName | grep "clientId" | cut -c16- | tr -d '",')
az role assignment create \
    --assignee $imgBuilderCliId \
    --role "Storage Blob Data Reader" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$strResourceGroup/providers/Microsoft.Storage/storageAccounts/$scriptStorageAcc/blobServices/default/containers/$scriptStorageAccContainer 
# create the user identity URI
imgBuilderId=/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/$identityName
```


## <a name="modify-the-example"></a>Wijzig het voorbeeld

Het voorbeeld .json-bestand te downloaden en deze configureren met de variabelen die u hebt gemaakt.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage/helloImageTemplateMsi.json -o helloImageTemplateMsi.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateMsi.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateMsi.json
sed -i -e "s/<region>/$location/g" helloImageTemplateMsi.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateMsi.json
sed -i -e "s%<scriptUrl>%$scriptUrl%g" helloImageTemplateMsi.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateMsi.json
sed -i -e "s%<runOutputName>%$runOutputName%g" helloImageTemplateMsi.json
```

## <a name="create-the-image"></a>De installatiekopie maken

De configuratie van de installatiekopie met de service Azure Image Builder indienen.

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateMsi.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
```

Start de build van de installatiekopie.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateMsi01 \
     --action Run 
```

Wacht tot de build te voltooien. Dit kan ongeveer 15 minuten duren.

## <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine maken op basis van de installatiekopie. 

```bash
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgVm00 \
  --admin-username aibuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Nadat de virtuele machine is gemaakt, start u een SSH-sessie met de virtuele machine.

```azurecli-interactive
ssh aibuser@<publicIp>
```

U ziet nu dat de installatiekopie is aangepast met een bericht van de dag zodra de SSH-verbinding tot stand is gebracht.

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up"></a>Opruimen

Wanneer u klaar bent, kunt u de resources verwijderen als ze niet langer nodig zijn.

```azurecli-interactive
az identity delete --ids $imgBuilderId
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
az group delete -n $imageResourceGroup
az group delete -n $strResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Als u eventuele problemen ondervindt bij het werken met Azure Image Builder, Zie [probleemoplossing](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md?toc=%2fazure%2fvirtual-machines%context%2ftoc.json).