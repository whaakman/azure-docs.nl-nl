---
title: Routering configureren voor een ExpressRoute-circuit | Microsoft Docs
description: Dit artikel begeleidt u stapsgewijs door de procedure voor het maken en inrichten van de persoonlijke, openbare en Microsoft-peering van een ExpressRoute-circuit. In dit artikel leest u hoe u de status controleert en peerings voor uw circuit bijwerkt of verwijdert.
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/05/2016
ms.author: ganesr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 45c0646f6eb1067f49bc185f1592cd1c94fc9470


---
# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Routering voor een ExpressRoute-circuit maken en wijzigen
> [!div class="op_single_selector"]
> [Azure Portal - Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
> [PowerShell - Resource Manager](expressroute-howto-routing-arm.md)
> [PowerShell - Klassiek](expressroute-howto-routing-classic.md)
> 
> 

In dit artikel worden de stappen besproken voor het maken en beheren van een routeringsconfiguratie voor een ExpressRoute-circuit met PowerShell en het Azure Resource Manager-implementatiemodel.  In de volgende stappen ziet u ook hoe u de status van een peering voor een ExpressRoute-circuit controleert en peerings bijwerkt of verwijdert en de inrichting ervan ongedaan maakt. 

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Configuratievereisten
* U hebt de nieuwste versie van de Azure PowerShell-modules, versie 1.0 of hoger, nodig. 
* Zorg dat u de pagina met [vereisten](expressroute-prerequisites.md), de pagina over [routeringsvereisten](expressroute-routing.md) en de pagina over [werkstromen](expressroute-workflows.md) hebt gelezen voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zijn ingericht en zijn ingeschakeld om de hieronder beschreven cmdlets te kunnen uitvoeren.

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services met Laag-2-connectiviteit aanbieden. Als u gebruikmaakt van een serviceprovider die beheerde Laag-3-services aanbiedt (meestal een IPVPN, zoals MPLS), zal de connectiviteitsprovider routering voor u configureren en beheren.

> [!IMPORTANT]
> Op dit moment bieden we nog geen peerings aan die door serviceproviders worden geconfigureerd via de beheerportal van de service. Deze mogelijkheid zal binnenkort worden ingeschakeld. Neem contact op met uw serviceprovider voordat u BGP-peerings configureert.
> 
> 

U kunt een, twee of alle drie de peerings (Azure privé, Azure openbaar en Microsoft) voor een ExpressRoute-circuit configureren. U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit. 

## <a name="azure-private-peering"></a>Persoonlijke Azure-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een persoonlijke Azure-peering voor een ExpressRoute-circuit. 

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken
1. Importeer de PowerShell-module voor ExpressRoute.
   
     U moet het meest recente installatieprogramma installeren vanuit de [PowerShell Gallery](http://www.powershellgallery.com/) en de Azure Resource Manager-modules importeren in de PowerShell-sessie om de ExpressRoute-cmdlets te kunnen gebruiken. U moet PowerShell tevens uitvoeren als beheerder.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Alle AzureRM.*-modules binnen het bereik van de bekende semantische versie importeren
   
        Import-AzureRM
   
    U kunt ook gewoon een bepaalde module binnen het bereik van de bekende semantische versie importeren 
   
        Import-Module AzureRM.Network 
   
    Aanmelden bij uw account
   
        Login-AzureRmAccount
   
    Het abonnement selecteren waarmee u een ExpressRoute-circuit wilt maken
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Maak een ExpressRoute-circuit.
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inrichten door de connectiviteitsprovider. 
   
    Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt. 
3. Controleer of het ExpressRoute-circuit is ingericht.
   
    Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld. Zie het voorbeeld hieronder.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Het antwoord ziet er ongeveer uit als in het voorbeeld hieronder:
   
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
4. Configureer persoonlijke Azure-peering voor het circuit.
   
    Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:
   
   * Een /30-subnet voor de primaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
   * Een /30-subnet voor de secundaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijk AS-nummer voor deze peering gebruiken. Zorg dat u niet 65515 gebruikt.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel**.
     
     U kunt de volgende cmdlet uitvoeren om persoonlijke Azure-peering voor uw circuit te configureren.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     U kunt onderstaande cmdlet gebruiken als u een MD5-hash wilt gebruiken.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200 -SharedKey "A1B2C3D4"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     > [!IMPORTANT]
     > Zorg dat u uw AS-nummer als peering-ASN opgeeft, niet als klant-ASN.
     > 
     > 

### <a name="to-view-azure-private-peering-details"></a>De details van persoonlijke Azure-peering weergeven
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt    


### <a name="to-update-azure-private-peering-configuration"></a>De configuratie van persoonlijke Azure-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken. In het volgende voorbeeld wordt de VLAN-id van het circuit gewijzigd van 100 in 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Persoonlijke Azure-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren.

> [!WARNING]
> Zorg ervoor dat alle virtuele netwerken zijn losgekoppeld van het ExpressRoute-circuit voordat u deze cmdlet uitvoert. 
> 
> 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Openbare Azure-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een openbare Azure-peering voor een ExpressRoute-circuit.

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken
1. Importeer de PowerShell-module voor ExpressRoute.
   
     U moet het meest recente installatieprogramma installeren vanuit de [PowerShell Gallery](http://www.powershellgallery.com/) en de Azure Resource Manager-modules importeren in de PowerShell-sessie om de ExpressRoute-cmdlets te kunnen gebruiken. U moet PowerShell tevens uitvoeren als beheerder.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Alle AzureRM.*-modules binnen het bereik van de bekende semantische versie importeren
   
        Import-AzureRM
   
    U kunt ook gewoon een bepaalde module binnen het bereik van de bekende semantische versie importeren 
   
        Import-Module AzureRM.Network 
   
    Aanmelden bij uw account
   
        Login-AzureRmAccount
   
    Het abonnement selecteren waarmee u een ExpressRoute-circuit wilt maken
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Maak een ExpressRoute-circuit.
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inrichten door de connectiviteitsprovider. 
   
    Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om openbare Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
3. Controleer of het ExpressRoute-circuit is ingericht.
   
    Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld. Zie het voorbeeld hieronder.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Het antwoord ziet er ongeveer uit als in het voorbeeld hieronder:
   
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
4. Configureer openbare Azure-peering voor het circuit.
   
    Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.
   
   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel**.
     
     U kunt de volgende cmdlet uitvoeren om openbare Azure-peering voor uw circuit te configureren.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     U kunt onderstaande cmdlet gebruiken als u een MD5-hash wilt gebruiken
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100 -SharedKey "A1B2C3D4"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Zorg dat u uw AS-nummer als peering-ASN opgeeft en niet als klant-ASN.

### <a name="to-view-azure-public-peering-details"></a>De details van openbare Azure-peering weergeven
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>De configuratie van openbare Azure-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

In het bovenstaande voorbeeld wordt de VLAN-id van het circuit gewijzigd van 200 in 600.

### <a name="to-delete-azure-public-peering"></a>Openbare Azure-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een Microsoft-peering voor een ExpressRoute-circuit. 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken
1. Importeer de PowerShell-module voor ExpressRoute.
   
     U moet het meest recente installatieprogramma installeren vanuit de [PowerShell Gallery](http://www.powershellgallery.com/) en de Azure Resource Manager-modules importeren in de PowerShell-sessie om de ExpressRoute-cmdlets te kunnen gebruiken. U moet PowerShell tevens uitvoeren als beheerder.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Alle AzureRM.*-modules binnen het bereik van de bekende semantische versie importeren
   
        Import-AzureRM
   
    U kunt ook gewoon een bepaalde module binnen het bereik van de bekende semantische versie importeren 
   
        Import-Module AzureRM.Network 
   
    Aanmelden bij uw account
   
        Login-AzureRmAccount
   
    Het abonnement selecteren waarmee u een ExpressRoute-circuit wilt maken
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Maak een ExpressRoute-circuit.
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inrichten door de connectiviteitsprovider. 
   
    Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
3. Controleer of het ExpressRoute-circuit is ingericht.
   
    Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld. Zie het voorbeeld hieronder.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Het antwoord ziet er ongeveer uit als in het voorbeeld hieronder:
   
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
4. Configureer Microsoft-peering voor het circuit.
   
    Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.
   
   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Geadverteerde voorvoegsels: u moet een lijst verstrekken van alle voorvoegsels die u via de BGP-sessie wilt adverteren. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. U kunt een met komma's gescheiden lijst verzenden als u een reeks voorvoegsels wilt verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
   * Klant-ASN: als u voorvoegsels adverteert die niet zijn geregistreerd op het AS-nummer van de peering, kunt u het AS-nummer opgeven waarop ze zijn geregistreerd. **Dit is optioneel**.
   * Naam van routeringsregister: u kunt het RIR/IRR opgeven waarbij het AS-nummer en de voorvoegsels zijn geregistreerd.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel.**
     
     U kunt de volgende cmdlet uitvoeren om Microsoft-peering voor uw circuit te configureren.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-get-microsoft-peering-details"></a>De details van Microsoft-peering ophalen
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Configuratie van Microsoft-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-microsoft-peering"></a>Microsoft-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Volgende stappen
Volgende stap, [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md).

* Voor meer informatie over ExpressRoute-werkstromen raadpleegt u [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-werkstromen).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).
* Bekijk het [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtual Network-overzicht) voor meer informatie over het gebruik van virtuele netwerken.




<!--HONumber=Nov16_HO2-->


