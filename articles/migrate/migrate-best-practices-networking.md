---
title: Aanbevolen procedures voor het instellen van netwerken aan voor workloads die zijn gemigreerd naar Azure | Microsoft Docs
description: Na de migratie naar Azure, get-aanbevolen procedures voor het instellen van netwerken voor uw gemigreerde workloads.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: raynew
ms.openlocfilehash: 6d679cc3d24e2f0f2cb79eb2c9f02422d0818c80
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215266"
---
# <a name="best-practices-to-set-up-networking-for-workloads-migrated-to-azure"></a>Aanbevolen procedures voor het instellen van netwerken aan voor workloads die zijn gemigreerd naar Azure

Als u plannen en ontwerpen voor migratie, naast de migratie zelf, is een van de belangrijkste stappen het ontwerpen en implementeren van Azure-netwerken. Dit artikel wordt beschreven aanbevolen procedures voor het netwerk wanneer u migreert naar IaaS en PaaS-implementaties in Azure.

> [!IMPORTANT]
> De aanbevolen procedures en meningen in dit artikel beschreven zijn gebaseerd op het Azure-platform en service-functies die beschikbaar zijn op het moment van schrijven. Functies en mogelijkheden veranderen verloop van tijd. Het is mogelijk dat niet alle aanbevelingen van toepassing zijn voor uw implementatie, dus selecteer die voor u werkt.


## <a name="design-virtual-networks"></a>Virtuele netwerken ontwerpen

Azure biedt virtuele netwerken (VNets):
- Azure-resources communiceren privé, rechtstreeks en veilig met elkaar via vnet's.
- U kunt endpoint-verbindingen configureren op VNets die zijn voor virtuele machines en services die internetcommunicatie is vereist.
- Een VNet is een logische isolatie van de Azure-cloud die toegewezen aan uw abonnement.
- U kunt meerdere VNets binnen elk Azure-abonnement en Azure-regio kunt implementeren.
- Elk VNet is geïsoleerd van andere vnet's.
- VNets mag persoonlijke en openbare IP-adressen die zijn gedefinieerd [RFC 1918](https://tools.ietf.org/html/rfc1918), uitgedrukt in CIDR-notatie. Openbare IP-adressen zijn niet rechtstreeks toegankelijk via internet.
- VNets verbinden met elkaar met behulp van VNet-peering. Aangesloten vnet's kunnen zich in de dezelfde of verschillende regio's. Dus kunnen resources in het ene VNet verbinden met resources in andere VNets.
- Standaard Azure routeert verkeer tussen subnetten binnen een VNet, verbonden VNets, on-premises netwerken en internet.


Er zijn een aantal dingen die u denken wanneer planning van de topologie van uw VNet moet, met inbegrip van het rangschikken van IP-adres opslagruimten, het implementeren van een ster-netwerk, hoe u VNets segmenteren in subnetten, DNS instellen en implementeren van Azure-beschikbaarheidszones.

## <a name="best-practice-plan-ip-addressing"></a>Aanbevolen: IP-adressering plannen

Wanneer u VNets als onderdeel van uw migratie maakt, is het belangrijk dat u van plan bent om uw VNet-IP-adresruimte.

- U moet een adresruimte die groter is dan een CIDR-bereik van /16 voor elke VNet niet toewijzen. VNets kunnen voor het gebruik van 65536 IP-adressen en toewijzen van een voorvoegsel kleiner is dan/16 zou leiden tot verlies van IP-adressen. Het is belangrijk dat u niet verspilt aan het IP-adressen, zelfs als ze zich in het persoonlijke bereiken die zijn gedefinieerd door RFC 1918.
- De VNet-adresruimte mag niet overlappen met bereiken van on-premises netwerk.
- NAT (Network Address Translation) mag niet worden gebruikt.
- Overlappende adressen kan leiden tot netwerken die niet worden verbonden en routering dat werkt niet goed. Als netwerken elkaar overlappen, moet u het netwerk ontwerpen of netwerkadresomzetting (NAT) gebruiken.

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) van Azure VNets.
- [Lezen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq) de netwerkondersteuning Veelgestelde vragen over.
- [Meer informatie over](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) beperkingen netwerken.


## <a name="best-practice-implement-a-hub-spoke-network-topology"></a>Aanbevolen: Een ster-netwerktopologie implementeren

Een ster-netwerktopologie worden geïsoleerd van werkbelastingen terwijl services zoals identiteit en beveiliging.
- De hub is een Azure-VNet die als een centraal punt van connectiviteit fungeert.
- De spaken zijn VNets die verbinding met de hub VNet maken met behulp van VNet-peering.
- Gedeelde services worden geïmplementeerd in de hub, terwijl afzonderlijke workloads zijn geïmplementeerd als knooppunten. 

Overweeg de volgende:
- Een hub en spoke-topologie implementeren in Azure, centraliseert algemene services zoals verbindingen met on-premises netwerken en firewalls isolatie tussen VNets. De hub VNet biedt een centraal punt van connectiviteit met on-premises netwerken en een plek voor host services gebruik door werkbelastingen die worden gehost in het knooppunt VNets.
- Een hub en spoke-configuratie wordt doorgaans gebruikt door grote ondernemingen. Kleinere netwerken overwegen een eenvoudigere ontwerp om op te slaan van de kosten en complexiteit.
- Knooppunt VNets kan worden gebruikt om workloads te isoleren, met elk knooppunt afzonderlijk beheerd vanaf andere knooppunten. Elke workload kan bestaan uit meerdere lagen en meerdere subnetten die zijn verbonden met Azure-load balancers.
- Hub en spoke-VNets kunnen worden geïmplementeerd in verschillende resourcegroepen en zelfs in verschillende abonnementen. Wanneer u virtuele netwerken in verschillende abonnementen koppelen, kunnen de abonnementen worden gekoppeld aan de dezelfde of verschillende tenants voor Azure Active Directory (AD). Hierdoor gedecentraliseerd beheer van elke workload, terwijl services in het hub-netwerk.

