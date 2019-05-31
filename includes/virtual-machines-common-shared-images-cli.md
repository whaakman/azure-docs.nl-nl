---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 841027fe8d6b97e661faa038dc9381edbb3d4cd8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226045"
---
## <a name="before-you-begin"></a>Voordat u begint

Als u wilt het voorbeeld in dit artikel hebt voltooid, moet u een bestaande beheerde installatiekopie van een gegeneraliseerde virtuele machine hebben. Zie [Zelfstudie: Een aangepaste installatiekopie van een Azure-VM maken met de Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Als de beheerde installatiekopie een gegevensschijf bevat, mag de grootte van de gegevensschijf niet meer dan 1 TB.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Als u liever te installeren en de CLI lokaal gebruikt, Zie [Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken 

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Toegestane tekens voor de naam van de galerie worden hoofdletters of kleine letters, cijfers, punten en perioden. Naam van de galerie mag geen streepjes bevatten.   Galerie-namen moeten uniek zijn binnen uw abonnement. 

Maak een installatiekopie galerie met [az sig maken](/cli/azure/sig#az-sig-create). Het volgende voorbeeld wordt een galerie met de naam *myGallery* in *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>De definitie van een installatiekopie maken

Definities van de installatiekopie van maken een logische groepering van installatiekopieën. Ze worden gebruikt voor het beheren van informatie over de versies van een installatiekopie die in deze worden gemaakt. Namen van de definitie van afbeeldingen kunnen bestaan uit hoofdletters of kleine letters, cijfers, punten, streepjes en punten. Zie voor meer informatie over de waarden die u voor de definitie van een installatiekopie opgeven kunt [Image definities](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

De definitie van een eerste installatiekopie maken in de galerie met [az sig installatiekopie-definitie maken](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
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

In dit voorbeeld de versie van de installatiekopie is *1.0.0* en we gaan maken 2 replica's in de *West-Centraal VS* regio, 1 replica in de *Zuid-centraal VS* regio en 1 replica's in de *VS-Oost 2* regio met behulp van de zone-redundante opslag.


```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1=Standard_ZRS" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> U moet wachten tot de versie van de installatiekopie wordt gemaakt en gerepliceerd voordat u dezelfde beheerde installatiekopie kunt maken van een andere versie van de installatiekopie volledig te voltooien.
>
> U kunt ook alle uw installatiekopie versie replica's in opslaan [Zone-redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) door toe te voegen `--storage-account-type standard_zrs` bij het maken van de versie van de installatiekopie.
>

## <a name="share-the-gallery"></a>Delen van de galerie

Het is raadzaam dat u met andere gebruikers op het niveau van de galerie deelt. Als u de object-ID van de galerie, gebruikt [az sig show](/cli/azure/sig#az-sig-show).

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

De object-ID gebruiken als bereik, samen met een e-mailadres en [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create) aan een gebruikerstoegang geven tot de gedeelde afbeeldingengalerie.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```


