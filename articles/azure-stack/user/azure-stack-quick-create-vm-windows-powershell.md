---
title: Een virtuele Windows-machine maken met behulp van PowerShell in Azure-Stack | Microsoft Docs
description: Een virtuele Windows-machine maken met PowerShell in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 7CA6C0AC-23B7-4007-BA32-7A950FD1F3B8
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/20/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 9f5752a969ff6a191ec60e175494316aea4abcaf
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-powershell-in-azure-stack"></a>Snelstartgids: een virtuele machine van Windows Server maken met behulp van PowerShell in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Windows Server 2016 virtuele machine maken met behulp van PowerShell voor Azure-Stack. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine. In dit artikel hebt u ook de stappen voor het:

* Verbinding maken met de virtuele machine met een externe client.
* De IIS-webserver installeren en de standaardstartpagina weergeven.
* Opschonen van uw resources.

>[!NOTE]
 U kunt de stappen in dit artikel vanuit de Azure-Stack Development Kit of vanuit een externe Windows-client als u via een VPN verbonden bent uitvoeren.

## <a name="prerequisites"></a>Vereisten

* Zorg ervoor dat uw Azure-Stack-operator de installatiekopie van het 'WindowsServer 2016' is toegevoegd aan de Stack van Azure marketplace.

* Azure-Stack is vereist voor een specifieke versie van Azure PowerShell maken en beheren van de resources. Als er geen PowerShell voor Azure-Stack is geconfigureerd, volgt u de stappen voor het [installeren](azure-stack-powershell-install.md) en [configureren](azure-stack-powershell-configure-user.md) PowerShell.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container in welke Azure-Stack resources worden geïmplementeerd en beheerd. Uitvoeren vanuit uw development kit of het systeem Azure Stack geïntegreerd, het volgende codeblok voor het maken van een resourcegroep. Waarden voor de variabelen in dit document zijn toegewezen, kunt u deze waarden gebruiken of nieuwe waarden toewijzen.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Storage-resources maken

Maak een opslagaccount en een opslagcontainer voor het opslaan van de installatiekopie van het Windows Server 2016.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob
```

## <a name="create-networking-resources"></a>Netwerkresources maken

Maak een virtueel netwerk, subnet en een openbaar IP-adres. Deze resources worden gebruikt voor verbinding met het netwerk aan de virtuele machine.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Een netwerkbeveiligingsgroep en een regel voor de netwerkbeveiligingsgroep maken

De netwerkbeveiligingsgroep beveiligt de virtuele machine met behulp van regels voor binnenkomend en uitgaand. U kunt een inkomende regel voor poort 3389 voor het toestaan van binnenkomende verbindingen van extern bureaublad en een inkomende regel voor poort 80 om toe te staan inkomend webverkeer maken.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleRDP `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleWWW `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Een netwerkkaart voor de virtuele machine maken

De netwerkkaart verbindt de virtuele machine met een subnet, netwerkbeveiligingsgroep en openbaar IP-adres.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een virtuele-machineconfiguratie. Deze configuratie bevat de instellingen die zijn gebruikt bij het implementeren van de virtuele machine. Bijvoorbeeld: referenties, grootte en de installatiekopie van de virtuele machine.

```powershell
# Define a credential object to store the username and password for the virtual machine
$UserName='demouser'
$Password='Password@123'| ConvertTo-SecureString -Force -AsPlainText
$Credential=New-Object PSCredential($UserName,$Password)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_A1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Windows `
  -ComputerName "MainComputer" `
  -Credential $Credential

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "MicrosoftWindowsServer" `
  -Offer "WindowsServer" `
  -Skus "2016-Datacenter" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Op afstand in de virtuele machine die u in de vorige stap hebt gemaakt, moet u het openbare IP-adres. Voer de volgende opdracht om op te halen van het openbare IP-adres van de virtuele machine:

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een extern bureaublad-sessie met de virtuele machine. Vervang het IP-adres door het publicIPAddress van de virtuele machine. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u hebt gebruikt bij het maken van de virtuele machine.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>IIS installeren met behulp van PowerShell

U bent nu aangemeld bij de VM van Azure en er is nog maar één regel code van PowerShell nodig om IIS te installeren en de regel voor de lokale firewall in te schakelen om webverkeer toe te staan. Open een PowerShell-prompt en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Met IIS is geïnstalleerd en met poort 80 is geopend op uw virtuele machine, kunt u een webbrowser van uw keuze om weer te geven van de standaardwelkomstpagina van IIS. Gebruik de *publicIpAddress* u gedocumenteerd in de vorige sectie de standaardpagina te bezoeken.

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png)

## <a name="delete-the-virtual-machine"></a>Verwijder de virtuele machine

Wanneer deze niet langer nodig is, moet u de volgende opdracht gebruiken om te verwijderen van de resourcegroep die de virtuele machine en de bijbehorende resources bevat:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

U kunt een eenvoudige Windows virtuele machine hebt geïmplementeerd in deze snelstartgids. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
