---
title: Vinden van de volgende hop met Azure-netwerk-Watcher volgende Hop - PowerShell | Microsoft Docs
description: In dit artikel wordt beschreven hoe u kunt vinden wat het volgende hoptype is en IP-adres met de volgende Hop met behulp van PowerShell.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: ef559fbbd3e8448d64167552cacee04790418343
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Uitzoeken wat het volgende hoptype gebruikt de mogelijkheid van de volgende Hop in Azure met behulp van PowerShell netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

De volgende hop is een functie van netwerk-Watcher die de mogelijkheid get biedt de volgende hoptype en IP-adres op basis van een opgegeven virtuele machine. Deze functie is handig bij het bepalen of een virtuele machine uitgaand verkeer van een gateway, internet of virtuele netwerken om te gaan naar de bestemming passeert.

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario gebruikt u de Azure-portal om het volgende hoptype en IP-adres te vinden.

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel maakt gebruik van volgende Hop, een functie van netwerk-Watcher waarmee wordt gezocht naar de volgende hoptype en IP-adres voor een resource. Voor meer informatie over de volgende Hop, gaat u naar [volgende Hop overzicht](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Ophalen van netwerk-Watcher

De eerste stap is het exemplaar van de netwerk-Watcher ophalen. De `$networkWatcher` variabele wordt doorgegeven aan de volgende hop cmdlet controleren.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>Een virtuele machine ophalen

Volgende hop retourneert de volgende hop en het IP-adres van de volgende hop van een virtuele machine. Een Id van een virtuele machine is vereist voor de cmdlet. Als u de ID van de virtuele machine wilt gebruiken al weet, kunt u deze stap overslaan.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> Volgende hop is vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

## <a name="get-the-network-interfaces"></a>Ophalen van de netwerkinterfaces

Het IP-adres van een NIC op de virtuele machine is vereist, in dit voorbeeld de NIC's op een virtuele machine worden opgehaald. Als u de IP-adres dat u wilt testen op de virtuele machine al weet, kunt u deze stap overslaan.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a>Ophalen van de volgende Hop

Nu we noemen de `Get-AzureRmNetworkWatcherNextHop` cmdlet. De cmdlet geven we de netwerk-Watcher, de virtuele machine Id, de IP-bronadres en het IP-doeladres. In dit voorbeeld is het IP-doeladres voor een virtuele machine in een ander virtueel netwerk. Er is een virtuele netwerkgateway tussen de twee virtuele netwerken.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Resultaat controleren

Na voltooiing wordt worden de resultaten geleverd. De volgende hop-IP-adres en het type resource dat is geretourneerd. In dit scenario is het openbare IP-adres van de virtuele netwerkgateway.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

De volgende lijst bevat de momenteel beschikbare NextHopType waarden:

**Volgend Hoptype**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bekijken van de groep beveiligingsinstellingen van uw netwerk via een programma te bezoeken [NSG controleren met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md)

















