---
title: Een virtueel netwerk configureren voor een Premium Azure Redis Cache | Microsoft Docs
description: Meer informatie over het maken en beheren van Virtual Network-ondersteuning voor uw Premium-laag Azure Cache voor instanties van Redis
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: yegu
ms.openlocfilehash: dafa9f09b6aed1f61facf3737c7e03c81b2a0e7d
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238246"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-cache-for-redis"></a>Over het configureren van Virtual Network-ondersteuning voor een Premium Azure Cache voor Redis
Azure Redis-Cache heeft een ander cache-aanbiedingen, waardoor u flexibiliteit bij de keuze van de grootte van de cache en -onderdelen, met inbegrip van Premium-functies zoals clustering, persistentie en virtual network-ondersteuning. Een VNet is een particulier netwerk in de cloud. Wanneer een Azure-Cache voor Redis-exemplaar is geconfigureerd met een VNet, is niet openbaar toegankelijk en kunnen alleen worden geopend van virtuele machines en toepassingen binnen het VNet. In dit artikel wordt beschreven hoe het configureren van virtual network-ondersteuning voor een premium Azure Cache voor Redis-exemplaar.

> [!NOTE]
> Azure Redis-Cache biedt ondersteuning voor zowel klassieke en Resource Manager VNets.
> 
> 

