---
title: Met Azure ExpressRoute globale bereiken toepassingsscenario | Microsoft Docs
description: Deze pagina bevat een toepassingsscenario voor globaal bereik.
documentationcenter: na
services: networking
author: cherylmc
manager: tracsman
ms.service: expressroute,global reach
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: cherylmc
ms.openlocfilehash: 2adb8debf641a9b3875c5c386df7a35273f2a258
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428235"
---
# <a name="expressroute-global-reach-application-scenario"></a>Toepassingsscenario ExpressRoute globaal bereik

Zie voor meer informatie over ExpressRoute globaal bereik, [ExpressRoute globaal bereik][Global Reach]. In dit artikel gaan we doorlopen een toepassingsscenario, vergelijken van de oplossing ExpressRoute globaal bereik voor een aantal andere oplossingen, globaal bereik configureren voor het voorbeeldscenario en controleer of de verbindingen. 

##<a name="application-scenario"></a>Toepassingsscenario

Fabrikam, Inc. is een grote fysieke aanwezigheid en Azure-implementatie in VS-Oost. Fabrikam is back-end-connectiviteit tussen de on-premises en Azure-implementaties via ExpressRoute. Contoso Ltd. heeft een aanwezigheid en Azure-implementatie in VS-West. Contoso heeft een back-end-connectiviteit tussen de on-premises en Azure-implementaties via ExpressRoute.  

Fabrikam Inc. acquires Contoso Ltd. Na de fusie wil Fabrikam interconnect de netwerken. De volgende afbeelding ziet u het scenario:

 [![1]][1]

De onderbroken pijlen in het midden van de bovenstaande afbeelding geven aan de vereiste netwerk onderlinge verbindingen. De volgende tabel ziet u de routetabel van de primaire persoonlijke peering van het ExpressRoute van Contoso Ltd. voorafgaand aan de fusie.

[![2]][2]

De volgende tabel ziet u de routetabel van de primaire persoonlijke peering van het ExpressRoute van Fabrikam Inc. voorafgaand aan de fusie.

[![3]][3]

## <a name="traditional-solutions"></a>Traditionele oplossingen

### <a name="option-1-interconnect-on-premises-networks"></a>Optie 1: On-premises netwerken InterConnect

De volgende afbeelding ziet u deze optie. Zoals wordt weergegeven, kunt u de twee on-premises netwerken met behulp van site-naar-site VPN- of andere opties breedbandverbinding interconnect en beheren van alle routering tussen de twee entiteiten. 

[![4]][4]

Deze optie heeft de volgende nadelen:

- U geforceerd interregionale Azure communicatie voor de implementatie via een suboptimale route.
- U hebt het voordeel van de hoge beschikbaarheid van het Microsoft-backbone-netwerk voor de communicatie tussen regionale weigert.
- U de volledige routering verantwoordelijkheid voor de communicatie tussen regionale geleid.

### <a name="option-2-expressroute-cross-connectivity-and-interconnect-on-premises-networks"></a>Optie 2: ExpressRoute-cross-connectiviteit en on-premises netwerken interconnect

De volgende afbeelding ziet u deze optie.

[![5]][5]

Zoals u in de afbeelding hierboven, kunt u ook verbinding tussen de ExpressRoute-circuits met de cross regionale vnet's maken. De cross regionale verbinding tussen VNet-naar de ExpressRoute-circuits, zou de volgende communicaties inschakelen:

- De VS-West / VS Oost-VNets respectievelijk communiceren met Fabrikam/Contoso on-premises netwerken.
- Het VNet VS West om te communiceren met het VNet VS Oost.

De onderlinge verbinding tussen de twee on-premises netwerken is nog steeds nodig voor de on-premises netwerken met elkaar communiceren.

Deze optie kunt interregionale Azure communicatie voor de persoonlijke implementatie te fietsen via de Microsoft-backbone en de communicatie tussen de on-premises netwerk te worden uitgevoerd via het externe netwerk. Het grootste nadeel van de oplossing is echter dat u wilt meerdere cross regionale verbindingen maken, die onderhoud en probleemoplossing bemoeilijken. Bovendien kunt de optie u niet profiteren van de hoge beschikbaarheid wereldwijde Microsoft-backbone voor de communicatie tussen de twee on-premises netwerken.

### <a name="option-3-vnet-peering-and-interconnect-on-premises-networks"></a>Optie 3: VNet-peering en on-premises netwerken interconnect

