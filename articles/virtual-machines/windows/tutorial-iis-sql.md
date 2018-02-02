---
title: Maken van virtuele machines met een SQL- &#92; IIS &#92;. NET-stack in Azure | Microsoft Docs
description: Zelfstudie - installatie van een Azure SQL, IIS, .NET stack is op een Windows virtuele machines.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6533ab205e07243e2f757ea0a66028e1d140c52b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Installeer een SQL- &#92; IIS &#92;. NET-stack in Azure

In deze zelfstudie installeren we een SQL- &#92; IIS &#92;. NET stack met Azure PowerShell. Deze stack bestaat uit twee virtuele machines met Windows Server 2016, één met IIS en .NET en de andere met SQL Server.

> [!div class="checklist"]
> * Een virtuele machine met behulp van New-AzVM maken
> * IIS en de kern van het .NET SDK installeren op de virtuele machine
> * Een virtuele machine met SQL Server maken
> * De uitbreiding voor de SQL Server installeren

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 5.1.1 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-iis-vm"></a>Een IIS-virtuele machine maken 

In dit voorbeeld gebruiken we de [nieuw AzVM](https://www.powershellgallery.com/packages/AzureRM.Compute.Experiments) cmdlet in de Cloud-Shell PowerShell snel een virtuele machine van Windows Server 2016 maken en vervolgens installeren IIS en .NET Framework. IIS en SQL-machines delen een resourcegroep en een virtueel netwerk, zodat we de variabelen voor deze namen maken.

Klik op de **probeert het** knop aan de rechterbovenhoek van het codeblok Cloud Shell in dit venster te starten. U wordt gevraagd referenties op te geven voor de virtuele machine op de opdrachtprompt.

```azurepowershell-interactive
$vmName = "IISVM$(Get-Random)"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRMVm -Name $vmName -ResourceGroupName $resourceGroup -VirtualNetworkName $vNetName 
```

Installatie van IIS en .NET framework met behulp van de extensie voor aangepaste scripts.

```azurepowershell-interactive

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="azure-sql-vm"></a>Azure SQL VM

We gebruiken een vooraf geconfigureerde Azure marketplace-installatiekopie van een SQL server de SQL-VM's te maken. We eerst de virtuele machine maken en de SQL Server-extensie wordt geïnstalleerd op de virtuele machine. 


```azurepowershell-interactive
# Create user object. You get a pop-up prompting you to enter the credentials for the VM.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroup
Add-AzureRmVirtualNetworkSubnetConfig -Name mySQLSubnet -VirtualNetwork $vNet -AddressPrefix "192.168.2.0/24"
Set-AzureRmVirtualNetwork -VirtualNetwork $vNet


# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location eastus `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow


# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location eastus `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name mySQLNic -ResourceGroupName $resourceGroup -Location eastus `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName mySQLVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName mySQLVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftSQLServer -Offer SQL2014SP2-WS2012R2 -Skus Enterprise -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM -ResourceGroupName $resourceGroup -Location eastus -VM $vmConfig
```

Gebruik [Set AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) toevoegen de [SQL Server-extensie](/sql/virtual-machines-windows-sql-server-agent-extension.md) voor de SQL-VM.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension -ResourceGroupName $resourceGroup -VMName mySQLVM -name "SQLExtension"
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geïnstalleerd een SQL- &#92; IIS &#92;. NET stack met Azure PowerShell. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtuele machine met behulp van New-AzVM maken
> * IIS en de kern van het .NET SDK installeren op de virtuele machine
> * Een virtuele machine met SQL Server maken
> * De uitbreiding voor de SQL Server installeren

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van IIS-webserver met SSL-certificaten.

> [!div class="nextstepaction"]
> [IIS-webserver met SSL-certificaten beveiligen](tutorial-secure-web-server.md)

