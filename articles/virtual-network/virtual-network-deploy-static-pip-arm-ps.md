---
title: Een virtuele machine maken met een statisch openbaar IP-adres - PowerShell | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statisch openbaar IP-adres met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: jdial
ms.openlocfilehash: 422a48a3671974248e9e3258be16d3537713f762
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58089995"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>Een virtuele machine maken met een statisch openbaar IP-adres met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt een virtuele machine maken met een statisch openbaar IP-adres. Een openbaar IP-adres kunt u communiceren met een virtuele machine via internet. Toewijzen van een statisch openbaar IP-adres, in plaats van een dynamisch adres, om ervoor te zorgen dat het adres nooit verandert. Meer informatie over [statische openbare IP-adressen](virtual-network-ip-addresses-overview-arm.md#allocation-method). Een openbaar IP-adres toegewezen aan een bestaande virtuele machine van dynamisch in statisch of om te werken met particuliere IP-adressen, Zie [toevoegen, wijzigen of verwijderen-IP-adressen](virtual-network-network-interface-addresses.md). Openbare IP-adressen hebben een [nominale kosten in rekening gebracht](https://azure.microsoft.com/pricing/details/ip-addresses), en er is een [limiet](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) aan het aantal openbare IP-adressen die u per abonnement kunt.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

U kunt de volgende stappen uitvoeren vanaf uw lokale computer of met behulp van de Azure Cloud Shell. Voor het gebruik van uw lokale computer, zorg ervoor dat u hebt de [Azure PowerShell is geïnstalleerd](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Selecteer voor het gebruik van de Azure Cloud Shell, **uitproberen** in de rechterbovenhoek van een willekeurige opdracht box die volgt. De Cloud Shell meldt u zich bij Azure.

1. Als de Cloud Shell gebruikt, gaat u verder met stap 2. Open een sessie en meld u in Azure met `Connect-AzAccount`.
2. Maak een resourcegroep met de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een resourcegroep gemaakt in de regio Oost VS Azure:

   ```azurepowershell-interactive
   New-AzResourceGroup -Name myResourceGroup -Location EastUS
   ```

3. Maak een virtuele machine met de [New-AzVM](/powershell/module/az.Compute/New-azVM) opdracht. De `-AllocationMethod "Static"` optie wordt een statisch openbaar IP-adres toegewezen aan de virtuele machine. Het volgende voorbeeld wordt een Windows Server-machine met een statisch, basic SKU openbaar IP-adres met de naam *myPublicIpAddress*. Geef desgevraagd een gebruikersnaam en wachtwoord moet worden gebruikt als de aanmelding referenties voor de virtuele machine:

   ```azurepowershell-interactive
   New-AzVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
   ```

   Als het openbare IP-adres een standaard-SKU moet, hebt u [een openbaar IP-adres maken](virtual-network-public-ip-address.md#create-a-public-ip-address), [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface), [het openbare IP-adres toewijzen aan de netwerkinterface](virtual-network-network-interface-addresses.md#add-ip-addresses), en vervolgens [een virtuele machine maken met de netwerkinterface](virtual-network-network-interface-vm.md#add-existing-network-interfaces-to-a-new-vm)in afzonderlijke stappen. Meer informatie over [openbaar IP-adres SKU's](virtual-network-ip-addresses-overview-arm.md#sku). Als de virtuele machine wordt toegevoegd aan de groep back-end van een openbare Azure Load Balancer, de SKU van het openbare IP-adres van de virtuele machine, moet overeenkomen met de SKU van het openbare IP-adres van de load balancer. Zie voor meer informatie, [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

4. Het openbare IP-adres dat is toegewezen en Bevestig dat het als een statisch adres is gemaakt met [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress):

   ```azurepowershell-interactive
   Get-AzPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
   ```

   Azure toegewezen openbaar IP-adres van de adressen die worden gebruikt in de regio die u hebt gemaakt met de virtuele machine in. U kunt de lijst met bereiken (voorvoegsels) downloaden voor de Azure-clouds [Openbaar](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) en [Duitsland](https://www.microsoft.com/download/details.aspx?id=57064).

> [!WARNING]
> De instellingen voor het IP-adres binnen het besturingssysteem van de virtuele machine mag niet worden gewijzigd. Het besturingssysteem is geen informatie over Azure openbare IP-adressen. Hoewel u privé IP-adresinstellingen aan het besturingssysteem toevoegen kunt, wordt aangeraden niet te doen, tenzij die nodig zijn, en niet pas na het lezen [een privé IP-adres toevoegen aan een besturingssysteem](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, kunt u [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [openbare IP-adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) in Azure
- Meer informatie over alle [openbare IP-adresinstellingen](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Meer informatie over [privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) en het toewijzen van een [statisch privé IP-adres](virtual-network-network-interface-addresses.md#add-ip-addresses) met een Azure-machine
- Meer informatie over het maken van [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machines
