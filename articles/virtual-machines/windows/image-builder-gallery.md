---
title: Azure Image Builder gebruiken met een galerie met installatiekopieën voor virtuele machines van Windows (preview)
description: Windows-installatiekopieën maken met Azure Image Builder en gedeelde galerie met installatiekopieën.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-widows
manager: jeconnoc
ms.openlocfilehash: 2453d37720bcf48b95b428cf78c6186de40b31aa
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160108"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Preview: Een Windows-installatiekopie maken en deze distribueren naar de galerie met installatiekopieën van een gedeeld 

In dit artikel is om weer te geven hoe u de Azure-Image Builder kunt gebruiken om te maken van de versie van een installatiekopie in een [gedeelde afbeeldingengalerie](shared-image-galleries.md), klikt u vervolgens de installatiekopie van het wereldwijd distribueren.

We gebruiken een .json-sjabloon voor het configureren van de installatiekopie. Het .json-bestand dat we gebruiken hier is: [helloImageTemplateforWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json). 

Als u wilt de installatiekopie distribueren naar de galerie met installatiekopieën van een gedeelde, de sjabloon maakt gebruik van [sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) als de waarde voor de `distribute` gedeelte van de sjabloon.

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
az provider show -n Microsoft.Compute | grep registrationState
```

Als ze niet geregistreerd zijn doen, voert u het volgende uit:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```

## <a name="set-variables-and-permissions"></a>Machtigingen en variabelen instellen 

We gebruiken sommige stukjes informatie herhaaldelijk, dus we gaan enkele variabelen die voor het opslaan van die informatie maken. Vervang de waarden voor de variabelen zoals `username` en `vmpassword`, door uw eigen waarden.

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="azureuser"
vmpassword="passwordfortheVM"
```

Maak een variabele voor uw abonnements-ID. U krijgt deze met `az account show | grep id`.

```azurecli-interactive
subscriptionID="Subscription ID"
```

Maak de resourcegroep.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


Geef Azure Image Builder toestemming voor het maken van resources in die resourcegroep. De `--assignee` waarde is de app-registratie-ID voor de Image Builder-service. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```





## <a name="create-an-image-definition-and-gallery"></a>Maak een definitie van de installatiekopie en een galerie

Maak een galerie met installatiekopieën. 

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

De definitie van een installatiekopie maken.

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher corpIT \
   --offer myOffer \
   --sku 2019 \
   --os-type Windows
```


## <a name="download-and-configure-the-json"></a>Downloaden en configureren van de .json

De JSON-sjabloon downloaden en configureer deze met uw variabelen.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json -o helloImageTemplateforWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforWinSIG.json
```

## <a name="create-the-image-version"></a>Versie van de installatiekopie maken

Het volgende gedeelte wordt de versie van de installatiekopie maken in de galerie. 

De configuratie van de installatiekopie met de service Azure Image Builder indienen.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Start de build van de installatiekopie.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforWinSIG01 \
     --action Run 
```

Even kunnen duren voordat de installatiekopie is gemaakt en deze te repliceren naar beide regio's. Wacht totdat deze onderdeel is voltooid voordat u doorgaat met het maken van een virtuele machine.


## <a name="create-the-vm"></a>De virtuele machine maken

Een virtuele machine maken vanaf de versie van de installatiekopie die is gemaakt door Azure Image Builder.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm001 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```


## <a name="verify-the-customization"></a>Controleer of de aanpassing
Maak een extern-bureaubladverbinding met de virtuele machine met behulp van de gebruikersnaam en het wachtwoord die u hebt ingesteld toen u de virtuele machine hebt gemaakt. Binnen de virtuele machine, open een opdrachtprompt en typ:

```console
dir c:\
```

U ziet nu een map met de naam `buildActions` die tijdens de aanpassing van de installatiekopie is gemaakt.


## <a name="clean-up-resources"></a>Resources opschonen
Als u nu proberen opnieuw aanpassen van de versie van de installatiekopie wilt voor het maken van een nieuwe versie van de dezelfde installatiekopie **Sla deze stap over** en gaat u op naar [gebruik Azure Image Builder te maken van een andere versie van de installatiekopie](image-builder-gallery-update-image-version.md).


Hiermee wordt de afbeelding die is gemaakt, samen met alle van de andere resourcebestanden verwijderd. Zorg ervoor dat u klaar bent met deze implementatie voordat u verwijdert de resources.

Bij het verwijderen van installatiekopie galerie resources, moet u alle van de versies van een installatiekopie verwijderen voordat u de definitie van de installatiekopie die wordt gebruikt om ze te maken kunt verwijderen. Als u wilt verwijderen van een galerie, moet u eerst alle van de installatiekopie die in de galerie hebt verwijderd.

Verwijder de image builder-sjabloon.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Ophalen van de versie van de installatiekopie die zijn gemaakt door de functie voor installatiekopieën maken, dit wordt altijd gestart met `0.`, en verwijder vervolgens de versie van de installatiekopie

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


Definitie van de installatiekopie verwijderen.

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

Verwijder de galerie.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Verwijder de resourcegroep.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het bijwerken van de versie van de installatiekopie die u hebt gemaakt, [gebruik Azure Image Builder te maken van een andere versie van de installatiekopie](image-builder-gallery-update-image-version.md).