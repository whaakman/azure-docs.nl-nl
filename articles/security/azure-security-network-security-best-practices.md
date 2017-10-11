---
title: Aanbevolen beveiligingsprocedures voor Azure-netwerk | Microsoft Docs
description: Dit artikel bevat een set met aanbevolen procedures voor het gebruik van netwerk-beveiliging ingebouwd in de mogelijkheden van Azure.
services: security
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: TomSh
ms.openlocfilehash: 659304937eebb1b2fe6faf019dfef63e1e29bcd4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-network-security-best-practices"></a>Aanbevolen beveiligingsprocedures voor Azure-netwerk
Microsoft Azure kunt u virtuele machines en apparaten verbinding maken met andere apparaten in het netwerk door ze te plaatsen op Azure Virtual Networks. Een Azure-netwerk is een virtueel netwerk constructie waarmee u virtuele netwerkinterfacekaarten verbinden met een virtueel netwerk om een TCP/IP-basis communicatie tussen netwerkapparaten. Azure virtuele Machines die zijn verbonden met een virtueel netwerk van Azure kunnen geen verbinding maken met apparaten op de dezelfde Azure Virtual Network, verschillende virtuele netwerken van Azure, op het Internet of zelfs op uw eigen on-premises netwerken.

In dit artikel bespreken we een verzameling aanbevolen beveiligingsprocedures voor Azure-netwerk. Deze aanbevolen procedures zijn afgeleid van onze ervaring met het Azure-netwerk en de ervaringen van klanten, zoals zelf.

Voor elke aanbevolen procedure wordt uitgelegd:

* Wat de beste handelswijze is
* Waarom u wilt dat het beste inschakelen
* Wat zijn het resultaat als u niet de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u kunt informatie over het inschakelen van de aanbevolen procedure

In dit artikel Best Practices voor beveiliging op Azure-netwerk is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, als deze bestaan in de tijd die in dit artikel is geschreven. Adviezen en technologieën op den duur veranderen en dit artikel wordt regelmatig gecontroleerd op basis van deze wijzigingen worden bijgewerkt.

Azure netwerk aanbevolen beveiligingsprocedures besproken in dit artikel zijn onder andere:

* Logisch segment subnetten
* Het gedrag van routering bepalen
* Geforceerde Tunneling inschakelen
* Gebruik virtuele netwerkapparaten
* DMZ's voor beveiliging zonering implementeren
* Vermijd blootgesteld aan Internet met specifieke WAN-verbindingen
* Beschikbaarheid en prestaties te optimaliseren
* Globale taakverdeling gebruiken
* De RDP-toegang tot virtuele Machines in Azure uitschakelen
* Schakel Azure Security Center
* Uw datacenter uitbreiden naar Azure

