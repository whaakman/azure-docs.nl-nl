---
title: Een Windows-VM maken met Azure Image Builder (preview)
description: Maak een Windows VM met de opbouwfunctie voor installatiekopieën van Azure.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: jeconnoc
ms.openlocfilehash: 01109aa83c12bda9b1d21ec25784d663f8abf700
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159718"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Preview: Een Windows VM maken met Azure Image Builder

Dit artikel is om weer te geven hoe u een aangepaste Windows-installatiekopie met behulp van de Azure VM Image Builder kunt maken. Het voorbeeld in dit artikel worden drie verschillende [aanpassers](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) voor het aanpassen van de afbeelding:
- PowerShell (ScriptUri) - downloaden en uitvoeren van een [PowerShell-script](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Opnieuw opstarten van Windows - wordt de VM opnieuw opgestart.
- PowerShell (inline): Voer een specifieke opdracht uit. In dit voorbeeld wordt een map wordt gemaakt op de virtuele machine met behulp `mkdir c:\\buildActions`.
- Bestand - een bestand kopiëren vanuit GitHub op de virtuele machine. In dit voorbeeld kopieert [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) naar `c:\buildArtifacts\index.html` op de virtuele machine.

We gebruiken een voorbeeldsjabloon .json het configureren van de installatiekopie. Het .json-bestand dat we gebruiken hier is: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


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
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Maak een variabele voor uw abonnements-ID. U krijgt deze met `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Maak de resourcegroep.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

Image Builder machtigen om resources te maken in die resourcegroep. De `--assignee` waarde is de app-registratie-ID voor de Image Builder-service. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-json-example"></a>De JSON-voorbeeld downloaden

Het voorbeeld .json-bestand te downloaden en deze configureren met de variabelen die u hebt gemaakt.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

## <a name="create-the-image"></a>De installatiekopie maken

De configuratie van de installatiekopie met de VM Image Builder-service verzenden

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Start de build van de installatiekopie.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Wacht totdat de bewerking voltooid is. Dit kan ongeveer 15 minuten duren.

## <a name="create-the-vm"></a>De virtuele machine maken

Maak de virtuele machine met behulp van de installatiekopie die u hebt gemaakt. Vervang *<password>* door uw eigen wachtwoord voor de `aibuser` op de virtuele machine.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Controleer of de aanpassing

Maak een extern-bureaubladverbinding met de virtuele machine met behulp van de gebruikersnaam en het wachtwoord die u hebt ingesteld toen u de virtuele machine hebt gemaakt. Binnen de virtuele machine, open een opdrachtprompt en typ:

```console
dir c:\
```

Hier ziet u deze twee mappen die zijn gemaakt tijdens de aanpassing van de afbeelding:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Opruimen

Wanneer u klaar bent, kunt u de resources verwijderen.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
az group delete -n $imageResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de onderdelen van het .json-bestand dat is gebruikt in dit artikel, [Image builder sjabloonverwijzing](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

