---
title: Vinden van de volgende hop met Azure-netwerk-Watcher volgende Hop - Azure CLI 2.0 | Microsoft Docs
description: In dit artikel wordt beschreven hoe u kunt vinden wat het volgende hoptype is en IP-adres met de volgende Hop met Azure CLI.
services: network-watcher
documentationcenter: na
author: georgewallace
manager: jimdial
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fb4a24fd758ad4b7231364f3ee7d56a9a2dbccb1
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a>Uitzoeken wat het volgende hoptype gebruikt de mogelijkheid van de volgende Hop in Azure met Azure CLI 2.0 netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

De volgende hop is een functie van netwerk-Watcher die de mogelijkheid get biedt de volgende hoptype en IP-adres op basis van een opgegeven virtuele machine. Deze functie is handig bij het bepalen of een virtuele machine uitgaand verkeer van een gateway, internet of virtuele netwerken om te gaan naar de bestemming passeert.

Dit artikel wordt de volgende generatie CLI gebruikt voor de resource management-implementatiemodel, Azure CLI 2.0, die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario gebruikt u de Azure CLI het volgende hoptype en IP-adres vinden.

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel maakt gebruik van volgende Hop, een functie van netwerk-Watcher waarmee wordt gezocht naar de volgende hoptype en IP-adres voor een resource. Voor meer informatie over de volgende Hop, gaat u naar [volgende Hop overzicht](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Ophalen van de volgende Hop

Ophalen van de volgende hop we noemen de `az network watcher show-next-hop` cmdlet. De cmdlet geven we de resourcegroep voor netwerk-Watcher, de NetworkWatcher virtuele machine Id, IP-bronadres en IP-doeladres. In dit voorbeeld is het IP-doeladres voor een virtuele machine in een ander virtueel netwerk. Er is een virtuele netwerkgateway tussen de twee virtuele netwerken.

Als u dit nog niet hebt nog installeren en configureren van de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login). Voer de volgende opdracht:

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
Als de virtuele machine meerdere NIC's heeft en doorsturen via IP is ingeschakeld op een van de NIC's en vervolgens de NIC-parameter (-ik nic-id) moet worden opgegeven. Anders is optioneel.

## <a name="review-results"></a>Resultaat controleren

Na voltooiing wordt worden de resultaten geleverd. De volgende hop-IP-adres en het type resource dat is geretourneerd.

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
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
