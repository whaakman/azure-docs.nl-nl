---
title: Azure-netwerken | Microsoft Docs
description: Meer informatie over Azure netwerkservices en mogelijkheden.
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: 7ed018c8c9759bc497c5fea129257486f6128531
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-networking"></a>Azure-netwerken

Azure biedt tal van netwerkmogelijkheden die samen of afzonderlijk kunnen worden gebruikt. Klik op een van de volgende belangrijke mogelijkheden voor meer informatie over deze:
- [Connectiviteit tussen Azure-resources](#connectivity): verbinding maken met Azure-resources samen in een beveiligd, persoonlijk virtueel netwerk in de cloud.
- [Verbinding met Internet](#internet-connectivity): en naar Azure-resources via Internet communiceren.
- [Lokale connectiviteit](#on-premises-connectivity): verbinding maken met een on-premises netwerk voor Azure-resources via een virtueel particulier netwerk (VPN) via Internet of via een speciale verbinding naar Azure.
- [Load balancing en verkeer richting](#load-balancing): Load balance verkeer naar servers in de dezelfde locatie en direct verkeer naar servers op verschillende locaties.
- [Beveiliging](#security): filteren van netwerkverkeer tussen subnetten of afzonderlijke virtuele machines (VM).
- [Routering](#routing): gebruik standaardroutering of volledig beheren routering tussen uw Azure- en on-premises resources.
- [Beheerbaarheid](#manageability): bewaken en beheren van uw Azure VPN-resources.
- [Hulpprogramma's voor implementatie en configuratie](#tools): gebruiken een webportal of netwerkbronnen platformoverschrijdende opdrachtregelprogramma's implementeren en configureren.

## <a name="Connectivity"></a>Connectiviteit tussen Azure-resources

Azure-resources, zoals virtuele Machines, Cloud Services-Schaalsets virtuele Machines en Azure App Service-omgevingen kunnen privé met elkaar communiceren via een virtueel Azure-netwerk (VNet). Een VNet is een logische isolatie van de Azure-cloud toegewezen aan uw [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). U kunt meerdere VNets binnen elk Azure-abonnement en Azure implementeren [regio](https://azure.microsoft.com/regions). Elke VNet is geïsoleerd van andere vnet's. U kunt voor elk VNet:

- Geef een aangepaste persoonlijke IP-adresruimte met behulp van openbare en persoonlijke (RFC 1918)-adressen. Resources in Azure wordt toegewezen en die is verbonden met het VNet een particulier IP-adres van de adresruimte die u toewijst.
- Het VNet segmenteren in een of meer subnetten en een deel van de VNet-adresruimte voor elk subnet toegewezen.
- Azure verschafte naamomzetting gebruiken of geef uw eigen DNS-server voor gebruik door resources die zijn verbonden met een VNet.

Lees voor meer informatie over de Azure Virtual Network service het [Virtual network-overzicht](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. Kunt u VNets met elkaar, het inschakelen van de resources die zijn verbonden met een VNet-naar-tussen VNets met elkaar communiceren. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** kunnen resources die zijn verbonden met andere Azure VNets binnen dezelfde Azure-regio met elkaar communiceren. De bandbreedte en de latentie tussen de VNets is hetzelfde als wanneer de resources zijn verbonden met hetzelfde VNet is. Lees voor meer informatie over de peering, de [peering Virtual network-overzicht](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **VPN-Gateway:** kunnen resources met andere Azure VNets in verschillende Azure-regio's om te communiceren met elkaar verbonden. Verkeer tussen VNets loopt via een Azure VPN-Gateway. Bandbreedte tussen VNets is beperkt tot de bandbreedte van de gateway. Lees meer informatie over het VNets verbinden met een VPN-Gateway, de [configureren van een VNet-naar-VNet-verbinding tussen regio's](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="internet-connectivity"></a>Verbinding met Internet

Uitgaande verbinding met Internet hebben alle Azure-resources is verbonden met een VNet standaard. Het privé IP-adres van de resource is Bronnetwerk adres vertaald (snat omzetten) naar een openbare IP-adres door de Azure-infrastructuur. Lees voor meer informatie over uitgaande internetverbinding, de [inzicht in uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

Inkomende communicatie voor Azure-resources van het Internet of om te communiceren met Internet, zonder snat omzetten uitgaand, moet een resource een openbaar IP-adres worden toegewezen. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="on-premises-connectivity"></a>Lokale connectiviteit

U kunt toegang tot bronnen in uw VNet veilig via een VPN-verbinding of een directe particuliere verbinding. Voor het verzenden van netwerkverkeer tussen uw virtuele Azure-netwerk en uw on-premises netwerk, moet u een virtuele netwerkgateway maken. Configureren van instellingen voor de gateway voor het maken van het type verbinding die u wilt, VPN- of ExpressRoute.

U kunt uw on-premises netwerk verbinden met een VNet met elke combinatie van de volgende opties:

**Punt-naar-site (VPN via SSTP)**

De volgende afbeelding ziet afzonderlijke punt op siteverbindingen tussen meerdere computers en een VNet:

![Punt-naar-site](./media/networking-overview/point-to-site.png)

Deze verbinding tot stand is gebracht tussen één computer en een VNet. Dit verbindingstype is handig als u alleen aan de slag met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk. Het is ook handig wanneer u verbinding vanaf een externe locatie, zoals een vergaderruimte maakt of thuis. Punt-naar-site-verbindingen zijn vaak samen met een site-naar-site-verbinding via de virtuele netwerkgateway. De verbinding wordt de SSTP-protocol gebruikt voor gecodeerde communicatie via Internet tussen de computer en het VNet. De latentie voor een punt-naar-site VPN-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.

**Site-naar-site (IPsec/IKE-VPN-tunnel)**

![Site-naar-site](./media/networking-overview/site-to-site.png)

Deze verbinding tot stand is gebracht tussen uw on-premises VPN-apparaat en een Azure VPN-Gateway. Dit verbindingstype kunt een on-premises resource die u toestemming voor toegang tot het VNet. De verbinding is een VPN IPSec/IKE waarmee gecodeerde communicatie via Internet tussen uw on-premises-apparaat en de Azure VPN-gateway. U kunt meerdere on-premises sites kunt verbinden met de dezelfde VPN-gateway. De on-premises VPN-apparaat op elke site moet een extern gericht openbaar IP-adres die zich niet achter een NAT bevinden. De latentie voor een site-naar-site-verbinding is onvoorspelbaar zijn, aangezien het verkeer over het Internet wordt verzonden.

**ExpressRoute (speciale persoonlijke verbinding)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Dit type verbinding tot stand is gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer niet via Internet verloopt. De latentie voor een ExpressRoute-verbinding is betrouwbaar, aangezien het verkeer niet via Internet verloopt. ExpressRoute kan worden gecombineerd met een site-naar-site-verbinding.

Lees voor meer informatie over de vorige verbindingsopties, de [gatewayverbindingsdiagrammen topologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="load-balancing"></a>Load balancing en verkeer richting

Microsoft Azure biedt meerdere services voor het beheren van het netwerkverkeer wordt gedistribueerd en taakverdeling. U kunt een van de volgende mogelijkheden afzonderlijk of samen gebruiken:

**DNS taakverdeling**

De service Azure Traffic Manager biedt globale DNS taakverdeling. Traffic Manager reageert op clients met het IP-adres van een gezonde-eindpunt op basis van een van de volgende methoden:
- **Geografisch:** Clients worden omgeleid naar specifieke eindpunten (Azure, externe of geneste) op basis van welke geografische locatie hun DNS-query is afkomstig uit. Deze methode kunt scenario's waar dat bekend is van een client geografische regio en routering toe op basis van het is belangrijk. Voorbeelden zijn die voldoet aan gegevens onafhankelijkheid vereist, lokalisatie van inhoud & gebruiker ervaring, en het meten van verkeer vanuit verschillende regio's.
- **Prestaties:** het IP-adres geretourneerd naar de client is de 'dichtstbijzijnde' naar de client. Het eindpunt 'dichtstbijzijnde' is niet noodzakelijkerwijs dichtstbijzijnde door geografische afstand wordt gemeten. Deze methode wordt in plaats daarvan het dichtstbijzijnde eindpunt bepaalt door te meten netwerklatentie. Traffic Manager onderhoudt een tabel van de latentie Internet om bij te houden de round trip-tijd tussen het IP-adresbereiken en elke Azure-datacenter.
- **Prioriteit:** verkeer wordt omgeleid naar de primaire (hoogste prioriteit)-eindpunt. Als het primaire eindpunt niet beschikbaar is, wordt in Traffic Manager het verkeer naar het tweede eindpunt gestuurd. Als de primaire en secundaire eindpunten niet beschikbaar zijn, wordt het verkeer gaat naar de andere, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de lopende eindpuntcontrole.
- **Gewogen round robin:** voor elke aanvraag kiest Traffic Manager willekeurig een beschikbare eindpunt. De kans van de keuze van een eindpunt is gebaseerd op het gewicht is toegewezen aan alle beschikbare eindpunten. Met behulp van hetzelfde gewicht over alle eindpunten resulteert in een zelfs verkeer distributie. Met behulp van hogere of lagere gewichten op specifieke eindpunten zorgt ervoor dat deze eindpunten moet meer of minder vaak worden geretourneerd in de DNS-antwoorden.

De volgende afbeelding ziet een aanvraag voor een webtoepassing omgeleid naar een Web-App-eindpunt. Eindpunten kunnen ook worden andere Azure-services zoals virtuele machines en Cloudservices.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

De client verbinding rechtstreeks naar dat eindpunt. Azure Traffic Manager detecteert wanneer een eindpunt niet in orde is en vervolgens clients naar een andere, in orde-eindpunt omleidt. Lees meer informatie over het Traffic Manager, de [overzicht van Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

**Taakverdeling voor toepassingen**

De service Azure Application Gateway biedt toepassing levering controller (ADC) als een service. Toepassingsgateway biedt verschillende laag 7 (HTTP/HTTPS) taakverdeling voor uw toepassingen, met inbegrip van web application firewall om te voorkomen dat uw webtoepassingen beveiligingsproblemen en aanvallen. Toepassingsgateway kunt u web-farm productiviteit optimaliseren door het offloaden van CPU-intensief SSL-beëindiging aan de toepassingsgateway. 

Andere mogelijkheden voor het doorsturen van laag 7 bevatten round-robin distributie van inkomend verkeer, sessie cookies gebaseerde affiniteit URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter een gateway één toepassing. Application Gateway kan worden geconfigureerd als een gateway internetgerichte, een gateway interne alleen-lezen of een combinatie van beide. Toepassingsgateway is volledig Azure beheerde, schaalbare en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en functies voor logboekregistratie voor betere beheersbaarheid. Lees voor meer informatie over Application Gateway, de [Application Gateway overzicht](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

De volgende afbeelding ziet URL-pad gebaseerde routering met Application Gateway:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Taakverdeling voor netwerken**

De Azure Load Balancer biedt hoge prestaties, lage latentie laag 4-taakverdeling voor alle UDP en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt de openbare en interne eindpunten voor netwerktaakverdeling configureren. U kunt regels voor binnenkomende verbindingen worden toegewezen aan de doelen van de back-end-pool met behulp van TCP- en HTTP-status probing opties voor het beheren van de beschikbaarheid van de service kunt definiëren. Lees voor meer informatie over de Load Balancer, het [overzicht van de Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

De volgende afbeelding ziet een internetgerichte toepassing met meerdere lagen die gebruikmaakt van zowel externe en interne load balancers:

![Load balancer](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Beveiliging

U kunt filteren verkeer van en naar Azure-resources met de volgende opties:

- **Netwerk:** u Azure netwerkbeveiligingsgroepen (nsg's) voor het filteren van binnenkomend en uitgaand verkeer voor Azure-resources kunt implementeren. Elke NSG bevat een of meer regels voor binnenkomend en uitgaand. Elke regel geeft u aan de bron-IP-adressen, IP-doeladressen, poort en protocol dat verkeer wordt gefilterd met. Nsg's kunnen worden toegepast op afzonderlijke subnetten en afzonderlijke virtuele machines. Lees voor meer informatie over Nsg de [netwerk beveiligingsgroepen overzicht](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Toepassing:** met behulp van een toepassingsgateway met web application firewall kunt u uw webtoepassingen beschermen tegen beveiligingsproblemen en aanvallen. Algemene voorbeelden zijn SQL-injectieaanvallen, cross-site scripting en een verkeerd ingedeelde kopteksten. Toepassingsgateway dit verkeer gefilterd en voorkomt dat uw webservers is bereikt. U zijn kunt configureren welke regels die u ingeschakeld wilt. De mogelijkheid voor het configureren van SSL-onderhandelingsbeleid is opgegeven voor het toestaan van bepaalde beleidsregels worden uitgeschakeld. Lees voor meer informatie over de web application firewall de [Web application firewall](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

Als u moet de mogelijkheid via het netwerk Azure niet opgeven of wilt gebruiken, netwerktoepassingen die u lokaal gebruikt, kunt u de producten in virtuele machines implementeren en verbind ze met uw VNet. De [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) bevat verschillende andere virtuele machines die vooraf geconfigureerd met de netwerktoepassingen die u kunt. Deze vooraf geconfigureerde virtuele machines worden gewoonlijk aangeduid als virtuele netwerkapparaten (NVA). NVAs zijn beschikbaar met toepassingen zoals firewall en WAN-optimalisatie.

## <a name="routing"></a>Routering

Azure maakt standaard routetabellen waarmee bronnen die zijn verbonden met een enkel subnet in een VNet met elkaar communiceren. U kunt een of beide van de volgende soorten routes voor het onderdrukken van de Azure maakt standaardroutes implementeren:
- **De gebruiker gedefinieerde:** kunt u aangepaste routetabellen met routes die bepalen waar verkeer wordt doorgestuurd naar voor elk subnet. Lees het artikel [User-defined routes](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.
- **Border gateway protocol (BGP):** als u uw VNet verbinding met uw on-premises netwerk via een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw vnet's. BGP is het standaardprotocol voor routering dat doorgaans op internet wordt gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens tussen twee of meer netwerken. Wanneer gebruikt in de context van Azure Virtual Networks, BGP Hiermee de Azure VPN-Gateways en uw on-premises VPN-apparaten, aangeroepen BGP-peers of neighbors, 'routes' uitwisselen die beide gateways over de beschikbaarheid en bereikbaarheid voor deze voorvoegsels informeren zodat ze via de gateways of routers betrokken. BGP ook transitroutering tussen meerdere netwerken door routes die een BGP-gateway van één BGP te propageren naar alle andere BGP-peers leert. Zie voor meer informatie over BGP, de [BGP met Azure VPN-Gateways overzicht](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="manageability"></a>Beheerbaarheid

Azure biedt de volgende hulpprogramma's om te controleren en beheren van netwerken:
- **Activiteitenlogboeken:** alle Azure-resources zijn activiteitenlogboeken die informatie over plaatsgevonden, status van de operations-bewerkingen bieden en wie de bewerking gestart. Lees voor meer informatie over activiteitenlogboeken, de [activiteit Logboeken overzicht](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Diagnostische logboeken:** periodiek en eigen initiatief gebeurtenissen zijn gemaakt door netwerkbronnen en geregistreerd in Azure storage-accounts, verzonden naar een Azure Event Hub of verzonden naar Azure-logboekanalyse. Diagnostische logboeken bieden inzicht in de status van een resource. Diagnostische logboeken zijn beschikbaar voor de Load Balancer (internetgerichte), netwerk-beveiligingsgroepen, routes en Application Gateway. Lees voor meer informatie over diagnostische logboeken, het [diagnostische logboeken overzicht](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Metrische gegevens:** metrische gegevens zijn metingen van de prestaties en prestatiemeteritems die gedurende een periode van bronnen worden verzameld. Metrische gegevens kan worden gebruikt voor het activeren van waarschuwingen op basis van drempelwaarden. Metrische gegevens zijn momenteel beschikbaar op Application Gateway. Lees voor meer informatie over metrische gegevens, de [overzicht van metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Problemen oplossen:** informatie over probleemoplossing is toegankelijk rechtstreeks in de Azure-portal. De informatie helpt bij het analyseren van algemene problemen met ExpressRoute, VPN-Gateway, Application Gateway, netwerk-beveiligingslogboeken, Routes, DNS, Load Balancer en Traffic Manager.
- **Op rollen gebaseerde toegangsbeheer (RBAC):** bepalen wie kunt maken en beheren van netwerkresources met op rollen gebaseerde toegangsbeheer (RBAC). Meer informatie over RBAC door het lezen van de [aan de slag met RBAC](../active-directory/role-based-access-control-what-is.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. 
- **Pakketopname:** de Azure-netwerk-Watcher-service biedt de mogelijkheid om uit te voeren van een pakketopname op een virtuele machine via een uitbreiding in de VM. Deze functie is beschikbaar voor Linux en Windows-VM's. Lees voor meer informatie over pakketopname de [pakket vastleggen overzicht](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Controleer of IP-stromen:** netwerk-Watcher kunt u controleren of de IP-stromen tussen een virtuele machine van Azure en een externe bron om te bepalen of er pakketten worden toegestaan of geweigerd. Deze mogelijkheid biedt beheerders de mogelijkheid om op te sporen snel problemen met de netwerkverbinding. Lees meer informatie over het controleren van de IP-stromen, de [IP-stroom controleren overzicht](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **VPN-connectiviteit oplossen:** de VPN-probleemoplosser mogelijkheid van netwerk-Watcher biedt de mogelijkheid om een verbinding of de gateway query en controleer of de status van de resources. Lees meer informatie over het oplossen van VPN-verbindingen, de [VPN-verbinding overzicht probleemoplossing](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Netwerktopologie weergeven:** een grafische weergave van de netwerkbronnen in een VNet met de netwerk-Watcher. Lees meer informatie over het weergeven van de netwerktopologie, de [topologie overzicht](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="tools"></a>Hulpprogramma's voor implementatie en configuratie

U kunt implementeren en configureren van netwerken Azure-resources met een van de volgende hulpprogramma's:

- **Azure-portal:** een grafische gebruikersinterface die wordt uitgevoerd in een browser. Open de [Azure Portal](http://portal.azure.com).
- **Azure PowerShell:** opdrachtregelprogramma's voor het beheren van Azure op Windows-computers. Meer informatie over Azure PowerShell door het lezen van de [overzicht van Azure PowerShell](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure-opdrachtregelinterface (CLI):** opdrachtregelprogramma's voor het beheren van Azure van Linux-, Mac OS- of Windows-computers. Meer informatie over de Azure CLI door het lezen van de [overzicht van Azure CLI](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure Resource Manager-sjablonen:** een bestand (in JSON-indeling) waarin de infrastructuur en configuratie van een Azure-oplossing. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Lees meer informatie over het ontwerpen van sjablonen, de [aanbevolen procedures voor het maken van sjablonen](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. Sjablonen kunnen worden geïmplementeerd met de Azure-portal, CLI of PowerShell. Om te beginnen met sjablonen meteen, implementeert u een van de vele vooraf geconfigureerde sjablonen in de [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=network) bibliotheek. 

## <a name="pricing"></a>Prijzen

Sommige van de Azure VPN-services hebben een kosten, terwijl andere gratis. Weergave de [virtueel netwerk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), [Application Gateway](https://azure.microsoft.com/en-us/pricing/details/application-gateway/), [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer), [netwerk-Watcher](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) en [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) prijzen van pagina's voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Maken van uw eerste VNet en verbinding maken met een paar virtuele machines, via de stappen in de [maken van uw eerste virtuele netwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- De computer met een VNet verbinden via de stappen in de [een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- Taakverdeling maken voor Internet-verkeer op openbare servers via de stappen in de [een internetgerichte load balancer maken](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
