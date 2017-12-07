---
title: Virtuele machine maken vanuit een beheerde-installatiekopie in Azure | Microsoft Docs
description: Een virtuele Windows-machine maken van een algemene beheerde installatiekopie met behulp van Azure PowerShell of de Azure-portal in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cynthn
ms.openlocfilehash: d8986c71fd0300af385750af898d620e6d3e1f24
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Een virtuele machine maken van een begeleide afbeelding

U kunt meerdere virtuele machines maken in een beheerde VM-installatiekopie met PowerShell of de Azure-portal. Een beheerde VM-installatiekopie bevat de benodigde gegevens voor het maken van een virtuele machine, met inbegrip van het besturingssysteem en gegevensschijven. De VHD's die gezamenlijk de afbeelding, met inbegrip van zowel de OS-schijven en eventuele gegevensschijven, als beheerde schijven zijn opgeslagen. 

U moet al hebben [gemaakt van een beheerde VM-installatiekopie](capture-image-resource.md) moet worden gebruikt voor het maken van de nieuwe virtuele machine. 

## <a name="use-the-portal"></a>Gebruik de portal

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **alle resources**. U kunt de resources door sorteren **Type** afbeeldingen gemakkelijk te vinden.
3. Selecteer de installatiekopie die u wilt gebruiken in de lijst. De installatiekopie van het **overzicht** pagina wordt geopend.
4. Klik op **+ maken VM** in het menu.
5. Geef de informatie van de virtuele machine op. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Na het voltooien klikt u op **OK**. U kunt de nieuwe virtuele machine maken in een bestaande groep voor Resrouce of kies **nieuw** om een nieuwe resourcegroep voor het opslaan van de virtuele machine te maken.
6. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 
7. Onder **instellingen**, zo nodig en klikt u op **OK**. 
8. Op de pagina overzicht ziet u de installatiekopienaam van uw die wordt vermeld voor **persoonlijke installatiekopie**. Klik op **Ok** implementatie van virtuele machine te starten.


## <a name="use-powershell"></a>PowerShell gebruiken

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u de nieuwste versies van de AzureRM.Compute en AzureRM.Network PowerShell-modules hebt. Open een PowerShell-prompt als beheerder en voer de volgende opdracht ze te installeren.

```azurepowershell-interactive
Install-Module AzureRM.Compute,AzureRM.Network
```
Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).



### <a name="collect-information-about-the-image"></a>Verzamelen van informatie over de installatiekopie

Er moet eerst algemene informatie over de installatiekopie van het verzamelen en maak een variabele voor de installatiekopie. In dit voorbeeld wordt een beheerde VM-installatiekopie met de naam **myImage** die zich in de **myResourceGroup** resourcegroep in de **West-Centraal VS** locatie. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maken van het vNet en het subnet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

Het subnet maken. In dit voorbeeld wordt een subnet met de naam **mySubnet** met het adresvoorvoegsel van **10.0.0.0/24**.  
   
```azurepowershell-interactive
$subnetName = "mySubnet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Maak het virtuele netwerk. In dit voorbeeld wordt een virtueel netwerk met de naam **myVnet** met het adresvoorvoegsel van **10.0.0.0/16**.  
   
```azurepowershell-interactive
$vnetName = "myVnet"
$vnet = New-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $singleSubnet
```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Een openbaar IP-adres en een netwerkinterface maken

Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

Een openbaar IP-adres maken. In dit voorbeeld wordt een openbaar IP-adres met de naam **myPip**. 
   
```azurepowershell-interactive
$ipName = "myPip"
$pip = New-AzureRmPublicIpAddress `
    -Name $ipName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Dynamic
```
       
Maken van de NIC. In dit voorbeeld wordt een NIC met de naam **myNic**. 
   
```azurepowershell-interactive
$nicName = "myNic"
$nic = New-AzureRmNetworkInterface `
    -Name $nicName `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id
```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken

Als u zich aanmelden bij uw virtuele machine met RDP, moet u een netwerkbeveiligingsregel (NSG) waarmee op poort 3389 van RDP-toegang hebben. 

In dit voorbeeld maakt u een NSG met de naam **myNsg** die een regel aangeroepen bevat **myRdpRule** waarmee RDP-verkeer via poort 3389. Zie voor meer informatie over Nsg [openen van poorten voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```azurepowershell-interactive
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Een variabele voor het virtuele netwerk maken

Een variabele voor het voltooide virtueel netwerk maken. 

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

### <a name="get-the-credentials-for-the-vm"></a>De referenties voor de virtuele machine ophalen

De volgende cmdlet wordt een venster waarin u een nieuwe gebruikersnaam en wachtwoord gebruiken als het lokale administrator-account voor het op afstand toegang tot de virtuele machine wordt invoeren geopend. 

```azurepowershell-interactive
$cred = Get-Credential
```

### <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Variabelen voor de VM-naam, computernaam en de grootte van de virtuele machine instellen

Variabelen voor de VM-naam en de naam van computer maken. Dit voorbeeld wordt de naam van de virtuele machine als **myVM** en de naam van de computer als **computer**.

```azurepowershell-interactive
$vmName = "myVM"
$computerName = "myComputer"
```

Stel de grootte van de virtuele machine. In dit voorbeeld maakt **Standard_DS1_v2** grootte van de virtuele machine. Zie de [VM-grootten](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentatie voor meer informatie.

```azurepowershell-interactive
$vmSize = "Standard_DS1_v2"
```

Het VM-naam en de grootte toevoegen aan de VM-configuratie.

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

### <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>De VM-installatiekopie als bronafbeelding voor de nieuwe virtuele machine instellen

Stel de broninstallatiekopie met de ID van de beheerde VM-installatiekopie.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

### <a name="set-the-os-configuration-and-add-the-nic"></a>Stel de configuratie van het besturingssysteem en voeg de NIC.

Voer het opslagtype (PremiumLRS of StandardLRS) en de grootte van de besturingssysteemschijf. In dit voorbeeld wordt het accounttype ingesteld op **PremiumLRS**, de schijfgrootte van de op **128 GB** en schijfcache naar **ReadWrite**.

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk -VM $vm  `
    -StorageAccountType PremiumLRS `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

### <a name="create-the-vm"></a>De virtuele machine maken

De nieuwe virtuele machine maken met de configuratie die we hebben gemaakt en opgeslagen in de **$vm** variabele.

```azurepowershell-interactive
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

### <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
Als u klaar is, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```azurepowershell-interactive
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell [maken en beheren van Windows-VM's met de Azure PowerShell-module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

