---
title: Veiligheidsconcepten & vereisten in Azure-netwerk | Microsoft Docs
description: " In dit artikel gemakkelijk te begrijpen wat Microsoft Azure te bieden op het gebied van beveiliging van het netwerk heeft. We bieden basic uitleg voor core network security concepten en vereisten en informatie over wat Azure te bieden voor elk van deze gebieden heeft. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: cefc15e7df0dabd9229196d0175dcf6546a6ebce
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-network-security-overview"></a>Overzicht van Azure-netwerk-beveiliging
Microsoft Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen bronnen in Azure, tussen on-premises en Azure gehoste bronnen, en naar en van het Internet en Azure.

Het doel van dit artikel is eenvoudiger voor u te begrijpen wat Microsoft Azure te bieden op het gebied van beveiliging van het netwerk heeft. We bieden hier basic uitleg voor core network security concepten en vereisten. We bieden u ook informatie over wat Azure te bieden in elk van deze gebieden, evenals koppelingen naar informatie waarmee u een beter begrip van interessante gebieden toegang heeft.

In dit artikel beveiligingsoverzicht van Azure-netwerk is gericht op de volgende gebieden:

* Azure-netwerken
* Netwerktoegangsbeheer
* Veilige externe toegang en cross-premises-connectiviteit
* Beschikbaarheid
* Naamomzetting
* DMZ-architectuur
* Controle en detectie van bedreigingen


## <a name="azure-networking"></a>Azure-netwerken
Verbinding met het netwerk, moeten virtuele machines. Azure vereist ter ondersteuning van dit vereiste, virtuele machines worden verbonden met een virtueel netwerk van Azure. Een Azure-netwerk is een logische constructie die is gebouwd op de fysieke netwerk van Azure-infrastructuur. Elke logische Azure Virtual Network is geïsoleerd van alle andere virtuele netwerken van Azure. Dit helpt ervoor zorgen dat netwerkverkeer in uw implementaties niet toegankelijk voor andere Microsoft Azure-klanten is.

Meer informatie:

* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Network Access Control
Netwerktoegangsbeheer wordt de verbinding van en naar specifieke apparaten of subnetten binnen een Azure-netwerk te beperken. Het doel van netwerktoegangsbeheer is om toegang tot uw virtuele machines en services voor goedgekeurde gebruikers en apparaten te beperken. Toegangsbeheer zijn gebaseerd op toestaan of weigeren beslissingen voor verbindingen naar en van de virtuele machine of service.

Azure ondersteunt meerdere typen van network access control zoals:

* Regeling van de laag
* Besturingselement routeren en geforceerde tunneling
* Virtueel netwerk beveiligingsapparaten

### <a name="network-layer-control"></a>Network Layer Control
Alle beveiligde implementatie vereist sommige meting van network access control. Het doel van netwerktoegangsbeheer is om communicatie van de virtuele machine met de benodigde systemen en dat andere communicatiepogingen worden geblokkeerd.

Als u Basisnetwerk toegangsbeheer op gebruikersniveau (op basis van IP-adres en de TCP- of UDP-protocollen) nodig hebt, kunt u Netwerkbeveiligingsgroepen gebruiken. Een Netwerkbeveiligingsgroep (NSG) is een eenvoudige filterregels voor pakketten firewall en Hiermee kunt u toegangsbeheer op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg's bieden geen application layer inspectie of geverifieerde toegangsbeheer.

Meer informatie:

