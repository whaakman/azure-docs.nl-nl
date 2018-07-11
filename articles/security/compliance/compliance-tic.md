---
title: Vertrouwde Internet verbinding richtlijnen voor Azure
description: Vertrouwde Internet verbinding richtlijnen voor Azure en SaaS-services
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: 7b813500eecba3aa1902c28b9b7c56da6c4516b7
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967531"
---
# <a name="trusted-internet-connection-guidance"></a>Richtlijnen voor vertrouwde CA-verbinding

## <a name="background"></a>Achtergrond

Het doel van het initiatief vertrouwde Internet verbindingen (TIC), is om te optimaliseren en te standaardiseren, de beveiliging van afzonderlijke externe netwerkverbindingen die momenteel in gebruik door federale instanties. Het beleid wordt beschreven in de OMB (beheer van Office en Budget) [Memo M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf).

In November 2007 de OMB tot stand gebracht boter wordt perimeterbeveiliging federale netwerk en de functies voor respons op incidenten te verbeteren. TIC is ontworpen voor netwerkanalyse van alle .gov binnenkomend en uitgaand verkeer naar specifieke handtekeningen en gegevens op basis van een patroon identificeren en ontdek gedragsafwijkingen, zoals botnet activiteit. Instanties zijn verplicht om te consolideren hun externe netwerkverbindingen en hebt al het verkeer gerouteerd via indringing detectie en preventie van apparaten (ook wel EINSTEIN) die zijn gehost op een beperkt aantal network-eindpunten (aangeduid als vertrouwde Internet Verbindingen).

Eenvoudig gezegd, is het doel van boter voor overheidsinstellingen weten:
- Wie komt in het netwerk (gemachtigd of niet-geautoriseerde)?
- Wanneer het netwerk wordt geopend en waarom?
- Welke bronnen worden gebruikt?

Vandaag die alle Bureau externe verbindingen moeten worden gerouteerd via een boter OMB goedgekeurd. Federale instanties zijn vereist om deel te nemen aan het boter-programma dat als een boter Access Provider (TICAP) of door het contracteren van services met een van de belangrijkste laag 1-internetproviders aangeduid als beheerd vertrouwde Internet Protocol Service (MTIPS)-providers.  TIC bevat verplichte essentiële mogelijkheden die vandaag zijn uitgevoerd door het Bureau en MTIPS-provider. In de huidige versie van TIC, de EINSTEIN versie 2 inbraakdetectie en EINSTEIN versie 3 versnelde (3A) inbraakdetectiehandtekeningen preventie apparaten worden geïmplementeerd op elke TICAP en MTIPS en het Bureau brengt een Memo van overeenstemming met de afdeling van Binnenlandse beveiliging (DHS) om te implementeren EINSTEIN mogelijkheden voor federale systemen.

Als onderdeel van haar verantwoordelijkheid om de .gov-netwerk te beveiligen, vereist DHS onbewerkte gegevensfeeds van Bureau Netflow gegevens te correleren van incidenten in de onderneming federale en uitvoeren van analyses met behulp van speciale hulpprogramma's. DHS routers bieden de mogelijkheid voor het verzamelen van IP-netwerkverkeer als deze binnengaat of een interface verlaat. De net door stroomgegevens te analyseren, kan een netwerkbeheerder bepalen, zoals de bron en het doel van verkeer, klasse van de service, enzovoort. NET stroomgegevens wordt beschouwd als 'gegevens niet-inhoud' (bijvoorbeeld koptekst, bron-IP, doel-IP, enzovoort) en kunnen DHS over informatie over de inhoud; dat wil zeggen, die werd uitgevoerd op wat en hoe lang.

Het initiatief omvat ook beleidsregels voor veiligheid, richtlijnen en frameworks die wordt ervan uitgegaan on-premises infrastructuur dat. Zoals overheidsinstanties naar de cloud verplaatsen om kosten te besparen, operationele efficiëntie en innovatie te realiseren, zijn de vereisten van de implementatie van TIC in sommige gevallen trager netwerkverkeer en de snelheid en flexibiliteit met welke government gebruikers kunnen te beperken toegang tot hun cloud-gebaseerde gegevens.

In dit artikel bevat informatie over hoe overheidsinstellingen Microsoft Azure Government kunnen gebruiken om het behalen van naleving boter-beleid in zowel IaaS en PaaS-services.

## <a name="summary-of-azure-networking-options"></a>Overzicht van Azure Networking-opties

