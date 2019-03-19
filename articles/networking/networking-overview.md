---
title: Azure-netwerken | Microsoft Docs
description: Meer informatie over Azure VPN-services en mogelijkheden.
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: c37e2357038e4f711b015fda80a0de8c4dbd9498
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880922"
---
# <a name="azure-networking"></a>Azure-netwerken

Azure biedt een aantal netwerkmogelijkheden die kunnen worden gebruikt samen of afzonderlijk. Klik op een van de volgende belangrijke mogelijkheden voor meer informatie over deze:
- [Connectiviteit tussen Azure-resources](#connectivity): Verbinding maken met Azure-resources samen in een beveiligde, persoonlijke virtueel netwerk in de cloud.
- [Verbinding met Internet](#internet-connectivity): Naar en van Azure-resources communiceren via Internet.
- [On-premises connectiviteit](#on-premises-connectivity): Een on-premises netwerk verbinden met Azure-resources via een virtueel particulier netwerk (VPN) via Internet of via een speciale verbinding naar Azure.
- [Laden van taakverdeling en verkeersbeheer richting](#load-balancing): Verkeer met gelijke taakverdeling met servers in de dezelfde locatie en het verkeer naar servers op verschillende locaties.
- [Beveiliging](#security): Netwerkverkeer filteren tussen subnetten of afzonderlijke virtuele machines (VM).
- [Routering](#routing): Gebruik standaardroutering of volledige controle over routering tussen uw on-premises en Azure-resources.
- [Beheerbaarheid](#manageability): Controleren en beheren van uw Azure-netwerkbronnen.
- [Hulpprogramma's voor implementatie en configuratie](#tools): Een webportal of platformonafhankelijke opdrachtregelprogramma's gebruiken om te implementeren en configureren van netwerkbronnen.

## <a name="Connectivity"></a>Connectiviteit tussen Azure-resources

Azure-resources zoals virtuele Machines, Cloud Services, Schaalsets van virtuele Machines en Azure App Service-omgevingen kunnen privé met elkaar communiceren via een Azure Virtual Network (VNet). Een VNet is een logische isolatie van de Azure-cloud toegewezen aan uw [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). U kunt meerdere VNets binnen elk Azure-abonnement en Azure implementeren [regio](https://azure.microsoft.com/regions). Elk VNet is geïsoleerd van andere vnet's. Voor elke VNet kunt u het volgende doen:

- Een aangepaste persoonlijke IP-adresruimte opgeven met behulp van openbare en persoonlijke adressen (RFC 1918). Azure wijst resources en die is verbonden met het VNet een privé IP-adres van de adresruimte die u toewijst.
- Het VNet segmenteren in een of meer subnetten en een gedeelte van de VNet-adresruimte aan elk subnet toewijzen.
- Azure geleverd naamomzetting gebruiken of uw eigen DNS-server voor gebruik door resources die zijn verbonden met een VNet opgeven.

Lees voor meer informatie over de Azure Virtual Network-service de [overzicht van Virtual network](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. U kunt VNets verbinden met elkaar, zodat resources die zijn verbonden met een VNet om te communiceren met elkaar via vnet's. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** Hiermee kunt resources die zijn verbonden met verschillende Azure VNets in dezelfde Azure-regio om te communiceren met elkaar. De bandbreedte en de latentie tussen de VNets is hetzelfde als wanneer de resources die zijn verbonden met hetzelfde VNet is. Lees voor meer informatie over peering, de [overzicht van Virtual network-peering](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **VPN-Gateway:** Hiermee kunt resources die zijn verbonden met verschillende Azure VNets in verschillende Azure-regio's om te communiceren met elkaar. Verkeer tussen VNets stroomt via een Azure VPN-Gateway. Bandbreedte tussen vnet's is beperkt tot de bandbreedte van de gateway. Lees voor meer informatie over het verbinden van vnet's met een VPN-Gateway, de [een VNet-naar-VNet-verbinding configureren tussen regio's](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="internet-connectivity"></a>Verbinding met Internet

Alle Azure-resources die zijn verbonden met een VNet hebben uitgaande verbinding met Internet standaard. De privé IP-adres van de resource is netwerk bronadres vertaald (SNAT) naar een openbaar IP-adres door de Azure-infrastructuur. Lees voor meer informatie over uitgaande verbinding met Internet heeft, de [uitleg over uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

Inkomend communiceren met Azure-resources vanaf het Internet of om te communiceren met Internet, zonder SNAT uitgaande, moet een openbaar IP-adres toegewezen aan een resource is geselecteerd. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="on-premises-connectivity"></a>On-premises connectiviteit

U kunt veilig toegang tot resources in uw VNet via een VPN-verbinding of een rechtstreekse privéverbinding. Voor het verzenden van netwerkverkeer tussen uw Azure-netwerk en uw on-premises netwerk, moet u een virtuele netwerkgateway maken. Configureren van instellingen voor de gateway voor het maken van het type verbinding dat u wilt, VPN of ExpressRoute.

U kunt uw on-premises netwerk verbinden met een VNet maken via een combinatie van de volgende opties:

**Punt-naar-site (VPN via SSTP)**

De volgende afbeelding ziet u afzonderlijke punt voor site-verbindingen tussen meerdere computers en een VNet:

![Punt-naar-site](./media/networking-overview/point-to-site.png)

Deze verbinding tot stand is gebracht tussen één computer en een VNet. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. Het is ook handig als u verbinding vanaf een externe locatie, zoals een conferentie maakt of thuis. Punt-naar-site-verbindingen zijn vaak gekoppeld aan een site-naar-site-verbinding via de virtuele netwerkgateway. De verbinding gebruikt de SSTP-protocol voor versleutelde communicatie via Internet tussen de computer en het VNet. De latentie voor een punt-naar-site-VPN is onvoorspelbaar zijn, omdat het verkeer gaat via Internet.

**Site-naar-site (IPsec/IKE VPN-tunnel)**

![Site-naar-site](./media/networking-overview/site-to-site.png)

Deze verbinding tot stand is gebracht tussen uw on-premises VPN-apparaat en een Azure VPN-Gateway. Dit verbindingstype krijgen alle on-premises resource die u toestemming geeft toegang tot het VNet. De verbinding is een IPSec/IKE VPN waarmee de versleutelde communicatie via Internet mogelijk tussen uw on-premises apparaat en de Azure VPN-gateway. U kunt meerdere on-premises sites verbinden met dezelfde VPN-gateway. De on-premises VPN-apparaat op elke site moet een extern gericht openbaar IP-adres dat zich niet achter een NAT bevinden. De latentie voor een site-naar-site-verbinding is onvoorspelbaar zijn, omdat het verkeer gaat via Internet.

**ExpressRoute (exclusieve privéverbinding)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Dit type verbinding tot stand is gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer loopt niet via Internet. De latentie voor een ExpressRoute-verbinding is betrouwbaar, omdat het verkeer niet via Internet. ExpressRoute kan worden gecombineerd met een site-naar-site-verbinding.

Lees voor meer informatie over de vorige verbindingsopties de [verbindingstopologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="load-balancing"></a>Taakverdeling en verkeersbeheer richting laden

Microsoft Azure biedt verschillende services voor het beheren van hoe netwerkverkeer wordt verdeeld en taakverdeling. U kunt een van de volgende mogelijkheden afzonderlijk of samen gebruiken:

**DNS wordt de taakverdeling**

De service Azure Traffic Manager biedt globale DNS wordt de taakverdeling. Traffic Manager reageert op clients met het IP-adres van een gezond eindpunt, op basis van een van de volgende methoden:
- **Geografische:** Clients worden omgeleid naar specifieke eindpunten (Azure, externe of geneste) op basis van op welke geografische locatie hun DNS-query is afkomstig uit. Deze methode kunt scenario's waarbij wetenschap dat de geografische regio van een client en routering toe op basis van het is belangrijk. Voorbeelden zijn die voldoet aan gegevens soevereiniteit mandaten, lokalisatie van content & gebruiker ervaring, en het verkeer van de verschillende regio's meten.
- **Prestaties:** Het IP-adres dat is geretourneerd naar de client is de 'dichtstbijzijnde' naar de client. Het eindpunt van de 'dichtstbijzijnde' is niet noodzakelijkerwijs dichtstbijzijnde wordt gemeten door geografische afstand. Deze methode bepaalt in plaats daarvan het eindpunt van de dichtstbijzijnde netwerklatentie meten. Traffic Manager onderhoudt een tabel Internet latentie voor het volgen van de retourtijd tussen IP-adresbereiken en elke Azure-datacenter.
- **Prioriteit:** Verkeer wordt omgeleid naar het primaire eindpunt van de (hoogste prioriteit). Als het primaire eindpunt niet beschikbaar is, stuurt Traffic Manager het verkeer naar het tweede eindpunt. Als de primaire en secundaire eindpunten niet beschikbaar zijn, wordt het verkeer wordt gerouteerd naar de derde, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de voortdurende eindpuntbewaking.
- **Gewogen round robin:** Voor elke aanvraag, wordt geen beschikbaar eindpunt willekeurig gekozen door Traffic Manager. De kans van het kiezen van een eindpunt is gebaseerd op het gewicht dat is toegewezen aan alle beschikbare eindpunten. Met behulp van hetzelfde gewicht over alle eindpunten resultaten in een zelfs verkeersdistributie. Met behulp van hogere of lagere gewichten op de specifieke eindpunten zorgt ervoor dat deze eindpunten moet meer of minder vaak worden geretourneerd in de DNS-antwoorden.

De volgende afbeelding ziet u een aanvraag voor een web-App omgeleid naar een Web-App-eindpunt. Eindpunten kunnen ook worden andere Azure-services zoals virtuele machines en Cloud Services.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

De client maakt rechtstreeks verbinding met dit eindpunt. Met Azure Traffic Manager detecteert wanneer een eindpunt niet in orde is en vervolgens clients omgeleid naar een ander, gezond eindpunt. Lees voor meer informatie over Traffic Manager, de [overzicht van Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

**Taakverdeling voor toepassingen**

De service Azure Application Gateway biedt application delivery controller (ADC) als een service. Application Gateway biedt verschillende Layer 7 (HTTP/HTTPS) load balancing mogelijkheden voor uw toepassingen, met inbegrip van een web application firewall ter bescherming van uw webtoepassingen tegen beveiligingsproblemen. Application Gateway kunt u productiviteit van webfarms optimaliseren door het offloaden van CPU-intensieve SSL-beëindiging aan de toepassingsgateway. 

Andere Routeringsmogelijkheden voor laag 7 bevatten round-robin-distributie van inkomend verkeer, cookies gebaseerde sessieaffiniteit, URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter één application gateway. Application Gateway kan worden geconfigureerd als een Internet gerichte gateway, een gateway alleen interne of een combinatie van beide. Application Gateway wordt volledig door Azure beheerde, schaalbare en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en functies voor logboekregistratie voor betere beheersbaarheid. Lees voor meer informatie over Application Gateway, de [overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

De volgende afbeelding ziet de URL-pad gebaseerde routering met Application Gateway:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Taakverdeling voor netwerken**

De Azure Load Balancer biedt hoge prestaties en lage latentie laag-4 taakverdeling voor alle UDP en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt openbare en interne taakverdeling eindpunten configureren. U kunt regels voor binnenkomende verbindingen naar back-end-pool bestemmingen toewijzen met behulp van TCP- en HTTP-statustesten opties voor het beheren van de beschikbaarheid van de service definiëren. Lees voor meer informatie over Load Balancer de [overzicht van Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

De volgende afbeelding ziet u een internetgerichte toepassing met meerdere lagen die gebruikmaakt van beide externe en interne load balancers:

![Load balancer](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Beveiliging

U kunt filteren, verkeer van en naar Azure-resources met de volgende opties:

- **Netwerk:** U kunt Azure netwerkbeveiligingsgroepen (nsg's) voor het filteren van binnenkomende en uitgaande verkeer naar Azure-resources kunt implementeren. Elke NSG bevat een of meer regels voor binnenkomend en uitgaand. Elke regel geeft de bron-IP-adressen, doel-IP-adressen, poort en protocol dat verkeer wordt gefilterd met. Nsg's kunnen worden toegepast op afzonderlijke subnetten en afzonderlijke virtuele machines. Lees voor meer informatie over nsg's de [overzicht van netwerkbeveiligingsgroepen](../virtual-network/security-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Toepassing:** U kunt uw webtoepassingen beveiligen tegen beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall. Algemene voorbeelden zijn SQL-injectieaanvallen scripting op meerdere sites en onjuist gevormde headers. Application gateway dit verkeer gefilterd en voorkomt dat het bereiken van uw webservers. U zijn kunt configureren welke regels die u wilt dat ingeschakeld. De mogelijkheid om te configureren van SSL-Onderhandelingsbeleidsregels is opgegeven waarmee bepaalde beleidsregels worden uitgeschakeld. Lees voor meer informatie over de web application firewall, de [Web application firewall](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

Als u nodig hebt voor netwerkmogelijkheden Azure niet bieden, of wilt u on-premises gebruiken netwerktoepassingen gebruiken, kunt u de producten implementeren in virtuele machines en verbind deze met uw VNet. De [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) bevat diverse verschillende virtuele machines die vooraf is geconfigureerd met netwerktoepassingen u momenteel mag gebruiken. Deze vooraf geconfigureerde VM's worden gewoonlijk aangeduid als virtuele netwerkapparaten (NVA). NVA's zijn beschikbaar met toepassingen zoals firewalls en WAN-optimalisatie.

## <a name="routing"></a>Routing

Azure maakt standaard routetabellen waarmee de resources die zijn verbonden met een subnet in elk VNet om te communiceren met elkaar. U kunt een of beide van de volgende typen van routes voor de onderdrukking van de maakt Azure standaardroutes implementeren:
- **De gebruiker gedefinieerde:** U kunt aangepaste routetabellen maken met routes die bepalen waar verkeer naartoe wordt doorgestuurd voor elk subnet. Lees het artikel [User-defined routes](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.
- **Border gateway protocol (BGP):** Als u uw VNet verbinden met uw on-premises netwerk met behulp van een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw vnet's. BGP is het standaardprotocol voor routering dat doorgaans op internet wordt gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens tussen twee of meer netwerken. Wanneer gebruikt in de context van Azure Virtual Networks, kan BGP de Azure VPN-Gateways en uw on-premises VPN-apparaten, aangeroepen BGP-peers of neighbors, 'routes' uitwisselen die beide gateways over de beschikbaarheid en bereikbaarheid voor deze voorvoegsels informeren zodat ze via de gateways of routers die betrokken zijn. BGP kan ook transitroutering tussen meerdere netwerken doorgeven van routes die een BGP-gateway van één BGP-peer voor alle andere BGP-peers leert. Zie voor meer informatie over BGP, de [BGP met Azure VPN-Gateways overzicht](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="manageability"></a>Beheerbaarheid

Azure biedt de volgende hulpprogramma's om te controleren en beheren van netwerken:
- **Activiteitenlogboeken:** Alle Azure-resources hebben activiteitenlogboeken Hier vindt u informatie over bewerkingen hebben plaatsgevonden, status van bewerkingen en wie de bewerking heeft gestart. Lees voor meer informatie over activiteitenlogboeken, de [activiteitenlogboeken overzicht](../azure-monitor/platform/activity-logs-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Logboeken met diagnostische gegevens:** Periodieke en spontaan gebeurtenissen worden gemaakt met netwerkbronnen en geregistreerd in Azure storage-accounts, verzonden naar een Azure Event Hub of verzonden naar Azure Monitor-Logboeken. Diagnoselogboeken bieden inzicht in de status van een resource. Logboeken met diagnostische gegevens zijn opgegeven voor de Load Balancer (internetgerichte), Network Security Groups, routes en Application Gateway. Lees voor meer informatie over de logboeken met diagnostische gegevens, de [diagnostische logboeken overzicht](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Metrische gegevens:** Metrische gegevens zijn metingen van de prestaties en de tellers die zijn verzameld gedurende een bepaalde periode op resources. Metrische gegevens kan worden gebruikt voor het activeren van waarschuwingen op basis van drempelwaarden. Metrische gegevens zijn momenteel beschikbaar in Application Gateway. Lees voor meer informatie over metrische gegevens, de [overzicht van metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Problemen oplossen:** Informatie over probleemoplossing is toegankelijk rechtstreeks in Azure portal. De informatie helpt bij het vaststellen van veelvoorkomende problemen met ExpressRoute, VPN-Gateway, Application Gateway, netwerk-Logboeken, Routes, DNS, Load Balancer en Traffic Manager.
- **Op rollen gebaseerd toegangsbeheer (RBAC):** Bepalen wie kunt maken en beheren van netwerkresources met op rollen gebaseerd toegangsbeheer (RBAC). Meer informatie over RBAC vindt de [aan de slag met RBAC](../role-based-access-control/overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. 
- **Pakketopname:** De service Azure Network Watcher biedt de mogelijkheid om uit te voeren van een pakketopname op een virtuele machine via een extensie in de virtuele machine. Deze mogelijkheid is beschikbaar voor Linux- en Windows-machines. Lees voor meer informatie over de pakketopname de [overzicht van Pakketopname](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Controleer of IP-stromen:** Network Watcher kunt u controleren of IP-stromen tussen een Azure-VM en een externe bron om te bepalen of er pakketten worden toegestaan of geweigerd. Deze mogelijkheid biedt beheerders de mogelijkheid om snel verbindingsproblemen vast te stellen. Lees voor meer informatie over het IP-stromen controleren de [IP-stroom controleren overzicht](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Problemen met VPN-verbinding oplossen:** De mogelijkheid van de probleemoplosser voor VPN van Network Watcher biedt de mogelijkheid om te vragen van een verbinding of de gateway en controleer of de status van de resources. Lees voor meer informatie over het oplossen van VPN-verbindingen, de [VPN-verbinding oplossen overzicht](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Topologie van netwerk weergeven:** Een grafische weergave van de netwerkbronnen in een VNet met Network Watcher. Lees voor meer informatie over het weergeven van de netwerktopologie, de [overzicht van de topologie](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="tools"></a>Hulpprogramma's voor implementatie en configuratie

U kunt implementeren en configureren van Azure-netwerkbronnen met een van de volgende hulpprogramma's:

- **Azure-portal:** Een grafische gebruikersinterface die wordt uitgevoerd in een browser. Open de [Azure Portal](https://portal.azure.com).
- **Azure PowerShell:** Opdrachtregelprogramma's voor het beheren van Azure van Windows-computers. Meer informatie over Azure PowerShell door het lezen van de [overzicht van Azure PowerShell](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure-opdrachtregelinterface (CLI):** Opdrachtregelprogramma's voor het beheren van Azure van Linux, macOS of Windows-computers. Meer informatie over de Azure CLI door te lezen die de [overzicht van Azure CLI](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure Resource Manager-sjablonen:** Een bestand (in JSON-indeling) die de infrastructuur en configuratie van een Azure-oplossing definieert. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Lees voor meer informatie over het ontwerpen van sjablonen, de [aanbevolen procedures voor het maken van sjablonen](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. Sjablonen kunnen worden geïmplementeerd met de Azure portal, CLI of PowerShell. U kunt meteen aan de slag met sjablonen, implementeert u een van de vele vooraf geconfigureerde sjablonen in de [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=network) bibliotheek. 

## <a name="pricing"></a>Prijzen

Sommige van de Azure VPN-services hebben een kosten in rekening gebracht, terwijl andere gratis zijn. Weergave de [virtueel netwerk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), [Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/), [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer), [Network Watcher](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) en [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) prijspagina's voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Uw eerste VNet maken en verbinden met een paar virtuele machines, via de stappen in de [maken van uw eerste virtuele netwerk](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- De computer met een VNet verbinden via de stappen in de [een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- Taakverdeling instellen voor Internet-verkeer op openbare servers via de stappen in de [maken van een internetgerichte load balancer](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
