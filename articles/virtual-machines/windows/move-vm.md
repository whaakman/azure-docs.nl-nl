---
title: Verplaatsen van een virtuele machine van Windows-resource in Azure | Microsoft Docs
description: Een Windows-VM verplaatsen naar een andere Azure-abonnement of de resource-groep in het Resource Manager-implementatiemodel.
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
ms.openlocfilehash: b98b8c947fb34b60c7bd27b006672e0e9d923d3b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30918146"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Een Windows VM verplaatsen naar een andere Azure-abonnement of resourcegroep groep
Dit artikel begeleidt u bij het verplaatsen van een virtuele machine van Windows tussen resourcegroepen of abonnementen. Verplaatsen tussen abonnementen kan handig zijn als u een virtuele machine hebt gemaakt in een persoonlijke abonnement en wilt verplaatsen naar een abonnement van uw bedrijf om na te gaan met uw werk.

> [!IMPORTANT]
>U kunt beheerde schijven op dit moment niet verplaatsen. 
>
>Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsing. Nadat de virtuele machine is verplaatst, moet u de hulpprogramma's en scripts die de nieuwe resource-ID bijwerken. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Powershell gebruiken voor het verplaatsen van een virtuele machine

Als een virtuele machine naar een andere resourcegroep verplaatsen, moet u ervoor zorgen dat u ook alle afhankelijke resources verplaatsen. De cmdlet Move-AzureRMResource gebruiken, moet u de ResourceId van elk van de resources. U kunt een lijst van het gebruik van de ResourceId ophalen de [zoeken AzureRMResource](/powershell/module/azurerm.resources/find-azurermresource) cmdlet.

```azurepowershell-interactive
 Find-AzureRMResource -ResourceGroupNameContains <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Voor het verplaatsen van een virtuele machine moet worden verplaatst van meerdere resources. We kunnen de uitvoer van zoeken AzureRMResource gebruiken voor het maken van een door komma's gescheiden lijst met de ResourceIds en die u wilt doorgeven [verplaatsen AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) om ze te verplaatsen naar de bestemming. 

```azurepowershell-interactive

Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
De om resources te verplaatsen naar een ander abonnement, omvatten de **- DestinationSubscriptionId** parameter. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources. 

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../resource-group-move-resources.md).    

