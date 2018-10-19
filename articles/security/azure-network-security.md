---
title: Azure-netwerkbeveiliging | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computing services met een ruime keuze aan rekenprocessen en -services die u omhoog en omlaag automatisch schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 1d94ac5f799fc4bad13ab6a5e97a225a7499380d
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405970"
---
# <a name="azure-network-security"></a>Azure-netwerkbeveiliging

We weten dat de beveiliging is taak in de cloud en hoe belangrijk is dat u nauwkeurige en tijdig informatie over Azure-beveiliging vinden. Een van de beste redenen om Azure te gebruiken voor uw toepassingen en services is om te profiteren van Azure breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Deze hulpprogramma's en mogelijkheden helpt u bij het mogelijk te maken van veilige oplossingen op het Azure-platform.

Microsoft Azure biedt vertrouwelijkheid, integriteit en beschikbaarheid van gegevens van de klant, terwijl ook transparante aansprakelijkheid. Als u wilt, kunt u beter inzicht in de verzameling van netwerk-beveiligingsmechanismen geïmplementeerd in Microsoft Azure vanuit het perspectief van de klant van is dit artikel, 'Azure Network Security', geschreven voor een uitgebreid overzicht van het netwerk beveiligingscontroles beschikbaar met Microsoft Azure.

Dit document is bedoeld om u te informeren over het brede aanbod van netwerk-besturingselementen die u configureren kunt ter verbetering van de beveiliging van de oplossingen die u in Azure implementeert. Als u geïnteresseerd bent in wat Microsoft doet voor het beveiligen van de netwerkinfrastructuur van de Azure-platform zelf, Zie de sectie Azure-beveiliging in de [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/cloudservices/azure).

## <a name="azure-platform"></a>Azure-platform

Azure is een openbare cloud service-platform die ondersteuning biedt voor een breed scala aan besturingssystemen, programmeertalen programmeertalen, frameworks, tools, databases en apparaten.  Het kunt Linux-containers uitvoeren met Docker-integratie; Bouw apps met JavaScript, Python, .NET, PHP, Java en Node.js; Bouw back-ends voor iOS, Android en Windows apparaten. Azure cloudservices ondersteunt dezelfde technologieën waar miljoenen ontwikkelaars en IT-professionals die al afhankelijk zijn van en vertrouwen.

Wanneer u baseren of IT-activa te migreren, een openbare cloud serviceprovider, u, vertrouwen al op de beveiligingmogelijkheden die aan uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

De opslaginfrastructuur van Azure van de faciliteit is zo ontworpen toepassingen miljoenen klanten tegelijkertijd kunnen hosten en biedt een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan de beveiligingsvereisten. Bovendien biedt Azure u een uitgebreide verzameling configureerbare beveiligingsopties en de mogelijkheid om deze te beheren zodat u de beveiliging om te voldoen aan de unieke vereisten van implementaties van uw organisatie kunt aanpassen.

## <a name="abstract"></a>Samenvatting

Openbare cloudservices van Microsoft bieden grootschalige services en infrastructuur, geavanceerde mogelijkheden en veel opties voor hybride verbindingen. U kunt kiezen voor toegang tot deze services via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. De Microsoft Azure-platform kunt u naadloos uitbreiden van uw infrastructuur in de cloud en bouw architecturen met meerdere lagen. Externe partijen kunt bovendien verbeterde mogelijkheden inschakelen door het aanbieden van security-services en virtuele apparaten.

De services van Azure-netwerk maximaliseert flexibiliteit, beschikbaarheid, tolerantie, beveiliging en integriteit van standaard. Dit technische document biedt details over de netwerkfuncties van Azure en informatie over hoe klanten de systeemeigen beveiligingsfuncties van Azure gebruiken kunnen om u te helpen beschermen van hun informatie-assets.

De beoogde doelgroepen voor dit technisch document zijn onder andere:

- Technische managers, netwerkbeheerders en -ontwikkelaars die hebben aan voor beveiligingsoplossingen beschikbaar en wordt ondersteund in Azure behoefte.

-   Kleine en middelgrote ondernemingen of proces leidinggevenden die willen Verkrijg een hoogstaand overzicht in de Azure VPN-technologieën en services die relevant voor discussies over netwerkbeveiliging in de openbare cloud van Azure zijn.

## <a name="azure-networking-big-picture"></a>Azure networking totaalbeeld
Microsoft Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen de resources die zich bevinden in Azure, tussen on-premises en wordt gehost op Azure-resources, en naar en van Internet en Azure.

![Azure Networking totaalbeeld](media/azure-network-security/azure-network-security-fig-1.png)

De [Azure netwerkinfrastructuur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) kunt u Azure-resources veilig met elkaar verbinden met virtuele netwerken (VNets). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van het Azure-cloud-netwerk toegewezen aan uw abonnement. U kunt VNets verbinden met uw on-premises netwerken.

Azure ondersteunt toegewezen WAN-verbinding verbinding met uw on-premises netwerk en een Azure-netwerk met [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). De koppeling tussen Azure en uw site maakt gebruik van een speciale verbinding die niet via het openbare Internet loopt. Als uw Azure-toepassing wordt uitgevoerd in meerdere datacenters, kunt u [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) te routeren van aanvragen van gebruikers op intelligente wijze over exemplaren van de toepassing. Ook kunt u verkeer routeren naar services niet wordt uitgevoerd in Azure als ze via Internet toegankelijk zijn.

## <a name="enterprise-view-of-azure-networking-components"></a>Enterprise-weergave van Azure netwerkonderdelen
Azure heeft veel netwerkonderdelen die relevant voor network security discussies zijn. We beschrijven deze netwerkonderdelen en richt u op de beveiligingsproblemen met betrekking tot deze.

> [!Note]
> Niet alle aspecten van Azure-netwerken worden beschreven: bespreken we alleen die zich pivotal in plannen en ontwerpen van een beveiligde netwerkinfrastructuur om uw services en toepassingen die u in Azure implementeert.

In dit artikel worden behandeld de volgende Azure-netwerken zakelijke mogelijkheden:

-   Netwerkverbinding

-   Hybride connectiviteit

-   Besturingselementen voor beveiliging

