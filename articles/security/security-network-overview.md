---
title: Beveiligingsconcepten en-vereisten in Azure | Microsoft Docs
description: In dit artikel bevat basic uitleg over de basisconcepten van het netwerk-beveiliging en vereisten en informatie over wat Azure te in elk van deze gebieden bieden.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: terrylan
ms.openlocfilehash: 30a7eea751a9fbfa0ee95a7edb039a13860be7f8
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115467"
---
# <a name="azure-network-security-overview"></a>Overzicht van de beveiliging van Azure-netwerk

Beveiliging van het netwerk kan worden gedefinieerd als het proces voor het beveiligen van bronnen tegen onbevoegde toegang of aanvallen door besturingselementen toe te passen op netwerkverkeer. Het doel is om ervoor te zorgen dat alleen legitieme verkeer is toegestaan. Azure bevat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen de resources die zich bevinden in Azure, tussen on-premises en wordt gehost op Azure-resources, en naar en van internet en Azure.

In dit artikel vindt u enkele van de opties die Azure in het gebied van beveiliging van het netwerk biedt. U kunt meer informatie over:

* Azure-netwerken
* Netwerktoegangsbeheer
* Azure Firewall
* Veilige externe toegang en cross-premises connectiviteit
* Beschikbaarheid
* Naamomzetting
* Netwerkarchitectuur van perimeternetwerk (DMZ)
* Azure DDoS Protection
* Azure voordeur
* Traffic Manager
* Controle en detectie van bedreigingen

## <a name="azure-networking"></a>Azure-netwerken

Virtuele machines worden verbonden met een Azure-netwerk is vereist in Azure. Een virtueel netwerk is een logische constructie die is gebaseerd op de fysieke netwerk van Azure-infrastructuur. Elk virtueel netwerk is geïsoleerd van andere virtuele netwerken. Dit zorgt ervoor dat het netwerkverkeer in uw implementaties is niet toegankelijk voor andere Azure-klanten.

Meer informatie:

