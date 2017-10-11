---
title: Azure Network Security | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: TomSh
ms.openlocfilehash: 6ab59dd02391287a1effc0b51502bb7eb90db319
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-network-security"></a>Azure-netwerkbeveiliging

We weten dat beveiliging taak in de cloud en hoe belangrijk is dat u tijdig en informatie over Azure-beveiliging vinden. Een van de beste redenen voor het gebruik van Azure voor uw toepassingen en services is om te profiteren van Azure breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Deze hulpprogramma's en mogelijkheden te maken het mogelijk om veilige oplossingen maken op de Azure-platform.

Microsoft Azure biedt vertrouwelijkheid, integriteit en beschikbaarheid van klantgegevens, maar ook transparante accountability. Als u wilt, kunt u beter inzicht in de verzameling van netwerk-beveiligingsmechanismen geïmplementeerd in Microsoft Azure vanuit het perspectief van de klant van is dit artikel 'Azure Network Security' geschreven om een uitgebreid overzicht van het netwerk beveiligingsmechanismen beschikbaar met Microsoft Azure.

Dit artikel is bedoeld om u te informeren over de breed scala aan netwerk-besturingselementen die u configureren kunt voor het verbeteren van de beveiliging van de oplossingen die u in Azure implementeren. Als u geïnteresseerd bent in Microsoft biedt voor het beveiligen van de infrastructuur van het netwerk van het Azure-platform zelf, Zie de sectie van de Azure-beveiliging in de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/azure-security).

## <a name="azure-platform"></a>Azure-platform

Azure is een openbare cloud service-platform die ondersteuning biedt voor een brede selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten.  Linux-containers kan worden uitgevoerd met Docker-integratie; bouwen van apps met JavaScript, Python, .NET, PHP, Java en Node.js; Build back-ends voor iOS, Android en Windows apparaten. Azure-cloudservices ondersteunen dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u bouwen op of IT-activa te migreren, een openbare cloud serviceprovider, u zijn afhankelijk van die organisatie mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

Azure infrastructuur van de faciliteit is ontworpen voor het hosten van miljoenen klanten tegelijkertijd van toepassingen en biedt een betrouwbare basis waarop bedrijven kunnen voldoen aan de beveiligingsvereisten. Bovendien biedt Azure een uitgebreide verzameling configureerbare beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt de beveiliging om te voldoen aan de unieke vereisten van uw organisatie implementaties aanpassen.

## <a name="abstract"></a>Abstracte

Microsoft openbare cloud-services leveren hyperschaal services en -infrastructuur, bedrijfsniveau mogelijkheden en veel keuzes voor hybride verbindingen. U kunt kiezen voor toegang tot deze services via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. Het Microsoft Azure-platform kunt u naadloos uw infrastructuur in de cloud uitbreiden en bouwen van meerdere lagen architecturen. Bovendien kunt derden uitgebreide mogelijkheden inschakelen door het aanbieden van beveiligingsservices en virtuele apparaten.

Azure netwerkservices maximaliseren flexibiliteit, beschikbaarheid, tolerantie, beveiliging en integriteit standaard. Dit technisch document worden details weergegeven over de netwerkfuncties van Azure en informatie over hoe klanten de systeemeigen beveiligingsfuncties van Azure gebruiken kunnen voor het beschermen van hun informatie-assets.

De beoogde doelgroepen voor dit technisch document opnemen:

- Technische managers, netwerkbeheerders en ontwikkelaars die op zoek bent naar beveiligingsoplossingen beschikbaar en ondersteund in Azure.

-   Kleine en middelgrote ondernemingen of proces leidinggevenden willen ophalen een overzicht van de Azure netwerktechnologieën en services die relevant voor discussies over netwerkbeveiliging in de Azure openbare cloud zijn.

## <a name="azure-networking-big-picture"></a>Azure networking grote afbeelding
Microsoft Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen bronnen in Azure, tussen on-premises en Azure gehoste bronnen, en naar en van het Internet en Azure.

![Azure Networking grote afbeelding](media/azure-network-security/azure-network-security-fig-1.png)