-   Het netwerk niet valideren

### <a name="basic-network-connectivity"></a>Netwerkverbinding

De [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) service kunt u Azure-resources veilig met elkaar verbinden met virtuele netwerken (VNet). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van de infrastructuur van de Azure-netwerk toegewezen aan uw abonnement. U kunt ook verbinding maken met VNets aan elkaar en aan uw on-premises netwerken met behulp van site-naar-site VPN-verbindingen en toegewezen [WAN-koppelingen](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Netwerkverbinding](media/azure-network-security/azure-network-security-fig-2.png)

Met het inzicht dat u virtuele machines op host-servers in Azure, met de vraag is hoe deze virtuele machines verbinding maken met een netwerk. Het antwoord is dat virtuele machines verbinding met maken een [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Virtuele netwerken van Azure zijn te vergelijken met de virtuele netwerken die u on-premises gebruiken met uw eigen virtualisatieoplossingen platform, zoals Microsoft Hyper-V of VMware.

#### <a name="intra-vnet-connectivity"></a>Intra-VNet-connectiviteit

U kunt VNets verbinden met elkaar, zodat resources die zijn verbonden met een VNet om te communiceren met elkaar via vnet's. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** kunnen resources met andere Azure VNets in dezelfde Azure-locatie om te communiceren met elkaar verbonden. De bandbreedte en de latentie tussen het VNet is hetzelfde als wanneer de resources die zijn verbonden met hetzelfde VNet is. Lees voor meer informatie over peering [peering van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Peering](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet-naar-VNet-verbinding:** kunnen resources die zijn verbonden met andere Azure-VNet binnen de dezelfde of verschillende Azure-locaties. In tegenstelling tot de peering, is bandbreedte tussen de VNets, omdat de verkeersstroom moet via een Azure VPN-Gateway.

![VNet-naar-VNet-verbinding](media/azure-network-security/azure-network-security-fig-4.png)


Lees voor meer informatie over het verbinden van vnet's met een VNet-naar-VNet-verbinding, het [configureren van een VNet-naar-VNet-verbinding artikel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Mogelijkheden voor Azure-netwerk:

Zoals u ziet, biedt een Azure-netwerk verbinding maken met het netwerk, zodat ze verbinding met andere netwerkbronnen op een veilige manier maken kunnen voor virtuele machines. Basisconnectiviteit is echter nog maar het begin. De volgende mogelijkheden van de Azure Virtual Network-service beschikbaar beveiligingskenmerken van het Virtueelnetwerk van Azure maken:

-   Isolatie

-   Internetconnectiviteit

-   Azure-resource-connectiviteit

-   VNet-connectiviteit

-   On-premises connectiviteit

-   Verkeer filteren

-   Routering

**Isolatie**

Vnet's zich [geïsoleerde](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) van elkaar. U kunt afzonderlijke VNets voor ontwikkeling, testen en productie die gebruikmaken van dezelfde maken [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) adres blokkeert. U kunt echter meerdere VNets die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden maken. U kunt een VNet onderverdelen in meerdere subnetten.

Azure biedt interne naamomzetting voor VM's en [Cloudservices](https://azure.microsoft.com/services/cloud-services/) rolinstanties die zijn verbonden met een VNet. U kunt eventueel een VNet voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.

U kunt implementeren om meerdere VNets in elke Azure [abonnement](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) en Azure [regio](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Elk VNet is geïsoleerd van andere vnet's. Voor elke VNet kunt u het volgende doen:

-   Een aangepaste persoonlijke IP-adresruimte opgeven met behulp van openbare en persoonlijke adressen (RFC 1918). Azure wijst resources die zijn verbonden met het VNet een privé IP-adres van de adresruimte, u toewijzen.

-   Het VNet segmenteren in een of meer subnetten en een gedeelte van de VNet-adresruimte aan elk subnet toewijzen.

-   Azure geleverd naamomzetting gebruiken of uw eigen DNS-server voor gebruik door resources die zijn verbonden met een VNet opgeven. Lees voor meer informatie over naamomzetting in VNets de [naamomzetting voor virtuele machines en Cloud Services](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**Verbinding met internet**

Alle [Azure Virtual Machines (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) en Cloud Services-rolinstanties die is verbonden met een VNet hebben standaard toegang tot het Internet. U kunt ook binnenkomende toegang tot bepaalde resources inschakelen indien nodig. (VM) en Cloud Services-rolinstanties die is verbonden met een VNet hebben standaard toegang tot het Internet. U kunt ook binnenkomende toegang tot bepaalde resources inschakelen indien nodig.

Alle resources die zijn verbonden met een VNet hebben uitgaande verbinding met Internet standaard. De privé IP-adres van de resource is netwerk bronadres vertaald (SNAT) naar een openbaar IP-adres door de Azure-infrastructuur. U kunt de standaard-connectiviteit wijzigen door het implementeren van aangepaste Routering en filteren van verkeer. Lees voor meer informatie over uitgaande verbinding met Internet heeft, de [uitleg over uitgaande verbindingen in Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

Inkomend communiceren met Azure-resources vanaf het Internet of om te communiceren met Internet, zonder SNAT uitgaande, moet een openbaar IP-adres toegewezen aan een resource is geselecteerd. Lees voor meer informatie over openbare IP-adressen, de [openbare IP-adressen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Azure-resource-connectiviteit**

[Azure-resources](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) zoals Cloudservices en virtuele machines kunnen worden verbonden met hetzelfde VNet. De resources kunnen verbinding maken met elkaar met behulp van privé IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, VNets en on-premises netwerken, zodat u niet hoeven te configureren en beheren van routes.

U kunt verschillende Azure-resources verbinden met een VNet, zoals virtuele Machines (VM), Cloud Services, App Service-omgevingen en Virtual Machine Scale Sets. Virtuele machines verbinding maken met een subnet binnen een VNet via een netwerkinterface (NIC). Lees voor meer informatie over NIC's, de [netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**VNet-connectiviteit**

[VNets](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kunnen worden verbonden met elkaar worden verbonden, zodat resources die zijn verbonden met een VNet om te communiceren met een resource in een andere VNet.

U kunt VNets verbinden met elkaar, zodat resources die zijn verbonden met een VNet om te communiceren met elkaar via vnet's. U kunt een of beide van de volgende opties VNets met elkaar verbinden:

- **Peering:** kunnen resources met andere Azure VNets in dezelfde Azure-locatie om te communiceren met elkaar verbonden. De bandbreedte en de latentie tussen de VNets is hetzelfde als wanneer de resources die zijn verbonden met de dezelfde VNet.To meer informatie over peering, lees de [peering van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-naar-VNet-verbinding:** kunnen resources die zijn verbonden met andere Azure-VNet binnen de dezelfde of verschillende Azure-locaties. In tegenstelling tot de peering, is bandbreedte tussen de VNets, omdat de verkeersstroom moet via een Azure VPN-Gateway. Voor meer informatie over het verbinden van vnet's met een VNet-naar-VNet-verbinding. Lees voor meer informatie de [een VNet-naar-VNet-verbinding configureren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**On-premises connectiviteit**

VNets kunnen worden verbonden met [on-premises](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) netwerken via VPN-verbindingen tussen uw netwerk en Azure, of via een site-naar-site VPN-verbinding via Internet.

U kunt uw on-premises netwerk verbinden met een VNet maken via een combinatie van de volgende opties:

- **Punt-naar-site virtueel particulier netwerk (VPN):** tot stand gebracht tussen een enkele PC verbonden met uw netwerk en het VNet. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. De verbinding gebruikt de SSTP-protocol voor versleutelde communicatie via Internet tussen de PC en het VNet. De latentie voor een punt-naar-site-VPN is onvoorspelbaar omdat het verkeer gaat via Internet.

- **Site-naar-site-VPN:** tot stand gebracht tussen uw VPN-apparaat en een Azure VPN-Gateway. Dit verbindingstype krijgen alle on-premises bron verleent u onbevoegde gebruikers toegang tot een VNet. De verbinding is een IPsec/IKE VPN waarmee de versleutelde communicatie via Internet mogelijk tussen uw on-premises apparaat en de Azure VPN-gateway. De latentie voor een site-naar-site-verbinding is onvoorspelbaar omdat het verkeer gaat via Internet.

- **Azure ExpressRoute:** wordt tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer loopt niet via Internet. De latentie voor een ExpressRoute-verbinding is voorspelbare omdat verkeer niet via Internet. Lees voor meer informatie over de vorige verbindingsopties de [verbindingstopologie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Verkeer filteren**

Instanties van de virtuele machine en Cloud Services [netwerkverkeer](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kunnen worden gefilterd binnenkomende en uitgaande IP-bronadres en poort, doel-IP-adres en poort en protocol.

U kunt netwerkverkeer filteren tussen subnetten met behulp van een of beide van de volgende opties:

- **Netwerkbeveiligingsgroepen (NSG):** elke NSG kan meerdere binnenkomende en uitgaande beveiligingsregels waarmee u verkeer filteren op bron- en IP-adres, poort en protocol bevatten. U kunt een NSG toepassen op elke NIC in een virtuele machine. U kunt ook een NSG toepassen op het subnet een NIC of andere Azure-resource is verbonden met. Lees voor meer informatie over nsg's de [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Virtuele netwerkapparaten:** een virtueel netwerkapparaat is een virtuele machine bij het uitvoeren van software die een netwerkfunctie, zoals een firewall uitvoert. Een lijst van beschikbare NVA's in de Azure Marketplace weergeven. NVA's zijn ook beschikbaar met de WAN-optimalisatie en andere netwerk verkeer functies. NVA's worden meestal gebruikt met de gebruiker gedefinieerde of BGP-routes. U kunt ook een NVA gebruiken voor het filteren van verkeer tussen VNets.

**Routering**

U kunt eventueel van Azure standaard routering met BGP-routes via een gateway van het netwerk of configureren van uw eigen routes overschrijven.

Azure maakt routetabellen maken waarmee de resources die zijn verbonden met een subnet in elk VNet om te communiceren met elkaar worden verbonden, wordt standaard ingeschakeld. U kunt een of beide van de volgende opties implementeren om de standaardroutes die Azure maakt te onderdrukken:

- **Gebruiker gedefinieerde routes:** u kunt aangepaste routetabellen maken met routes die bepalen waar verkeer wordt doorgestuurd naar voor elk subnet. Lees voor meer informatie over de gebruiker gedefinieerde routes de [gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP-routes:** als u uw VNet verbinden met uw on-premises netwerk met behulp van een Azure VPN-Gateway of ExpressRoute-verbinding, kunt u BGP-routes doorgegeven aan uw vnet's.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Hybride verbinding met internet: verbinding maken met een on-premises netwerk
U kunt uw on-premises netwerk verbinden met een VNet maken via een combinatie van de volgende opties:

-   Internetconnectiviteit

-   Punt-naar-site-VPN (P2S VPN)

-   Site-naar-Site-VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Verbinding met Internet

Zoals de naam al aangeeft, kunt verbinding met Internet u uw workloads toegankelijk is vanaf het Internet, doordat u verschillende openbare eindpunten op werkbelastingen die live binnen het virtuele netwerk. Deze werkbelastingen kunnen worden beschikbaar gesteld met [internetgerichte Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) of gewoon een openbaar IP-adres toewijzen aan de virtuele machine. Op deze manier wordt het mogelijk dat alles op Internet om te kunnen bereiken die virtuele machine, een hostfirewall opgegeven [netwerkbeveiligingsgroepen (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), en [door de gebruiker gedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) die u wilt toestaan gebeuren.

In dit scenario kunt u een toepassing die moet worden openbare met Internet en geen verbinding maken met vanaf elke locatie of op specifieke locaties, afhankelijk van de configuratie van uw workloads worden weergeven.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Punt-naar-Site VPN- of Site-naar-Site-VPN
Deze twee valt in dezelfde categorie. Beide uw VNet om een VPN-Gateway nodig hebt en u verbinding mee kan maken met behulp van een VPN-Client voor uw werkstation als onderdeel van de [punt-naar-Site-configuratie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) of u kunt uw on-premises configureren [VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)kunnen een site-naar-site-VPN is beëindigd. Op deze manier on-premises apparaten kunnen verbinding maken met bronnen binnen het VNet.

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol).

![Punt-naar-Site-VPN](media/azure-network-security/azure-network-security-fig-5.png)

Punt-naar-Site-verbindingen zijn handig als u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie center, of wanneer u slechts een paar clients hebt die moeten verbinding maken met een virtueel netwerk.

Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer. P2S is daarom niet aanbevolen manier om te verbinden met Azure als u een permanente verbinding uit veel on-premises apparaten en computers met uw Azure-netwerk.

![Site-to-Site VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Zie voor meer informatie over punt-naar-Site-verbindingen, de [punt-naar-Site FA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk.

Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Deze verbinding vindt plaats via Internet en u kunt 'tunnel' informatie binnen een gecodeerde verbinding tussen uw netwerk en Azure. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. Versleuteling van de tunnel wordt uitgevoerd met behulp van [IPsec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

Site-naar-site VPN is een technologie voor vertrouwde, betrouwbare en tot stand gebrachte, verkeer binnen de tunnel via Internet. Bovendien is de bandbreedte relatief beperkt tot een maximum van ongeveer 200 Mbps.

Als u een uitzonderlijke mate van veiligheid of prestaties voor uw cross-premises verbindingen nodig hebt, raden wij aan dat u Azure ExpressRoute voor uw cross-premises-connectiviteit. ExpressRoute is een speciale WAN koppeling tussen uw on-premises locatie of een Exchange-hostingprovider. Omdat dit een telco-verbinding, worden uw gegevens niet via Internet worden verzonden en daarom geen toegang heeft tot de mogelijke risico's in de communicatie via Internet.

> [!Note]
> Zie voor meer informatie over VPN-gateways, [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Toegewezen WAN-verbinding
Microsoft Azure ExpressRoute kunt u uw on-premises netwerken uitbreiden naar Azure via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider.

ExpressRoute-verbindingen gaan niet via het openbare internet. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.

![ Toegewezen WAN-verbinding](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> Zie voor meer informatie over verbinding maken tussen uw netwerk naar Microsoft met behulp van ExpressRoute [ExpressRoute-connectiviteitsmodellen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) en [technisch overzicht van ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Als met de site-naar-site VPN-opties kunt ExpressRoute u ook verbinding maken met bronnen die zich niet per se in slechts één VNet. In feite, afhankelijk van de SKU, u kunt verbinding maken met 10 VNets. Als u hebt de [premium-invoegonderdeel](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), verbindingen met maximaal 100 vnet's zijn mogelijk, afhankelijk van de bandbreedte. Lees voor meer informatie over wat deze typen verbindingen bekijken, zoals [verbindingstopologie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Besturingselementen voor beveiliging
Een Azure Virtual Network biedt een veilige, logische netwerk dat is geïsoleerd van andere virtuele netwerken en biedt ondersteuning voor veel beveiligingsmaatregelen die u op uw on-premises netwerken gebruikt. Klanten maken hun eigen structuur met behulp van: subnetten: ze hun eigen privé-IP-adresbereik gebruiken, routetabellen configureren, netwerkbeveiligingsgroepen, toegangsbeheerlijsten (ACL's), gateways en virtuele apparaten voor het uitvoeren van hun workloads in de cloud.

Hier volgen beveiligingscontroles die kunt u op uw virtuele Azure-netwerken:

-   Netwerktoegangsbeheer

-   Gebruiker gedefinieerde Routes

-   Apparaat voor netwerkbeveiliging

-   Application Gateway

-   Azure Web Application Firewall

-   Besturingselement voor netwerk-beschikbaarheid

#### <a name="network-access-controls"></a>Netwerktoegangsbeheer
Terwijl de virtuele Azure-netwerk (VNet) de hoeksteen van Azure-netwerkmodel is en isolatie en bescherming biedt, de [Netwerkbeveiligingsgroep groepen (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) zijn de belangrijkste hulpprogramma dat u gebruiken om te dwingen en beheren van regels voor netwerkverkeer op de het niveau van het netwerk.

![ Netwerktoegangsbeheer](media/azure-network-security/azure-network-security-fig-8.png)


U kunt toegang toestaan of weigeren van communicatie tussen de werkbelastingen binnen een virtueel netwerk, van systemen op klantnetwerken via cross-premises-connectiviteit, beheren of directe communicatie met Internet.

In het diagram, zowel VNets en nsg's zich bevinden in een bepaalde laag in de Azure algehele security-stack, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt om de beveiligingsgrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk te maken.

Nsg's gebruik van een 5-tuple om te evalueren verkeer (en worden gebruikt in de regels die u voor de NSG configureert):

-   [Bron- en IP-adres](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Bron-en doelpoort](https://technet.microsoft.com/library/dd197515)

-   Protocol: [Transmission controleprotocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) of [User Datagram Protocol (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Dit betekent dat u de toegang tussen een enkele virtuele machine en een groep VM's of een enkele virtuele machine naar een andere één virtuele machine, of tussen volledige subnetten kunt beheren. Nogmaals, houd er rekening mee dat dit eenvoudige filterregels voor pakketten, niet voor volledige pakketinspectie. Er is geen protocolvalidatie of het netwerkniveau id's of de mogelijkheid van de IP-Adressen in een Netwerkbeveiligingsgroep.

Een NSG wordt geleverd met enkele ingebouwde regels die u moet rekening houden met. Dit zijn:

-   **Toestaan dat al het verkeer binnen een bepaald virtueel netwerk:** alle virtuele machines op dezelfde Azure-netwerk met elkaar communiceren.

-   **Azure load balancing inkomende toestaan:** met deze regel staat verkeer vanaf elk bronadres naar een bestemmingsadres voor de Azure load balancer.

-   **Alle binnenkomend verkeer weigeren:** met deze regel blokkeert al het verkeer sourcing vanaf het Internet die u expliciet zijn toegestaan.

-   **Al het verkeer uitgaand verkeer naar Internet toestaan:** met deze regel kunnen virtuele machines moet verbinding met Internet initiëren. Als u niet dat deze verbindingen geïnitieerd wilt, moet u het maken van een regel voor het blokkeren van deze verbindingen of af te dwingen geforceerde tunneling.

#### <a name="system-routes-and-user-defined-routes"></a>Systeemroutes en de gebruiker gedefinieerde routes

Wanneer u virtuele machines (VM's) aan een virtueel netwerk (VNet) in Azure toevoegen, merkt u dat de virtuele machines met elkaar communiceren via het netwerk, automatisch zijn. U hoeft geen gateway op te geven, ook niet als de virtuele machines tot verschillende subnetten behoren.

Hetzelfde geldt voor de communicatie tussen VM's en internet. Als er een hybride verbinding is tussen Azure en uw eigen datacentrum, is er zelfs communicatie met uw on-premises netwerk mogelijk.

![Systeemroutes](media/azure-network-security/azure-network-security-fig-9.png)

Deze communicatiestroom is mogelijk omdat de IP-verkeersstromen in Azure wordt gedefinieerd met behulp van een reeks systeemroutes. Systeemroutes bepalen de communicatiestroom in de volgende scenario's:

-   Binnen een en hetzelfde subnet.

-   Van het ene naar het andere subnet binnen een VNet.

-   Van virtuele machines naar internet.

-   Van het ene naar het andere VNet via een VPN-gateway.

-   Van een VNet met een andere VNet via VNet-Peering ([Servicechaining](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Van een VNet naar uw on-premises netwerk via een VPN-gateway.

Veel bedrijven hebben een strikte beveiligingsbeheer en nalevingsvereisten waarvoor on-premises inspectie van alle netwerkpakketten om af te dwingen een specifiek beleid. Azure biedt een mechanisme genaamd [geforceerde tunneling](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) dat verkeer van de virtuele machines met on-premises routes door het maken van een aangepaste route of [Border Gateway Protocol (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) aankondigingen via ExpressRoute - of VPN.

Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes (UDR). Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de Azure VPN-gateway.

Het volgende gedeelte bevat de huidige beperking van de tabel en routes de routering voor een Azure-netwerk:

-   Elk virtueel netwerksubnet heeft een ingebouwde, systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:

 -  **Lokale VNet routes:** rechtstreeks naar de bestemming VM's in hetzelfde virtuele netwerk

 - **Op de lokale routes:** naar de Azure VPN-gateway

 -  **Standaard-route:** rechtstreeks met Internet. Pakketten dat is bestemd voor het particuliere IP-adressen niet wordt gedekt door de vorige twee routes worden verwijderd.

-   U kunt met het uitbrengen van de gebruiker gedefinieerde routes, een routeringstabel om toe te voegen een standaardroute maken en vervolgens de routeringstabel koppelen aan uw VNet-subnet om in te schakelen geforceerde tunneling op deze subnetten.

-   U moet een 'standaard-site"tussen de cross-premises lokale sites die zijn verbonden met het virtuele netwerk instellen.

-   Geforceerde tunneling moet worden gekoppeld aan een VNet met een VPN-gateway voor dynamische routering (niet een statische gateway genoemd).

- ExpressRoute geforceerde tunneling is niet geconfigureerd via dit mechanisme, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies.

> [!Note]
> Zie voor meer informatie de [documentatie voor ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

#### <a name="network-security-appliances"></a>Beveiliging van netwerkapparaten
Netwerkbeveiligingsgroepen en door de gebruiker gedefinieerde Routes kan zorgen voor een bepaalde mate van netwerkbeveiliging op het netwerk en transport lagen van de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), er zijn nog situaties waarin u wilt of moet beveiliging inschakelen hogere niveaus van de netwerkstack. In dergelijke situaties is het raadzaam dat u een virtueel netwerk-beveiligingsapparaten die is geleverd door Azure-partners implementeert.

![Beveiliging van netwerkapparaten](./media/azure-network-security/azure-network-security-fig-10.png)

Azure-netwerk-beveiligingsapparaten verbeteren VNet beveiligings- en netwerkfuncties en ze beschikbaar zijn van verschillende leveranciers via de [Azure Marketplace](https://azuremarketplace.microsoft.com). Deze virtuele-beveiligingsapparaten kunnen worden geïmplementeerd om te bieden:

-   Maximaal beschikbare firewalls

-   Inbraakdetectiehandtekeningen preventie

-   Indringers detecteren

-   Web application firewalls (WAF's)

-   WAN-optimalisatie

-   Routering

-   Taakverdeling

-   VPN

-   Certificaatbeheer

-   Active Directory

-   Meervoudige verificatie

#### <a name="application-gateway"></a>Toepassingsgateway

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) is een speciaal virtueel apparaat waarmee u een application delivery controller (ADC) als een service.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Application Gateway kunt u web farm prestaties en beschikbaarheid optimaliseren door het offloaden van CPU intensieve SSL-beëindiging aan de toepassingsgateway (SSL-offloading). Het bevat ook andere Routeringsmogelijkheden van laag 7 met inbegrip van:

-   Round robin-distributie van inkomend verkeer

-   Cookies gebaseerde sessieaffiniteit

-   URL-pad gebaseerde routering

-   Mogelijkheid voor het hosten van meerdere websites achter één Application Gateway


Een [web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) is ook beschikbaar als onderdeel van de toepassingsgateway. Dit biedt beveiliging voor webtoepassingen tegen veelvoorkomende beveiligingsproblemen op het web. Application Gateway kan worden geconfigureerd als een internetgerichte gateway, interne enige gateway of een combinatie van beide.

Application Gateway WAF kan worden uitgevoerd in de modus voor detectie of te voorkomen. Er is een gebruikelijk voor beheerders om uit te voeren in de detectiemodus voor van verkeer voor schadelijke patronen in acht nemen. Zodra een potentiële aanvallen zijn gedetecteerd, wordt preventiemodus verdachte inkomend verkeer wordt geblokkeerd.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Bovendien Application Gateway WAF helpt u bij het bewaken van webtoepassingen tegen aanvallen met behulp van een real-time logboek van WAF die is geïntegreerd met [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) en [Azure Security Center](https://azure.microsoft.com/services/security-center/) om bij te houden van de WAF-waarschuwingen en gemakkelijk trends te bewaken.

Het opgemaakte JSON-logboek gaat rechtstreeks naar het opslagaccount van de klant. U hebt volledige controle over deze logboeken en uw eigen bewaarbeleid kunt toepassen.

U kunt deze logboeken ook opnemen in uw eigen analytics system met [Azure-Logboekintegratie](https://aka.ms/AzLog). WAF-Logboeken ook worden geïntegreerd met [Log Analytics](../log-analytics/log-analytics-overview.md) zodat u Log Analytics gebruiken kunt voor het uitvoeren van geavanceerde fijnmazig query's.

#### <a name="azure-web-application-firewall-waf"></a>Azure-web application firewall (WAF)

Webtoepassingen zijn in toenemende mate doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen, zoals SQL-injectie, aanvallen via cross-site scripting en andere aanvallen die worden weergegeven in de [OWASP top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Zo wordt voorkomen dat dergelijke aanvallen in de toepassing vereist tevens veel onderhoud, patching en controle op meerdere lagen van de Toepassingstopologie.

 ![Azure Web Application Firewall (WAF)](./media/azure-network-security/azure-network-security-fig-13.png)

Een gecentraliseerde [web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) kunt beveiligen tegen aanvallen via Internet en vereenvoudigt het beveiligingsbeheer zonder eventuele wijzigingen in de toepassing.

Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

#### <a name="network-availability-controls"></a>Besturingselementen voor netwerk-beschikbaarheid

Er zijn verschillende opties voor het distribueren van netwerkverkeer met behulp van Microsoft Azure. Deze opties werken verschillend. Ze hebben een eigen functieset en ondersteunen verschillende scenario's. Ze kunnen elk afzonderlijk worden gebruikt, maar ook worden gecombineerd.

Hieronder vindt u de besturingselementen van de beschikbaarheid van netwerk:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure Load balancer**

Biedt hoge beschikbaarheid en netwerkprestaties voor uw toepassingen. Het is een Layer 4 (TCP, UDP) load balancer die inkomend verkeer over gezonde exemplaren van services die zijn gedefinieerd in een load balancer-set distribueert.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Azure Load Balancer kan worden geconfigureerd om:

-   Laden saldo inkomend internetverkeer op virtuele machines. Deze configuratie wordt ook wel [internetgerichte load balancing](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Taakverdeling voor verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in cloudservices of tussen on-premises computers en virtuele machines in een virtueel netwerk met cross-premises. Deze configuratie wordt ook wel [intern gelijke taakverdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Extern verkeer doorsturen naar een specifieke virtuele machine.

Alle resources in de cloud moeten een openbaar IP-adres aan bereikbaar zijn vanaf Internet. De cloudinfrastructuur in Azure maakt gebruik van niet-routeerbare IP-adressen voor de daarbij behorende bronnen. Azure maakt gebruik van netwerkadresomzetting (NAT) met openbare IP-adressen om te communiceren met Internet.

 **Toepassingsgateway**

 Application Gateway werkt op de toepassingslaag (laag 7 in de referentiestack van het netwerk). Deze service fungeert als een omgekeerde proxyservice, beëindigt de clientverbinding en stuurt aanvragen door naar back-endeindpunten.

 **Traffic manager**

Microsoft Azure Traffic Manager kunt u de distributie van gebruikersverkeer voor service-eindpunten in verschillende datacenters beheren. Ondersteund door Traffic Manager-service-eindpunten zijn Azure-VM's, Web-Apps en cloudservices. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn.

Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients op het meest geschikte eindpunt op basis van een [verkeersrouteringsmethode](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) en de status van de eindpunten. Traffic Manager biedt een aantal methoden routering van verkeer op basis van behoeften van de andere toepassing, de gezondheid van eindpunt [bewaking](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), en automatische failover. Traffic Manager is bestand tegen storingen, waaronder het uitvallen van een hele Azure-regio.

Met Azure Traffic Manager kunt u voor het beheren van de distributie van verkeer tussen de toepassingseindpunten van uw. Een eindpunt is een internetgerichte service die binnen of buiten Azure wordt gehost.

Traffic Manager biedt twee belangrijke voordelen:

-   Distributie van verkeer op basis van een van verschillende [routeringsmethoden voor verkeer](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Doorlopende bewaking van status van eindpunt](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) en automatische failover wanneer eindpunten mislukken.

Wanneer een client probeert verbinding maken met een service, moet deze eerst de DNS-naam van de service omzetten naar een IP-adres. De client maakt vervolgens verbinding met IP-adres voor toegang tot de service. Traffic Manager gebruikt DNS om clients omleiden naar specifieke service-eindpunten op basis van de regels van de verkeersrouteringsmethode. Clients rechtstreeks verbinding maken met het geselecteerde eindpunt. Traffic Manager is niet een proxy of een gateway. Traffic Manager het verkeer te geven tussen de client en de service niet te zien.

### <a name="azure-network-validation"></a>Validatie van de Azure-netwerk

Validatie van de Azure-netwerk is om ervoor te zorgen dat het Azure-netwerk wordt uitgevoerd als deze is geconfigureerd en validatie kan worden gedaan met behulp van de services en functies die beschikbaar zijn om het netwerk te controleren. Met Azure Network Watcher kunt u toegang tot een breed spectrum van logboekregistratie en diagnostische mogelijkheden waarmee u met inzichten om te begrijpen van de prestaties van het netwerk en de status. Deze mogelijkheden zijn toegankelijk via de Portal, Power Shell, CLI, Rest-API en SDK.

Azure operationele beveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere items in Microsoft Azure. Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit een diverse mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Center , en een diep besef van het landschap van cyberveiligheidsbedreigingen security cyberaanvallen.

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure Network watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure resourcemanager

De mensen en processen die Microsoft Azure gebruiken zijn mogelijk de belangrijkste beveiligingsfunctie van het platform. In deze sectie beschrijft de functies van de globale datacenterinfrastructuur van Microsoft die helpen verbeteren en onderhouden van beveiliging, bedrijfscontinuïteit en privacy.

De infrastructuur voor uw toepassing bestaat meestal uit veel onderdelen, zoals een virtuele machine, een opslagaccount en een virtueel netwerk, of een webtoepassing, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. Met Azure Resource Manager kunt u met de resources als groep in uw oplossing werken.

U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

**De voordelen van het gebruik van Resource Manager**

Resource Manager biedt diverse voordelen:

-   U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

-   U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

-   U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

-   U kunt de afhankelijkheden tussen resources, zodat ze zijn geïmplementeerd in de juiste volgorde definiëren.

-   U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.

-   U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

-   U kunt facturering voor uw organisatie verduidelijken door te kijken naar de kosten voor een groep resources met tag.

> [!Note]
> Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u het eerdere implementatiemodel hebt gebruikt en meer te weten wilt komen over de wijzigingen, leest u [Resource Manager-implementatie en klassieke implementatie begrijpen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Azure-netwerk logboekregistratie en bewaking

Azure biedt veel hulpprogramma's om te controleren, voorkomen, detecteren en reageren op beveiligingsgebeurtenissen netwerk. Enkele van de meest krachtige hulpprogramma's die u in dit gebied zijn:

-   Network Watcher

-   De Resource niveau Netwerkbewaking

-   Log Analytics

### <a name="network-watcher"></a>Netwerk-watcher

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -Scenario's gebaseerde controle is opgegeven met de functies van Network Watcher. Deze service omvat pakket vastleggen, volgende hop, IP-stroom controleren, weergave van de beveiligingsgroep, NSG-stroomlogboeken. Scenario niveau bewaking biedt een end-to-weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

 ![Network Watcher](./media/azure-network-security/azure-network-security-fig-15.png)

Network Watcher is een regionale service waarmee u kunt bewaken en diagnoses uitvoeren omstandigheden op netwerkscenarioniveau in, en naar Azure. Diagnose en visualisatie hulpprogramma's die beschikbaar zijn met Network Watcher kunnen u begrijpen, diagnosticeren en inzicht verkrijgen in uw netwerk in Azure.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

#### <a name="topology"></a>Topologie

[Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) geeft als resultaat een grafiek van netwerkbronnen in een virtueel netwerk. De grafiek ziet u de onderlinge verbinding tussen de resources om weer te geven van de end-to-netwerkverbinding. In de portal retourneert topologie van de resourceobjecten op aan de hand van virtueel netwerk uit te voeren. De relaties worden weergegeven met lijnen tussen de bronnen buiten de regio Network Watcher, zelfs als in de bron de groep niet worden weergegeven. De resources die worden geretourneerd in de portal weergave vormen een subset van de netwerkonderdelen die diagram worden weergegeven aan. De volledige lijst van netwerkresources wilt bekijken, kunt u [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) of [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Omdat er resources zijn geretourneerd wordt de verbinding tussen ze zijn gemodelleerd onder twee relaties.

- **Containment** -Virtueelnetwerk bevat een Subnet, waarin een NIC.

- **Dat is gekoppeld** -een NIC is gekoppeld aan een virtuele machine.

#### <a name="variable-packet-capture"></a>Variabele pakketopname

Network Watcher [variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) kunt u packet capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Packet capture helpt om op te sporen netwerk afwijkingen beide reactief en proactivity. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen.

Pakketopname is de extensie van een virtuele machine die op afstand via Network Watcher is gestart. Deze mogelijkheid vereenvoudigt het ongemak van het uitvoeren van een pakketopname handmatig op de gewenste virtuele machine, waardoor waardevolle tijd worden opgeslagen. Pakketopname kan worden geactiveerd via de portal, PowerShell, CLI of REST-API. Er is een voorbeeld van hoe pakketopname kan worden geactiveerd met waarschuwingen van de virtuele Machine.

#### <a name="ip-flow-verify"></a>IP-stroomverificatie

[IP-stromen controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) als een pakket wordt toegestaan of verboden vanaf of naar een virtuele machine op basis van 5-tuple-informatie wordt gecontroleerd. Deze informatie bestaat uit de richting, protocol, lokale IP, externe IP-, lokale poort en externe poort. Als het pakket is geweigerd door een beveiligingsgroep, wordt de naam van de regel die het pakket geweigerd geretourneerd. Terwijl de IP-bron- of doelserver kan worden gekozen, helpt deze functie beheerders snel vaststellen van connectiviteitsproblemen met van of naar het internet en van of naar de on-premises omgeving.

IP-stromen controleren is gericht op een netwerkinterface van een virtuele machine. Netwerkverkeer wordt vervolgens geverifieerd op basis van de geconfigureerde instellingen naar of van de netwerkinterface. Deze mogelijkheid is nuttig bij het bevestigen van als binnenkomende of uitgaande verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een Netwerkbeveiligingsgroep.

#### <a name="next-hop"></a>Volgende hop

Bepaalt de [volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) voor pakketten worden gerouteerd in de Azure-netwerk-Fabric, zodat u kunt voor het vaststellen van een gebruiker gedefinieerde routes geconfigureerd. Verkeer van een virtuele machine wordt verzonden naar een bestemming op basis van de effectieve routes die zijn gekoppeld aan een NIC wordt gebruikt. Volgende hop worden de volgende hoptype en het IP-adres van een pakket opgehaald uit een specifieke virtuele machine en NIC Zo kunt u bepalen of het pakket wordt omgeleid naar de bestemming of het verkeer wordt zwarte gaan is.

Volgende hop retourneert ook de routetabel die zijn gekoppeld aan de volgende hop. Bij het opvragen van de volgende hop als de route is gedefinieerd als een gebruiker gedefinieerde route, worden die route geretourneerd. Anders retourneert de volgende hop 'Systeemroute'.

#### <a name="security-group-view"></a>weergave van de beveiligingsgroep

Hiermee haalt u de effectieve en toegepaste beveiligingsregels die worden toegepast op een virtuele machine. Netwerkbeveiligingsgroepen worden gekoppeld op het subnetniveau van een of op een NIC-niveau. Als dat is gekoppeld op het subnetniveau van een, geldt dit voor alle VM-exemplaren in het subnet. Netwerk [beveiligingsgroep weergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) retourneert de geconfigureerde Netwerkbeveiligingsgroepen en regels die zijn gekoppeld op het niveau van een NIC en het subnet voor een virtuele machine biedt inzicht in de configuratie. Bovendien worden de effectieve beveiligingsregels voor elk van de NIC's in een virtuele machine geretourneerd. Met behulp van Network Security Group weergeven, kunt u een virtuele machine voor netwerk-beveiligingsproblemen, zoals poorten openen beoordelen. U kunt ook controleren of uw Netwerkbeveiligingsgroep werkt zoals verwacht op basis van een [vergelijking tussen de geconfigureerde en de effectieve beveiligingsregels](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>Stroomlogboeken logboekregistratie

 Stroomlogboeken voor Netwerkbeveiligingsgroepen kunnen u voor het vastleggen van logboeken met betrekking tot het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels in de groep. De stroom wordt gedefinieerd door een 5-tuple-informatie, bron-IP, doel-IP, doelpoort, bronpoort en Protocol.

[Stroomlogboeken van Netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Virtuele netwerkgateway en verbinding oplossen

Network Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van de netwerkresources van uw in Azure. Een van deze mogelijkheden is resource-oplossen van problemen. [Het oplossen van resource](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) kan worden aangeroepen in PowerShell, CLI of REST-API. Indien aangeroepen, wordt Network Watcher inspecteert de status van een virtuele netwerkgateway of een verbinding en retourneert de bevindingen.

In deze sectie doorloopt u de verschillende beheertaken die momenteel beschikbaar voor het oplossen van de resource zijn.

-   [Oplossen van een virtuele netwerkgateway](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Problemen met een verbinding oplossen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Limieten voor netwerk-abonnement

[De abonnementslimieten netwerk](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) u voorzien van details van het gebruik van elk van de netwerk-resource in een abonnement in een regio op basis van het maximum aantal beschikbare resources.

#### <a name="configuring-diagnostics-log"></a>Configuratie van diagnostische gegevens Log

Network Watcher biedt een [diagnostische logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor registratie in diagnoselogboek. In deze weergave kunt u in- en netwerkresources uitschakelen gemakkelijk en snel.

### <a name="network-resource-level-monitoring"></a>De resource niveau netwerkbewaking

De volgende functies zijn beschikbaar voor resource niveau bewaking:

#### <a name="audit-log"></a>Auditlogboek

Bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken worden geregistreerd. Deze auditlogboeken zijn essentieel om verschillende ingebouwde stand te brengen. Deze logboeken kunnen worden weergegeven in de Azure-portal of opgehaald met behulp van Microsoft-hulpprogramma's zoals Power BI of hulpprogramma's van derden. Auditlogboeken zijn beschikbaar via de portal, PowerShell, CLI en Rest-API.

> [!Note]
> Zie voor meer informatie over auditlogs [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Auditlogboeken zijn beschikbaar voor bewerkingen die op alle netwerkbronnen.


#### <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn metingen van de prestaties en de tellers die zijn verzameld gedurende een periode. Metrische gegevens zijn momenteel beschikbaar voor Application Gateway. Metrische gegevens kan worden gebruikt voor het activeren van waarschuwingen op basis van de drempelwaarde. Azure Application Gateway standaard controleert de status van alle resources in de back-end-pool en worden een resource als slecht beschouwd uit de groep automatisch verwijderd. Application Gateway blijft voor het bewaken van de exemplaren en terug naar de goede back-end-adrespool toegevoegd zodra ze beschikbaar komen en op statuscontroles reageren. Application gateway verzendt dat de statuscontroles met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen. Deze configuratie zorgt ervoor dat de test dezelfde poort die klanten zouden worden gebruikt om te verbinden met de back-end wordt getest.

> [!Note]
> Zie [Application Gateway Diagnostics](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) om weer te geven hoe metrische gegevens kunnen worden gebruikt om waarschuwingen te maken.

#### <a name="diagnostic-logs"></a>Diagnostische logboeken

Periodieke en spontaan gebeurtenissen worden gemaakt met netwerkbronnen en vastgelegd in de storage-accounts, verzonden naar een Event Hub of Log Analytics. Deze logboeken bieden inzicht in de status van een resource. Deze logboeken kunnen worden weergegeven in de hulpprogramma's zoals Power BI en Log Analytics. Als u wilt weten hoe u logboeken met diagnostische gegevens weergeven, gaat u naar [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Logboeken met diagnostische gegevens zijn beschikbaar voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routes, en [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Network Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor registratie in diagnoselogboek. In deze weergave kunt u in- en netwerkresources uitschakelen gemakkelijk en snel.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) is een service in Azure waarmee u uw cloud en on-premises omgevingen voor het onderhouden van hun beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

Log Analytics biedt de volgende oplossingen voor het bewaken van uw netwerken:

-   Netwerkprestatiemeter (NPM)

-   Azure Application Gateway analytics

-   Azure Network Security Group analytics

#### <a name="network-performance-monitor-npm"></a>Netwerkprestatiemeter (NPM)
De [Network Performance Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) management-oplossing is een Netwerkcontrole-oplossing die u de status, beschikbaarheid en bereikbaarheid van netwerken bewaakt.

Het wordt gebruikt voor het bewaken van verbinding tussen:

-   Openbare cloud en on-premises

-   -Datacenters en locaties van de gebruiker (filialen)

-   Subnetten die als host fungeert voor verschillende lagen van een toepassing met meerdere lagen.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Azure application gateway analytics in log analytics

De volgende logboeken worden voor Toepassingsgateways ondersteund:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

De volgende metrische gegevens worden ondersteund voor Application Gateways:

-   doorvoer van 5 minuten

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Azure network security group analytics in log analytics

De volgende logboeken worden ondersteund voor [netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** bevat vermeldingen voor welke NSG regels worden toegepast op VM's en functies op basis van MAC-adres-exemplaar. De status van deze regels worden elke 60 seconden worden verzameld.

- **NetworkSecurityGroupRuleCounter:** bevat vermeldingen voor het aantal keren dat elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over beveiliging vindt u enkele van de onderwerpen over onze uitgebreide beveiliging:

-   [Logboekanalyses voor Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Netwerken innovaties waarmee u onderbreking van de cloud uitbreiden](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: De netwerkondersteuning overschakelen software die door de algemene Microsoft-Cloud](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Hoe Microsoft de snelle en betrouwbare wereldwijde netwerk maakt](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Netwerk innovatie wordt geopend](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
