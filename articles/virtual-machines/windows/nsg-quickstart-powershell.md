---
title: Openen van poorten voor een virtuele machine met Azure PowerShell | Microsoft Docs
description: Meer informatie over het openen van een poort / maken van een eindpunt met uw Windows-virtuele machine met de modus Azure resource manager-implementatie en de Azure PowerShell
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: d9918992ef1ac46b3b000480b6be7e984f0858e1
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Het openen van poorten en eindpunten voor een virtuele machine in Azure met behulp van PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
Maken van een Netwerkbeveiligingsgroep en de ACL-regels moet u [de meest recente versie van Azure PowerShell geïnstalleerd](/powershell/azureps-cmdlets-docs). U kunt ook [u deze stappen uitvoert met de Azure portal](nsg-quickstart-portal.md).

Aanmelden bij uw Azure-account:

```powershell
Login-AzureRmAccount
```

In de volgende voorbeelden kunt u parameternamen vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *myNetworkSecurityGroup*, en *myVnet*.

Maak een regel met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Het volgende voorbeeld wordt een regel met naam *myNetworkSecurityGroupRule* om toe te staan *tcp* verkeer op poort *80*:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Maak vervolgens uw netwerk van een beveiligingsgroep met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) en wijs de HTTP-regel die u zojuist hebt gemaakt, als volgt. Het volgende voorbeeld wordt een Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Nu gaan we uw Netwerkbeveiligingsgroep toewijzen aan een subnet. Het volgende voorbeeld wordt een bestaand virtueel netwerk met de naam *myVnet* aan de variabele *$vnet* met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

De Netwerkbeveiligingsgroep koppelen aan uw subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt gekoppeld aan het subnet met de naam *mySubnet* aan uw Netwerkbeveiligingsgroep:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Tot slot werkt uw virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) om uw wijzigingen pas van kracht:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunt u aan de slag te kunnen met verkeer naar uw virtuele machine. Netwerkbeveiligingsgroepen bieden veel handige functies en granulatie voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier maken van een Netwerkbeveiligingsgroep en ACL regels](tutorial-virtual-network.md#secure-network-traffic).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Load Balancer van Azure plaatsen. De load balancer wordt verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee wordt verkeer gefilterd. Zie voor meer informatie [het laden van Linux virtuele machines in Azure maken van een maximaal beschikbare toepassing in evenwicht](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is een netwerkbeveiligingsgroep (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Overzicht van Azure Resource Manager voor Load Balancers](../../load-balancer/load-balancer-arm.md)

