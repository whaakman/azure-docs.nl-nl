---
title: 'Controleer de verbinding - problemen oplossen met ExpressRoute: Azure| Microsoft Docs'
description: Deze pagina vindt instructies voor het oplossen van problemen en valideren van end-to-end-connectiviteit van een ExpressRoute-circuit.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 09/26/2017
ms.author: rambala
ms.custom: seodec18
ms.openlocfilehash: c4b20b4007a4824ee2dea0e1b22f1ea886218885
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453606"
---
# <a name="verifying-expressroute-connectivity"></a>Connectiviteit ExpressRoute controleren
Dit artikel helpt u bij het controleren en oplossen van ExpressRoute-connectiviteit. ExpressRoute, wat een on-premises netwerk naar de Microsoft cloud via een persoonlijke verbinding die mogelijk gemaakt door een connectiviteitsprovider uitbreidt wordt, omvat de volgende drie afzonderlijke network-zones:

-   Klantnetwerk
-   Provider-netwerk
-   Microsoft Datacenter

Het doel van dit document is voor de gebruiker kan aangeven wat waar u (of zelfs als) een probleem met de netwerkverbinding bestaat en in welke zone, waardoor te zoeken naar Help-informatie van het juiste team om het probleem te verhelpen. Als Microsoft-ondersteuning is vereist om een probleem te verhelpen, open een ondersteuningsticket met [Microsoft Support][Support].

> [!IMPORTANT]
> Dit document is bedoeld om u te helpen bij het vaststellen en eenvoudige problemen oplossen. Het is niet bedoeld om te worden van een vervanging voor ondersteuning van Microsoft. Open een ondersteuningsticket met [Microsoft Support] [ Support] als u niet het probleem met behulp van de richtlijnen op te lossen.
>
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht
Het volgende diagram toont de logische connectiviteit van het netwerk van een klant met Microsoft-netwerk met behulp van ExpressRoute.
[![1]][1]

De getallen geven in het voorgaande diagram sleutel netwerk punten. De netwerk-punten worden vaak in dit artikel verwezen door hun bijbehorende nummers.

De netwerk-punten 3 en 4 kunnen switches (Layer 2-apparaten) zijn afhankelijk van het ExpressRoute-connectiviteit-model (Cloud Exchange CO-locatie, Point-to-Point Ethernet-verbinding of Any-to-any (IPVPN)). De belangrijkste netwerk punten geïllustreerd zijn als volgt:

1.  Klant compute apparaat (bijvoorbeeld een server of een PC)
2.  CEs: Klant-randrouters 
3.  PEs (CE gerichte): Provider van edge. routers/schakelaars die klant randrouters worden geconfronteerd. Genoemd PE CEs in dit document.
4.  PEs (MSEE gerichte): Provider van Microsoft edge. routers/schakelaars die msee's worden geconfronteerd. Genoemd PE-msee's in dit document.
5.  Msee's: Microsoft Enterprise Edge (MSEE) ExpressRoute routers
6.  Gateway van virtueel netwerk (VNet)
7.  COMPUTE-apparaat op het Azure-VNet

Als u de connectiviteitsmodellen Cloud Exchange CO-locatie of Point-to-Point Ethernet-verbinding gebruikt, zou de klantenrandrouter (2) tot stand brengen BGP-peering met msee's (5). Netwerk punten 3 en 4 wordt nog steeds aanwezig zijn, maar enigszins transparant als Layer 2-apparaten.

Als het model van de connectiviteit Any-to-any (IPVPN) wordt gebruikt, de PEs (MSEE gerichte) (4) BGP-peering met msee's (5) wilt maken. Routes zou worden doorgegeven aan terug naar het netwerk van de klant via de IPVPN serviceprovider-netwerk.

