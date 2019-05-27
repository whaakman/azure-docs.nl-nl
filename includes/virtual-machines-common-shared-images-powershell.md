---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 82187b05a398c066f9da94c57cbe8a59a6ba3275
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145810"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/powershell](https://shell.azure.com/powershell) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.


## <a name="get-the-managed-image"></a>De installatiekopie van het beheerde ophalen

U ziet een lijst met installatiekopieën die beschikbaar in een resource-groep met zijn [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Zodra u weet dat de installatiekopie met de naam en de resourcegroep waarvan het zich bevindt, kunt u `Get-AzImage` opnieuw naar de installatiekopie-object ophalen en opslaan in een variabele voor later gebruik. In dit voorbeeld wordt een installatiekopie met de naam *myImage* uit de resourcegroep 'myResourceGroup' en wijst deze toe aan de variabele *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken 

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Toegestane tekens voor de naam van de galerie worden hoofdletters of kleine letters, cijfers, punten en perioden. Naam van de galerie mag geen streepjes bevatten. Galerie-namen moeten uniek zijn binnen uw abonnement. 

Maak een installatiekopie galerie met [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). Het volgende voorbeeld wordt een galerie met de naam *myGallery* in de *myGalleryRG* resourcegroep.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>De definitie van een installatiekopie maken 

Definities van de installatiekopie van maken een logische groepering van installatiekopieën. Ze worden gebruikt voor het beheren van informatie over de versies van een installatiekopie die in deze worden gemaakt. Namen van de definitie van afbeeldingen kunnen bestaan uit hoofdletters of kleine letters, cijfers, punten, streepjes en punten. Zie voor meer informatie over de waarden die u voor de definitie van een installatiekopie opgeven kunt [Image definities](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Maken van de installatiekopie definitie met [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). In dit voorbeeld wordt de afbeelding met de naam *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```


## <a name="create-an-image-version"></a>De versie van een installatiekopie maken

De versie van een installatiekopie van het gebruik van een beheerde installatiekopie maken [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). 

Toegestane tekens in voor de versie van installatiekopie zijn cijfers en punten. Cijfers moet binnen het bereik van een 32-bits geheel getal zijn. Indeling: *MajorVersion*.*MinorVersion*.*Patch*.

In dit voorbeeld de versie van de installatiekopie is *1.0.0* en deze worden gerepliceerd naar beide *West-Centraal VS* en *Zuid-centraal VS* datacenters. Bij het kiezen van doelregio's voor replicatie, houd er rekening mee dat u ook hebt om op te nemen de *bron* regio bevinden als een doel voor replicatie.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Duurt het even voor het repliceren van de afbeelding op alle van de doelregio's, zodat we een taak hebt gemaakt, zodat we de voortgang kunt bijhouden. Ziet u de voortgang van de taak `$job.State`.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> U moet wachten tot de versie van de installatiekopie wordt gemaakt en gerepliceerd voordat u dezelfde beheerde installatiekopie kunt maken van een andere versie van de installatiekopie volledig te voltooien. 
>
> U kunt ook de versie van uw installatiekopie in opslaan [Zone-redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) door toe te voegen `-StorageAccountType Standard_ZRS` bij het maken van de versie van de installatiekopie.
>
