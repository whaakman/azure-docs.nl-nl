---
title: Verplaatsen van een virtuele machine van Windows-resource in Azure | Microsoft Docs
description: Een Windows-VM verplaatsen naar een andere Azure-abonnement of de resource-groep in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 1db25a5d9ff5cb6aa2787a0cafa40cfb010e3b06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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
Als een virtuele machine naar een andere resourcegroep verplaatsen, moet u ervoor zorgen dat u ook alle afhankelijke resources verplaatsen. Voor het gebruik van de cmdlet Move-AzureRMResource, moet u de naam van de bron en het type resource. U kunt beide van de cmdlet zoeken AzureRMResource ophalen.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


Voor het verplaatsen van een virtuele machine moet worden verplaatst van meerdere resources. We kunnen alleen afzonderlijke variabelen voor elke resource maken en deze vervolgens te vermelden. In dit voorbeeld bevat het merendeel van de algemene resources voor een virtuele machine, maar u kunt meer behoefte toevoegen.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

De om resources te verplaatsen naar een ander abonnement, omvatten de **- DestinationSubscriptionId** parameter. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources. Type **Y** om te bevestigen dat u wilt verplaatsen van de resources.

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../resource-group-move-resources.md).    

