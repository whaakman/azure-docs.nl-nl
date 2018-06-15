---
title: Beveiligingsconcepten en-vereisten in Azure | Microsoft Docs
description: In dit artikel bevat basic uitleg over core network security concepten en vereisten en informatie over Azure in elk van deze gebieden biedt.
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
ms.openlocfilehash: fbd589aedb955ee4bd61dc0ec754d8713a98179a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365624"
---
# <a name="azure-network-security-overview"></a>Overzicht van Azure-netwerk-beveiliging
Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen bronnen in Azure, tussen on-premises en Azure gehoste bronnen, en naar en van het internet en Azure.

Het doel van dit artikel is waarin wordt uitgelegd wat Azure biedt in het gebied van netwerkbeveiliging. Naast eenvoudige uitleg over de vereisten en concepten van core-beveiliging kunt u meer informatie over:

* Azure-netwerken
* Netwerktoegangsbeheer
* Veilige externe toegang en cross-premises-connectiviteit
* Beschikbaarheid
* Naamomzetting
* Perimeter-netwerkarchitectuur (DMZ)
* Controle en detectie van bedreigingen

## <a name="azure-networking"></a>Azure-netwerken
Verbinding met het netwerk, moeten virtuele machines. Azure vereist ter ondersteuning van dit vereiste, virtuele machines te verbinden met Azure Virtual Network. Een virtueel netwerk is een logische constructie die is gebouwd op de fysieke netwerk van Azure-infrastructuur. Elk logisch netwerk voor virtuele is geïsoleerd van andere virtuele netwerken. Dit zorgt ervoor dat het netwerkverkeer in uw implementaties niet toegankelijk voor andere Azure-klanten is.

Meer informatie:

* [Overzicht van Virtual network](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Netwerktoegangsbeheer
Netwerktoegangsbeheer wordt de verbinding van en naar specifieke apparaten of subnetten binnen een virtueel netwerk te beperken. Het doel van netwerktoegangsbeheer is om toegang tot uw virtuele machines en services voor goedgekeurde gebruikers en apparaten te beperken. Toegangsbeheer zijn gebaseerd op de beslissingen die u wilt toestaan of weigeren van verbindingen naar en van de virtuele machine of service.

Azure ondersteunt verschillende soorten netwerktoegang, zoals:

* Regeling van de laag
* Besturingselement routeren en geforceerde tunneling
* Virtueel netwerk beveiligingsapparaten

### <a name="network-layer-control"></a>Regeling van de laag
Alle beveiligde implementatie vereist sommige meting van network access control. Het doel van netwerktoegangsbeheer is om communicatie van de virtuele machine met de benodigde systemen. Andere communicatiepogingen worden geblokkeerd.

Als u moet Basisnetwerk toegangsbeheer op gebruikersniveau (op basis van IP-adres en de TCP- of UDP-protocollen), kunt u Netwerkbeveiligingsgroepen (nsg's). Een NSG is een basic, stateful-, firewall voor pakketten en Hiermee kunt u toegangsbeheer op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg's bieden geen application layer inspectie of geverifieerde toegangsbeheer.

Meer informatie:

* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md)

### <a name="route-control-and-forced-tunneling"></a>Besturingselement routeren en geforceerde tunneling
De mogelijkheid om te bepalen de werking van de routering op uw virtuele netwerken is essentieel. Als routering onjuist is geconfigureerd, toepassingen en services die worden gehost op de virtuele machine mogelijk verbinding met niet-geautoriseerde apparaten, zoals systemen die eigendom zijn van en beheerd door potentiële aanvallers.

Azure-netwerken ondersteunt de mogelijkheid om aan te passen van het routeringsgedrag voor netwerkverkeer op uw virtuele netwerken. Hiermee kunt u voor het wijzigen van de standaard tabelvermeldingen routering in het virtuele netwerk. Besturingselement van het routeringsgedrag kunt u ervoor zorgen dat al het verkeer van een bepaald apparaat of een groep apparaten binnengaat of het virtuele netwerk via een specifieke locatie verlaat.

U wellicht bijvoorbeeld beveiliging van een virtueel netwerkapparaat in het virtuele netwerk. U om ervoor te zorgen dat alle verkeer van en naar het virtuele netwerk dat toestel virtuele beveiliging doorloopt. U kunt dit doen door het configureren van [gebruiker gedefinieerde Routes](../virtual-network/virtual-networks-udr-overview.md) (udr's) in Azure.

[Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme die u gebruiken kunt om ervoor te zorgen dat uw services niet zijn toegestaan voor het initiëren van een verbinding met apparaten op internet. Houd er rekening mee dat dit verschilt van binnenkomende verbindingen accepteert en vervolgens aan hen reageert. Front-endwebservers moeten reageren op aanvragen van internet-hosts en dus internet afkomstig verkeer wordt toegestaan inkomend verkeer met deze webservers en de webservers kunnen reageren.

Wat u niet wilt toestaan, is een front-endwebserver een uitgaande aanvraag initiëren. Dergelijke aanvragen mogelijk een beveiligingsrisico vertegenwoordigen, omdat deze verbindingen kunnen worden gebruikt voor het downloaden van schadelijke software. Zelfs als u dat deze-front-endservers uitgaande aanvragen met het internet te initiëren wilt, is het raadzaam om te zorgen dat ze de webproxy van uw lokale doorlopen. Hiermee kunt u profiteren van de URL voor het filteren en logboekregistratie.

In plaats daarvan zou u geforceerde tunneling gebruiken om dit te voorkomen. Wanneer u geforceerde tunneling inschakelt, worden alle verbindingen met het internet via uw on-premises gateway geforceerd. U kunt configureren om geforceerde tunneling door gebruik te maken van udr's.

Meer informatie:

* [Wat zijn de gebruiker gedefinieerde Routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtueel netwerk beveiligingsapparaten
Tijdens het nsg's, udr's en geforceerde tunneling u een niveau van beveiliging op de netwerk- en lagen van bieden de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), u kunt ook beveiliging op toepassingsniveau hoger is dan het netwerk inschakelen.

Bijvoorbeeld: de beveiligingsvereisten van uw kunnen opnemen:

* Verificatie en autorisatie voordat ze toegang krijgen tot uw toepassing
* Inbraakdetectie en inbraakdetectie antwoord
* Application layer inspectie van protocollen op hoog niveau
* URL-filtering
* Netwerk niveau antivirus- en antimalwaresoftware
* Bescherming tegen bot
* Toegangsbeheer voor toepassing
* Aanvullende DDoS-bescherming (boven de DDoS-bescherming geleverd door de Azure-infrastructuur zelf)

U kunt deze beveiligingsfuncties verbeterde netwerk openen met behulp van een Azure-partner-oplossing. U vindt de meest recente Azure partnernetwerk beveiligingsoplossingen in via de [Azure Marketplace](https://azure.microsoft.com/marketplace/), en zoeken naar 'beveiliging' en "netwerkbeveiliging."

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Veilige externe toegang en cross-premises-connectiviteit
Installatie, configuratie en beheer van de behoeften van uw Azure-resources op afstand worden uitgevoerd. Bovendien kunt u implementeren [hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) oplossingen die onderdelen on-premises en in de openbare Azure-cloud. Deze scenario's moet veilige externe toegang.

Azure-netwerken ondersteunt de volgende scenario's voor veilige externe toegang:

* Afzonderlijke werkstations verbinden met een virtueel netwerk
* Uw on-premises netwerk verbinden met een virtueel netwerk met een VPN
* Uw on-premises netwerk verbinden met een virtueel netwerk met een specifieke WAN-verbinding
* Virtuele netwerken met elkaar verbinden

### <a name="connect-individual-workstations-to-a-virtual-network"></a>Afzonderlijke werkstations verbinden met een virtueel netwerk
Het is raadzaam om in te schakelen individuele ontwikkelaars of medewerkers van de bewerkingen voor het beheren van virtuele machines en services in Azure. Stel dat u moet toegang hebben tot een virtuele machine op een virtueel netwerk. Maar het beveiligingsbeleid is niet toegestaan voor RDP of SSH externe toegang tot afzonderlijke virtuele machines. In dit geval kunt u een punt-naar-site VPN-verbinding.

De punt-naar-site VPN-verbinding gebruikt de [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protocol waarmee u kunt een persoonlijke en beveiligde verbinding tussen de gebruiker en het virtuele netwerk instellen. Wanneer de VPN-verbinding tot stand is gebracht, de gebruiker kan RDP of SSH via de VPN-verbinding in een virtuele machine op het virtuele netwerk. (Hierbij wordt verondersteld dat de gebruiker kan worden geverifieerd en gemachtigd is.)

Meer informatie:

* [Een punt-naar-site-verbinding met een virtueel netwerk met behulp van PowerShell configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>Uw on-premises netwerk verbinden met een virtueel netwerk met een VPN
Mogelijk wilt uw hele bedrijfsnetwerk of delen van het verbinden met een virtueel netwerk. Dit is gebruikelijk in hybride IT scenario's waarbij organisaties [datacenter op locatie uitbreiden in Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In veel gevallen hosten organisaties delen van een service in Azure en delen on-premises. Bijvoorbeeld, ze mogelijk doen wanneer een oplossing front-endwebservers in Azure bevat en lokale back-end-databases. Deze typen 'cross-premises' verbindingen maken ook beheer van Azure bevinden resources beter beveiligen en scenario's mogelijk, zoals Active Directory-domeincontrollers uit te breiden naar Azure.

Een manier voor het uitvoeren van dit is het gebruik van een [site-naar-site VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Het verschil tussen een site-naar-site-VPN en een punt-naar-site VPN is dat de laatste een enkel apparaat verbindt met een virtueel netwerk. Een site-naar-site VPN verbindt een gehele netwerk (zoals uw on-premises netwerk) met een virtueel netwerk. Site-naar-site VPN-verbindingen met een virtueel netwerk gebruiken maximaal beveiligde IPSec-tunnelmodus VPN-protocol.

Meer informatie:

* [Een Resource Manager VNet maken met een site-naar-site VPN-verbinding met de Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Planning en ontwerp voor VPN Gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>Uw on-premises netwerk verbinden met een virtueel netwerk met een specifieke WAN-verbinding
Punt-naar-site en site-naar-site VPN-verbindingen zijn nuttig voor het inschakelen van cross-premises connectiviteit. Sommige organisaties overwegen echter voor de volgende nadelen:

* VPN-verbindingen verplaatsen gegevens via internet. Hiermee wordt deze verbindingen met mogelijke beveiligingsproblemen die betrokken zijn bij het verplaatsen van gegevens via een openbaar netwerk. Bovendien worden niet betrouwbaarheid en beschikbaarheid voor internet-verbindingen gegarandeerd.
* VPN-verbindingen naar virtuele netwerken wellicht niet de bandbreedte voor sommige toepassingen en -doeleinden, als ze max uit met ongeveer 200 Mbps.

Organisaties die doorgaans moeten van het hoogste niveau van beveiliging en beschikbaarheid voor hun cross-premises verbindingen gebruiken specifieke WAN-verbindingen verbinding maken met externe sites. Azure biedt de mogelijkheid om met een specifieke WAN-verbinding die u kunt uw on-premises netwerk verbinden met een virtueel netwerk. Azure ExpressRoute kunt dit.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-virtual-networks-to-each-other"></a>Virtuele netwerken met elkaar verbinden
Het is mogelijk met meerdere virtuele netwerken voor uw implementaties. Er zijn diverse redenen waarom u dit kunt doen. U kunt het beheer vereenvoudigen of kunt u betere beveiliging. Ongeacht de reden voor het plaatsen van bronnen op verschillende virtuele netwerken kan gebeuren als u wilt dat resources op elk van de netwerken te verbinden met elkaar.

Een mogelijkheid voor services op één virtueel netwerk verbinding maken met services op een ander virtueel netwerk met 'terug lussen' is via het internet. De verbinding wordt gestart op een virtueel netwerk, verloopt via internet en vervolgens weer naar het virtuele netwerk van de bestemming. Deze optie wordt de verbinding met de beveiligingsproblemen die inherent aan internet-communicatie.

Een betere optie mogelijk te maken van een site-naar-site VPN die verbinding tussen twee virtuele netwerken maakt. Deze methode maakt gebruik van dezelfde [IPsec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) protocol als de cross-premises site-naar-site VPN-verbinding hierboven vermeld.

Het voordeel van deze benadering is dat de VPN-verbinding tot stand is gebracht via het netwerk van Azure-infrastructuur, in plaats van via internet verbinding maken. Dit biedt u een extra beveiligingslaag, vergeleken met de site-naar-site-VPN's die verbinding via internet maken.

Meer informatie:

* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Beschikbaarheid
Beschikbaarheid is een belangrijk onderdeel van elk beveiligingsprogramma. Als uw gebruikers en systemen geen toegang wat ze moeten toegang via het netwerk tot, de service kan worden overwogen aangetast. Azure heeft netwerktechnologieën die ondersteuning bieden voor de volgende mechanismen voor hoge beschikbaarheid:

* HTTP-gebaseerde taakverdeling
* Niveau voor netwerktaakverdeling
* Globale taakverdeling

Taakverdeling is een mechanisme ontworpen gelijkmatig distribueren verbindingen tussen meerdere apparaten. De doelstellingen van taakverdeling zijn:

* Om beschikbaarheid te verhogen. Wanneer u saldo verbindingen op meerdere apparaten laadt, zijn een of meer van de apparaten niet beschikbaar in gevaar de service. De services die worden uitgevoerd op de resterende online apparaten kunnen blijven voldoen aan de inhoud van de service.
* Voor betere prestaties. Wanneer u saldo verbindingen op meerdere apparaten laadt, geen één apparaat voor het afhandelen van de verwerking. In plaats daarvan wordt de capaciteit en geheugenbronnen eisen voor de inhoud verdeeld over meerdere apparaten.

### <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
Organisaties die vaak webservices uitvoeren willen een HTTP-gebaseerde taakverdeling voor deze webservices. Dit helpt ervoor te zorgen voldoende niveaus qua prestaties en hoge beschikbaarheid. Traditionele, op basis van network load balancers zijn afhankelijk van netwerk- en layer-protocollen. HTTP-gebaseerde netwerktaakverdelers aan de andere kant beslissingen nemen op basis van kenmerken van het HTTP-protocol.

Azure Application Gateway HTTP-gebaseerde taakverdeling biedt voor uw web gebaseerde services. Toepassingsgateway ondersteunt:

* Sessie cookies gebaseerde affiniteit. Deze mogelijkheid zorgt ervoor dat de verbindingen met een van de servers achter die load balancer tussen de client en server intact blijft. Dit zorgt ervoor dat de stabiliteit van transacties.
* SSL-offload. Wanneer een client verbinding met de load balancer maakt, wordt die sessie versleuteld met behulp van het protocol HTTPS (SSL). U kunt echter om het verhogen van de prestaties (niet-versleuteld) HTTP-protocol gebruiken verbinding maken tussen de load balancer en de webserver achter de load balancer. Dit wordt aangeduid als 'SSL-offload,' omdat de webservers achter de load balancer niet de betrokken met versleuteling processoroverhead optreden. De webservers kunnen daarom serviceaanvragen sneller.
* URL gebaseerde inhoud routering. Deze functie maakt het mogelijk is voor de load balancer om beslissingen te nemen over waar u kunt de volgende verbindingen op basis van de doel-URL. Dit biedt veel meer flexibiliteit dan oplossingen die u de werklast verdelen beslissingen op basis van IP-adressen.

Meer informatie:

* [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Niveau voor netwerktaakverdeling
In tegenstelling tot HTTP-gebaseerde taakverdeling, kunt u besluiten op basis van IP-adres en poort (TCP of UDP) nummers netwerktaakverdeling voor niveau.
U kunt de voordelen van het niveau voor netwerktaakverdeling in Azure met behulp van Azure Load Balancer toegang. Er zijn een aantal belangrijke kenmerken van de Load Balancer:

* Netwerktaakverdeling voor niveau op basis van IP-adres en poort nummers.
* Ondersteuning voor elk application layer protocol.
* Load balans wordt gezocht naar Azure virtuele machines en cloud services-rolexemplaren.
* Kan worden gebruikt voor zowel de internetgerichte (externe load balancing) en de internet-gerichte (interne load balancing)-toepassingen en virtuele machines.
* Eindpunt bewaking, die wordt gebruikt om te bepalen of een van de services die achter de load balancer niet beschikbaar zijn geworden.

Meer informatie:

* [Internet gerichte load balancer tussen meerdere virtuele machines of services](../load-balancer/load-balancer-internet-overview.md)
* [Interne load balancer-overzicht](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globale taakverdeling
Sommige organisaties willen het hoogste niveau van beschikbaarheid mogelijk. Een manier om dit doel te bereiken is host voor toepassingen in globaal gedistribueerde datacenters. Wanneer een toepassing wordt gehost in datacenters zich in de hele wereld, is het mogelijk voor een hele geopolitieke regio niet beschikbaar en hebt nog steeds de toepassing van en worden uitgevoerd.

Deze strategie taakverdeling levert ook prestatievoordelen. U kunt aanvragen voor de service naar het datacenter die het dichtst bij het apparaat dat de aanvraag wordt ingediend sturen.

In Azure krijgt u de voordelen van globale taakverdeling met behulp van Azure Traffic Manager.

Meer informatie:

* [Wat is Traffic Manager?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Naamomzetting
Naamomzetting is een essentiële functie voor alle services die u in Azure host. Vanuit beveiligingsoogpunt, kan inbreuk op een van de functie name resolution leiden tot een aanvaller omleiden van aanvragen van uw sites van een aanvaller. Beveiligde naamomzetting is een vereiste voor alle in de cloud gehoste services.

Er zijn twee soorten naamomzetting die uw vergt aandacht:

* Interne naamomzetting. Dit wordt gebruikt door de services op uw virtuele netwerken, uw on-premises netwerken of beide. Gebruikt voor interne naamomzetting namen zijn niet toegankelijk via het internet. Voor een optimale beveiliging is het belangrijk uw interne naam resolutie-schema is niet toegankelijk voor externe gebruikers.
* Externe naamomzetting. Dit wordt gebruikt door mensen en apparaten buiten uw on-premises netwerken en virtuele netwerken. Dit zijn de namen die zichtbaar zijn voor het internet en worden gebruikt voor rechtstreekse verbinding met uw cloud-gebaseerde services.

Voor interne naamomzetting hebt u twee opties:

* Een virtueel netwerk DNS-server. Wanneer u een nieuw virtueel netwerk maakt, wordt een DNS-server voor u gemaakt. Deze DNS-server kan de namen van de machines die zich bevinden op dit virtuele netwerk omzetten. Deze DNS-server kan niet worden geconfigureerd, wordt beheerd door beheer van Azure-infrastructuur en daarom kunt u uw name resolution-oplossing beveiligen.
* Breng uw eigen DNS-server. U hebt de optie bij het plaatsen van een DNS-server van uw eigen keuze in het virtuele netwerk. Deze DNS-server is dat een Active Directory geïntegreerde DNS-server of een specifieke DNS-server-oplossing geleverd door een Azure partner, kunt u vanuit Azure Marketplace.

Meer informatie:

* [Overzicht van Virtual network](../virtual-network/virtual-networks-overview.md)
* [DNS-Servers die worden gebruikt door een virtueel netwerk beheren](../virtual-network/manage-virtual-network.md#change-dns-servers)

Voor de omzetting van de externe naam hebt u twee opties:

* Uw eigen externe DNS-server on-premises hosten.
* Host uw eigen externe DNS-server met een serviceprovider.

Veel grote organisaties hosten hun eigen DNS-servers on-premises. Ze kunnen dit doen omdat ze de VPN-expertise en wereldwijde aanwezigheid om dit te doen.

In de meeste gevallen is het beter om uw DNS-naamomzettingsservices host met een serviceprovider. Deze serviceproviders hebben de netwerk-expertise en wereldwijde aanwezigheid zeer hoge beschikbaarheid voor uw services voor naamomzetting. Beschikbaarheid is essentieel voor DNS-services, omdat als uw naamomzettingsservices mislukken, niemand kunt bereiken van de services van uw internetverbinding.

Azure biedt u met een maximaal beschikbare en zodat externe DNS-oplossing in de vorm van Azure DNS. Deze oplossing externe naam oplossing maakt gebruik van de wereldwijd Azure DNS-infrastructuur. Hiermee kunt u voor het hosten van uw domein in Azure, met de dezelfde referenties, API's, hulpprogramma's en facturering als uw andere Azure-services. Als onderdeel van Azure, ook neemt de sterke beveiligingsmechanismen die is ingebouwd in het platform.

Meer informatie:

* [Azure DNS-overzicht](../dns/dns-overview.md)

## <a name="perimeter-network-architecture"></a>Perimeter-netwerkarchitectuur
Veel grote organisaties perimeternetwerken gebruiken bij het segmenteren van hun netwerken en een buffer-zone tussen het internet en hun services maken. De perimeter-gedeelte van het netwerk wordt beschouwd als een laag beveiligingszone en geen activa hoge waarde in die netwerksegment worden geplaatst. Doorgaans ziet u beveiliging netwerkapparaten waarvoor een netwerkinterface in de perimeter-netwerksegment. Een andere netwerkinterface is verbonden met een netwerk met virtuele machines en services die accepteren van binnenkomende verbindingen via internet.

U kunt perimeternetwerken ontwerpen in een aantal verschillende manieren. De beslissing voor het implementeren van een perimeternetwerk en welk type perimeter netwerk moet worden gebruikt als u wilt gebruiken, is afhankelijk van de beveiligingsvereisten van uw netwerk.

Meer informatie:

* [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>Controle en detectie van bedreigingen

Azure biedt mogelijkheden om u te helpen op dit gebied sleutel vroege detectie, bewaking, en het verzamelen en beoordelen van netwerkverkeer.

### <a name="azure-network-watcher"></a>Azure-netwerk-Watcher
Azure-netwerk-Watcher kunt u oplossen, en voorziet in een geheel nieuwe set van hulpprogramma's om u te helpen bij de identificatie van beveiligingsproblemen.

[Weergave van de beveiligingsgroep ](../network-watcher/network-watcher-security-group-view-overview.md) helpt bij de naleving van controle en beveiliging van virtuele Machines. Gebruik deze functie om uit te voeren programmatische audits vergelijken van de basislijn-beleidsregels dat is gedefinieerd in uw organisatie aan effectieve regels voor elk van uw virtuele machines. Hiermee kunt u afwijking van de configuratie te identificeren.

[Pakketopname](../network-watcher/network-watcher-packet-capture-overview.md) kunt u netwerkverkeer naar en van de virtuele machine vastleggen. U kunt netwerk statistische gegevens verzamelen en problemen oplossen van problemen met toepassingen, wat kunnen waardevol zijn bij het onderzoek van netwerk beveiligingsrisico's. U kunt ook deze functie samen met Azure Functions netwerkopnamen starten in reactie op specifieke waarschuwingen van Azure.

Zie voor meer informatie over de netwerk-Watcher en het testen van sommige functies in uw labs [Azure-netwerk-watcher bewakingsoverzicht](../network-watcher/network-watcher-monitoring-overview.md).

>[!NOTE]
Voor de meest recente meldingen op beschikbaarheid en de status van deze service controleert de [pagina Azure updates](https://azure.microsoft.com/updates/?product=network-watcher).

### <a name="azure-security-center"></a>Azure Security Center
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen en biedt dat u grotere zichtbaarheid van, en controle over, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een groot aantal beveiligingsoplossingen te detecteren.

Security Center helpt u te optimaliseren en netwerkbeveiliging door te controleren:

* Het bieden van netwerk-aanbevelingen voor beveiliging.
* Bewaken van de status van uw netwerkconfiguratie voor beveiliging.
* U wordt gewaarschuwd voor netwerk op basis van bedreigingen, zowel op het niveau van het eindpunt en het netwerk.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)


### <a name="logging"></a>Logboekregistratie
Logboekregistratie op het netwerkniveau van een is een belangrijke functie voor een netwerk voor beveiliging. U kunt zich aanmelden voor het nsg's ophalen netwerkniveau logboekinformatie verkregen gegevens in Azure. Met NSG logboekregistratie beschikt u over gegevens van:

* [Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Deze Logboeken gebruiken om alle bewerkingen die worden verzonden naar uw Azure-abonnementen weer te geven. Deze logboeken zijn standaard ingeschakeld en kunnen worden gebruikt in de Azure-portal. Ze werden voorheen bekend als audit- of operationele Logboeken.
* Gebeurtenis wordt geregistreerd. Deze logboeken bevatten informatie over welke NSG-regels zijn toegepast.
* Logboeken voor prestatiemeteritems. Deze logboeken zodat u weet hoe vaak elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

U kunt ook [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), een krachtige gegevensvisualisatie hulpprogramma, weergeven en analyseren van deze logboeken.

Meer informatie:

* [Log Analytics voor Netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md)