De [Azure netwerkinfrastructuur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) kunt u veilig Azure-resources met elkaar te verbinden met virtuele netwerken (vnet's). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van het Azure-cloud-netwerk toegewezen aan uw abonnement. U kunt de VNets verbinden met uw on-premises netwerken.

Azure ondersteunt toegewezen WAN-koppeling connectiviteit voor uw on-premises netwerk en een Azure-netwerk met [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). De koppeling tussen Azure en uw site maakt gebruik van een speciale verbinding die u niet via het openbare Internet gaat. Als uw Azure-toepassing wordt uitgevoerd in meerdere datacenters, kunt u [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) op route-aanvragen van gebruikers op intelligente wijze over exemplaren van de toepassing. Ook kunt u verkeer routeren voor services in Azure niet worden uitgevoerd als ze toegankelijk vanaf Internet zijn.

## <a name="enterprise-view-of-azure-networking-components"></a>Enterprise-weergave van Azure netwerkonderdelen
Azure heeft veel netwerkonderdelen die relevant voor netwerk beveiliging discussies zijn. We beschrijven deze netwerkonderdelen en zich richten op de beveiligingsproblemen die hieraan zijn gekoppeld.

> [!Note]
> Niet alle aspecten van de Azure-netwerken worden beschreven: bespreken we alleen de belangrijke zich in plannen en ontwerpen van een veilige netwerkinfrastructuur rond uw services en toepassingen die u in Azure implementeert.

In dit artikel niet behandeld de volgende Azure enterprise netwerkmogelijkheden:

-   Netwerkverbinding

-   Hybride verbindingen

-   Beveiligingsmechanismen

-   Netwerkvalidatie van het

### <a name="basic-network-connectivity"></a>Netwerkverbinding

De [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) service kunt u veilig Azure-resources met elkaar te verbinden met virtuele netwerken (VNet). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van de infrastructuur van de Azure-netwerk toegewezen aan uw abonnement. U kunt ook verbinding maken VNets met elkaar en aan uw on-premises netwerken via een site-naar-site VPN en dedicated [WAN-koppelingen](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Netwerkverbinding](media/azure-network-security/azure-network-security-fig-2.png)

Denk eraan dat u virtuele machines voor hostservers in Azure gebruikt, is de vraag hoe deze VMs verbinding maken met een netwerk. Het antwoord is dat virtuele machines verbinding met maken een [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Virtuele netwerken van Azure lijken op de virtuele netwerken op lokale gebruiken met uw eigen virtualisatieoplossingen platform, zoals Microsoft Hyper-V- of VMware.

#### <a name="intra-vnet-connectivity"></a>Intra-VNet-connectiviteit

Kunt u VNets met elkaar, het inschakelen van de resources die zijn verbonden met een VNet-naar-tussen VNets met elkaar communiceren. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** kunnen resources die zijn verbonden met andere Azure VNets binnen dezelfde Azure-locatie met elkaar communiceren. De bandbreedte en de latentie tussen het VNet is hetzelfde als wanneer de resources zijn verbonden met hetzelfde VNet is. Lees voor meer informatie over de peering, [virtuele netwerk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Peering](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet-naar-VNet-verbinding:** kunnen resources die zijn verbonden met andere Azure-VNet binnen de dezelfde of verschillende Azure-locaties. In tegenstelling tot de peering, is bandbreedte tussen VNets beperkt, omdat het verkeer door een Azure VPN-Gateway moet lopen.

![VNet-naar-VNet-verbinding](media/azure-network-security/azure-network-security-fig-4.png)


Lees meer informatie over het VNets verbinden met een VNet-naar-VNet-verbinding, de [configureren van een VNet-naar-VNet-verbinding artikel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Mogelijkheden voor virtuele Azure-netwerk:

Zoals u zien kunt, biedt een virtueel netwerk van Azure virtuele machines verbinding maken met het netwerk, zodat ze verbinding met andere netwerkbronnen op een veilige wijze maken kunnen. Basic-connectiviteit is echter slechts een begin. De volgende mogelijkheden van de service Azure Virtual Network weergeven security kenmerken van het virtuele netwerk van Azure:

-   Isolatie

-   Internetconnectiviteit

-   Azure-resource-connectiviteit

-   VNet-connectiviteit

-   On-premises connectiviteit

-   Filteren van verkeer

-   Routering

**Isolatie**

Vnet's zijn [geïsoleerde](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) van elkaar. U kunt afzonderlijke VNets voor ontwikkeling, testen en productie die gebruikmaken van dezelfde maken [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blokken adres. Als u daarentegen, kunt u meerdere VNets die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden. U kunt een VNet segmenteren in meerdere subnetten.

Azure biedt interne naamomzetting voor VM's en [Cloudservices](https://azure.microsoft.com/services/cloud-services/) rolinstanties verbonden met een VNet. U kunt desgewenst een VNet voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.

U kunt meerdere VNets binnen elke Azure implementeren [abonnement](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) en Azure [regio](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Elke VNet is geïsoleerd van andere vnet's. U kunt voor elk VNet:

-   Geef een aangepaste persoonlijke IP-adresruimte met behulp van openbare en persoonlijke (RFC 1918)-adressen. Azure wordt toegewezen resources verbonden met het VNet een particulier IP-adres van de adresruimte, u toewijzen.

-   Het VNet segmenteren in een of meer subnetten en een deel van de VNet-adresruimte voor elk subnet toegewezen.

-   Azure verschafte naamomzetting gebruiken of geef uw eigen DNS-server voor gebruik door resources die zijn verbonden met een VNet. Lees voor meer informatie over naamomzetting in vnet's, de [naamomzetting voor VM's en Cloudservices](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**Verbinding met internet**

Alle [Azure virtuele Machines (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) en Cloud Services-rolexemplaren die is verbonden met een VNet hebben toegang tot het Internet, standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig. (VM) en Cloud Services-rolexemplaren die is verbonden met een VNet hebben toegang tot het Internet, standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig.

Uitgaande verbinding met Internet hebben alle resources die zijn verbonden met een VNet standaard. Het privé IP-adres van de resource is Bronnetwerk adres vertaald (snat omzetten) naar een openbare IP-adres door de Azure-infrastructuur. U kunt de standaard-connectiviteit wijzigen door het implementeren van aangepaste Routering en het filteren van verkeer. Lees voor meer informatie over uitgaande internetverbinding, de [inzicht in uitgaande verbindingen in Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

Inkomende communicatie voor Azure-resources van het Internet of om te communiceren met Internet, zonder snat omzetten uitgaand, moet een resource een openbaar IP-adres worden toegewezen. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Azure-resource-connectiviteit**

[Azure-resources](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) zoals Cloudservices en virtuele machines kunnen worden verbonden met hetzelfde VNet. De resources kunnen verbinden met elkaar met behulp van privé IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, VNets en on-premises netwerken, zodat u niet hoeft te configureren en beheren van routes.

U kunt verschillende Azure-resources verbinden met een VNet, zoals virtuele Machines (VM), Cloud Services-App Service-omgevingen en virtuele-Machineschaalsets. Virtuele machines verbinding maken met een subnet binnen een VNet via een netwerkinterface (NIC). Lees voor meer informatie over NIC's, de [netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**VNet-connectiviteit**

[VNets](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kunnen worden verbonden met elkaar, waardoor bronnen die zijn verbonden met een VNet om te communiceren met alle bronnen op eventuele andere VNet.

Kunt u VNets met elkaar, het inschakelen van de resources die zijn verbonden met een VNet-naar-tussen VNets met elkaar communiceren. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** kunnen resources die zijn verbonden met andere Azure VNets binnen dezelfde Azure-locatie met elkaar communiceren. De bandbreedte en de latentie tussen de VNets is hetzelfde als wanneer de resources zijn verbonden met de dezelfde VNet.To meer informatie over peering, gebruikt u de [virtuele netwerk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-naar-VNet-verbinding:** kunnen resources die zijn verbonden met andere Azure-VNet binnen de dezelfde of verschillende Azure-locaties. In tegenstelling tot de peering, is bandbreedte tussen VNets beperkt, omdat het verkeer door een Azure VPN-Gateway moet lopen. Voor meer informatie over het VNets verbinden met een VNet-naar-VNet-verbinding. Lees voor meer informatie de [een VNet-naar-VNet-verbinding configureren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**On-premises connectiviteit**

Vnet's kunnen worden verbonden met [lokale](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) netwerken via persoonlijke netwerkverbindingen tussen uw netwerk en Azure of een site-naar-site VPN-verbinding via Internet.

U kunt uw on-premises netwerk verbinden met een VNet met elke combinatie van de volgende opties:

- **Punt-naar-site virtueel particulier netwerk (VPN):** tot stand gebracht tussen één PC verbonden met uw netwerk en het VNet. Dit verbindingstype is handig als u alleen aan de slag met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk. De verbinding wordt de SSTP-protocol gebruikt voor gecodeerde communicatie via Internet tussen de PC en het VNet. De latentie voor een punt-naar-site VPN-verbinding is onvoorspelbare omdat het verkeer over het Internet wordt verzonden.

- **Site-naar-site-VPN:** tot stand gebracht tussen uw VPN-apparaat en een Azure VPN-Gateway. Dit verbindingstype kunt een on-premises-resource die geeft u toestemming voor toegang tot een VNet. De verbinding is een VPN IPsec/IKE waarmee gecodeerde communicatie via Internet tussen uw on-premises-apparaat en de Azure VPN-gateway. De latentie voor een site-naar-site-verbinding is onvoorspelbare omdat het verkeer over het Internet wordt verzonden.

- **Azure ExpressRoute:** tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer niet via Internet verloopt. De latentie voor een ExpressRoute-verbinding is voorspelbaar omdat verkeer niet via Internet verloopt. Lees voor meer informatie over de vorige verbindingsopties, de [gatewayverbindingsdiagrammen topologie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Verkeer filteren**

Virtuele machine en Cloud Services-rolexemplaren [netwerkverkeer](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kunnen binnenkomende en uitgaande worden gefilterd op de bron-IP-adres en poort, doel-IP-adres en poort en protocol.

U kunt filteren netwerkverkeer tussen subnetten op met behulp van een of beide van de volgende opties:

- **Netwerkbeveiligingsgroepen (NSG):** elke NSG kan meerdere binnenkomende en uitgaande beveiligingsregels voor verbindingen waarmee u verkeer filteren door de bron en doel-IP-adres, poort en protocol bevatten. U kunt een NSG toepassen op elke NIC op een virtuele machine. U kunt ook een NSG toepassen op het subnet een NIC of andere Azure-resource is verbonden met. Lees voor meer informatie over Nsg de [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Virtuele netwerkapparaten:** een virtueel netwerk-apparaat is een virtuele machine uitvoeren van software waarmee een netwerkfunctie, zoals een firewall. Een lijst met beschikbare NVAs weergeven in Azure Marketplace. NVAs zijn ook beschikbaar met WAN-optimalisatie en andere netwerkapparaten verkeer functies. NVAs worden doorgaans gebruikt voor de gebruiker gedefinieerde of BGP-routes. U kunt ook een NVA gebruiken voor het filteren van verkeer tussen VNets.

**Routering**

U kunt eventueel Azure standaard routering met BGP-routes via een netwerkgateway of configureren van uw eigen routes overschrijven.

Azure maakt routetabellen waarmee bronnen die zijn verbonden met een enkel subnet in een VNet om te communiceren met elkaar, standaard. U kunt een of beide van de volgende opties voor het onderdrukken van de Azure maakt standaardroutes implementeren:

- **Gebruiker gedefinieerde routes:** kunt u aangepaste routetabellen met routes die bepalen waar verkeer wordt doorgestuurd naar voor elk subnet. Lees voor meer informatie over de gebruiker gedefinieerde routes, de [gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP-routes:** als u uw VNet verbinding met uw on-premises netwerk via een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw vnet's.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Hybride verbinding met internet: verbinding maken met een on-premises netwerk
U kunt uw on-premises netwerk verbinden met een VNet met elke combinatie van de volgende opties:

-   Internetconnectiviteit

-   Punt-naar-site VPN (P2S VPN)

-   Site-naar-Site VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Verbinding met Internet

Als de naam ervan wordt voorgesteld, toegankelijk verbinding met Internet uw werkbelastingen vanaf Internet, doordat u verschillende openbare eindpunten werkbelastingen die live binnen het virtuele netwerk beschikbaar. Deze werkbelastingen kunnen worden blootgesteld met [Internet gerichte Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) of gewoon een openbaar IP-adres toewijzen aan de virtuele machine. Op deze manier wordt het mogelijk dat alles op Internet om te kunnen bereiken dat de virtuele machine, een hostfirewall opgegeven [netwerkbeveiligingsgroepen (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), en [zelfgedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) die u wilt toestaan gebeuren.

In dit scenario kan u een toepassing die moet worden openbare met Internet en kunnen tot stand te brengen van een willekeurige plaats of op een specifieke locatie, afhankelijk van de configuratie van de werkbelasting van uw tonen.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Punt-naar-Site VPN- of Site-naar-Site VPN
Deze twee worden onderverdeeld in dezelfde categorie. Beide moeten uw VNet-naar-een VPN-Gateway hebt en u verbinding mee kan maken met behulp van een VPN-Client voor uw werkstation als onderdeel van de [punt-naar-Site-configuratie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) of u kunt uw on-premises configureren [VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)kunnen een site-naar-site VPN wordt beëindigd. Op deze manier kunnen on-premises apparaten aansluiten op resources binnen het VNet.

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol).

![Punt-naar-Site VPN](media/azure-network-security/azure-network-security-fig-5.png)

Punt-naar-Site-verbindingen zijn handig als u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of op een conferentie center, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk.

Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer. P2S wordt daarom niet aanbevolen manier verbinding maken met Azure geval u een permanente verbinding van veel on-premises apparaten en computers met uw Azure-netwerk moet.

![Site-naar-Site VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Zie voor meer informatie over punt-naar-Site-verbindingen de [punt-naar-Site VA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk.

Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Deze verbinding vindt plaats via het Internet en u kunt informatie in een versleutelde verbinding tussen uw netwerk en Azure 'tunnel'. Site-naar-site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. Tunnel-versleuteling wordt uitgevoerd met [IPsec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

Site-naar-site VPN is een technologie voor het vertrouwde, betrouwbare en tot stand gebracht, verkeer binnen de tunnel Internet passeren. Bovendien bandbreedte relatief beperkt tot een maximum van ongeveer 200 Mbps.

Als u een uitzonderlijke niveau van beveiliging of prestaties voor uw cross-premises verbindingen vereist, wordt u aangeraden dat u Azure ExpressRoute voor uw cross-premises-connectiviteit gebruiken. ExpressRoute is een speciale WAN koppeling tussen uw on-premises locatie of een Exchange-hostingprovider. Omdat dit een telco verbinding, worden uw gegevens niet via Internet worden verzonden en daarom geen toegang heeft tot de mogelijke risico's in de communicatie via Internet.

> [!Note]
> Zie voor meer informatie over VPN-gateways over [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Specifieke WAN-verbinding
Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Azure uitbreiden via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider.

ExpressRoute-verbindingen gaan niet via het openbare internet. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.

![ Specifieke WAN-verbinding](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> Zie voor informatie over verbinding maken tussen uw netwerk en Microsoft met behulp van ExpressRoute, [ExpressRoute connectiviteitsmodellen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) en [technisch overzicht van ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Als met de site-naar-site VPN-opties kunt ExpressRoute u ook verbinding maken met bronnen die niet noodzakelijkerwijs in slechts één VNet. In feite is afhankelijk van de SKU, kunt u 10 VNets. Als u hebt de [premium-invoegtoepassing](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), verbindingen met maximaal 100 VNets moeten zijn, afhankelijk van de bandbreedte. Lees voor meer informatie over wat dit soort verbindingen bekijken, zoals [gatewayverbindingsdiagrammen topologie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Beveiligingsmechanismen
Een virtueel netwerk van Azure biedt een veilige, logische netwerk dat is geïsoleerd van andere virtuele netwerken en veel beveiligingsmechanismen die u voor uw on-premises netwerken ondersteunt. Klanten maken hun eigen structuur met behulp van: subnetten: ze hun eigen persoonlijke IP-adresbereik gebruiken, routetabellen configureren, netwerkbeveiligingsgroepen, toegangsbeheerlijsten (ACL's), gateways en virtuele apparaten naar hun werkbelasting uitvoeren in de cloud.

Hier volgen beveiligingsmechanismen die kunt u op uw virtuele Azure-netwerken gebruiken:

-   Netwerk-toegangsbeheer

-   Gebruiker gedefinieerde Routes

-   Beveiliging van netwerkapparaat

-   Application Gateway

-   Azure-Web Application Firewall

-   Network beschikbaarheid Control

#### <a name="network-access-controls"></a>Netwerk-toegangsbeheer
Terwijl de virtuele Azure-netwerk (VNet) de basis van Azure model voor netwerken vormt en isolatie en bescherming biedt, de [Netwerkbeveiligingsgroep groepen (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) zijn de belangrijkste hulpprogramma voor het afdwingen van en het beheren van regels voor netwerkverkeer op de het netwerkniveau van het.

![ Netwerk-toegangsbeheer](media/azure-network-security/azure-network-security-fig-8.png)


U kunt toegang door toestaan of weigeren van communicatie tussen de werkbelastingen binnen een virtueel netwerk, van systemen op klantnetwerken via cross-premises-connectiviteit te controleren of directe communicatie met Internet.

In het diagram zowel VNets en nsg's zich bevinden in een bepaalde laag in de Azure algemene beveiliging stack, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt beveiligingsgrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk maken.

Nsg's met een 5-tuple kunt u verkeer (en worden gebruikt in de regels die u voor het NSG configureert):

-   [Bron en doel-IP-adres](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Bron- en doelserver poort](https://technet.microsoft.com/library/dd197515)

-   Protocol: [Transmission controleprotocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) of [User Datagram Protocol (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Dit betekent dat u kunt de toegang tussen één VM en een groep VM's of een enkele virtuele machine naar een andere één virtuele machine, of tussen volledige subnetten. Houd er rekening mee dat dit eenvoudige filterregels voor pakketten, is niet volledig pakketinspecties opnieuw. Er is geen protocol validatie of het niveau van de id's of de mogelijkheid van de IP-Adressen in een Netwerkbeveiligingsgroep.

Een NSG wordt geleverd met enkele ingebouwde regels waarmee u houden moet rekening. Dit zijn:

-   **Alle verkeer binnen een specifieke virtuele netwerk:** alle VM's op hetzelfde virtuele Azure-netwerk met elkaar communiceren.

-   **Azure load balancing inkomende toestaan:** met deze regel kunnen verkeer van het adres van een bronserver naar een doeladres voor de Azure load balancer.

-   **Alle binnenkomende weigeren:** met deze regel blokkeert al het verkeer bron van het Internet die u expliciet zijn toegestaan.

-   **Al het verkeer uitgaand verkeer naar Internet toestaan:** met deze regel kunnen virtuele machines verbindingen met het Internet te initiëren. Als u niet dat deze verbindingen geïnitieerd wilt, moet u een regel maken om te blokkeren die verbindingen of af te dwingen geforceerde tunneling.

#### <a name="system-routes-and-user-defined-routes"></a>Systeemroutes en door gebruiker gedefinieerde routes

Wanneer u virtuele machines (VM's) aan een virtueel netwerk (VNet) in Azure toevoegen, ziet u dat de virtuele machines met elkaar communiceren via het netwerk automatisch zijn. U hoeft geen gateway op te geven, ook niet als de virtuele machines tot verschillende subnetten behoren.

Hetzelfde geldt voor de communicatie tussen VM's en internet. Als er een hybride verbinding is tussen Azure en uw eigen datacentrum, is er zelfs communicatie met uw on-premises netwerk mogelijk.

![Systeemroutes](media/azure-network-security/azure-network-security-fig-9.png)

Deze communicatiestroom is mogelijk omdat de IP-verkeersstromen in Azure wordt gedefinieerd met behulp van een reeks systeemroutes. Systeemroutes bepalen de communicatiestroom in de volgende scenario's:

-   Binnen een en hetzelfde subnet.

-   Van het ene naar het andere subnet binnen een VNet.

-   Van virtuele machines naar internet.

-   Van het ene naar het andere VNet via een VPN-gateway.

-   Van een VNet naar het andere VNet via VNet-Peering ([Service Chaining](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Van een VNet naar uw on-premises netwerk via een VPN-gateway.

Veel bedrijven hebben strikte beveiliging en nalevingsvereisten waarvoor lokale inspectie van alle netwerkpakketten af te dwingen specifieke beleidsregels. Azure biedt een mechanisme aangeroepen [geforceerde tunneling](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) die verkeer tussen VM's met on-premises gerouteerd door een aangepaste route maken of door [Border Gateway Protocol (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) advertenties via ExpressRoute of VPN.

Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes (UDR). Intranetwebproxyserver verkeer omleidt naar een on-premises site wordt uitgedrukt als een standaardroute voor de Azure VPN-gateway.

Het volgende gedeelte bevat de huidige beperking van de routering tabel en routes voor een Azure-netwerk:

-   Elk virtueel netwerksubnet heeft een ingebouwd systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:

 -  **Lokale VNet routes:** rechtstreeks aan de doel-virtuele machines in hetzelfde virtuele netwerk

 - **Op de lokale routes:** naar de Azure VPN-gateway

 -  **Standaardroute:** rechtstreeks met het Internet. Pakketten die bestemd zijn voor de persoonlijke IP-adressen niet wordt gedekt door de vorige twee routes worden verwijderd.

-   U kunt met het uitbrengen van de gebruiker gedefinieerde routes, een routeringstabel om toe te voegen een standaardroute maken en vervolgens de routeringstabel koppelen aan uw VNet subnet moet het inschakelen van geforceerde tunneling op deze subnetten.

-   U moet een "standaardsite instellen ' tussen de cross-premises lokale sites verbonden met het virtuele netwerk.

-   Geforceerde tunneling moet worden gekoppeld aan een VNet met een VPN-gateway voor dynamische routering (geen statische gateway genoemd).

- ExpressRoute geforceerde tunneling via dit mechanisme niet is geconfigureerd, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies.

> [!Note]
> Zie voor meer informatie de [ExpressRoute-documentatie](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

#### <a name="network-security-appliances"></a>Beveiliging van netwerkapparaten
Terwijl Netwerkbeveiligingsgroepen en door de gebruiker gedefinieerde Routes een zekere mate van netwerkbeveiliging op de netwerk- en lagen van bieden kunt de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), er gaan worden als situaties waarbij moet of wilt u beveiliging op inschakelen hogere niveaus van de netwerkstack. In dergelijke situaties wordt aangeraden dat u een virtueel netwerk beveiligingsapparaten geleverd door Azure partners implementeert.

![Beveiliging van netwerkapparaten](./media/azure-network-security/azure-network-security-fig-10.png)

Azure-netwerk beveiligingsapparaten VNet beveiligings- en netwerkfuncties te verbeteren, en ze zijn beschikbaar via meerdere leveranciers via de [Azure Marketplace](https://azuremarketplace.microsoft.com). Deze virtuele beveiligingsapparaten kunnen worden geïmplementeerd om op te geven:

-   Maximaal beschikbare firewalls

-   Inbraakdetectie voorkomen

-   Inbraakdetectie

-   Web application firewalls (WAFs)

-   WAN-optimalisatie

-   Routering

-   Taakverdeling

-   VPN

-   Certificaatbeheer

-   Active Directory

-   Multifactor-verificatie

#### <a name="application-gateway"></a>Toepassingsgateway

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) is een toegewezen virtueel apparaat waarmee een toepassing levering controller (ADC) als een service.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Toepassingsgateway kunt u web-farm prestaties en beschikbaarheid optimaliseren door het offloaden van CPU intensief SSL-beëindiging aan de toepassingsgateway (SSL-offloading). Het bevat ook een andere laag 7 routering mogelijkheden, waaronder:

-   Round-robin distributie van het binnenkomende verkeer

-   Sessie cookies gebaseerde affiniteit

-   URL-pad gebaseerde routering

-   Mogelijkheid voor het hosten van meerdere websites achter één Application Gateway


Een [web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) is ook beschikbaar als onderdeel van de toepassingsgateway. Dit biedt beveiliging voor webtoepassingen van algemene web beveiligingsproblemen en aanvallen. Application Gateway kan worden geconfigureerd als een Internetgericht gateway, interne enige gateway of een combinatie van beide.

Application Gateway WAF kan worden uitgevoerd in de modus voor detectie of te voorkomen. Er is een algemene gebruiksvoorbeeld voor beheerders om uit te voeren in de modus van de detectie van verkeer voor schadelijke patronen in acht nemen. Zodra een potentiële aanvallen zijn gedetecteerd, schakelen naar de modus voor preventie verdachte inkomend verkeer blokkeert.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Bovendien Application Gateway WAF helpt u bij het bewaken van webtoepassingen tegen aanvallen met een realtime WAF logboek die is geïntegreerd met [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) en [Azure Security Center](https://azure.microsoft.com/services/security-center/) WAF waarschuwingen bijhouden en trends gemakkelijk bewaken.

Het opgemaakte JSON-logboek gaat u rechtstreeks naar de storage-account van de klant. U hebt volledige controle over deze logboeken en uw eigen bewaarbeleidsregels kunt toepassen.

U kunt ook deze logboeken opnemen in uw eigen analytics-systeem met [Azure Log integratie](https://aka.ms/AzLog). WAF Logboeken ook worden geïntegreerd met [Operations Management Suite (OMS)](https://www.microsoft.com/cloud-platform/operations-management-suite) zodat u de logboekanalyse OMS kunt geavanceerde fijnmazig query's uitvoeren.

#### <a name="azure-web-application-firewall-waf"></a>Azure-web application firewall (WAF)

Webtoepassingen zijn steeds meer doelen van aanvallen die gebruikmaken van algemene bekende beveiligingsproblemen, zoals SQL-injectie, cross-site scripting aanvallen en andere aanvallen die worden weergegeven in de [OWASP top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Zo wordt voorkomen dat dergelijke aanvallen in de toepassing vereist strengere onderhoud, patchen en controle op meerdere lagen van de Toepassingstopologie.

 ![Azure-Web Application Firewall (WAF)](./media/azure-network-security/azure-network-security-fig-13.png)

Een gecentraliseerde [web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) kunt beschermen tegen aanvallen via Internet en beveiligingsbeheer zonder eventuele wijzigingen in de toepassing.

Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

#### <a name="network-availability-controls"></a>Beschikbaarheid netwerkfuncties

Er zijn verschillende opties voor het distribueren van netwerkverkeer met behulp van Microsoft Azure. Deze opties werken verschillend. Ze hebben een eigen functieset en ondersteunen verschillende scenario's. Ze kunnen elk afzonderlijk worden gebruikt, maar ook worden gecombineerd.

Hieronder vindt u de besturingselementen van de beschikbaarheid van netwerk:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure Load balancer**

Hoge beschikbaarheid en netwerk prestaties leveren aan uw toepassingen. Het is een laag 4 (TCP, UDP) load balancer die binnenkomend verkeer over orde exemplaren van services gedefinieerd in een set met gelijke taakverdeling distribueert.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Azure Load Balancer kan worden geconfigureerd voor:

-   Binnenkomend internetverkeer saldo aan virtuele machines worden geladen. Deze configuratie wordt ook wel [internetgerichte taakverdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Load balance verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in de cloud-services of tussen virtuele machines in een virtueel netwerk met cross-premises en lokale computers. Deze configuratie wordt ook wel [interne load balancing](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Externe verkeer naar een specifieke virtuele machine doorsturen.

Alle resources in de cloud moeten een openbaar IP-adres aan bereikbaar zijn vanaf Internet. De cloudinfrastructuur in Azure maakt gebruik van niet-routeerbare IP-adressen voor de bronnen. Azure maakt gebruik van NAT (Netwerkadresomzetting) met openbare IP-adressen om te communiceren met Internet.

 **Toepassingsgateway**

 Toepassingsgateway werkt op de toepassingslaag (laag 7 in de OSI-netwerkstack verwijzing). Deze service fungeert als een omgekeerde proxyservice, beëindigt de clientverbinding en stuurt aanvragen door naar back-endeindpunten.

 **Het Traffic manager**

Microsoft Azure Traffic Manager kunt u bepalen van de distributie van gebruikersverkeer voor service-eindpunten in verschillende datacenters. Service-eindpunten die worden ondersteund door Traffic Manager bevatten Azure virtuele machines, Web-Apps en cloudservices. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn.

Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients naar de meest geschikte eindpunt op basis van een [verkeersroutering methode](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) en de status van de eindpunten. Traffic Manager biedt een reeks verkeersroutering methoden aan de behoeften van verschillende groepen van toepassingen, eindpunt health [bewaking](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), en automatische failover. Traffic Manager is mislukt, met inbegrip van het uitvallen van een volledige Azure-regio tegen.

Azure Traffic Manager kunt u het beheer van de distributie van verkeer tussen de eindpunten van uw toepassing. Een eindpunt is een internetgerichte service die wordt gehost binnen of buiten Azure.

Traffic Manager biedt twee belangrijke voordelen:

-   Distributie van verkeer volgens een van de [verkeersroutering methoden](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Doorlopende bewaking van de status van endpoint](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) en automatische failover wanneer eindpunten mislukken.

Wanneer een client probeert verbinding maken met een service, moet deze eerst de DNS-naam van de service leiden naar een IP-adres. De client maakt vervolgens verbinding met dat IP-adres voor toegang tot de service. Traffic Manager gebruikt DNS om clients omleiden naar een specifieke service-eindpunten op basis van de regels van de methode verkeer routering. Clients rechtstreeks verbinding gemaakt met het geselecteerde eindpunt. Traffic Manager is niet een proxy of gateway. Traffic Manager is niet zichtbaar voor het verkeer tussen de client en de service wordt doorgegeven.

### <a name="azure-network-validation"></a>Validatie van de Azure-netwerk

Validatie van de Azure-netwerk is om ervoor te zorgen dat het Azure-netwerk wordt uitgevoerd als deze is geconfigureerd en validatie kan worden gedaan met behulp van de services en functies die beschikbaar zijn voor het bewaken van het netwerk. Met Azure-netwerk-Watcher u toegang hebt tot een breed spectrum van logboekregistratie en diagnostische mogelijkheden waarmee u met insights om uw netwerkervaring voor prestaties en status te begrijpen. Deze mogelijkheden zijn toegankelijk via de Portal, Power Shell, CLI, Rest-API en SDK.

Azure bedrijfsbeveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure. Azure operationele beveiliging is gebaseerd op een framework dat de kennis die is opgedaan met een verschillende mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Centre opgenomen , en grondige kennis van de cyberbeveiliging beveiliging threat Liggend.

-   [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure-netwerk-watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure resourcemanager

De mensen en processen die Microsoft Azure werken zijn mogelijk de belangrijkste beveiligingsfunctie van het platform. Deze sectie beschrijft de functies van de globale datacenterinfrastructuur van Microsoft die helpen verbeteren en onderhouden van beveiliging, bedrijfscontinuïteit en privacy.

De infrastructuur voor uw toepassing bestaat meestal uit van veel onderdelen, zoals een virtuele machine, storage-account en virtueel netwerk of een web-app, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. Met Azure Resource Manager kunt u met de resources als groep in uw oplossing werken.

U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

**De voordelen van het gebruik van Resource Manager**

Resource Manager biedt diverse voordelen:

-   U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

-   U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

-   U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

-   U kunt de afhankelijkheden tussen resources, zodat deze zijn geïmplementeerd in de juiste volgorde definiëren.

-   U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.

-   U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

-   U kunt facturering voor uw organisatie verduidelijken door kosten voor een groep resources delen tag weer te geven.

> [!Note]
> Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u het eerdere implementatiemodel hebt gebruikt en meer te weten wilt komen over de wijzigingen, leest u [Resource Manager-implementatie en klassieke implementatie begrijpen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Azure-netwerk logboekregistratie en controle

Azure biedt veel hulpprogramma's om te controleren, detecteren, voorkomen van en reageren op beveiligingsgebeurtenissen netwerk. Enkele van de meest krachtige hulpprogramma's die u in dit gedeelte:

-   Network Watcher

-   Resource niveau Netwerkbewaking

-   Log Analytics

### <a name="network-watcher"></a>Netwerk-watcher

[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -bewaking op basis van een Scenario met de functies in netwerk-Watcher is opgegeven. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

 ![Network Watcher](./media/azure-network-security/azure-network-security-fig-15.png)

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

#### <a name="topology"></a>Topologie

[Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) retourneert een grafiek van netwerkbronnen in een virtueel netwerk. De grafiek wordt de onderlinge verbinding tussen de bronnen voor de end-to-end-netwerkverbinding. In de portal retourneert topologie resourceobjecten op aan de hand van de basis van het virtuele netwerk. De relaties zijn afgebeeld met lijnen tussen de bronnen buiten het gebied van netwerk-Watcher, zelfs als in de groep niet weergegeven. De resources die worden geretourneerd in de portal weergave zijn een subset van de netwerkonderdelen die diagram worden weergegeven aan. De volledige lijst van netwerkresources wilt bekijken, kunt u [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) of [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Resources worden geretourneerd worden de verbinding tussen ze gemodelleerd onder twee-relaties.

- **Containment** -virtueel netwerk bevat een Subnet, waardoor een NIC. bevat

- **Gekoppeld** -A NIC is gekoppeld aan een virtuele machine.

#### <a name="variable-packet-capture"></a>Variabele pakketopname

Netwerk-Watcher [variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) kunt u pakket vastleggen sessies om bij te houden van verkeer van en naar een virtuele machine maken. Helpt bij het vastleggen van pakket op te sporen netwerk afwijkingen beide reactief en proactivity. Andere toepassingen zijn onder andere het verzamelen van netwerkstatistieken, krijgt informatie over het netwerk beveiligingsrisico's voor foutopsporing van client-servercommunicaties en nog veel meer.

Pakketopname is een uitbreiding van virtuele machine die op afstand via het netwerk-Watcher wordt gestart. Deze mogelijkheid kan de werkbelasting van een pakketopname handmatig worden uitgevoerd op de gewenste virtuele machine, die kostbare tijd bespaart vergemakkelijken. Pakketopname kan worden geactiveerd via de portal, PowerShell, CLI of REST-API. Er is een voorbeeld van hoe pakketopname kan worden geactiveerd met waarschuwingen van de virtuele Machine.

#### <a name="ip-flow-verify"></a>IP-stroom controleren

[IP-stromen controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) controleert of een pakket wordt toegestaan of geweigerd naar of van een virtuele machine op basis van 5-tuple informatie. Deze informatie bestaat uit de richting, protocol, lokaal IP-, externe IP-, lokale poort en externe poort. Als het pakket wordt geweigerd door een beveiligingsgroep, wordt de naam van de regel die het pakket geweigerd geretourneerd. Terwijl de IP-bron- of doelserver kan worden gekozen, is deze functie kan beheerders een Analyseer snel problemen met de netwerkverbinding van of met het internet en van of naar de on-premises omgeving.

IP-stromen controleren is bedoeld voor een netwerkinterface van een virtuele machine. Netwerkverkeer is geverifieerd op basis van de geconfigureerde instellingen naar of van de netwerkinterface. Deze functie is handig bij de bevestiging als een regel in een Netwerkbeveiligingsgroep toegangsroutes en uitgaande verkeer naar of van een virtuele machine wordt geblokkeerd.

#### <a name="next-hop"></a>Volgende hop

Bepaalt de [volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) voor pakketten in de Fabric Azure-netwerk worden gerouteerd, zodat u voor het vaststellen van een gebruiker gedefinieerde routes geconfigureerd. Verkeer van een virtuele machine wordt verzonden naar een bestemming op basis van de effectieve routes die zijn gekoppeld aan een NIC. Volgende hop opgehaald uit het volgende hoptype en IP-adres van een pakket vanuit een specifieke virtuele machine en de NIC. Dit helpt te bepalen of het pakket wordt omgeleid naar de bestemming of is het verkeer wordt zwarte gaan.

Volgende hop retourneert ook de routetabel die zijn gekoppeld aan de volgende hop. Tijdens het opvragen van een volgende hop als de route is gedefinieerd als een gebruiker gedefinieerde route worden die route geretourneerd. Anders retourneert de volgende hop 'Systeemroute'.

#### <a name="security-group-view"></a>beveiliging groep weergeven

Hiermee haalt u de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine. Netwerkbeveiligingsgroepen zijn gekoppeld op het subnetniveau van een of een NIC-niveau. Als gekoppeld op het subnetniveau van een, geldt dit voor alle VM-instanties in het subnet. Netwerk [beveiligingsgroep weergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) retourneert de geconfigureerde nsg's en regels die zijn gekoppeld aan een NIC en het subnetmasker niveau hebben voor een virtuele machine biedt meer informatie over de configuratie. Bovendien worden de regels voor een effectieve beveiligingsmethode voor elk van de NIC's in een virtuele machine geretourneerd. Met behulp van de Netwerkbeveiligingsgroep weergeven, kunt u een virtuele machine op netwerk beveiligingsproblemen zoals open poorten beoordelen. U kunt ook valideren als de Netwerkbeveiligingsgroep werkt zoals verwacht op basis van een [vergelijking tussen de geconfigureerde en de regels voor een effectieve beveiligingsmethode](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>NSG-Flow-logboekregistratie

 Stroom logboeken voor Netwerkbeveiligingsgroepen kunt u de logboeken die betrekking hebben op het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep opnemen. De stroom wordt gedefinieerd door de gegevens van een 5-tuple: bron-IP, doel-IP, bronpoort, doelpoort en -Protocol.

[Netwerkbeveiligingsgroep stroom logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Virtuele netwerkgateway en verbinding probleemoplossing

Netwerk-Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van uw netwerkbronnen in Azure. Een van deze mogelijkheden resource is het oplossen van problemen. [Het oplossen van resource](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) kan worden aangeroepen door PowerShell, CLI of REST-API. Als deze wordt aangeroepen, netwerk-Watcher inspecteert de status van een virtuele netwerkgateway of een verbinding en retourneert de bevindingen zijn.

Deze sectie leert u de verschillende beheertaken die momenteel beschikbaar voor het oplossen van resource zijn.

-   [Problemen met een virtuele netwerkgateway](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Problemen met een verbinding](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Netwerk-abonnementen

[Netwerk-abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) voorzien van details over het gebruik van elk van de netwerkbron in een abonnement in een regio op basis van het maximum aantal bronnen die beschikbaar zijn.

#### <a name="configuring-diagnostics-log"></a>Configuratie van diagnostische gegevens logboek

Netwerk-Watcher biedt een [diagnostische logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor diagnostische gegevens vastleggen. U kunt in deze weergave inschakelen en uitschakelen van netwerkresources snel en gemakkelijk.

### <a name="network-resource-level-monitoring"></a>Resource niveau netwerkbewaking

De volgende functies zijn beschikbaar voor bewaking van niveau resource:

#### <a name="audit-log"></a>Controlelogboek

Bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken worden geregistreerd. Deze controlelogboeken essentieel zijn voor verschillende conformiteit tot stand brengen. Deze logboeken kunnen worden weergegeven in de Azure-portal of opgehaald met behulp van Microsoft-hulpprogramma's zoals Power BI of hulpprogramma's van derden. Controlelogboeken zijn beschikbaar via de portal, PowerShell, CLI en Rest-API.

> [!Note]
> Zie voor meer informatie over controlelogboeken [bewerkingen met Resource Manager controleren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Controlelogboeken zijn beschikbaar voor bewerkingen die op alle netwerkbronnen.


#### <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn metingen van de prestaties en prestatiemeteritems die gedurende een periode worden verzameld. Metrische gegevens zijn momenteel beschikbaar voor de toepassingsgateway. Metrische gegevens kan worden gebruikt voor het activeren van waarschuwingen op basis van de drempelwaarde. Azure Application Gateway standaard bewaakt de status van alle resources in de back-end-pool en worden alle bronnen die niet in orde beschouwd uit de groep automatisch verwijderd. Toepassingsgateway blijft de slechte instanties bewaken en terug naar de goede back-end-pool toegevoegd zodra ze beschikbaar en op statuscontroles reageren. Toepassingsgateway verzendt dat de statuscontroles met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen. Deze configuratie zorgt ervoor dat de test is het testen van dezelfde poort die klanten gebruik maken van zouden verbinding maken met de back-end.

> [!Note]
> Zie [Application Gateway Diagnostics](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) om weer te geven hoe metrische gegevens kunnen worden gebruikt om waarschuwingen te maken.

#### <a name="diagnostic-logs"></a>Diagnostische logboeken

Periodieke en eigen initiatief gebeurtenissen zijn gemaakt door netwerkbronnen en storage-accounts, verzonden naar een Event Hub of Log Analytics aangemeld. Deze logboeken bieden inzicht in de status van een resource. Deze logboeken kunnen worden weergegeven in de hulpprogramma's, zoals Power BI en Log Analytics. Als u wilt weten hoe u logboeken met diagnostische gegevens bekijken, gaat u naar [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Diagnostische logboeken beschikbaar zijn voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routes, en [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Netwerk-Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor diagnostische gegevens vastleggen. U kunt in deze weergave inschakelen en uitschakelen van netwerkresources snel en gemakkelijk.

### <a name="log-analytics"></a>Log Analytics

[Meld u Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) is een service in [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

Log Analytics biedt de volgende oplossingen voor het bewaken van uw netwerken:

-   Netwerk-Prestatiemeter (NPM)

-   Azure Application Gateway analytics

-   Netwerkbeveiligingsgroep Azure analytics

#### <a name="network-performance-monitor-npm"></a>Netwerk-Prestatiemeter (NPM)
De [netwerk Prestatiemeter](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) management-oplossing is een oplossing die u de status, beschikbaarheid en bereikbaarheid van netwerken bewaakt voor netwerkbeheer.

Het wordt gebruikt voor het bewaken van de verbinding tussen:

-   openbare cloud en on-premises

-   datacenters en gebruikerslocaties (filialen)

-   de subnetten die als host fungeert voor verschillende lagen van een toepassing met meerdere lagen.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Azure application gateway analyses in logboekanalyse

De volgende logboeken worden ondersteund voor Toepassingsgateways:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

De volgende metrische gegevens worden voor Toepassingsgateways ondersteund:

-   doorvoer van 5 minuten

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Azure-netwerk-beveiligingsanalyse groep in logboekanalyse

De volgende logboeken worden ondersteund voor [netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** bevat vermeldingen voor welke NSG regels worden toegepast op virtuele machines en functies op basis van MAC-adres-instantie. De status voor deze regels worden verzameld om de 60 seconden.

- **NetworkSecurityGroupRuleCounter:** bevat vermeldingen voor hoe vaak elke NSG regel wordt toegepast om te weigeren of verkeer toestaan.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over beveiliging door het lezen van enkele van de onderwerpen over onze uitgebreide beveiliging:

-   [Log Analytics voor Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Netwerkmogelijkheden innovaties die station onderbreking van de cloud](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: De netwerken overschakelen software dat Hiermee wordt de algemene Microsoft-Cloud](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Hoe Microsoft maakt voor de snelle en betrouwbare globaal netwerk](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Licht up netwerk innovatie](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
