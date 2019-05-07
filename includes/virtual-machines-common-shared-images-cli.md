---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 04/30/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 7e4ca54d8f97646192d19d5923bee24a906e8df7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149675"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt het voorbeeld in dit artikel hebt voltooid, moet u een bestaande beheerde installatiekopie van een gegeneraliseerde virtuele machine hebben. Zie [Zelfstudie: Een aangepaste installatiekopie van een Azure-VM maken met de Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). 


## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken 

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Toegestane tekens voor de naam van de galerie worden hoofdletters of kleine letters, cijfers, punten en perioden. Naam van de galerie mag geen streepjes bevatten.   Galerie-namen moeten uniek zijn binnen uw abonnement. 

Maak een installatiekopie galerie met [az sig maken](/cli/azure/sig#az-sig-create). Het volgende voorbeeld wordt een galerie met de naam *myGallery* in *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create -g myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>De definitie van een installatiekopie maken

Definities van de installatiekopie van maken een logische groepering van installatiekopieën. Ze worden gebruikt voor het beheren van informatie over de versies van een installatiekopie die in deze worden gemaakt. Namen van de definitie van afbeeldingen kunnen bestaan uit hoofdletters of kleine letters, cijfers, punten, streepjes en punten. Zie voor meer informatie over de waarden die u voor de definitie van een installatiekopie opgeven kunt [Image definities](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

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

Versies van de installatiekopie maken indien nodig met behulp van [az image galerie maken-image-versie](/cli/azure/sig/image-version#az-sig-image-version-create). U moet doorgeven in de ID van de beheerde installatiekopie gebruiken als een basislijn voor het maken van de versie van de installatiekopie. U kunt [az afbeeldingenlijst](/cli/azure/image?view#az-image-list) voor informatie over installatiekopieën die zich in een resourcegroep. 

Toegestane tekens in voor de versie van installatiekopie zijn cijfers en punten. Cijfers moet binnen het bereik van een 32-bits geheel getal zijn. Indeling: *MajorVersion*.*MinorVersion*.*Patch*.

In dit voorbeeld de versie van de installatiekopie is *1.0.0* en we gaan maken 2 replica's in de *West-Centraal VS* regio, 1 replica in de *Zuid-centraal VS* regio en 1 replica's in de *VS-Oost 2* regio.


```azurecli-interactive 
az sig image-version create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> U moet wachten tot de versie van de installatiekopie wordt gemaakt en gerepliceerd voordat u dezelfde beheerde installatiekopie kunt maken van een andere versie van de installatiekopie volledig te voltooien.