![Wijzigingsbeheer](./media/migrate-best-practices-networking/hub-spoke.png)
*Hub en spoke-topologie*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) een hub en spoke-topologie.
- Ontvang aanbevelingen voor het uitvoeren van Azure network [Windows](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/windows-vm#network-recommendations) en [Linux](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/linux-vm#network-recommendations) VM's.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) VNet-peering.


## <a name="best-practice-design-subnets"></a>Aanbevolen: Ontwerp subnetten

Voor isolatie binnen een VNet, kunt u het segment in een of meer subnetten en een gedeelte van de adresruimte van het VNet op elk subnet toewijzen.
- U kunt meerdere subnetten binnen elk VNet maken.
- Standaard Azure routeert het netwerkverkeer tussen alle subnetten in een VNet.
- Uw subnet beslissingen zijn gebaseerd op uw technische en organisatorische vereisten.  
- U maakt met behulp van de CIDR-notatie subnetten.
- Bij het bepalen van de netwerkbereik voor subnetten, is het belangrijk te weten dat Azure blijft vijf IP-adressen van elk subnet dat kan niet worden gebruikt. Bijvoorbeeld, als u het kleinste beschikbaar subnet van /29 (met acht IP-adressen) maakt, Azure, behouden vijf adressen, zodat u slechts drie bruikbare adressen die kunnen worden toegewezen aan hosts op het subnet hebben.
- In de meeste gevallen /28 gebruik als de kleinste subnet wordt aanbevolen.

### <a name="example"></a>Voorbeeld

De tabel ziet u een voorbeeld van een VNet met een adresruimte van 10.245.16.0/20 gesegmenteerd in subnetten, voor een geplande migratie.

**Subnet** | **CIDR** | **Adressen** | **Gebruiken**
--- | --- | --- | ---
DEV-FE-EUS2 | 10.245.16.0/22 | 1019 | Front-end-/ weblaag VM 's
DEV-APP-EUS2 | 10.245.20.0/22 | 1019 | App-laag virtuele machines
DEV-DB-EUS2 | 10.245.24.0/23 | 507 | Database-VM 's

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#segmentation) subnetten ontwerpen.
- [Informatie over hoe](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure) een fictief bedrijf (Contoso) hun netwerkinfrastructuur voorbereid voor migratie.


## <a name="best-practice-set-up-a-dns-server"></a>Aanbevolen: Een DNS-server instellen

Azure toevoegt standaard een DNS-server wanneer u een VNet implementeert. Hiermee kunt u snel vnet's maken en implementeren van resources. Deze DNS-server biedt echter alleen services tot de resources op dit VNet. Als u wilt meerdere VNets met elkaar verbinden, of verbinding maken met een on-premises server via vnet's, moet u extra UPN-mogelijkheden voor probleemoplossing. Bijvoorbeeld, moet u mogelijk Active Directory om op te lossen van DNS-namen tussen virtuele netwerken. Om dit te doen, moet u uw eigen aangepaste DNS-server in Azure implementeren.

- DNS-servers in een VNet kunnen DNS-query's doorsturen naar de recursieve resolvers in Azure. Hiermee kunt u voor het omzetten van hostnamen binnen dat VNet. Een domeincontroller die wordt uitgevoerd in Azure kunt bijvoorbeeld reageren op DNS-query's voor een eigen domeinen en alle andere query's sturen naar Azure.
- Doorsturen van de DNS kunt virtuele machines om te zien van uw on-premises bronnen (via de domeincontroller) en de hostnamen van de opgegeven Azure (met behulp van de doorstuurserver). Toegang tot de recursieve resolvers in Azure wordt geleverd met behulp van het virtuele IP-adres 168.63.129.16.
- Doorsturen van de DNS ook kan DNS-omzetting tussen VNets en kunt on-premises machines naar het omzetten van hostnamen die zijn verstrekt door Azure.
    - Om op te lossen de naam van een VM-host, de virtuele machine van de DNS-server moet zich in hetzelfde VNet bevinden, en worden geconfigureerd voor doorsturen host de naam van query's naar Azure.
    - Omdat de DNS-achtervoegsel anders in elk VNet is, kunt u voorwaardelijk doorsturen regels voor het verzenden van DNS-query's naar het juiste VNet voor de omzetting.
- Wanneer u uw eigen DNS-servers gebruikt, kunt u meerdere DNS-servers opgeven voor elke VNet. U kunt ook meerdere DNS-servers per netwerkinterface (voor Azure Resource Manager) of per cloudservice (voor het klassieke implementatiemodel) opgeven.
- DNS-servers die zijn opgegeven voor een netwerk-interface of cloud service hebben voorrang op de DNS-servers die zijn opgegeven voor het VNet.
- In het implementatiemodel Azure Resource Manager kunt u DNS-servers voor een VNet en een netwerkinterface, maar de aanbevolen procedure is het gebruik van de instelling alleen voor VNets.

    ![DNS-servers](./media/migrate-best-practices-networking/dns2.png) *DNS-servers voor VNet*

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure) naamomzetting wanneer u uw eigen DNS-server.
- [Meer informatie over](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-subscriptions) DNS naamgevingsregels en -beperkingen.


## <a name="best-practice-set-up-availability-zones"></a>Aanbevolen: Beschikbaarheidszones instellen

Beschikbaarheidszones verhogen hoge beschikbaarheid voor uw apps en gegevens beschermen tegen storingen in datacenters.

- Beschikbaarheidszones zijn unieke fysieke locaties binnen een Azure-regio.
- Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken.
- Om ervoor te zorgen tolerantie, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's.
- De fysieke scheiding tussen beschikbaarheidszones binnen een regio beschermt toepassingen en gegevens tegen storingen in datacenters.
- Zone-redundante services repliceren uw toepassingen en gegevens in meerdere beschikbaarheidszones om te beschermen tegen single point of failure. --Met beschikbaarheidszones biedt Azure een SLA van de virtuele machine een uptime van 99,99%.

    ![Binnen een beschikbaarheidszone](./media/migrate-best-practices-networking/availability-zone.png) *binnen een beschikbaarheidszone*

- U kunt plannen en hoge beschikbaarheid bouwen in uw migratiearchitectuur van compute, opslag, netwerken en gegevensbronnen binnen een zone plaatsen, en ze te repliceren in andere zones. Azure-services die ondersteuning voor beschikbaarheidszones worden onderverdeeld in twee categorieën:
    - Zonegebonden services: U kunt een resource koppelen aan een specifieke zone. Voor beheerde schijven, IP-adressen voor voorbeeld-VM's).
    - Zone-redundante services: De resource wordt automatisch gerepliceerd in zones. Bijvoorbeeld: zone-redundante opslag, Azure SQL Database.
