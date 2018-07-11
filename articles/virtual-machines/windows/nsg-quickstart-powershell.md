---
title: Openen van poorten op een virtuele machine met behulp van Azure PowerShell | Microsoft Docs
description: Leer hoe u een poort openen / maken van een eindpunt voor uw Windows-VM met behulp van de modus Azure resource manager-implementatie en Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: f79db8cdec0aa48ae300aff4c58072fb6afdc932
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932779"
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Het openen van poorten en eindpunten aan een virtuele machine in Azure met behulp van PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
Maken van een Netwerkbeveiligingsgroep en ACL-regels moet u [de nieuwste versie van Azure PowerShell is geïnstalleerd](/powershell/azureps-cmdlets-docs). U kunt ook [deze stappen uitvoeren met de Azure-portal](nsg-quickstart-portal.md).

Meld u aan bij uw Azure-account:

```powershell
Connect-AzureRmAccount
```

In de volgende voorbeelden kunt u namen van parameters vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen *myResourceGroup*, *myNetworkSecurityGroup*, en *myVnet*.

Maak een regel met [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Het volgende voorbeeld wordt een regel met de naam *myNetworkSecurityGroupRule* om toe te staan *tcp* verkeer op poort *80*:

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

Maak vervolgens uw netwerkbeveiligingsgroep met [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) en de HTTP-regel die u zojuist hebt gemaakt, als volgt toe te wijzen. Het volgende voorbeeld wordt een Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Nu gaan we uw Netwerkbeveiligingsgroep aan een subnet toewijzen. In het volgende voorbeeld wordt een bestaand virtueel netwerk met de naam *myVnet* aan de variabele *$vnet* met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Uw Netwerkbeveiligingsgroep koppelen aan uw subnet met [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt gekoppeld aan het subnet met de naam *mySubnet* met uw Netwerkbeveiligingsgroep:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Werk tot slot het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) om uw wijzigingen worden doorgevoerd:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met de verkeersstroom naar uw virtuele machine. Netwerkbeveiligingsgroepen bevatten veel geweldige functies en granulariteit voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier het maken van een Netwerkbeveiligingsgroep en ACL-regels](tutorial-virtual-network.md#secure-network-traffic).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Azure Load Balancer plaatsen. De load balancer verdeelt het verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee verkeer filteren. Zie voor meer informatie, [laden saldo van Linux virtuele machines in Azure om te maken van een maximaal beschikbare toepassing](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. U vindt meer informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is er een netwerkbeveiligingsgroep?](../../virtual-network/security-overview.md)
* [Overzicht van Azure Resource Manager voor Load Balancers](../../load-balancer/load-balancer-arm.md)

