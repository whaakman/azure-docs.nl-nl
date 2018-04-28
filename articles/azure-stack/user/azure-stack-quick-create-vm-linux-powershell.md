---
title: Een virtuele Linux-machine maken met behulp van PowerShell in Azure-Stack | Microsoft Docs
description: Een virtuele Linux-machine maken met PowerShell in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 03EE5929-4F05-47D7-B246-EA93D6FC47CD
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 86597defad7c76d41065270030a4c77ee901b014
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-powershell-in-azure-stack"></a>Snelstartgids: een virtuele Linux-server-machine maken met behulp van PowerShell in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Ubuntu Server 16.04 TNS virtuele machine maken met behulp van PowerShell voor Azure-Stack. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine.  In dit artikel hebt u ook de stappen voor het:

* Verbinding maken met de virtuele machine met een externe client.
* Niet-gebruikte resources opschonen.

## <a name="prerequisites"></a>Vereisten

* **Een Linux-installatiekopie in de Stack van Azure marketplace**

   De Stack van Azure marketplace bevat geen standaard een Linux-installatiekopie. Ophalen van de Azure-Stack-operator om te bieden de **Ubuntu Server 16.04 LTS** u moet de installatiekopie. De operator kunt de stappen in de [downloaden marketplace-items van Azure naar Azure Stack](../azure-stack-download-azure-marketplace-item.md) artikel.

* Azure-Stack is vereist voor een specifieke versie van Azure PowerShell maken en beheren van resources. Als er geen PowerShell voor Azure-Stack is geconfigureerd, aanmelden bij de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) en volg de stappen voor het [ Installeer](azure-stack-powershell-install.md) en [configureren](azure-stack-powershell-configure-user.md) PowerShell.

* Een openbare SSH-sleutel met de naam id_rsa.pub opgeslagen in de SSH-map van uw Windows-gebruikersprofiel. Zie voor gedetailleerde informatie over het maken van SSH-sleutels [maken van SSH-sleutels op Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin u kunt implementeren en beheren van resources voor Azure-Stack. Uitvoeren vanuit uw development kit of het systeem Azure Stack geïntegreerd, het volgende codeblok voor het maken van een resourcegroep. Waarden voor de variabelen in dit document zijn toegewezen, kunt u deze waarden gebruiken of nieuwe waarden toewijzen.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Storage-resources maken

Een opslagaccount maken en maak vervolgens een opslagcontainer voor de installatiekopie Ubuntu Server 16.04 LTS.

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

De netwerkbeveiligingsgroep beveiligt de virtuele machine met behulp van regels voor binnenkomend en uitgaand. Een inkomende regel voor poort 3389 voor het toestaan van binnenkomende verbindingen van extern bureaublad en een inkomende regel voor poort 80 om toe te staan inkomend webverkeer maken.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location local `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
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

Maak een virtuele-machineconfiguratie. Deze configuratie bevat de instellingen die zijn gebruikt bij het implementeren van de virtuele machine. Bijvoorbeeld: de referenties van gebruiker, de grootte en de installatiekopie van de virtuele machine.

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
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

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the virtual machine
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Nadat de virtuele machine is geïmplementeerd, configureert u een SSH-verbinding voor de virtuele machine. Gebruik de opdracht [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-4.3.1) om het openbare IP-adres van de virtuele machine te retourneren.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht verbinding maken met de virtuele machine van een clientsysteem met SSH geïnstalleerd. Als u in Windows werkt, kunt u [Putty](http://www.putty.org/) om de verbinding te maken.

```
ssh <Public IP Address>
```

Wanneer u wordt gevraagd, typt u azureuser als de aanmeldingsgebruiker. Als u een wachtwoordzin gebruikt bij het maken van de SSH-sleutels, hebt u de wachtwoordzin opgeven.

## <a name="clean-up-resources"></a>Resources opschonen

Opschonen van de resources die u niet langer nodig. U kunt de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup?view=azurermps-4.3.1) opdracht om te verwijderen van deze resources. Voer de volgende opdracht voor het verwijderen van de resourcegroep en alle bijbehorende resources:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een eenvoudige virtuele machine van een Linux-server geïmplementeerd. Voor meer informatie over Azure-Stack virtuele machines, gaat u naar [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