>[!NOTE]
>Voor hoge beschikbaarheid van ExpressRoute vereist Microsoft dat een redundant paar BGP-sessies tussen msee's (5) en de PE-msee's (4). Een redundant paar netwerkpaden wordt ook aangemoedigd tussen klantnetwerk en PE CEs. In Any-to-any (IPVPN) verbinding model, kan echter een enkel CE-apparaat (2) zijn verbonden met een of meer PEs (3).
>
>

Voor het valideren van een ExpressRoute-circuit, vallen de volgende stappen uit (met het netwerkpunt aangegeven door het bijbehorende nummer):
1. [Valideren van circuitinrichting en -status (5)](#validate-circuit-provisioning-and-state)
2. [Valideren van ten minste één ExpressRoute-peering is geconfigureerd (5)](#validate-peering-configuration)
3. [Valideren van ARP tussen Microsoft en de service provider (koppeling tussen 4 en 5)](#validate-arp-between-microsoft-and-the-service-provider)
4. [Valideren van BGP en routes op de MSEE (BGP tussen 4 en 5 en 5 tot en met 6 als een VNet is verbonden)](#validate-bgp-and-routes-on-the-msee)
5. [Controleer de statistieken verkeer (verkeer te geven tot en met 5)](#check-the-traffic-statistics)

Meer controles en controles wordt toegevoegd in de toekomst, terugkomen maandelijks!

## <a name="validate-circuit-provisioning-and-state"></a>Valideren van circuitinrichting en -status
Een ExpressRoute-circuit moet worden gemaakt, ongeacht het model connectiviteit en dus een sleutel gegenereerd voor het inrichten van het circuit. Een ExpressRoute-circuit provisioning maakt een redundante laag-2-verbindingen tussen PE-msee's (4) en de msee's (5). Zie het artikel voor meer informatie over het maken, wijzigen, inrichten en controleer of een ExpressRoute-circuit [maken en aanpassen van een ExpressRoute-circuit][CreateCircuit].

>[!TIP]
>Een servicesleutel identificatie unieke van een ExpressRoute-circuit. Deze sleutel is vereist voor het merendeel van de powershell-opdrachten die worden vermeld in dit document. Bovendien moet u hulp van Microsoft of van een ExpressRoute-partner voor een ExpressRoute-probleem kunt oplossen, geeft u de servicesleutel voor het identificeren van het circuit gemakkelijk nodig hebt.
>
>

### <a name="verification-via-the-azure-portal"></a>Verificatie via Azure portal
In de Azure-portal, de status van een ExpressRoute-circuit kan worden gecontroleerd door het selecteren van ![2][2] in het menu links zijbalk en selecteer vervolgens het ExpressRoute-circuit. Selecteren van een ExpressRoute opent circuit vermeld onder 'Alle resources' u de blade van ExpressRoute-circuit. In de ![3][3] sectie van de blade, de ExpressRoute essentials worden vermeld, zoals wordt weergegeven in de volgende schermafbeelding:

![4][4]    

In de Essentials ExpressRoute *Circuit status* geeft de status van het circuit aan de kant van Microsoft. *Providerstatus* geeft aan of het circuit is *ingericht/niet ingericht* aan de service-provider. 

Voor een ExpressRoute-circuit operationeel is, worden de *Circuit status* moet *ingeschakeld* en de *providerstatus* moet *ingerichte*.

>[!NOTE]
>Als de *Circuit status* is niet ingeschakeld, neem contact op met [Microsoft Support][Support]. Als de *providerstatus* is niet ingericht, neem contact op met uw serviceprovider.
>
>

### <a name="verification-via-powershell"></a>Verificatie via PowerShell
Als u de ExpressRoute-circuits in een resourcegroep, gebruik de volgende opdracht:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>U kunt de naam van resourcegroep ophalen via de Azure. Zie de vorige subsectie van dit document en houd er rekening mee dat de naam van de resource is opgenomen in de schermopname.
>
>

Selecteer een bepaalde ExpressRoute-circuit in een resourcegroep, gebruik de volgende opdracht:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Een van het voorbeeldantwoord is:

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

### <a name="verification-via-powershell-classic"></a>Verificatie via PowerShell (klassiek)
Als u de ExpressRoute-circuits onder een abonnement, gebruik de volgende opdracht:

    Get-AzureDedicatedCircuit

Selecteer een bepaalde ExpressRoute-circuit, gebruik de volgende opdracht:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

Een van het voorbeeldantwoord is:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Om te bevestigen of een ExpressRoute-circuit operationele speciale aandacht besteden aan de volgende velden: ServiceProviderProvisioningState : Ingerichte Status: Ingeschakeld

>[!NOTE]
>Als de *Status* is niet ingeschakeld, neem contact op met [Microsoft Support][Support]. Als de *ServiceProviderProvisioningState* is niet ingericht, neem contact op met uw serviceprovider.
>
>

## <a name="validate-peering-configuration"></a>Een Peeringconfiguratie valideren
Nadat de service-provider is voltooid de inrichting van het ExpressRoute-circuit, kan een configuratie van de routering via de ExpressRoute-circuit tussen de MSEE-pull-aanvragen (4) en de msee's (5) worden gemaakt. Elk ExpressRoute-circuit kan een, twee of drie routering contexten ingeschakeld hebben: Azure private peering (verkeer naar persoonlijke virtuele netwerken in Azure), Azure openbare peering (verkeer naar openbare IP-adressen in Azure) en Microsoft peering (verkeer tot Office 365 en Dynamics 365). Zie het artikel voor meer informatie over het maken en wijzigen van de configuratie van de routering [maken en wijzigen van de routering voor een ExpressRoute-circuit][CreatePeering].

### <a name="verification-via-the-azure-portal"></a>Verificatie via Azure portal

>[!NOTE]
>Als laag-3 wordt geleverd door de service-provider en de peerings leeg in de portal zijn, moet u de configuratie van het Circuit met de vernieuwknop in de portal vernieuwen. Met deze bewerking is de juiste configuratie van de routering van toepassing op uw circuit. 
>
>

In Azure portal, de status van een ExpressRoute-circuit kan worden gecontroleerd door het selecteren van ![2][2] in het menu links zijbalk en selecteer vervolgens het ExpressRoute-circuit. Het selecteren van een ExpressRoute zou circuit vermeld onder 'Alle resources' de ExpressRoute-circuit blade geopend. In de ![3][3] sectie van de blade, de ExpressRoute essentials worden vermeld zoals wordt weergegeven in de volgende schermafbeelding:

![5][5]

In het voorgaande voorbeeld als vermelde Azure is private peering routering context ingeschakeld, terwijl Azure openbare en Microsoft-peering routering contexten zijn niet ingeschakeld. Een context die is ingeschakeld peering moet ook de primaire en secundaire point-to-point (vereist voor BGP)-subnetten die worden vermeld. Het/30-subnetten worden gebruikt voor het IP-adres van de interface van de msee's en de PE-msee's. 

>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u als de primaire en secundaire subnetten die overeenkomt met de configuratie op PE Msee. Als dit niet het geval is, als u wilt wijzigen van de configuratie op de MSEE-routers, Raadpleeg [maken en wijzigen van de routering voor een ExpressRoute-circuit][CreatePeering]
>
>

### <a name="verification-via-powershell"></a>Verificatie via PowerShell
Als u de configuratiedetails Azure privé-peering, gebruikt u de volgende opdrachten:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Een van het voorbeeldantwoord, voor een correct geconfigureerde privé-peering, is:

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

 Een context die is ingeschakeld peering moet de primaire en secundaire adresvoorvoegsels die worden vermeld. Het/30-subnetten worden gebruikt voor het IP-adres van de interface van de msee's en de PE-msee's.

Als u de Azure configuratiedetails voor openbare peering, gebruikt u de volgende opdrachten:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt

Als u de configuratiegegevens van de Microsoft-peering, gebruikt u de volgende opdrachten:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
     Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Als u een peering niet is geconfigureerd, zou er een foutbericht weergegeven. Een voorbeeldantwoord, wanneer de vermelde peering (openbare Azure-peering in dit voorbeeld) is niet geconfigureerd in het circuit:

    Get-AzExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u als de primaire en secundaire subnetten die overeenkomt met de configuratie op de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureASN*, en *PeerASN* op msee's worden gebruikt en als deze waarden worden toegewezen aan de die worden gebruikt op de gekoppelde PE-MSEE. Als MD5-hash is gekozen, is de gedeelde sleutel moet hetzelfde zijn op een sleutelpaar met MSEE en PE MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van de routering voor een ExpressRoute-circuit] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>Verificatie via PowerShell (klassiek)
Als u de configuratiedetails Azure privé-peering, gebruikt u de volgende opdracht uit:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Er is een voorbeeldantwoord voor een persoonlijke peering is geconfigureerd:

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

Een ingeschakeld is, peering context moet de primaire en secundaire peer-subnetten die worden vermeld. Het/30-subnetten worden gebruikt voor het IP-adres van de interface van de msee's en de PE-msee's.

Als u de Azure configuratiedetails voor openbare peering, gebruikt u de volgende opdrachten:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

Als u de configuratiegegevens van de Microsoft-peering, gebruikt u de volgende opdrachten:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>Als laag-3-peerings zijn ingesteld door de serviceprovider, overschrijft instellen van de ExpressRoute-peerings via de portal of PowerShell de instellingen van de service-provider. Opnieuw instellen van de instellingen van de peering kant vereist de ondersteuning van de serviceprovider. De ExpressRoute-peerings alleen wijzigen als het is zeker van zijn dat de serviceprovider services op laag 2 alleen biedt!
>
>

<p/>
>[!NOTE]
>Als een peering niet is ingeschakeld, controleert u als de primaire en secundaire peer-subnetten die overeenkomt met de configuratie op de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureAsn*, en *PeerAsn* op msee's worden gebruikt en als deze waarden worden toegewezen aan de die worden gebruikt op de gekoppelde PE-MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van de routering voor een ExpressRoute-circuit] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a>Valideren van ARP tussen Microsoft en de service-provider
In deze sectie maakt gebruik van PowerShell (klassiek)-opdrachten. Als u PowerShell Azure Resource Manager-opdrachten gebruikt hebt, zorgt u ervoor dat u beheerder/co-beheerder toegang tot het abonnement hebben. Voor problemen oplossen met behulp van Azure Resource Manager opdrachten raadpleegt u de [Getting ARP-tabellen in het Resource Manager-implementatiemodel] [ ARP] document.

>[!NOTE]
>Als u ARP, kunnen Azure portal en Azure Resource Manager PowerShell-opdrachten worden gebruikt. Als er fouten optreden met de Azure Resource Manager PowerShell-opdrachten, klassieke PowerShell-opdrachten gebruiken als klassieke PowerShell opdrachten ook met Azure Resource Manager ExpressRoute-circuits werken.
>
>

Als u de ARP-tabel van de primaire MSEE-router voor persoonlijke peering, gebruikt u de volgende opdracht uit:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Een voorbeeldantwoord voor de opdracht, in het scenario voor geslaagd:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Op deze manier kunt u de ARP-tabel uit de MSEE in controleren de *primaire*/*secundaire* pad voor *persoonlijke*/*openbare*  / *Microsoft* peerings.

Het volgende voorbeeld ziet dat het antwoord van de opdracht voor een peering bestaat niet.

    ARP Info:
       
>[!NOTE]
>Wanneer de ARP-tabel heeft geen IP-adressen van de interfaces die zijn toegewezen aan de MAC-adressen, controleert u de volgende informatie:
>1. Als het eerste IP-adres van het/30 subnet toegewezen voor de koppeling tussen de MSEE-pull-aanvraag en de MSEE wordt gebruikt op de interface van de MSEE-pull-aanvraag. Azure maakt altijd gebruik van het tweede IP-adres voor msee's.
>2. Controleer of als de klant (C-code) en de service (S-code) VLAN-tags overeenkomen met beide op een sleutelpaar met een MSEE-pull-aanvraag en de MSEE.
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a>BGP en routes op de MSEE valideren
In deze sectie maakt gebruik van PowerShell (klassiek)-opdrachten. Als u PowerShell Azure Resource Manager-opdrachten gebruikt hebt, zorgt u ervoor dat u beheerder/co-beheerder toegang tot het abonnement hebben.

>[!NOTE]
>Als u BGP-gegevens, kunnen zowel Azure portal en Azure Resource Manager PowerShell-opdrachten worden gebruikt. Als er fouten optreden met de Azure Resource Manager PowerShell-opdrachten, klassieke PowerShell-opdrachten gebruiken als klassieke PowerShell opdrachten ook met Azure Resource Manager ExpressRoute-circuits werken.
>
>

Als u de routeringstabel (neighbor BGP) samenvatting voor een bepaalde context van de routering, gebruikt u de volgende opdracht uit:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Een voorbeeld van een antwoord is:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Zoals u in het voorgaande voorbeeld, is de opdracht is handig om te bepalen voor hoe lang de routering-context is ingesteld. Tevens wordt aangegeven aantal route voorvoegsels geadverteerd door de peering-router.

>[!NOTE]
>Als de status in actief of niet-actief is, moet u controleren als de primaire en secundaire peer-subnetten die overeenkomt met de configuratie op de gekoppelde PE-MSEE. Controleer ook of de juiste *VlanId*, *AzureAsn*, en *PeerAsn* op msee's worden gebruikt en als deze waarden worden toegewezen aan de die worden gebruikt op de gekoppelde PE-MSEE. Als MD5-hash is gekozen, is de gedeelde sleutel moet hetzelfde zijn op een sleutelpaar met MSEE en PE MSEE. Als u de configuratie op de MSEE-routers, verwijzen naar [maken en wijzigen van de routering voor een ExpressRoute-circuit][CreatePeering].
>
>

<p/>
>[!NOTE]
>Als bepaalde bestemmingen niet bereikbaar via een bepaalde peering zijn, controleert u de routetabel van de msee's die behoren tot de specifieke context van de peering. Als een overeenkomend voorvoegsel (kan worden NATed IP) in de routeringstabel aanwezig is, moet u controleren als er firewalls/NSG/ACL's op het pad en als ze het verkeer is toegestaan.
>
>

Voor de volledige-routeringstabel van de MSEE over de *primaire* pad voor de betreffende *persoonlijke* routering context, gebruik de volgende opdracht:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Er is een voorbeeld van de geslaagde resultaat voor de opdracht:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Op deze manier kunt u de routeringstabel van de MSEE in controleren de *primaire*/*secundaire* pad voor *persoonlijke* /  *Openbare*/*Microsoft* een peering-context.

Het volgende voorbeeld ziet u dat het antwoord van de opdracht voor een peering bestaat niet:

    Route Table Info:

## <a name="check-the-traffic-statistics"></a>De statistieken verkeer controleren
Als u de statistieken van de gecombineerde pad naar de primaire en secundaire verkeer--bytes in en uit--van een peering-context, gebruikt de volgende opdracht uit:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Er is een voorbeeld van de uitvoer van de opdracht:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Er is een voorbeeld van de uitvoer van de opdracht voor een niet-bestaande peering:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie en hulp:

- [Microsoft-ondersteuning][Support]
- [Een ExpressRoute-circuit maken en wijzigen][CreateCircuit]
- [Routering voor een ExpressRoute-circuit maken en wijzigen][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "logische Express Route-verbinding"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Pictogram voor alle resources"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Overzicht van pictogram"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials voorbeeld schermafbeelding"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials voorbeeld schermafbeelding"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