Zie voor meer informatie over andere functies van de cache premium [Inleiding tot de Azure-Cache voor Premium-laag Redis](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Waarom VNet?
[Azure-netwerk (VNet)](https://azure.microsoft.com/services/virtual-network/) implementatie biedt verbeterde beveiliging en isolatie voor uw Azure-Cache voor Redis, evenals subnetten, toegangsbeheerbeleid en andere functies voor het verder beperken van toegang.

## <a name="virtual-network-support"></a>Ondersteuning voor virtuele netwerken
Ondersteuning voor Virtual Network (VNet) is geconfigureerd op de **nieuwe Azure-Cache voor Redis** blade tijdens het maken van de cache. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nadat u een premium-prijscategorie hebt geselecteerd, kunt u de Redis-VNet-integratie configureren door het selecteren van een VNet dat is in hetzelfde abonnement en dezelfde locatie als de cache. Voor het gebruik van een nieuw VNet, maakt u deze eerst met de stappen in [een virtueel netwerk maken met de Azure-portal](../virtual-network/manage-virtual-network.md#create-a-virtual-network) of [een virtueel netwerk (klassiek) maken met behulp van de Azure-portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) en ga vervolgens terug naar de **Nieuwe Azure-Cache voor Redis** blade maken en configureren uw premium-cache.

Als u wilt het VNet voor uw nieuwe cache configureren, klikt u op **Virtueelnetwerk** op de **nieuwe Azure-Cache voor Redis** blade, en selecteer de gewenste VNet uit de vervolgkeuzelijst.

![Virtueel netwerk][redis-cache-vnet]

Selecteer de gewenste subnet uit de **Subnet** vervolgkeuzelijst lijst en geeft u de gewenste **statisch IP-adres**. Als u een klassiek VNet de **statisch IP-adres** veld is optioneel en als er niets is opgegeven, wordt een gekozen uit het geselecteerde subnet.

> [!IMPORTANT]
> Wanneer u een Azure-Cache voor Redis met een Resource Manager VNet implementeert, wordt de cache moet zich in een toegewezen subnet die geen andere resources, met uitzondering van Azure Cache voor instanties van Redis bevat. Als een poging is gedaan voor het implementeren van een Azure-Cache voor Redis met een Resource Manager VNet aan een subnet met andere resources, mislukt de implementatie.
> 
> 

![Virtueel netwerk][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure reserveert bepaalde IP-adressen binnen elk subnet, en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten zijn gereserveerd voor protocolconformiteit en drie adressen voor Azure-services. Zie voor meer informatie, [gelden er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Naast de IP-adressen die worden gebruikt door de Azure-VNET-infrastructuur, elke Redis-exemplaar in het subnet gebruikt twee IP-adressen per shard en een extra IP-adres voor de load balancer. Een niet-geclusterde cache wordt beschouwd als één shard hebben.
> 
> 

Nadat de cache is gemaakt, kunt u de configuratie voor het VNet weergeven door te klikken op **Virtueelnetwerk** uit de **resourcemenu**.

![Virtueel netwerk][redis-cache-vnet-info]

Voor verbinding met uw Azure-Cache voor Redis-exemplaar bij het gebruik van een VNet, geeft u de hostnaam van uw cache in de verbindingsreeks, zoals wordt weergegeven in het volgende voorbeeld:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-cache-for-redis-vnet-faq"></a>Azure Cache voor Redis VNet Veelgestelde vragen
De volgende lijst bevat antwoorden op veelgestelde vragen over Azure Cache voor het schalen van Redis.

* Wat zijn enkele veelvoorkomende configuratiefouten bij de Azure-Cache voor Redis en VNets?
* [Hoe kan ik controleren of mijn cache in een VNET werkt?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Tijdens het verbinden met mijn Azure-Cache voor Redis in een VNET, waarom krijg ik een foutmelding als dat het externe certificaat is ongeldig?](#when-trying-to-connect-to-my-redis-cache-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid)
* [Kan ik vnet's gebruiken met een standard- of basic-cache?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* Waarom het maken van een Azure-Cache voor Redis mislukken in sommige subnetten, maar niet voor andere?
* [Wat zijn de vereisten voor de subnet-adres?](#what-are-the-subnet-address-space-requirements)
* [Werken alle functies van de cache bij het hosten van een cache in een VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-vnets"></a>Wat zijn enkele veelvoorkomende configuratiefouten bij de Azure-Cache voor Redis en VNets?
Wanneer Azure voor Redis-Cache wordt gehost in een VNet, worden de poorten in de volgende tabellen worden gebruikt. 

>[!IMPORTANT]
>Als de poorten in de volgende tabellen worden geblokkeerd, is het mogelijk dat de cache niet goed werkt. Met een of meer van deze poorten geblokkeerd, is dat de meest voorkomende onjuiste configuratie-probleem bij het gebruik van Azure Cache voor Redis in een VNet.
> 
> 

- [Vereisten voor uitgaande poort](#outbound-port-requirements)
- [Vereisten voor binnenkomende poort](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Vereisten voor uitgaande poort

Er zijn zeven vereisten van de uitgaande poort.

- Als gewenste, al het uitgaande verbindingen naar het internet kunnen worden gemaakt door middel van een client on-premises controle apparaat.
- Drie van de poorten die verkeer gerouteerd naar Azure-eindpunten onderhoud van Azure Storage en Azure DNS.
- De resterende poortbereiken en voor interne communicatie voor Redis-subnet. Er is geen subnet-NSG-regels zijn vereist voor interne communicatie voor Redis-subnet.

| Poort(en) | Richting | Transportprotocol | Doel | Lokaal IP | Extern IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Uitgaand |TCP |Redis-afhankelijkheden op Azure Storage/PKI (Internet) | (Redis subnet) |* |
| 53 |Uitgaand |TCP/UDP |Redis-afhankelijkheden van DNS (Internet/VNet) | (Redis subnet) |* |
| 8443 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) | (Redis subnet) |
| 10221-10231 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) | (Redis subnet) |
| 20226 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 13000-13999 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 15000-15999 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 6379-6380 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |


#### <a name="inbound-port-requirements"></a>Vereisten voor binnenkomende poort

Er zijn acht vereisten voor het bereik van binnenkomende poort. Inkomende aanvragen in deze bereiken zijn binnenkomend van andere services die worden gehost in hetzelfde VNET of intern is aan de Redis-subnet-communicatie.

| Poort(en) | Richting | Transportprotocol | Doel | Lokaal IP | Extern IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Inkomend |TCP |Communicatie van clients met Redis, Azure load balancing | (Redis subnet) | (Redis subnet), Virtual Network, Azure Load Balancer |
| 8443 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 8500 |Inkomend |TCP/UDP |Azure load balancer | (Redis subnet) |Azure Load Balancer |
| 10221-10231 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet), Azure Load Balancer |
| 13000-13999 |Inkomend |TCP |Communicatie van clients met Redis-Clusters, Azure load balancing | (Redis subnet) |Virtual Network, Azure Load Balancer |
| 15000-15999 |Inkomend |TCP |Clientcommunicatie met Redis-Clusters, Azure load Balancing | (Redis subnet) |Virtual Network, Azure Load Balancer |
| 16001 |Inkomend |TCP/UDP |Azure load balancer | (Redis subnet) |Azure Load Balancer |
| 20226 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |

#### <a name="additional-vnet-network-connectivity-requirements"></a>Aanvullende vereisten voor VNET netwerk connectiviteit

Er zijn netwerkverbindingsvereisten voor Azure Cache voor Redis die niet in eerste instantie kan worden voldaan in een virtueel netwerk. Azure Redis-Cache moet de volgende items te laten functioneren bij gebruik binnen een virtueel netwerk.

* Uitgaande netwerkconnectiviteit met Azure Storage-eindpunten die over de hele wereld. Dit omvat eindpunten die zich in dezelfde regio bevinden als de Azure-Cache voor Redis-exemplaar, evenals de storage-eindpunten die zich in **andere** Azure-regio's. Azure Storage-eindpunten kunt oplossen door onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*, en *file.core.windows.net*. 
* Uitgaande netwerkconnectiviteit te *ocsp.msocsp.com*, *mscrl.microsoft.com*, en *crl.microsoft.com*. Deze verbinding nodig is voor de ondersteuning van SSL-functionaliteit.
* De DNS-configuratie voor het virtuele netwerk moet zijn geschikt voor het oplossen van alle eindpunten en domeinen die worden vermeld in de vorige punten. Deze DNS-vereisten kunnen worden voldaan door ervoor te zorgen dat een geldig DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.
* Uitgaande netwerkconnectiviteit met de volgende Azure Monitoring-eindpunten, die onder de volgende DNS-domeinen oplossen: shoebox2-black.shoebox2.metrics.nsatc.net, Noord-prod2.prod2.metrics.nsatc.net azglobal-black.azglobal.metrics.nsatc.net , shoebox2-red.shoebox2.metrics.nsatc.net, Oost-prod2.prod2.metrics.nsatc.net azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Hoe kan ik controleren of mijn cache in een VNET werkt?

>[!IMPORTANT]
>Wanneer u verbinding maakt met een Azure-Cache voor Redis-exemplaar dat wordt gehost in een VNET, moet uw cache-clients in hetzelfde VNET of in een VNET met een VNET-peering ingeschakeld. Dit omvat alle testtoepassingen of diagnostische hulpprogramma's voor niet pingen. Ongeacht waar de clienttoepassing wordt gehost, worden netwerkbeveiligingsgroepen geconfigureerd dat het netwerkverkeer van de client is toegestaan tot de Redis-exemplaar.
>
>

Nadat de Poortvereisten zijn geconfigureerd zoals beschreven in de vorige sectie, kunt u controleren of uw cache werkt door de volgende stappen uit te voeren.

- [Opnieuw opstarten](cache-administration.md#reboot) alle van de cache-knooppunten. Als alle cacheafhankelijkheden vereist kan niet worden bereikt (zoals beschreven in [inkomende Poortvereisten](cache-how-to-premium-vnet.md#inbound-port-requirements) en [uitgaande Poortvereisten](cache-how-to-premium-vnet.md#outbound-port-requirements)), de cache is niet mogelijk om opnieuw te starten is.
- Nadat de cache-knooppunten opnieuw hebt opgestart (zoals gemeld door de status van de cache in Azure portal), kunt u de volgende tests uitvoeren:
  - Ping het cache-eindpunt (met behulp van poort 6380 zijn) van een virtuele machine die zich in hetzelfde VNET als de cache met behulp van [tcping](https://www.elifulkerson.com/projects/tcping.php). Bijvoorbeeld:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Als de `tcping` hulpprogramma wordt gerapporteerd dat de poort is geopend, wordt de cache is beschikbaar voor de verbinding van clients in het VNET.

  - Een andere manier om te testen is het maken van een test-cacheclient (die mogelijk een eenvoudige consoletoepassing met StackExchange.Redis) die verbinding maakt met de cache en wordt toegevoegd en worden enkele items opgehaald uit de cache. Installeer de voorbeeldtoepassing van de client naar een virtuele machine die zich in hetzelfde VNET als de cache en uitvoeren om te controleren of de verbinding met de cache.


### <a name="when-trying-to-connect-to-my-azure-cache-for-redis-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>Tijdens het verbinden met mijn Azure-Cache voor Redis in een VNET, waarom krijg ik een foutmelding als dat het externe certificaat is ongeldig?

Tijdens het verbinden met een Azure-Cache voor Redis in een VNET, ziet u een certificaat validatiefout zoals deze:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

De oorzaak kan zijn dat u verbinding maakt met de host door het IP-adres. We raden u aan met behulp van de hostnaam. Met andere woorden, gebruikt u het volgende:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Vermijd het gebruik van het IP-adres die vergelijkbaar is met de volgende verbindingsreeks:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Als u zich niet kunt oplossen door de DNS-naam, sommige clientbibliotheken omvatten configuratieopties, zoals `sslHost` die wordt geleverd door de StackExchange.Redis-client. Hiermee kunt u voor de onderdrukking van de hostnaam die wordt gebruikt om certificaten te valideren. Bijvoorbeeld:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kan ik vnet's gebruiken met een standard- of basic-cache?
Vnet's kunnen alleen worden gebruikt met premium-caches.

### <a name="why-does-creating-an-azure-cache-for-redis-fail-in-some-subnets-but-not-others"></a>Waarom het maken van een Azure-Cache voor Redis mislukken in sommige subnetten, maar niet voor andere?
Als u een Azure-Cache voor Redis met een Resource Manager VNet implementeert, wordt de cache moet zich in een toegewezen subnet die er is geen resourcetype bevat. Als een poging is gedaan voor het implementeren van een Azure-Cache voor Redis aan een Resource Manager-VNet-subnet die andere resources bevat, mislukt de implementatie. U moet de bestaande resources binnen het subnet verwijderen voordat u een nieuwe Azure-Cache voor Redis maken kunt.

Als u onvoldoende beschikbare IP-adressen hebt, kunt u meerdere typen resources aan een klassiek VNet implementeren.

### <a name="what-are-the-subnet-address-space-requirements"></a>Wat zijn de vereisten voor de subnet-adres?
Azure reserveert bepaalde IP-adressen binnen elk subnet, en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten zijn gereserveerd voor protocolconformiteit en drie adressen voor Azure-services. Zie voor meer informatie, [gelden er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Naast de IP-adressen die worden gebruikt door de Azure-VNET-infrastructuur, elke Redis-exemplaar in het subnet gebruikt twee IP-adressen per shard en een extra IP-adres voor de load balancer. Een niet-geclusterde cache wordt beschouwd als één shard hebben.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Werken alle functies van de cache bij het hosten van een cache in een VNET?
Wanneer uw cache deel van een VNET uitmaakt, kan alleen clients in het VNET toegang krijgen tot de cache. Als gevolg hiervan werken de volgende functies voor cache niet op dit moment.

* Redis-Console - omdat Redis-Console wordt uitgevoerd in uw lokale browser, die zich buiten het VNET, er geen verbinding met uw cache.


## <a name="use-expressroute-with-azure-cache-for-redis"></a>Gebruik ExpressRoute met Azure Cache voor Redis

Klanten kunnen verbinding maken met een [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuit in hun virtuele netwerken-infrastructuur, waardoor hun on-premises netwerk uitbreiden naar Azure. 

Standaard een nieuwe ExpressRoute-circuit niet werkt met geforceerde tunnels (aankondiging van een standaard-route 0.0.0.0/0) op een VNET. Als gevolg hiervan uitgaande verbinding met Internet is toegestaan rechtstreeks vanuit het VNET en clienttoepassingen zijn geen verbinding maken met andere Azure-eindpunten met inbegrip van Azure Cache voor Redis.

Een veelvoorkomende configuratie van de klant is echter gebruik van geforceerde tunneling (adverteren een standaardroute) waardoor uitgaand internetverkeer naar in plaats daarvan flow on-premises. Deze verkeersstroom verbroken connectiviteit met Azure Cache Redis als het uitgaande verkeer en on-premises wordt geblokkeerd dat de Azure-Cache voor Redis-exemplaar kan niet communiceren met de bijbehorende afhankelijkheden.

De oplossing is voor het definiëren van een (of meer) gebruiker gedefinieerde routes (udr's) op het subnet waarin de Azure-Cache voor Redis. Een UDR definieert de subnet-specifieke routes die worden gebruikt in plaats van de standaardroute.

Het verdient indien mogelijk, gebruik de volgende configuratie:

* De ExpressRoute-configuratie adverteert 0.0.0.0/0 en standaard geforceerde tunnels al het uitgaande verkeer on-premises.
* De UDR toegepast op het subnet met de Azure-Cache voor Redis definieert 0.0.0.0/0 met een route werken voor TCP/IP-verkeer naar het openbare internet. bijvoorbeeld instellen door het volgende hoptype 'Internet'.

Het gecombineerde effect van deze stappen is dat het subnetniveau UDR voorrang boven de ExpressRoute geforceerde tunneling, dus ervoor te zorgen dat uitgaande internettoegang vanaf de Azure-Cache voor Redis.

Verbinding maken met een Azure-Cache voor Redis-exemplaar van een on-premises is toepassing met behulp van ExpressRoute niet een typische gebruiksscenario vanwege prestatieredenen (voor de beste prestaties Azure Cache voor Redis-clients moeten zich in dezelfde regio bevinden als de Azure-Cache voor Redis) .

>[!IMPORTANT] 
>De routes zijn gedefinieerd in een UDR **moet** specifiek genoeg voor voorrang boven alle routes die door de configuratie van ExpressRoute aangekondigd. Het volgende voorbeeld wordt het brede adresbereik 0.0.0.0/0 gebruikt, en als zodanig kan mogelijk per ongeluk worden overschreven door routeadvertenties met behulp van specifiekere adresbereiken.

>[!WARNING]  
>Azure Redis-Cache wordt niet ondersteund met ExpressRoute-configuraties die **onjuist advertentieoverschrijdende routes van de openbare-peeringpad naar het pad voor persoonlijke peering**. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen routeadvertenties van Microsoft voor een groot aantal Microsoft Azure IP-adresbereiken. Als deze adresbereiken onjuist advertentieoverschrijdend op het pad voor persoonlijke peering zijn, wordt het resultaat is dat alle uitgaande netwerkpakketten vanuit de Azure-Cache voor het subnet van de Redis-exemplaar onjuist geforceerde aan van een klant on-premises netwerk zijn infrastructuur. Deze netwerkstroom wordt Azure Cache voor Redis. De oplossing voor dit probleem is om te stoppen advertentieoverschrijdende routes van het pad voor openbare peering naar het pad voor persoonlijke peering.


Achtergrondinformatie over de gebruiker gedefinieerde routes is beschikbaar in deze [overzicht](../virtual-network/virtual-networks-udr-overview.md).

Zie voor meer informatie over ExpressRoute [technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer functies voor premium-cache.

* [Inleiding tot de Azure-Cache voor Premium-laag Redis](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

