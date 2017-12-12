---
title: Een virtuele Windows-machine maken met behulp van PowerShell in Azure-Stack | Microsoft Docs
description: Een virtuele Windows-machine maken met PowerShell in Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 7CA6C0AC-23B7-4007-BA32-7A950FD1F3B8
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 688ab6c55867d72d55e27c21c883c14ef90078d2
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-windows-virtual-machine-by-using-powershell-in-azure-stack"></a>Een virtuele Windows-machine maken met behulp van PowerShell in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

Deze handleiding gegevens met behulp van PowerShell voor het maken van een Windows Server 2016 virtuele machine in Azure-Stack. U kunt de stappen in dit artikel vanuit de Azure-Stack Development Kit of vanaf een externe Windows-client als u via VPN-verbinding verbonden bent uitvoeren. 

## <a name="prerequisites"></a>Vereisten 

* Zorg ervoor dat uw Azure-Stack-operator de installatiekopie van het 'WindowsServer 2016' is toegevoegd aan de Stack van Azure marketplace.  

* Azure-Stack is vereist voor een specifieke versie van Azure PowerShell maken en beheren van de resources. Als er geen PowerShell voor Azure-Stack is geconfigureerd, volgt u de stappen voor het [installeren](azure-stack-powershell-install.md) en [configureren](azure-stack-powershell-configure-user.md) PowerShell.    

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container in welke Azure-Stack resources worden geïmplementeerd en beheerd. Uitvoeren vanuit uw development kit of het systeem Azure Stack geïntegreerd, het volgende codeblok voor het maken van een resourcegroep. We waarden voor alle variabelen in dit document hebt toegewezen, kunt u deze als is of een andere waarde toe te wijzen.  

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

Maak een virtuele-machineconfiguratie. De configuratie van bevat de instellingen die worden gebruikt bij het implementeren van de virtuele machine, zoals de installatiekopie van een virtuele machine, grootte, referenties.

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

#Create the virtual machine.
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
 
Gebruik de volgende opdracht voor het maken van een extern bureaublad-sessie met de virtuele machine. Het IP-adres vervangen door de publicIPAddress van uw virtuele machine. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u hebt gebruikt bij het maken van de virtuele machine.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>IIS installeren met behulp van PowerShell

U bent nu aangemeld bij de VM van Azure en er is nog maar één regel code van PowerShell nodig om IIS te installeren en de regel voor de lokale firewall in te schakelen om webverkeer toe te staan. Open een PowerShell-prompt en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het *publicIpAddress* dat u hierboven hebt gedocumenteerd. 

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png) 


## <a name="delete-the-virtual-machine"></a>Verwijder de virtuele machine

Wanneer deze niet langer nodig is, moet u de volgende opdracht gebruiken om te verwijderen van de resourcegroep die de virtuele machine en de bijbehorende resources bevat:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

U kunt een eenvoudige Windows virtuele machine hebt geïmplementeerd in deze snelstartgids. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

