---
title: Vinden van de volgende Hop met Azure-netwerk-Watcher volgende Hop - REST | Microsoft Docs
description: In dit artikel wordt beschreven hoe u kunt vinden wat het volgende hoptype is en IP-adres met de volgende Hop met de REST-API van Azure
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b541cd5cb7e49468af2c522b16c3a3b9fe75fd54
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2018
ms.locfileid: "31809936"
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Uitzoeken wat het volgende hoptype gebruikt de mogelijkheid van de volgende Hop in Azure met Azure REST API netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

De volgende hop is een functie van netwerk-Watcher die de mogelijkheid get biedt de volgende hoptype en IP-adres op basis van een opgegeven virtuele machine. Deze functie is handig bij het bepalen of een virtuele machine uitgaand verkeer van een gateway, internet of virtuele netwerken om te gaan naar de bestemming passeert.

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel maakt gebruik van volgende Hop, een functie van netwerk-Watcher waarmee wordt gezocht naar de volgende hoptype en IP-adres voor een resource. Voor meer informatie over de volgende Hop, gaat u naar [volgende Hop overzicht](network-watcher-next-hop-overview.md).

U wordt in dit scenario:

* De volgende hop voor een virtuele machine worden opgehaald.

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

Aanmelden bij armclient met uw Azure-referenties.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Een virtuele machine ophalen

Voer het volgende script om te retourneren van een virtuele machine. Deze informatie is nodig voor het uitvoeren van volgende hop.

De volgende code moet waarden voor de volgende variabelen:

- **subscriptionId** -de abonnements-Id te gebruiken.
- **resourceGroupName** -de naam van een resourcegroep die virtuele machines bevatten.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

De id van de virtuele machine wordt gebruikt van de volgende uitvoer in het volgende voorbeeld:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Ophalen van de volgende Hop

Zodra de autorisatie-header is gemaakt, kan de volgende hop van een virtuele machine worden opgehaald. De volgende waarden moeten worden vervangen voor het voorbeeld om te werken.

> [!Important]
> Voor netwerk-Watcher REST API-aanroepen dat de naam van de resourcegroep in de aanvraag-URI is de resourcegroep met de netwerk-Watcher, niet de resources u diagnostische acties op uitvoert.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> Volgende hop is vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

## <a name="results"></a>Resultaten

Het volgende codefragment is een voorbeeld van de uitvoer ontvangen. De resultaten bevatten de volgende waarden:

* **nextHopType** -deze waarde is een van de volgende waarden: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway of None.
* **nextHopIpAddress** -het IP-adres van de volgende hop.
* **routeTableId** : de waarde van een uri voor de routetabel die zijn gekoppeld aan de route is of als geen gebruiker gedefinieerde route is gedefinieerd de waarde van *Systeemroute* wordt geretourneerd.

Hieronder vindt u de resultaten in json-indeling.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Volgende stappen

Als u gelukt om erachter te komen de volgende hop voor een virtuele machine, kunt u de beveiliging van uw netwerkbronnen bekijken in via [overzicht van de beveiliging](network-watcher-security-group-view-overview.md)














