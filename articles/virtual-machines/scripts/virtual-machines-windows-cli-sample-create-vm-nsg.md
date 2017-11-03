---
title: 'Azure CLI-Script voorbeeld: twee virtuele machines maken met een interne en externe NSG | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: twee virtuele machines maken met de interne en externe NSG'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 84092e3a70f1bfde923ba3395fbc0a46c11e233e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Beveiligen van netwerkverkeer tussen virtuele machines

Dit script maakt twee virtuele machines en binnenkomend verkeer op beide beveiligt. Een virtuele machine op het internet toegankelijk is en is een netwerkbeveiligingsgroep (NSG) is geconfigureerd voor verkeer op poort 3389 en 80. De tweede virtuele machine is niet toegankelijk op het internet en is een NSG is geconfigureerd om alleen het verkeer van de eerste virtuele machine. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en subnet. |
| [AZ network vnet subnet maken](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Maakt een subnet. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Met deze opdracht geeft ook de afbeelding van de virtuele machine moet worden gebruikt en de beheerdersreferenties.  |
| [AZ netwerk nsg regel update](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_update) | Een regel voor het NSG-updates. In dit voorbeeld wordt wordt de back-end-regel bijgewerkt verkeer doorlaten alleen via het front-end-subnet. |
| [lijst van de regel met het nsg van netwerken AZ](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_list) | Retourneert informatie over een groep voor de netwerkbeveiligingsregel. In dit voorbeeld wordt de naam van de opgeslagen in een variabele voor gebruik verderop in het script. |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [virtuele machine van Windows Azure-documentatie](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
