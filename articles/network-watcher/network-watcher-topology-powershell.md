---
title: Azure-netwerk-Watcher-topologie - PowerShell weergeven | Microsoft Docs
description: In dit artikel wordt beschreven hoe u PowerShell gebruikt om op te vragen uw netwerktopologie.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 84e925b4461e55e570e9848bf03d3d352bfff898
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a>Netwerk-Watcher-topologie met PowerShell weergeven

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

De functie van de topologie van netwerk-Watcher biedt een visuele representatie van de netwerkbronnen in een abonnement. In de portal voor is deze visualisatie aan u gepresenteerd automatisch. De gegevens achter de Topologieweergave in de portal kan worden opgehaald via PowerShell.
Hierdoor is de topologische informatie flexibeler omdat de gegevens kunnen worden gebruikt door andere hulpprogramma's voor het bouwen van de visualisatie.

De koppeling is gemodelleerd onder twee-relaties.

- **Containment** -voorbeeld: VNet bevat een Subnet bevat een NIC
- **Gekoppeld** -voorbeeld: NIC is gekoppeld aan een virtuele machine

De volgende lijst bevat eigenschappen die worden geretourneerd bij het opvragen van de topologie REST-API.

* **naam** -de naam van de resource
* **id** -de uri van de resource.
* **locatie** -de locatie waar de resource is opgeslagen.
* **koppelingen** -een lijst met koppelingen naar het object waarnaar wordt verwezen.
    * **naam** -de naam van de bron waarnaar wordt verwezen.
    * **resourceId** -resourceId van de is de uri van de bron waarnaar wordt verwezen in de koppeling.
    * **associationType** -deze waarde verwijst naar de relatie tussen het onderliggende object en het bovenliggende item. Geldige waarden zijn **bevat** of **gekoppelde**.

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario gebruikt u de `Get-AzureRmNetworkWatcherTopology` cmdlet voor het ophalen van de topologische informatie. Er is ook een artikel over het [ophalen netwerktopologie met REST-API](network-watcher-topology-rest.md).

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel wordt het antwoord van de topologie voor een opgegeven resourcegroep opgehaald.

## <a name="retrieve-network-watcher"></a>Ophalen van netwerk-Watcher

De eerste stap is het exemplaar van de netwerk-Watcher ophalen. De `$networkWatcher` variabele wordt doorgegeven aan de `Get-AzureRmNetworkWatcherTopology` cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a>Topologie ophalen

De `Get-AzureRmNetworkWatcherTopology` cmdlet haalt de topologie voor een bepaalde groep.

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a>Resultaten

De resultaten hebben een eigenschap name 'Resources', waarin de json-antwoordtekst voor de `Get-AzureRmNetworkWatcherTopology` cmdlet.  Het antwoord bevat de resources in de Netwerkbeveiligingsgroep en de bijbehorende koppelingen (dat wil zeggen, bevat, gekoppelde).

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG stroom logboeken met Power BI in via [visualiseren NSG loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)


