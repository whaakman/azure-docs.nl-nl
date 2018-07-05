---
title: Verplaatsen van een Windows VM-resource in Azure | Microsoft Docs
description: Een Windows-VM verplaatsen naar een andere Azure-abonnement of resourcegroep groep in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: cynthn
ms.openlocfilehash: 168ba57399b2649af29820f7321dd0151618346e
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436477"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Een Windows-VM verplaatsen naar een andere Azure-abonnement of resourcegroep groep
In dit artikel leert u hoe u een Windows-VM verplaatsen tussen resourcegroepen of abonnementen. Verplaatsen tussen abonnementen kan handig zijn als u een virtuele machine hebt gemaakt in een persoonlijke abonnement en wil verplaatsen naar het abonnement van uw bedrijf kunnen doorgaan met uw werk.

> [!IMPORTANT]
>U kunt beheerde schijven op dit moment niet verplaatsen. 
>
>Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsing. Zodra de virtuele machine is verplaatst, moet u de hulpprogramma's en scripts gebruikmaken van de nieuwe resource-id's bijwerken. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Powershell gebruiken voor het verplaatsen van een virtuele machine

Om een virtuele machine naar een andere resourcegroep verplaatsen, moet u ervoor zorgen dat u ook alle van de afhankelijke resources verplaatsen. Voor het gebruik van de cmdlet Move-AzureRMResource, moet u de ResourceId van elk van de resources. Krijgt u een lijst van de ResourceId met behulp van de [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet.

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

We moeten voor het verplaatsen van een virtuele machine om meerdere resources te verplaatsen. We kunnen de uitvoer van Get-AzureRMResource gebruiken voor het maken van een door komma's gescheiden lijst met de ResourceIds en dat doorgeven aan [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) om ze te verplaatsen naar de bestemming. 

```azurepowershell-interactive

Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Om de resources verplaatsen naar een ander abonnement, bevatten de **- DestinationSubscriptionId** parameter. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources. 

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../resource-group-move-resources.md).    