De volgende afbeelding ziet u deze optie. De optie maakt gebruik van VNet-peering voor het interregionale VNet-communicatie. De optie, is zoals wordt geïllustreerd, niet volledig omdat het niet-overschrijdende VNet-naar-on-premises-communicatie (aangegeven via de twee regels met punten pijl in het midden)-adres. Gebruik on-premises naar on-premises-verbinding (zoals in optie 1) of ExpressRoute-connectiviteit (zoals in optie 2) voor de regio-overschrijdend cross communicatie tussen on-premises.

[![6]][6]

Deze optie biedt optimale routering voor interregionale VNet-communicatie. Het valt echter korte als Fabrikam of Contoso een meer complexe Azure-implementatie, zoals een ster-VNet-model heeft. Ook als de vorige twee opties kunt met deze optie u niet profiteren van de hoge beschikbaarheid wereldwijde Microsoft-backbone voor de communicatie tussen de twee on-premises netwerken.

## <a name="global-reach"></a>Globaal bereik

ExpressRoute globaal bereik koppelingen persoonlijke peering van het ExpressRoute-circuits, zoals wordt geïllustreerd in de volgende afbeelding:

[![7]][7]

Globaal bereik configureren tussen de twee ExpressRoute-circuits kunt privécommunicatie tussen de twee on-premises netwerken en de Azure-implementatie in de twee regio's. Dus globaal bereik voldoet aan de gewenste communicatie (aangegeven via stippellijn in de eerste afbeelding van dit artikel) via de Microsoft-backbone-netwerk.

### <a name="configure-global-reach"></a>Wereldwijd bereik configureren

Zie voor informatie over het configureren van ExpressRoute globaal bereik, [globaal bereik configureren][Configure Global Reach]. 

Omdat Fabrikam, Inc. en Contoso Ltd. onboarding Azure als afzonderlijke bedrijven, de ExpressRoute-circuits van de twee bedrijven zich in twee verschillende Azure-abonnementen. Voor het maken van het globale bereik voor de abonnementen, moet u een autorisatie in die horen bij Contoso Ltd. het ExpressRoute-circuit maken en deze doorgeven aan de Fabrikam Inc. ExpressRoute-circuit.


Voor het maken van een autorisatie voor ExpressRoute-circuit van Contoso, de eerste keer aanmelden bij Contoso Azure-account en selecteer het juiste abonnement (als er meerdere abonnementen). De PowerShell-opdrachten voor deze stappen zijn:

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```
Hieronder vindt u de stappen voor het maken van een ExpressRoute-circuit-autorisatie:

```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```

De uitvoer 'Set-AzureRmExpressRouteCircuit' wordt een lijst de expressroute-circuit. Houd er rekening mee de persoonlijke peering-ID en de autorisatiesleutel die aan het einde van de aanbieding worden vermeld. Zie een hieronder voor uitvoer fragment van voorbeeld van PowerShell:

```powershell
ServiceKey                       : <serviceKey>
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 65020,
                                       "PrimaryPeerAddressPrefix": "192.168.11.16/30",
                                       "SecondaryPeerAddressPrefix": "192.168.11.20/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 110,
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "4",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : [
                                     {
                                       "Name": "Auth4-ASH-Cust11",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/authorizations/Auth4-ASH-Cust11",
                                       "AuthorizationKey": "<authorizationKey>",
                                       "AuthorizationUseStatus": "Used",
                                       "ProvisioningState": "Succeeded"
                                     }
                                   ]
