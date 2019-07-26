---
title: Azure-netwerken | Microsoft Docs
description: Meer informatie over netwerk services in Azure en hun mogelijkheden.
services: networking
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2019
ms.author: kumud
ms.openlocfilehash: 9fb7fc9b4f0e5af0847876ff41b6a307f8a09749
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348040"
---
# <a name="azure-networking"></a>Azure-netwerken

De netwerk services in azure bieden diverse netwerk mogelijkheden die samen of afzonderlijk kunnen worden gebruikt. Klik op een van de volgende belang rijke mogelijkheden om meer te weten te komen over:
- [**Connectiviteits Services**](#connect): Maak verbinding met Azure-resources en on-premises resources met behulp van een of een combi natie van deze netwerk services in azure-Virtual Network (VNet), Virtual WAN, ExpressRoute, VPN Gateway, Azure DNS of Azure Bastion.
- [**Services voor toepassings beveiliging**](#protect) Bescherm uw toepassingen met een of een combi natie van deze netwerk services in azure-DDoS Protection, firewall, netwerk beveiligings groepen, Web Application firewall of Virtual Network-eind punten.
- [**Services**](#deliver) voor het leveren van toepassingen Lever toepassingen in het Azure-netwerk met behulp van een of een combi natie van deze netwerk services in azure-Content Delivery Network (CDN), Azure front-deur service, Traffic Manager, Application Gateway of Load Balancer.
- [**Netwerk bewaking**](#monitor) : Bewaak uw netwerk bronnen met behulp van een of een combi natie van deze netwerk services in Azure-Network Watcher, ExpressRoute Monitor, Azure monitor of VNet-Terminal toegangs punt (tik).

## <a name="connect"></a>Connectiviteits Services
 
In deze sectie worden de services beschreven die verbinding bieden tussen Azure-resources, connectiviteit van een on-premises netwerk met Azure-resources en vertakking tot filiaal connectiviteit in azure-Virtual Network, ExpressRoute, VPN Gateway, virtueel WAN, DNS en Azure Bastion.

|Service|Waarom gebruiken?|Scenario's|
|---|---|---|
|[Virtueel netwerk](#vnet)|Hiermee kunnen Azure-bronnen veilig communiceren met elkaar, Internet en on-premises netwerken.| <p>[Netwerkverkeer filteren](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[Netwerkverkeer routeren](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[Netwerktoegang tot resources beperken](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[Virtuele netwerken verbinden](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[ExpressRoute](#expressroute)|Breidt uw on-premises netwerken uit in de micro soft-Cloud via een persoonlijke verbinding die wordt vereenvoudigd door een connectiviteits provider.|<p>[Een ExpressRoute-circuit maken en wijzigen](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[Peering voor een ExpressRoute-circuit maken en wijzigen](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[Een VNet koppelen aan een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[Route filters configureren en beheren voor ExpressRoute-circuits](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN Gateway](#vpngateway)|Versleuteld verkeer verzenden tussen een virtueel Azure-netwerk en een on-premises locatie via het open bare Internet.|<p>[Site-naar-site-verbindingen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[VNet-naar-VNet-verbindingen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[Punt-naar-site-verbindingen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[Virtueel WAN](#virtualwan)|Optimaliseert en automatiseert de verbinding tussen filialen en Azure. Azure-regio's fungeren als hubs die u kunt gebruiken om uw filialen te koppelen aan.|<p>[Site-naar-site-verbindingen](../virtual-wan/virtual-wan-site-to-site-portal.md), [ExpressRoute-verbindingen](../virtual-wan/virtual-wan-expressroute-portal.md)</p>|
|[Azure DNS](#dns)|Host DNS-domeinen die naam omzetting bieden met behulp van Microsoft Azure-infra structuur.|<p>[Uw domein hosten in Azure DNS](../dns/dns-delegate-domain-azure-dns.md)</p><p>[DNS-records voor een web-app maken](../dns/dns-web-sites-custom-domain.md)</p> <p>[Een alias record maken voor Traffic Manager](../dns/tutorial-alias-tm.md)</p> <p>[Een alias record maken voor openbaar IP-adres](../dns/tutorial-alias-pip.md)</p> <p>[Een alias record maken voor de zone bron record](../dns/tutorial-alias-rr.md)</p>|
|[Azure-Bastion (preview-versie)](#bastion)|Configureer beveiligde en naadloze RDP-/SSH-connectiviteit in je virtuele machine, rechtstreeks in de Azure-portal via SSL. Wanneer u verbinding maakt via Azure Bastion, hebt u geen openbaar IP-adres nodig voor uw virtuele machines|<p>[Een Azure bastion-host maken](../bastion/bastion-create-host-portal.md)</p><p>[Via SSH verbinding maken met een virtuele Linux-machine](../bastion/bastion-connect-vm-ssh.md)</p><p>[Verbinding maken met behulp van RDP met een Windows VM](/bastion/bastion-connect-vm-rdp.md)</p>|
||||


### <a name="vnet"></a>Virtueel netwerk

Azure Virtual Network (VNet) is de fundamentele bouw steen voor uw particuliere netwerk in Azure. U kunt een VNets gebruiken om het volgende te doen:
- **Communiceren tussen Azure-resources**: U kunt virtuele machines en diverse andere soorten Azure-resources implementeren op een virtueel netwerk, zoals Azure App Service-omgevingen, de Azure Kubernetes Service (AKS) en Azure Virtual Machine Scale Sets. Zie [Integratie van virtuele netwerkservices](../virtual-network/virtual-network-for-azure-services.md) voor een volledige lijst met Azure-resources die u in een virtueel netwerk kunt implementeren.
- **Communiceren tussen**elkaar: U kunt virtuele netwerken met elkaar verbinden, zodat resources in beide virtuele netwerken met elkaar kunnen communiceren, met behulp van peering voor virtuele netwerken. De virtuele netwerken die u met elkaar verbindt, kunnen zich in dezelfde of verschillende Azure-regio's bevinden. Zie peering van [virtuele netwerken](../virtual-network/virtual-network-peering-overview.md)voor meer informatie.
- **Communiceren met Internet**: Alle resources in een VNet kunnen standaard uitgaand verkeer naar Internet communiceren. U kunt binnenkomend communiceren met een resource door er een openbaar IP-adres of een openbare Load Balancer aan toe te wijzen. U kunt ook [open bare IP-adressen](../virtual-network/virtual-network-public-ip-address.md) of open bare [Load Balancer](../load-balancer/load-balancer-overview.md) gebruiken om uw uitgaande verbindingen te beheren.
- **Communiceren met on-premises netwerken**: U kunt uw on-premises computers en netwerken verbinden met een virtueel netwerk met behulp van [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of [ExpressRoute](../expressroute/expressroute-introduction.md).

Zie [Wat is Azure Virtual Network?](../virtual-network/virtual-networks-overview.md)voor meer informatie.

### <a name="expressroute"></a>ExpressRoute
Met ExpressRoute kunt u uw on-premises netwerken in de micro soft Cloud uitbreiden via een persoonlijke verbinding die wordt vereenvoudigd door een connectiviteits provider. Deze verbinding is een privéverbinding. Verkeer gaat niet via internet. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.  Zie [Wat is ExpressRoute?](../expressroute/expressroute-introduction.md)voor meer informatie.

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpngateway"></a>VPN Gateway
VPN Gateway helpt u bij het maken van versleutelde cross-premises verbindingen met uw virtuele netwerk vanaf on-premises locaties of het maken van versleutelde verbindingen tussen VNets. Er zijn verschillende configuraties beschikbaar voor VPN Gateway verbindingen, zoals site-naar-site, punt-naar-site-of VNet-naar-VNet.
In het volgende diagram ziet u meerdere site-naar-site-VPN-verbindingen met hetzelfde virtuele netwerk.

![Verbindingen van site-naar-site Azure-VPN Gateway](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

Zie [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)voor meer informatie over verschillende typen VPN-verbindingen.

### <a name="virtualwan"></a>Virtueel WAN
Azure Virtual WAN is een netwerk service die zorgt voor geoptimaliseerde en geautomatiseerde vertakkings connectiviteit met, en via Azure. Azure-regio's fungeren als hubs die u kunt gebruiken om uw filialen te koppelen aan. U kunt de Azure-backbone gebruiken om ook vertakkingen te verbinden en te profiteren van de connectiviteit van Branch-to-VNet. Met Azure Virtual WAN worden veel Azure Cloud Connectivity Services, zoals site-naar-site VPN, ExpressRoute, punt-naar-site-gebruikers-VPN, in één operationele interface gecombineerd. Connectiviteit met Azure VNets wordt tot stand gebracht met behulp van virtuele netwerk verbindingen. Zie [Wat is virtueel WAN van Azure?](../virtual-wan/virtual-wan-about.md)voor meer informatie.

![Virtual WAN-diagram](./media/networking-overview/virtualwan1.png)

### <a name="dns"></a>Azure DNS
Azure DNS is een service voor het hosten van DNS-domeinen die naamomzetting verzorgt met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Zie [Wat is Azure DNS?](../dns/dns-overview.md)voor meer informatie.

### <a name="bastion"></a>Azure-Bastion (preview-versie)
De Azure Bastion-service is een nieuwe, volledig door het platform beheerde PaaS-service die u in uw virtuele netwerk hebt ingericht. Het biedt een veilige en naadloze RDP/SSH-verbinding met uw virtuele machines rechtstreeks in de Azure Portal via SSL. Wanneer u verbinding maakt met Azure Bastion, hebben uw virtuele machines geen openbaar IP nodig. Zie [Wat is Azure Bastion?](/bastion/bastion-overview.md)voor meer informatie.

![Azure Bastion-architectuur](./media/networking-overview/architecture.png)


## <a name="protect"></a>Services voor toepassings beveiliging

In deze sectie worden netwerk services in azure beschreven waarmee u uw netwerk bronnen kunt beveiligen DDoS Protection, Web Application firewall, Azure Firewall, netwerk beveiligings groepen en service-eind punten.

|Service|Waarom gebruiken?|Scenario|
|---|---|---|
|[DDoS-beveiliging](#ddosprotection) |Hoge Beschik baarheid voor uw toepassingen met beveiliging tegen overtollige IP-verkeer|[Azure DDoS Protection beheren](../virtual-network/manage-ddos-protection.md)|
|[Web Application firewall](#waf)|<p>[Azure WAF met Application Gateway](../application-gateway/waf-overview.md) biedt regionale beveiliging van entiteiten in open bare en privé-adres ruimte</p><p>[Azure WAF met de voor deur](../frontdoor/waf-overview.md) biedt beveiliging aan de rand van het netwerk tot open bare eind punten.</p>|<p>[Bot-beveiligings regels configureren](../frontdoor/waf-front-door-policy-configure-bot-protection.md)</p> <p>[Aangepaste antwoord code configureren](../frontdoor/waf-front-door-configure-custom-response-code.md)</p> <p>[IP-beperkings regels configureren](../frontdoor/waf-front-door-configure-ip-restriction.md)</p> <p>[Regel voor frequentie limiet configureren](../frontdoor/waf-front-door-rate-limit-powershell.md)</p> |
|[Azure Firewall](#firewall)|Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledig stateful firewall als een service met ingebouwde hoge Beschik baarheid en een onbeperkte schaal baarheid van de Cloud.|<p>[Een Azure Firewall implementeren in een Vnet](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[-Een Azure Firewall implementeren in een hybride netwerk](../firewall/tutorial-hybrid-ps.md)</p> <p>[Inkomend verkeer filteren met Azure Firewall DNAT](../firewall/tutorial-firewall-dnat.md)</p>|
|[Netwerkbeveiligingsgroepen](#nsg)|Volledig granulair gedistribueerd besturings element voor eind knooppunten op VM/subnet voor alle netwerk verkeer stromen|[Netwerk verkeer filteren met netwerk beveiligings groepen](../virtual-network/tutorial-filter-network-traffic.md)|
|[Service-eindpunten voor virtueel netwerk](#serviceendpoints)|Hiermee kunt u de netwerk toegang tot bepaalde Azure-service bronnen beperken tot een subnet van een virtueel netwerk|[Netwerktoegang tot PaaS-resources beperken](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
|||
### <a name="ddosprotection"></a>DDoS Protection 
[Azure DDoS Protection](../virtual-network/manage-ddos-protection.md) biedt tegen maatregelen tegen de meest geavanceerde DDoS bedreigingen. De service biedt verbeterde mogelijkheden voor DDoS-beperking voor uw toepassing en resources die zijn geïmplementeerd in uw virtuele netwerken. Klanten die Azure DDoS Protection gebruiken, hebben bovendien toegang tot DDoS snelle reactie ondersteuning om DDoS-experts te benaderen tijdens een actieve aanval.

![DDoS Protection](./media/networking-overview/ddos-protection.png)

### <a name="waf"></a>Web Application firewall

Azure Web Application firewall (WAF) biedt beveiliging voor uw webtoepassingen van veelvoorkomende webtoepassingen en zwakke plekken, zoals SQL-injectie en scripting op meerdere sites. Azure WAF voorziet in out-of-Box-beveiliging van OWASP Top 10 van beveiligings problemen via beheerde regels. Daarnaast kunnen klanten aangepaste regels configureren, die door de klant beheerde regels zijn om extra beveiliging te bieden op basis van het bron-IP-bereik, en om kenmerken zoals kopteksten, cookies, formulier gegevens velden of query reeks parameters op te geven.

Klanten kunnen ervoor kiezen om [Azure WAF te implementeren met Application Gateway](../application-gateway/waf-overview.md) die regionale beveiliging biedt aan entiteiten in open bare en privé-adres ruimte. Klanten kunnen er ook voor kiezen om [Azure WAF te implementeren met de voor deur](../frontdoor/waf-overview.md) die beveiliging biedt aan de rand van het netwerk tot open bare eind punten.

![Web Application Firewall](./media/networking-overview/waf-overview.png)


### <a name="firewall"></a>Azure Firewall
Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Met behulp van Azure Firewall kunt u toepassingen en beleids regels voor de toepassing en het netwerk op verschillende locaties en in virtuele netwerken centraal maken, afdwingen en registreren. Azure Firewall maakt gebruik van een statisch openbaar IP-adres voor uw virtuele-netwerkresources zodat externe firewalls verkeer dat afkomstig is van uw virtuele netwerk kunnen identificeren. 

Raadpleeg de [Azure firewall documentatie](../firewall/overview.md)voor meer informatie over Azure firewall.

![Firewalloverzicht](./media/networking-overview/firewall-threat.png)

### <a name="nsg"></a>Netwerk beveiligings groepen
U kunt netwerk verkeer van en naar Azure-resources in een virtueel Azure-netwerk filteren met een netwerk beveiligings groep. Zie [Security Overview](../virtual-network/security-overview.md)(Engelstalig) voor meer informatie.

### <a name="serviceendpoints"></a>Service-eind punten
Met service-eindpunten van Virtual Network (VNet) kunt u de privé-adresruimte van uw virtuele netwerk en de identiteit van uw VNet uitbreiden naar Azure-services, via een directe verbinding. Met eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar uw virtuele netwerken. Verkeer van uw VNet naar de Azure-service blijft altijd in het Microsoft Azure-backbonenetwerk. Zie [service-eind punten voor virtuele netwerken](../virtual-network/virtual-network-service-endpoints-overview.md)voor meer informatie.

![Service-eindpunten voor virtueel netwerk](./media/networking-overview/vnet-service-endpoints-overview.png)

## <a name="deliver"></a>Services voor het leveren van toepassingen

In deze sectie worden de netwerk services in azure beschreven die u helpen bij het leveren van toepassingen Content Delivery Network (CDN), de Azure front-deur service, Traffic Manager, Application Gateway en Load Balancer.

|Service|Waarom gebruiken?|Scenario|
|---|---|---|
|[Content Delivery Network](#cdn)|Levert inhoud met hoge band breedte aan gebruikers. Cdn's slaat inhoud in de cache op rand servers op in POP-locaties (Point-of-Presence) die zich dicht bij eind gebruikers bevinden, om latentie te minimaliseren|<p>[CDN toevoegen aan een web-app](../cdn/cdn-add-to-web-app.md)</p> <p>[-Toegang tot opslag-blobs met behulp van een Azure CDN aangepast domein via HTTPS](..//cdn/cdn-storage-custom-domain-https.md)</p> <p>[Een aangepast domein toevoegen aan uw Azure CDN-eind punt](../cdn/cdn-map-content-to-custom-domain.md)</p> <p>[HTTPS op een aangepast Azure CDN-domein configureren](../cdn/cdn-custom-ssl.md?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)</p>|
|[Azure front-deur service](#frontdoor)|Hiermee kunt u de wereld wijde route ring voor uw webverkeer definiëren, beheren en bewaken door te optimaliseren voor de beste prestaties en directe globale failover voor hoge Beschik baarheid.|<p>[Een aangepast domein toevoegen aan uw Azure front-deur service](../frontdoor/front-door-custom-domain.md)</p> <p>[HTTPS configureren op een aangepast domein voor de voor deur](../frontdoor/front-door-custom-domain-https.md)</p><p>[Beleid voor geofiltering Web Application firewall instellen](../frontdoor/front-door-tutorial-geo-filtering.md)|
|[Traffic Manager](#trafficmanager)|Distribueert verkeer op basis van DNS naar Services in wereld wijde Azure-regio's, en biedt hoge Beschik baarheid en reactie snelheid|<p> [Verkeer routeren voor lage latentie](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[Verkeer routeren naar een prioriteitseindpunt](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [Verkeer beheren met gewogen eindpunten](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[Verkeer routeren op basis van de geografische locatie van het eind punt](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [Verkeer routeren op basis van het subnet van de gebruiker](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[Load balancer](#loadbalancer)|Voorziet in regionale taak verdeling door verkeer te routeren tussen beschikbaarheids zones en in uw VNets. Voorziet in interne taak verdeling door verkeer tussen en tussen uw resources te routeren om uw regionale toepassing te bouwen.|<p> [Internetverkeer gelijkmatig verdelen over VM's](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[Taak verdeling van verkeer tussen virtuele machines in een virtueel netwerk](../load-balancer/tutorial-load-balancer-basic-internal-portal.md)<p>[Poort verkeer door sturen naar een specifieke poort op specifieke Vm's](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [Taak verdeling en uitgaande regels configureren](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[Application Gateway](#applicationgateway)|Azure Application Gateway is een load balancer voor webverkeer waarmee u het verkeer naar uw webapps kunt beheren.|<p>[Webverkeer omleiden met Azure-toepassing gateway](../application-gateway/quick-create-portal.md)</p><p>[Een toepassingsgateway configureren met SSL-beëindiging](../application-gateway/create-ssl-portal.md)</p><p>[Een toepassingsgateway maken met een omleiding op basis van een URL-pad](../application-gateway/create-url-route-portal.md) </p>|
|

### <a name="cdn"></a>Content Delivery Network
Azure Content Delivery Network (CDN) biedt ontwikkelaars een globale oplossing voor het snel leveren van inhoud met hoge bandbreedte door de inhoud op strategische, fysieke knooppunten in de hele wereld in de cache op te slaan. Zie voor meer informatie over Azure CDN [Azure Content Delivery Network](../cdn/cdn-overview.md)

![Azure CDN](./media/networking-overview/cdn-overview.png)

### <a name="frontdoor"></a>Azure front-deur service
Met de Azure Front Door Service kunt u de internationale routering van uw webverkeer definiëren, beheren en bewaken door te optimaliseren voor de beste prestaties en directe wereldwijde failover voor hoge beschikbaarheid. Met Front Door kunt u uw internationale (multiregionale) klant- en bedrijfstoepassingen transformeren in robuuste, hoogwaardige, gepersonaliseerde moderne toepassingen, API’s en inhoud die een wereldwijd bereik hebben met Azure. Zie [Azure front-deur](../frontdoor/front-door-overview.md)voor meer informatie.


### <a name="trafficmanager"></a>Traffic Manager

Azure Traffic Manager is een op DNS gebaseerde load balancer waarmee u verkeer optimaal over services kunt verdelen in Azure-regio's wereldwijd, terwijl u over hoge beschikbaarheid en een hoge reactiesnelheid beschikt. Traffic Manager biedt een reeks verkeers routerings methoden voor het distribueren van verkeer zoals prioriteit, gewicht, prestaties, geografisch, meerdere waarden of subnet. Zie [Traffic Manager routerings methoden](../traffic-manager/traffic-manager-routing-methods.md)voor meer informatie over routerings methoden voor verkeer.

In het volgende diagram ziet u route ring op basis van eind punten met Traffic Manager:

![Azure Traffic Manager ' Priority ' Traffic-routerings methode](./media/networking-overview/priority.png)

Zie [Wat is Azure Traffic Manager?](../traffic-manager/traffic-manager-overview.md) voor meer informatie over Traffic Manager.

### <a name="loadbalancer"></a>Load Balancer
De Azure Load Balancer biedt hoge prestaties en lage latentie taak verdeling laag 4 voor alle UDP-en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt open bare en interne eind punten met taak verdeling configureren. U kunt regels definiëren voor het toewijzen van binnenkomende verbindingen aan bestemmingen van back-endservers door gebruik te maken van TCP-en HTTP-beschikbaarheids opties voor het beheren van de beschik baarheid van de service. Lees het artikel [overzicht van Load Balancer](../load-balancer/load-balancer-overview.md) voor meer informatie over Load Balancer.

In de volgende afbeelding ziet u een toepassing met meerdere lagen op Internet die gebruikmaakt van zowel externe als interne load balancers:

![Azure Load Balancer-voor beeld](./media/networking-overview/IC744147.png)


### <a name="applicationgateway"></a>Application Gateway
Azure Application Gateway is een load balancer voor webverkeer waarmee u het verkeer naar uw webapps kunt beheren. Het is een ADC (Application Delivery controller) als een service met verschillende mogelijkheden voor taak verdeling voor laag 7 voor uw toepassingen. Zie [Wat is Azure-toepassing gateway?](../application-gateway/overview.md)voor meer informatie.

Het volgende diagram toont op URL-pad gebaseerde route ring met Application Gateway.

![Application Gateway-voor beeld](./media/networking-overview/figure1-720.png)

## <a name="monitor"></a>Netwerk bewakings Services
In deze sectie worden de netwerk services in azure beschreven die u helpen bij het bewaken van uw netwerk bronnen-Network Watcher, ExpressRoute monitor, Azure Monitor en Virtual Network tikken.

|Service|Waarom gebruiken?|Scenario|
|---|---|---|
|[Network Watcher](#networkwatcher)|Helpt bij het bewaken en oplossen van verbindings problemen, helpt bij het vaststellen van problemen met VPN-, NSG-en route ring, het vastleggen van pakketten op uw virtuele machine, het automatiseren van diagnostische hulpprogram ma's voor activering met Azure Functions en Logic Apps|<p>[Probleem met VM-verkeers filter vaststellen](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[Problemen met VM-route ring vaststellen](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[De communicatie tussen Vm's controleren](../network-watcher/connection-monitor.md)</p><p>[Communicatieproblemen tussen netwerken vaststellen](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[Netwerkverkeer van en naar een VM in een logboek vastleggen](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[Monitor voor ExpressRoute](#expressroutemonitor)|Biedt real-time bewaking van netwerk prestaties, Beschik baarheid en gebruik, helpt bij het automatisch detecteren van de netwerk topologie, biedt snellere fout isolatie, detecteert tijdelijke netwerk problemen, helpt historische netwerk prestaties te analyseren kenmerken, ondersteunt meerdere abonnementen|<p>[Netwerkprestatiemeter configureren voor ExpressRoute](../expressroute/how-to-npm.md)</p><p>[Bewaking, metrische gegevens en waarschuwingen ExpressRoute](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|[Azure Monitor](#azuremonitor)|Helpt u inzicht te krijgen in de manier waarop uw toepassingen worden uitgevoerd en proactief problemen identificeren die van invloed zijn op deze en de resources waarvan ze afhankelijk zijn.|<p>[Metrische gegevens en waarschuwingen Traffic Manager](../traffic-manager/traffic-manager-metrics-alerts.md)</p><p>[Diagnostische gegevens van Azure monitor voor Standard Load Balancer](../load-balancer/load-balancer-standard-diagnostics.md)</p><p>[Azure Firewall logboeken en metrische gegevens controleren](../firewall/tutorial-diagnostics.md)</p><p>[Controle en logboekregistratie van Azure Web Application Firewall](../frontdoor/waf-front-door-monitor.md)</p>|
|[Virtual Network Tik](#vnettap)|Biedt continue streaming van netwerk verkeer van de virtuele machine naar pakket verzamelaar, biedt oplossingen voor netwerk-en toepassings prestaties en hulpprogram ma's voor beveiligings analyse|[Een VNet-bron maken TAP](../virtual-network/tutorial-tap-virtual-network-cli.md)|
|

### <a name="networkwatcher"></a>Network Watcher
Azure Network Watcher biedt hulpprogramma's voor het controleren, diagnosticeren en weergeven van metrische gegevens en het in- of uitschakelen van logboekregistratie voor resources in een virtueel Azure-netwerk. Zie [Wat is Network Watcher?](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)voor meer informatie.
### <a name="expressroutemonitor"></a>Monitor voor ExpressRoute
Zie [ExpressRoute controle, metrische gegevens en waarschuwingen](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json)voor meer informatie over het weer geven van metrische gegevens voor ExpressRoute-circuits, Diagnostische logboeken en waarschuwingen.
### <a name="azuremonitor"></a>Azure Monitor
Azure Monitor maximaliseert de beschik baarheid en prestaties van uw toepassingen door een uitgebreide oplossing te bieden voor het verzamelen, analyseren en uitvoeren van telemetrie in uw Cloud-en on-premises omgevingen. Het helpt u begrijpen hoe uw toepassingen presteren en stelt proactief problemen vast die betrekking hebben op de toepassingen en de resources waarvan ze afhankelijk zijn. Zie [Azure monitor Overview](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json)voor meer informatie.
### <a name="vnettap"></a>Virtual Network Tik
Met het virtuele netwerk van Azure (Terminal Access Point) kunt u het netwerk verkeer van de virtuele machine continu streamen naar een netwerk pakket verzamelaar of een analyse programma. Het hulp programma Collector of Analytics wordt verschaft door een [virtuele netwerk apparaat](https://azure.microsoft.com/solutions/network-appliances/) -partner. 

In de volgende afbeelding ziet u hoe virtuele netwerk tikken werkt. 

![Hoe Virtual Network TIKT werkt](./media/networking-overview/virtual-network-tap-architecture.png)

Zie [Wat is Virtual Network Tik](../virtual-network/virtual-network-tap-overview.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Maak uw eerste VNet en Verbind een aantal virtuele machines met de virtuele machine door de stappen in het artikel [Create Your First Virtual Network](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) uit te voeren.
- Verbind uw computer met een VNet door de stappen in het [artikel een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)te volt ooien.
- Taak verdeling van Internet verkeer naar open bare servers door de stappen in het artikel [een Internet gerichte Load Balancer maken](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) uit te voeren.
 
 
   