- U kunt een standaard Azure taakverdeling met internetgerichte workloads of app-lagen voor zonegebonden fouttolerantie implementeren.

    ![Netwerktaakverdeler](./media/migrate-best-practices-networking/load-balancer.png) *Load balancer*

**Meer informatie:**
-   [Bekijk een overzicht](https://docs.microsoft.com/azure/availability-zones/az-overview) van de beschikbaarheidszones.



## <a name="design-hybrid-cloud-networking"></a>Ontwerp hybride cloud-netwerken

Voor een geslaagde migratie is het essentieel dat on-premises zakelijke netwerken verbinden met Azure. Hiermee maakt u een always on-verbinding wel een hybride cloud-netwerk, waar services worden opgegeven met de Azure-cloud voor zakelijke gebruikers. Er zijn twee opties voor het maken van dit type netwerk:

- **Site-to-site VPN:** U maken een site-naar-site-verbinding tussen uw compatibel on-premises VPN-apparaat en een Azure VPN-gateway die geïmplementeerd in een VNet. Een on-premises geautoriseerd resource toegang heeft tot de VNets. Site-naar-site-berichten worden verzonden via een gecodeerde tunnel via internet. 
- **Azure ExpressRoute:** U een Azure ExpressRoute-verbinding tussen uw on-premises netwerk en Azure, via een ExpressRoute-partner tot stand brengen. Deze verbinding is privé; verkeer niet via internet gaan.

**Meer informatie:**

- [Meer informatie](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/vpn) over hybride cloud-netwerken.

## <a name="best-practice-implement-a-highly-available-site-to-site-vpn"></a>Aanbevolen: Een maximaal beschikbare site-naar-site-VPN implementeren

Voor het implementeren van een site-naar-site-VPN, moet u een VPN-gateway in Azure instellen.
- Een VPN-gateway is een specifiek type VNet-gateway die wordt gebruikt voor het verzenden van versleuteld verkeer tussen een Azure VNet en een on-premises locatie via het openbare Internet.
- U kunt ook een VPN-gateway gebruiken voor het verzenden van versleuteld verkeer tussen Azure-VNets via de Microsoft-netwerk.
- Elk VNet kan slechts één VPN-gateway hebben.
- U kunt meerdere verbindingen naar dezelfde VPN-gateway maken. Wanneer u meerdere verbindingen maken, delen alle VPN-tunnels de bandbreedte van de gateway beschikbaar.
- Elke Azure VPN-gateway bestaat uit twee instanties in een actieve stand-byconfiguratie.
    - Voor gepland onderhoud of unplaned onderbreking van de actieve sessie van failover wordt uitgevoerd en de standby-instantie heeft ten opzichte van automatisch en wordt hervat de site-naar-site of de VNet-naar-VNet-verbinding. 
    - De overschakeling zorgt ervoor dat een korte onderbreking.
    - Bij gepland onderhoud moet connectiviteit binnen 10 tot 15 seconden worden hersteld.
    - Voor niet-geplande problemen wordt herstellen van de verbinding niet langer, ongeveer 1 tot en met 1.5 minuten in het ergste geval.
    - Punt-naar-site (P2S) VPN-clientverbindingen met de gateway wordt verbroken en moeten de gebruikers opnieuw verbinding maken vanaf clientcomputers.

Bij het instellen van een site-naar-site-VPN, doen u het volgende:
 - U moet een VNet die het adresbereik niet overlappen met de on-premises netwerk waarmee de VPN-verbinding verbinding maken.
 - U maakt een gateway-subnet in het netwerk.
 - U maakt een VPN-gateway, geef het Gatewaytype (VPN) en of de gateway op basis van beleid of op basis van route. Een RouteBased VPN wordt aanbevolen als meer geschikt en toekomstbestendige.
 - U on-premises een lokale netwerkgateway maken en configureren uw on-premises VPN-apparaat. 
 - U maakt een failover-site-naar-site VPN-verbinding tussen de VNet-gateway en de on-premises-apparaat. Met op route gebaseerde VPN beschikt u over actief-passief of actief / actief-verbindingen met Azure. Op route gebaseerde biedt ook ondersteuning voor zowel de site-naar-site (vanaf elke computer) en de punt-naar-site (vanaf één computer) verbindingen tegelijkertijd.
 - Geeft u de gateway-SKU die u wilt gebruiken. Dit zal afhankelijk zijn van uw vereisten voor werkbelasting, doorvoer, functies en sla's.
 - Border gateway protocol (BGP) is een optionele functie kunt u met Azure ExpressRoute en VPN-gateways op basis van route naar uw on-premises BGP-routes doorgegeven aan uw vnet's.

![VPN](./media/migrate-best-practices-networking/vpn.png)
*Site-naar-site-VPN*
 
**Meer informatie:**

- [Beoordeling](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) compatibel on-premises VPN-apparaten.
- [Bekijk een overzicht](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) van VPN-gateways.
- [Meer informatie over](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable) maximaal beschikbare VPN-verbindingen.
- [Meer informatie over](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) plannen en ontwerpen van een VPN-gateway.
- [Beoordeling](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) VPN gateway-instellingen.
- [Beoordeling](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku) gateway-SKU's.
- [Meer informatie over](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview) instellen van BGP met Azure VPN-gateways.


### <a name="best-practice-configure-a-gateway-for-vpn-gateways"></a>Aanbevolen: Een gateway voor VPN-Gateways configureren

Wanneer u een VPN-gateway in Azure maken, moet u een speciaal subnet met de naam GatewaySubnet gebruiken. Bij het maken van deze opmerking subnet deze aanbevolen procedures:

- De lengte van het voorvoegsel van het gatewaysubnet kan een maximale lengte van 29 (bijvoorbeeld 10.119.255.248/29) hebben. De huidige aanbeveling is dat u een voorvoegsel van 27 (bijvoorbeeld 10.119.255.224/27).
- Wanneer u de adresruimte van het gatewaysubnet definieert, gebruikt u het laatste deel van de VNet-adresruimte.
- Wanneer u het GatewaySubnet Azure gebruikt, nooit implementeren virtuele machines of andere apparaten, zoals Application Gateway in het gatewaysubnet.
- Wijs geen een netwerkbeveiligingsgroep (NSG) toe aan dit subnet. Dit zorgt ervoor dat de gateway niet meer werkt.

**Meer informatie:**
- [Gebruik dit hulpprogramma](https://gallery.technet.microsoft.com/scriptcenter/Address-prefix-calculator-a94b6eed) om te bepalen van uw IP-adresruimte.

## <a name="best-practice-implement-azure-virtual-wan-for-branch-offices"></a>Aanbevolen: Azure virtuele WAN worden geïmplementeerd voor filialen

Voor meerdere VPN-verbindingen is Azure virtuele WAN een netwerkservice, dat geoptimaliseerde en geautomatiseerde vertakking-vertakking-connectiviteit via Azure biedt.
- Met Virtual WAN kunt u apparaten in filialen verbinden en configureren, zodat deze met Azure kunnen communiceren. Dit kan worden gedaan handmatig of met behulp van de gewenste provider apparaten via een virtueel WAN-partner.
- Met behulp van de provider van de gewenste apparaten kan voor eenvoudig gebruik, connectiviteit en Configuratiebeheer.
- De Azure-WAN ingebouwde dashboard geeft onmiddellijk probleemoplossing inzicht die tijd besparen en bieden een eenvoudige manier om bij te houden van grootschalige site-naar-site-connectiviteit. 

**Meer informatie:**
[meer informatie over](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about) Azure virtuele WAN.

### <a name="best-practice-implement-expressroute-for-mission-critical-connections"></a>Aanbevolen: ExpressRoute voor essentiële taken kritiek verbindingen implementeren

De service Azure ExpressRoute kunt u uw on-premises infrastructuur in de Microsoft cloud uitbreiden door het maken van privéverbindingen tussen het virtuele Azure-datacenter en on-premises netwerken.
- ExpressRoute-verbindingen kunnen zich via een any-to-any (IP VPN) netwerk, een point-to-point Ethernet-netwerk of via een connectiviteitsprovider. Ze lopen niet via het openbare internet.
- ExpressRoute-verbindingen bieden betere beveiliging, betrouwbaarheid en hogere snelheden (maximaal 10 Gbps), samen met consistente latentie.
- ExpressRoute is handig voor het virtuele datacenters, omdat klanten van nalevingsregels voor die zijn gekoppeld aan particuliere verbindingen profiteren kunnen.
- Met ExpressRoute Direct u kunt rechtstreeks verbinding maken met Microsoft-routers op 100Gbps voor grotere bandbreedtebehoeften.
- ExpressRoute maakt gebruik van BGP voor het uitwisselen van routes tussen on-premises netwerken, Azure-exemplaren en openbare adressen van Microsoft.

ExpressRoute-verbindingen meestal implementatie omvat het omgang met een ExpressRoute-serviceprovider. Voor een snel starten, het is gebruikelijk voor in eerste instantie met een site-naar-site-VPN-verbinding tot stand brengen van verbinding tussen het virtuele datacenter en on-premises bronnen, en vervolgens migreren naar een ExpressRoute-verbinding als een fysieke onderlinge verbinding met uw serviceprovider tot stand gebracht.

**Meer informatie:**
- [Bekijk een overzicht](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) van ExpressRoute.
- [Meer informatie over](https://docs.microsoft.com/azure/expressroute/expressroute-erdirect-about) ExpressRoute Direct.

### <a name="best-practice-optimize-expressroute-routing-with-bgp-communities"></a>Aanbevolen: ExpressRoute-routering met BGP-community's optimaliseren

Als u meerdere ExpressRoute-circuits hebt, hebt u meer dan één pad om verbinding te maken met Microsoft. Suboptimale routering kan gebeuren als gevolg hiervan, en uw verkeer kan duren voordat een langer pad naar Microsoft en Microsoft contact met uw netwerk. Hoe langer het netwerkpad, hoe groter de latentie. Latentie heeft een directe invloed op app-prestaties en gebruikerservaring.

#### <a name="example"></a>Voorbeeld

We nemen een voorbeeld.

- U hebt twee kantoren in de VS, één in Los Angeles en één in New York.
- Uw kantoren zijn aangesloten op een WAN, wat kan uw eigen backbone-netwerk of van de serviceprovider IP VPN zijn.
- U hebt twee ExpressRoute-circuits, één in US - west en één in US - oost. Deze zijn ook aangesloten op het WAN. U hebt uiteraard twee paden om verbinding te maken met het Microsoft-netwerk.


 
**Probleem** nu Stel, u hebt een Azure-implementatie (bijvoorbeeld Azure App Service) in VS West en VS-Oost.
- U wilt dat gebruikers in elk kantoor voor toegang tot de dichtstbijzijnde Azure-services voor een optimale ervaring.
- Daarom wilt u gebruikers in Los Angeles verbinden met Azure VS West en gebruikers in New York met Azure VS Oost.
- Dit werkt voor gebruikers van de oostkust maar niet voor de west. Het probleem is als volgt:
    - Op elk ExpressRoute-circuit adverteren we beide voorvoegsels in Azure VS Oost (23.100.0.0/16) en Azure VS West (13.100.0.0/16).
    - Zonder te weten welk voorvoegsel bij welke regio hoort, is voorvoegsels anders worden niet behandeld.
    - Uw WAN-netwerk kunt ervan uitgaan dat beide voorvoegsels zich dichter bij VS Oost dan bij VS West en gebruikers van beide kantoren daarom te naar het ExpressRoute-circuit in VS-Oost routeren, bieden een minder dan optimale ervaring voor gebruikers in het kantoor in Los Angeles.

![VPN](./media/migrate-best-practices-networking/bgp1.png)
*BGP-community's niet-geoptimaliseerde verbinding*

**Oplossing**

Voor het optimaliseren van routering voor gebruikers van beide kantoren, moet u weet welk voorvoegsel van Azure VS West is en dat afkomstig is van Azure VS Oost. U kunt deze informatie coderen met behulp van BGP-Communitywaarden.
- U kunt een unieke BGP-communitywaarde toewijzen aan elke Azure-regio. Bijvoorbeeld 12076: 51004 voor VS-Oost; 12076: 51006 voor VS-West.
- Nu het is duidelijk welk voorvoegsel behoort tot welke Azure-regio, kunt u een voorkeur ExpressRoute-circuit configureren.
- Omdat u BGP om routeringsinformatie gebruikt, kunt u lokale voorkeur van het BGP-routering beïnvloeden.
- In ons voorbeeld toewijzen u een hogere waarde van de lokale voorkeur aan 13.100.0.0/16 in VS West dan in VS-Oost en op dezelfde manier, een hogere waarde voor de lokale voorkeur aan 23.100.0.0/16 in VS Oost dan in VS West. 
- Deze configuratie zorgt ervoor dat als beide paden naar Microsoft beschikbaar zijn, wordt gebruikers in Los Angeles maakt verbinding met Azure VS West met behulp van het circuit west en New York gebruikers verbinding maken met Azure VS Oost met behulp van het circuit Oost. Routering is nu aan beide zijden geoptimaliseerd.

![VPN](./media/migrate-best-practices-networking/bgp2.png)
*BGP-community's geoptimaliseerd verbinding*


**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing) routering optimaliseren

## <a name="securing-vnets"></a>VNets beveiligen

De verantwoordelijkheid voor het beveiligen van vnet's wordt gedeeld tussen Microsoft en u. Microsoft biedt veel functies voor netwerkbeheer, evenals services waarmee de resources te beveiligen. Bij het ontwerpen van beveiliging voor vnet's, zijn er een aantal aanbevolen procedures die u volgen moet, met inbegrip van de implementatie van een perimeternetwerk bevinden, met behulp van filters en beveiligingsgroepen, het beveiligen van toegang tot bronnen en IP-adressen en implementeren van beveiliging tegen aanvallen.

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices) van aanbevolen procedures voor netwerkbeveiliging.
- [Meer informatie over het](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#security) ontwerp voor beveiligde netwerken.

## <a name="best-practice-implement-an-azure-perimeter-network"></a>Aanbevolen: Implementeren van een Azure-perimeternetwerk

Hoewel Microsoft sterk investeert bij het beschermen van de cloudinfrastructuur, moet u ook uw cloudservices en -resourcegroepen beveiligen. Een meerlaagse aanpak voor beveiliging biedt de beste verdediging. Een perimeternetwerk plaatsen in plaats is een belangrijk onderdeel van deze strategie defense.

- Een perimeternetwerk voorkomt interne netwerkbronnen dat een niet-vertrouwd netwerk. 
- Dit is de buitenste laag die wordt blootgesteld aan internet. Het algemeen kunnen uitwisselen internet en de enterprise-infrastructuur, meestal met een vorm van beveiliging aan beide zijden. 
- In de netwerktopologie van een typische bedrijf, wordt de basisinfrastructuur sterk verbeterd op de verbindingen, met meerdere lagen van beveiligingsapparaten. De grens van elke laag bestaat uit de apparaten en beleid afdwingingspunten.
- Elke laag kan een combinatie van de oplossingen voor netwerkbeveiliging, zoals firewalls, voorkomen van Denial of Service (DoS), intrusion detection/inbraakdetectiehandtekeningen protection systemen (IDS/IPS) en VPN-apparaten bevatten.
- Afdwingen van beleid in het perimeternetwerk kunt gebruiken voor firewall-beleid, toegangsbeheerlijsten (ACL's) of specifieke routering.
- Als het inkomende verkeer van het internet ontvangt, heeft het probleem is onderschept en verwerkt door een combinatie van oplossing voor de beveiliging voor blok-aanvallen en schadelijke verkeer, terwijl legitieme aanvragen in het netwerk.
- Inkomend verkeer kunt rechtstreeks naar resources in het perimeternetwerk versturen. De netwerkbron perimeternetwerk kan vervolgens communiceren met andere resources die dieper in het netwerk, om verkeer doorsturen naar het netwerk na de validatie te verplaatsen.

De volgende afbeelding toont een voorbeeld van een perimeternetwerk één subnet in een bedrijfsnetwerk bevinden, met twee grenzen voor netwerkbeveiliging.

![VPN](./media/migrate-best-practices-networking/perimeter.png)
*de implementatie van de Perimeter*

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) implementeren van een perimeternetwerk tussen Azure en uw on-premises datacenter.


## <a name="best-practice-filter-vnet-traffic-with-nsgs"></a>Aanbevolen: VNet-verkeer met nsg's filteren

Netwerkbeveiligingsgroepen (NSG) bevat meerdere binnenkomende en uitgaande beveiligingsregels die netwerkverkeer naar en van resources te filteren. Filters kan worden door de bron en doel-IP-adres, poort en protocol. 
- Nsg's bevatten beveiligingsregels die toestaan of weigeren van inkomend verkeer naar (of uitgaand netwerkverkeer van) verschillende soorten Azure-resources. Voor elke regel kunt u de bron en het doel, de poort en het protocol opgeven.
- NSG-regels worden geëvalueerd op basis van prioriteit met behulp van de gegevens 5-tuple (bron, bronpoort, doel, bestemmingspoort en protocol) wilt toestaan of weigeren van het verkeer.
- Voor bestaande verbindingen wordt een stroomrecord gemaakt. Communicatie wordt toegestaan of geweigerd op basis van de verbindingsstatus van de stroomrecord.
- De record van een stroom kunt een NSG te zijn stateful. Bijvoorbeeld, als u een beveiligingsregel voor uitgaand naar een adres via poort 80 opgeeft, hoeft u niet een beveiligingsregel voor binnenkomend verkeer om te reageren op het uitgaande verkeer. U hoeft alleen een beveiligingsregel voor binnenkomend verkeer op te geven als de communicatie extern is gestart.
- Het omgekeerde geldt ook. Als binnenkomend verkeer is toegestaan via een poort, moet u niet een uitgaande beveiligingsregel om te reageren op verkeer via de poort opgeven.
- Bestaande verbindingen worden niet onderbroken wanneer u een regel waarmee de stroom verwijdert. Verkeersstromen zijn onderbroken wanneer er verbindingen worden gestopt en er is geen verkeersstromen in beide richtingen ten minste een paar minuten.
- Wanneer u nsg's maakt, maakt u als enkele mogelijk, maar als veel die nodig zijn.

### <a name="best-practice-secure-northsouth-and-eastwest-traffic"></a>Aanbevolen: Noordelijke/zuidelijke richting en Oostelijke/Westelijke verkeer beveiligen

Bij het beveiligen van vnet's, is het belangrijk te bedenken aanvalsvectoren.
- Gebruik alleen subnet-nsg's maakt uw omgeving eenvoudiger, maar alleen voor beveiliging van verkeer in uw subnet. Dit staat bekend als noordelijke/zuidelijke richting verkeer.
- Verkeer tussen virtuele machines in hetzelfde subnet wordt ook wel Oost/west-verkeer.
- Het is belangrijk dat u gebruikmaken van beide vormen van beveiliging, zodat als een hacker toegang van buiten deze moeten worden gestopt tijdens het koppelen van de machines die zich in hetzelfde subnet.

### <a name="use-service-tags-on-nsgs"></a>Servicetags gebruiken in nsg 's

Een servicetag vertegenwoordigt een groep van IP-adresvoorvoegsels. Met behulp van een servicetag helpt bij het minimaliseren van complexiteit bij het maken van NSG-regels.
- U kunt servicetags gebruiken in plaats van specifieke IP-adressen wanneer u regels maakt.
- Microsoft beheert de adresvoorvoegsels die zijn gekoppeld aan een servicetag en werkt de servicetag automatisch bij wanneer adressen veranderen.
- U kunt uw eigen servicetag maken of opgeven welke IP-adressen zijn opgenomen in een tag.

Servicetags duren voordat de handmatig werk buiten een regel toewijzen aan groepen van Azure-services. Bijvoorbeeld, als u toestaan dat een web-servers toegang tot een Azure SQL Database met VNet-subnet wilt, u kan maken van een regel voor uitgaande poort 1433 en gebruikt de **Sql** servicetag.
- Dit **Sql** tag geeft de adresvoorvoegsels van de Azure SQL Database en Azure SQL Data Warehouse-services.
- Als u opgeeft **Sql** als de waarde verkeer wordt toegestaan of geweigerd op Sql.
- Als u wilt dat alleen voor toegang tot **Sql** in een bepaalde regio, kunt u deze regio. Bijvoorbeeld, als u toestaan dat alleen toegang tot Azure SQL Database in de regio VS-Oost wilt, kunt u **Sql.EastUS** als servicetag.
- De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. Bijvoorbeeld de tag vertegenwoordigt de service Azure SQL Database, maar niet een bepaalde SQL-database of de server.
- Alle adresvoorvoegsels die worden vertegenwoordigd door deze tag worden ook vertegenwoordigd door de tag **Internet**.


**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/security-overview) nsg's.
- [Beoordeling](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) de servicetags die beschikbaar zijn voor nsg's.


## <a name="best-practice-use-application-security-groups"></a>Aanbevolen: Gebruik van beveiligingsgroepen voor toepassing

Toepassingsbeveiligingsgroepen kunnen u netwerkbeveiliging configureren als een natuurlijke uitbreiding van een app.

- U kunt virtuele machines groeperen en beleidsregels voor netwerkbeveiliging op basis van beveiligingsgroepen voor toepassing definiëren.
- Toepassingsbeveiligingsgroepen kunnen u uw beveiligingsbeleid op grote schaal zonder handmatig onderhoud van expliciete IP-adressen gebruiken.
- Beveiligingsgroepen voor toepassing verwerkt de complexiteit van expliciete IP-adressen en meerdere regelsets, zodat u zich kunt richten op uw bedrijfslogica. 

### <a name="example"></a>Voorbeeld

![Toepassingsbeveiligingsgroep](./media/migrate-best-practices-networking/asg.png)
*voorbeeld van de toepassing security group*

**Netwerkinterface** | **Toepassingsbeveiligingsgroep**
--- | ---
NIC1 | AsgWeb
NIC2 | AsgWeb
NIC3 | AsgLogic
NIC4 | AsgDb 

- Elke netwerkinterface behoort tot slechts één toepassingsbeveiligingsgroep in ons voorbeeld, maar in feite een interface kan behoren tot meerdere groepen, in overeenstemming met de Azure-limieten.
- Geen van de netwerkinterfaces heeft gekoppelde NSG. NSG1 is gekoppeld aan beide subnetten en de volgende regels bevat.

    **De naam van regel** | **Doel** | **Details**
    --- | --- | ---   
    Allow-HTTP-Inbound-Internet | Verkeer van internet naar de webservers toestaan. Inkomend verkeer van internet wordt geweigerd door de DenyAllInbound standaardbeveiligingsregel, zodat er geen aanvullende regel voor de toepassingsbeveiligingsgroepen AsgLogic of AsgDb nodig is. | Prioriteit: 100<br/><br/> Bron: internet<br/><br/> Bronpoort: *<br/><br/> Bestemming: AsgWeb<br/><br/> Doelpoort: 80<br/><br/> Protocol: TCP<br/><br/> Toegang: Toestaan.
    Deny-Database-All | De beveiliging standaardregel AllowVNetInBound kan alle communicatie tussen resources in hetzelfde VNet, met deze regel is nodig om verkeer te weigeren van alle resources. | Prioriteit: 120<br/><br/> Bron: *<br/><br/> Bronpoort: *<br/><br/> Bestemming: AsgDb<br/><br/> Doelpoort: 1433<br/><br/> Protocol: Alle<br/><br/> Toegang: Weigeren.
    Allow-Database-BusinessLogic | Verkeer van de toepassingsbeveiligingsgroep AsgLogic aan de toepassingsbeveiligingsgroep AsgDb toestaan. De prioriteit voor deze regel is hoger dan de regel voor weigeren-Database-All en voordat u deze regel wordt verwerkt, zodat verkeer van de toepassingsbeveiligingsgroep AsgLogic is toegestaan, en al het andere verkeer wordt geblokkeerd. | Prioriteit: 110<br/><br/> Bron: AsgLogic<br/><br/> Bronpoort: *<br/><br/> Bestemming: AsgDb<br/><br/> Doelpoort: 1433<br/><br/> Protocol: TCP<br/><br/> Toegang: Toestaan.

- De regels die een toepassingsbeveiligingsgroep als bron of doel opgeven, worden alleen toegepast op netwerkinterfaces die lid van de toepassingsbeveiligingsgroep. Als de netwerkinterface geen lid is van een toepassingsbeveiligingsgroep, wordt de regel niet toegepast op de netwerkinterface, ook niet als de netwerkbeveiligingsgroep aan het subnet is gekoppeld.

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) toepassingsbeveiligingsgroepen.