## <a name="logically-segment-subnets"></a>Logisch segment subnetten
[Virtuele netwerken van Azure](https://azure.microsoft.com/documentation/services/virtual-network/) zijn vergelijkbaar met een LAN op uw on-premises netwerk. Het idee achter een Azure-netwerk is dat u maakt een enkel IP-adres op basis van ruimte privénetwerk waarop u alle plaatsen kunt uw [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/). De persoonlijke IP-adresruimten beschikbaar zijn in de klasse A (10.0.0.0/8), de klasse B (172.16.0.0/12) en de klasse C (192.168.0.0/16) bereiken.

Vergelijkbaar met wat u lokaal uitvoeren, moet u een grotere adresruimte segmenteren in subnetten. U kunt [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) op basis van de principes van de subnetten voor het maken van de subnetten.

Routering tussen subnetten gebeurt automatisch en hoeft u niet handmatig configureren van routeringstabellen. De standaardinstelling is echter dat er geen toegang tot netwerk besturingselementen zijn tussen de subnetten die u op het virtuele netwerk van Azure maakt. Om het netwerk toegang besturingselementen tussen subnetten maakt, moet u een object tussen de subnetten gezet.

Een van de dingen die u kunt deze taak is een [Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) (NSG). Nsg's zijn eenvoudige stateful packet inspection apparaten die gebruikmaken van de 5-tuple (de bron-IP, bronpoort, doel-IP, doelpoort en het protocol van laag 4) benadering voor het toestaan/weigeren maken van regels voor netwerkverkeer. U kunt toestaan of weigeren van verkeer van en naar één IP-adres naar en van meerdere IP-adressen of zelfs naar en van volledige subnetten.

Met nsg's voor network access control tussen subnetten kunt u resources die bij de rol op hun eigen subnetten of dezelfde beveiligingszone horen plaatsen. Bijvoorbeeld, een eenvoudige 3-laagse toepassing met een weblaag, een toepassing logicalaag en een databaselaag zien. U plaatsen virtuele machines die deel uitmaken van elk van deze lagen in hun eigen subnetten. U kunt vervolgens nsg's gebruiken waarmee verkeer tussen de subnetten:

* Web-laag virtuele machines kunnen alleen verbindingen met de toepassing logica machines initiëren en accepteert alleen clientverbindingen van het Internet
* Toepassing logica virtuele machines kunnen alleen verbindingen met databaselaag initiëren en accepteert alleen verbindingen van de weblaag
* Database-laag virtuele machines verbinding met iets buiten hun eigen subnet kan niet starten en accepteert alleen verbindingen van de logische laag van de toepassing

Lees het artikel voor meer informatie over Netwerkbeveiligingsgroepen en hoe u ze kunt gebruiken bij het segmenteren van uw Azure Virtual Networks logisch [wat is er een Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Het gedrag van routering bepalen
Als u een virtuele machine op een Azure Virtual Network plaatst, zult u merken dat de virtuele machine verbinding met een andere virtuele machine op de dezelfde Azure Virtual Network maken kunt, zelfs als de andere virtuele machines op verschillende subnetten. De reden waarom dit mogelijk is, is er een verzameling van systeemroutes die standaard zijn ingeschakeld waardoor dit type communicatie. Deze standaardroutes zodat virtuele machines op de dezelfde Azure Virtual Network verbindingen met elkaar en met het Internet (voor uitgaande communicatie met Internet alleen) te initiëren.

Het systeem standaardroutes zijn handig voor vele implementatiescenario's, maar er zijn dat u wilt aanpassen van de routeringsconfiguratie voor uw implementaties. Deze aanpassingen kunt u het adres van de volgende hop bereiken van bepaalde bestemmingen configureren.

We raden aan dat de gebruiker gedefinieerde Routes configureren wanneer u een virtueel netwerk security-toestel, dat we in latere best practice bespreken implementeert.

> [!NOTE]
> gebruiker gedefinieerde Routes zijn niet vereist en de systeemroutes standaard in de meeste gevallen zal werken.
>
>

U kunt meer informatie over de gebruiker gedefinieerde Routes en hoe ze configureren door te lezen van het artikel [wat de gebruiker gedefinieerde Routes en doorsturen via IP zijn](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Geforceerde Tunneling inschakelen
Voor meer informatie over geforceerde tunneling, is het handig om te begrijpen welke 'gesplitste tunneling'.
De meest voorkomende voorbeeld van gesplitste tunneling is gezien met VPN-verbindingen. Stel een VPN-verbinding te maken van uw ruimte hotel met uw bedrijfsnetwerk. Deze verbinding kunt u verbinding maken met resources in uw bedrijfsnetwerk en alle communicatie naar resources in uw bedrijfsnetwerk via de VPN-tunnel.

Wat gebeurt er wanneer u verbinding wilt maken met resources op Internet? Wanneer gesplitste tunneling is ingeschakeld, gaat u deze verbindingen rechtstreeks met het Internet en niet via de VPN-tunnel. Sommige beveiligingsexperts Houd rekening met deze optie om te worden van een potentieel risico en daarom raden aan dat gesplitste tunneling uitgeschakeld en alle verbindingen, die bestemd zijn voor het Internet en die bestemd zijn voor bedrijfsbronnen, Ga via de VPN-tunnel. Het voordeel van dit te doen is dat verbindingen met het Internet vervolgens worden geforceerd door middel van de zakelijke beveiliging netwerkapparaten die zou niet het geval als de VPN-client is verbonden met Internet buiten de VPN-tunnel.

Nu gaan we terugbrengen dit aan virtuele machines op een Azure-netwerk. De standaardroutes voor een virtuele Azure-netwerk, zodat virtuele machines verkeer naar Internet te initiëren. Dit kan te bestaan uit een beveiligingsrisico als deze uitgaande verbindingen de kwetsbaarheid van een virtuele machine verhoogt kunnen en door aanvallers worden gebruikt.
Daarom is het raadzaam dat u geforceerde tunneling op uw virtuele machines inschakelt wanneer u cross-premises connectiviteit tussen uw virtuele netwerk van Azure en uw on-premises netwerk hebt. We zullen hebben over cross-premises-connectiviteit verderop in dit Azure netwerken best practices-document.

Als u een cross-premises-verbinding niet hebt, controleert u of u profiteren van Netwerkbeveiligingsgroepen (eerder besproken) of Azure virtuele beveiligingsapparaten (besproken naast) om te voorkomen dat uitgaande verbindingen met het Internet uw virtuele Azure-netwerk Machines.

Lees het artikel voor meer informatie over geforceerde tunneling en het inschakelen ervan, [configureren met behulp van PowerShell en Azure Resource Manager geforceerde Tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Gebruik virtuele netwerkapparaten
Terwijl Netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routering een zekere mate van netwerkbeveiliging op de netwerk- en lagen van bieden kunnen de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), er gaan worden als situaties waar je moet of wilt u beveiliging inschakelen op hoog niveau van de stack. In dergelijke situaties wordt aangeraden dat u een virtueel netwerk beveiligingsapparaten geleverd door Azure partners implementeert.

Azure-netwerk beveiligingsapparaten kunnen aanzienlijk uitgebreide niveaus van beveiliging bieden wat is opgegeven door netwerk niveau besturingselementen. Enkele van de netwerk-beveiligingsmogelijkheden geleverd door de beveiliging van virtuele netwerkapparaten:

* Gebruik
* Inbraakdetectie detectie inbraakdetectie voorkomen
* Beveiligingslek management
* Toepassingsbeheer
* Netwerk gebaseerde afwijkingsdetectie
* Web filteren
* Antivirus
* Botnet beveiliging

Als u een hoger niveau van netwerkbeveiliging dan kunt u met netwerkfuncties toegangsniveau vereist, vervolgens wordt aangeraden dat u onderzoeken en beveiligingsapparaten virtuele Azure-netwerk te implementeren.

Voor meer informatie over welke virtuele Azure-netwerk beveiligingsapparaten beschikbaar zijn en over hun mogelijkheden, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoek naar 'beveiliging' en 'Netwerkbeveiliging'.

## <a name="deploy-dmzs-for-security-zoning"></a>DMZ's voor beveiliging zonering implementeren
Een DMZ of 'perimeternetwerk' is een fysieke of logische netwerksegment dat zodanig ontworpen is dat een extra beveiligingslaag tussen uw bedrijfsmiddelen en het Internet. De bedoeling van het Perimeternetwerk is om gespecialiseerde access control netwerkapparaten op de rand van het netwerk DMZ plaatsen, zodat alleen de gewenste verkeer is toegestaan voorbij het netwerkapparaat voor beveiliging en in uw Azure Virtual Network.

DMZ's zijn nuttig omdat u uw netwerk toegangsbeheer, bewaking, logboekregistratie en rapportage over de apparaten die aan de rand van uw Azure Virtual Network zich kunt richten. U zou hier doorgaans inschakelen DDoS preventie, inbraakdetectie detectie/inbraakdetectie preventie systemen (id's / IP-Adressen), firewall-regels en beleid, web filteren, netwerk antimalware en meer. De beveiliging netwerkapparaten tussen het Internet en het virtuele netwerk van Azure en een interface op beide netwerken hebben.

Dit is de opzet van een DMZ genoemd, maar er zijn veel verschillende DMZ ontwerpen, zoals back to back, drie homed, multihomed, en andere.

We raden voor alle implementaties van hoge beveiliging een DMZ om te verbeteren van het niveau van netwerkbeveiliging voor uw Azure-resources te implementeren.

Lees het artikel voor meer informatie over DMZ's en het implementeren ervan in Azure, [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermijd blootgesteld aan Internet met specifieke WAN-verbindingen
Veel organisaties hebben ervoor gekozen de hybride IT-route. In hybride IT zijn enkele van de bedrijfsgegevens in Azure, terwijl andere lokale blijven. In veel gevallen wordt een aantal onderdelen van een service wordt uitgevoerd in Azure terwijl andere onderdelen op lokale blijven.

In het hybride IT-scenario is meestal een soort cross-premises connectiviteit. Dit cross-premises connectiviteit kan het bedrijf verbinding maken met hun on-premises netwerken van Azure Virtual Networks. Er zijn twee oplossingen voor cross-premises-connectiviteit beschikbaar:

* Site-naar-site VPN
* ExpressRoute

[Site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) vertegenwoordigt een virtuele particuliere verbinding tussen uw on-premises netwerk en een Azure-netwerk. Deze verbinding vindt plaats via het Internet en u kunt informatie in een versleutelde verbinding tussen uw netwerk en Azure 'tunnel'. Site-naar-site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. Tunnel-versleuteling wordt uitgevoerd met [IPsec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

Site-naar-site VPN is een technologie voor het vertrouwde, betrouwbare en tot stand gebracht, verkeer binnen de tunnel Internet passeren. Bovendien bandbreedte relatief beperkt tot maximaal over 200 Mbps.

Als u een uitzonderlijke niveau van beveiliging of prestaties voor uw cross-premises verbindingen vereist, wordt u aangeraden dat u Azure ExpressRoute voor uw cross-premises-connectiviteit gebruiken. ExpressRoute is een speciale WAN koppeling tussen uw on-premises locatie of een Exchange-hostingprovider. Omdat dit een telco verbinding, worden uw gegevens niet via Internet worden verzonden en daarom geen toegang heeft tot de mogelijke risico's in de communicatie via Internet.

Lees het artikel voor meer informatie over de werking van Azure ExpressRoute en het implementeren van [technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Beschikbaarheid en prestaties te optimaliseren
Vertrouwelijkheid, integriteit en beschikbaarheid (CIA) bestaat uit de drie van de meest invloedrijke beveiligingsmodel van vandaag. Vertrouwelijkheid is over codering en privacy, integriteit is bedoeld om ervoor te zorgen dat gegevens worden niet gewijzigd door onbevoegde personen en beschikbaarheid is bedoeld om ervoor te zorgen dat de bevoegde personen toegang tot de informatie die ze zijn gemachtigd voor toegang tot zijn. Fout in een van deze gebieden vertegenwoordigt een mogelijke inbreuk in beveiliging.

Beschikbaarheid kan worden beschouwd als over de beschikbaarheid en prestaties. Als een service niet actief is, kan informatie kan niet worden geopend. Als prestaties zo slecht garantie voor de gegevens onbruikbaar, kunnen we de gegevens niet meer toegankelijk beschouwen. Daarom vanuit het beveiligingsoogpunt van moeten we doen wat wij kunnen om ervoor te zorgen dat onze services een optimale beschikbaarheid en prestaties.
Er is een populaire en effectieve methode voor het verbeteren van de beschikbaarheid en prestaties met taakverdeling. Taakverdeling is een methode voor het distribueren van netwerkverkeer op servers die deel van een service uitmaken. Bijvoorbeeld, als u front-endwebservers als onderdeel van uw service hebt, kunt u taakverdeling voor de distributie van het verkeer over uw meerdere front-endwebservers.

Beschikbaarheid van verhoogd deze distributie van verkeer omdat het als een van de webservers niet beschikbaar is, de load balancer wordt stoppen met het verzenden van verkeer op die server en dat verkeer wordt omgeleid naar de servers die nog steeds online zijn. Taakverdeling helpt ook bij prestaties, omdat de processor, netwerk en geheugen overhead voor het voldoen aan aanvragen is verdeeld over alle load balanced servers.

Het is raadzaam dat u gebruikmaken van taakverdeling zoveel mogelijk en geschikt is voor uw services. We je adres geschiktheid in de volgende secties.
Op het niveau van Azure Virtual Network biedt Azure dat u met drie primaire opties voor load balancing:

* HTTP-gebaseerde taakverdeling
* Externe load balancing
* Interne taakverdeling

## <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
HTTP-gebaseerde taakverdeling nemen van beslissingen over welke server voor het verzenden van verbindingen met behulp van de kenmerken van het HTTP-protocol is gebaseerd. Azure heeft een HTTP-load balancer die door de naam van de toepassingsgateway gaat.

We raden aan dat u ons Azure Application Gateway wanneer:

* Toepassingen waarvoor aanvragen van dezelfde gebruiker/clientsessie zijn vereist om dezelfde virtuele back-endmachine te bereiken. Voorbeelden van deze zou worden winkelen winkelwagen apps en web-e-mailservers.
* Toepassingen die u wilt vrijmaken web server-farms van SSL-beëindiging overhead door gebruik te maken van de toepassingsgateway [SSL-offload](https://f5.com/glossary/ssl-offloading) functie.
* Toepassingen, zoals een netwerk voor inhoudslevering, waarvoor meerdere HTTP-aanvragen op de dezelfde langlopende TCP-verbinding om te worden doorgestuurd of load balanced met verschillende back-endservers.

Lees het artikel voor meer informatie over hoe Azure Application Gateway werkt en hoe u het kunt gebruiken in uw implementaties, [Application Gateway Overview](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Externe Load Balancing
Externe load balancing vindt plaats wanneer het binnenkomende verbindingen via Internet worden verdeeld tussen de servers die zich in een virtuele Azure-netwerk. De externe Azure-Load balancer kunt u deze mogelijkheid en het is raadzaam dat u deze gebruiken wanneer u geen nodig voor de tijdelijke sessies hebt of SSL-offload.

In tegenstelling tot HTTP-gebaseerde taakverdeling gebruikt de externe Load Balancer informatie op de netwerk- en lagen van de OSI-model voor netwerken om te bepalen welke server saldo verbinding met het laden.

Het is raadzaam dat u externe Load Balancing wanneer u [staatloze toepassingen](http://whatis.techtarget.com/definition/stateless-app) accepteren van binnenkomende aanvragen via Internet.

Voor meer informatie over hoe de externe Load Balancer van Azure werkt en hoe u kunt implementeren, lees het artikel [aan de slag maken van een internetgerichte Load Balancer in Resource Manager, met behulp van PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interne Load Balancing
Interne load balancing is vergelijkbaar met de externe load balancing en maakt gebruik van hetzelfde mechanisme saldo verbindingen met de servers achter te laden. Het enige verschil is dat de load balancer in dit geval verbindingen accepteert van virtuele machines die zich niet op het Internet. In de meeste gevallen worden de verbindingen die worden geaccepteerd voor taakverdeling geïnitieerd door apparaten op een Azure Virtual Network.

Het is raadzaam dat u voor de interne load balancer voor scenario's die van deze mogelijkheid gebruikmaken, zoals wanneer u wilt laden saldo verbindingen met SQL-Servers of interne webservers.

Lees het artikel voor meer informatie over hoe Azure interne taakverdeling werkt en hoe u deze kunt implementeren, [maken van een interne Load Balancer met behulp van PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Globale taakverdeling gebruiken
Openbare cloud computing maakt het mogelijk voor het implementeren van globaal gedistribueerde toepassingen waarvoor onderdelen zich in datacenters over de hele wereld. Dit is mogelijk in Microsoft Azure als gevolg van de Azure datacenter globale aanwezigheid. In tegenstelling tot de taakverdeling van technologieën eerder genoemde maakt globale taakverdeling het mogelijk maken services beschikbaar, zelfs wanneer volledige datacenters mogelijk niet meer beschikbaar.

U kunt dit type globale taakverdeling in Azure door gebruik te maken van ophalen [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). Zorgt ervoor dat het Traffic Manager is het mogelijk om te laden saldo verbindingen met uw services op basis van de locatie van de gebruiker.

Bijvoorbeeld, als de gebruiker het verzoek met uw service uit de Europese Unie, is de verbinding omgeleid naar uw services zich in een datacenter EU. Dit deel van Traffic Manager globale taakverdeling helpt de prestaties te verbeteren omdat verbinding te maken met het dichtstbijzijnde datacentrum sneller dan verbinding maken met datacenters die ver zijn geladen.

Aan de kant beschikbaarheid zorgt globale taakverdeling ervoor dat uw service beschikbaar is, zelfs als een hele datacenter mag niet meer beschikbaar.

Bijvoorbeeld, als een Azure-datacenter mag niet meer beschikbaar milieu oorzaken of als gevolg van storingen (zoals regionale netwerkfouten), verbindingen met uw service zou worden gerouteerd naar de dichtstbijzijnde online datacenter. Deze globale taakverdeling wordt gerealiseerd door gebruik te maken van DNS-beleidsregels die u in het Traffic Manager kunt maken.

U wordt aangeraden Traffic Manager voor een cloudoplossing die u ontwikkelt die heeft een bereik op grote schaal gedistribueerd over meerdere regio's en moet van het hoogste niveau van beschikbaarheid mogelijk.

Lees het artikel voor meer informatie over Azure Traffic Manager en het implementeren van deze [wat Traffic Manager is](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>De RDP/SSH-toegang tot virtuele Machines in Azure uitschakelen
Het is mogelijk te bereiken Azure virtuele Machines met de [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) en de [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH)-protocollen. Deze protocollen maken het mogelijk om virtuele machines beheren vanaf externe locaties en zijn standaard in het datacenter computing.

Het eventuele beveiligingsprobleem met het gebruik van deze protocollen via Internet is dat aanvallers verschillende gebruiken kunnen [beveiligingsaanvallen](https://en.wikipedia.org/wiki/Brute-force_attack) technieken voor toegang op Azure Virtual Machines. Zodra de aanvallers toegang krijgen, kunnen ze de virtuele machine gebruiken als een startpunt voor inbreuk op andere computers in uw Azure Virtual Network of zelfs aanvallen netwerkapparaten buiten Azure.

Als gevolg hiervan is het raadzaam schakelen directe RDP en SSH-toegang voor uw Azure Virtual Machines vanaf Internet. Nadat u directe RDP en SSH toegang via het Internet is uitgeschakeld, hebt u andere opties die u gebruiken kunt voor toegang tot deze virtuele machines voor extern beheer:

* Punt-naar-site VPN
* Site-naar-site VPN
* ExpressRoute

[Punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) is een andere term voor VPN-client/server-verbinding voor externe toegang. Een punt-naar-site-VPN kunt één gebruiker via Internet verbinding maken met een Azure-netwerk. Nadat de punt-naar-site-verbinding tot stand is gebracht, zich de gebruiker RDP of SSH verbinding maken met alle virtuele machines op het virtuele Azure-netwerk waarmee de gebruiker via een punt-naar-site VPN verbonden gebruiken. Hierbij wordt ervan uitgegaan dat de gebruiker is gemachtigd om te bereiken die virtuele machines.

Punt-naar-site VPN is veiliger dan direct RDP of SSH-verbindingen, omdat de gebruiker heeft om te verifiëren tweemaal voordat u verbinding maakt met een virtuele machine. Eerst moet de gebruiker geverifieerd (en worden geautoriseerd) tot stand brengen van de punt-naar-site VPN-verbinding; ten tweede moet de gebruiker geverifieerd (en worden geautoriseerd) het RDP of SSH-sessie tot stand brengen.

Een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) een gehele netwerk verbindt met een ander netwerk via Internet. Een site-naar-site-VPN kunt u uw on-premises netwerk verbinden met een virtueel netwerk van Azure. Als u een site-naar-site VPN-, gebruikers implementeert op kan uw on-premises netwerk verbinding maken met virtuele machines op uw Azure Virtual Network door middel van het RDP of SSH-protocol via de site-naar-site VPN-verbinding en vereist niet dat u direct RDP of SSH toegang toestaan via het Internet.

U kunt ook een specifieke WAN-verbinding gebruiken om de functionaliteit is vergelijkbaar met de site-naar-site VPN-verbinding te bieden. De belangrijkste verschillen zijn 1. de speciale WAN-koppeling via niet Internet en 2. specifieke WAN-verbindingen zijn meestal meer stabiel en zodat. Azure biedt u een oplossing voor specifieke WAN-verbinding in de vorm van [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Schakel Azure Security Center
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen en biedt dat u grotere zichtbaarheid van, en controle over, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Azure Security Center helpt u te optimaliseren en netwerkbeveiliging door te controleren:

* Aanbevelingen voor netwerk-beveiliging bieden
* Bewaking van de status van uw netwerkconfiguratie voor beveiliging
* U wordt gewaarschuwd voor op basis van bedreigingen voor netwerken zowel op het niveau van het eindpunt en het netwerk

Het is raadzaam dat u Azure Security Center voor al uw Azure-implementaties inschakelen.

Lees het artikel voor meer informatie over Azure Security Center en het in te schakelen voor uw implementaties [Inleiding tot Azure Security Center](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Veilig uitbreiden van uw datacenter in Azure
Veel bedrijven organisaties willen uitbreiden in de cloud in plaats van hun datacentra lokale groeit. Deze uitbreiding vertegenwoordigt een uitbreiding van bestaande IT-infrastructuur naar de openbare cloud. Door gebruik te maken van cross-premises connectiviteitsopties is het mogelijk te behandelen van uw Azure Virtual Networks als gewoon een subnet op de infrastructuur van uw lokale netwerk.

Er is echter veel planning en ontwerp problemen die moeten eerst worden opgelost. Dit is vooral belangrijk in het gebied van netwerkbeveiliging. Een van de beste manieren om te begrijpen hoe u een ontwerp voor deze benadering is een voorbeeld bekijken.

Microsoft heeft gemaakt de [Architectuurdiagram van Datacenter extensie verwijzing](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) en de ondersteunende collateral om te begrijpen hoe deze een verlenging van het datacenter eruit zou. Dit zorgt voor een voorbeeldimplementatie verwijzing die u gebruiken kunt voor het plannen en ontwerpen van een beveiligde enterprise datacenter uitbreiding van de cloud. Het is raadzaam om dit document voor een beter beeld van de belangrijkste onderdelen van een veilige oplossing te controleren.

Bekijk de video voor meer informatie over het veilig uw datacenter uitbreiden naar Azure, [uw Datacenter uitbreiden naar Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
