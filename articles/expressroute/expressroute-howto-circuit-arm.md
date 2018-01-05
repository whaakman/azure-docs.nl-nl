---
title: 'Maken en een ExpressRoute-circuit wijzigen: PowerShell: Azure Resource Manager | Microsoft Docs'
description: In dit artikel wordt beschreven hoe maken, richten, controleren, bijwerken, verwijderen en een ExpressRoute-circuit inrichting ervan ongedaan.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: ef092a48994b68268109cb98bd6cd4526e259d5b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Maken en een ExpressRoute-circuit met PowerShell wijzigen
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
>

In dit artikel wordt beschreven hoe een Azure ExpressRoute-circuit maken met behulp van PowerShell-cmdlets en de Azure Resource Manager-implementatiemodel. Dit artikel ziet u ook het Controleer de status van het circuit, bijwerken of verwijderen en deze inrichting ervan ongedaan.

## <a name="before-you-begin"></a>Voordat u begint
* Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie voor meer informatie [overzicht van Azure PowerShell](/powershell/azure/overview).
* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.


## <a name="create"></a>Maken en een ExpressRoute-circuit inrichten
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Aanmelden bij uw Azure-account en uw abonnement te selecteren
Om te beginnen met uw configuratie, moet u zich aanmelden bij uw Azure-account. Met de volgende voorbeelden kunt u verbinding kunt maken:

```powershell
Login-AzureRmAccount
```

Controleer de abonnementen voor het account:

```powershell
Get-AzureRmSubscription
```

Selecteer het abonnement dat u wilt maken van een ExpressRoute-circuit voor:

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. De lijst met ondersteunde providers, locaties en bandbreedten ophalen
Voordat u een ExpressRoute-circuit maken, moet u de lijst met ondersteunde connectiviteitsproviders, locaties en bandbreedte-opties.

De PowerShell-cmdlet **Get-AzureRmExpressRouteServiceProvider** retourneert deze informatie, die u in latere stappen:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Controleer of uw connectiviteitsprovider er wordt weergegeven. Noteer de volgende informatie, die u later nodig hebt bij het maken van een circuit:

* Naam
* PeeringLocations
* BandwidthsOffered

U kunt nu gereed voor het maken van een ExpressRoute-circuit.   

### <a name="3-create-an-expressroute-circuit"></a>3. Een ExpressRoute-circuit maken
Als u nog een resourcegroep hebt, moet u een maken voordat u uw ExpressRoute-circuit maken. U kunt dit doen door de volgende opdracht uit te voeren:

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


Het volgende voorbeeld ziet hoe u een 200-Mbps ExpressRoute-circuit via Equinix in Silicon Valley maakt. Als u een andere provider en andere instellingen gebruikt, vervangt u die informatie door wanneer u uw aanvraag. Gebruik het volgende voorbeeld om aan te vragen van een nieuwe servicesleutel:

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Zorg ervoor dat u de juiste SKU-laag en de SKU-serie opgeven:

* SKU-laag bepaalt of een standaard ExpressRoute of een premium-invoegtoepassing voor ExpressRoute is ingeschakeld. U kunt opgeven *standaard* standaard SKU ophalen of *Premium* voor de premium-invoegtoepassing.
* SKU-serie, bepaalt de facturering. U kunt opgeven *Metereddata* voor een plan datalimiet en *Unlimiteddata* voor een onbeperkte gegevens-plan. Kunt u het type van de facturering van *Metereddata* naar *Unlimiteddata*, maar niet het type van *Unlimiteddata* naar *Metereddata*.

> [!IMPORTANT]
> Uw ExpressRoute-circuit wordt gefactureerd vanaf het moment dat de sleutel van een service wordt verleend. Zorg ervoor dat u deze bewerking niet uitvoeren wanneer de connectiviteitsprovider gereed is voor het inrichten van het circuit.
> 
> 

Het antwoord bevat de sleutel van de service. U kunt gedetailleerde beschrijvingen van alle parameters krijgen met de volgende opdracht:

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Lijst van alle ExpressRoute-circuits
Als u een lijst met alle ExpressRoute-circuits die u hebt gemaakt, voer de **Get-AzureRmExpressRouteCircuit** opdracht:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Het antwoord lijkt op het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

U kunt deze informatie op elk gewenst moment ophalen met behulp van de `Get-AzureRmExpressRouteCircuit` cmdlet. De oproep zonder parameters geeft een lijst van alle circuits. De sleutel van uw service wordt vermeld in de *ServiceKey* veld:

```powershell
Get-AzureRmExpressRouteCircuit
```


Het antwoord lijkt op het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


