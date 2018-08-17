---
title: Opnieuw instellen van Azure ExpressRoute-peerings | Microsoft Docs
description: Het uitschakelen en inschakelen van peeerings van een ExpressRoute-circuit.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180767"
---
# <a name="reset-expressroute-peerings"></a>Opnieuw instellen van ExpressRoute-peerings

In dit artikel wordt beschreven hoe u uitschakelen en inschakelen van peerings van een ExpressRoute-circuit met behulp van PowerShell. Wanneer u een peering de uitschakelt, wordt de BGP-sessie op de primaire verbinding en de secundaire verbinding van uw ExpressRoute-circuit worden afgesloten. U verliest connectiviteit via deze peering naar Microsoft. Wanneer u een peering hebt ingeschakeld, wordt de BGP-sessie op de primaire verbinding en de secundaire verbinding van uw ExpressRoute-circuit van worden gebracht. U wordt opnieuw verbonden is via deze peering naar Microsoft. U kunt in- en Microsoft-Peering en Privépeering van Azure onafhankelijk van een ExpressRoute-circuit uitschakelen. Wanneer u eerst de peerings op uw ExpressRoute-circuit configureert, wordt de peerings zijn standaard ingeschakeld. 

Er zijn een aantal scenario's waarin u mogelijk nuttig opnieuw instellen van uw ExpressRoute-peerings.
* Test uw disaster recovery ontwerpen en implementeren. Bijvoorbeeld, hebt u twee ExpressRoute-circuits. U kunt de peerings van één aansluiting uitschakelen en afdwingen van uw verkeer failover naar het andere circuit.
* Schakel in twee richtingen doorsturen van detectie (BFD) op Azure Privépeering van uw ExpressRoute-circuit. BFD is standaard ingeschakeld als uw ExpressRoute-circuit is gemaakt na 1 augustus 2018. Als uw circuit voordat die is gemaakt, is niet BFD ingeschakeld. U kunt BFD inschakelen door het uitschakelen van de peering en deze opnieuw inschakelen. Er moet worden opgemerkt dat BFD alleen op persoonlijke Azure-Peering wordt ondersteund.


## <a name="reset-a-peering"></a>Opnieuw instellen van een peering

1. Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor meer informatie.

2. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Voer de volgende opdrachten om op te halen van uw ExpressRoute-circuit.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. Identificeer de peering die u wilt in- of uitschakelen. *Peerings* is een matrix. In het volgende voorbeeld is Peerings [0] persoonlijke Azure-Peering en Microsoft-Peering Peerings [1].

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. Voer de volgende opdrachten de status van de peering wijzigen.

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
De peering moet zich in een status die u instelt. 

## <a name="next-steps"></a>Volgende stappen
Als u help voor een ExpressRoute-probleem op te lossen, Raadpleeg de volgende artikelen:
* [ExpressRoute-connectiviteit controleren](expressroute-troubleshooting-expressroute-overview.md)
* [Oplossen van problemen met prestaties van het netwerk](expressroute-troubleshooting-network-performance.md)
