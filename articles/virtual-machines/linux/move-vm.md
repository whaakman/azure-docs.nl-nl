---
title: Verplaatsen van een virtuele Linux-machine in Azure | Microsoft Docs
description: Linux-VM verplaatsen naar een andere Azure-abonnement of de resource-groep in het Resource Manager-implementatiemodel.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: a4a7dd5541fe298675232ffa803f749e71f6a03f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907489"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Linux-VM verplaatsen naar een ander abonnement of de resource-groep
In dit artikel leert u hoe u een Linux-VM verplaatsen tussen resourcegroepen of abonnementen. Een virtuele machine verplaatsen tussen abonnementen kan handig zijn als u een virtuele machine in een persoonlijke abonnement hebt gemaakt en wilt verplaatsen naar een abonnement van uw bedrijf.

> [!IMPORTANT]
>U kunt beheerde schijven op dit moment niet verplaatsen. 
>
>Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsing. Nadat de virtuele machine is verplaatst, moet u de hulpprogramma's en scripts die de nieuwe resource-ID bijwerken. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>De Azure CLI gebruiken om te verplaatsen van een virtuele machine


Voordat u uw virtuele machine met behulp van de CLI verplaatsen kunt, moet u controleren of dat de bron- en doelserver abonnementen bestaan binnen dezelfde tenant. Gebruiken om te controleren dat beide abonnementen de dezelfde tenant-ID hebben, [az account weergeven](/cli/azure/account#az_account_show).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Als de tenant-id's voor de bron- en doelserver abonnementen niet hetzelfde zijn zijn, moet u contact opnemen [ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) de resources verplaatsen naar een nieuwe tenant.

Als u wilt een virtuele machine is verplaatst, moet u de virtuele machine en de ondersteunende resources verplaatsen. Gebruik de [az resourcelijst](/cli/azure/resource#az_resource_list) opdracht om een lijst van alle resources in een resourcegroep en de id's. Het nuttig om de uitvoer van deze opdracht doorsluizen naar een bestand, zodat u kunt kopiëren en plakken van de id's in latere opdrachten.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

U kunt een virtuele machine en de bijbehorende resources verplaatsen naar een andere resourcegroep met [az resource verplaatsen](/cli/azure/resource#az_resource_move). Het volgende voorbeeld laat zien hoe verplaatsen van een virtuele machine en de meest voorkomende bronnen die is vereist. Gebruik de **-id's** parameter en geeft u een door komma's gescheiden lijst (zonder spaties) met id's voor de resources te verplaatsen.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageacountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Als u wilt dat de virtuele machine en de bijbehorende resources verplaatsen naar een ander abonnement, voegt u de **--bestemming subscriptionId** parameter om het doelabonnement.

Als u wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resource. Type **Y** om te bevestigen dat u wilt verplaatsen van de resources.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../resource-group-move-resources.md).    

