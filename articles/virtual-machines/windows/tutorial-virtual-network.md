---
title: Windows virtuele Machines en virtuele netwerken in Azure | Microsoft Docs
description: Zelfstudie - virtuele Azure-netwerken en virtuele Machines van Windows met Azure PowerShell beheren
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: feaef679a3090491b64c69ac69bf22153c281d31
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Virtuele Azure-netwerken en virtuele Machines van Windows met Azure PowerShell beheren

Virtuele Azure-machines maken gebruik van Azure-netwerken voor interne en externe communicatie. In deze zelfstudie wordt uitgelegd hoe u twee virtuele machines implementeert en Azure-netwerken configureert voor deze virtuele machines. In de voorbeelden in deze zelfstudie wordt ervan uitgegaan dat de virtuele machines een webtoepassing met een databaseback-end hosten, maar er wordt geen toepassing geïmplementeerd in de zelfstudie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een virtueel netwerk en een subnet maken
> * Een openbaar IP-adres maken
> * Een front-end virtuele machine maken
> * Netwerkverkeer beveiligen
> * Een back-end virtuele machine maken



Voor deze zelfstudie is module versie 4.3.1 of hoger van AzureRM.Compute vereist. Voer `Get-Module -ListAvailable AzureRM.Compute` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Overzicht van VM-netwerken

Virtuele Azure-netwerken maken beveiligde netwerkverbindingen mogelijk tussen virtuele machines, internet en andere Azure-services zoals Azure SQL database. Virtuele netwerken zijn onderverdeeld in logische segmenten die subnetten worden genoemd. Subnetten worden gebruikt om de netwerkstroom te beheersen, en als een beveiligingsgrens. Wanneer een virtuele machine wordt geïmplementeerd, omvat deze doorgaans een virtuele netwerkinterface, die is gekoppeld aan een subnet.

Tijdens het volgen van deze zelfstudie worden de volgende resources gemaakt:

