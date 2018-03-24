---
title: Azure CLI-voorbeeldscript - Filter VM-netwerkverkeer | Microsoft Docs
description: Azure CLI-script steekproef - binnenkomend en uitgaand netwerkverkeer voor VM filteren.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: bfc894ea718205ac77be48552f6cb5a076572395
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filteren van binnenkomende en uitgaande netwerkverkeer voor VM

Dit voorbeeldscript wordt een virtueel netwerk gemaakt met front-end en back-end-subnetten. Binnenkomend netwerkverkeer naar de front-end-subnet is beperkt tot HTTP, HTTPS en SSH, terwijl uitgaand verkeer naar internet van de back-end-subnet is niet toegestaan. Nadat het script is uitgevoerd, hebt u één virtuele machine met twee NIC's. Elke NIC is verbonden met een ander subnet.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/bash), of vanuit een lokale installatie van de Azure CLI. Als u de CLI lokaal gebruikt, wordt dit script vereist dat u versie 2.0.28 uitvoert of hoger. Ga voor de geïnstalleerde versie uitvoeren `az --version`. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, het virtuele netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de volgende tabel is gekoppeld aan de opdracht specifieke documentatie bij:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en de front-end-subnet. |
| [AZ netwerksubnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Maakt een back-end-subnet. |
| [AZ network vnet subnet bijwerken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) | Nsg's koppelt aan subnetten. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het internet. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Virtuele netwerkinterfaces maakt en gekoppeld aan het virtuele netwerk front-end en back-end-subnetten. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Netwerk maakt beveiligingsgroepen (NSG) die gekoppeld aan de front-end en back-end subnetten zijn. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |Maakt NSG-regels die bepaalde poorten tot specifieke subnetten blokkeren of toestaan. |
| [az vm create](/cli/azure/vm#az_vm_create) | Hiermee maakt u virtuele machines en een NIC koppelt aan elke virtuele machine. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine te gebruiken en de beheerdersreferenties. |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep en alle resources die deze bevat. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Extra virtuele netwerk CLI scriptvoorbeelden kunnen u vinden in [virtuele netwerk CLI voorbeelden](../cli-samples.md).