AllowClassicOperations           : False
GatewayManagerEtag               : 4
```

U kunt met de peering-ID en de autorisatiesleutel maken de globaal bereik onder van de Fabrikam ExpressRoute-circuit. Meld u aan bij Azure-account van de Fabrikam. Als er meer dan één abonnement, selecteert u het juiste abonnement.

Wereldwijd bereik maakt een redundante set point-to-point-verbindingen tussen de twee MSEE-paren. Voor de twee point-to-point-verbindingen, moet u om op te geven van een/29-adresvoorvoegsel (voor het actieve voorbeeld gaan we gebruiken 192.168.11.64/29). Gebruik de volgende opdrachten om de verbinding met globaal bereik te maken:

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix "__.__.__.__/29" -AuthorizationKey "########-####-####-####-############"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Nadat de bovenstaande opdrachten worden uitgevoerd, duurt het enkele minuten om de redundante verbindingen met globaal bereik te maken.

### <a name="verify-global-reach"></a>Controleer of wereldwijd bereik

De volgende tabel ziet u de routetabel van de primaire persoonlijke peering van het ExpressRoute van Contoso Ltd. na het configureren van globaal bereik.

[![8]][8]

De volgende tabel ziet u de routetabel van de primaire persoonlijke peering van het ExpressRoute van Fabrikam Inc. na het configureren van globaal bereik.

[![9]][9]

Ziet u de voorvoegsels voor de 'Netwerk' verwachte doel en de juiste 'volgende HOP' worden vermeld in de bovenstaande tabellen.

Hierboven ziet u de blade 'Get-routetabel' die kan worden geopend in de Azure-portal onder persoonlijke peering van een ExpressRoute-circuit. U kunt ook een lijst een routetabel voor ExpressRoute met behulp van de volgende PowerShell-opdracht:

```powershell
Get-AzExpressRouteCircuitRouteTable -DevicePath 'primary' -ExpressRouteCircuitName "Your_circuit_name" -PeeringType AzurePrivatePeering -ResourceGroupName "Your_resource_group"
```

Als u wilt zien van de routetabel van de secundaire MSEE, Vervang 'primaire' met het trefwoord 'secondary' in de bovenstaande opdracht.

Laten we Controleer ook of de toegang tot de gegevens vlak door pingen naar andere bestemming van verschillende netwerken. De volgende drie pings Controleer de gegevens vlak verbinding met de Contoso on-premises netwerk, Contoso Azure VNet, en Fabrikam Azure VNet van Fabrikam on-premises netwerk.


    C:\Users\FabrikamUser>ping 10.1.11.10
    
    Pinging 10.1.11.10 with 32 bytes of data:
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    
    Ping statistics for 10.1.11.10:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 69ms, Maximum = 69ms, Average = 69ms
    
    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=82ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=81ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 82ms, Average = 78ms
    
    C:\Users\FabrikamUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=2ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 3ms, Average = 2ms


De volgende twee pings Controleer de gegevens vlak verbinding naar de Azure-VNet Contoso en Fabrikam Azure VNet van Contoso on-premises netwerk.

    C:\Users\ContosoUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=6ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 5ms, Maximum = 6ms, Average = 5ms
    
    C:\Users\ContosoUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=74ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 73ms, Maximum = 74ms, Average = 73ms

De volgende opdracht ping verifieert toegang tot de gegevenslaag van gegevens naar de Contoso Azure VNet van de Fabrikam Azure VNet.

    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=78ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 78ms, Average = 77ms

## <a name="optimization-for-latency-sensitive-traffic"></a>Optimalisatie voor latentie gevoelige verkeer

Wereldwijd bereik stuurt verkeer via Microsoft Edge-apparaten. Voor het specifieke scenario beschouwd als in dit artikel, kunt u bereiken meer optimale routering tussen de twee VNets door in te schakelen van VNet-peering tussen beide. U kunt op dezelfde manier bereiken meer optimale routering tussen de twee on-premises netwerken via een serviceprovider, die een meer rechtstreekse WAN-verbinding tussen de sites kunnen bieden. In dergelijke scenario's kunt u globaal bereik routering als een mislukken back-optie voor deze verbindingen. 

## <a name="next-steps"></a>Volgende stappen

Wereldwijd bereik wordt geïmplementeerd op basis van per land. Als u wilt zien als globaal bereik is beschikbaar in de landen die u wilt, Zie [ExpressRoute globaal bereik][Global Reach].

<!--Image References-->
[1]: ./media/expressroute-globalreach-applicationscenario/PreMergerScenario.png "scenario van de toepassing"
[2]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Premerger.png "Contoso ExpressRoute routetabel vóór fusie"
[3]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Premerger.png "Fabrikam ExpressRoute routetabel vóór fusie"
[4]: ./media/expressroute-globalreach-applicationscenario/S2SVPN-Solution.png "elkaar on-premises netwerken"
[5]: ./media/expressroute-globalreach-applicationscenario/ExR-XConnect-Solution.png "ExpressRoute Cross Connect"
[6]: ./media/expressroute-globalreach-applicationscenario/VNet-peering-Solution.png "VNet-peering"
[7]: ./media/expressroute-globalreach-applicationscenario/GlobalReach-Solution.png "Global Reach"
[8]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Postmerger.png "Contoso ExpressRoute routetabel met globaal bereik"
[9]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Postmerger.png "Fabrikam ExpressRoute routetabel met globaal bereik"

<!--Link References-->
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach





