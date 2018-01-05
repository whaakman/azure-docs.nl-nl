---
title: 'Het configureren van routering (peering) voor een ExpressRoute-circuit: Azure: klassieke | Microsoft Docs'
description: Dit artikel begeleidt u stapsgewijs door de procedure voor het maken en inrichten van de persoonlijke, openbare en Microsoft-peering van een ExpressRoute-circuit. In dit artikel leest u hoe u de status controleert en peerings voor uw circuit bijwerkt of verwijdert.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 37713db70f3ae837edafc997b78b16b121d0a885
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Maken en wijzigen van de peering voor een ExpressRoute-circuit (klassiek)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure-CLI](howto-routing-cli.md)
> * [Video - persoonlijke peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - openbare peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-routing-classic.md)
> 

Dit artikel begeleidt u bij de stappen voor het maken en beheren van routeringsconfiguratie voor een ExpressRoute-circuit met PowerShell en het klassieke implementatiemodel. In de volgende stappen ziet u ook hoe u de status van een peering voor een ExpressRoute-circuit controleert en peerings bijwerkt of verwijdert en de inrichting ervan ongedaan maakt.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Configuratievereisten
* U moet de meest recente versie van de Azure Service Management (SM) PowerShell-cmdlets. Zie voor meer informatie [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview).  
* Zorg dat u de pagina met [vereisten](expressroute-prerequisites.md), de pagina over [routeringsvereisten](expressroute-routing.md) en de pagina over [werkstromen](expressroute-workflows.md) hebt gelezen voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zijn ingericht en zijn ingeschakeld om de hieronder beschreven cmdlets te kunnen uitvoeren.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services met Laag-2-connectiviteit aanbieden. Als u gebruikmaakt van een serviceprovider die beheerde Laag-3-services aanbiedt (meestal een IPVPN, zoals MPLS), zal de connectiviteitsprovider routering voor u configureren en beheren.
> 
> 

U kunt een, twee of alle drie de peerings (Azure privé, Azure openbaar en Microsoft) voor een ExpressRoute-circuit configureren. U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit.


### <a name="log-in-to-your-azure-account-and-select-a-subscription"></a>Aanmelden bij uw Azure-account en een abonnement selecteren
1. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

        Login-AzureRmAccount

2. Controleer de abonnementen voor het account.

        Get-AzureRmSubscription

3. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Gebruik vervolgens de volgende cmdlet als u uw Azure-abonnement toevoegen aan PowerShell voor het klassieke implementatiemodel.

        Add-AzureAccount


## <a name="azure-private-peering"></a>Persoonlijke Azure-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een persoonlijke Azure-peering voor een ExpressRoute-circuit. 

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken
1. **Importeer de PowerShell-module voor ExpressRoute.**
   
    U moet de Azure en een ExpressRoute-modules importeren in de PowerShell-sessie om te beginnen met behulp van de ExpressRoute-cmdlets. Voer de volgende opdrachten om te importeren van de Azure en een ExpressRoute-modules in de PowerShell-sessie.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Maak een ExpressRoute-circuit.**
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt. 
3. **Controleer de ExpressRoute-circuit is ingericht.**
   
    Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld. Zie het voorbeeld hieronder.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Zorg ervoor dat het circuit ingericht en ingeschakeld bevat. Als dit niet het geval is, werkt u met uw connectiviteitsprovider uw circuit met de vereiste toestand en status ophalen.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Configureer persoonlijke Azure-peering voor het circuit.**
   
    Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:
   
   * Een /30-subnet voor de primaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
   * Een /30-subnet voor de secundaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijk AS-nummer voor deze peering gebruiken. Zorg dat u niet 65515 gebruikt.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel**.
     
    U kunt de volgende cmdlet uitvoeren om persoonlijke Azure-peering voor uw circuit te configureren.
     
        Nieuwe AzureBGPPeering - AccessType persoonlijke - ServiceKey ' *** ' - PrimaryPeerSubnet '10.0.0.0/30' - SecondaryPeerSubnet '10.0.0.4/30' - PeerAsn 1234 - VlanId 100
     
    U kunt onderstaande cmdlet gebruiken als u een MD5-hash wilt gebruiken.
     
        Nieuwe AzureBGPPeering - AccessType persoonlijke - ServiceKey ' *** ' - PrimaryPeerSubnet '10.0.0.0/30' - SecondaryPeerSubnet '10.0.0.4/30' - PeerAsn 1234 - VlanId 100 - SharedKey 'A1B2C3D4'
     
     > [!IMPORTANT]
     > Zorg dat u uw AS-nummer als peering-ASN opgeeft, niet als klant-ASN.
     > 
     > 

### <a name="to-view-azure-private-peering-details"></a>De details van persoonlijke Azure-peering weergeven
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>De configuratie van persoonlijke Azure-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken. In het volgende voorbeeld wordt de VLAN-id van het circuit gewijzigd van 100 in 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Persoonlijke Azure-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren.

