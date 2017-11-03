---
title: Azure CLI Sample Script - maken van een WindowsServer-machine | Microsoft Docs
description: Azure CLI Script voorbeeld - een WindowsServer-machine maken
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 037ea2c0d4c61c7d8ca3b058b5c41c2e296da96f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-with-the-azure-cli"></a>Een virtuele machine maken met de Azure CLI

Dit script maakt een Azure-virtuele Machine met Windows Server 2016. Nadat het script is uitgevoerd, kunt u de virtuele machine via Extern bureaublad-verbinding openen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

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
| [AZ netwerk openbare ip-maken](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [nsg voor AZ netwerk maken](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Maakt een netwerkbeveiligingsgroep (NSG), die een beveiligingsgrens tussen het internet en de virtuele machine. |
| [AZ netwerk nic maken](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Hiermee maakt u een virtueel netwerkkaart en gekoppeld aan het virtuele netwerk, subnet en NSG. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Met deze opdracht geeft ook de afbeelding van de virtuele machine moet worden gebruikt en de beheerdersreferenties.  |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [virtuele machine van Windows Azure-documentatie](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
