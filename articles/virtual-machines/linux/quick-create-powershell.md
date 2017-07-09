---
title: Azure Quick Start - Een VM PowerShell maken | Microsoft Docs
description: Ontdek snel hoe u virtuele Linux-machines maakt met PowerShell
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 6c9a50c1f3dcd55cd03a694c7d4e13b8a55c3cd4
ms.contentlocale: nl-nl
ms.lasthandoff: 05/31/2017

---

# <a name="create-a-linux-virtual-machine-with-powershell"></a>Een virtuele Linux-machine maken met PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. In deze handleiding staat informatie over het gebruik van de Azure PowerShell-module om een virtuele machine te implementeren met de Ubuntu-server. Zodra de server is geïmplementeerd, wordt een SSH-verbinding gemaakt en een NGINX-webserver geïnstalleerd.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze Quick Start is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Tot slot moet er een openbare SSH-sleutel met de naam *id_rsa.pub* worden opgeslagen in de map *.ssh* van uw Windows-gebruikersprofiel. Zie voor gedetailleerde informatie over het maken van SSH-sleutels voor Azure [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (SSH-sleutels maken voor Azure).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a>Netwerkresources maken

Maak een virtueel netwerk, subnet en een openbaar IP-adres. Deze resources worden gebruikt voor netwerkconnectiviteit met de virtuele machine en om verbinding met internet te maken.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Maak een netwerkbeveiligingsgroep en een regel voor de netwerkbeveiligingsgroep. De netwerkbeveiligingsgroep beveiligt de virtuele machine met binnenkomende en uitgaande regels. In dit geval is er een binnenkomende regel gemaakt voor poort 22, waarmee binnenkomende SSH-verbindingen worden toegestaan. We willen ook een regel maken voor poort 80, de poort voor binnenkomend webverkeer.

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
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Maak een netwerkkaart met [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) voor de virtuele machine. De netwerkkaart verbindt de virtuele machine met een subnet, netwerkbeveiligingsgroep en openbaar IP-adres.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele-machineconfiguratie. Deze configuratie bevat de instellingen die worden gebruikt bij het implementeren van de virtuele machine, zoals een installatiekopie van de virtuele machine, de grootte en de verificatieconfiguratie.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u een SSH-verbinding met de virtuele machine.

Gebruik de opdracht [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de virtuele machine te retourneren.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Op een systeem waarop SSH is geïnstalleerd, gebruikt u de volgende opdracht om verbinding te maken met de virtuele machine. Als u in Windows werkt, kan [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) worden gebruikt om de verbinding tot stand te brengen. 

```bash 
ssh <Public IP Address>
```

Als u hierom wordt gevraagd, is de gebruikersnaam voor het aanmelden *azureuser*. Als er een wachtwoordzin is opgegeven bij het maken van SSH-sleutels, moet u deze ook invoeren.


## <a name="install-nginx"></a>NGINX installeren

Gebruik het volgende bash-script om pakketbronnen bij te werken en het meest recente NGINX-pakket te installeren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>De welkomstpagina van NGIX weergeven

Nu NGINX is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van NGINX weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het openbare IP-adres dat u hierboven hebt gedocumenteerd. 

![Standaardsite van NGINX](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Snel starten hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkbeveiligingsgroepregel gemaakt en een webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Linux-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Linux-machines](./tutorial-manage-vm.md)

