---
title: Configureren van ExpressRoute Direct - Azure | Microsoft Docs
description: Deze pagina kunt u ExpressRoute Direct configureren.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: 9157466f0768b052f55f6d225e72ae7a036f8b9c
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822169"
---
# <a name="how-to-configure-expressroute-direct"></a>ExpressRoute Direct configureren

ExpressRoute Direct biedt de mogelijkheid om rechtstreeks in het wereldwijde netwerk van Microsoft op peeringlocaties strategisch verdeeld over de hele wereld verbinding te maken. Zie voor meer informatie, [over ExpressRoute rechtstreeks verbinding maken met](expressroute-erdirect-about.md).

## <a name="resources"></a>Maken van de resource

1. Aanmelden bij Azure en selecteer het abonnement. De resource Direct van ExpressRoute en ExpressRoute-circuits moeten zich in hetzelfde abonnement.

  ```powershell
  Connect-AzureRMAccount 

  Select-AzureRMSubscription -Subscription “<SubscriptionID or SubscriptionName>”
  ```
2. Lijst met alle locaties waar ExpressRoute Direct wordt ondersteund.
  
  ```powershell
  Get-AzureRmExpressRoutePortsLocation
  ```

  **Voorbeeld van uitvoer**
  
  ```powershell
  Name                : Equinix-Ashburn-DC2
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-D
                        C2
  ProvisioningState   : Succeeded
  Address             : 21715 Filigree Court, DC2, Building F, Ashburn, VA 20147
  Contact             : support@equinix.com
  AvailableBandwidths : []

  Name                : Equinix-Dallas-DA3
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA
                        3
  ProvisioningState   : Succeeded
  Address             : 1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207
  Contact             : support@equinix.com
  AvailableBandwidths : []

  Name                : Equinix-San-Jose-SV1
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
  ProvisioningState   : Succeeded
  Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
  Contact             : support@equinix.com
  AvailableBandwidths : []
  ```
3. Bepalen of een locatie die hierboven vermeld beschikbare bandbreedte heeft

  ```powershell
  Get-AzureRmExpressRoutePortsLocation -LocationName "Equinix-San-Jose-SV1"
  ```

  **Voorbeeld van uitvoer**

  ```powershell
  Name                : Equinix-San-Jose-SV1
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
  ProvisioningState   : Succeeded
  Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
  Contact             : support@equinix.com
  AvailableBandwidths : [
                          {
                            "OfferName": "100 Gbps",
                            "ValueInGbps": 100
                          }
                        ]
  ```
4. Maak een ExpressRoute-Direct-resource op basis van de hierboven gekozen locatie

  ExpressRoute Direct biedt ondersteuning voor zowel QinQ en Dot1Q inkapseling. Als QinQ is ingeschakeld, wordt elk ExpressRoute-circuit wordt dynamisch toegewezen een S-Tag en is uniek in de resource ExpressRoute Direct. Elke C-code op het circuit moet uniek zijn voor het circuit, maar ook niet voor de ExpressRoute-Direct.  

  Als Dot1Q inkapseling is ingeschakeld, moet u uniekheid van de C-code (VLAN) in de hele ExpressRoute Direct resourcegroepen beheren.  

  > [!IMPORTANT]
  > ExpressRoute Direct mag alleen bestaan uit één encapsulation-type. Inkapseling kan niet worden gewijzigd nadat de ExpressRoute Direct is gemaakt.
  > 
 
  ```powershell 
  $ERDirect = New-AzureRMExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName -PeeringLocation $PeeringLocationName -BandwidthInGbps 100.0 -Encapsulation QinQ | Dot1Q -Location $AzureRegion
  ```

  > [!NOTE]
  > Het kenmerk inkapseling kan ook worden ingesteld op Dot1Q. 
  >

  **Voorbeeld van uitvoer:**

  ```powershell
  Name                       : Contoso-Direct
  ResourceGroupName          : Contoso-Direct-rg
  Location                   : westcentralus
  Id                         : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                               ressRoutePorts/Contoso-Direct
  Etag                       : W/"<etagnumber> "
  ResourceGuid               : <number>
  ProvisioningState          : Succeeded
  PeeringLocation            : Equinix-Seattle-SE2
  BandwidthInGbps            : 100
  ProvisionedBandwidthInGbps : 0
  Encapsulation              : QinQ
  Mtu                        : 1500
  EtherType                  : 0x8100
  AllocationDate             : Saturday, September 1, 2018
  Links                      : [
                                 {
                                   "Name": "link1",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link1",
                                   "RouterName": "tst-09xgmr-cis-1",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "link2",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link2",
                                   "RouterName": "tst-09xgmr-cis-2",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
  Circuits                   : []
  ```