* [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Route besturingselement en geforceerde Tunneling
De mogelijkheid om te bepalen de werking van de routering op uw virtuele netwerken van Azure is een kritieke beveiliging en toegang besturingselement mogelijkheid via het netwerk. Als routering onjuist is geconfigureerd, toepassingen en services die worden gehost op de virtuele machine kunnen verbinding maken met niet-geautoriseerde apparaten, zoals systemen die eigendom zijn van en beheerd door potentiële aanvallers.

Azure-netwerken ondersteunt de mogelijkheid om aan te passen van het routeringsgedrag voor netwerkverkeer op uw virtuele netwerken van Azure. Hiermee kunt u voor het wijzigen van de standaardvermeldingen in de routeringstabel in uw Azure Virtual Network. Besturingselement van het routeringsgedrag kunt u ervoor zorgen dat al het verkeer van een bepaald apparaat of een groep apparaten binnengaat of het virtuele netwerk via een specifieke locatie verlaat.

U wellicht bijvoorbeeld beveiliging van een virtueel netwerkapparaat op uw Azure Virtual Network. U om ervoor te zorgen dat alle verkeer van en naar uw Azure Virtual Network dat toestel virtuele beveiliging doorloopt. U kunt dit doen door het configureren van [gebruiker gedefinieerde Routes](../virtual-network/virtual-networks-udr-overview.md) in Azure.

[Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme die u gebruiken kunt om ervoor te zorgen dat uw services niet zijn toegestaan voor het initiëren van een verbinding met apparaten op Internet. Houd er rekening mee dat dit verschilt van binnenkomende verbindingen accepteert en vervolgens aan hen reageert. Front-endwebservers moeten reageren op aanvragen van Internet-hosts en dus Internet afkomstig verkeer wordt toegestaan inkomend verkeer met deze webservers en de webservers kunnen reageren.

Wat u niet wilt toestaan, is een front-endwebserver een uitgaande aanvraag initiëren. Dergelijke aanvragen mogelijk een beveiligingsrisico vertegenwoordigen, omdat deze verbindingen kunnen worden gebruikt voor kwaadaardige software downloaden. Zelfs als u deze-front-endservers uitgaande aanvragen met het Internet te initiëren wenst, is het raadzaam om af te dwingen om te gaan via de webproxy van uw lokale zodat u kunt profiteren van de URL voor het filteren en logboekregistratie.

In plaats daarvan zou u geforceerde tunneling gebruiken om dit te voorkomen. Wanneer u geforceerde tunneling inschakelt, worden alle verbindingen met het Internet via uw on-premises gateway geforceerd. U kunt configureren om geforceerde tunneling door gebruik te maken van de gebruiker gedefinieerde Routes.

Meer informatie:

* [Wat zijn de gebruiker gedefinieerde Routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtueel netwerk beveiligingsapparaten
Terwijl Netwerkbeveiligingsgroepen, de gebruiker gedefinieerde Routes en geforceerde tunneling u een niveau van beveiliging op de netwerk- en lagen van bieden de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), het kan gebeuren wanneer u beveiliging wilt inschakelen op niveaus die hoger is dan de netwerk.

Bijvoorbeeld: de beveiligingsvereisten van uw kunnen opnemen:

* Verificatie en autorisatie voordat ze toegang krijgen tot uw toepassing
* Inbraakdetectie en inbraakdetectie antwoord
* Application layer inspectie van protocollen op hoog niveau
* URL-filtering
* Netwerk niveau antivirus- en antimalwaresoftware
* Bescherming tegen bot
* Toegangsbeheer voor toepassing
* Aanvullende DDoS-bescherming (boven de DDoS-bescherming de Azure-infrastructuur zelf geleverd)

U kunt deze beveiligingsfuncties verbeterde netwerk openen met behulp van een Azure-partner-oplossing. U vindt de meest recente Azure partnernetwerk beveiligingsoplossingen in via de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoeken naar 'beveiliging' en "netwerkbeveiliging."

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Veilige externe toegang en Cross-Premises-connectiviteit
Installatie, configuratie en beheer van de behoeften van uw Azure-resources op afstand worden uitgevoerd. Bovendien kunt u implementeren [hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) oplossingen die onderdelen on-premises en in de openbare Azure-cloud. Deze scenario's moet veilige externe toegang.

Azure-netwerken ondersteunt de volgende scenario's voor veilige externe toegang:

* Afzonderlijke werkstations verbinden met een Azure Virtual Network
* Verbinding maken met uw on-premises netwerk om een virtueel Azure-netwerk met een VPN
* Verbinding maken met uw on-premises netwerk om een virtueel Azure-netwerk met een specifieke WAN-verbinding
* Virtuele netwerken in Azure met elkaar verbinden

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Afzonderlijke werkstations verbinden met een Azure-netwerk
Het kan gebeuren als u inschakelen, afzonderlijke ontwikkelaars of operations personeel wilt voor het beheren van virtuele machines en services in Azure. Bijvoorbeeld, moet u toegang tot een virtuele machine op een Azure-netwerk en het beveiligingsbeleid kan geen RDP of SSH externe toegang tot afzonderlijke virtuele machines. In dit geval kunt u een punt-naar-site VPN-verbinding.

De punt-naar-site VPN-verbinding gebruikt de [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protocol waarmee u kunt een persoonlijke en beveiligde verbinding tussen de gebruiker en het virtuele Azure-netwerk instellen. Wanneer de VPN-verbinding is gemaakt, zich de gebruiker RDP of SSH via de VPN-verbinding in een virtuele machine op het virtuele netwerk van Azure (op ervan uitgaande dat de gebruiker kan worden geverifieerd en gemachtigd).

Meer informatie:

* [Een punt-naar-Site-verbinding met een virtueel netwerk met behulp van PowerShell configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Uw On-Premises netwerk verbinden met een Azure-netwerk met een VPN
U kunt verbinding maken met uw hele bedrijfsnetwerk of gedeelten daarvan, met een virtueel netwerk van Azure. Dit is gebruikelijk in hybride IT scenario's waarin bedrijven [datacenter op locatie uitbreiden in Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In veel gevallen host bedrijven delen van een service in Azure en delen op locatie, zoals wanneer een oplossing voor de front-endwebservers in Azure en back-end databases on-premises bevat. Deze typen 'cross-premises' verbindingen maken ook beheer van Azure bevinden resources beter beveiligen en scenario's mogelijk, zoals Active Directory-domeincontrollers uit te breiden naar Azure.

Een manier voor het uitvoeren van dit is het gebruik van een [site-naar-site VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Het verschil tussen een site-naar-site-VPN en een punt-naar-site VPN is een punt-naar-site VPN één apparaat verbindt met een virtueel Azure-netwerk, een site-naar-site VPN wordt een volledige netwerk (zoals uw on-premises netwerk) verbonden met een virtueel netwerk van Azure. Site-naar-site VPN-verbindingen met een virtueel netwerk van Azure maximaal beveiligde IPSec-tunnelmodus VPN-protocol gebruiken.

Meer informatie:

* [Een Resource Manager VNet maken met een site-naar-site VPN-verbinding met de Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Planning en ontwerp voor VPN Gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Uw On-premises netwerk verbinden met een Azure-netwerk met een specifieke WAN-verbinding
Punt-naar-site en site-naar-site VPN-verbindingen zijn nuttig voor het inschakelen van cross-premises connectiviteit. Sommige organisaties overwegen echter voor de volgende nadelen:

* VPN-verbindingen gegevens verplaatst via het Internet – dit beschrijft deze verbindingen met mogelijke beveiligingsproblemen die betrokken zijn bij het verplaatsen van gegevens via een openbaar netwerk. Bovendien worden niet betrouwbaarheid en beschikbaarheid voor Internet-verbindingen gegarandeerd.
* VPN-verbindingen naar virtuele netwerken van Azure kunnen worden overwogen bandbreedte beperkt voor sommige toepassingen en -doeleinden, als ze max uit met ongeveer 200 Mbps.

Organisaties die doorgaans moeten van het hoogste niveau van beveiliging en beschikbaarheid voor hun cross-premises verbindingen gebruiken specifieke WAN-verbindingen verbinding maken met externe sites. Azure biedt u een speciale WAN-koppeling die u gebruiken kunt om uw on-premises netwerk verbinding met een virtueel netwerk van Azure te gebruiken. Dit wordt ingeschakeld via Azure ExpressRoute.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Virtuele netwerken in Azure met elkaar verbinden
Het is mogelijk dat u meerdere virtuele netwerken van Azure gebruiken voor uw implementaties. Er zijn diverse redenen waarom u dit kunt doen. Een van de redenen mogelijk voor het vereenvoudigen van beheer; het is mogelijk dat een andere uit veiligheidsoverwegingen. Ongeacht de doel- of logica voor het plaatsen van bronnen op verschillende virtuele netwerken van Azure, het kan gebeuren als u wilt dat resources op elk van de netwerken te verbinden met elkaar.

Een optie voor services op één virtueel Azure-netwerk verbinding maken met services op een andere Azure Virtual Network door 'terug lussen' niet via het Internet. De verbinding wilt starten op één Azure Virtual Network, via het Internet en keert u terug naar de bestemming Azure Virtual Network. Deze optie wordt de verbinding met de beveiligingsproblemen die deel uitmaken van een Internet-communicatie.

Een betere optie mogelijk te maken van een Azure virtuele netwerk naar Azure Virtual Network site-naar-site VPN. Deze Azure virtuele netwerk naar Azure Virtual Network site-naar-site VPN maakt gebruik van dezelfde [IPsec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) protocol als de cross-premises site-naar-site VPN-verbinding hierboven vermeld.

Het voordeel van het gebruik van een Azure virtuele netwerk naar Azure Virtual Network site-naar-site VPN is dat de VPN-verbinding tot stand is gebracht via het netwerk van Azure-infrastructuur in plaats van via Internet verbinding maken. Dit biedt u een extra beveiligingslaag vergeleken met de site-naar-site-VPN's die verbinding via Internet maken.

Meer informatie:

* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Beschikbaarheid
Beschikbaarheid is een belangrijk onderdeel van elk beveiligingsprogramma. Als uw gebruikers en systemen geen toegang wat ze moeten toegang via het netwerk tot, de service kan worden overwogen aangetast. Azure heeft netwerktechnologieën die ondersteuning bieden voor de volgende mechanismen voor hoge beschikbaarheid:

* HTTP-gebaseerde taakverdeling
* Niveau voor netwerktaakverdeling
* Globale taakverdeling

Taakverdeling is een mechanisme ontworpen gelijkmatig distribueren verbindingen tussen meerdere apparaten. De doelstellingen van taakverdeling zijn:

* Beschikbaarheid – wanneer u saldo verbindingen op meerdere apparaten laden, een of meer van de apparaten niet beschikbaar zijn en de services die worden uitgevoerd op de resterende online apparaten kunnen blijven voldoen aan de inhoud van de service te verbeteren
* Betere prestaties – wanneer u saldo verbindingen op meerdere apparaten laadt, één apparaat hoeft niet te nemen van de processor treffer. In plaats daarvan wordt de capaciteit en geheugenbronnen eisen voor de inhoud verdeeld over meerdere apparaten.

### <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
Organisaties die vaak webservices uitvoeren willen een HTTP-gebaseerde taakverdeling voor deze webservices om te zorgen dat voldoende niveaus qua prestaties en hoge beschikbaarheid. In tegenstelling tot traditionele netwerk gebaseerde load balancers, zijn het taakverdeling beslissingen van HTTP-gebaseerde load balancers gebaseerd op kenmerken van het HTTP-protocol niet op het netwerk en transport layer-protocol.

Om u te bieden op basis van HTTP taakverdeling voor uw web gebaseerde services, biedt Azure u de Azure-toepassingsgateway. De Azure-toepassingsgateway ondersteunt:

* HTTP-gebaseerde taakverdeling – load balancing beslissingen zijn gemaakt op basis van kenmerk speciale het HTTP-protocol
* Sessie cookies gebaseerde affiniteit – deze mogelijkheid ervoor zorgt dat de verbindingen met een van de servers achter die load balancer tussen de client en server intact blijft. Hierdoor weet u zeker stabiliteit van transacties.
* SSL-offload – wanneer een client wordt verbinding met de load balancer sessie tussen de client en de load balancer is versleuteld met behulp van het HTTPS (SSL /) protocol. Om betere prestaties, hebt u echter de optie om de verbinding tussen de load balancer en de webserver achter de load balancer gebruiken (niet-versleuteld) HTTP-protocol. Dit wordt wel 'SSL-offload' omdat de webservers achter de load balancer niet bij de versleuteling de processoroverhead van de ondervinden en daarom kunnen serviceaanvragen sneller moet.
* URL gebaseerde inhoud routering – deze functie maakt het mogelijk voor de load balancer om beslissingen te nemen over waar u het doorsturen van verbindingen op basis van de doel-URL. Dit biedt veel meer flexibiliteit dan oplossingen die u de werklast verdelen beslissingen op basis van IP-adressen.

Meer informatie:

* [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Niveau voor netwerktaakverdeling
In tegenstelling tot HTTP-gebaseerde taakverdeling beslissingen niveau Netwerktaakverdeling load balancing op basis van IP-adres en poort (TCP of UDP) getallen.
U kunt de voordelen van het niveau voor netwerktaakverdeling in Azure met behulp van de Azure Load Balancer toegang. Er zijn een aantal belangrijke kenmerken van de Azure Load Balancer:

* Netwerktaakverdeling voor niveau op basis van IP-adres en poort-nummers
* Ondersteuning voor elk application layer protocol
* Load balans wordt gezocht naar Azure virtuele machines en cloud services-rolinstanties
* Kan worden gebruikt voor zowel de internetgerichte (externe load balancing) en de Internet-gerichte (interne load balancing)-toepassingen en virtuele machines
* Eindpunt bewaking, die wordt gebruikt om te bepalen of een van de services die achter de load balancer zijn niet beschikbaar

Meer informatie:

* [Internetgerichte load balancers tussen meerdere virtuele machines of services](../load-balancer/load-balancer-internet-overview.md)
* [Interne Load Balancer-overzicht](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globale taakverdeling
Sommige organisaties willen het hoogste niveau van beschikbaarheid mogelijk. Een manier om dit doel te bereiken is host voor toepassingen in globaal gedistribueerde datacenters. Wanneer een toepassing wordt gehost in datacenters zich in de hele wereld, is het mogelijk voor een hele geopolitieke regio niet beschikbaar en hebt nog steeds de toepassing van en worden uitgevoerd.

Naast de beschikbaarheid voordelen die u verkrijgen door toepassingen in globaal gedistribueerde datacenters hosten, kunt u ook prestatievoordelen ophalen. Deze prestatievoordelen kunnen worden verkregen met behulp van een mechanisme waarmee aanvragen voor de service naar het datacenter die het dichtst bij het apparaat dat de aanvraag wordt ingediend.

Globale taakverdeling kunt bieden u beide van deze voordelen. In Azure krijgt u de voordelen van globale taakverdeling met behulp van Azure Traffic Manager.

Meer informatie:

* [Wat is Traffic Manager?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Naamomzetting
Naamomzetting is een essentiële functie voor alle services die u in Azure host. Vanuit beveiligingsoogpunt, kan inbreuk op een van de functie name resolution leiden tot een aanvaller omleiden van aanvragen van uw sites van een aanvaller. Beveiligde naamomzetting is een vereiste voor alle in de cloud gehoste services.

Er zijn twee soorten naamomzetting die uw vergt aandacht:

* Interne naamomzetting – interne naamomzetting wordt gebruikt door de services op uw virtuele Azure-netwerken, uw on-premises netwerken of beide. Gebruikt voor interne naamomzetting namen zijn niet toegankelijk via het Internet. Voor een optimale beveiliging is het belangrijk uw interne naam resolutie-schema is niet toegankelijk voor externe gebruikers.
* Externe naamomzetting – externe naamomzetting wordt gebruikt door mensen en apparaten buiten uw on-premises en virtuele netwerken van Azure. Dit zijn de namen die zichtbaar zijn voor het Internet en worden gebruikt voor rechtstreekse verbinding met uw cloud-gebaseerde services.

Voor interne naamomzetting hebt u twee opties:

* Een Azure Virtual Network DNS-server – wanneer u een nieuw virtueel netwerk in Azure maakt een DNS-server is voor u gemaakt. Deze DNS-server kan de namen van de machines die zich bevinden op dit virtuele netwerk van Azure omzetten. Deze DNS-server kan niet worden geconfigureerd en wordt beheerd door beheer van Azure-infrastructuur, zodat een veilige naam resolutie-oplossing.
* Breng uw eigen DNS-server: u hebt de optie bij het plaatsen van een DNS-server van uw eigen kiezen op uw Azure Virtual Network. Deze DNS-server wordt mogelijk dat een Active Directory geïntegreerde DNS-server of een specifieke DNS-server-oplossing geleverd door een Azure partner, kunt u vanuit Azure Marketplace.

Meer informatie:

* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
* [DNS-Servers die worden gebruikt door een virtueel netwerk (VNet) beheren](../virtual-network/virtual-network-manage-network.md#dns-servers)

Voor externe DNS-omzetting hebt u twee opties:

* Host uw eigen externe DNS-server on-premises
* Uw eigen externe DNS-server host met een serviceprovider

Veel grote organisaties hun eigen DNS-servers on-premises fungeert als host. Ze kunnen dit doen omdat ze de VPN-expertise en wereldwijde aanwezigheid om dit te doen.

In de meeste gevallen is het beter om uw DNS-naamomzettingsservices host met een serviceprovider. Deze serviceproviders hebben de netwerk-expertise en wereldwijde aanwezigheid zeer hoge beschikbaarheid voor uw services voor naamomzetting. Beschikbaarheid is essentieel voor DNS-services omdat als uw naamomzettingsservices mislukken, niemand kunt bereiken van de services van uw internetverbinding.

Azure biedt u een maximaal beschikbare en zodat externe DNS-oplossing in de vorm van Azure DNS. Deze oplossing externe naam oplossing maakt gebruik van de wereldwijd Azure DNS-infrastructuur. Hiermee kunt u voor het hosten van uw domein in Azure met de dezelfde referenties, API's, hulpprogramma's en facturering als uw andere Azure-services. Als onderdeel van Azure, ook neemt de sterke beveiligingsmechanismen die is ingebouwd in het platform.

Meer informatie:

* [Azure DNS-overzicht](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-architectuur
Veel enterprise organisaties gebruiken DMZ's bij het segmenteren van hun netwerken voor het maken van een buffer-zone tussen het Internet en hun services. De DMZ gedeelte van het netwerk wordt beschouwd als een laag beveiligingszone en geen activa hoge waarde in die netwerksegment worden geplaatst. Doorgaans ziet u beveiliging netwerkapparaten die een netwerkinterface op het DMZ segment en een andere netwerkinterface is verbonden met een netwerk met virtuele machines en services die accepteren van binnenkomende verbindingen via Internet hebben.

Er zijn een aantal variaties DMZ ontwerp-en het besluit een DMZ, implementeren en vervolgens wat voor soort DMZ moet worden gebruikt als u wilt gebruiken, is gebaseerd op de beveiligingsvereisten van uw netwerk.

Meer informatie:

* [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>Controle en detectie van bedreigingen

Azure biedt mogelijkheden om u te helpen op dit gebied sleutel vroegtijdig, bewaking en de mogelijkheid te verzamelen en controleren van netwerkverkeer.

### <a name="azure-network-watcher"></a>Azure-netwerk-Watcher
Azure-netwerk-Watcher bevat een groot aantal mogelijkheden die helpen bij het oplossen van problemen, evenals een geheel nieuwe set hulpprogramma's om te helpen voorzien van de identificatie van beveiligingsproblemen.

[Weergave van de beveiligingsgroep ](/network-watcher/network-watcher-security-group-view-overview.md) helpt bij de naleving van controle en beveiliging van virtuele Machines en kan worden gebruikt om uit te voeren programmatische audits vergelijken van de basislijnen beleid gedefinieerd door uw organisatie aan effectieve regels voor elk van uw virtuele machines. Hiermee kunt u afwijking van de configuratie te identificeren.

[Pakketopname](/network-watcher/network-watcher-packet-capture-overview.md) kunt u netwerkverkeer naar en van de virtuele machine vastleggen. Naast helpen doordat u netwerk statistische gegevens verzamelen en met het oplossen van problemen met toepassing mag pakketopname waardevol zijn bij het onderzoek van netwerk beveiligingsrisico's. U kunt ook deze functionaliteit samen met Azure Functions netwerkopnamen starten in reactie op specifieke waarschuwingen van Azure.

Voor meer informatie over Azure-netwerk-Watcher en het testen van sommige functies in uw labs Kijk eens naar de [bewakingsoverzicht Azure-netwerk-watcher](/network-watcher/network-watcher-monitoring-overview.md)

>[!NOTE]
Azure-netwerk-watcher wordt nog steeds openbare preview hoeft deze mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als de services die in het algemeen beschikbaarheid release. Bepaalde functies mogelijk niet wordt ondersteund, kunnen hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze service controleert de [pagina Azure-updates](https://azure.microsoft.com/updates/?product=network-watcher)

### <a name="azure-security-center"></a>Azure Security Center
Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen en vindt dat u meer inzicht in, en controle over, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een groot aantal beveiligingsoplossingen te detecteren.

Azure Security Center helpt u te optimaliseren en netwerkbeveiliging door te controleren:

* Aanbevelingen voor netwerk-beveiliging bieden
* Bewaking van de status van uw netwerkconfiguratie voor beveiliging
* U wordt gewaarschuwd voor op basis van bedreigingen voor netwerken zowel op het niveau van het eindpunt en het netwerk

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)


### <a name="logging"></a>Logboekregistratie
Logboekregistratie op het netwerkniveau van een is een belangrijke functie voor een netwerk voor beveiliging. U kunt zich aanmelden voor Netwerkbeveiligingsgroepen ophalen netwerkniveau logboekinformatie verkregen gegevens in Azure. Met NSG logboekregistratie beschikt u over gegevens van:

* [Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) – deze logboeken worden gebruikt om alle bewerkingen die worden verzonden naar uw Azure-abonnementen weer te geven. Deze logboeken zijn standaard ingeschakeld en kunnen worden gebruikt in de Azure-portal. Ze werden voorheen bekend als 'Controlelogboeken' of 'Operationele Logs'.
* Gebeurtenislogboeken: deze logboeken bevatten informatie over welke NSG-regels zijn toegepast.
* Teller logboeken – met deze logboeken zodat u weet hoe vaak elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

U kunt ook [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), een krachtige gegevensvisualisatie hulpprogramma, weergeven en analyseren van deze logboeken.

Meer informatie:

* [Log Analytics voor Netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md)
