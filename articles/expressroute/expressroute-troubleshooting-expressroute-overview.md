---
title: 'Controleren of verbinding: Azure ExpressRoute Troubleshooting Guide | Microsoft Docs'
description: Deze pagina vindt u instructies voor het oplossen van problemen en valideren van de connectiviteit van de end-to-end van een ExpressRoute-circuit.
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: cherylmc
ms.openlocfilehash: e52e53255a1462522f297d8918eb1c347a460f77
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="verifying-expressroute-connectivity"></a>ExpressRoute-verbinding controleren
ExpressRoute, die een uitbreiding is een on-premises netwerk via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider in de Microsoft cloud, omvat de volgende drie afzonderlijke netwerkzones:

-   Klantnetwerk
-   Serviceprovider-netwerk
-   Microsoft-datacentrum

Het doel van dit document is zodat de gebruiker om te bepalen waar u (of zelfs als) een verbindingsprobleem bestaat en in welke zone, waardoor te zoeken naar Help-informatie van het juiste team om het probleem te verhelpen. Als Microsoft ondersteuning nodig is een probleem op te lossen, opent u een ondersteuningsticket met [Microsoft Support][Support].

> [!IMPORTANT]
> Dit document is bedoeld om u te helpen opsporen en oplossen van eenvoudige problemen. Het is niet bedoeld als vervanging voor ondersteuning van Microsoft. Open een ondersteuningsticket met [Microsoft Support] [ Support] als u niet voor het oplossen van het probleem op door middel van de richtlijnen.
>
>

## <a name="overview"></a>Overzicht
Het volgende diagram toont de logische verbindingen van een klantnetwerk met Microsoft-netwerk met behulp van ExpressRoute.
[![1]][1]

De getallen geven in het voorgaande diagram netwerk belangrijke punten. De netwerk-punten zijn vaak in dit artikel verwezen door hun bijbehorende nummers.

Afhankelijk van het ExpressRoute-connectiviteit model (Cloud Exchange CO-locatie, Point-to-Point Ethernet-verbinding of Any-to-any (IPVPN)) mogelijk de netwerk-punten 3 en 4 switches (Layer 2-apparaten). De belangrijkste netwerk punten geïllustreerd zijn als volgt:

1.  Klant compute-apparaat (bijvoorbeeld een server of een PC)
2.  CEs: Klant-randrouters 
3.  PEs (CE facing): Provider rand routers/schakelaars waarmee worden geconfronteerd randrouters klant. Genoemd PE CEs in dit document.
4.  PEs (MSEE facing): Provider rand routers/schakelaars waarmee worden geconfronteerd msee's. Genoemd PE-msee's in dit document.
5.  Msee's: Microsoft Enterprise Edge (MSEE) ExpressRoute-routers
6.  De Gateway virtuele netwerk (VNet)
7.  COMPUTE-apparaat op het Azure VNet

Als u de connectiviteitsmodellen Cloud Exchange CO-locatie of Point-to-Point Ethernet-verbinding gebruikt, zou de router van de klant rand (2) tot stand brengen BGP-peer met msee's (5). Netwerk punten 3 en 4 wordt nog steeds aanwezig zijn, maar enigszins transparant als Layer 2-apparaten.

Als het model van de connectiviteit Any-to-any (IPVPN) wordt gebruikt, de PEs (MSEE facing) (4) zou tot stand brengen BGP-peer met msee's (5). Routes wilt vervolgens terug naar de klantnetwerk via de IPVPN serviceprovider-netwerk doorgeven.

>[!NOTE]
>Microsoft vereist voor ExpressRoute hoge beschikbaarheid, een paar redundante BGP-sessies tussen de msee's (5) en PE-msee's (4). Een paar redundante netwerkpaden wordt ook aangemoedigd tussen klantnetwerk en PE CEs. In Any-to-any (IPVPN) verbinding model mogen echter een enkel CE-apparaat (2) worden verbonden met een of meer PEs (3).
>
>

