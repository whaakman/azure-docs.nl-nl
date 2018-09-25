---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/13/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 830deb7569772b610b7e6abde649830b7ad67a92
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047391"
---
## <a name="before-you-begin"></a>Voordat u begint

Als u wilt het voorbeeld in dit artikel hebt voltooid, moet u een bestaande beheerde installatiekopie van een gegeneraliseerde virtuele machine hebben. Zie voor meer informatie, [zelfstudie: een aangepaste installatiekopie van een Azure-VM maken met de Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). 

## <a name="preview-register-the-feature"></a>Voorbeeld: De functie registreren

Gedeelde Afbeeldingsgalerieën is in preview, maar u moet de functie registreren voordat u deze kunt gebruiken. De functie Gedeelde Afbeeldingsgalerieën registreren:

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

Het duurt een paar minuten voor het registreren van de functie. U kunt controleren de installatievoortgang met:

```azurecli-interactive
az provider show -n Microsoft.Compute
```

## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken 

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Galerie-namen moeten uniek zijn binnen uw abonnement. Maak een installatiekopie galerie met [az sig maken](/cli/azure/sig#az-sig-create). Het volgende voorbeeld wordt een galerie met de naam *myGallery* in *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create -g myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>De definitie van een installatiekopie maken

De definitie van een eerste installatiekopie maken in de galerie met [az sig installatiekopie-definitie maken](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```

## <a name="create-an-image-version"></a>De versie van een installatiekopie maken 
 
Versies van de installatiekopie maken indien nodig met behulp van [az image galerie maken-image-versie](/cli/azure/sig/image-version#az-sig-image-version-create). U moet doorgeven in de ID van de beheerde installatiekopie gebruiken als een basislijn voor het maken van de versie van de installatiekopie. U kunt [az afbeeldingenlijst](/cli/azure/image?view#az-image-list) voor informatie over installatiekopieën die zich in een resourcegroep. In dit voorbeeld de versie van de installatiekopie is *1.0.0* en we gaan maken van 5-totaal aantal replica's in de *West-Centraal VS*, *Zuid-centraal VS* en VS-Oost 2 * regio's.

```azurecli-interactive 
az sig image-version create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1" \
   --replica-count 5 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

