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
ms.openlocfilehash: b34d37fa79ccb8344fdacd99877403d61ba5f5c2
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138921"
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

De beschrijving van het gebruik van een galerie van update ([az sig update](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-update). 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


Bijwerken van de beschrijving van een installatiekopie definitie met [az sig installatiekopie-definitie-update](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update).

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

Bijwerken van de versie van een installatiekopie om toe te voegen een regio worden gerepliceerd naar de met behulp van [az sig image-versie bijwerken](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update). Deze wijziging duurt even als de afbeelding worden gerepliceerd naar de nieuwe regio.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

## <a name="delete-resources"></a>Resources verwijderen

U hebt om resources te verwijderen in omgekeerde volgorde door de versie van de installatiekopie eerst verwijderen. Nadat u alle van de versies van een installatiekopie verwijderen, kunt u de definitie van de installatiekopie verwijderen. Nadat u alle definities van de installatiekopie hebt verwijderd, kunt u de galerie verwijderen. 

Verwijderen van een installatiekopie versie via [az sig-versie van installatiekopie verwijderen](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-delete).

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

Verwijderen van een installatiekopie definitie met [az sig-definitie van installatiekopie verwijderen](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-delete).

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


Verwijderen van een installatiekopie galerie met [az sig verwijderen](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-delete).

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```