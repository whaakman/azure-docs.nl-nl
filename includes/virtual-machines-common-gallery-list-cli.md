---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f2e252a000c5633b2cc1ef34683d7d2c7c133c03
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047680"
---
## <a name="using-rbac-to-share-images"></a>Met het RBAC voor het delen van afbeeldingen

U kunt installatiekopieën delen tussen abonnementen met behulp van rollen gebaseerd toegangsbeheer (RBAC). Elke gebruiker die heeft voor de versie van een installatiekopie, zelfs tussen abonnementen leesmachtigingen, zich op een virtuele Machine met behulp van de versie van de installatiekopie implementeren.

Zie voor meer informatie over het delen van resources met behulp van RBAC [beheren van toegang via RBAC en Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Gegevens weergeven

De locatie, de status en andere informatie over de beschikbare afbeeldingsgalerieën met behulp van [az sig lijst](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

Lijst met de definities van de installatiekopie in een galerie, inclusief informatie over het type besturingssysteem en de status, met behulp van [lijst met de definitie van de installatiekopie van de az sig](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

De versies van de gedeelde installatiekopie in een galerie weergeven met behulp van [lijst met de versie van de installatiekopie van de az sig](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myGalleryImage -o table
```

Haal de ID van een installatiekopie versie via [az sig-versie van de installatiekopie show](/cli/azure/sig/image-version#az-sig-image-version-show).

```
az sig image-version show \
-g myGalleryRG \     
-r myGallery \     
-i myGalleryImage \     
--gallery-image-version-name 1.0.0 \     
--query "id"
```