### <a name="best-practice-secure-access-to-paas-using-vnet-service-endpoints"></a>Aanbevolen: Beveiligde toegang tot PaaS met behulp van VNet-service-eindpunten

VNet-service-eindpunten Breid uw privé-adresruimte van VNet en de identiteit van Azure-services via een directe verbinding.

- Eindpunten kunt u voor het beveiligen van belangrijke Azure-serviceresources naar uw vnet's alleen. Verkeer van uw VNet naar de Azure-service blijft altijd in het Microsoft Azure-backbonenetwerk.
- Privé-adresruimte van VNet kunt overlappende en kan daarom niet worden gebruikt voor het aanduiden van verkeer dat afkomstig is van een VNet.
- Nadat de service-eindpunten zijn ingeschakeld in uw VNet, kunt u Azure-serviceresources beveiligen door een VNet-regel toe te voegen aan de service-resources. Dit biedt betere beveiliging door volledig door de openbare internettoegang tot resources, en het verkeer wordt toegestaan alleen vanaf uw VNet.

![Service-eindpunten](./media/migrate-best-practices-networking/endpoint.png)
*Service-eindpunten*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) VNet-service-eindpunten.


## <a name="best-practice-control-public-ip-addresses"></a>Aanbevolen: Openbare IP-adressen