> [!WARNING]
> Zorg ervoor dat alle virtuele netwerken zijn losgekoppeld van het ExpressRoute-circuit voordat u deze cmdlet uitvoert. 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Openbare Azure-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een openbare Azure-peering voor een ExpressRoute-circuit.

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken
1. **Importeer de PowerShell-module voor ExpressRoute.**
   
    U moet de Azure en een ExpressRoute-modules importeren in de PowerShell-sessie om te beginnen met behulp van de ExpressRoute-cmdlets. Voer de volgende opdrachten om te importeren van de Azure en een ExpressRoute-modules in de PowerShell-sessie. 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Een ExpressRoute-circuit maken**
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om openbare Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
3. **Controleer of ExpressRoute-circuit is ingericht**
   
    Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld. Zie het voorbeeld hieronder.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Zorg ervoor dat het circuit ingericht en ingeschakeld bevat. Als dit niet het geval is, werkt u met uw connectiviteitsprovider uw circuit met de vereiste toestand en status ophalen.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Configureer openbare Azure-peering voor het circuit**
   
    Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.
   
   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel**.
     
    U kunt de volgende cmdlet uitvoeren om openbare Azure-peering voor uw circuit te configureren.
     
        Nieuwe AzureBGPPeering - AccessType openbare - ServiceKey ' *** ' - PrimaryPeerSubnet '131.107.0.0/30' - SecondaryPeerSubnet '131.107.0.4/30' - PeerAsn 1234 - VlanId 200
     
    U kunt onderstaande cmdlet gebruiken als u een MD5-hash wilt gebruiken
     
        Nieuwe AzureBGPPeering - AccessType openbare - ServiceKey ' *** ' - PrimaryPeerSubnet '131.107.0.0/30' - SecondaryPeerSubnet '131.107.0.4/30' - PeerAsn 1234 - VlanId 200 - SharedKey 'A1B2C3D4'
     
     > [!IMPORTANT]
     > Zorg dat u uw AS-nummer als peering-ASN opgeeft en niet als klant-ASN.
     > 
     > 

### <a name="to-view-azure-public-peering-details"></a>De details van openbare Azure-peering weergeven
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>De configuratie van openbare Azure-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

In het bovenstaande voorbeeld wordt de VLAN-id van het circuit gewijzigd van 200 in 600.

### <a name="to-delete-azure-public-peering"></a>Openbare Azure-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft-peering
In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een Microsoft-peering voor een ExpressRoute-circuit. 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken
1. **Importeer de PowerShell-module voor ExpressRoute.**
   
    U moet de Azure en een ExpressRoute-modules importeren in de PowerShell-sessie om te beginnen met behulp van de ExpressRoute-cmdlets. Voer de volgende opdrachten om te importeren van de Azure en een ExpressRoute-modules in de PowerShell-sessie.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Een ExpressRoute-circuit maken**
   
    Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
3. **Controleer of ExpressRoute-circuit is ingericht**
   
    Eerst moet u controleren of het ExpressRoute-circuit is ingericht en ingeschakeld status.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Zorg ervoor dat het circuit ingericht en ingeschakeld bevat. Als dit niet het geval is, werkt u met uw connectiviteitsprovider uw circuit met de vereiste toestand en status ophalen.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Configureer Microsoft-peering voor het circuit**
   
    Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.
   
   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Geadverteerde voorvoegsels: u moet een lijst verstrekken van alle voorvoegsels die u via de BGP-sessie wilt adverteren. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. U kunt een met komma's gescheiden lijst verzenden als u een reeks voorvoegsels wilt verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
   * Klant-ASN: als u voorvoegsels adverteert die niet zijn geregistreerd op het AS-nummer van de peering, kunt u het AS-nummer opgeven waarop ze zijn geregistreerd. **Dit is optioneel**.
   * Naam van routeringsregister: u kunt het RIR/IRR opgeven waarbij het AS-nummer en de voorvoegsels zijn geregistreerd.
   * Een MD5-hash, als u er een wilt gebruiken. **Dit is optioneel.**
     
    U kunt de volgende cmdlet als u wilt Microsoft pering voor uw circuit te configureren uitvoeren
     
        Nieuwe AzureBGPPeering - AccessType Microsoft - ServiceKey ' *** ' - PrimaryPeerSubnet '131.107.0.0/30' - SecondaryPeerSubnet '131.107.0.4/30' - VlanId 300 - PeerAsn 1234 - CustomerAsn. 2245 - AdvertisedPublicPrefixes '123.0.0.0/30' - RoutingRegistryName 'ARIN' - SharedKey 'A1B2C3D4'

### <a name="to-view-microsoft-peering-details"></a>De details van Microsoft-peering weergeven
Voer de volgende cmdlet uit als u de configuratiegegevens wilt weergeven.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Configuratie van Microsoft-peering bijwerken
Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken.

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft-peering verwijderen
U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Volgende stappen
Vervolgens [een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md).

* Zie voor meer informatie over werkstromen [ExpressRoute-werkstromen](expressroute-workflows.md).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).

