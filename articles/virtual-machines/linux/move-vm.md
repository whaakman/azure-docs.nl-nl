---
title: Verplaatsen van een virtuele Linux-machine in Azure | Microsoft Docs
description: Een Linux-VM verplaatsen naar een andere Azure-abonnement of resourcegroep groep in het Resource Manager-implementatiemodel.
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
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: c451377d6274c50f22e3b1d4cd32fb0f3edd9d9e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220389"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Een Linux-VM verplaatsen naar een ander abonnement of resourcegroep groep
In dit artikel leert u hoe u een Linux virtuele machine (VM) verplaatsen tussen resourcegroepen of abonnementen. Een virtuele machine verplaatsen tussen abonnementen kan handig zijn als u een virtuele machine in een persoonlijke abonnement hebt gemaakt en nu wilt verplaatsen naar een abonnement van uw bedrijf.

> [!IMPORTANT]
>U kunt Azure Managed Disks op dit moment niet verplaatsen. 
>
>Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsing. Nadat de virtuele machine is verplaatst, moet u de hulpprogramma's en scripts gebruikmaken van de nieuwe resource-id's bijwerken. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>De Azure CLI gebruiken voor een virtuele machine verplaatsen


Voordat u uw virtuele machine verplaatsen kunt met behulp van de Azure CLI, moet u om te controleren of dat de bron- en -abonnementen bestaan binnen dezelfde tenant. Gebruik om te controleren dat beide abonnementen de dezelfde tenant-ID hebben, [az account show](/cli/azure/account#az_account_show).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Als de tenant-id's voor de bron- en -abonnementen niet hetzelfde zijn zijn, moet u contact opnemen [ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) de resources verplaatsen naar een nieuwe tenant.

Als u wilt een virtuele machine is verplaatst, moet u de virtuele machine en alle ondersteunende resources verplaatsen. Gebruik de [az resource list](/cli/azure/resource#az_resource_list) opdracht om alle resources in een resourcegroep en de id's weer te geven. Zo kunt u de uitvoer van deze opdracht doorsluizen naar een bestand, zodat u kunt kopiëren en plakken van de id's in latere opdrachten.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Gebruiken om een virtuele machine en de daarbij behorende bronnen naar een andere resourcegroep verplaatsen, [verplaatsen van de az resource](/cli/azure/resource#az_resource_move). Het volgende voorbeeld ziet hoe u een virtuele machine en de meest voorkomende bronnen hiervoor te verplaatsen. Gebruik de **-id's** parameter en geeft u een lijst met door komma's gescheiden (zonder spaties) met id's voor de resources te verplaatsen.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Als u verplaatsen van de virtuele machine en de daarbij behorende bronnen naar een ander abonnement wilt, voegt u toe de **--bestemming-subscriptionId** parameter opgeven voor het doelabonnement.

Wanneer u wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resources, voer **Y** om te bevestigen.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../../resource-group-move-resources.md).    