Openbare IP-adressen in Azure kunnen worden gekoppeld aan virtuele machines, load balancers, application gateways en VPN-gateways.

- Openbare IP-adressen kunnen internetbronnen binnenkomend communiceren met Azure-resources en Azure-resources om te communiceren uitgaand naar het internet.
- Openbare IP-adressen worden gemaakt met een basis of standaard-SKU, met een aantal verschillen tussen deze. Standaard-SKU's kunnen worden toegewezen aan elke service, maar het meest gewoonlijk worden geconfigureerd op virtuele machines, load balancers en toepassingsgateways.
- Het is belangrijk te weten dat geen basic openbaar IP-adres een NSG automatisch geconfigureerd. U moet uw eigen configureren en regels voor het beheren van toegang toewijzen. Standaard SKU-IP-adressen hebben een NSG en regels standaard toegewezen.
- Als een best practice, mag niet de virtuele machines worden geconfigureerd met een openbaar IP-adres.
    - Als u een poort geopend nodig hebt, moet deze alleen voor web-services, zoals poort 80 of 443.
    - Extern beheer standaard poorten zoals SSH (22) en RDP (3389) moeten worden ingesteld om te weigeren, samen met alle andere poorten, met nsg's.
- Een betere manier is om plaatst u virtuele machines achter een Azure load balancer of application gateway. Als de toegang tot poorten voor extern beheer is vereist, kunt u de just-in-time-VM-toegang in Azure Security Center gebruiken.

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) openbare IP-adressen in Azure.
- [Lees meer](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) op just-in-time-VM-toegang in Azure Security Center.


