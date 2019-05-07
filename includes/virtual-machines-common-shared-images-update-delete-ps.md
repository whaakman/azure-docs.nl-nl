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
ms.openlocfilehash: 10b95a92f36ad6eb340ae864cbfd9fcbeac371a8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148735"
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
- Doelregio's
- Uitsluiting van de meest recente
- Einde van de levensduur van datum



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

