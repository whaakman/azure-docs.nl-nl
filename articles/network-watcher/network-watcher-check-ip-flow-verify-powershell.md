---
title: Controleer of verkeer met Azure-netwerk-Watcher IP-stroom controleren of - PowerShell | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt controleren als verkeer naar of van een virtuele machine wordt toegestaan of geweigerd met behulp van PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 5257a70aa2dbc25bfe4eca5e2e0db87ca5e6b6fe
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Controleer of het verkeer wordt toegestaan of geweigerd naar of van een virtuele machine met IP-stroom controleren of een onderdeel van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-stroom controleren is een functie van netwerk-Watcher die u controleren kunt of er verkeer is toegestaan naar of van een virtuele machine. Dit scenario is nuttig voor het ophalen van de huidige status of een virtuele machine contact met een externe bron of de back-end opnemen kunt. IP-stroom controleren om te controleren of als uw regels Netwerkbeveiligingsgroep (NSG) juist zijn geconfigureerd en problemen oplossen stromen die worden geblokkeerd door het NSG-regels kunnen worden gebruikt. Een andere reden voor het gebruik van IP-stroom controleren om ervoor te zorgen verkeer dat u blokkeren wilt is goed door het NSG worden geblokkeerd.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher of een bestaand exemplaar van netwerk-Watcher hebben. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Dit scenario maakt gebruik van IP-stroom controleren om te controleren of als een virtuele machine naar een bekende Bing IP-adres kan communiceren. Als het verkeer wordt geweigerd, wordt de beveiligingsregel die dat verkeer wordt geweigerd. Ga voor meer informatie over IP-stroom controleren naar [IP-stroom overzicht controleren](network-watcher-ip-flow-verify-overview.md)

## <a name="retrieve-network-watcher"></a>Ophalen van netwerk-Watcher

De eerste stap is het exemplaar van de netwerk-Watcher ophalen. De `$networkWatcher` variabele is doorgegeven aan de IP-adres stroom cmdlet controleren.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Een virtuele machine ophalen

IP-stroom controleren tests verkeer naar of vanuit een IP-adres op een virtuele machine naar of vanuit een externe bestemming. Een Id van een virtuele machine is vereist voor de cmdlet. Als u de ID van de virtuele machine wilt gebruiken al weet, kunt u deze stap overslaan.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a>Ophalen van de NIC 's

Het IP-adres van een NIC op de virtuele machine is vereist, in dit voorbeeld de NIC's op een virtuele machine worden opgehaald. Als u de IP-adres dat u wilt testen op de virtuele machine al weet, kunt u deze stap overslaan.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a>Voer IP-stroom controleren

Nu dat we de vereiste informatie hebben voor het uitvoeren van de cmdlet wordt uitgevoerd de `Test-AzureRmNetworkWatcherIPFlow` cmdlet voor het testen van het verkeer. In dit voorbeeld gebruiken we het eerste IP-adres op de eerste NIC.

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> IP-stroom controleren vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

## <a name="review-results"></a>Resultaten bekijken

Nadat u `Test-AzureRmNetworkWatcherIPFlow` de resultaten worden geretourneerd, is het volgende voorbeeld de resultaten van de vorige stap.

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a>Volgende stappen

Zie als verkeer wordt geblokkeerd en mag geen [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die zijn gedefinieerd.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













