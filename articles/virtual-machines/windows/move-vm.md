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
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: ede2092be4e4eaf201e15307a7d9934ea267ae37
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55980700"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Een Windows-VM verplaatsen naar een andere Azure-abonnement of resourcegroep groep
In dit artikel leert u hoe u een Windows virtuele machine (VM) verplaatsen tussen resourcegroepen of abonnementen. Verplaatsen tussen abonnementen kan handig zijn als u een virtuele machine hebt gemaakt in een persoonlijke abonnement en wil verplaatsen naar het abonnement van uw bedrijf kunnen doorgaan met uw werk.

> [!IMPORTANT]
>Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsing. Nadat de virtuele machine is verplaatst, moet u de hulpprogramma's en scripts gebruikmaken van de nieuwe resource-id's bijwerken. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Powershell gebruiken voor het verplaatsen van een virtuele machine

Om een virtuele machine naar een andere resourcegroep verplaatsen, moet u ervoor zorgen dat u ook alle van de afhankelijke resources verplaatsen. Als u een lijst met de resource-ID van elk van deze resources, gebruikt de [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet.

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

U kunt de uitvoer van de vorige opdracht gebruiken als een door komma's gescheiden lijst van resource-id's [verplaatsen AzResource](https://docs.microsoft.com/powershell/module/az.resources/move-azresource) elke resource verplaatsen naar de bestemming. 

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Om de resources verplaatsen naar een ander abonnement, bevatten de **- DestinationSubscriptionId** parameter. 

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Wanneer u wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resources, voer **Y** om te bevestigen.

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../../resource-group-move-resources.md).    

