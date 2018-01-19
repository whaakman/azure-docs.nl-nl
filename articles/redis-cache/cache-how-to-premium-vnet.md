---
title: Een virtueel netwerk configureren voor een Premium Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het maken en beheren van Virtual Network-ondersteuning voor uw Azure Redis-Cache-exemplaren van Premium-laag
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: wesmc
ms.openlocfilehash: 74ec104bebec2004a8b7116865c2394c02b12638
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Ondersteuning voor virtuele netwerken configureren voor een Premium Azure Redis-Cache
Azure Redis-Cache heeft verschillende cache aanbiedingen die flexibiliteit bij de keuze van cachegrootte en -functies bieden, zoals de Premium-laag functies zoals clustering, persistentie en virtual network-ondersteuning. Een VNet is een particulier netwerk in de cloud. Wanneer een Azure Redis-Cache-exemplaar is geconfigureerd met een VNet, is niet openbaar toegankelijk en kan alleen worden benaderd vanuit virtuele machines en toepassingen binnen het VNet. In dit artikel wordt beschreven hoe het configureren van virtual network-ondersteuning voor een premium Azure Redis-Cache-exemplaar.

> [!NOTE]
> Azure Redis-Cache ondersteunt zowel klassieke en Resource Manager VNets.
> 
> 

