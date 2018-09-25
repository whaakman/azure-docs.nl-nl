---
title: Gedeelde VM-installatiekopieën maken met de Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u de Azure CLI gebruiken om te maken van een gedeelde installatiekopie van een virtuele machine in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: axayjo
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 2f8ddae05b97b3fa6f1d3f65681defdd4e5a38b7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047689"
---
# <a name="preview-create-a-shared-image-gallery-with-the-azure-cli"></a>Voorbeeld: Een gedeelde afbeeldingengalerie maken met de Azure CLI

De galerie met installatiekopieën van gedeelde vereenvoudigt aangepaste installatiekopie voor uw organisatie te delen. Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor het opstarten van configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. De galerie met installatiekopieën gedeeld kunt u uw aangepaste VM-installatiekopieën delen met anderen in uw organisatie, binnen of tussen regio's binnen een AAD-tenant. Kies welke installatiekopieën die u wilt delen, welke regio's die u wilt maken ze beschikbaar zijn in, en die u wilt delen. U kunt meerdere galerieën kunt maken, zodat u kunt gedeelde afbeeldingen logisch groeperen. De galerie is een op het hoogste niveau resource die volledig op rollen gebaseerd toegangsbeheer (RBAC) biedt. Installatiekopieën kunnen worden bijgehouden, en u kunt kiezen voor het repliceren van elke versie van de installatiekopie naar een andere set Azure-regio's. De galerie werkt alleen met beheerde installatiekopieën.

In dit artikel maakt u uw eigen galerie en aangepaste installatiekopieën van een virtuele machine van Azure. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maken van een gedeelde Afbeeldingengalerie
> * De definitie van een gedeelde installatiekopie maken
> * De versie van een gedeelde installatiekopie maken
> * Een virtuele machine maken van een gedeelde installatiekopie
> * Een resources verwijderen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.46 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine maken van de installatiekopie versie met [az vm maken](/cli/azure/vm#az-vm-create).

```azurecli-interactive 
az vm create\
   -g myGalleryRG \
   -n myVM \
   --image "/subscriptions/<subscription-ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
   --generate-ssh-keys
```

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over gedeelde installatiekopie galerieën, de [overzicht](shared-image-galleries.md).