## <a name="leverage-azure-security-features-for-networking"></a>Gebruikmaken van functies van Azure-beveiliging voor netwerken

Azure heeft platform beveiligingsfuncties die eenvoudig te gebruiken en uitgebreide tegenmaatregelen om algemene netwerkaanvallen te bieden. Het gaat hierbij om Azure-Firewall-, Web Application Firewall- en Network Watcher.

## <a name="best-practice-deploy-azure-firewall"></a>Aanbevolen: Firewall van Azure implementeren

Firewall van Azure is een beheerde, cloud-gebaseerde beveiliging netwerkservice die worden beveiligd met uw VNet-resources. Het is een volledig stateful firewall-as-a-service met ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte.

![Service-eindpunten](./media/migrate-best-practices-networking/firewall.png)
*Firewall van Azure*

- In de Firewall van Azure, kunt Centraal maken, afdwingen en meld u toepassings- en network connectivity beleidsregels voor abonnementen en VNets.
- Firewall van Azure gebruikt een statisch openbaar IP-adres voor uw VNet-resources, zodat buiten firewalls om verkeer dat afkomstig is van uw VNet te identificeren.
- Firewall van Azure is volledig geïntegreerd met Azure Monitor voor logboekregistratie en analyse.
- Als een best practice bij het maken van firewallregels voor Azure, door de FQDN-codes te gebruiken om regels te maken.
    - Een FQDN-tag vertegenwoordigt een groep van de FQDN-namen die zijn gekoppeld aan bekende services van Microsoft.
    - U kunt een FQDN-tag om toe te staan de vereiste uitgaande netwerkverkeer via de firewall.
