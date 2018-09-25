---
title: Een virtuele machine maken met een statisch openbaar IP-adres - Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statisch openbaar IP-adres met behulp van de Azure-opdrachtregelinterface (CLI).
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: jdial
ms.openlocfilehash: 974c984cda2dd2198d09fb0dd004e640727b8c48
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982644"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-cli"></a>Een virtuele machine maken met een statisch openbaar IP-adres met de Azure CLI

U kunt een virtuele machine maken met een statisch openbaar IP-adres. Een openbaar IP-adres kunt u communiceren met een virtuele machine via internet. Toewijzen van een statisch openbaar IP-adres, in plaats van een dynamisch adres, om ervoor te zorgen dat het adres nooit verandert. Meer informatie over [statische openbare IP-adressen](virtual-network-ip-addresses-overview-arm.md#allocation-method). Een openbaar IP-adres toegewezen aan een bestaande virtuele machine van dynamisch in statisch of om te werken met particuliere IP-adressen, Zie [toevoegen, wijzigen of verwijderen-IP-adressen](virtual-network-network-interface-addresses.md). Openbare IP-adressen hebben een [nominale kosten in rekening gebracht](https://azure.microsoft.com/pricing/details/ip-addresses), en er is een [limiet](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) aan het aantal openbare IP-adressen die u per abonnement kunt.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

U kunt de volgende stappen uitvoeren vanaf uw lokale computer of met behulp van de Azure Cloud Shell. Voor het gebruik van uw lokale computer, zorg ervoor dat u hebt de [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Selecteer voor het gebruik van de Azure Cloud Shell, **uitproberen** in de rechterbovenhoek van een willekeurige opdracht box die volgt. De Cloud Shell meldt u zich bij Azure.

1. Als de Cloud Shell gebruikt, gaat u verder met stap 2. Open een sessie en meld u in Azure met `az login`.
2. Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az-group-create). Het volgende voorbeeld wordt een resourcegroep gemaakt in de regio Oost VS Azure:

   ```azurecli-interactive
   az group create --name myResourceGroup --location eastus
   ```

3. Maak een virtuele machine met de opdracht [az vm create](/cli/azure/vm#az-vm-create). De `--public-ip-address-allocation=static` optie wordt een statisch openbaar IP-adres toegewezen aan de virtuele machine. Het volgende voorbeeld wordt een virtuele Ubuntu-machine met een statisch, basic SKU openbaar IP-adres met de naam *myPublicIpAddress*:

   ```azurecli-interactive
   az vm create \
     --resource-group myResourceGroup \
     --name myVM \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys \
     --public-ip-address myPublicIpAddress \
     --public-ip-address-allocation static
   ```

   Als het openbare IP-adres een standaard-SKU moet, voegt u toe `--public-ip-sku Standard` naar de vorige opdracht. Meer informatie over [openbaar IP-adres SKU's](virtual-network-ip-addresses-overview-arm.md#sku). Als de virtuele machine wordt toegevoegd aan de groep back-end van een openbare Azure Load Balancer, de SKU van het openbare IP-adres van de virtuele machine, moet overeenkomen met de SKU van het openbare IP-adres van de load balancer. Zie voor meer informatie, [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

4. Het openbare IP-adres dat is toegewezen en Bevestig dat het als een statisch, basic SKU-adres is gemaakt met [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show):

   ```azurecli-interactive
   az network public-ip show \
     --resource-group myResourceGroup \
     --name myPublicIpAddress \
     --query [ipAddress,publicIpAllocationMethod,sku] \
     --output table
   ```

   Azure toegewezen openbaar IP-adres van de adressen die worden gebruikt in de regio die u hebt gemaakt met de virtuele machine in. U kunt de lijst met bereiken (voorvoegsels) downloaden voor de Azure-clouds [Openbaar](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) en [Duitsland](https://www.microsoft.com/download/details.aspx?id=57064).

> [!WARNING]
De instellingen voor het IP-adres binnen het besturingssysteem van de virtuele machine mag niet worden gewijzigd. Het besturingssysteem is geen informatie over Azure openbare IP-adressen. Hoewel u privé IP-adresinstellingen aan het besturingssysteem toevoegen kunt, wordt aangeraden niet te doen, tenzij die nodig zijn, en niet pas na het lezen [een privé IP-adres toevoegen aan een besturingssysteem](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, als deze niet meer nodig zijn:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [openbare IP-adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) in Azure
- Meer informatie over alle [openbare IP-adresinstellingen](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Meer informatie over [privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) en het toewijzen van een [statisch privé IP-adres](virtual-network-network-interface-addresses.md#add-ip-addresses) met een Azure-machine
- Meer informatie over het maken van [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machines