Er zijn drie belangrijkste opties bij het verbinden met Azure-services:

1. Directe verbinding met Internet: verbinding maken met Azure-services rechtstreeks via een open internetverbinding. Het medium is openbaar en de verbinding. Toepassings- en versleuteling op blokniveau transport zijn leveranciers vertrouwen privacy te waarborgen. Bandbreedte wordt beperkt door van een site-verbinding met Internet en meerdere actieve providers kunnen worden gebruikt om ervoor te zorgen tolerantie
1. Virtueel particulier netwerk: Verbinding maken met uw Azure Virtual Network privé met een VPN-Gateway.
Het medium is openbaar, zoals over de internetverbinding van een site wordt verzonden, maar de verbinding is versleuteld in een tunnel privacy te waarborgen. Bandbreedte is beperkt, afhankelijk van de VPN-apparaten en de gekozen configuratie. Azure Point-to-Site-verbindingen zijn meestal beperkt tot 100 Mbps tijdens Site-naar-Site-verbindingen beperkt tot 1,25 Gbps zijn.
1. Microsoft ExpressRoute: ExpressRoute is een directe verbinding met Microsoft-services. Aangezien connectiviteit via een geïsoleerde Fibre channel is, kan de verbinding zijn openbaar of privé zijn afhankelijk van de configuratie die wordt gebruikt. De bandbreedte is meestal beperkt tot maximaal 10 Gbps.

Er zijn verschillende manieren om te voldoen aan de vereisten voor vertrouwde Internet verbinding bijlage H (overwegingen voor Cloud), in het Ministerie van binnenlandse beveiliging van, 'Vertrouwde Internet-verbindingen (TIC) architectuur referentiedocument, versie 2.0' gevonden. In dit document, wordt wel boter 2.0 DHS boter richtlijnen worden genoemd.

Om in te schakelen van de verbinding van de afdeling of instantie (D/A) naar Azure of Office 365, zonder het routeren van verkeer via de TIC D/A moet de D/A een gecodeerde tunnel en/of een speciale verbinding naar de Cloud Service Provider (CSP) gebruiken. De CSP-services kunnen controleren of de verbinding met de D/A cloud-activa zijn niet beschikbaar met het openbare internet voor directe Bureau personeel toegang.

Office 365 is compatibel met TIC 2.0 bijlage H met behulp van de Express-Route met [Microsoft-Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) ingeschakeld of een internetverbinding waarmee alle verkeer gebruik TLS 1.2 wordt versleuteld.  D/A eindgebruikers helpen bij het netwerk D/een kunt maken verbinding via hun bureau Netwerk- en boter-infrastructuur via internet. Alle externe internettoegang tot O365 is geblokkeerd en wordt doorgestuurd via de instantie. De D/A kan ook verbinding maken met Office 365 via een Express Route-verbinding met Microsoft-peering, dit een type openbare Peering is, ingeschakeld.  

Voor Azure, kunnen opties (VPN) 2 en 3 (ExpressRoute) voldoen aan deze vereisten wanneer deze worden gebruikt in combinatie met services die de toegang tot Internet beperken.

![Microsoft vertrouwde Internet Verbindingsdiagram (TIC)](media/tic-diagram-a.png)

## <a name="how-azure-infrastructure-as-a-service-offerings-can-help-with-tic-compliance"></a>Hoe Azure-infrastructuur als een Service-aanbiedingen kan helpen met boter-naleving

BOTER-beleid met IaaS naleven is relatief eenvoudig omdat Azure-klanten hun eigen virtuele netwerk routeren beheren.

De belangrijkste vereiste om te garanderen van naleving van de referentiearchitectuur boter is om ervoor te zorgen dat uw Virtueelnetwerk (VNet) een privé-extensie van de afdeling of de instantie van netwerk wordt. Om te worden een _persoonlijke_ extensie, het beleid vereist dat geen verkeer laat uw netwerk, behalve via de On-Premises boter-netwerkverbinding. Dit proces staat bekend als 'Geforceerde Tunneling', die bij boter naleving gebruikt, is het proces van al het verkeer van elk systeem in de CSP-omgeving te gaan via een on-premises gateway van een organisatie netwerk naar internet via de boter routering.

Azure IaaS boter-naleving kan worden onderverdeeld in twee belangrijke stappen:

1. Configuratie
1. Controleren

### <a name="azure-iaas-tic-compliance-configuration"></a>Configuratie van Azure IaaS boter-naleving