* [Overzicht van Virtual network](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Netwerktoegangsbeheer

Netwerktoegangsbeheer is de handeling van de connectiviteit van en naar specifieke apparaten of subnetten binnen een virtueel netwerk te beperken. Het doel van netwerktoegangsbeheer is om toegang tot uw virtuele machines en services voor goedgekeurde gebruikers en apparaten te beperken. Besturingselementen voor toegang worden op basis van besluiten wilt toestaan of weigeren van verbindingen naar en van uw virtuele machine of service.

Azure ondersteunt verschillende soorten netwerktoegangsbeheer, zoals:

* Besturingselement voor netwerk-laag
* Besturingselement routeren en geforceerde tunneling
* Beveiliging van virtuele netwerkapparaten

### <a name="network-layer-control"></a>Besturingselement voor netwerk-laag

Alle beveiligde implementatie vereist bepaalde meting van network access control. Het doel van netwerktoegangsbeheer is om te beperken de communicatie van de virtuele machine naar de systemen die nodig zijn. Andere communicatiepogingen worden geblokkeerd.

>[!NOTE]
Storage-Firewalls worden behandeld in de [beveiligingsoverzicht voor Azure storage](security-storage-overview.md) artikel

#### <a name="network-security-rules-nsgs"></a>Netwerkbeveiligingsregels (nsg's)

Als u eenvoudige niveau netwerktoegangsbeheer (op basis van IP-adres en de protocollen TCP of UDP), kunt u Netwerkbeveiligingsgroepen (nsg's). Een NSG is een basic, stateful, firewall filteren van pakketten en Hiermee kunt u voor het beheren van toegang op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg's bevatten functionaliteit om te vereenvoudigen en verminderen de kans op configuratiefouten:

* **Uitgebreide beveiligingsregels** definitie van de NSG-regel vereenvoudigen en kunt u complexe regels in plaats van dat u hoeft te maken van meerdere eenvoudige regels voor het bereiken van hetzelfde resultaat te maken.
* **Servicetags** Microsoft gemaakt labels die staan voor een groep met IP-adressen. Ze bijwerken dynamisch om op te nemen van IP-adresbereiken die voldoen aan de voorwaarden opgenomen in het label. Als u wilt maken van een regel die van toepassing op alle Azure-opslag in de regio Oost kunt u bijvoorbeeld Storage.EastUS gebruiken
* **Toepassingsbeveiligingsgroepen** kunt u resources implementeren op groepen en de toegang tot deze bronnen wordt beheerd door regels die gebruikmaken van deze groepen te maken. Als u geïmplementeerd op de toepassingsgroep 'Webservers' webservers hebt kunt u bijvoorbeeld een regel die van toepassing is een NSG 443 verkeer wordt toegestaan vanaf het Internet op alle systemen in de toepassingsgroep 'Webservers' maken.

Nsg's bieden geen toepassing laag inspectie of geverifieerde besturingselementen voor toegang.

Meer informatie:

* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md)

#### <a name="asc-just-in-time-vm-access"></a>ASC just-in-time-VM-toegang

[Azure security center](../security-center/security-center-intro.md) kunnen de Netwerkbeveiligingsgroepen op VM's beheren en toegang tot de virtuele machine te vergrendelen totdat een gebruiker met de juiste op rollen gebaseerd toegangsbeheer [RBAC](../role-based-access-control/overview.md) machtigingen aanvragen toegang. Wanneer de gebruiker nog niet is maakt geautoriseerde ASC wijzigingen in de nsg's voor toegang tot geselecteerde poorten voor de tijd die is opgegeven. Wanneer het tijd is verlopen worden de nsg's worden teruggezet naar de vorige status beveiligd.

Meer informatie:

* [Azure Security Center Just-in-Time-toegang](../security-center/security-center-just-in-time.md)

#### <a name="service-endpoints"></a>Service-eindpunten

Service-eindpunten zijn een andere manier om toe te passen van controle over uw verkeer. U kunt communicatie met ondersteunde services beperken tot alleen uw VNets via een directe verbinding. Verkeer van uw VNet naar de opgegeven Azure-service blijft in de Microsoft Azure-backbone-netwerk.  

Meer informatie:

* [Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md#securing-azure-services-to-virtual-networks)

### <a name="route-control-and-forced-tunneling"></a>Besturingselement routeren en geforceerde tunneling

De mogelijkheid voor het beheren van de werking van routering op uw virtuele netwerken is essentieel. Routering onjuist is geconfigureerd, toepassingen en services die worden gehost op uw virtuele machine kunnen verbinding maken met niet-geautoriseerde apparaten, inclusief systemen die eigendom zijn en worden beheerd door potentiële aanvallers.

Azure-netwerken ondersteunt de mogelijkheid om aan te passen van de routering-gedrag voor netwerkverkeer op uw virtuele netwerken. Hiermee kunt u de routering van vermeldingen in het virtuele netwerk standaardwaarde wijzigen. Controle over de werking van routering kunt u ervoor zorgen dat al het verkeer van een bepaald apparaat of groep apparaten binnengaat of verlaat van het virtuele netwerk via een specifieke locatie.

Bijvoorbeeld, mogelijk hebt u een virtueel apparaat voor netwerkbeveiliging in het virtuele netwerk. U wilt om ervoor te zorgen dat al het verkeer naar en van het virtuele netwerk via dat apparaat voor de virtuele beveiliging gaat. U kunt dit doen door het configureren van [de gebruiker gedefinieerde Routes](../virtual-network/virtual-networks-udr-overview.md) (udr's) in Azure.

[Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme dat u gebruiken kunt om ervoor te zorgen dat uw services zijn niet toegestaan om een verbinding aan apparaten op internet te zetten. Houd er rekening mee dat dit wijkt af van het accepteren van binnenkomende verbindingen en klikt u vervolgens reageren op deze. Front-end-webservers moeten reageren op aanvragen van internet-hosts en dus internet afkomstig verkeer is toegestaan inkomend verkeer naar deze webservers en de webservers kunnen reageren.

Wat u niet wilt toestaan dat is een front-end-webserver aan een uitgaande aanvraag starten. Dergelijke aanvragen mogelijk een beveiligingsrisico weer omdat deze verbindingen kunnen worden gebruikt om malware te downloaden. Zelfs als u dat deze-front-endservers om te starten, uitgaande aanvragen met het internet wilt, is het raadzaam om te zorgen dat ze via uw on-premises web proxy's. Hiermee kunt u profiteren van URL-filtering en logboekregistratie.

In plaats daarvan, zou u geforceerde tunneling gebruiken om dit te voorkomen. Wanneer u de geforceerde tunneling inschakelt, worden alle verbindingen met internet via uw on-premises gateway geforceerd. U kunt configureren om geforceerde tunnels zijn door te profiteren van udr's.

Meer informatie:

* [Wat zijn de gebruiker gedefinieerde Routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Beveiliging van virtuele netwerkapparaten

Terwijl de nsg's, udr's en geforceerde tunnels u een niveau van beveiliging op het netwerk en transport lagen van bieden de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), u mogelijk ook beveiliging wilt inschakelen op een niveau hoger is dan het netwerk.

Bijvoorbeeld, zijn uw beveiligingsvereisten voor onder andere:

* Verificatie en autorisatie voordat de toegang tot uw toepassing
* Indringingsdetectie en inbraakdetectie antwoord
* Controle van de toepassing laag voor protocollen die op hoog niveau
* URL-filtering
* Netwerk niveau antivirus- en antimalwaresoftware
* Anti-bot-beveiliging
* Toegangsbeheer voor toepassingen
* Extra DDoS-beveiliging (boven de DDoS protection wordt geleverd door de Azure-infrastructuur zelf)

U kunt toegang tot de beveiligingsfuncties van deze verbeterde netwerk met behulp van een Azure-partner. U kunt de meest recente Azure-partner-netwerk beveiligingsoplossingen vinden door naar de pagina de [Azure Marketplace](https://azure.microsoft.com/marketplace/), en het zoeken naar "beveiliging" en "netwerkbeveiliging."

## <a name="azure-firewall"></a>Azure Firewall

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledige stateful firewall als een service met ingebouwde hoge beschikbaarheid en onbeperkte cloudschaalbaarheid. Sommige functies zijn onder andere:

* Hoge beschikbaarheid
* Cloudschaalbaarheid
* Regels voor het filteren van de FQDN van toepassingen
* Regels voor het filteren van netwerkverkeer

Meer informatie:

* [Overzicht van de Firewall van Azure](../firewall/overview.md)

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Veilige externe toegang en cross-premises connectiviteit

Installatie, configuratie en beheer van de behoeften van uw Azure-resources op afstand worden uitgevoerd. Bovendien kunt u implementeren [hybride IT](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -oplossingen waarbij on-premises onderdelen en in de openbare cloud van Azure. Deze scenario's vereisen veilige externe toegang.

Azure-netwerken ondersteunt de volgende veilige externe toegang-scenario's:

* Afzonderlijke werkstations verbinden met een virtueel netwerk
* Uw on-premises netwerk verbinden met een virtueel netwerk met een VPN-verbinding
* Uw on-premises netwerk verbinden met een virtueel netwerk met een toegewezen WAN-verbinding
* Virtuele netwerken met elkaar verbinden

### <a name="connect-individual-workstations-to-a-virtual-network"></a>Afzonderlijke werkstations verbinden met een virtueel netwerk

Het is raadzaam om in te schakelen van individuele ontwerpers of door personeel dat bewerkingen voor het beheren van virtuele machines en services in Azure. Stel dat u hebt toegang tot een virtuele machine op een virtueel netwerk. Maar het beveiligingsbeleid kan geen externe RDP of SSH-toegang tot afzonderlijke virtuele machines. In dit geval kunt u een [punt-naar-site VPN](../vpn-gateway/point-to-site-about.md) verbinding.

De punt-naar-site VPN-verbinding kunt u voor het instellen van een privé en veilig verbinding tussen de gebruiker en het virtuele netwerk. Wanneer de VPN-verbinding tot stand is gebracht, de gebruiker kan RDP of SSH via de VPN-verbinding in een virtuele machine op het virtuele netwerk. (Hierbij wordt ervan uitgegaan dat de gebruiker kan worden geverifieerd en gemachtigd is.) Biedt ondersteuning voor punt-naar-site-VPN:

* Secure Socket Tunneling Protocol (SSTP), een eigen VPN op basis van een SSL-protocol. Een SSL VPN-oplossing kan passeren, firewalls, omdat de meeste firewalls open TCP-poort 443, dat gebruikmaakt van SSL. SSTP wordt alleen ondersteund op Windows-apparaten. Azure ondersteunt alle versies van Windows die SSTP (Windows 7 en hoger) hebben.

* IKEv2 VPN, een op standaarden gebaseerde IPsec VPN-oplossing. IKEv2 VPN kan worden gebruikt om verbinding te maken vanaf Mac-apparaten (OSX-versie 10.11 en hoger).

* [OpenVPN](https://azure.microsoft.com/updates/openvpn-support-for-azure-vpn-gateways/)

Meer informatie:

* [Een punt-naar-site-verbinding met een virtueel netwerk met behulp van PowerShell configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>Uw on-premises netwerk verbinden met een virtueel netwerk met een VPN-verbinding

Mogelijk wilt u uw hele bedrijfsnetwerk, of delen van het verbinden met een virtueel netwerk. Dit is gebruikelijk in hybride IT-scenario's, waar organisaties [hun on-premises datacentrum uitbreiden naar Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In veel gevallen hosten organisaties delen van een service in Azure en delen on-premises. Bijvoorbeeld, kunnen ze doen wanneer een oplossing front-end webservers in Azure omvat en on-premises back-end-databases. Deze typen 'cross-premises' verbindingen maken ook beheer van Azure bevindt resources meer beveiligen, en scenario's, zoals Active Directory-domeincontrollers uitbreiden naar Azure.

Een manier om uit te voeren, dit is met een [site-naar-site VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Het verschil tussen een site-naar-site-VPN- en een punt-naar-site-VPN is dat de laatste verbinding moet voor een apparaat met een virtueel netwerk maken. Een site-naar-site-VPN verbindt een heel netwerk (zoals uw on-premises netwerk) met een virtueel netwerk. Site-naar-site VPN-verbindingen met een virtueel netwerk gebruiken de zwaar beveiligde IPsec-tunnelmodus VPN-protocol.

Meer informatie:

* [Een Resource Manager VNet maken met een site-naar-site VPN-verbinding met Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Planning en ontwerp voor VPN Gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>Uw on-premises netwerk verbinden met een virtueel netwerk met een toegewezen WAN-verbinding

Punt-naar-site en site-naar-site VPN-verbindingen zijn nuttig voor het inschakelen van cross-premises-connectiviteit. Sommige organisaties overwegen echter deze hebben de volgende nadelen:

* VPN-verbindingen gaan gegevens via internet. Hiermee wordt aangegeven dat deze verbindingen met mogelijke beveiligingsproblemen die betrokken zijn bij het verplaatsen van gegevens via een openbaar netwerk. Bovendien kunnen niet de betrouwbaarheid en beschikbaarheid voor verbindingen via internet worden gegarandeerd.
* VPN-verbindingen naar virtuele netwerken hebben mogelijk niet de bandbreedte voor sommige toepassingen en toepassing zodra ze max uit met ongeveer 200 Mbps.

Organisaties die normaal gesproken moeten van het hoogste niveau van beveiliging en beschikbaarheid voor hun cross-premises verbindingen gebruiken specifieke WAN-verbindingen voor verbinding met externe sites. Azure biedt de mogelijkheid om met een toegewezen WAN-verbinding die u kunt uw on-premises netwerk verbinden met een virtueel netwerk. Azure ExpressRoute, met Express route direct- en Express-route wereldwijd bereik inschakelen dit.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)
* [ExpressRoute direct](../expressroute/expressroute-erdirect-about.md)
* [Express route wereldwijd bereik](..//expressroute/expressroute-global-reach.md)

### <a name="connect-virtual-networks-to-each-other"></a>Virtuele netwerken met elkaar verbinden

Het is mogelijk te veel virtuele netwerken gebruiken om uw implementaties. Er zijn verschillende redenen waarom u dit kunt doen. Voor een vereenvoudigd beheer kunt u, of kunt u betere beveiliging. Ongeacht de reden voor het plaatsen van resources op verschillende virtuele netwerken, is er mogelijk dat u resources op elk van de netwerken met elkaar verbinden.

Een optie is voor services op één virtueel netwerk verbinding maken met services op een ander virtueel netwerk, met "terug lussen" via het internet. De verbinding wordt gestart op één virtueel netwerk, verloopt via het internet en vervolgens weer naar het virtuele doelnetwerk. Deze optie wordt aangegeven dat de verbinding met de beveiligingskwesties op te lossen een internet-communicatie.

Een betere optie is mogelijk te maken van een site-naar-site-VPN dat verbinding tussen twee virtuele netwerken maakt. Deze methode maakt gebruik van dezelfde [IPSec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) protocol als de cross-premises site-naar-site VPN-verbinding wordt gebruikt, zoals hierboven vermeld.

Het voordeel van deze benadering is dat de VPN-verbinding tot stand is gebracht via de Azure-netwerk-fabric, in plaats van met elkaar verbinden via het internet. Dit biedt u een extra beveiligingslaag, vergeleken met site-naar-site-VPN's die verbinding via internet maken.

Meer informatie:

* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Een andere manier om uw virtuele netwerken te verbinden is [VNET-peering](../virtual-network/virtual-network-peering-overview.md). Deze functie kunt u verbinding maken met twee netwerken in Azure, zodat de communicatie ertussen gebeurt via het Microsoft-backbone-infrastructuur zonder dat dit ooit gebeurt via het Internet. VNET-peering kan verbinding maken met twee VNETs in dezelfde regio of twee VNETs Azure-regio's. Nsg's kunnen worden gebruikt om de connectiviteit tussen verschillende subnetten of systemen beperken.

## <a name="availability"></a>Beschikbaarheid

Beschikbaarheid is een belangrijk onderdeel van een security-programma. Als uw gebruikers en systemen geen toegang tot wat ze nodig hebben voor toegang tot via het netwerk, de service kan worden beschouwd als aangetast. Azure heeft netwerktechnologieën die ondersteuning bieden voor de volgende mechanismen voor hoge beschikbaarheid:

* HTTP-gebaseerde taakverdeling
* Niveau voor netwerktaakverdeling
* Globale taakverdeling

Taakverdeling is een mechanisme dat is ontworpen voor het gelijkmatig verdelen verbindingen tussen meerdere apparaten. De doelstellingen van taakverdeling zijn:

* Om beschikbaarheid te vergroten. Wanneer u saldo-verbindingen op meerdere apparaten laadt, zijn een of meer van de apparaten niet beschikbaar zonder verlies van de service. De services die worden uitgevoerd op de resterende online apparaten kunnen nog steeds de inhoud van de service.
* Voor betere prestaties. Wanneer u saldo verbindingen op meerdere apparaten laadt, geen een enkel apparaat voor het afhandelen van de verwerking. In plaats daarvan de capaciteit en geheugenbronnen eisen voor het uitvoeren van de inhoud is verdeeld over meerdere apparaten.

### <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling

Organisaties die worden uitgevoerd op basis van web services vaak vereisen dat een HTTP-gebaseerde load balancer voor deze webservices. Dit zorgt ervoor dat voldoende niveaus qua prestaties en hoge beschikbaarheid. Traditionele, op basis van netwerk-load balancers, is afhankelijk van netwerk- en transport layer-protocollen. HTTP-gebaseerde load balancers worden aan de andere kant beslissingen op basis van kenmerken van het HTTP-protocol.

Azure Application Gateway biedt HTTP-gebaseerde taakverdeling voor uw web gebaseerde services. Application Gateway ondersteunt:

* Cookies gebaseerde sessieaffiniteit. Deze functionaliteit zorgt ervoor dat de verbindingen met een van de servers achter deze load balancer tussen de client en server intact blijft. Dit zorgt ervoor dat de stabiliteit van transacties.
* SSL-offload. Wanneer een client verbinding met de load balancer maakt, worden die sessie worden versleuteld met behulp van het protocol HTTPS (SSL). U kunt echter om betere prestaties, het HTTP (niet-versleutelde)-protocol gebruiken om verbinding tussen de load balancer en de server achter de load balancer te maken. Dit heet 'SSL-offload,' omdat de webservers achter de load balancer de processoroverhead die betrokken zijn bij de versleuteling niet werkt. De webservers kunnen daarom serviceaanvragen sneller.
* URL-routering op basis van inhoud. Deze functie maakt het mogelijk is voor de load balancer om beslissingen te nemen over waar u kunt de volgende verbindingen op basis van de doel-URL. Dit biedt veel meer flexibiliteit dan oplossingen waarmee u load balancing beslissingen op basis van IP-adressen.

Meer informatie:

* [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Niveau voor netwerktaakverdeling

In tegenstelling tot HTTP-gebaseerde load balancing, beslissingen netwerktaakverdeling voor niveau op basis van IP-adres en poort (TCP of UDP) getallen.
U kunt profiteren van de voordelen van niveau voor netwerktaakverdeling in Azure met behulp van Azure Load Balancer. Enkele belangrijke kenmerken van de Load Balancer zijn onder andere:

* Netwerktaakverdeling voor niveau op basis van IP-adres en poort nummers.
* Ondersteuning voor alle application layer-protocol.
* Verdeelt de belasting op virtuele Azure-machines en rolinstanties van cloudservices.
* Kan worden gebruikt voor internetgerichte (externe load balancing) en niet-internet facing (interne taakverdeling) toepassingen en virtuele machines.
* Eindpunt controleren, dat wordt gebruikt om te bepalen als een van de services achter de load balancer niet beschikbaar zijn geworden.

Meer informatie:

* [Internetgerichte load balancer tussen meerdere virtuele machines of services](../load-balancer/load-balancer-internet-overview.md)
* [Overzicht van interne load balancer](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globale taakverdeling

Sommige organisaties wil het hoogste niveau van beschikbaarheid mogelijk. Er is een manier om dit doel te bereiken om toepassingen te hosten in wereldwijd verspreide datacenters. Wanneer een toepassing wordt gehost in datacenters over de hele wereld, is het mogelijk dat een hele geopolitieke regio niet beschikbaar en nog steeds van de toepassing en worden uitgevoerd.

Deze strategie load balancing kan ook prestatievoordelen opleveren. U kunt aanvragen voor de service naar het datacenter die het dichtst bij het apparaat dat ervoor zorgt dat de aanvraag sturen.

In Azure krijgt u de voordelen van het globale taakverdeling met behulp van Azure Traffic Manager.

Meer informatie:

* [Wat is Traffic Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="name-resolution"></a>Naamomzetting

Naamomzetting is een belangrijke functie voor alle services die u in Azure hosten. Vanuit het beveiligingsoogpunt kan inbreuk op de naam van het probleem zou moeten functie leiden tot een aanvaller omleiden van aanvragen van uw sites van een aanvaller. Beveiligde naamomzetting is een vereiste voor alle services in de cloud worden gehost.

Er zijn twee soorten naamomzetting die u afhandelen moet:

* Interne naamomzetting. Dit wordt gebruikt door de services op uw virtuele netwerken, uw on-premises netwerken of beide. Namen die worden gebruikt voor interne naamomzetting zijn niet toegankelijk via internet. Voor een optimale beveiliging is het belangrijk dat uw interne naam resolutie-schema niet toegankelijk voor externe gebruikers is.
* Externe naamomzetting. Dit wordt gebruikt door mensen en apparaten die buiten uw on-premises netwerken en virtuele netwerken. Dit zijn de namen die zichtbaar zijn voor het internet en worden gebruikt om te leiden van verbinding met uw cloud-gebaseerde services.

Voor interne naamomzetting hebt u twee opties:

* Een virtueel netwerk DNS-server. Wanneer u een nieuw virtueel netwerk maakt, wordt een DNS-server voor u gemaakt. Deze DNS-server kan de namen van de machines die zich op dit virtuele netwerk omzetten. Deze DNS-server kan niet worden geconfigureerd, wordt beheerd door beheer van Azure-infrastructuur en daarom kunt u uw naam resolutie-oplossing beveiligen.
* Breng uw eigen DNS-server. U hebt de mogelijkheid van het plaatsen van een DNS-server van uw eigen keuze in het virtuele netwerk. Deze DNS-server is dat een Active Directory geïntegreerde DNS-server of een specifieke DNS-server-oplossing geleverd door een Azure-partner die u via de Azure Marketplace verkrijgen kunt.

Meer informatie:

* [Overzicht van Virtual network](../virtual-network/virtual-networks-overview.md)
* [DNS-Servers die worden gebruikt door een virtueel netwerk beheren](../virtual-network/manage-virtual-network.md#change-dns-servers)

Voor het omzetten van de externe naam hebt u twee opties:

* Uw eigen externe DNS-server on-premises hosten.
* Host uw eigen externe DNS-server met een serviceprovider.

Veel grote organisaties hosten hun eigen DNS-servers on-premises. Ze kunnen dit doen omdat ze de netwerken expertise en wereldwijde aanwezigheid om dit te doen.

In de meeste gevallen is het beter voor het hosten van uw services voor DNS-naamomzetting met een serviceprovider. Deze serviceproviders hebben de expertise van netwerk en wereldwijde aanwezigheid om ervoor te zorgen zeer hoge beschikbaarheid voor uw services voor naamomzetting. Beschikbaarheid is essentieel voor DNS-services, omdat als uw naamomzettingsservices mislukken, niemand kunnen de services van uw internetgerichte bereiken.

Azure voorziet u van een maximaal beschikbare en zeer goed presterende externe DNS-oplossing in de vorm van Azure DNS. Deze externe naam resolutie-oplossing maakt gebruik van de wereldwijde Azure DNS-infrastructuur. Hiermee kunt u voor het hosten van uw domein in Azure, met behulp van dezelfde referenties, API's, hulpprogramma's en facturering als uw andere Azure-services. Als onderdeel van Azure neemt deze ook de krachtige beveiliging-besturingselementen die zijn ingebouwd in het platform.

Meer informatie:

* [Overzicht van Azure DNS](../dns/dns-overview.md)
* [Azure DNS private zones](../dns/private-dns-overview.md) kunt u persoonlijke DNS-namen voor Azure-resources in plaats van de namen van de automatisch toegewezen zonder de noodzaak om toe te voegen een aangepaste DNS-oplossing te configureren.

## <a name="perimeter-network-architecture"></a>Perimeter-netwerkarchitectuur

Veel grote organisaties perimeternetwerken gebruiken bij het segmenteren van hun netwerken, en een buffer-zone tussen internet en hun services. De perimeter-gedeelte van het netwerk wordt beschouwd als een laag-beveiligingszone en geen waardevolle activa in dat netwerksegment worden geplaatst. Hier ziet u meestal beveiliging netwerkapparaten waarvoor een netwerkinterface in de perimeter-netwerksegment. Een andere netwerkinterface is verbonden met een netwerk met virtuele machines en services die binnenkomende verbindingen via internet accepteren.

In een aantal verschillende manieren kunt u perimeternetwerken ontwerpen. De beslissing om een perimeternetwerk en welk type perimeter network te gebruiken als u besluit te gebruiken, te implementeren, is afhankelijk van de beveiligingsvereisten van uw netwerk.

Meer informatie:

* [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md)

## <a name="azure-ddos-protection"></a>Azure DDoS Protection

DDoS-aanvallen (Distributed Denial of Service-aanvallen) vormen een van de grootste beschikbaarheids- en beveiligingsproblemen voor klanten die hun toepassingen verplaatsen naar de cloud. Een DDoS-aanval wil de bronnen van een toepassing, waardoor de toepassing niet beschikbaar voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op elk eindpunt dat openbaar bereikbaar is via internet.
Microsoft biedt DDoS protection, ook wel **Basic** als onderdeel van het Azure-Platform. Dit wordt geleverd zonder kosten en bevat altijd op bewakings- en realtime oplossing voor algemene op netwerkniveau-aanvallen. Naast de die deel uitmaakt van DDoS protection **Basic** kunt u de **Standard** optie. DDoS Protection Standard-functies zijn onder andere:

* **Systeemeigen platform-integratie:** Geïntegreerd in Azure. Bevat configuratie via de Azure-portal. DDoS Protection standaard begrijpt dat uw resources en resourceconfiguratie.
* **Ingebouwde beveiliging:** Vereenvoudigde configuratie beschermt meteen alle resources in een virtueel netwerk zodra DDoS Protection standaard is ingeschakeld. Er is geen tussenkomst of gebruiker definitie is vereist. DDoS Protection standaard vermindert meteen en automatisch de aanval, zodra deze wordt gedetecteerd.
* **Verkeer altijd controleren:** Uw verkeerspatronen van toepassingen worden bewaakt 24 uur per dag, 7 dagen per week, op zoek naar indicatoren van DDoS-aanvallen. Risicobeperking wordt uitgevoerd wanneer het beveiligingsbeleid voor apps worden overschreden.
* **Risicobeperking rapporten aanvallen** aanval risicobeperking rapporten samengevoegde netwerk stroomgegevens gebruiken om aan te bieden gedetailleerde informatie over aanvallen die gericht is op uw resources.
* **Risicobeperking Flow logboeken aanvallen** aanval risicobeperking Flow Logboeken kunt u bekijken van het verwijderde verkeer doorgestuurd verkeer en andere gegevens van de aanval in bijna realtime tijdens een actieve DDoS-aanval.
* **Adaptieve afstemming:** Intelligente verkeer profilering leert van verkeer van uw toepassing gedurende een periode, selecteert en het profiel dat het meest geschikt is voor uw service-updates. Het profiel wordt aangepast wanneer verkeer na verloop van tijd verandert. Laag-3 in laag 7 protection: Biedt volledige stack DDoS protection, gebruikt in combinatie met een web application firewall.
* **Uitgebreide risicobeperking schaal:** Meer dan 60 verschillende aanval typen kunnen worden verholpen, met wereldwijde capaciteit, om te beveiligen tegen het grootste bekende DDoS-aanvallen.
* **Aanval metrische gegevens:** Samengevatte metrische gegevens van elke aanval zijn toegankelijk zijn via Azure Monitor.
* **Aanval waarschuwingen:** Waarschuwingen kunnen worden geconfigureerd op het starten en stoppen van een aanval en over de duur van de aanval, met ingebouwde aanval metrische gegevens. Waarschuwingen integreren in uw operationele software, zoals Microsoft Azure Log Analytics, Splunk, Azure Storage, e-mailadres en de Azure-portal.
* **Kosten garantie:**  Overdracht van gegevens en toepassingen scale-out servicetegoeden voor gedocumenteerde DDoS-aanvallen.
* **DDoS-snelle responsieve** DDoS Protection Standard-klanten hebben nu toegang tot Rapid Response team tijdens een actieve aanval. DRR kan helpen bij onderzoek van de aanval, aangepaste oplossingen tijdens een aanval en na aanval analyse.


Meer informatie:

* [Overzicht van DDOS protection](../virtual-network/ddos-protection-overview.md)

## <a name="azure-front-door"></a>Azure voordeur

Azure voordeur Service kunt u definiëren, beheren en bewaken van de globale routering van uw webverkeer. Het optimaliseren van uw verkeer routeren voor optimale prestaties en hoge beschikbaarheid. Met Azure Front Door kunt u regels voor toegangsbeheer maken voor aangepaste webtoepassingsfirewalls (WAF) om uw HTTP/HTTPS-workload te beschermen tegen exploitatie op basis van klant-IP-adressen, landcode en http-parameters. Bovendien voordeur ook kunt u regels voor het gevecht kwaadaardig botverkeer snelheidsbeperking maken, het SSL-offloading en per-HTTP/HTTPS-aanvraag bevat toepassingslaag verwerking.

Voordeur-platform zelf wordt beveiligd door Azure DDoS Protection Basic. Voor verdere bescherming kan Azure DDoS Protection Standard worden ingeschakeld op uw VNETs en bronnen beschermen tegen netwerklaagaanvallen (TCP/UDP) via automatische afstemming en risicobeperking. Voordeur een omgekeerde proxy van laag 7, wordt hierdoor kan alleen webverkeer doorgeven aan de back-end servers en andere typen verkeer standaard geblokkeerd.

Meer informatie:

* Klep mogelijkheden die u kunt bekijken voor meer informatie over de hele set Azure Front de [voordeur Azure-overzicht](../frontdoor/front-door-overview.md)

## <a name="azure-traffic-manager"></a>Met Azure Traffic manager

Azure Traffic Manager is een op DNS gebaseerde load balancer waarmee u verkeer optimaal over services kunt verdelen in Azure-regio's wereldwijd, terwijl u over hoge beschikbaarheid en een hoge reactiesnelheid beschikt. Traffic Manager maakt gebruik van DNS om aanvragen van clients door te sturen naar de meest geschikte eindpunten op basis van een methode om verkeer te routeren en van de status van de eindpunten. Een eindpunt is een internetgerichte service die binnen of buiten Azure wordt gehost. Traffic manager bewaakt de eindpunten en heeft geen directe voor verkeer naar alle eindpunten die niet beschikbaar zijn.

Meer informatie:

* [Overzicht van Azure Traffic manager](../traffic-manager/traffic-manager-overview.md)

## <a name="monitoring-and-threat-detection"></a>Controle en detectie van bedreigingen

Azure biedt mogelijkheden voor het op dit gebied key van vroege detectie, bewaking, en het verzamelen en controleren van netwerkverkeer.

### <a name="azure-network-watcher"></a>Azure Network Watcher

Azure Network Watcher kunt u op te lossen, en biedt een geheel nieuwe set hulpprogramma's om u te helpen bij de identificatie van beveiligingsproblemen met zich mee.

[Weergave van de beveiligingsgroep](../network-watcher/network-watcher-security-group-view-overview.md) helpt bij de naleving van controlebeleid en beveiligingslogboek van virtuele Machines. Deze functie gebruiken om uit te voeren programmatische audits, vergelijken van de basislijn-beleid dat door uw organisatie aan effectieve regels voor elk van uw VM's gedefinieerd. Hiermee kunt u eventuele afwijkende configuraties te identificeren.

[Pakketopname](../network-watcher/network-watcher-packet-capture-overview.md) kunt u netwerkverkeer naar en van de virtuele machine vastleggen. U kunt verzamelen van netwerkstatistieken en oplossen van problemen met toepassingen, die zeer waardevol bij het onderzoek van indringers netwerk worden kunnen. U kunt deze functie samen met Azure Functions ook gebruiken om te beginnen netwerkopnamen in reactie op een specifieke Azure-waarschuwingen.

Zie voor meer informatie over Network Watcher en het starten van enkele van de functionaliteit testen in uw labs [Azure network watcher bewakingsoverzicht](../network-watcher/network-watcher-monitoring-overview.md).

>[!NOTE]
Controleer voor de meest recente meldingen over de beschikbaarheid en de status van deze service, de [updates van de Azure-pagina](https://azure.microsoft.com/updates/?product=network-watcher).

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center helpt u bij het voorkomen, detecteren en direct reageren op bedreigingen, en biedt dat u meer inzicht in, en controle, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een groot aantal beveiligingsoplossingen te detecteren.

Security Center helpt u te optimaliseren en beveiliging van het netwerk door te controleren:

* Het leveren van netwerk-aanbevelingen voor beveiliging.
* Bewaking van de status van de configuratie van uw netwerkbeveiliging.
* U wordt gewaarschuwd voor netwerk op basis van bedreigingen, zowel op het niveau van het eindpunt en het netwerk.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)

### <a name="virtual-network-tap"></a>Virtual Network TAP

Met Azure virtual network TAP (Terminal Access Point) kunt u continu stroom netwerkverkeer van uw virtuele machine naar een netwerk-pakket collector of analytics-hulpprogramma. De collector of analytics-hulpprogramma wordt geleverd door een partner netwerk, virtueel apparaat. U kunt hetzelfde virtuele netwerk resource te TIKKEN op het totale verkeer van meerdere netwerkinterfaces in dezelfde of verschillende abonnementen.

Meer informatie:

* [Virtueel netwerk-TAP](../virtual-network/virtual-network-tap-overview.md)

### <a name="logging"></a>Logboekregistratie

Logboekregistratie op het niveau van een netwerk is een belangrijke functie voor elk netwerk security scenario. In Azure, kunt u zich aanmelden voor nsg's om op te halen netwerkniveau logboekinformatie verkregen gegevens. Met NSG-logboeken krijgt u informatie uit:

* [Activiteitenlogboeken](../azure-monitor/platform/activity-logs-overview.md). Deze Logboeken gebruiken om alle bewerkingen die worden verzonden naar uw Azure-abonnementen weer te geven. Deze logboeken zijn standaard ingeschakeld en kunnen worden gebruikt in Azure portal. Ze werden voorheen bekend als controle- of operationele Logboeken.
* Gebeurtenislogboeken. Deze logboeken bevatten informatie over welke NSG-regels zijn toegepast.
* Logboeken voor prestatiemeteritems. Deze logboeken laten u weten hoe vaak elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

U kunt ook [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), een krachtig hulpmiddel voor gegevensvisualisatie, weergeven en analyseren van deze logboeken.
Meer informatie:

* [Logboekanalyses voor Netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md)
