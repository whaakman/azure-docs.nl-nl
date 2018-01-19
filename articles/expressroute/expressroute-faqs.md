---
title: Veelgestelde vragen voor Azure ExpressRoute | Microsoft Docs
description: De veelgestelde vragen over de ExpressRoute bevat informatie over ondersteunde Azure-Services, kosten, gegevens en verbindingen, SLA, Providers en locaties, bandbreedte en aanvullende technische gegevens.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 09b17bc4-d0b3-4ab0-8c14-eed730e1446e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 62e532f3750adf8f4defe3e8f8aabe5b9f0446a0
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="expressroute-faq"></a>Veelgestelde vragen ExpressRoute

## <a name="what-is-expressroute"></a>Wat is ExpressRoute?

ExpressRoute is een Azure-service waarmee u particuliere verbindingen maken tussen Microsoft-datacenters en infrastructuur on-premises of in een functie van de plaatsing. ExpressRoute-verbindingen niet via het openbare Internet en bieden betere beveiliging, betrouwbaarheid en snelheden met lagere latenties dan gewone verbindingen via Internet.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>Wat zijn de voordelen van het gebruik van ExpressRoute- en particuliere netwerkverbindingen?

ExpressRoute-verbindingen gaan niet via het openbare internet. Ze bieden betere beveiliging, betrouwbaarheid en snelheden, met lagere en consistente latenties dan gewone verbindingen via Internet. In sommige gevallen gebruik van ExpressRoute-verbindingen voor het overbrengen van gegevens tussen on-premises apparaten en Azure aanzienlijke kostenvoordelen kan worden verkregen.

### <a name="where-is-the-service-available"></a>Waar de service beschikbaar is?

