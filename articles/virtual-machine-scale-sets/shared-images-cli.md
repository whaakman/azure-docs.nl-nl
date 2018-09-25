---
title: Gebruik gedeelde VM-installatiekopieën te maken van een schaalset in Azure | Microsoft Docs
description: Leer hoe u de Azure CLI gebruiken voor het maken van gedeelde VM-installatiekopieën te gebruiken voor het implementeren van virtuele-machineschaalsets in Azure.
services: virtual-machine-scale-sets
documentationcenter: ''
author: axayjo
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 777f46a0bbe8deb4f8b207e099e4fd83d9fc8507
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047401"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-cli-20"></a>Maken en gebruiken van gedeelde-installatiekopieën voor virtuele-machineschaalsets met de Azure CLI 2.0

Wanneer u een schaalset maakt, geeft u een installatiekopie op die moet worden gebruikt wanneer de VM-exemplaren zijn geïmplementeerd. De galerie met installatiekopieën van gedeelde-service vereenvoudigt aangepaste installatiekopie voor uw organisatie te delen. Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor het opstarten van configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. De galerie met installatiekopieën gedeeld kunt u uw aangepaste VM-installatiekopieën delen met anderen in uw organisatie, binnen of tussen regio's binnen een AAD-tenant. Kies welke installatiekopieën die u wilt delen, welke regio's die u wilt maken ze beschikbaar zijn in, en die u wilt delen. U kunt meerdere galerieën kunt maken, zodat u kunt gedeelde afbeeldingen logisch groeperen. De galerie is een op het hoogste niveau resource die volledig op rollen gebaseerd toegangsbeheer (RBAC) biedt. Installatiekopieën kunnen worden bijgehouden, en u kunt kiezen voor het repliceren van elke versie van de installatiekopie naar een andere set Azure-regio's. De galerie werkt alleen met beheerde installatiekopieën. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Maken van een gedeelde Afbeeldingengalerie
> * De definitie van een gedeelde installatiekopie maken
> * De versie van een gedeelde installatiekopie maken
> * Een virtuele machine maken van een gedeelde installatiekopie
> * Een resources verwijderen

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


>[!NOTE]
> Dit artikel helpt bij het proces van het gebruik van een gegeneraliseerde beheerde installatiekopie. Het maken van een schaalset op basis van een gespecialiseerde VM-installatiekopie wordt niet ondersteund.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.46 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).


[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Een schaalset maken op basis van de aangepaste VM-installatiekopie
Maak een schaalset met [az vmss create](/cli/azure/vmss#az-vmss-create). In plaats van een platforminstallatiekopie, zoals *UbuntuLTS* of *CentOS*, geeft u de naam van uw aangepaste VM-installatiekopie op. In het volgende voorbeeld wordt een schaalset gemaakt met de naam *myScaleSet*, die gebruikmaakt van de aangepaste installatiekopie met de naam *myImage* uit de vorige stap:

```azurecli-interactive
az vmss create \
  -g myGalleryRG \
  -n myScaleSet \
  --image "/subscriptions/<subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.


[!INCLUDE [virtual-machines-common-gallery-list-cli](../../includes/virtual-machines-common-gallery-list-cli.md)]


## <a name="clean-up-resources"></a>Resources opschonen
Als u de schaalset en aanvullende resources wilt verwijderen, verwijdert u de resourcegroep en alle bijbehorende resources met [az group delete](/cli/azure/group#az_group_delete). De parameter `--no-wait` retourneert het besturingselement naar de prompt zonder te wachten totdat de bewerking is voltooid. De parameter `--yes` bevestigt dat u de resources wilt verwijderen, zonder een extra prompt om dit te doen.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt, kunt u [oplossen gedeelde afbeeldingsgalerieën](troubleshooting-shared-images.md).