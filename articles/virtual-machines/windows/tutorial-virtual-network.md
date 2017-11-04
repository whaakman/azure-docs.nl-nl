---
title: Windows virtuele Machines en virtuele netwerken in Azure | Microsoft Docs
description: Zelfstudie - virtuele Azure-netwerken en virtuele Machines van Windows met Azure PowerShell beheren
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/12/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 21f2d586a4c468071bec55c65005b35baf323fe7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Virtuele Azure-netwerken en virtuele Machines van Windows met Azure PowerShell beheren

Azure netwerken virtuele Azure-machines gebruiken voor interne en externe communicatie. Deze zelfstudie wordt begeleid twee virtuele machines implementeren en configureren van Azure netwerken voor deze virtuele machines. De voorbeelden in deze zelfstudie wordt ervan uitgegaan dat de virtuele machines als host voor een webtoepassing met een database back-end optreden, maar een toepassing niet is geïmplementeerd in de zelfstudie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een virtueel netwerk en subnet maken
> * Een openbaar IP-adres maken
> * Een front-virtuele machine maken
> * Netwerkverkeer beveiligen
> * Back-end virtuele machine maken

Tijdens het voltooien van deze zelfstudie ziet u deze resources gemaakt:

![Virtueel netwerk met twee subnetten](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -het virtuele netwerk dat de virtuele machines gebruiken om te communiceren met elkaar en het internet.
- *myFrontendSubnet* -het subnet in *myVNet* die door de front-bronnen.
- *myPublicIPAddress* -het openbare IP-adres gebruikt voor toegang tot *myFrontendVM* vanaf internet.
- *myFrontentNic* -de netwerkinterface die wordt gebruikt door *myFrontendVM* om te communiceren met *myBackendVM*.
- *myFrontendVM* -de VM wordt gebruikt voor communicatie tussen het internet en *myBackendVM*.
- *myBackendNSG* -de netwerkbeveiligingsgroep waarmee de communicatie tussen de *myFrontendVM* en *myBackendVM*.
- *myBackendSubnet* -het subnet gekoppeld *myBackendNSG* en gebruikt door de back-end-resources.
- *myBackendNic* -de netwerkinterface die wordt gebruikt door *myBackendVM* om te communiceren met *myFrontendVM*.
- *myBackendVM* -de virtuele machine die poort 1433 gebruikt om te communiceren met *myFrontendVM*.

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Overzicht van VM-netwerken

Virtuele netwerken van Azure inschakelen beveiligde netwerkverbindingen tussen virtuele machines, internet en andere Azure-services zoals Azure SQL database. Virtuele netwerken zijn onderverdeeld in logische segmenten subnetten genoemd. Subnetten worden gebruikt voor Controlestroom netwerk, en als een beveiligingsgrens. Bij het implementeren van een virtuele machine, omvat het doorgaans de virtuele netwerkinterface, is gekoppeld aan een subnet.

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en subnet maken

Voor deze zelfstudie wordt één virtueel netwerk met twee subnetten gemaakt. Een front-end-subnet voor het hosten van een webtoepassing en een back-end-subnet voor het hosten van een database-server.

Voordat u een virtueel netwerk maken kunt, maakt u een resource-groep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myRGNetwork* in de *EastUS* locatie:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Subnetconfiguraties maken

Maken van de subnetconfiguratie van een met de naam *myFrontendSubnet* met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Maken van de subnetconfiguratie van een met de naam *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Virtueel netwerk maken

Maak een VNET met de naam *myVNet* met *myFrontendSubnet* en *myBackendSubnet* met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

Op dit moment is een netwerk gemaakt en gesegmenteerde in twee subnetten, één voor de front-end-services en een andere voor back-end-services. In de volgende sectie worden de virtuele machines gemaakt en verbonden met deze subnetten.

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Een openbaar IP-adres kan Azure-resources toegankelijk zijn op het internet. De toewijzingsmethode van het openbare IP-adres kan worden geconfigureerd als dynamische of statische. Een openbaar IP-adres wordt standaard dynamisch toegewezen. Dynamische IP-adressen worden vrijgegeven wanneer een virtuele machine toewijzing ongedaan wordt gemaakt. Dit gedrag zorgt ervoor dat het IP-adres moet wijzigen tijdens een bewerking met een VM toewijzing is opgeheven.

De toewijzingsmethode kan worden ingesteld op statische elektriciteit zorgt ervoor dat het IP-adres toegewezen aan een virtuele machine, zelfs tijdens de status van een toewijzing ongedaan is gemaakt blijft. Wanneer u een statisch toegewezen IP-adres, kan niet het IP-adres zelf worden opgegeven. Het wordt toegewezen uit een groep met beschikbare adressen.

Maken van een openbaar IP-adres met de naam *myPublicIPAddress* met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

U kunt de parameter - AllocationMethod wijzigen `Static` statisch openbaar IP-adres toewijzen.

## <a name="create-a-front-end-vm"></a>Een front-virtuele machine maken

Voor een virtuele machine om te communiceren in een virtueel netwerk, moet de virtuele netwerkinterface (NIC). Maak een NIC met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Stel de gebruikersnaam en wachtwoord nodig voor het beheerdersaccount op de virtuele machine met behulp [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). U gebruikt deze referenties verbinding maken met de virtuele machine in extra stappen uitvoeren:

```azurepowershell-interactive
$cred = Get-Credential
```

Maken van de virtuele machines met [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), en [nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="secure-network-traffic"></a>Netwerkverkeer beveiligen

Een netwerkbeveiligingsgroep (Network Security Group, NSG) bevat een lijst met beveiligingsregels waarmee netwerkverkeer naar resources die zijn verbonden met virtuele Azure-netwerken (VNet) wordt toegestaan of geweigerd. Nsg's kunnen worden gekoppeld aan subnetten of afzonderlijke netwerkinterfaces. Wanneer u een NSG is gekoppeld aan een netwerkinterface, geldt deze de bijbehorende virtuele machine. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet.

### <a name="network-security-group-rules"></a>Netwerkbeveiligingsgroepen

NSG-regels definiëren netwerken poorten waarover verkeer wordt toegestaan of geweigerd. De regels kunnen bron en doel-IP-adresbereiken bevatten, zodat verkeer wordt geregeld tussen specifieke systemen of subnetten. NSG-regels bevatten ook een prioriteit (tussen 1- en 4096). Regels worden geëvalueerd in de volgorde van prioriteit. Een regel met een prioriteit van 100 wordt geëvalueerd voor een regel met prioriteit 200.

Alle NSG's bevatten een set met standaardregels. De standaardregels kunnen niet worden verwijderd, maar omdat ze de laagste prioriteit hebben, kunnen ze worden overschreven door de regels die u maakt.

- **Virtueel netwerk** - die afkomstig zijn van verkeer en eindigt in een virtueel netwerk is toegestaan beide kanten van binnenkomend en uitgaand.
- **Internet** - uitgaand verkeer is toegestaan, maar dat binnenkomend verkeer wordt geblokkeerd.
- **De load balancer** -toestaan dat de load balancer van Azure naar de status van uw VM's en rolinstanties. Als u een set met gelijke taakverdeling niet gebruikt, kunt u deze regel onderdrukken.

### <a name="create-network-security-groups"></a>Netwerk-beveiligingsgroepen maken

Maken van een inkomende regel met de naam *myFrontendNSGRule* waarmee inkomend webverkeer op *myFrontendVM* met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

U kunt interne verkeer beperkt tot *myBackendVM* alleen *myFrontendVM* door het maken van een NSG voor het subnet voor back-end. Het volgende voorbeeld wordt een NSG regel voor licentiecontrole *myBackendNSGRule*:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Toevoegen van een netwerkbeveiligingsgroep met de naam *myFrontendNSG* met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Voeg nu een netwerkbeveiligingsgroep met de naam *myBackendNSG* met behulp van New-AzureRmNetworkSecurityGroup:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

De netwerkbeveiligingsgroepen toevoegen aan de subnetten:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Maak een back-end-VM

De eenvoudigste manier om de back-end virtuele machine maken voor deze zelfstudie is met een installatiekopie van SQL Server. Deze zelfstudie alleen de virtuele machine maakt met de database-server, maar geen biedt informatie over het openen van de database.

Maak *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

De gebruikersnaam en wachtwoord nodig voor het beheerdersaccount op de virtuele machine met Get-Credential instellen:

```azurepowershell-interactive
$cred = Get-Credential
```

Maak *myBackendVM*:

```azurepowershell-interactive
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

De afbeelding die wordt gebruikt SQL Server is geïnstalleerd, maar niet in deze zelfstudie wordt gebruikt. Het is opgenomen om weer te geven hoe u een virtuele machine voor het afhandelen van webverkeer en een virtuele machine voor het afhandelen van database-beheer kunt configureren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt en beveiligd Azure-netwerken met betrekking tot virtuele machines. 

> [!div class="checklist"]
> * Een virtueel netwerk en subnet maken
> * Een openbaar IP-adres maken
> * Een front-virtuele machine maken
> * Netwerkverkeer beveiligen
> * Maak een back-end-VM

Ga naar de volgende zelfstudie voor meer informatie over het controleren van het beveiligen van gegevens op virtuele machines met behulp van Azure backup.

> [!div class="nextstepaction"]
> [Back-up van Windows virtuele machines in Azure](./tutorial-backup-vms.md)