Zie voor informatie over andere functies van premium-cache, [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Waarom VNet?
[Azure-netwerk (VNet)](https://azure.microsoft.com/services/virtual-network/) implementatie biedt verbeterde beveiliging en isolatie voor uw Azure Redis-Cache, evenals de subnetten, toegangscontrolebeleid, en andere functies nog toegang beperken.

## <a name="virtual-network-support"></a>Ondersteuning voor virtuele netwerken
Ondersteuning voor Virtual Network (VNet) is geconfigureerd op de **nieuwe Redis-Cache** blade tijdens het maken van de cache. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nadat u een premium-prijscategorie hebt geselecteerd, kunt u VNet Redis-integratie configureren door het selecteren van een VNet dat zich in hetzelfde abonnement en dezelfde locatie als uw cache. Voor het gebruik van een nieuw VNet maken eerst de stappen in [een virtueel netwerk maken met de Azure-portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [een virtueel netwerk (klassiek) maken met behulp van de Azure-portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) en ga daarna terug naar de **nieuwe Redis-Cache** blade maken en configureren van uw premium-cache.

U configureert het VNet voor de nieuwe cache door op **virtueel netwerk** op de **nieuwe Redis-Cache** blade en selecteer de gewenste VNet uit de vervolgkeuzelijst.

![Virtueel netwerk][redis-cache-vnet]

Selecteer de gewenste subnet uit de **Subnet** vervolgkeuze-lijst, en geef de gewenste **statisch IP-adres**. Als u gebruikmaakt van een klassiek VNet en de **statisch IP-adres** veld is optioneel en als niets wordt opgegeven, wordt een gekozen uit het geselecteerde subnet.

> [!IMPORTANT]
> Bij het implementeren van een Azure Redis-Cache met een Resource Manager VNet, wordt de cache moet zich in een speciale subnet dat geen andere resources, met uitzondering van Azure Redis-Cache-exemplaren bevat. Als een poging is gedaan voor het implementeren van een Azure Redis-Cache op een Resource Manager VNet met een subnet met andere resources, mislukt de implementatie.
> 
> 

![Virtueel netwerk][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure bepaalde IP-adressen binnen elk subnet gereserveerd en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten die zijn gereserveerd voor protocol overeenstemming, samen met drie meer adressen voor Azure-services gebruikt. Zie voor meer informatie [zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Naast de IP-adressen die worden gebruikt door de Azure-VNET-infrastructuur, elke Redis-exemplaar in het subnet gebruikt twee IP-adressen per shard en één extra IP-adres voor de load balancer. Een niet-geclusterde cache wordt beschouwd als een shard hebben.
> 
> 

Nadat de cache is gemaakt, kunt u de configuratie voor het VNet weergeven door te klikken op **virtueel netwerk** van de **Resource menu**.

![Virtueel netwerk][redis-cache-vnet-info]

Voor verbinding met uw Azure Redis-cache-exemplaar bij gebruik van een VNet, geef de hostnaam van uw cache in de verbindingsreeks op zoals weergegeven in het volgende voorbeeld:

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

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis-Cache VNet Veelgestelde vragen
De volgende lijst bevat antwoorden op veelgestelde vragen over het schalen van Azure Redis-Cache.

* [Wat zijn enkele veelvoorkomende configuratiefouten bij Azure Redis-Cache en VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Hoe kan ik mijn cache werkt in een VNET controleren?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Kan ik VNets met een standaard of basic-cache gebruiken?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Waarom een Redis-cache niet in sommige subnetten maar geen andere?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Wat zijn de subnet-adresvereisten?](#what-are-the-subnet-address-space-requirements)
* [Werk alle functies van de cache bij het hosten van een cache in een VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Wat zijn enkele veelvoorkomende configuratiefouten bij Azure Redis-Cache en VNets?
Wanneer Azure Redis-Cache wordt gehost in een VNet, worden de poorten in de volgende tabellen worden gebruikt. 

>[!IMPORTANT]
>Als de poorten in de volgende tabellen zijn geblokkeerd, is het mogelijk dat de cache niet goed werkt. Met een of meer van deze poorten geblokkeerd is het meest voorkomende onjuiste configuratie probleem wanneer u Azure Redis-Cache in een VNet.
> 
> 

- [Vereisten voor de uitgaande poort](#outbound-port-requirements)
- [Vereisten voor de binnenkomende poort](#inbound-port-requirements)

### <a name="outbound-port-requirements"></a>Vereisten voor de uitgaande poort

Er zijn zeven vereisten van de uitgaande poort.

- Zo kunnen gewenste, alle uitgaande verbindingen met het internet worden gemaakt via een client on-premises controlemogelijkheden apparaten.
- Drie van de poorten routeren verkeer naar Azure-eindpunten onderhoud Azure Storage en Azure DNS.
- De resterende bereikwaarden en voor interne communicatie voor Redis-subnet. Er is geen subnet NSG-regels zijn vereist voor interne communicatie voor Redis-subnet.

| Poort(en) | Richting | Protocol-Transport | Doel | Lokaal IP | Externe IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Uitgaand |TCP |Redis-afhankelijkheden op Azure Storage/PKI (Internet) | (Redis subnet) |* |
| 53 |Uitgaand |TCP/UDP |Afhankelijkheden van DNS (Internet/VNet) redis | (Redis subnet) |* |
| 8443 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) | (Redis subnet) |
| 10221-10231 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) | (Redis subnet) |
| 20226 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 13000-13999 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 15000-15999 |Uitgaand |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |


### <a name="inbound-port-requirements"></a>Vereisten voor de binnenkomende poort

Er zijn acht vereisten voor het bereik van binnenkomende poort. Inkomende aanvragen in deze bereiken zijn inkomende verkeer van andere services die worden gehost in hetzelfde VNET of interne tot de Redis-subnet-communicatie.

| Poort(en) | Richting | Protocol-Transport | Doel | Lokaal IP | Externe IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Inkomend |TCP |Communicatie van clients met Redis, Azure load balancing | (Redis subnet) |Virtueel netwerk, Azure Load Balancer |
| 8443 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |
| 8500 |Inkomend |TCP/UDP |Azure load balancing | (Redis subnet) |Azure Load Balancer |
| 10221-10231 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet), Load Balancer van Azure |
| 13000-13999 |Inkomend |TCP |Communicatie van clients met Redis-Clusters, Azure load balancing | (Redis subnet) |Virtueel netwerk, Azure Load Balancer |
| 15000-15999 |Inkomend |TCP |Clientcommunicatie met Redis-Clusters, Azure load Balancing | (Redis subnet) |Virtueel netwerk, Azure Load Balancer |
| 16001 |Inkomend |TCP/UDP |Azure load balancing | (Redis subnet) |Azure Load Balancer |
| 20226 |Inkomend |TCP |Interne communicatie voor Redis | (Redis subnet) |(Redis subnet) |

### <a name="additional-vnet-network-connectivity-requirements"></a>Aanvullende VNET-connectiviteit netwerkvereisten

Er zijn verbindingsproblemen netwerkvereisten voor Azure Redis-Cache kan niet in eerste instantie in een virtueel netwerk worden voldaan. Azure Redis-Cache is vereist voor de volgende items goed te laten functioneren wanneer binnen een virtueel netwerk wordt gebruikt.

* Uitgaande netwerkverbinding naar Azure Storage-eindpunten die over de hele wereld. Dit omvat eindpunten zich in dezelfde regio bevinden als het exemplaar van Azure Redis-Cache, evenals de storage-eindpunten zich in **andere** Azure-regio's. Azure Storage-eindpunten omgezet onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*, en *file.core.windows.net*. 
* Uitgaande netwerkverbinding met *ocsp.msocsp.com*, *mscrl.microsoft.com*, en *crl.microsoft.com*. Deze connectiviteit is nodig om SSL-functionaliteit te ondersteunen.
* De DNS-configuratie voor het virtuele netwerk moet geschikt is voor het oplossen van alle eindpunten en domeinen die worden vermeld in de eerdere punten. Deze DNS-vereisten kunnen worden voldaan door te zorgen voor een geldig DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.
* Uitgaande netwerkverbinding met de volgende Azure Monitoring-eindpunten die onder de volgende DNS-domeinen oplossen: shoebox2 black.shoebox2.metrics.nsatc.net, Noord prod2.prod2.metrics.nsatc.net, azglobal black.azglobal.metrics.nsatc.net, shoebox2 red.shoebox2.metrics.nsatc.net, Oost-prod2.prod2.metrics.nsatc.net, azglobal red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Hoe kan ik mijn cache werkt in een VNET controleren?

>[!IMPORTANT]
>Wanneer u verbinding maakt met een Azure Redis-Cache-exemplaar dat wordt gehost in een VNET, moet uw cacheclients in hetzelfde VNET, met inbegrip van de testtoepassingen of diagnostische hulpprogramma's voor pingen.
>
>

Nadat de Poortvereisten zijn geconfigureerd zoals beschreven in de vorige sectie, kunt u controleren of uw cache werkt door de volgende stappen uit te voeren.

- [Opnieuw opstarten](cache-administration.md#reboot) alle knooppunten van het cache. Als alle cacheafhankelijkheden vereist kan niet worden bereikt (zoals beschreven in [Poortvereisten voor binnenkomende](cache-how-to-premium-vnet.md#inbound-port-requirements) en [uitgaande Poortvereisten](cache-how-to-premium-vnet.md#outbound-port-requirements)), de cache op te starten is niet mogelijk.
- Nadat de cache-knooppunten opnieuw hebt opgestart (zoals gemeld door de status van de cache in de Azure portal), kunt u de volgende tests uitvoeren:
  - Ping de cache-eindpunt (met behulp van poort 6380) vanaf een machine die zich binnen hetzelfde VNET als de cache met behulp van [tcping](https://www.elifulkerson.com/projects/tcping.php). Bijvoorbeeld:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Als de `tcping` tool meldt dat de poort is geopend, wordt de cache is beschikbaar voor de verbinding van clients in het VNET.

  - Testen op een andere manier is het maken van een test-cacheclient (die mogelijk een eenvoudige consoletoepassing met StackExchange.Redis) die verbinding maakt met de cache en wordt toegevoegd en sommige items opgehaald uit de cache. Installeer de voorbeeld-clienttoepassing op een virtuele machine die zich in hetzelfde VNET als de cache en uitvoeren om te controleren of de verbinding met de cache.


### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kan ik VNets met een standaard of basic-cache gebruiken?
VNets kan alleen worden gebruikt met premium-caches.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Waarom een Redis-cache niet in sommige subnetten maar geen andere?
Als u een Azure Redis-Cache met een Resource Manager VNet implementeert, wordt de cache moet zich in een speciale subnet op dat er is geen resourcetype bevat. Als een poging is gedaan voor het implementeren van een Azure Redis-Cache met een Resource Manager VNet subnet die andere resources bevat, mislukt de implementatie. Voordat u een nieuwe Redis-cache kunt maken, moet u de bestaande resources binnen het subnet verwijderen.

U kunt meerdere typen resources implementeren op een klassiek VNet, zolang er voldoende beschikbare IP-adressen.

### <a name="what-are-the-subnet-address-space-requirements"></a>Wat zijn de subnet-adresvereisten?
Azure bepaalde IP-adressen binnen elk subnet gereserveerd en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten die zijn gereserveerd voor protocol overeenstemming, samen met drie meer adressen voor Azure-services gebruikt. Zie voor meer informatie [zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Naast de IP-adressen die worden gebruikt door de Azure-VNET-infrastructuur, elke Redis-exemplaar in het subnet gebruikt twee IP-adressen per shard en één extra IP-adres voor de load balancer. Een niet-geclusterde cache wordt beschouwd als een shard hebben.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Werk alle functies van de cache bij het hosten van een cache in een VNET?
Wanneer uw cache deel van een VNET uitmaakt, alleen de clients in het VNET, hebben toegang tot de cache. Als gevolg hiervan werken de volgende functies van de cache-management niet op dit moment.

* Console redis - omdat Redis-Console wordt uitgevoerd in uw lokale browser, die zich buiten het VNET, er kan geen verbinding met uw cache.

## <a name="use-expressroute-with-azure-redis-cache"></a>ExpressRoute gebruiken met Azure Redis-Cache
Klanten kunnen verbinding maken met een [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuit met hun virtuele netwerkinfrastructuur, waardoor hun on-premises netwerk uitbreiden naar Azure. 

Standaard een nieuwe ExpressRoute-circuit voert geen geforceerde tunneling (aankondiging van een standaardroute 0.0.0.0/0) op een VNET. Als gevolg hiervan uitgaande internetverbinding is toegestaan rechtstreeks vanuit het VNET en clienttoepassingen kunnen geen verbinding maken met andere Azure-eindpunten met inbegrip van Azure Redis-Cache.

Een algemene configuratie van de klant is echter gebruik van geforceerde tunneling (adverteren een standaardroute) waardoor uitgaand internetverkeer in plaats daarvan lokale stromen. Dit netwerkverkeer connectiviteit met Azure Redis-Cache wordt verbroken als het uitgaande verkeer vervolgens lokale geblokkeerd, zodat de Azure Redis-Cache-exemplaar is niet kan communiceren met de bijbehorende afhankelijkheden.

De oplossing is voor het definiëren van een (of meer) gebruiker gedefinieerde routes (udr's) op het subnet waarin de Azure Redis-Cache. Een UDR definieert subnet-specifieke routes die in plaats van de standaardroute gehonoreerd.

Het verdient indien mogelijk, gebruik de volgende configuratie:

* Configuratie van de ExpressRoute kondigt 0.0.0.0/0 en standaard force alle uitgaande verkeer lokale tunnels.
* De UDR toegepast op het subnet met de Azure Redis-Cache definieert 0.0.0.0/0 met een werkende route voor TCP/IP-verkeer naar het openbare internet; bijvoorbeeld instellen door het volgende hoptype met 'Internet'.

Het gecombineerde effect van deze stappen is dat subnetniveau UDR voorrang op de ExpressRoute geforceerde tunneling heeft, zodat uitgaande toegang tot Internet vanaf de Azure Redis-Cache.

Via een on-premises toepassing met behulp van ExpressRoute verbinding maakt met een Azure Redis-Cache-exemplaar is niet een typische gebruiksscenario om prestatieredenen (voor de beste prestaties Azure Redis-Cache clients in dezelfde regio bevinden als de Azure Redis-Cache moeten).

>[!IMPORTANT] 
>De routes die zijn gedefinieerd in een UDR **moet** specifiek genoeg is, hebben voorrang boven alle routes die worden geadverteerd door de ExpressRoute-configuratie. Het volgende voorbeeld het adresbereik brede 0.0.0.0/0 gebruikt en als zodanig kan mogelijk worden per ongeluk genegeerd door route-advertisements met behulp van specifieke adresbereiken.

>[!WARNING]  
>Azure Redis-Cache wordt niet ondersteund met ExpressRoute-configuraties die **onjuist cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering**. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangt de route-advertisements van Microsoft voor een groot aantal Microsoft Azure-IP-adresbereiken. Als deze adresbereiken onjuist cross aangekondigd op het pad voor persoonlijke peering, wordt het resultaat is dat alle uitgaande pakketten van de Azure Redis-Cache-exemplaar subnet onjuist force via een tunnel naar een klant on-premises netwerkinfrastructuur zijn. Deze stroom van het netwerk verbreekt Azure Redis-Cache. De oplossing voor dit probleem is cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen.


Achtergrondinformatie over de gebruiker gedefinieerde routes zijn beschikbaar in deze [overzicht](../virtual-network/virtual-networks-udr-overview.md).

Zie voor meer informatie over ExpressRoute [technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer Premiumfuncties cache.

* [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