![Virtueel netwerk met twee subnetten](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet*: het virtuele netwerk dat de virtuele machines gebruiken om met elkaar en met internet te communiceren.
- *myFrontendSubnet*: het subnet in *myVNet* dat door de front-endresources wordt gebruikt.
- *myPublicIPAddress*: het openbare IP-adres dat wordt gebruikt voor toegang tot *myFrontendVM* via internet.
- *myFrontentNic*: de netwerkinterface die wordt gebruikt door *myFrontendVM* om te communiceren met *myBackendVM*.
- *myFrontendVM*: de VM die wordt gebruikt voor communicatie tussen internet en *myBackendVM*.
- *myBackendNSG*: de netwerkbeveiligingsgroep waardoor de communicatie tussen de *myFrontendVM* en *myBackendVM* wordt bepaald.
- *myBackendSubnet*: het subnet dat is gekoppeld aan *myBackendNSG* en door de back-endresources wordt gebruikt.
- *myBackendNic*: de netwerkinterface die wordt gebruikt door *myBackendVM* om te communiceren met *myFrontendVM*.
- *myBackendVM* -de virtuele machine die poort 1433 gebruikt om te communiceren met *myFrontendVM*.


## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken

Voor deze zelfstudie wordt één virtueel netwerk met twee subnetten gemaakt. Een front-endsubnet voor het hosten van een webtoepassing en een back-endsubnet voor het hosten van een databaseserver.

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

Op dit moment is er een netwerk gemaakt en gesegmenteerd in twee subnetten, één voor de front-endservices en een andere voor back-endservices. In de volgende sectie worden de virtuele machines gemaakt en verbonden met deze subnetten.

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Met een openbaar IP-adres kunnen Azure-resources toegankelijk zijn via internet. De toewijzingsmethode van het openbare IP-adres kan worden geconfigureerd als dynamisch of statisch. Een openbaar IP-adres wordt standaard dynamisch toegewezen. Dynamische IP-adressen worden vrijgegeven wanneer de toewijzing van een virtuele machine ongedaan wordt gemaakt. Dit gedrag zorgt ervoor dat het IP-adres wijzigt tijdens een bewerking waarbij de toewijzing van een virtuele machine ongedaan wordt gemaakt.

De toewijzingsmethode kan worden ingesteld op statisch. Dit zorgt ervoor dat het IP-adres aan een virtuele machine toegewezen blijft, zelfs bij een status waarin de toewijzing ongedaan is gemaakt. Wanneer u een statisch toegewezen IP-adres gebruikt, kan het IP-adres zelf niet worden opgegeven. Dit wordt toegewezen uit een groep met beschikbare adressen.

Maken van een openbaar IP-adres met de naam *myPublicIPAddress* met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

U kunt de parameter - AllocationMethod wijzigen `Static` statisch openbaar IP-adres toewijzen.

## <a name="create-a-front-end-vm"></a>Een front-end virtuele machine maken

Voor een virtuele machine om te communiceren in een virtueel netwerk, moet de virtuele netwerkinterface (NIC). Maak een NIC met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontend `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Stel de gebruikersnaam en wachtwoord nodig voor het beheerdersaccount op de virtuele machine met behulp [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). U gebruikt deze referenties verbinding maken met de virtuele machine in extra stappen uitvoeren:

```azurepowershell-interactive
$cred = Get-Credential
```

Maken van de virtuele machines met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myFrontend `
   -PublicIpAddressName myPublicIPAddress `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -Size Standard_D1 `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

## <a name="secure-network-traffic"></a>Netwerkverkeer beveiligen

Een netwerkbeveiligingsgroep (Network Security Group, NSG) bevat een lijst met beveiligingsregels waarmee netwerkverkeer naar resources die zijn verbonden met virtuele Azure-netwerken (VNet) wordt toegestaan of geweigerd. Netwerkbeveiligingsgroepen kunnen worden gekoppeld aan subnetten of afzonderlijke netwerkinterfaces. Wanneer een netwerkbeveiligingsgroep is gekoppeld aan een netwerkinterface, is deze alleen van toepassing op de gekoppelde virtuele machine. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet.

### <a name="network-security-group-rules"></a>Regels voor netwerkbeveiligingsgroepen

Met regels voor netwerkbeveiligingsgroepen worden netwerkpoorten gedefinieerd waarover verkeer wordt toegestaan of geweigerd. De regels kunnen bron- en doel-IP-adresbereiken bevatten, zodat verkeer wordt geregeld tussen specifieke systemen of subnetten. Regels voor netwerkbeveiligingsgroepen bevatten ook een prioriteit (tussen 1- en 4096). Regels worden in volgorde van prioriteit gecontroleerd. Een regel met een prioriteit van 100 wordt geëvalueerd vóór een regel met een prioriteit van 200.

Alle NSG's bevatten een set met standaardregels. De standaardregels kunnen niet worden verwijderd, maar omdat ze de laagste prioriteit hebben, kunnen ze worden overschreven door de regels die u maakt.

- **Virtueel netwerk**: verkeer dat afkomstig is van en eindigt in een virtueel netwerk wordt toegestaan in zowel binnenkomende als uitgaande richting.
- **Internet**: uitgaand verkeer is toegestaan, maar binnenkomend verkeer wordt geblokkeerd.
- **Load balancer:**: toestaan dat de load balancer van Azure de status van uw VM's en rolexemplaren controleert. Als u geen set met taakverdeling gebruikt, kunt u deze regel onderdrukken.

### <a name="create-network-security-groups"></a>Netwerkbeveiligingsgroepen maken

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

## <a name="create-a-back-end-vm"></a>Een back-end virtuele machine maken

De eenvoudigste manier om de back-end virtuele machine maken voor deze zelfstudie is met een installatiekopie van SQL Server. Deze zelfstudie alleen de virtuele machine maakt met de database-server, maar geen biedt informatie over het openen van de database.

Maak *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackend `
  -SubnetId $vnet.Subnets[1].Id
```

De gebruikersnaam en wachtwoord nodig voor het beheerdersaccount op de virtuele machine met Get-Credential instellen:

```azurepowershell-interactive
$cred = Get-Credential
```

Maak *myBackendVM*.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myBackend `
   -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

De afbeelding die wordt gebruikt SQL Server is geïnstalleerd, maar niet in deze zelfstudie wordt gebruikt. Het is opgenomen om weer te geven hoe u een virtuele machine voor het afhandelen van webverkeer en een virtuele machine voor het afhandelen van database-beheer kunt configureren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure-netwerken met betrekking tot virtuele machines gemaakt en beveiligd. 

> [!div class="checklist"]
> * Een virtueel netwerk en een subnet maken
> * Een openbaar IP-adres maken
> * Een front-end virtuele machine maken
> * Netwerkverkeer beveiligen
> * Een back-end virtuele machine maken

Ga naar de volgende zelfstudie voor meer informatie over het controleren van het beveiligen van gegevens op virtuele machines met behulp van Azure backup.

> [!div class="nextstepaction"]
> [Back-up van Windows virtuele machines in Azure](./tutorial-backup-vms.md)
