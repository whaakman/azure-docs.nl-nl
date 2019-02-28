---
title: Een virtuele machine maken met een statisch privé IP-adres - Azure PowerShell | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een privé IP-adres met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2019
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 536ca8895d130fd9273d556332830d020a485dc3
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958283"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>Een virtuele machine maken met een statisch privé IP-adres met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt een virtuele machine (VM) maken met een statisch privé IP-adres. Een statisch privé IP-adres, in plaats van een dynamisch adres toewijzen als u wilt selecteren welk adres van een subnet is toegewezen aan een virtuele machine. Meer informatie over [statische privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#allocation-method). Een privé IP-adres toegewezen aan een bestaande virtuele machine van dynamisch in statisch of om te werken met openbare IP-adressen, Zie [toevoegen, wijzigen of verwijderen-IP-adressen](virtual-network-network-interface-addresses.md).

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

U kunt de volgende stappen uitvoeren vanaf uw lokale computer of met behulp van de Azure Cloud Shell. Voor het gebruik van uw lokale computer, zorg ervoor dat u hebt de [Azure PowerShell is geïnstalleerd](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Selecteer voor het gebruik van de Azure Cloud Shell, **uitproberen** in de rechterbovenhoek van een willekeurige opdracht box die volgt. De Cloud Shell meldt u zich bij Azure.

1. Als de Cloud Shell gebruikt, gaat u verder met stap 2. Open een sessie en meld u in Azure met `Connect-AzAccount`.
2. Maak een resourcegroep met de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een resourcegroep gemaakt in de regio Oost VS Azure:

   ```azurepowershell-interactive
   $RgName = "myResourceGroup"
   $Location = "eastus"
   New-AzResourceGroup -Name $RgName -Location $Location
   ```

3. Maak een subnetconfiguratie en een virtueel netwerk met de [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) en [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) opdrachten:

   ```azurepowershell-interactive
   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object for use in a later step.
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

4. Een netwerkinterface maken in het virtuele netwerk en een privé IP-adres van het subnet toewijzen aan de netwerkinterface met de [New-AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) en [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) opdrachten:

   ```azurepowershell-interactive
   $IpConfigName1 = "IPConfig-1"
   $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

   $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
   ```

5. Maak een VM-configuratie met [New-AzVMConfig](/powershell/module/Az.Compute/New-AzVMConfig), en maak vervolgens de virtuele machine met [New-AzVM](/powershell/module/az.Compute/New-azVM). Geef desgevraagd een gebruikersnaam en wachtwoord moet worden gebruikt als de aanmelding referenties voor de virtuele machine:

   ```azurepowershell-interactive
   $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
   $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
   $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
   $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
   New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
   ```

> [!WARNING]
> Hoewel u privé IP-adresinstellingen aan het besturingssysteem toevoegen kunt, wordt aangeraden niet in dat geval pas na het lezen [een privé IP-adres toevoegen aan een besturingssysteem](virtual-network-network-interface-addresses.md#private).
>
>
<a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>

> [!IMPORTANT]
> Voor toegang tot de virtuele machine via internet, moet u een openbaar IP-adres toewijzen aan de virtuele machine. U kunt ook een dynamisch privé IP-adrestoewijzing aan een statische toewijzing wijzigen. Zie voor meer informatie, [toevoegen of wijzigen van IP-adressen](virtual-network-network-interface-addresses.md). Het bovendien raadzaam het netwerkverkeer te beperken tot de virtuele machine te koppelen van een netwerkbeveiligingsgroep naar de netwerkinterface, het subnet dat u hebt gemaakt met de netwerkinterface in of beide. Zie voor meer informatie, [netwerkbeveiligingsgroepen beheren](manage-network-security-group.md).
>
>

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, kunt u [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) en het toewijzen van een [statisch privé IP-adres](virtual-network-network-interface-addresses.md#add-ip-addresses) met een Azure-machine.
- Meer informatie over het maken van [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machines.
