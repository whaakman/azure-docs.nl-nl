---
title: Verplaatsen van een virtuele Linux-machine in Azure | Microsoft Docs
description: Linux-VM verplaatsen naar een andere Azure-abonnement of de resource-groep in het Resource Manager-implementatiemodel.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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
Als u wilt een virtuele machine is verplaatst, moet u de virtuele machine en de ondersteunende resources verplaatsen. Gebruik de **azure-groep weergeven** opdracht om een lijst van alle resources in een resourcegroep en de id's. Het nuttig om de uitvoer van deze opdracht doorsluizen naar een bestand, zodat u kunt kopiëren en plakken van de id's in latere opdrachten.

    azure group show <resourceGroupName>

U kunt een virtuele machine en de bijbehorende resources verplaatsen naar een andere resourcegroep met de **azure-resource verplaatsen** CLI-opdracht. Het volgende voorbeeld laat zien hoe verplaatsen van een virtuele machine en de meest voorkomende bronnen die is vereist. We gebruiken de **-i** parameter en geeft u een door komma's gescheiden lijst (zonder spaties) met id's voor de resources te verplaatsen.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Als u wilt dat de virtuele machine en de bijbehorende resources verplaatsen naar een ander abonnement, voegt u de **--bestemming subscriptionId &#60; destinationSubscriptionID &#62;** parameter om het doelabonnement.

Als u vanaf de opdrachtprompt op een Windows-computer werkt, moet u toevoegen een  **$**  voor de namen van variabelen wanneer u ze declareren. Dit is niet nodig in Linux.

U wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resource. Type **Y** om te bevestigen dat u wilt verplaatsen van de resources.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../resource-group-move-resources.md).    