- Als u wilt toestaan handmatig Windows Update-netwerkverkeer via de firewall, moet u bijvoorbeeld om meerdere regels van de toepassing te maken. Gebruik van tags van de FQDN-naam, u een toepassing-regel maken, en de tag Windows-Updates bevatten. Met deze regel is ingevoerd, kan door uw firewall netwerkverkeer naar Microsoft Windows Update-eindpunten stromen.

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/firewall/overview) van Azure-Firewall.
- [Meer informatie over](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN tags.


## <a name="best-practice-deploy-azure-web-application-firewall-waf"></a>Aanbevolen: Azure Web Application Firewall (WAF) implementeren

Webtoepassingen worden steeds doel van aanvallen die gebruikmaken van bekende beveiligingsproblemen. Aanvallen zijn SQL-injectieaanvallen en aanvallen via cross-site scripting. Zo wordt voorkomen dat dergelijke aanvallen in toepassingscode kan lastig zijn, en kan tevens veel onderhoud, patching en controle op meerdere lagen van de Toepassingstopologie vereisen. Een gecentraliseerde web application firewall kunt maakt het beveiligingsbeheer veel eenvoudiger en app-beheerders beveiliging tegen bedreigingen of aanvallen. Een web-app-firewall kan reageren op bedreigingen sneller en patches voor bekende beveiligingsproblemen op een centrale locatie, in plaats van afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

Azure Web application firewall (WAF) is een functie van Azure application gateway.
- WAF biedt gecentraliseerde beveiliging van uw webtoepassingen, tegen algemene aanvallen en beveiligingsproblemen.
- WAF beveiligt zonder aanpassingen aan back endcode.
- Het kan meerdere web-apps beveiligen op hetzelfde moment achter een application gateway
- WAF is geïntegreerd met Azure Security Center.
- U kunt de WAF-regels en regelgroepen volgens de appvereisten van uw aanpassen.
- Als een best practice, moet u een WAF voorkant gebruiken voor een gerichte web-app, met inbegrip van apps op Azure Virtual machines of als een Azure App Service.

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/application-gateway/waf-overview) WAF.
- [Beoordeling](https://docs.microsoft.com/azure/application-gateway/application-gateway-waf-configuration) WAF beperkingen en uitsluitingen.


## <a name="best-practice-implement-azure-network-watcher"></a>Aanbevolen: Implementeer Azure Network Watcher

Azure Network Watcher biedt hulpprogramma's voor het bewaken van resources en communicatie in een Azure VNet. U kunt bijvoorbeeld communicatie tussen een virtuele machine en een eindpunt, zoals een andere virtuele machine of FQDN-naam, resources weergeven en relaties tussen resources in een VNet, controleren of verkeer netwerkproblemen vaststellen.

![Network Watcher](./media/migrate-best-practices-networking/network-watcher.png)
*Network Watcher*

- U kunt met Network Watcher controleren en diagnosticeren van netwerkproblemen zonder u te melden bij virtuele machines.
- U kunt de pakketopname activeren door het instellen van waarschuwingen en toegang krijgen tot informatie over realtime prestaties op packetniveau. Wanneer u een probleem ziet, kunt u het kunt onderzoeken in detail.
- Als best practice, moet u Network Watcher om te controleren van NSG-stroomlogboeken.
    - NSG-stroomlogboeken in Network Watcher kunnen u informatie bekijken over inkomende en uitgaande IP-verkeer via een NSG.
    - Logboeken van de stroom worden geschreven in json-indeling.
    - Logboeken van de stroom weergeven binnenkomende en uitgaande stromen op basis van per regel, de netwerkinterface (NIC) waarop de stroom van toepassing is, 5-tuple-informatie over de stroom (bron-/ doel-IP, bron-/ doel-poort en protocol), en of het verkeer is toegestaan of geweigerd.

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/network-watcher) van Network Watcher.
- [Meer informatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) flow logboeken over NSG.