## <a name="state"></a>Status van beheerder wijzigen van koppelingen

  Dit proces moet worden gebruikt voor het uitvoeren van een laag 1-test, ervoor zorgen dat elke cross-verbinding juist patches naar elke router voor primaire en secundaire.
1. ExpressRoute Direct details ophalen.

  ```powershell
  $ERDirect = Get-AzureRmExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
  ```
2. Koppeling naar ingeschakelde instellen. Herhaal deze stap voor het instellen van elke koppeling ingeschakeld.

  Koppelingen [0] is de primaire-poort en koppelingen [1] is de secundaire-poort.

  ```powershell
  $ERDirect.Links[0].AdminState = “Enabled”
  Set-AzureRmExpressRoutePort -ExpressRoutePort $ERDirect
  $ERDirect = Get-AzureRmExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
  $ERDirect.Links[1].AdminState = “Enabled”
  Set-AzureRmExpressRoutePort -ExpressRoutePort $ERDirect
  ```
  **Voorbeeld van uitvoer:**

  ```powershell
  Name                       : Contoso-Direct
  ResourceGroupName          : Contoso-Direct-rg
  Location                   : westcentralus
  Id                         : /subscriptions/<number>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                             ressRoutePorts/Contoso-Direct
  Etag                       : W/"<etagnumber> "
  ResourceGuid               : <number>
  ProvisioningState          : Succeeded
  PeeringLocation            : Equinix-Seattle-SE2
  BandwidthInGbps            : 100
  ProvisionedBandwidthInGbps : 0
  Encapsulation              : QinQ
  Mtu                        : 1500
  EtherType                  : 0x8100
  AllocationDate             : Saturday, September 1, 2018
  Links                      : [
                               {
                                 "Name": "link1",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link1",
                                 "RouterName": "tst-09xgmr-cis-1",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               },
                               {
                                 "Name": "link2",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link2",
                                 "RouterName": "tst-09xgmr-cis-2",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               }
                             ]
  Circuits                   : []
  ```

  Gebruik dezelfde procedure met `AdminState = “Disabled”` om in te schakelen de poorten.

## <a name="circuit"></a>Een circuit maken

U kunt standaard 10 circuits maken in het abonnement waarin de resource ExpressRoute Direct is. Dit kan worden verhoogd met ondersteuning. U bent verantwoordelijk voor het bijhouden van zowel ingericht en bandbreedte gebruikt. Ingerichte bandbreedte is de som van de bandbreedte van alle op de resource ExpressRoute Direct-circuits en gebruikte bandbreedte is het fysieke gebruik van de onderliggende fysieke interfaces.

Er zijn aanvullende circuit bandbreedten die kunnen worden gebruikt op ExpressRoute Direct alleen ter ondersteuning van de scenario's die hierboven worden beschreven. Dit zijn: 40Gbps en 100Gbps.

Standard- of premium circuits kunnen worden gemaakt. Standard-circuits zijn opgenomen in de kosten, terwijl premium-circuits kosten verbonden op basis van de bandbreedte die is geselecteerd zijn. Circuits kunnen alleen worden gemaakt als de verbindingen naar gebruik, als onbeperkte wordt niet ondersteund voor ExpressRoute Direct.

Een circuit maken op de resource ExpressRoute Direct.

  ```powershell
  New-AzureRmExpressRouteCircuit -Name $Name -ResourceGroupName $ResourceGroupName -ExpressRoutePort $ERDirect -BandwidthinGbps 100.0  -Location $AzureRegion -SkuTier Premium -SkuFamily MeteredData 
  ```

  Andere bandbreedten zijn onder andere: 5.0, 10.0 en 40,0

  **Voorbeeld van uitvoer:**

  ```powershell
  Name                             : ExpressRoute-Direct-ckt
  ResourceGroupName                : Contoso-Direct-rg
  Location                         : westcentralus
  Id                               : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Netwo
                                   rk/expressRouteCircuits/ExpressRoute-Direct-ckt
  Etag                             : W/"<etagnumber>"
  ProvisioningState                : Succeeded
  Sku                              : {
                                     "Name": "Premium_MeteredData",
                                     "Tier": "Premium",
                                     "Family": "MeteredData"
                                   }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
    ServiceProviderProperties        : null
  ExpressRoutePort                 : {
                                     "Id": "/subscriptions/<subscriptionID>n/resourceGroups/Contoso-Direct-rg/providers/Micros
                                   oft.Network/expressRoutePorts/Contoso-Direct"
                                   }
  BandwidthInGbps                  : 10
  Stag                             : 2
  ServiceKey                       : <number>
  Peerings                         : []
  Authorizations                   : []
  AllowClassicOperations           : False
  GatewayManagerEtag     
  ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over ExpressRoute Direct de [overzicht](expressroute-erdirect-about.md).
