---
title: Een Linux-VM maken met Azure Image Builder (preview)
description: Maak een virtuele Linux-machine met de opbouwfunctie voor installatiekopieën van Azure.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: 854645af95d780053d94668921e41ac189bbbfb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159508"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Preview: Een virtuele Linux-machine maken met Azure Image Builder

Dit artikel leest u hoe u een aangepaste installatiekopie van Linux met behulp van de Azure-functie voor installatiekopieën te maken en de Azure CLI kunt maken. Het voorbeeld in dit artikel worden drie verschillende [aanpassers](image-builder-json.md#properties-customize) voor het aanpassen van de afbeelding:

- Shell (ScriptUri) - downloads en wordt uitgevoerd een [shell-script](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh).
- Shell (inline) - specifieke opdrachten wordt uitgevoerd. In dit voorbeeld zijn de inline-opdrachten het maken van een map en het besturingssysteem.
- Bestand - exemplaren een [bestand vanuit GitHub](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) in een map op de virtuele machine.

We gebruiken een voorbeeldsjabloon .json het configureren van de installatiekopie. Het .json-bestand dat we gebruiken hier is: [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

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
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
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
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

## <a name="create-the-image"></a>De installatiekopie maken
De configuratie van de installatiekopie met de VM Image Builder-service verzenden

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Start de build van de installatiekopie.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Wacht totdat de bewerking voltooid is. Dit kan ongeveer 15 minuten duren.


## <a name="create-the-vm"></a>De virtuele machine maken

Maak de virtuele machine met behulp van de installatiekopie die u hebt gemaakt.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Het IP-adres ophalen uit de uitvoer van de virtuele machine maken en deze gebruiken voor SSH naar de virtuele machine.

```azurecli-interactive
ssh azureuser@<pubIp>
```

U ziet nu dat de installatiekopie is aangepast met een bericht van de dag zodra de SSH-verbinding tot stand is gebracht.

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Type `exit` wanneer u klaar bent om te sluiten van de SSH-verbinding.

## <a name="check-the-source"></a>Controleer de bron

In de Image Builder-sjabloon, in de 'Eigenschappen', ziet u de broninstallatiekopie, aanpassing script wordt uitgevoerd, en waar deze is gedistribueerd.

```azurecli-interactive
cat helloImageTemplateLinux.json
```

Zie voor meer informatie over deze JSON-bestand, [Image builder sjabloonverwijzing](image-builder-json.md)

## <a name="clean-up"></a>Opruimen

Wanneer u klaar bent, kunt u de resources verwijderen.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01

az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de onderdelen van het .json-bestand dat is gebruikt in dit artikel, [Image Builder sjabloonverwijzing](image-builder-json.md).