Voor het valideren van een ExpressRoute-circuit, worden de volgende stappen uit (met het netwerkpunt aangegeven door het bijbehorende nummer) behandeld:
1. [Valideren van circuitinrichting en status (5)](#validate-circuit-provisioning-and-state)
2. [Valideren van ten minste één ExpressRoute-peering is geconfigureerd (5)](#validate-peering-configuration)
3. [Valideren van ARP tussen Microsoft en de service provider (koppeling tussen 4 en 5)](#validate-arp-between-microsoft-and-the-service-provider)
4. [Valideren van BGP en routes op de MSEE (BGP tussen 4 en 5 en 5 tot en met 6 als een VNet is verbonden)](#validate-bgp-and-routes-on-the-msee)
5. [Controleer de statistieken verkeer (verkeer doorlopen 5)](#check-the-traffic-statistics)

Meer validaties en controles worden toegevoegd in de toekomst, controle terug maandelijks!

##<a name="validate-circuit-provisioning-and-state"></a>Valideren van circuitinrichting en status
Ongeacht het model connectiviteit een ExpressRoute-circuit moet worden gemaakt en dus een servicesleutel gegenereerd voor het inrichten van het circuit. Een ExpressRoute-circuit inrichten, stelt u een redundante laag-2-verbindingen tussen PE-msee's (4) en de msee's (5). Zie het artikel voor meer informatie over het maken, wijzigen, inrichten en een ExpressRoute-circuit controleren [maken en een ExpressRoute-circuit wijzigen][CreateCircuit].

>[!TIP]
>Een servicesleutel is een unieke identificatie voor een ExpressRoute-circuit. Deze sleutel is vereist voor het merendeel van de powershell-opdrachten die in dit document worden vermeld. Ook nodig u hulp hebt van Microsoft of van een ExpressRoute-partner voor een ExpressRoute-probleem kunt oplossen, geeft u de sleutel voor het circuit gemakkelijk te identificeren.
>
>

###<a name="verification-via-the-azure-portal"></a>Verificatie via de Azure-portal
In de Azure portal, de status van een ExpressRoute-circuit kan worden gecontroleerd door het selecteren van ![2][2] in het menu links zijmarge en vervolgens te klikken op het ExpressRoute-circuit. Selecteren van een ExpressRoute circuit vermeld onder 'Alle resources' de ExpressRoute-circuit blade geopend. In de ![3][3] sectie van de blade de ExpressRoute essentials worden vermeld, zoals wordt weergegeven in de volgende schermopname:

![4][4]    

In de Essentials ExpressRoute *Circuit status* geeft de status van het circuit aan de kant van Microsoft. *Providerstatus* geeft aan of het circuit is *ingericht/niet ingericht* aan de kant van de serviceprovider. 

Voor een ExpressRoute-circuit operationeel, de *Circuit status* moet *ingeschakeld* en de *providerstatus* moet *ingericht*.

>[!NOTE]
>Als de *Circuit status* is niet ingeschakeld, neem contact op met [Microsoft Support][Support]. Als de *providerstatus* is niet ingericht, neem contact op met uw serviceprovider.
>
>

###<a name="verification-via-powershell"></a>Verificatie via PowerShell
Als de ExpressRoute-circuits in een resourcegroep weergeven, gebruikt u de volgende opdracht:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>U kunt de naam van uw resourcegroep ophalen via de Azure. Zie de vorige subsectie van dit document en controleer of de naam van de resourcegroep wordt vermeld in de schermopname voorbeeld.
>
>

Als u wilt een bepaalde ExpressRoute-circuit in een resourcegroep selecteren, moet u de volgende opdracht gebruiken:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Er is een voorbeeldantwoord:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

Om te bevestigen of een ExpressRoute-circuit operationele speciale aandacht besteden aan de volgende velden:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
>Als de *CircuitProvisioningState* is niet ingeschakeld, neem contact op met [Microsoft Support][Support]. Als de *ServiceProviderProvisioningState* is niet ingericht, neem contact op met uw serviceprovider.
>
>

###<a name="verification-via-powershell-classic"></a>Verificatie via PowerShell (klassiek)
Als de ExpressRoute-circuits voor een abonnement weergeven, gebruikt u de volgende opdracht:

    Get-AzureDedicatedCircuit

Selecteer een bepaalde ExpressRoute-circuit, gebruik de volgende opdracht:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

Er is een voorbeeldantwoord:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Om te bevestigen of een ExpressRoute-circuit operationele speciale aandacht besteden aan de volgende velden: ServiceProviderProvisioningState: Status ingericht: ingeschakeld

>[!NOTE]
>Als de *Status* is niet ingeschakeld, neem contact op met [Microsoft Support][Support]. Als de *ServiceProviderProvisioningState* is niet ingericht, neem contact op met uw serviceprovider.
>
>

##<a name="validate-peering-configuration"></a>Peeringconfiguratie valideren
Nadat de serviceprovider is voltooid de inrichting van het ExpressRoute-circuit, kan een routeringsconfiguratie worden gemaakt via de ExpressRoute-circuit tussen de MSEE-PRs (4) en de msee's (5). Elk ExpressRoute-circuit kan hebben een, twee of drie routing-contexten ingeschakeld: Azure persoonlijke peering (verkeer naar persoonlijke virtuele netwerken in Azure), Azure openbare peering (verkeer naar openbare IP-adressen in Azure) en Microsoft peering (verkeer met Office 365 en Dynamics 365). Raadpleeg het artikel voor meer informatie over het maken en beheren van routeringsconfiguratie [maken en wijzigen van routering voor een ExpressRoute-circuit][CreatePeering].

###<a name="verification-via-the-azure-portal"></a>Verificatie via de Azure-portal

>[!NOTE]
>Als laag 3 wordt geleverd door de provider en de peerings leeg in de portal zijn, moet u de Circuit-configuratie met de knop Vernieuwen op de protal vernieuwen. Deze bewerking geldt de configuratie van de juiste routering op uw circuit. 
>
>

In de Azure portal, status van een ExpressRoute-circuit kan worden gecontroleerd door het selecteren van ![2][2] in het menu links zijmarge en vervolgens te klikken op het ExpressRoute-circuit. Selecteren van een ExpressRoute opent circuit vermeld onder 'Alle resources' de ExpressRoute-circuit-blade. In de ![3][3] sectie van de blade de ExpressRoute essentials zou worden weergegeven, zoals wordt weergegeven in de volgende schermopname:

![5][5]

In het voorgaande voorbeeld als vermelde Azure is persoonlijke peering routering context ingeschakeld, terwijl Azure openbare en Microsoft peering routing-contexten zijn niet ingeschakeld. Een context die peering is ingeschakeld, moet ook de primaire en secundaire point-to-point (vereist voor BGP)-subnetten die worden vermeld. Het/30-subnetten worden gebruikt voor de interface-IP-adres van de msee's en PE-msee's. 

>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u of de primaire en secundaire subnetten toegewezen overeenkomen met de configuratie voor PE-msee's. Als dit niet het geval is, als u de configuratie op de MSEE-routers, Raadpleeg [maken en wijzigen van routering voor een ExpressRoute-circuit][CreatePeering]
>
>

###<a name="verification-via-powershell"></a>Verificatie via PowerShell
Als u het Azure configuratiedetails van persoonlijke peering, gebruikt u de volgende opdrachten:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Een antwoord voorbeeld voor een correct geconfigureerde persoonlijke peering, is:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Een context die peering is ingeschakeld, moet de primaire en secundaire adresvoorvoegsels vermeld. Het/30-subnetten worden gebruikt voor de interface-IP-adres van de msee's en PE-msee's.

Als u Azure openbare peering configuratiedetails, gebruikt u de volgende opdrachten:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

Als u de configuratiegegevens van de Microsoft-peering, gebruikt u de volgende opdrachten:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

Als een peering niet is geconfigureerd, zou er een foutbericht weergegeven. Een voorbeeld-antwoord, wanneer de vermelde peering (Azure openbare peering in dit voorbeeld) niet is geconfigureerd in het circuit:

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u of de primaire en secundaire subnetten toegewezen overeenkomen met de configuratie op de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureASN*, en *PeerASN* op msee's worden gebruikt en als deze waarden worden toegewezen aan de namen die op de gekoppelde PE-MSEE. Als MD5-hash is gekozen, is de gedeelde sleutel moet hetzelfde zijn op de combinatie van MSEE en PE MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van routering voor een ExpressRoute-circuit] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>Verificatie via PowerShell (klassiek)
Als u het Azure configuratiedetails van persoonlijke peering, gebruikt u de volgende opdracht:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Is een reactie voorbeeld voor een persoonlijke peering Prestatietesten zijn geconfigureerd:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

Een ingeschakeld is, moet peering context dan de primaire en secundaire peer-subnetten die worden vermeld. Het/30-subnetten worden gebruikt voor de interface-IP-adres van de msee's en PE-msee's.

Als u Azure openbare peering configuratiedetails, gebruikt u de volgende opdrachten:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

Als u de configuratiegegevens van de Microsoft-peering, gebruikt u de volgende opdrachten:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>Als laag 3-peerings zijn ingesteld door de serviceprovider, overschrijft instellen van de ExpressRoute-peerings via de portal of PowerShell de instellingen van de serviceprovider. Opnieuw instellen van de instellingen van de peering naast vereist de ondersteuning van de serviceprovider. De ExpressRoute-peerings alleen wijzigen als u zeker dat de serviceprovider alleen layer 2-services levert!
>
>

<p/>
>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u of de primaire en secundaire peer subnetten toegewezen overeenkomen met de configuratie op de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureAsn*, en *PeerAsn* op msee's worden gebruikt en als deze waarden worden toegewezen aan de namen die op de gekoppelde PE-MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van routering voor een ExpressRoute-circuit] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a>ARP tussen Microsoft en de serviceprovider valideren
Deze sectie wordt gebruikt (klassiek) PowerShell-opdrachten. Als u PowerShell Azure Resource Manager-opdrachten gebruikt hebt, zorg ervoor dat u beheerder/co-beheerder toegang tot het abonnement. Voor het oplossen van problemen met Azure Resource Manager opdrachten raadpleegt u de [ophalen ARP-tabellen in het Resource Manager-implementatiemodel] [ ARP] document.

>[!NOTE]
>Als u ARP, kunnen de Azure portal en de Azure Resource Manager PowerShell-opdrachten worden gebruikt. Als er fouten optreden met de Azure Resource Manager PowerShell-opdrachten, klassieke PowerShell-opdrachten gebruiken als klassieke PowerShell opdrachten ook worden gebruikt met Azure Resource Manager ExpressRoute-circuits.
>
>

Als u de ARP-tabel van de primaire MSEE-router voor persoonlijke peering, gebruikt u de volgende opdracht:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Een voorbeeld van antwoord voor de opdracht in het scenario voor geslaagd:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Op deze manier kunt u de ARP-tabel van de MSEE in controleren de *primaire*/*secundaire* pad voor *persoonlijke*/*openbare*/*Microsoft* peerings.

Het volgende voorbeeld ziet dat het antwoord van de opdracht voor een peering bestaat niet.

    ARP Info:
       
>[!NOTE]
>Als de ARP-tabel geen IP-adressen van de interfaces die zijn toegewezen aan de MAC-adressen heeft, moet u de volgende informatie bekijken:
>1. Als het eerste IP-adres van het/30 subnet voor de koppeling tussen de MSEE-PR en MSEE wordt gebruikt op de interface van MSEE PR. toegewezen Azure gebruikt altijd het tweede IP-adres voor de msee's.
>2. Controleer of als de klant (C-code) en VLAN-servicetags (S-Tag) overeenkomt met beide op een combinatie van MSEE PR en MSEE.
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a>BGP-routes op de MSEE en valideren
Deze sectie wordt gebruikt (klassiek) PowerShell-opdrachten. Als u PowerShell Azure Resource Manager-opdrachten gebruikt hebt, zorg ervoor dat u beheerder/co-beheerder toegang tot het abonnement.

>[!NOTE]
>Als u BGP-informatie, kunnen de Azure portal en de Azure Resource Manager PowerShell-opdrachten worden gebruikt. Als er fouten optreden met de Azure Resource Manager PowerShell-opdrachten, klassieke PowerShell-opdrachten gebruiken als klassieke PowerShell opdrachten ook worden gebruikt met Azure Resource Manager ExpressRoute-circuits.
>
>

Als u de routeringstabel (neighbor BGP) samenvatting voor een bepaalde routering context, gebruikt u de volgende opdracht:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Er is een voorbeeld van antwoord:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Zoals u in het voorgaande voorbeeld, wordt de opdracht is handig om te bepalen hoe lang de routering context is ingesteld. Het aantal route voorvoegsels die worden geadverteerd door de router peering wordt tevens aangegeven.

>[!NOTE]
>Als de status in actief of inactief is, moet u controleren als de primaire en secundaire peer subnetten toegewezen overeenkomen met de configuratie voor de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureAsn*, en *PeerAsn* op msee's worden gebruikt en als deze waarden worden toegewezen aan de namen die op de gekoppelde PE-MSEE. Als MD5-hash is gekozen, is de gedeelde sleutel moet hetzelfde zijn op de combinatie van MSEE en PE MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van routering voor een ExpressRoute-circuit][CreatePeering].
>
>

<p/>
>[!NOTE]
>Als bepaalde bestemmingen niet bereikbaar via een bepaalde peering zijn, controleert u de routetabel van de msee's die horen bij de specifieke context van de peering. Als een overeenkomende voorvoegsel (kan worden NATed IP) in de routeringstabel aanwezig is, moet u controleren als er firewalls/NSG/ACL's op het pad en als het verkeer is toegestaan.
>
>

De volledige-routeringstabel van de MSEE ophalen op de *primaire* pad voor de betreffende *persoonlijke* routering context, gebruik de volgende opdracht:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Er is een voorbeeld van de goede afloop voor de opdracht:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Op deze manier kunt u de routeringstabel van de MSEE in controleren de *primaire*/*secundaire* pad voor *persoonlijke*/*openbare*/*Microsoft* een peering context.

Het volgende voorbeeld ziet u dat het antwoord van de opdracht voor een peering bestaat niet:

    Route Table Info:

##<a name="check-the-traffic-statistics"></a>De statistieken voor het verkeer controleren
Als u de statistieken van de gecombineerde primaire en secundaire pad verkeer--bytes in en uit--van een peering context gebruikt de volgende opdracht:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Er is een voorbeeld van uitvoer van de opdracht:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Er is een voorbeeld van uitvoer van de opdracht voor een niet-bestaande peering:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende koppelingen voor meer informatie en hulp:

- [Microsoft-ondersteuning][Support]
- [Maken en een ExpressRoute-circuit wijzigen][CreateCircuit]
- [Maken en aanpassen van routering voor een ExpressRoute-circuit][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "logische Express Route-verbinding"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Pictogram voor alle resources"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Overzicht-pictogram"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "Schermopname van ExpressRoute Essentials-voorbeeld"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "Schermopname van ExpressRoute Essentials-voorbeeld"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






