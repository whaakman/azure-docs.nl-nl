---
title: Een in zones ingedeelde virtuele Windows-machine maken - Azure PowerShell | Microsoft Docs
description: Een virtuele Windows-machine maken in een beschikbaarheidszone met Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: e86fcb4dbf170e5bc07553165e09d6fc3d3cf283
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-powershell"></a>Een virtuele Windows-machine maken in een beschikbaarheidszone met PowerShell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruikt om in een Azure-beschikbaarheidszone een virtuele Azure-machine te maken waarop Windows Server 2016 wordt uitgevoerd. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Zorg ervoor dat u de nieuwste Azure PowerShell-module hebt geïnstalleerd. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In dit voorbeeld wordt er een resourcegroep met de naam *MyResourceGroup* gemaakt in de *eastus2*. VS Oost 2 is een van de Azure-regio's die ondersteuning biedt voor beschikbaarheidszones in preview.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus2
```

## <a name="create-networking-resources"></a>Netwerkresources maken

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Een virtueel netwerk, subnet en openbaar IP-adres maken 
Deze resources worden gebruikt voor netwerkconnectiviteit met de virtuele machine en om verbinding met internet te maken. Maak het IP-adres in een beschikbaarheidszone, *2* in dit voorbeeld. Als u de virtuele machine wilt maken in een beschikbaarheidszone (weergegeven in een latere stap), moet u dezelfde zone opgeven die is gebruikt voor het maken van het IP-adres.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus2 `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address in an availability zone and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus2 -Zone 2 `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Een netwerkbeveiligingsgroep en een regel voor de netwerkbeveiligingsgroep maken 
De netwerkbeveiligingsgroep beveiligt de virtuele machine met binnenkomende en uitgaande regels. In dit geval is er een binnenkomende regel gemaakt voor poort 3389, waarmee binnenkomende verbindingen met een extern bureaublad worden toegestaan. We willen ook een regel maken voor poort 80, de poort voor binnenkomend webverkeer.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus2 `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Een netwerkkaart voor de virtuele machine maken 
Maak een netwerkkaart met [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) voor de virtuele machine. De netwerkkaart verbindt de virtuele machine met een subnet, netwerkbeveiligingsgroep en openbaar IP-adres.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus2 `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele-machineconfiguratie. Deze configuratie bevat de instellingen die worden gebruikt bij het implementeren van de virtuele machine, zoals een installatiekopie van de virtuele machine, de grootte en de verificatieconfiguratie. De grootte *Standard_DS1_v2* in dit voorbeeld wordt ondersteund in de preview van beschikbaarheidszones. In deze configuratie wordt ook de beschikbaarheidszone opgegeven die u hebt ingesteld bij het maken van het IP-adres. Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS1_v2 -Zone 2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus2 -VM $vmConfig
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zone voor IP-adres en beheerde schijf

U hebt de resource van het IP-adres van de virtuele machine gemaakt in dezelfde beschikbaarheidszone als de virtuele machine. De resource van de beheerde schijf voor de virtuele machine wordt ook in dezelfde beschikbaarheidszone gemaakt. U kunt dit controleren met [Get-AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk):

```powershell
Get-AzureRmDisk -ResourceGroupName myResourceGroup
```

In de uitvoer ziet u dat de beheerde schijf zich in dezelfde beschikbaarheidszone bevindt als de virtuele machine:

```powershell
ResourceGroupName  : myResourceGroup
AccountType        : PremiumLRS
OwnerId            : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
ManagedBy          : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              : {2}
TimeCreated        : 9/7/2017 6:57:26 PM
OsType             : Windows
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 127
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/disks/myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Name               : myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Type               : Microsoft.Compute/disks
Location           : eastus2
Tags               : {}
```




## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een virtuele machine maakt in een beschikbaarheidszone. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.