U kunt gedetailleerde beschrijvingen van alle parameters krijgen met de volgende opdracht:

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. De servicesleutel verzenden naar uw connectiviteitsprovider voor inrichting
*ServiceProviderProvisioningState* bevat informatie over de huidige status van inrichting aan de kant van de serviceprovider. De status van de biedt status aan de kant van Microsoft. Zie voor meer informatie over het circuit inrichtingstatuswaarden [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Wanneer u een nieuwe ExpressRoute-circuit maakt, wordt het circuit in de volgende status is:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Het circuit wordt gewijzigd in de volgende status wanneer de connectiviteitsprovider wordt deze voor u inschakelen:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Voordat u kunt een ExpressRoute-circuit gebruiken, moet deze de status van de volgende zijn:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Controleer regelmatig de status en de status van de sleutel van het circuit
Controleren of de status en de status van de sleutel van het circuit, laat u weten wanneer uw provider uw circuit is ingeschakeld. Na de configuratie van het circuit *ServiceProviderProvisioningState* wordt weergegeven als *ingericht*, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Het antwoord lijkt op het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
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
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Maken van uw configuratie van de routering
Zie voor stapsgewijze instructies de [ExpressRoute-circuit routeringsconfiguratie](expressroute-howto-routing-arm.md) artikel maken en wijzigen van circuit peerings.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services op laag 2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IP VPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Een virtueel netwerk koppelen aan een ExpressRoute-circuit
Vervolgens moet u een virtueel netwerk koppelen aan uw ExpressRoute-circuit. Gebruik de [virtuele netwerken koppelen aan ExpressRoute-circuits](expressroute-howto-linkvnet-arm.md) artikel als u met het implementatiemodel van Resource Manager werkt.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Opvragen van de status van een ExpressRoute-circuit
U kunt deze informatie op elk gewenst moment ophalen met behulp van de **Get-AzureRmExpressRouteCircuit** cmdlet. De oproep zonder parameters geeft een lijst van alle circuits.

```powershell
Get-AzureRmExpressRouteCircuit
```


Het antwoord is vergelijkbaar met het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
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
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


U kunt informatie over een specifieke ExpressRoute-circuit krijgen door de naam van een resourcegroep en de naam van het circuit doorgegeven als parameter voor de aanroep:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Het antwoord lijkt op het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
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
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


U kunt gedetailleerde beschrijvingen van alle parameters krijgen met de volgende opdracht:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Wijzigen van een ExpressRoute-circuit
U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder enige impact op connectiviteit.

U kunt de volgende taken zonder uitvaltijd doen:

* In- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor ExpressRoute-circuit.
* De bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. De bandbreedte van een circuit downgraden wordt niet ondersteund. 
* Wijzig het softwarelicentiecontrole plan van Datalimiet gegevens in onbeperkte gegevens. Het plan softwarelicentiecontrole wijzigen van onbeperkt naar Datalimiet gegevens wordt niet ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

Zie voor meer informatie over limieten en beperkingen de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium inschakelen
U kunt de invoegtoepassing ExpressRoute premium inschakelen voor uw bestaande circuit met behulp van de volgende PowerShell-fragment:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Het circuit heeft nu de ExpressRoute premium-invoegtoepassing voor functies ingeschakeld. We beginnen facturering voor de premium-invoegtoepassing mogelijkheid zodra de opdracht met succes is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium uitschakelen
> [!IMPORTANT]
> Als u resources die groter zijn dan wat voor het standaard circuit is toegestaan, kan deze bewerking mislukken.
> 
> 

Houd rekening met de volgende informatie:

* Voordat u van premium op standaard downgraden, moet u ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan het circuit kleiner dan 10 is. Als u dat niet uw aanvraag bijwerken mislukt en er kosten in rekening brengen volgens de premietarieven voor.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u dit doet, mislukt de updateaanvraag en we u volgens de premietarieven voor rekening.
* De routetabel moet minder dan 4000 routes voor persoonlijke peering. Als uw tabel route groter dan 4000 routes is, wordt de BGP-sessie zakt en pas het aantal geadverteerde voorvoegsels daaronder 4.000 won't worden ingeschakeld.

U kunt de invoegtoepassing ExpressRoute premium voor een bestaand circuit uitschakelen met behulp van de volgende PowerShell-cmdlet:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Bijwerken van de bandbreedte van het ExpressRoute-circuit
Ondersteunde bandbreedte-opties voor uw provider, Controleer of de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md). U kunt de grootte die groter zijn dan de grootte van uw bestaande circuit verzamelen.

> [!IMPORTANT]
> U moet de ExpressRoute-circuit opnieuw maken als er onvoldoende capaciteit op de bestaande poort. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Downgraden bandbreedte, moet u het ExpressRoute-circuit inrichting ervan ongedaan en vervolgens opnieuw inrichten van nieuwe ExpressRoute-circuit.
> 

Nadat u welke grootte die u nodig hebt besluit, gebruikt u de volgende opdracht om het formaat van uw circuit te:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Uw circuit wordt bepaald van de Microsoft-zijde. U kunt vervolgens moet contact op met uw connectiviteitsprovider om bij te werken-configuraties van hun kant overeenkomen met deze wijziging. Nadat u deze melding, beginnen wij u voor de optie bijgewerkte bandbreedte facturering.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Als u wilt verplaatsen van de SKU van datalimiet op onbeperkt
U kunt de SKU van een ExpressRoute-circuit wijzigen met behulp van de volgende PowerShell-fragment:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Toegang tot het klassieke en het Resource Manager-omgevingen
Lees de instructies in [verplaatsen ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](expressroute-howto-move-arm.md).  

## <a name="delete"></a>Opheffen van inrichting en een ExpressRoute-circuit verwijderen
Houd rekening met de volgende informatie:

* U moet alle virtuele netwerken van het ExpressRoute-circuit ontkoppelen. Als deze bewerking is mislukt, controleert u of er geen virtuele netwerken zijn gekoppeld aan het circuit.
* Als de ExpressRoute-circuit serviceprovider Inrichtingsstatus **inrichten** of **ingericht** moet u werken met uw serviceprovider voor inrichting ervan ongedaan maakt het circuit op hun kant. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid opheffen van inrichting het circuit en waarschuwt ons.
* Als de provider heeft het circuit deprovisioned (de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**), kunt u het circuit verwijderen. Hierdoor wordt voorkomen dat facturering voor het circuit.

U kunt uw ExpressRoute-circuit verwijderen door de volgende opdracht uit te voeren:

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Volgende stappen

Nadat u het circuit hebt gemaakt, zorg dat u de volgende stappen voor het volgende doen:

* [Maken en aanpassen van routering voor ExpressRoute-circuit](expressroute-howto-routing-arm.md)
* [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)