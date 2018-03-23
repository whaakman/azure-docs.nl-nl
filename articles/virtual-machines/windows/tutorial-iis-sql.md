---
title: Virtuele machines met een SQL&#92;IIS&#92;.NET-stack maken in Azure | Microsoft Docs
description: Zelfstudie - Een Azure SQL, IIS, .NET-stack installeren op virtuele Windows-machines.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ac6038f7600d6eb4c8d021998f9cfc40bd369332
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Een SQL&#92;IIS&#92;.NET-stack installeren in Azure

In deze zelfstudie installeren we een SQL&#92;IIS&#92;.NET-stack met Azure PowerShell. Deze stack bestaat uit twee virtuele machines met Windows Server 2016, waarvan een met IIS en .NET en een met SQL Server.

> [!div class="checklist"]
> * Een virtuele machine maken 
> * IIS en .NET Core SDK installeren op de virtuele machine
> * Een VM maken waarop SQL Server wordt uitgevoerd
> * De SQL Server-extensie installeren

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 4.3.1 of hoger van AzureRM.Compute vereist. Voer `Get-Module -ListAvailable AzureRM.Compute` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="create-a-iis-vm"></a>Een virtuele IIS-machine maken 

In dit voorbeeld gebruiken we de cmdlet [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) in de Cloud Shell van PowerShell om snel een virtuele machine met Windows Server 2016 te maken en vervolgens IIS en .NET Framework te installeren. De virtuele IIS en SQL-machines delen een resourcegroep en een virtueel netwerk, dus voor deze namen maken we variabelen.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

Installeer IIS en .NET Framework met behulp van de aangepaste scriptextensie.

```azurepowershell-interactive
Set-AzureRmVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Nog een subnet maken

Maak een tweede subnet voor de SQL-VM. Haal het vNet op met behulp van [Get-AzureRmVirtualNetwork]{/powershell/module/azurerm.network/get-azurermvirtualnetwork}.

```azurepowershell-interactive
$vNet = Get-AzureRmVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

Maak een configuratie voor het subnet met behulp van [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig).


```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

Werk het vNet bij met de nieuwe subnetgegevens met behulp van de cmdlet [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).
   
```azurepowershell-interactive   
$vNet | Set-AzureRmVirtualNetwork
```

## <a name="azure-sql-vm"></a>Azure SQL-VM

Gebruik een vooraf geconfigureerde Azure Marketplace-installatiekopie van een SQL-server om de SQL-VM te maken. We maken eerst de virtuele machine en installeren vervolgens de SQL Server-extensie op de VM. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

Gebruik [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) om de [SQL Server-extensie](/sql/virtual-machines-windows-sql-server-agent-extension.md) toe te voegen aan de SQL-VM.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een SQL&#92;IIS&#92;.NET-stack geïnstalleerd met behulp van Azure PowerShell. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtuele machine maken 
> * IIS en .NET Core SDK installeren op de virtuele machine
> * Een VM maken waarop SQL Server wordt uitgevoerd
> * De SQL Server-extensie installeren

Ga door naar de volgende zelfstudie om te leren hoe u een IIS-webserver kunt beveiligen met behulp van SSL-certificaten.

> [!div class="nextstepaction"]
> [Een IIS-webserver beveiligen met SSL-certificaten](tutorial-secure-web-server.md)