## <a name="use-partner-tools-in-the-azure-marketplace"></a>Gebruik hulpprogramma's in de Azure Marketplace

Voor complexere netwerktopologieën met, kunt u security-producten van Microsoft-partners, in bepaalde virtuele netwerkapparaten (NVA's).

- Een NVA is een virtuele machine die een netwerkfunctie, zoals een firewall, WAN-optimalisatie of andere netwerkfunctie uitvoert.
- NVA's vergroot de functies van de beveiliging en netwerk van de VNet. Ze kunnen worden geïmplementeerd voor maximaal beschikbare firewalls, inbraakdetectiehandtekeningen preventie inbraakdetectie, web application firewalls (WAF's), WAN-optimalisatie, routering, taakverdeling, VPN, Certificaatbeheer, Active Directory en meervoudige verificatie.
- NVA is beschikbaar via een groot aantal leveranciers in de [Azure Marketplace](https://azuremarketplace.microsoft.com/). 
 

## <a name="best-practice-implement-firewalls-and-nvas-in-hub-networks"></a>Aanbevolen: Firewalls en virtuele netwerkapparaten in hub-netwerken implementeren

Het perimeternetwerk (met toegang tot het internet) wordt in de hub, meestal beheerd via een Firewall van Azure, een firewall-farm, of met Web Application Firewalls (WAF's). Houd rekening met de volgende vergelijkingen.

**Firewall-type** | **Details**
--- | ---
WAF 's | Web-apps worden gebruikt en vaak last van beveiligingsproblemen en potentiële aanvallen.<br/><br/> WAF's zijn ontworpen voor het detecteren van aanvallen tegen de webtoepassingen (HTTP/HTTPS) meer in het bijzonder dan een algemene firewall.<br/><br/> Vergeleken met traditie firewalltechnologie, hebben WAF's een aantal specifieke functies die interne webservers te tegen bedreigingen beveiligen.
Azure Firewall | Zoals NVA firewall-farms, Firewall van Azure maakt gebruik van een algemene mechanisme voor het beheer en een set beveiligingsregels ter bescherming van workloads die worden gehost op spoke-netwerken en aan toegang tot on-premises netwerken beheren.<br/><br/> De Azure-Firewall heeft ingebouwde schaalbaarheid.
NVA-firewalls | Firewall-farms hebben, zoals Azure Firewall NVA algemene mechanisme voor beheer en een set beveiligingsregels ter bescherming van workloads die worden gehost op spoke-netwerken en aan toegang tot on-premises netwerken beheren.<br/><br/> NVA firewalls kunnen handmatig worden geschaald achter een load balancer.<br/><br/> Hoewel een NVA-firewall minder heeft gespecialiseerde software dan een WAF heeft breder toepassingsbereik om te filteren en elk type van uitgaande en binnenkomende netwerkverkeer te inspecteren.<br/><br/> Als u gebruiken NVA die u vindt deze in de Azure Marketplace wilt.

Wordt u aangeraden één set met Azure-Firewalls (of NVA's) voor verkeer dat afkomstig is op het internet, en één voor verkeer dat afkomstig is on-premises.
- Met behulp van slechts één set firewalls voor zowel is gevaarlijk zijn, omdat er geen beveiligingsperimeter tussen de twee sets netwerkverkeer.
- Met behulp van afzonderlijke firewall lagen vermindert de complexiteit van de beveiligingsregels controleren en het duidelijk is welke regels overeenkomen met die inkomende netwerkaanvraag.

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) met behulp van NVA's in een Azure VNet.

## <a name="next-steps"></a>Volgende stappen 

Bekijk de andere aanbevolen procedures:

- [Aanbevolen procedures](migrate-best-practices-security-management.md) voor beveiliging en beheer na de migratie.
- [Aanbevolen procedures](migrate-best-practices-costs.md) voor kostenbeheer na de migratie.
