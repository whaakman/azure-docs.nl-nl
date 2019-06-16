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
ms.openlocfilehash: d2a85f3947e9993e5d1853e45c6d03586a074cf6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66145764"
---
## <a name="update-resources"></a>Resources bijwerken

Er zijn enkele beperkingen met betrekking tot wat kan worden bijgewerkt. De volgende items kunnen worden bijgewerkt: 

Galerie met installatiekopieën van de gedeelde:
- Description

de definitie van de installatiekopie:
- Aanbevolen vcpu 's
- Aanbevolen geheugen
- Description
- Einde van de levensduur van datum

Installatiekopieversie:
- Aantal regionale replica 's
- Doelregio 's
- Uitsluiting van de meest recente
- Einde van de levensduur van datum

Als u van plan bent over het toevoegen van replica-regio's, een beheerde installatiekopie van de bron niet verwijderen. Een beheerde installatiekopie van de bron nodig is voor de versie van de installatiekopie die wordt gerepliceerd naar extra regio's. 

Gebruiken voor het bijwerken van de beschrijving van een galerie, [Update AzGallery](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery).

```azurepowershell-interactive
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

In dit voorbeeld ziet u hoe u [Update AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) om bij te werken van de datum einde van de levenscyclus voor de definitie van de installatiekopie.

```azurepowershell-interactive
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

In dit voorbeeld ziet u hoe u [Update AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) moeten worden uitgesloten van deze versie van de installatiekopie van wordt gebruikt als de *nieuwste* installatiekopie.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```


## <a name="clean-up-resources"></a>Resources opschonen

Bij het verwijderen van resources, moet u beginnen met het laatste item in de geneste resources - versie van de installatiekopie. Wanneer de versies zijn verwijderd, kunt u de definitie van de installatiekopie verwijderen. U kunt de galerie niet verwijderen totdat alle resources onder deze zijn verwijderd.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$gallery = "myGallery"
$imageDefinition = "myImageDefinition"
$imageVersion = "myImageVersion"

Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition `
   -GalleryName $gallery `
   -Name $imageVersion `
   -ResourceGroupName $resourceGroup

Remove-AzGalleryImageDefinition `
   -ResourceGroupName $resourceGroup `
   -GalleryName $gallery `
   -GalleryImageDefinitionName $imageDefinition

Remove-AzGallery `
   -Name $gallery `
   -ResourceGroupName $resourceGroup

Remove-AzResourceGroup -Name $resourceGroup
```