Als u wilt configureren een boter-compatibele architectuur met Azure, moet u eerst te voorkomen dat directe toegang tot het internet met uw virtuele netwerk en vervolgens internetverkeer via de on-premises netwerk.

#### <a name="prevent-direct-internet-access"></a>Voorkomen dat directe internettoegang

Azure IaaS-netwerken wordt uitgevoerd via virtuele netwerken die bestaat uit subnetten, waaraan de Network Interface controller (NIC's) van virtuele Machines zijn gekoppeld.

Er is het eenvoudigste scenario boter-naleving kunt ondersteunen om te bevestigen dat een virtuele Machine of een verzameling, kan geen verbinding met externe bronnen maken. Het verbreken van de externe netwerken kan met behulp van Netwerkbeveiligingsgroepen (nsg's), die kan worden gebruikt voor het beheren van verkeer naar een of meer NIC's of subnetten in het virtuele netwerk worden gewaarborgd. Een netwerkbeveiligingsgroep bevat toegangsbeheerregels waarmee verkeer wordt toegestaan of geweigerd op basis van verkeersrichting, protocol, bronadres en poort en doeladres en poort. De regels van een Netwerkbeveiligingsgroep kunnen op elk gewenst moment worden gewijzigd en wijzigingen worden toegepast op alle bijbehorende instanties.  Zie voor meer informatie over het maken van een NSG, in dit artikel [het maken van een NSG](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-on-premises-network"></a>Internetverkeer via On-Premises netwerk

Azure maakt automatisch systeemroutes en wijst de routes toe aan elk subnet in een virtueel netwerk. U kunt geen systeemroutes maken en kunt u de systeemroutes verwijderen, maar kunt u bepaalde systeemroutes met aangepaste routes overschrijven. Azure maakt standaardsysteemroutes voor elk subnet en voegt aanvullende optionele standaardroutes op specifieke subnetten of elk subnet, wanneer u specifieke mogelijkheden van Azure. Deze Routering zorgt ervoor dat verkeer dat bestemd is binnen het Virtueelnetwerk blijft in het virtuele netwerk, IANA aangewezen privéadresruimten zoals 10.0.0.0/8 zijn verwijderd (tenzij opgenomen in de adresruimte van het Virtueelnetwerk), en de "laatste redmiddel" routering 0.0.0.0/0 naar Internet-eindpunt van het Virtueelnetwerk.

![TIC geforceerde tunneling](media/tic-diagram-c.png)

Om ervoor te zorgen dat alle verkeer de TIC D/A passeert, moet het Virtueelnetwerk uitgaand verkeer van alle worden gerouteerd via de On-Premises-verbinding.  U kunt aangepaste routes door ofwel het maken van het door de gebruiker gedefinieerde routes, of door het uitwisselen van border gateway protocol (BGP) routes tussen de gateway van uw on-premises netwerk en de gateway van een Azure-netwerk maken. Meer informatie over de gebruiker gedefinieerde routes kan worden gevonden op https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined. Meer informatie over het Border Gateway Protocol kan ook worden gevonden op https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol.

#### <a name="user-defined-routes"></a>Gebruiker gedefinieerde Routes

Als u van een op route gebaseerde virtuele netwerkgateway gebruikmaakt, kan geforceerde tunneling worden uitgevoerd in Azure door een gebruiker gedefinieerde Route (UDR) instelling 0.0.0.0/0 verkeer worden doorgestuurd naar een 'volgende Hop' van uw virtuele netwerkgateway toe te voegen. Azure bepaalt de gebruiker gedefinieerde Routes via het systeem gedefinieerde Routes volgorde, zodat dit tot alle niet-VNet-verkeer wordt verzonden naar uw virtuele netwerkgateway die u vervolgens naar leiden zou On-Premises doorsturen kunt. Nadat gedefinieerd, moet u deze door de gebruiker gedefinieerde Route die is gekoppeld aan alle subnetten in bestaande of nieuw gemaakt in alle virtuele netwerken in uw Azure-omgeving.

![gebruiker gedefinieerde routes en ticket m.](media/tic-diagram-d.png)

#### <a name="border-gateway-protocol"></a>Border Gateway Protocol

Als u ExpressRoute gebruikt of een Border Gateway Protocol (BGP) ingeschakeld virtuele netwerkgateway, is BGP de beste manier om routes te adverteren. Aangekondigd route 0.0.0.0/0 en ExpressRoute- en BGP op de hoogte Gateways van virtueel netwerk zorgt ervoor dat deze standaardroute wordt toegepast op alle subnetten in uw virtuele netwerken met een BGP.

### <a name="azure-iaas-tic-compliance-auditing"></a>Azure IaaS boter naleving controleren

Azure biedt verschillende manieren boter naleving controleren.

#### <a name="effective-routes"></a>Effectieve routes

Om te bevestigen dat uw standaard-route is doorgegeven, ziet u de 'effectieve Routes' van een bepaalde virtuele machine, een specifieke NIC of een door de gebruiker gedefinieerde Route-Table. Dit kan worden gedaan via de Azure-portal zoals beschreven op https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal, of via PowerShell, zoals beschreven op https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell. De effectieve Routes-blade kunt u om te zien van dat de relevante door de gebruiker gedefinieerde Routes, de BGP-routes en systeemroutes die betrekking hebben op de relevante entiteit geadverteerd zoals hieronder wordt weergegeven.

![Schermafbeelding van routes](media/tic-screen-1.png)

**Houd er rekening mee**: U kunt de effectieve routes niet weergeven voor een NIC, tenzij deze die zijn gekoppeld aan een actieve virtuele machine.

#### <a name="network-watcher"></a>Network Watcher

Azure Network Watcher biedt verschillende hulpprogramma's die kunnen worden gebruikt om te controleren boter-naleving.  Meer informatie over Network Watcher op https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview.

##### <a name="network-security-groups-flow-logs"></a>Stroomlogboeken van Network Security groepen 

Azure Network Watcher biedt de mogelijkheid om vast te leggen netwerk stroomlogboeken die wijzen op de IP-verkeer rond metagegevens. Naast andere gegevens bevatten de logboeken van de stroom netwerk de bron- en bestemmingen adressen doelen. Dit, in combinatie met de logboeken van de Gateway van het virtuele netwerk, On-Premises edge-apparaten en/of de boter, kunnen voor het bewaken van al het verkeer wordt inderdaad gerouteerde On-Premises. 

## <a name="how-azure-platform-as-a-service-offerings-can-help-with-tic-compliance"></a>Hoe Azure-Platform als een Service-aanbiedingen kan helpen met boter-naleving

Azure PaaS-services zoals Azure Storage zijn toegankelijk via een via Internet bereikbaar-URL. Iedereen met goedgekeurde referenties hebben toegang tot de resource, zoals een storage-account vanaf elke locatie zonder het doorlopen van een boter. Daarom sluiten veel overheidsklanten ten onrechte dat Azure PaaS-services die niet compatibel zijn met boter-beleid. Veel Azure PaaS-services kunnen in feite zijn compatibel met boter-beleid met behulp van dezelfde architectuur als een boter-compatibele IaaS-omgeving beschreven als ze kunnen worden gekoppeld aan een Virtueelnetwerk (VNet). Azure PaaS-services integreren met een Azure-VNet, kan de service dat VNet privé toegankelijk en kan aangepaste routering voor 0.0.0.0/0 moet worden toegepast via door de gebruiker gedefinieerde Routes of BGP, ervoor zorgen dat alle internetverkeer gerouteerde On-Premises naar de boter passeren.  Sommige Azure-services kunnen worden geïntegreerd in vnet's met behulp van de volgende patronen:

- **Toegewezen exemplaar van de service implementeren**: een toenemend aantal PaaS-services kunnen worden geïmplementeerd als toegewezen instanties met een VNet eindpunten die zijn gekoppeld. Als u bijvoorbeeld kan een App Service Environment (ASE) worden geïmplementeerd in de modus 'Geïsoleerd', zodat de netwerkeindpunt om te worden beperkt tot een VNet. Deze as-omgeving kan veel Azure PaaS-services, zoals Web-Apps, API's en functies kunt hosten.
- **VNet-Service-eindpunten**: een toenemend aantal PaaS-services toestaan hun eindpunt overstappen naar een VNet privé IP-adres in plaats van een openbaar adres.

Services die ondersteuning bieden voor implementatie van toegewezen instanties in een VNet of Service-eindpunten vanaf mei 2018 worden hieronder vermeld: * (beschikbaarheid geeft Azure Commercial, Azure Government beschikbaarheid in behandeling).

### <a name="service-endpoints"></a>Service-eindpunten

|Service                   |Status            |
|--------------------------|------------------|
|Met Azure Key Vault            | Beperkte Preview-versie  |
|Cosmos DB                 | Beperkte Preview-versie  |
|Identiteit                  | Beperkte Preview-versie  |
|Azure Data Lake           | Beperkte Preview-versie  |
|SQL Postgress/Mysql       | Beperkte Preview-versie  |
|Azure SQL Data Warehouse  | Openbare Preview   |
|Azure SQL                 | Algemene beschikbaarheid               |
|Storage                   | Algemene beschikbaarheid               |

### <a name="vnet-injection"></a>VNet-injectie

|Service                            |Status            |
|-----------------------------------|------------------|
|Met SQL beheerd exemplaar               | Openbare Preview   |
|Azure Container Service(AKS)       | Openbare Preview   |
|Service Fabric                     | Algemene beschikbaarheid               |
|API Management                     | Algemene beschikbaarheid               |
|Azure Active Directory             | Algemene beschikbaarheid               |
|Azure Batch                        | Algemene beschikbaarheid               |
|AS-OMGEVING                                | Algemene beschikbaarheid               |
|Redis                              | Algemene beschikbaarheid               |
|HDI                                | Algemene beschikbaarheid               |
|Virtuele-machineschaalset COMPUTE  | Algemene beschikbaarheid               |
|COMPUTE-Service in de Cloud              | Algemene beschikbaarheid               |

### <a name="vnet-integration-details"></a>Details van de VNet-integratie

Het onderstaande diagram begeleid bij de algemene netwerkstroom voor toegang tot PaaS-services met behulp van VNet-injectie en VNet-Servicetunneling.  Meer informatie over Service-netwerkgateways, van het VNet en servicetags vindt u hier https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags.

![PaaS-connectiviteitsopties voor ticket m.](media/tic-diagram-e.png)

1. Particuliere verbinding met Azure met behulp van ExpressRoute. ExpressRoute-privépeering met geforceerde tunnels wordt gebruikt om af te dwingen van al klant-VNet-verkeer via ExpressRoute terug naar on-premises. Microsoft-Peering is niet vereist.
2. Azure VPN-Gateway gebruikt in combinatie met de ExpressRoute-Microsoft-peering kan worden gebruikt om end-to-end IPsec-versleuteling tussen de klant-VNet en de rand van het on-premises overlay. 
3. De netwerkverbinding met het VNet van de klant wordt beheerd met behulp van Netwerkbeveiligingsgroep groepen (NSG) zodat klanten kunnen het toestaan/weigeren op basis van IP, poort en protocol.
4. De klant-VNet is uitgebreid naar de PaaS-service door het maken van een Service-eindpunt voor de service van de klant.
5. Het PaaS-Service-eindpunt is beveiligd naar standaard Alles weigeren en alleen toegang toestaan uit opgegeven subnetten binnen het VNet van de klant.  De standaardwaarde ook weigeren bevat verbindingen die afkomstig zijn van het Internet.
6. Andere Azure-services die nodig hebt voor toegang tot resources binnen het VNet van de klant moet:  
  a. Rechtstreeks in het VNet zijn geïmplementeerd  
  b. Toegestaan selectief op basis van de richtlijnen van de betreffende Azure-service.

#### <a name="option-1-dedicated-instance-vnet-injection"></a>Optie 1: Toegewezen exemplaar "VNet injectie"

Klanten met VNet-injectie, kunnen selectief toegewezen instanties van een bepaald Azure-service, zoals HDInsight, in hun eigen VNet implementeren. Service-exemplaren zijn geïmplementeerd in een toegewezen subnet in het VNet van de klant. VNet-injectie kan de service-resources toegankelijk zijn via het Internet-routeerbare adressen.  On-premises instanties hebben toegang tot deze service-exemplaren via VNet-adresruimte rechtstreeks via ExpressRoute of Site-naar-Site VPN, in plaats van firewalls op het openbare Internet-adresruimte te openen. Met een toegewezen exemplaar die is gekoppeld aan een eindpunt, de dezelfde strategieën gebruikt voor IaaS boter naleving kunnen worden gebruikt, met standaard routering ervoor te zorgen dat die internetverkeer wordt omgeleid naar een virtuele netwerkgateway voor On-Premises gebonden. Inkomende en uitgaande toegang kan worden verder beheerd door Netwerkbeveiligingsgroepen (nsg's) voor het opgegeven subnet.

![Overzichtsdiagram van de VNet-injectie](media/tic-diagram-f.png)

#### <a name="option-2-vnet-service-endpoints"></a>Optie 2: De VNet-Service-eindpunten 

Een toenemend aantal meerdere tenants van Azure-services bieden een mogelijkheid 'Service-eindpunt', een alternatieve methode om te integreren met Azure vnet's. VNet-Service-eindpunten Breid uw VNet-IP-adresruimte en de identiteit van uw VNet naar de service via een directe verbinding.  Verkeer van het VNet naar de Azure-service blijft altijd in de Azure-backbone-netwerk. Zodra een Service-eindpunt is ingeschakeld voor een service, is met de service kunnen worden beperkt tot dat VNet via beleidsregels die worden weergegeven door de service. Toegangscontroles in het platform worden afgedwongen door de Azure-Service en de toegang tot vergrendelde resource wordt alleen verleend als de aanvraag afkomstig van de toegestane VNet/Subnet is, en/of de twee IP-adressen gebruikt voor het identificeren van uw on-verkeer als u ExpressRoute gebruikt. Dit kan worden gebruikt om effectief te voorkomen dat Inkomend/uitgaand verkeer rechtstreeks verlaten van de PaaS-Service.

![Overzichtsdiagram van de service-eindpunten](media/tic-diagram-g.png)

## <a name="using-cloud-native-tools-for-network-situational-awareness"></a>Met behulp van systeemeigen cloud-hulpprogramma's voor netwerk verzamelen awareness

Azure biedt de cloud systeemeigen hulpprogramma's om ervoor te zorgen dat u het verzamelen bewustzijn nodig om te begrijpen van de verkeersstromen van uw netwerk hebt. Ze zijn niet vereist om te voldoen aan beleid voor boter, maar ze uw beveiligingsmogelijkheden aanzienlijk kunnen verbeteren.

### <a name="azure-policy"></a>Azure Policy

Azure Policy (https://azure.microsoft.com/services/azure-policy/) is een Azure-service die uw organisatie biedt betere mogelijkheid om te controleren en afdwingen van nalevingsinitiatieven.  Momenteel beschikbaar in openbare Preview in commerciële Clouds van Azure, maar nog niet in Microsoft Azure voor de overheid, boter-bewust klanten kunnen starten plannen en testen van de beleidsregels voor naleving in de toekomst zekerheid. Azure Policy is gericht op het abonnementsniveau en biedt een centrale interface voor het beheren van initiatieven, beleidsdefinities, controle en het afdwingen van de resultaten en Uitzonderingsbeheer. Naast de vele ingebouwde Azure Policy-definities, kunnen beheerders hun eigen aangepaste definities via eenvoudige json templating definiëren. Voor veel klanten wordt aangeraden de prioriteitsaanduiding van de controle over afdwingen, waar mogelijk.

Het volgende voorbeeld-beleidsregels zijn mogelijk nuttig zijn voor boter naleving scenario's:

|Beleid  |Voorbeeldscenario  |Starten van sjabloon  |
|---------|---------|---------|
|Afdwingen van de gebruiker gedefinieerde routetabel |     Zorg ervoor dat de standaardroute op alle virtuele netwerken punten op een goedgekeurde virtuele netwerkgateway voor de routering naar On-Premises | https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table |
|Als Network Watcher is niet ingeschakeld voor de regio controleren  | Zorg ervoor dat de Network Watcher is ingeschakeld voor alle regio's gebruikt  | https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled |
|NSG X in elk subnet  | Zorg ervoor dat er een NSG (of set van goedgekeurde nsg's) met internetverkeer geblokkeerd wordt toegepast op alle subnetten in elke VNet | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet |
|NSG X op elke NIC | Zorg ervoor dat er een NSG met internetverkeer geblokkeerd wordt toegepast op alle NIC's op alle virtuele machines. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic |
|Gebruik VNet goedgekeurd voor VM-netwerkinterfaces  | Zorg ervoor dat alle NIC's op een goedgekeurde VNet | https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics |
|Toegestane locaties | Zorg ervoor dat alle resources worden geïmplementeerd in de regio's met compatibele vnet's en netwerk-Watcher-configuratie  | https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs |
|Niet toegestane resourcetypen, zoals PublicIPs  | De implementatie van de resourcetypen die nog geen een plan naleving verbieden. Als voorbeeld, kan dit beleid worden gebruikt om te voorkomen dat de implementatie van resources voor openbaar IP-adres. Terwijl de NSG-regels kunnen worden gebruikt voor het effectief geblokkeerd binnenkomend verkeer van Internet, vermindert zo wordt voorkomen dat het gebruik van openbare IP-adressen verder de kwetsbaarheid voor aanvallen.    | https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type  |

### <a name="azure-traffic-analytics"></a>Azure Traffic Analytics

Azure Network Watcher van Traffic Analytics verbruikt stroom logboekgegevens en andere logboeken voor een overzicht op hoog niveau van het netwerkverkeer. Deze gegevens kan nuttig zijn voor boter naleving controleren en het identificeren van knelpunten. Een dashboard op hoog niveau kan worden gebruikt om snel virtuele machines met het internet, die vervolgens een gerichte lijst communiceren voor de routering van boter opgeven wilt scherm.

![Schermafbeelding van Traffic Analytics](media/tic-traffic-analytics-1.png)

Een "geografische kaart' kan worden gebruikt om snel te identificeren de waarschijnlijk fysieke bestemmingen van internetverkeer voor uw VM's, zodat u kunt om te bepalen en verdachte locaties of patroon wijzigingen sorteren.

![Schermafbeelding van Traffic Analytics](media/tic-traffic-analytics-2.png)

Een overzicht van de topologie van netwerk kan worden gebruikt voor het snel krijgt een overzicht van bestaande virtuele netwerken:

![Schermafbeelding van Traffic Analytics](media/tic-traffic-analytics-3.png)

### <a name="azure-network-watcher-next-hop"></a>Volgende Hop van Azure Network Watcher

Netwerken in regio's die worden bewaakt door de Network Watcher kunnen uitvoeren 'Volgende Hop'-tests, eenvoudig op basis van een Portal toegang te typen in een bron en bestemming voor een voorbeeld-netwerkstroom, waarvoor Network Watcher het 'Volgende Hop'-doel wordt opgelost. Dit kan worden gebruikt voor VM's en voorbeeld van de Internet-adressen om ervoor te zorgen dat het 'volgende Hop' inderdaad de virtuele netwerkgateway.

![Volgende hop voor netwerk-watcher](media/tic-network-watcher.png)

## <a name="conclusions"></a>Conclusies

Toegang tot Microsoft Azure, Office 365 en Dynamics 365 kan eenvoudig worden geconfigureerd om te voldoen aan de richtlijnen TIC 2.0 bijlage H geschreven en gedefinieerd in mei 2018.  Microsoft is zich bewust zijn dat deze richtlijnen kan gewijzigd worden en inspannen zal om u te helpen klanten te voldoen aan de richtlijnen tijdig terwijl nieuwe richtlijnen wordt uitgebracht.

## <a name="appendix-tic-patterns-for-common-workloads"></a>Bijlage: TIC patronen voor veelvoorkomende Workloads

| Category | Workload | IaaS | Toegewezen PaaS / VNet-injectie  | Service-eindpunten  |
|---------|---------|---------|---------|--------|
| Compute | Linux Virtual Machines | Ja | | |
| Compute | Windows Virtual Machines | Ja | | |
| Compute | Virtual Machine Scale Sets | Ja | | |
| Compute | Azure Functions | | via App Service-omgeving (ASE) | |
| Internet en mobiel | Interne webtoepassingen | | via App Service-omgeving (ASE) | |
| Internet en mobiel | Interne mobiele toepassing | | via App Service-omgeving (ASE) | |
| Internet en mobiel | API Apps | | via App Service-omgeving (ASE) | |
| Containers | Azure Containerservice (ACS) | | | Ja |
| Containers | Azure Containerservice (AKS) * | | | Ja |
| Database | SQL Database | | Azure SQL Database beheerde exemplaar * | Azure SQL |
| Database | Azure Database for MySQL | | | Ja |
| Database | Azure Database for PostgreSQL | | | Ja |
| Database | SQL Data Warehouse | | | Ja |
| Database | Azure Cosmos DB | | | Ja |
| Database | Redis Cache | | Ja | |
| Storage | Blobs | Ja | | |
| Storage | Bestanden | Ja | | |
| Storage | Wachtrijen | Ja | | |
| Storage | Tabellen | Ja | | |
| Storage | Disks | Ja | | |

*: Openbare Preview in Azure Government vanaf mei 2018  
**: Beperkte Preview-versie in Azure Government vanaf mei 2018

