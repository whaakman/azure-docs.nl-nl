---
title: Maken van een nieuwe versie van de installatiekopie van een bestaande installatiekopieversie met behulp van Azure Image Builder (preview)
description: Maak een nieuwe installatiekopieversie van een bestaande installatiekopieversie met behulp van Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: jeconnoc
ms.openlocfilehash: f1bce4c2e5c7ae9dc7ec5917fbc5ac115eecdffa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65160093"
---
# <a name="preview-create-a-new-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Preview: Maken van een nieuwe versie van de installatiekopie van een bestaande installatiekopieversie met behulp van Azure Image Builder

Dit artikel leest u hoe u kunt een bestaande versie van de installatiekopie van een [gedeelde afbeeldingengalerie](shared-image-galleries.md), bijwerken en deze publiceren als een nieuwe installatiekopieversie naar de galerie.

We gebruiken een voorbeeldsjabloon .json het configureren van de installatiekopie. Het .json-bestand dat we gebruiken hier is: [helloImageTemplateforSIGfromWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json). 

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

Als u hebt gebruikt [een installatiekopie maken en distribueren naar de galerie met installatiekopieën van een gedeelde](image-builder-gallery.md) voor het maken van de galerie met installatiekopieën voor gedeelde, u de variabelen die we nodig hebben al hebt gemaakt. Als dat niet het geval is, stelt u een enkele variabelen die moet worden gebruikt voor dit voorbeeld.

Voor Preview-versie ondersteunt image builder alleen in dezelfde resourcegroep bevinden als de bron beheerde installatiekopie maken van aangepaste installatiekopieën. Naam van de resourcegroep in dit voorbeeld moet dezelfde resourcegroep bevinden als uw beheerde broninstallatiekopie bijwerken.

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
username="user name for the VM"
vmpassword="password for the VM"
```

Maak een variabele voor uw abonnements-ID. U krijgt deze met `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Ophalen van de versie van de installatiekopie die u wilt bijwerken.

```azurecli-interactive
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```


Als u al uw eigen afbeeldingengalerie gedeeld, en niet voldeed aan het vorige voorbeeld, moet u machtigingen voor Image Builder voor toegang tot de resourcegroep, zodat deze toegang heeft tot de galerie toe te wijzen.


```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="modify-helloimage-example"></a>Voorbeeld van de helloImage wijzigen
U kunt bekijken in het voorbeeld worden gebruikt door het openen van het .json-bestand hier: [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) samen met de [Image Builder sjabloonverwijzing](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


De JSON-voorbeeld downloaden en configureer deze met uw variabelen. 

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json -o helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromWinSIG.json
```

## <a name="create-the-image"></a>De installatiekopie maken

De configuratie van de installatiekopie van de VM Image Builder-Service verzenden.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n imageTemplateforSIGfromWinSIG01
```

Start de build van de installatiekopie.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n imageTemplateforSIGfromWinSIG01 \
     --action Run 
```

Wacht totdat de installatiekopie is gemaakt en replicatie voordat u doorgaat met de volgende stap.


## <a name="create-the-vm"></a>De virtuele machine maken

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm002 \
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

U ziet nu twee mappen:
- `buildActions` dat is gemaakt in de eerste versie van de installatiekopie.
- `buildActions2` dat is gemaakt als onderdeel van de eerste afbeeldingsversie voor het maken van de tweede versie van de installatiekopie wordt bijgewerkt.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de onderdelen van het .json-bestand dat is gebruikt in dit artikel, [Image builder sjabloonverwijzing](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).