---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8770aaeff3e0d7b2d6a39f596aafebf15ed48b23
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984989"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/powershell](https://shell.azure.com/powershell) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.


## <a name="preview-register-the-feature"></a>Preview: De functie registreren

Gedeelde Afbeeldingsgalerieën is in preview, maar u moet de functie registreren voordat u deze kunt gebruiken. De functie Gedeelde Afbeeldingsgalerieën registreren:

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>De installatiekopie van het beheerde ophalen

U ziet een lijst met installatiekopieën die beschikbaar in een resource-groep met zijn [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Zodra u weet dat de installatiekopie met de naam en de resourcegroep waarvan het zich bevindt, kunt u `Get-AzImage` opnieuw naar de installatiekopie-object ophalen en opslaan in een variabele voor later gebruik. In dit voorbeeld wordt een installatiekopie met de naam *myImage* uit de resourcegroep 'myResourceGroup' en wijst deze toe aan de variabele *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken 

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Galerie-namen moeten uniek zijn binnen uw abonnement. Maak een installatiekopie galerie met [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). Het volgende voorbeeld wordt een galerie met de naam *myGallery* in de *myGalleryRG* resourcegroep.

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

Maak de galerie installatiekopie definitie met [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). In dit voorbeeld wordt de afbeelding met de naam *myGalleryImage*.

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

In een toekomstige release zal het mogelijk gebruik van uw persoonlijke gedefinieerde **-uitgever**, **-bieden** en **- Sku** waarden om te zoeken en geef een definitie van een afbeelding, klikt u vervolgens een virtuele machine maken met behulp van meest recente versie van de installatiekopie van de definitie van de overeenkomende installatiekopie. Dit zijn bijvoorbeeld drie definities die installatiekopie en de bijbehorende waarden:

|Definitie van installatiekopie|Uitgever|Aanbieding|Sku|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Testen|standardOffer|testSku|

Alle drie deze hebben unieke sets waarden. In een toekomstige release kunt u zich aan deze waarden combineren om een aanvraag van de meest recente versie van een specifieke installatiekopie. 

```powershell
# The following should set the source image as myImage1 from the table above
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Dit is vergelijkbaar met hoe u deze voor de op dat moment kunt opgeven [Azure Marketplace-installatiekopieën](../articles/virtual-machines/windows/cli-ps-findimage.md) om een VM te maken. Met dat gegeven in gedachte moet elke definitie van de installatiekopie van een unieke set van deze waarden hebben. U kunt de versies van een installatiekopie die delen van één of twee, maar niet alle drie waarden hebben. 

##<a name="create-an-image-version"></a>De versie van een installatiekopie maken

De versie van een installatiekopie van het gebruik van een beheerde installatiekopie maken [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) . In dit voorbeeld de versie van de installatiekopie is *1.0.0* en deze worden gerepliceerd naar beide *West-Centraal VS* en *Zuid-centraal VS* datacenters.


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