Deze pagina en beschikbaarheid van servicelocatie: [ExpressRoute-partners en locaties](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Hoe kan ik ExpressRoute verbinding maakt met Microsoft als er geen samenwerking met een van de provider van ExpressRoute-partners gebruiken?

U kunt een regionale luchtvaartmaatschappij selecteren en land Ethernet-verbindingen op een van de uitwisseling van de ondersteunde locaties van de provider. U kunt vervolgens peer met Microsoft op de locatie van de provider. Controleer de laatste sectie van [ExpressRoute-partners en locaties](expressroute-locations.md) om te controleren of uw serviceprovider aanwezig is in een van de exchange-locaties. Vervolgens kunt u een ExpressRoute-circuit via de serviceprovider verbinding maken met Azure bestellen.

### <a name="how-much-does-expressroute-cost"></a>Wat kost ExpressRoute?

Controleer [prijsinformatie](https://azure.microsoft.com/pricing/details/expressroute/) voor informatie over prijzen.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Als ik voor een ExpressRoute-circuit van een bepaalde bandbreedte betalen, de VPN-verbinding die ik bij mijn netwerkprovider aanschaffen hoeft te zijn dezelfde snelheid?

Nee. U kunt een VPN-verbinding met een snelheid aanschaffen bij uw serviceprovider. De verbinding met Azure is echter beperkt tot de bandbreedte van het ExpressRoute-circuit die u aanschaft.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-necessary"></a>Als ik voor een ExpressRoute-circuit van een bepaalde bandbreedte betalen, heb ik de mogelijkheid om te burst tot hogere snelheden, indien nodig?

Ja. ExpressRoute-circuits zijn geconfigureerd zodat u kunt maximaal twee keer de Bandbreedtelimiet die u zonder extra kosten aangeschaft burst. Neem contact op met uw serviceprovider om te zien als deze functie wordt ondersteund.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Kan ik tegelijkertijd dezelfde particulier netwerkverbinding met het virtuele netwerk en andere Azure-services gebruiken?

Ja. Een ExpressRoute-circuit nadat deze is ingesteld, kunt u tegelijkertijd toegang hebben tot services binnen een virtueel netwerk en andere Azure-services. U verbinding maken met virtuele netwerken via het pad voor persoonlijke peering en andere services via het pad voor openbare peering.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Biedt ExpressRoute een Service Level Agreement (SLA)?

Zie voor meer informatie, de [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) pagina.

## <a name="supported-services"></a>Ondersteunde services

ExpressRoute ondersteunt [drie Routeringsdomeinen](expressroute-circuit-peerings.md) voor verschillende soorten services.

### <a name="private-peering"></a>Persoonlijke peering

* Virtuele netwerken, inclusief alle virtuele machines en cloud-services

### <a name="public-peering"></a>Openbare peering

* Power BI
* Dynamics 365 voor Financiën en bewerkingen (voorheen bekend als Dynamics AX Online)
* De meeste van de Azure-services, met de volgende enkele uitzonderingen:
  * CDN
  * Visual Studio Team Services Load Testing
  * Multi-Factor Authentication
  * Traffic Manager

### <a name="microsoft-peering"></a>Microsoft-peering

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Dynamics 365 betrokkenheid van de klant-toepassingen (voorheen bekend als CRM Online)
  * Dynamics 365 voor verkoop
  * Dynamics 365 voor klantenservice
  * Dynamics 365 voor veld Service
  * Dynamics 365 voor Project-Service
* Met behulp van [routefilters](#route-filters-for-microsoft-peering), krijgt u toegang tot de dezelfde openbare services met de Microsoft-peering:
  * Power BI
  * Dynamics 365 voor Financiën en bewerkingen
  * De meeste van de Azure-services, met de volgende enkele uitzonderingen:
    * CDN
    * Visual Studio Team Services Load Testing
    * Multi-Factor Authentication
    * Traffic Manager

## <a name="data-and-connections"></a>Gegevens en verbindingen

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>Zijn er beperkingen met betrekking tot de hoeveelheid gegevens die ik kunnen worden overgedragen met behulp van ExpressRoute?

We Stel een limiet van de hoeveelheid gegevens overgedragen. Raadpleeg [prijsinformatie](https://azure.microsoft.com/pricing/details/expressroute/) voor meer informatie over de tarieven voor bandbreedte.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Welke verbindingssnelheden worden ondersteund door ExpressRoute?

Ondersteunde bandbreedte-aanbiedingen:

50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps

### <a name="which-service-providers-are-available"></a>Welke serviceproviders zijn beschikbaar?

Zie [ExpressRoute-partners en locaties](expressroute-locations.md) voor de lijst met serviceproviders en locaties.

## <a name="technical-details"></a>Technische details

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Wat zijn de technische vereisten voor het verbinden van mijn on-premises-locatie met Azure?

Zie [ExpressRoute vereisten pagina](expressroute-prerequisites.md) voor vereisten.

### <a name="are-connections-to-expressroute-redundant"></a>Verbindingen met ExpressRoute redundante zijn?

Ja. Elk ExpressRoute-circuit heeft een redundante paar cross-verbindingen die zijn geconfigureerd voor maximale beschikbaarheid.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Ik verliest als een van mijn ExpressRoute-koppelingen mislukken?

U wordt niet de verbinding verbroken als een van de verbindingen tussen mislukt. Is een redundante verbinding beschikbaar ter ondersteuning van de belasting van uw netwerk en bieden hoge beschikbaarheid van uw ExpressRoute-circuit. Bovendien kunt u een circuit maken in een andere peering locatie herstelmogelijkheden circuit niveau bereiken.

### <a name="how-do-i-ensure-high-availability-on-a-virtual-network-connected-to-expressroute"></a>Hoe kan ik ervoor zorgen dat hoge beschikbaarheid op een virtueel netwerk is verbonden met ExpressRoute?

U kunt de hoge beschikbaarheid door verbinding te maken van ExpressRoute-circuits in verschillende peeringlocaties (bijvoorbeeld Singapore, Singapore2) bereiken met het virtuele netwerk. Als één ExpressRoute-circuit uitvalt, wordt connectiviteit schakelt over naar een ander ExpressRoute-circuit. Het virtuele netwerk uitgaand verkeer wordt doorgestuurd op basis van op gelijke kosten multi path Routing (ECMP) standaard. U kunt verbinding gewicht één circuit liever naar een andere. Zie [ExpressRoute-routering optimaliseren](expressroute-optimize-routing.md) voor meer informatie over het gewicht van de verbinding.

### <a name="onep2plink"></a>Als ik wil niet CO-locatie op een cloudexchange en mijn serviceprovider point-to-point-verbinding biedt, heb ik nodig om twee fysieke verbindingen tussen mijn on-premises netwerk en Microsoft?

Als uw serviceprovider via de fysieke verbinding tot stand twee virtuele Ethernet-circuits brengen kan, hoeft u slechts één fysieke verbinding. De fysieke verbinding (bijvoorbeeld een Network) wordt beëindigd op een laag 1 (L1)-apparaat (Zie de afbeelding). De twee virtuele Ethernet-circuits zijn gelabeld met verschillende VLAN-id, één voor het primaire circuit en één voor de secundaire. Deze VLAN-id's zijn in de buitenste 802.1Q Ethernet-header. De interne 802.1Q Ethernet-header (niet weergegeven) is toegewezen aan een specifieke [ExpressRoute routeringsdomein](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Kan ik uitbreiden een van mijn VLAN's naar Azure met behulp van ExpressRoute?

Nee. Wij ondersteunen geen layer 2-connectiviteit uitbreidingen in Azure.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Kan ik meer dan één ExpressRoute-circuit hebben in mijn abonnement?

Ja. U kunt meer dan één ExpressRoute-circuit hebben in uw abonnement. De standaardlimiet is ingesteld op 10. U kunt contact opnemen met Microsoft Support Verhoog de limiet, indien nodig.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Kan ik ExpressRoute-circuits van verschillende serviceproviders hebben?

Ja. U kunt ExpressRoute-circuits hebben met veel serviceproviders. Elk ExpressRoute-circuit is gekoppeld aan een serviceprovider. 

### <a name="i-see-two-expressroute-peering-locations-in-the-same-metro-eg-singapore-and-singapore2-which-peering-location-should-i-choose-to-create-my-expressroute-circuit"></a>Er worden twee ExpressRoute-peeringlocaties in de dezelfde metro bijvoorbeeld Singapore en Singapore2. Welke peeringlocatie moet ik kiezen voor het maken van mijn ExpressRoute-circuit?
Als uw serviceprovider ExpressRoute op beide sites biedt, kunt u werken met uw provider en de site voor het instellen van ExpressRoute kiezen. 

### <a name="can-i-have-multiple-expressroute-circuits-in-the-same-metro-can-i-link-them-to-the-same-virtual-network"></a>Kan ik meerdere ExpressRoute-circuits hebben in de dezelfde metro? Kan ik deze koppelen aan hetzelfde virtuele netwerk?

Ja. U kunt meerdere ExpressRoute-circuits hebben met de dezelfde of verschillende serviceproviders. Als de metro meerdere ExpressRoute-peeringlocaties heeft en de circuits op verschillende peeringlocaties zijn gemaakt, kunt u ze kunt koppelen aan hetzelfde virtuele netwerk. Als de circuits worden gemaakt op dezelfde locatie peering, kunt u ze niet koppelen aan hetzelfde virtuele netwerk.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Hoe kan ik mijn virtuele netwerken verbinden met een ExpressRoute-circuit

De eenvoudige stappen zijn:

* Een ExpressRoute-circuit tot stand brengen en hebben de serviceprovider inschakelen.
* U of de provider moet de BGP peering (s) configureren.
* Het virtuele netwerk koppelen aan het ExpressRoute-circuit.

Zie voor meer informatie [ExpressRoute-werkstromen voor circuitinrichting en circuitstatussen](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Zijn er connectiviteit grenzen voor mijn ExpressRoute-circuit?

Ja. De [ExpressRoute-partners en locaties](expressroute-locations.md) artikel bevat een overzicht van de grenzen van de verbinding voor een ExpressRoute-circuit. Connectiviteit voor een ExpressRoute-circuit is beperkt tot één geopolitieke regio. Connectiviteit kan cross geopolitieke regio's door het inschakelen van de ExpressRoute premium-functie worden uitgebreid.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Kan ik koppelen aan meer dan één virtueel netwerk aan een ExpressRoute-circuit?

Ja. U kunt maximaal 10 virtuele netwerken verbindingen op een standaard ExpressRoute-circuit en maximaal 100 hebben op een [premium ExpressRoute-circuit](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Ik heb meerdere Azure-abonnementen die virtuele netwerken bevatten. Kan ik virtuele netwerken die zich in afzonderlijke abonnementen op één ExpressRoute-circuit verbinden?

Ja. U kunt maximaal 10 andere Azure-abonnementen gebruiken één ExpressRoute-circuit autoriseren. Deze limiet kan worden verhoogd met het inschakelen van de ExpressRoute premium-functie.

Zie voor meer informatie [een ExpressRoute-circuit delen tussen meerdere abonnementen](expressroute-howto-linkvnet-arm.md).

### <a name="i-have-multiple-azure-subscriptions-associated-to-different-azure-active-directory-tenants-or-enterprise-agreement-enrollments-can-i-connect-virtual-networks-that-are-in-separate-tenants-and-enrollments-to-a-single-expressroute-circuit-not-in-the-same-tenant-or-enrollment"></a>Ik heb meerdere Azure-abonnementen die is gekoppeld aan andere Azure Active Directory-tenants of Enterprise Agreement inschrijvingen. Kan ik virtuele netwerken die zich in afzonderlijke tenants en inschrijvingen van één ExpressRoute-circuit niet in dezelfde tenant of inschrijving verbinden?

Ja. ExpressRoute autorisaties kunnen abonnement, tenant en de inschrijving grenzen omvatten zonder aanvullende configuratie vereist. 

Zie voor meer informatie [een ExpressRoute-circuit delen tussen meerdere abonnementen](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Virtuele netwerken verbinding hebt met hetzelfde circuit van elkaar zijn afgezonderd?

Nee. Vanuit het routering oogpunt van alle virtuele netwerken die zijn gekoppeld aan hetzelfde ExpressRoute-circuit deel uitmaken van hetzelfde routeringsdomein en zijn niet van elkaar geïsoleerd. Als u route isolatie nodig hebt, moet u een afzonderlijke ExpressRoute-circuit maken.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Kan ik een virtueel netwerk is verbonden met meer dan één ExpressRoute-circuit hebben?

Ja. U kunt één virtueel netwerk met maximaal vier ExpressRoute-circuits koppelen. Ze moeten worden geordend tot en met 4 verschillende [ExpressRoute-locaties](expressroute-locations.md).

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>Kan ik toegang tot het Internet van mijn virtuele netwerken die zijn verbonden met ExpressRoute-circuits?

Ja. Als u hebt niet aangekondigd standaardroutes (0.0.0.0/0) of Internet route adresvoorvoegsels via de BGP-sessie, kunt u verbinding met het Internet vanaf een virtueel netwerk is gekoppeld aan een ExpressRoute-circuit.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Kan ik een internetverbinding naar virtuele netwerken die zijn verbonden met ExpressRoute-circuits blokkeren?

Ja. U kunt standaardroutes bekendmaakt (0.0.0.0/0) voor het blokkeren van alle Internet-verbindingen naar virtuele machines die zijn geïmplementeerd in een virtueel netwerk en alle verkeer van via het ExpressRoute-circuit te routeren.

Als u standaardroutes bekendmaakt, dwingt we verkeer in de services die worden aangeboden via openbare peering (zoals Azure-opslag en SQL-database) terug naar uw bedrijf. U moet de routers om terug te keren verkeer naar Azure via het pad voor openbare peering of via Internet configureren. Als u een service-eindpunt (preview) voor de service hebt ingeschakeld, wordt het verkeer naar de service niet op uw locatie geforceerd. Het verkeer blijft in het Azure-backbone-netwerk. Zie voor meer informatie over service-eindpunten, [Virtual network service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md?toc=%2fazure%2fexpressroute%2ftoc.json)

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Kunnen virtuele netwerken die zijn gekoppeld aan hetzelfde ExpressRoute-circuit met elkaar communiceren?

Ja. Virtuele machines die worden geïmplementeerd in virtuele netwerken die zijn verbonden met hetzelfde ExpressRoute-circuit kunnen met elkaar communiceren.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Kan ik site-naar-site-connectiviteit voor virtuele netwerken in combinatie met ExpressRoute gebruiken?

Ja. ExpressRoute kan worden gecombineerd met site-naar-site VPN-verbindingen.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Kan ik een virtueel netwerk verplaatsen van site-naar-site / punt-naar-site configuratie ExpressRoute gebruiken?

Ja. U moet een ExpressRoute-gateway in uw virtuele netwerk maken. Er is een kleine uitvaltijd gekoppeld aan het proces.

### <a name="why-is-there-a-public-ip-address-associated-with-the-expressroute-gateway-on-a-virtual-network"></a>Waarom is er een openbaar IP-adres die zijn gekoppeld aan de ExpressRoute-gateway op een virtueel netwerk?

Het openbare IP-adres wordt gebruikt voor interne management alleen. Dit openbare IP-adres niet wordt blootgesteld aan Internet en geen vormt een beveiligingsrisico van het virtuele netwerk.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>Wat moet ik via ExpressRoute verbinding maken met Azure storage?

U moet een ExpressRoute-circuit maken en configureren van routes voor openbare peering.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Zijn er beperkingen met betrekking tot het aantal routes die ik kunt adverteren?

Ja. We accepteren maximaal 4000 voorvoegsels van de route voor persoonlijke peering en 200 voor openbare peering en Microsoft-peering. U kunt deze verhogen naar 10.000 routes voor persoonlijke peering als u de ExpressRoute premium-functie inschakelen.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>Zijn er beperkingen voor IP-adresbereiken die ik via de BGP-sessie kunt adverteren?

We accepteren geen persoonlijke voorvoegsels (RFC1918) in de openbare en Microsoft-peering BGP-sessie.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Wat gebeurt er als ik het BGP overschrijdt beperkt?

BGP-sessies worden verwijderd. Ze worden opnieuw ingesteld zodra het aantal voorvoegsel onder de limiet gaat.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Wat is de tijd van de wachtstand ExpressRoute BGP? Kan het worden aangepast?

De tijd van de blokkering is 180. De keepalive-berichten worden verzonden om de 60 seconden. Deze instellingen vast aan de kant van Microsoft die niet kan worden gewijzigd. Het is mogelijk andere timers configureren en de parameters van BGP-sessie wordt onderhandeld over dienovereenkomstig.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>Wanneer ik de standaardroute (0.0.0.0/0) naar Mijn virtuele netwerken adverteren, kan ik mijn Azure VM's waarop Windows niet activeren. Hoe naar ik dit oplossen?

De volgende stappen helpen Azure de activeringsaanvraag herkend:

1. Stel de openbare peering voor uw ExpressRoute-circuit.
2. Een DNS-zoekopdracht uitvoeren en het IP-adres vinden **kms.core.windows.net**
3. De Key Management Service moet herkennen dat de activeringsaanvraag afkomstig zijn van Azure en eer de aanvraag. Voer een van de volgende drie taken:

   * Op uw on-premises netwerk door het verkeer dat is bestemd voor het IP-adres dat u hebt verkregen in stap 2 terug naar Azure via openbare peering te sturen.
   * Het verkeer naar Azure via openbare peering voor uw haar NSP provider-pincode hebben.
   * Een gebruiker gedefinieerde route die het IP-adres dat Internet als een volgende hop verwijst maken en toepassen op de subnetten waar deze virtuele machines zijn.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Kan ik de bandbreedte van een ExpressRoute-circuit wijzigen?

Ja, u kunt proberen om te verhogen van de bandbreedte van uw ExpressRoute-circuit in de Azure portal of met behulp van PowerShell. Als er capaciteit is beschikbaar op de fysieke poort waarop uw circuit is gemaakt, wordt uw wijziging slaagt. 

Als de wijziging is mislukt, betekent dit dat ofwel er niet voldoende capaciteit links op de huidige poort en moet u een nieuwe ExpressRoute-circuit maken met de hogere bandbreedte of dat er is geen extra capaciteit op die locatie, in welk geval u niet mogelijk om te verhogen van de bandbreedte. 

U moet ook Neem contact op met uw connectiviteitsprovider om ervoor te zorgen dat ze de vertragingen in hun netwerken ter ondersteuning van de bandbreedte toename bijwerkt. U kunt de bandbreedte van uw ExpressRoute-circuit echter niet, reduceren. U hebt een nieuwe ExpressRoute-circuit met lagere bandbreedte maken en verwijderen van het oude circuit.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Hoe wijzig ik de bandbreedte van een ExpressRoute-circuit?

U kunt de bandbreedte van het ExpressRoute-circuit met de REST-API of PowerShell-cmdlet kunt bijwerken.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>Wat is ExpressRoute premium?

ExpressRoute premium is een verzameling van de volgende functies:

* Routering tabel heeft limiet verhoogd van 4000 routes naar 10.000 routes voor persoonlijke peering.
* Aantal VNets die kunnen worden verbonden met het ExpressRoute-circuit verhoogd (de standaardwaarde is 10). Zie voor meer informatie de [ExpressRoute limieten](#limits) tabel.
* De verbinding met Office 365 en Dynamics 365.
* Globale connectiviteit via het netwerk van Microsoft core. U kunt nu een VNet in een geopolitieke regio een ExpressRoute-circuit in een andere regio koppelen.<br>
    **Voorbeelden:**

    *  U kunt een VNet gemaakt in West-Europa aan een ExpressRoute-circuit dat is gemaakt in Silicon Valley koppelen. 
    *  Voorvoegsels van andere geopolitieke regio's zijn via openbare peering geadverteerd zodat u op verbinding, bijvoorbeeld SQL Azure in West-Europa een circuit in Silicon Valley maken kan.


### <a name="limits"></a>Hoeveel VNets kan ik koppelen aan een ExpressRoute-circuit als ik ExpressRoute premium ingeschakeld?

De volgende tabellen tonen de ExpressRoute-limieten en het aantal VNets per ExpressRoute-circuit:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>Hoe kan ik ExpressRoute premium inschakelen?

ExpressRoute premium-functies kunnen worden ingeschakeld wanneer de functie is ingeschakeld en kunnen worden afgesloten door het bijwerken van de status van het circuit. U kunt ExpressRoute premium inschakelen tijdens de aanmaak van circuit of de REST-API kan aanroepen / PowerShell-cmdlet.

### <a name="how-do-i-disable-expressroute-premium"></a>Hoe kan ik ExpressRoute premium uitschakelen?

U kunt ExpressRoute premium uitschakelen door het aanroepen van de REST-API of PowerShell-cmdlet. U moet ervoor zorgen dat u de verbinding moet voldoen aan de standaardlimiet voordat u ExpressRoute premium uitschakelen hebt aangepast. Als het gebruik van uw schaalt vallen buiten de grenzen van de standaard, wordt de aanvraag voor het uitschakelen van ExpressRoute premium mislukt.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Kan ik Kies de gewenste van de premium-functieset functies?

Nee. De functies, kan niet worden opgenomen. We kunnen alle functies inschakelen wanneer u ExpressRoute premium inschakelen.

### <a name="how-much-does-expressroute-premium-cost"></a>Wat kost ExpressRoute premium?

Raadpleeg [prijsinformatie](https://azure.microsoft.com/pricing/details/expressroute/) voor kosten.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Betaal ik voor ExpressRoute premium naast de standaard ExpressRoute kosten?

Ja. ExpressRoute premium gelden boven op het ExpressRoute-circuit kosten en kosten vereist door de connectiviteitsprovider.

## <a name="expressroute-for-office-365-and-dynamics-365"></a>ExpressRoute voor Office 365 en Dynamics 365

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-dynamics-365"></a>Hoe maak ik een ExpressRoute-circuit verbinding maken met Office 365-services en Dynamics 365?

1. Controleer de [ExpressRoute vereisten pagina](expressroute-prerequisites.md) om ervoor te zorgen dat u voldoet aan de vereisten.
2. Om ervoor te zorgen dat aan de behoeften van uw connectiviteit wordt voldaan, bekijk de lijst met serviceproviders en locaties in de [ExpressRoute-partners en locaties](expressroute-locations.md) artikel.
3. Aan de hand van plan bent uw capaciteitsvereisten [netwerkplanning en prestatieafstemming voor Office 365](http://aka.ms/tune/).
4. Volg de stappen die worden vermeld in de werkstromen voor het instellen van connectiviteit [ExpressRoute-werkstromen voor circuitinrichting en circuitstatussen](expressroute-workflows.md).

> [!IMPORTANT]
> Zorg ervoor dat u premium-invoegtoepassing voor ExpressRoute hebt ingeschakeld bij het configureren van de verbinding met Office 365-services en Dynamics 365.
> 
> 

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-dynamics-365"></a>Heb ik nodig om in te schakelen openbare Azure-peering voor verbinding met Office 365-services en Dynamics 365?

Nee, moet u alleen om in te schakelen van Microsoft-Peering. Verificatieverkeer naar Azure AD wordt verzonden via Microsoft-Peering. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-dynamics-365"></a>Biedt ondersteuning voor mijn bestaande ExpressRoute-circuits verbinding met Office 365-services en Dynamics 365?

Ja. Uw bestaande ExpressRoute-circuit kan worden geconfigureerd ter ondersteuning van de verbinding met Office 365-services. Zorg ervoor dat u hebt onvoldoende capaciteit om te verbinden met Office 365-services en dat u premium-invoegtoepassing hebt ingeschakeld. [Netwerkplanning en prestatieafstemming voor Office 365](http://aka.ms/tune/) helpt u van plan bent uw verbinding nodig heeft. Zie ook [maken en een ExpressRoute-circuit wijzigen](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Welke Office 365 services toegankelijk is via een ExpressRoute-verbinding?

Raadpleeg [Office 365-URL's en IP-adresbereiken](http://aka.ms/o365endpoints) pagina voor een bijgewerkte lijst met services die via ExpressRoute worden ondersteund.

### <a name="how-much-does-expressroute-for-office-365-services-and-dynamics-365-cost"></a>Hoeveel ondersteunt ExpressRoute voor Office 365-services en de kosten van Dynamics 365?

Office 365-services en Dynamics 365 moet premium-invoegtoepassing moet worden ingeschakeld. Zie de [detailpagina met prijzen](https://azure.microsoft.com/pricing/details/expressroute/) voor kosten.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Welke regio's wordt ExpressRoute voor Office 365 ondersteund?

Zie [ExpressRoute-partners en locaties](expressroute-locations.md) voor meer informatie.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>Heb ik toegang tot Office 365 via Internet, zelfs als ExpressRoute is geconfigureerd voor mijn organisatie?

Ja. Office 365-service-eindpunten zijn bereikbaar is via het Internet, hoewel ExpressRoute is geconfigureerd voor uw netwerk. Als u zich in een locatie die is geconfigureerd voor verbinding met Office 365-services via ExpressRoute, maakt u verbinding via ExpressRoute.

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Kan ik Office 365 US Government Community (GCC) services openen via een Azure US Government ExpressRoute-circuit?

Ja. Office 365 GCC service-eindpunten kunnen worden bereikt via het Azure US Government ExpressRoute. Echter, moet u eerst een ondersteuningsticket in de Azure portal om de voorvoegsels die u wilt adverteren naar Microsoft te openen. De verbinding met Office 365 GCC services worden vastgesteld na de ondersteuningsticket is opgelost. 

### <a name="can-dynamics-365-for-operations-formerly-known-as-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics 365 voor bewerkingen (voorheen bekend als Dynamics AX Online) toegankelijk is via een ExpressRoute-verbinding?

Ja. [Dynamics 365 voor bewerkingen](https://www.microsoft.com/dynamics365/operations) wordt gehost op Azure. U kunt inschakelen om openbare Azure-peering op uw ExpressRoute-circuit tot stand te brengen.

## <a name="route-filters-for-microsoft-peering"></a>Routefilters voor Microsoft-peering

### <a name="i-am-turning-on-microsoft-peering-for-the-first-time-what-routes-will-i-see"></a>Ik ben bij Microsoft-peering voor het eerst inschakelen welke routes ik zien?

U ziet alle routes. U hebt een routefilter koppelen aan uw circuit voorvoegsel advertenties starten. Zie voor instructies [routefilters configureren voor Microsoft-peering](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-to-select-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-to-do-it"></a>Ik heb ingeschakeld Microsoft-peering en ik wil nu selecteren Exchange Online, maar het geeft mij een fout die ik ben niet gemachtigd om dat te doen.

Wanneer u routefilters gebruikt, kunt elke klant inschakelen Microsoft-peering. Echter, voor Office 365-services worden verbruikt, u nog steeds ophalen geautoriseerd door Office 365.

### <a name="do-i-need-to-get-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Moet ik autorisatie Dynamics 365 kunt inschakelen via het Microsoft-peering ophalen?

Nee, u hoeft niet autorisatie voor Dynamics 365. U kunt een regel maken en selecteer Dynamics 365 community zonder toestemming.

### <a name="i-enabled-microsoft-peering-prior-to-august-1st-2017-how-can-i-take-advantage-of-route-filters"></a>Ik Microsoft-peering vóór 1e augustus 2017 hoe kan ik profiteren van routefilters ingeschakeld?

Uw bestaande circuit blijft de voorvoegsels adverteren voor Office 365 en Dynamics 365. Als u wilt dat Azure openbare voorvoegsels advertenties toevoegen via de hetzelfde Microsoft-peering, kunt u een routefilter maken, selecteer de services u moet aangekondigd (inclusief de Office 365 (s), u moet en Dynamics 365) en het filter koppelen aan uw Microsoft peering. Zie voor instructies [routefilters configureren voor Microsoft-peering](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-to-enable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Ik heb Microsoft-peering op één locatie, nu ik probeer in te schakelen op een andere locatie en ik ben geen voorvoegsels niet zien.

* Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd, voordat u 1 augustus 2017 hebben alle service voorvoegsels die worden geadverteerd via Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd.

* Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 geen geen voorvoegsels aangekondigd totdat een routefilter is gekoppeld aan het circuit. Standaard worden er geen voorvoegsels.
