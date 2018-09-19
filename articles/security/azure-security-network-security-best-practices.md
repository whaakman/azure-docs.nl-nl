---
title: Aanbevolen beveiligingsprocedures voor Azure-netwerk | Microsoft Docs
description: Dit artikel bevat een set van aanbevolen procedures voor het gebruik van netwerk-security ingebouwde mogelijkheden van Azure.
services: security
documentationcenter: na
author: TomShinder
manager: mbaldwin
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: TomSh
ms.openlocfilehash: d89972ff0f7e3035fa20f8d9ee2863b68fa52e9f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124062"
---
# <a name="azure-network-security-best-practices"></a>Aanbevolen beveiligingsprocedures voor Azure-netwerk
U kunt verbinding maken [virtuele Azure-machines (VM's)](https://azure.microsoft.com/services/virtual-machines/) en apparaten op andere apparaten in het netwerk door ze te plaatsen op [virtuele netwerken van Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Dat wil zeggen, kunt u virtuele netwerkinterfacekaarten verbinding maken met een virtueel netwerk om een TCP/IP-gebaseerde communicatie tussen apparaten met een netwerk. Virtuele machines die zijn verbonden met een Azure-netwerk kan verbinding maken met apparaten in hetzelfde virtuele netwerk, verschillende virtuele netwerken, het internet of uw eigen on-premises netwerken.

Dit artikel wordt een verzameling van aanbevolen beveiligingsprocedures voor Azure-netwerk. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure-netwerken en de ervaringen van klanten zoals zelf.

Voor elke aanbevolen procedure in dit artikel wordt uitgelegd:

* Wat de beste manier is
* Waarom u deze wilt inschakelen, die best practices
* Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

In dit artikel aanbevolen procedures voor beveiliging op Azure-netwerk is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, zoals ze bestaan op het moment dat dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

De volgende secties worden aanbevolen procedures voor netwerkbeveiliging.

## <a name="logically-segment-subnets"></a>Logisch segment subnetten
Virtuele Azure-netwerken zijn vergelijkbaar met een LAN op uw on-premises netwerk. Het idee achter een Azure-netwerk is één privé IP-adres op basis van ruimte netwerk waarop u uw Azure virtual machines kunt plaatsen te maken. De persoonlijke IP-adresruimten beschikbaar zijn in de klasse A (10.0.0.0/8), klasse B (172.16.0.0/12), en -bereiken van klasse C (192.168.0.0/16).

Aanbevolen procedures voor het segmenteren logisch subnetten zijn onder andere:

**Beste**: de grotere adresruimte segmenteren in subnetten.   
**Details**: Gebruik [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)-op basis van subnetten beginselen voor het maken van de subnetten.

**Beste**: netwerktoegangsbeheer tussen subnetten maken. Routering tussen subnetten wordt automatisch uitgevoerd en u hoeft te routetabellen handmatig te configureren. Standaard moet u er geen netwerktoegangsbeheer zijn tussen de subnetten die u op Azure virtual network maakt.   
**Details**: gebruik een [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) (NSG). Nsg's kunnen eenvoudig, stateful pakket inspectie apparaten die gebruikmaken van de 5-tuple (de bron-IP, bronpoort, doel-IP, doelpoort en het protocol van laag 4)-benadering voor het maken van regels voor netwerkverkeer toestaan/weigeren. U toestaan of weigeren van verkeer van en naar een enkel IP-adres, naar en van meerdere IP-adressen, of naar en van volledige subnetten.

Wanneer u nsg's voor network access control tussen subnetten, kunt u plaatst resources die deel uitmaken van de rol in hun eigen subnet of dezelfde beveiligingszone.

## <a name="control-routing-behavior"></a>Routering gedrag beheren
Wanneer u een virtuele machine op een Azure-netwerk plaatst, wordt de virtuele machine kunt verbinden met andere VM's in hetzelfde virtuele netwerk, zelfs als de andere VM's zich op verschillende subnetten. Dit is mogelijk omdat dit type communicatie mogelijk is een verzameling van systeemroutes standaard ingeschakeld. Deze standaardroutes zodat VM's in hetzelfde virtuele netwerk moet verbinding met elkaar en met internet (voor uitgaande communicatie met internet alleen) initiëren.

Hoewel de standaardsysteemroutes handig voor vele implementatiescenario's zijn, zijn er tijden wanneer u wilt aanpassen van de configuratie van de routering voor uw implementaties. U kunt de volgende hop-adres voor het bereiken van bepaalde bestemmingen configureren.

Het is raadzaam dat u configureert [gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md) wanneer u een apparaat voor de beveiliging voor een virtueel netwerk implementeert. We praten over dit in een volgende sectie met de titel [uw kritieke Azure-serviceresources beveiligen naar alleen uw virtuele netwerken](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Gebruiker gedefinieerde routes zijn niet vereist en de standaardsysteemroutes meestal werken.
>
>

## <a name="enable-forced-tunneling"></a>Geforceerde tunneling inschakelen
Voor meer informatie over geforceerde tunneling, is het handig om te begrijpen welke 'gesplitste tunneling' is. De meest voorkomende voorbeeld van gesplitste tunneling is gezien met virtueel particulier netwerk (VPN)-verbindingen. Stel een VPN-verbinding te maken van een hotelkamer met uw bedrijfsnetwerk. Deze verbinding kunt u toegang tot bedrijfsbronnen. Alle communicatie met uw bedrijfsnetwerk Ga via de VPN-tunnel.

Wat gebeurt er als u wilt verbinding maken met bronnen op het internet? Als gesplitste tunneling is ingeschakeld, gaat u deze verbindingen rechtstreeks met internet en niet via de VPN-tunnel. Sommige beveiligingsexperts Houd rekening met deze optie om te worden van een potentieel risico. Ze kunt het beste uitschakelen van gesplitste tunneling en ervoor te zorgen dat alle verbindingen, dat is bestemd voor internet en dat is bestemd voor bedrijfsbronnen, gaat u via de VPN-tunnel. Het voordeel van het uitschakelen van gesplitste tunneling is dat verbindingen met het internet vervolgens via het bedrijfsnetwerk beveiligingsapparaten worden geforceerd. Als de VPN-client is verbonden met het internet buiten de VPN-tunnel, zou dat de aanvraag niet.

Nu gaan we brengen dit terug aan virtuele machines in een Azure-netwerk. De standaardroutes voor een Azure-netwerk zodat VM's kunnen het verkeer naar internet te initiëren. Dit kan ook een beveiligingsrisico vertegenwoordigen omdat deze uitgaande verbindingen door de kwetsbaarheid voor aanvallen van een virtuele machine verhogen mogelijk en door aanvallers worden gebruikt. Daarom raden wij aan dat u [inschakelen geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) op uw virtuele machines wanneer u cross-premises connectiviteit tussen uw Azure-netwerk en uw on-premises netwerk. We praten over cross-premises connectiviteit verderop in de aanbevolen procedures voor netwerken.

Als u geen een cross-premises-verbinding, moet u om te profiteren van nsg's (eerder besproken) of Azure virtual network-beveiligingsapparaten (dit wordt hierna besproken) om te voorkomen dat uitgaande verbindingen met het internet van uw Azure virtual machines.

## <a name="use-virtual-network-appliances"></a>Virtuele netwerkapparaten gebruiken
Nsg's en de gebruiker gedefinieerde routering kunnen bieden een zekere mate van netwerkbeveiliging op het netwerk en transport lagen van de [OSI-model](https://en.wikipedia.org/wiki/OSI_model). Maar in sommige gevallen moet of wilt u beveiliging op hoog niveau van de stack inschakelen. In dergelijke situaties is het raadzaam dat u een virtueel netwerk-beveiligingsapparaten die is geleverd door Azure-partners implementeert.

Azure-netwerk-beveiligingsapparaten bieden betere beveiliging dan welke besturingselementen op netwerkniveau biedt. Netwerk-beveiligingsmogelijkheden van beveiliging van virtuele netwerkapparaten zijn onder andere:


* Netwerkfunctie
* Indringers detecteren/inbraakdetectiehandtekeningen preventie
* Beveiligingsproblemen
* Toepassingsbeheer
* Detectie van afwijkingen op het netwerk
* Web filteren
* Antivirus
* Botnet beveiliging

Als u beschikbare Azure-netwerk-beveiligingsapparaten zoekt, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoek naar "beveiliging" en "netwerkbeveiliging."

## <a name="deploy-perimeter-networks-for-security-zones"></a>Perimeternetwerken voor beveiligingszones implementeren
Een [perimeternetwerk](https://docs.microsoft.com/azure/best-practices-network-security) (ook wel een DMZ genoemd) is een fysieke of logische netwerksegment dat een extra beveiligingslaag tussen uw assets en het internet biedt. Apparaten met gespecialiseerde netwerk toegang besturingselement aan de rand van een perimeternetwerk kunnen alleen het gewenste verkeer in het virtuele netwerk.

Perimeternetwerken zijn handig omdat u uw netwerk-toegangsbeheer, bewaking, logboekregistratie en rapportage op de apparaten aan de rand van uw Azure-netwerk zich kunt richten. Hier inschakelen u doorgaans gedistribueerde denial-of service (DDoS) te voorkomen, intrusion detection/inbraakdetectiehandtekeningen preventie systemen (IDS/IPS), firewall-regels en beleid, webfiltering, netwerk antimalware en meer. De netwerk-beveiligingsapparaten tussen internet en uw Azure-netwerk en een interface op beide netwerken hebben.

Hoewel dit de opzet van een perimeternetwerk, zijn er veel verschillende ontwerpen, zoals back to back, drie-homed en multihomed.

We raden voor alle implementaties van hoge beveiliging voor het verbeteren van het niveau van netwerkbeveiliging voor uw Azure-resources met behulp van een perimeternetwerk bevinden.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermijd blootgesteld aan Internet met specifieke WAN-verbindingen
Veel organisaties hebben gekozen de hybride IT-route. In hybride IT zijn aantal gegevensassets van het bedrijf in Azure, terwijl anderen on-premises blijven. Sommige onderdelen van een service worden uitgevoerd in Azure in veel gevallen, terwijl andere onderdelen on-premises blijven.

In de hybride IT-scenario is het doorgaans een soort van cross-premises-connectiviteit. Cross-premises connectiviteit kan het bedrijf de on-premises netwerken verbinden met Azure-netwerken. Er zijn twee oplossingen voor cross-premises-connectiviteit beschikbaar:

* **Site-naar-site VPN**: het is een betrouwbare, betrouwbare en tot stand gebrachte-technologie, maar de verbinding vindt plaats via internet. Bandbreedte wordt beperkt tot een maximum van ongeveer 200 Mbps. Site-naar-site VPN is een optie wenselijk in sommige scenario's en verder wordt besproken in de sectie [uitschakelen RDP/SSH-toegang tot virtuele machines](#disable-rdpssh-access-to-virtual-machines).
* **Met Azure ExpressRoute**: raden wij aan dat u [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) voor uw cross-premises-connectiviteit. ExpressRoute is een speciale WAN koppeling tussen uw on-premises locatie of een Exchange-hostingprovider. Omdat dit een telco-verbinding, worden uw gegevens niet via internet worden verzonden en daarom geen toegang heeft tot de mogelijke risico's van communicatie via internet.

## <a name="optimize-uptime-and-performance"></a>Uptime en prestaties optimaliseren
Als een service niet actief is, kan de informatie kan niet worden geopend. Als prestaties zo slecht dat de gegevens niet bruikbaar is is, kunt u rekening houden met de gegevens toegankelijk is. U moet doen wat u kunt om ervoor te zorgen dat uw services optimale beschikbaarheid en prestaties hebt vanuit het beveiligingsoogpunt.

Een populair en effectieve methode voor het verbeteren van de beschikbaarheid en prestaties is taakverdeling. Taakverdeling is een methode voor het distribueren van netwerkverkeer op servers die deel van een service uitmaken. Bijvoorbeeld, als u de front-end webservers als onderdeel van uw service hebt, kunt u taakverdeling voor de distributie van het verkeer over uw meerdere front-end webservers.

Beschikbaarheid van verhoogd deze distributie van verkeer omdat als een van de webservers niet beschikbaar is, de load balancer stopt verkeer verzenden naar die server en wordt omgeleid naar de servers die nog steeds online zijn. Taakverdeling helpt ook prestaties, omdat de processor-, netwerk- en geheugen overhead voor het uitvoeren van aanvragen voor alle servers met load balancing wordt gedistribueerd.

Het is raadzaam dat u gebruikmaken van taakverdeling wanneer mogelijk kunt u, en als geschikt is voor uw services. Hieronder vindt u scenario's op zowel het niveau van de Azure-netwerk en het globale niveau, samen met opties voor elke load balancing.

**Scenario**: U hebt een toepassing die:

- Aanvragen van dezelfde gebruiker/clientsessie tot dezelfde back-end virtuele machine vereist. Voorbeelden hiervan zijn winkelwagen-apps en webmailservers winkelen.
- Accepteert alleen een beveiligde verbinding, zodat niet-versleutelde communicatie met de server niet een acceptabele optie is.
- Vereist dat meerdere HTTP-aanvragen op dezelfde langlopende TCP-verbinding om te worden doorgestuurd of verdeeld naar andere back-endservers.

**Load balancing optie**: Gebruik [Azure Application Gateway](../application-gateway/application-gateway-introduction.md), een HTTP-web-verkeer load balancer. Application Gateway biedt ondersteuning voor end-to-end SSL-versleuteling en [SSL-beëindiging](../application-gateway/application-gateway-introduction.md) bij de gateway. Webservers kunnen vervolgens worden ontlast van versleuteling en ontsleuteling overhead en verkeer naar de back-endservers.

**Scenario**: U wilt laden saldo binnenkomende verbindingen via internet tussen de servers die zich in een Azure-netwerk. Scenario's zijn wanneer u:

- Stateless toepassingen die accepteren van binnenkomende aanvragen van het internet hebben.
- Geen tijdelijke sessies is vereist of SSL-offload. Tijdelijke sessies is een methode met taakverdeling van toepassing, voor het bereiken van server-affiniteit.

**Load balancing optie**: de Azure portal gebruiken om [maken van een externe load balancer](../load-balancer/quickstart-create-basic-load-balancer-portal.md) die inkomende aanvragen verspreidt over meerdere virtuele machines naar een hoger niveau van beschikbaarheid.

**Scenario**: U wilt laden saldo verbindingen van virtuele machines die zich niet op het internet. In de meeste gevallen worden de verbindingen die worden geaccepteerd voor de taakverdeling in gang gezet door apparaten op een Azure-netwerk, zoals SQL Server-exemplaren of interne webservers.   
**Load balancing optie**: de Azure portal gebruiken om [maken van een interne load balancer](../load-balancer/quickstart-create-basic-load-balancer-powershell.md) die inkomende aanvragen verspreidt over meerdere virtuele machines naar een hoger niveau van beschikbaarheid.

**Scenario**: globale taakverdeling nodig omdat u:

- Een cloudoplossing die op grote schaal wordt gedistribueerd over meerdere regio's en het hoogste niveau van bedrijfstijd (beschikbaarheid) mogelijk vereist zijn.
- Moet het hoogste niveau van beschikbaarheid mogelijk om ervoor te zorgen dat uw service beschikbaar is, zelfs als een heel datacenter niet meer beschikbaar is.

**Load balancing optie**: gebruik Azure Traffic Manager. Traffic Manager maakt het mogelijk is om te laden saldo-verbindingen met de services op basis van de locatie van de gebruiker.

Bijvoorbeeld, als de gebruiker een aanvraag bij de service uit de Europese Unie indient, is de verbinding omgeleid naar uw services bevinden zich in een datacenter van de EU. Dit deel van Traffic Manager globale taakverdeling kunt u de prestaties verbeteren omdat sneller dan de verbinding te maken met datacenters die zich ver verbinding te maken met het dichtstbijzijnde datacenter is geladen.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>RDP/SSH-toegang tot virtuele machines uitschakelen
Het is mogelijk te bereiken van virtuele machines van Azure met behulp van [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) en de [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH)-protocol. Deze protocollen inschakelen van de beheer-VM's vanaf externe locaties en zijn standaard in het datacenter computing.

Mogelijke problemen met het gebruik van deze protocollen via internet security is dat een aanvaller kan gebruiken [beveiligingsaanvallen](https://en.wikipedia.org/wiki/Brute-force_attack) technieken voor toegang tot virtuele machines van Azure. Nadat de aanvallers toegang hebt gekregen, kunnen ze uw virtuele machine gebruiken als een startpunt voor inbreuk op andere computers in het virtuele netwerk of zelfs aanvallen netwerkapparaten buiten Azure.

Het is raadzaam om de directe RDP en SSH-toegang aan uw Azure virtual machines uit te schakelen via internet. Als directe RDP en SSH-toegang via internet is uitgeschakeld, hebt u andere opties die u gebruiken kunt voor toegang tot deze VM's voor extern beheer.

**Scenario**: een enkele gebruiker via internet verbinding maken met een Azure-netwerk.   
**Optie**: [punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) is een andere term voor VPN-client/server-verbinding voor externe toegang. Nadat de punt-naar-site-verbinding tot stand is gebracht, de gebruiker kan gebruiken via RDP of SSH verbinding maken met virtuele machines zich op het Azure-netwerk dat de gebruiker heeft verbinding via punt-naar-site VPN gemaakt. Hierbij wordt ervan uitgegaan dat de gebruiker is gemachtigd om te bereiken die virtuele machines.

Punt-naar-site VPN is veiliger dan direct RDP of SSH-verbindingen, omdat de gebruiker heeft om te verifiëren tweemaal voordat u verbinding maakt met een virtuele machine. Eerst moet de gebruiker om te verifiëren (en worden geautoriseerd) de punt-naar-site VPN-verbinding tot stand brengen. Ten tweede moet de gebruiker om te verifiëren (en worden geautoriseerd) de RDP of SSH-sessie tot stand brengen.

**Scenario**: kunnen gebruikers op uw on-premises netwerk verbinding maken met virtuele machines in uw Azure-netwerk.   
**Optie**: A [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) een heel netwerk verbindt met een ander netwerk via internet. Een site-naar-site-VPN kunt u uw on-premises netwerk verbinden met een Azure-netwerk. Gebruikers met uw on-premises netwerk verbinding maken met behulp van RDP of SSH-protocol via de site-naar-site VPN-verbinding. U hebt geen directe RDP of SSH-toegang via het internet is toegestaan.

**Scenario**: een speciale WAN-verbinding gebruiken om functionaliteit die vergelijkbaar is met de site-naar-site VPN-verbinding te bieden.   
**Optie**: Gebruik [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). Het programma biedt functionaliteit die vergelijkbaar is met de site-naar-site VPN-verbinding. Dit zijn de belangrijkste verschillen:

- De toegewezen WAN-verbinding via niet internet.
- Specifieke WAN-verbindingen zijn doorgaans meer stabiel en beter presteren.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Uw kritieke Azure-serviceresources beveiligen naar alleen uw virtuele netwerken
Gebruik virtual network-service-eindpunten om uit te breiden het privé-adresruimte van uw virtuele netwerk en de identiteit van het virtuele netwerk naar Azure-services, via een directe verbinding. Met eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar uw virtuele netwerken. Verkeer van het virtuele netwerk naar de Azure-service blijft altijd in de Microsoft Azure-backbone-netwerk.

Service-eindpunten bieden de volgende voordelen:

- **Verbeterde beveiliging voor uw Azure-serviceresources**: met service-eindpunten kunnen Azure-serviceresources op veilige wijze worden gekoppeld aan uw virtuele netwerk. Serviceresources met een virtueel netwerk beveiligen biedt verbeterde beveiliging door volledig door de openbare internettoegang tot resources, en alleen verkeer vanaf uw virtuele netwerk wordt toegestaan.
- **Optimale routering voor Azure-serviceverkeer vanaf uw virtuele netwerk**: routes in uw virtuele netwerk die internetverkeer naar uw on-premises en/of virtuele apparaten, wel geforceerde tunneling, ook het verkeer van de Azure-service worden gedwongen de route hetzelfde als het internetverkeer. Service-eindpunten bieden een optimale routering voor Azure-verkeer.

  Eindpunten altijd dat serviceverkeer rechtstreeks van uw virtuele netwerk naar de service op het Azure-backbone-netwerk. Zorgt ervoor dat verkeer op het Azure-backbone-netwerk, kunt u blijven controleren en bewaken van uitgaand internetverkeer vanaf uw virtuele netwerken via geforceerde tunneling, zonder gevolgen voor netwerkverkeer voor de service. Meer informatie over [gebruiker gedefinieerde routes en geforceerde tunneling](../virtual-network/virtual-networks-udr-overview.md).

- **Eenvoudig te installeren met minder beheeroverhead**: hoeft u niet meer gereserveerde openbare IP-adressen in uw virtuele netwerken voor het beveiligen van Azure-resources via een IP-firewall. Er zijn geen NAT- of gatewayapparaten vereist voor het instellen van de service-eindpunten. Service-eindpunten worden geconfigureerd met een simpele klik op een subnet. Er is geen extra overhead voor het onderhouden van de eindpunten.

Zie voor meer informatie over service-eindpunten en de Azure-services en regio's die beschikbaar zijn voor service-eindpunten, [service-eindpunten voor virtuele netwerken](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="next-step"></a>Volgende stap
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.
