---
title: Azure-netwerk beveiliging | Microsoft Docs
description: Meer informatie over Cloud Computing Services die een breed scala aan reken instanties bevatten & Services die automatisch omhoog en omlaag kunnen worden geschaald om te voldoen aan de behoeften van uw toepassing of onderneming.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: db3f5aca0240c19f67d5d0775148d5eec76daa03
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726991"
---
# <a name="azure-network-security"></a>Azure-netwerkbeveiliging

We weten dat de veiligheid taak in de Cloud is en hoe belang rijk het is dat u nauw keurige en tijdige informatie over Azure-beveiliging vindt. Een van de beste redenen om Azure te gebruiken voor uw toepassingen en services is om te profiteren van de vele mogelijkheden van Azure voor beveiligings hulpprogramma's en-functies. Met deze hulpprogram ma's en mogelijkheden kunt u beveiligde oplossingen maken op het Azure-platform.

Microsoft Azure biedt vertrouwelijkheid, integriteit en beschik baarheid van klant gegevens, terwijl ook transparante verantwoording wordt ingeschakeld. Dit artikel, ' Azure-netwerk beveiliging ', is bedoeld om u te helpen beter inzicht te krijgen in de verzameling van netwerk beveiligings controles die zijn geïmplementeerd in Microsoft Azure vanuit het oogpunt van de klant. beschikbaar met Microsoft Azure.

Dit artikel is bedoeld om u te informeren over het grote aantal netwerk besturings elementen dat u kunt configureren om de beveiliging te verbeteren van de oplossingen die u in azure implementeert. Als u geïnteresseerd bent in wat micro soft doet om de netwerk infrastructuur van het Azure-platform zelf te beveiligen, raadpleegt u de sectie Azure-beveiliging in het [micro soft vertrouwens centrum](https://microsoft.com/en-us/trustcenter/cloudservices/azure).

## <a name="azure-platform"></a>Azure-platform

Azure is een openbaar Cloud service platform dat ondersteuning biedt voor een groot aantal besturings systemen, programmeer talen, frameworks, hulpprogram ma's, data bases en apparaten.  Het kan Linux-containers uitvoeren met docker-integratie; bouw apps met Java script, Python, .NET, PHP, Java en node. js; Maak back-ends voor iOS-, Android-en Windows-apparaten. Azure Cloud Services ondersteunt dezelfde technologieën die miljoenen ontwikkel aars en IT-professionals al vertrouwen op en vertrouwt.

Wanneer u IT-assets bouwt op of migreert naar, een open bare Cloud serviceprovider, bent u van plan op de mogelijkheden van die organisatie om uw toepassingen en gegevens te beschermen met de services en de besturings elementen die ze bieden om de beveiliging van uw cloud-gebaseerde assets te beheren.

De infra structuur van Azure is ontworpen met het oog op toepassingen om miljoenen klanten tegelijkertijd te hosten en biedt een betrouw bare basis waarop bedrijven kunnen voldoen aan de beveiligings vereisten. Bovendien biedt Azure u een uitgebreide verzameling Configureer bare beveiligings opties en de mogelijkheid om deze te beheren, zodat u de beveiliging kunt aanpassen zodat deze voldoet aan de unieke vereisten van de implementaties van uw organisatie.

## <a name="abstract"></a>Samenvatting

De open bare Cloud Services van micro soft bieden grootschalige Services en infra structuur, mogelijkheden op bedrijfs niveau en vele opties voor hybride connectiviteit. U kunt ervoor kiezen om toegang te krijgen tot deze services via internet of met Azure ExpressRoute, die een particuliere netwerk verbinding biedt. Met het Microsoft Azure-platform kunt u uw infra structuur naadloos uitbreiden naar de Cloud en architecturen met meerdere lagen bouwen. Daarnaast kunnen derden verbeterde mogelijkheden inschakelen door beveiligings Services en virtuele apparaten aan te bieden.

De netwerk services van Azure maximaliseren flexibiliteit, Beschik baarheid, tolerantie, veiligheid en integriteit per ontwerp. In dit technisch document vindt u informatie over de netwerk functies van Azure en informatie over hoe klanten de systeem eigen beveiligings functies van Azure kunnen gebruiken om hun informatie-assets te helpen beschermen.

De beoogde doel groepen voor dit technisch document zijn onder andere:

- Technische beheerders, netwerk beheerders en ontwikkel aars die op zoek zijn naar beveiligings oplossingen die beschikbaar zijn en worden ondersteund in Azure.

-   Kmo's of zakelijke proces managers die een overzicht moeten krijgen van de Azure-netwerk technologieën en-services die relevant zijn voor discussies over netwerk beveiliging in de open bare Azure-Cloud.

## <a name="azure-networking-big-picture"></a>Azure-netwerken voor grote afbeeldingen
Microsoft Azure bevat een robuuste netwerk infrastructuur ter ondersteuning van uw toepassings-en service connectiviteits vereisten. De netwerk verbinding is mogelijk tussen bronnen in azure, tussen on-premises en Azure gehoste resources, en naar en van Internet en Azure.

![Azure-netwerken voor grote afbeeldingen](./media/network-security/azure-network-security-fig-1.png)

Met de [Azure-netwerk infrastructuur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) kunt u Azure-resources veilig met elkaar verbinden met virtuele netwerken (VNets). Een VNet is een weer gave van uw eigen netwerk in de Cloud. Een VNet is een logische isolatie van het Azure-Cloud netwerk dat specifiek is voor uw abonnement. U kunt VNets verbinding maken met uw on-premises netwerken.

Azure biedt ondersteuning voor speciale WAN-koppelings connectiviteit met uw on-premises netwerk en een Azure-Virtual Network met [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). De koppeling tussen Azure en uw site maakt gebruik van een speciale verbinding die niet via het open bare Internet gaat. Als uw Azure-toepassing wordt uitgevoerd in meerdere data centers, kunt u [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) gebruiken om aanvragen van gebruikers op intelligente wijze te routeren in verschillende exemplaren van de toepassing. U kunt ook verkeer routeren naar services die niet worden uitgevoerd in azure als ze toegankelijk zijn via internet.

## <a name="enterprise-view-of-azure-networking-components"></a>Enter prise-weer gave van Azure-netwerk onderdelen
Azure heeft veel netwerk onderdelen die relevant zijn voor netwerk beveiligings discussies. We beschrijven deze netwerk onderdelen en richten zich op de beveiligings problemen die eraan zijn gerelateerd.

> [!Note]
> Niet alle aspecten van Azure Networking worden beschreven: we bespreken alleen die als draaiende in het plannen en ontwerpen van een veilige netwerk infrastructuur rond uw services en toepassingen die u in azure implementeert.

In dit artikel komen de volgende mogelijkheden voor Azure-netwerk ondernemingen aan bod:

-   Basis netwerk connectiviteit

-   Hybride connectiviteit

-   Besturingselementen voor beveiliging

-   Netwerk validatie

### <a name="basic-network-connectivity"></a>Basis netwerk connectiviteit

Met de [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) -service kunt u op een veilige manier verbinding maken tussen Azure-resources en virtuele netwerken (VNet). Een VNet is een weer gave van uw eigen netwerk in de Cloud. Een VNet is een logische isolatie van de Azure-netwerk infrastructuur die is toegewezen aan uw abonnement. U kunt VNets ook verbinden met elkaar en met uw on-premises netwerken met behulp van site-naar-site-Vpn's en specifieke [WAN-koppelingen](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Basis netwerk connectiviteit](./media/network-security/azure-network-security-fig-2.png)

Met de uitleg over het gebruik van Vm's voor het hosten van servers in azure, is de vraag hoe deze Vm's verbinding maken met een netwerk. Het antwoord is dat Vm's verbinding maken met een [Azure-Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Virtuele netwerken van Azure zijn net als de virtuele netwerken die u on-premises gebruikt met uw eigen virtualisatieoplossingen, zoals micro soft Hyper-V of VMware.

#### <a name="intra-vnet-connectivity"></a>Intra-VNet-connectiviteit

U kunt VNets met elkaar verbinden, waardoor resources die zijn verbonden met VNet met elkaar communiceren via VNets. U kunt een of beide van de volgende opties gebruiken om VNets met elkaar te verbinden:

- **Peering** Hiermee kunnen resources die zijn verbonden met verschillende Azure-VNets binnen dezelfde Azure-locatie communiceren met elkaar. De band breedte en latentie over het VNet zijn hetzelfde als wanneer de resources zijn verbonden met hetzelfde VNet. Lees voor meer informatie over peering de [peering van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

  ![Peering](./media/network-security/azure-network-security-fig-3.png)

- **VNet-naar-VNet-verbinding:** Hiermee kunnen resources die zijn verbonden met verschillende Azure VNet binnen dezelfde of verschillende Azure-locaties worden aangesloten. In tegens telling tot peering is de band breedte beperkt tussen VNets, omdat verkeer moet stromen via een Azure-VPN Gateway.

![VNet-naar-VNet-verbinding](./media/network-security/azure-network-security-fig-4.png)


Lees het [artikel een vnet-naar-vnet-verbinding configureren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json)voor meer informatie over het verbinden van VNets met een vnet-naar-vnet-verbinding.

#### <a name="azure-virtual-network-capabilities"></a>Mogelijkheden van Azure Virtual Network:

Zoals u kunt zien, biedt Azure Virtual Network virtuele machines een verbinding met het netwerk, zodat ze op een veilige manier verbinding kunnen maken met andere netwerk bronnen. Basis connectiviteit is echter gewoon het begin. De volgende mogelijkheden van de Azure Virtual Network-service bieden beveiligings kenmerken van de Azure-Virtual Network:

-   Isolatie

-   Internetconnectiviteit

-   Azure-bron connectiviteit

-   VNet-connectiviteit

-   On-premises connectiviteit

-   Verkeer filteren

-   Routering

**Zonder**

VNets zijn van elkaar [geïsoleerd](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) . U kunt afzonderlijke VNets maken voor ontwikkeling, testen en productie die gebruikmaken van dezelfde [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) -adres blokken. U kunt daarentegen meerdere VNets maken die gebruikmaken van verschillende CIDR-adres blokken en netwerken met elkaar verbinden. U kunt een VNet in meerdere subnetten segmenteren.

Azure biedt interne naam omzetting voor Vm's en [Cloud Services](https://azure.microsoft.com/services/cloud-services/) rolinstanties die zijn verbonden met een VNet. U kunt desgewenst een VNet configureren voor het gebruik van uw eigen DNS-servers, in plaats van de interne naam omzetting van Azure te gebruiken.

U kunt meerdere VNets implementeren binnen elk Azure- [abonnement](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) en Azure- [regio](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Elk VNet is geïsoleerd van andere vnet's. Voor elk VNet kunt u het volgende doen:

-   Een aangepaste persoonlijke IP-adresruimte opgeven met behulp van openbare en persoonlijke adressen (RFC 1918). Azure wijst resources die zijn verbonden met het VNet een persoonlijk IP-adres toe uit de adres ruimte die u toewijst.

-   Segmenteer het VNet in een of meer subnetten en wijs een deel van de VNet-adres ruimte toe aan elk subnet.

-   Gebruik door Azure opgegeven naam omzetting of geef uw eigen DNS-server op voor gebruik door resources die zijn verbonden met een VNet. Lees de [naam omzetting voor virtuele machines en Cloud Services](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)voor meer informatie over naam omzetting in VNets.

**Verbinding met internet**

Alle [Azure virtual machines-exemplaren (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) en Cloud Services rolinstantie die zijn verbonden met een VNet hebben standaard toegang tot het internet. U kunt indien nodig ook binnenkomende toegang tot specifieke bronnen inschakelen. (VM) en Cloud Services rolinstantie die zijn verbonden met een VNet hebben standaard toegang tot het internet. U kunt indien nodig ook binnenkomende toegang tot specifieke bronnen inschakelen.

Alle resources die zijn verbonden met een VNet, hebben standaard een uitgaande verbinding met het internet. Het privé IP-adres van de bron is een bron netwerk adres vertaald (SNAT) naar een openbaar IP-adres door de Azure-infra structuur. U kunt de standaard connectiviteit wijzigen door aangepaste route ring en verkeer filtering te implementeren. Lees de [uitleg over uitgaande verbindingen in azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json)voor meer informatie over uitgaande internet connectiviteit.

Als u binnenkomende naar Azure-resources via internet wilt communiceren of als u uitgaand verkeer naar Internet wilt communiceren zonder SNAT, moet aan een resource een openbaar IP-adres worden toegewezen. Lees de [open bare IP-adressen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)voor meer informatie over open bare IP-adressen.

**Azure-bron connectiviteit**

[Azure-resources](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) , zoals Cloud Services en vm's, kunnen worden verbonden met hetzelfde VNet. De bronnen kunnen met elkaar worden verbonden met behulp van privé-IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaard routering tussen subnetten, VNets en on-premises netwerken, zodat u geen routes hoeft te configureren en te beheren.

U kunt verschillende Azure-resources verbinden met een VNet, zoals Virtual Machines (VM), Cloud Services, App Service omgevingen en Virtual Machine Scale Sets. Vm's maken verbinding met een subnet binnen een VNet via een netwerk interface (NIC). Lees de [netwerk interfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface)voor meer informatie over nic's.

**VNet-connectiviteit**

[VNets](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kunnen met elkaar worden verbonden, waardoor resources die zijn verbonden met een VNet, kunnen communiceren met een wille keurige resource in een ander vnet.

U kunt VNets met elkaar verbinden, waardoor resources die zijn verbonden met VNet met elkaar communiceren via VNets. U kunt een of beide van de volgende opties gebruiken om VNets met elkaar te verbinden:

- **Peering** Hiermee kunnen resources die zijn verbonden met verschillende Azure-VNets binnen dezelfde Azure-locatie communiceren met elkaar. De band breedte en latentie in de VNets zijn hetzelfde als wanneer de resources zijn verbonden met dezelfde VNet.To meer informatie over peering, lees de peering van het [virtuele netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-naar-VNet-verbinding:** Hiermee kunnen resources die zijn verbonden met verschillende Azure VNet binnen dezelfde of verschillende Azure-locaties worden aangesloten. In tegens telling tot peering is de band breedte beperkt tussen VNets, omdat verkeer moet stromen via een Azure-VPN Gateway. Voor meer informatie over het verbinden van VNets met een VNet-naar-VNet-verbinding. Lees de [een vnet-naar-VNet-verbinding configureren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie.

**On-premises connectiviteit**

VNets kan worden verbonden met [on-premises](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) netwerken via particuliere netwerk verbindingen tussen uw netwerk en Azure, of via een site-naar-site-VPN-verbinding via internet.

U kunt uw on-premises netwerk verbinden met een VNet met behulp van een combi natie van de volgende opties:

- **Punt-naar-site virtueel particulier netwerk (VPN):** Tot stand gebracht tussen één PC die is verbonden met uw netwerk en het VNet. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. De verbinding maakt gebruik van het SSTP-protocol om versleutelde communicatie via internet te bieden tussen de PC en het VNet. De latentie voor een punt-naar-site-VPN kan onvoorspelbaar zijn omdat het verkeer het Internet passeert.

- **Site-to-site VPN:** Tot stand gebracht tussen uw VPN-apparaat en een Azure-VPN Gateway. Dit verbindings type zorgt ervoor dat elke on-premises resource die u toemachtigt toegang krijgt tot een VNet. De verbinding is een IPsec/IKE VPN die versleutelde communicatie via internet verzorgt tussen uw on-premises apparaat en de Azure VPN-gateway. De latentie voor een site-naar-site-verbinding is onvoorspelbaar omdat het verkeer het Internet passeert.

- **Azure ExpressRoute:** Wordt tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer gaat niet via internet. De latentie voor een ExpressRoute-verbinding is voorspelbaar omdat verkeer niet via internet verloopt. Lees de diagrammen van de [verbindings topologie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json)voor meer informatie over de vorige verbindings opties.

**Verkeer filteren**

VM-en Cloud Services- [rolinstanties kunnen worden](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) gefilterd op inkomend en uitgaand door het bron-IP-adres en de poort, het doel-IP-adres en de poort en het protocol.

U kunt netwerkverkeer filteren tussen subnetten met behulp van een of beide van de volgende opties:

- **Netwerk beveiligings groepen (NSG):** Elk NSG kan meerdere binnenkomende en uitgaande beveiligings regels bevatten waarmee u verkeer kunt filteren op bron-en doel-IP-adres, poort en protocol. U kunt een NSG Toep assen op elke NIC in een VM. U kunt ook een NSG Toep assen op het subnet een NIC of een andere Azure-resource, waarmee verbinding wordt gemaakt. Lees de [netwerk beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)voor meer informatie over nsg's.

- **Virtual Network apparaten:** Een virtueel netwerk apparaat is een VM met software die een netwerk functie uitvoert, zoals een firewall. Bekijk een lijst met beschik bare Nva's in azure Marketplace. Nva's zijn ook beschikbaar die WAN-optimalisatie en andere functies voor netwerk verkeer bieden. Nva's worden meestal gebruikt met door de gebruiker gedefinieerde of BGP-routes. U kunt ook een NVA gebruiken om verkeer tussen VNets te filteren.

**Routering**

U kunt eventueel de standaard routering van Azure negeren door uw eigen routes te configureren of door BGP-routes via een netwerk gateway te gebruiken.

Azure maakt route tabellen waarmee resources die zijn verbonden met een subnet in een VNet, standaard kunnen communiceren met elkaar. U kunt een of beide van de volgende opties implementeren om de standaardroutes die Azure maakt te onderdrukken:

- **Door de gebruiker gedefinieerde routes:** U kunt aangepaste routetabellen maken met routes die bepalen waar verkeer naartoe wordt doorgestuurd voor elk subnet. Lees de door de [gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)voor meer informatie over door de gebruiker gedefinieerde routes.

- **BGP-routes:** Als u uw VNet verbindt met uw on-premises netwerk met behulp van een Azure VPN Gateway-of ExpressRoute-verbinding, kunt u BGP-routes door geven aan uw VNets.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Hybride Internet connectiviteit: Verbinding maken met een on-premises netwerk
U kunt uw on-premises netwerk verbinden met een VNet met behulp van een combi natie van de volgende opties:

-   Internetconnectiviteit

-   Punt-naar-site-VPN (P2S VPN)

-   Site-naar-site VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internet verbinding

Zoals de naam adviseert, zorgt Internet connectiviteit ervoor dat uw workloads toegankelijk zijn via internet, doordat u verschillende open bare eind punten beschikbaar maakt voor werk belastingen die in het virtuele netwerk wonen. Deze werk belastingen kunnen worden blootgesteld met [Internet gerichte Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) of eenvoudigweg een openbaar IP-adres toewijzen aan de virtuele machine. Op deze manier is het mogelijk om alles op het Internet te bereiken waarmee de virtuele machine kan worden bereikt, op voor waarde dat een hardwarefirewall, [netwerk beveiligings groepen (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)en door de [gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) zich kunnen voordoen.

In dit scenario kunt u een toepassing weer geven die openbaar moet zijn voor Internet en er overal of vanaf specifieke locaties verbinding mee kunnen maken, afhankelijk van de configuratie van uw workloads.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Punt-naar-site-VPN of site-naar-site-VPN
Deze twee vallen in dezelfde categorie. Beide moeten uw VNet een VPN Gateway hebben en u kunt verbinding maken met een VPN-client voor uw werk station als onderdeel van de [punt-naar-site-configuratie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) of u kunt uw on-premises [VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) zo configureren dat een site-naar-site kan worden beëindigd VPN. Op deze manier kunnen on-premises apparaten verbinding maken met bronnen binnen het VNet.

Met een punt-naar-site-configuratie (P2S) kunt u een beveiligde verbinding maken tussen een afzonderlijke clientcomputer en een virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol).

![Punt-naar-Site-VPN](./media/network-security/azure-network-security-fig-5.png)

Punt-naar-site-verbindingen zijn handig wanneer u verbinding wilt maken met uw VNet vanaf een externe locatie, zoals vanaf thuis of vanuit een conferentie centrum, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een virtueel netwerk.

Voor P2S-verbindingen hebt u geen VPN-apparaat of een openbaar IP-adres nodig. U brengt de VPN-verbinding tot stand vanaf de clientcomputer. Daarom wordt P2S niet aanbevolen om verbinding te maken met Azure voor het geval u een permanente verbinding nodig hebt van een groot aantal on-premises apparaten en computers naar uw Azure-netwerk.

![Site-to-Site VPN](./media/network-security/azure-network-security-fig-6.png)

> [!Note]
> Zie voor meer informatie over punt-naar-site-verbindingen de [punt-naar-site-VA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk.

Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Deze verbinding vindt plaats via internet en geeft u de mogelijkheid om gegevens te ' tunnelen ' in een versleutelde koppeling tussen uw netwerk en Azure. Site-naar-site-VPN is een veilige, rijpere technologie die is geïmplementeerd door ondernemingen van elke omvang voor tien tallen. Tunnel versleuteling wordt uitgevoerd met de [IPSec-tunnel modus](https://technet.microsoft.com/library/cc786385.aspx).

Site-naar-site-VPN is een betrouw bare, betrouw bare en opgezette technologie en verkeer binnen de tunnel gaat via internet. Daarnaast is de band breedte relatief beperkt tot een maximum van 200 Mbps.

Als u een uitzonderlijk niveau van beveiliging of prestaties voor uw cross-premises verbindingen nodig hebt, raden we u aan Azure ExpressRoute te gebruiken voor uw cross-premises-connectiviteit. ExpressRoute is een specifieke WAN-verbinding tussen uw on-premises locatie of een Exchange-hosting provider. Omdat dit een telecommunicatie-verbinding is, worden uw gegevens niet via internet gereisd en worden ze daarom niet blootgesteld aan de potentiële Risico's die inherent zijn aan Internet communicatie.

> [!Note]
> Zie over [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)voor meer informatie over VPN-gateways.

#### <a name="dedicated-wan-link"></a>Toegewezen WAN-verbinding
Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken uitbreiden naar Azure via een speciale persoonlijke verbinding die wordt vereenvoudigd door een connectiviteits provider.

ExpressRoute-verbindingen gaan niet via het openbare internet. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.

![ Toegewezen WAN-verbinding](./media/network-security/azure-network-security-fig-7.png)

> [!Note]
> Zie [ExpressRoute Connectivity modellen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) en [ExpressRoute Technical Overview](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)(Engelstalig) voor informatie over het verbinden van uw netwerk met micro soft met behulp van ExpressRoute.

Net als bij de site-naar-site VPN-opties kunt u met ExpressRoute ook verbinding maken met bronnen die niet noodzakelijkerwijs in één VNet zijn. Afhankelijk van de SKU kunt u bijvoorbeeld verbinding maken met 10 VNets. Als u de [Premium-invoeg toepassing](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)hebt, zijn er verbindingen met maxi maal 100 VNets mogelijk, afhankelijk van de band breedte. Als u meer wilt weten over de manier waarop deze typen verbindingen eruitzien, kunt u [verbindings topologie diagrammen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json)lezen.

### <a name="security-controls"></a>Beveiligings controles
Een Azure-Virtual Network biedt een veilig, logisch netwerk dat is geïsoleerd van andere virtuele netwerken en ondersteunt veel beveiligings controles die u gebruikt in uw on-premises netwerken. Klanten maken hun eigen structuur met behulp van: subnetten — ze gebruiken hun eigen privé IP-adres bereik, configureren route tabellen, netwerk beveiligings groepen, Toegangs beheer lijsten (Acl's), gateways en virtuele apparaten om hun werk belastingen in de Cloud uit te voeren.

Hieronder vindt u beveiligings besturings elementen die u op uw virtuele Azure-netwerken kunt gebruiken:

-   Netwerk toegangs beheer

-   Door de gebruiker gedefinieerde routes

-   Netwerk beveiligings apparaat

-   Application Gateway

-   Firewall voor Azure Web Application

-   Beheer van netwerk beschikbaarheid

#### <a name="network-access-controls"></a>Netwerk toegangs beheer
De Azure Virtual Network (VNet) is de hoek steen van het Azure-netwerk model en biedt isolatie en bescherming, de [netwerk beveiligings groepen (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) zijn het belangrijkste hulp programma dat u gebruikt om regels voor netwerk verkeer af te dwingen en te beheren op netwerk niveau.

![ Netwerk toegangs beheer](./media/network-security/azure-network-security-fig-8.png)


U kunt de toegang beheren door communicatie toe te staan of te weigeren tussen de werk belastingen binnen een virtueel netwerk, van systemen op de netwerken van de klant via cross-premises-connectiviteit of rechtstreekse Internet communicatie.

In het diagram bevinden zowel VNets als Nsg's zich in een specifieke laag in de algemene beveiligings stack van Azure, waarbij Nsg's, UDR en virtuele netwerk apparaten kunnen worden gebruikt om beveiligings grenzen te maken om de toepassings implementaties in het beveiligde netwerk te beveiligen.

Nsg's een 5-tuple gebruiken om verkeer te evalueren (en worden gebruikt in de regels die u configureert voor de NSG):

-   [IP-adres van bron en doel](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Bron-en doel poort](https://technet.microsoft.com/library/dd197515)

-   Protocol: [Transmission Control Protocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) of [UDP (User Data gram Protocol)](https://technet.microsoft.com/library/cc940034.aspx)

Dit betekent dat u de toegang tot een enkele virtuele machine en een groep virtuele machines, of een enkele virtuele machine naar een andere virtuele subnet, of tussen de hele subnetwerken kunt beheren. Denk eraan dat dit eenvoudige stateful pakket filtering is, geen volledige pakket inspectie. Er is geen protocol validatie of netwerk niveau-ID'S of IP'S-mogelijkheden in een netwerk beveiligings groep.

Een NSG wordt geleverd met een aantal ingebouwde regels waar u rekening mee moet houden. Dit zijn:

-   **Al het verkeer binnen een specifiek virtueel netwerk toestaan:** Alle Vm's op dezelfde Azure-Virtual Network kunnen met elkaar communiceren.

-   **Azure Load Balancing toestaan op inkomend:**  deze regel staat verkeer toe van elk bron adres naar een doel adres voor de Azure-Load Balancer.

-   **Alle inkomende gegevens weigeren:**  deze regel blokkeert alle verkeer van Internet dat u expliciet hebt toegestaan.

-   **Alle uitgaande verkeer naar Internet toestaan:** Met deze regel kunnen Vm's verbindingen met Internet initiëren. Als u niet wilt dat deze verbindingen worden gestart, moet u een regel maken om deze verbindingen te blok keren of geforceerde tunneling af te dwingen.

#### <a name="system-routes-and-user-defined-routes"></a>Systeem routes en door de gebruiker gedefinieerde routes

Wanneer u virtuele machines (Vm's) aan een virtueel netwerk (VNet) in azure toevoegt, ziet u dat de Vm's automatisch met elkaar kunnen communiceren via het netwerk. U hoeft geen gateway op te geven, ook niet als de virtuele machines tot verschillende subnetten behoren.

Hetzelfde geldt voor de communicatie tussen VM's en internet. Als er een hybride verbinding is tussen Azure en uw eigen datacentrum, is er zelfs communicatie met uw on-premises netwerk mogelijk.

![Systeemroutes](./media/network-security/azure-network-security-fig-9.png)

Deze communicatiestroom is mogelijk omdat de IP-verkeersstromen in Azure wordt gedefinieerd met behulp van een reeks systeemroutes. Systeemroutes bepalen de communicatiestroom in de volgende scenario's:

-   Binnen een en hetzelfde subnet.

-   Van het ene naar het andere subnet binnen een VNet.

-   Van virtuele machines naar internet.

-   Van het ene naar het andere VNet via een VPN-gateway.

-   Van een VNet naar een ander VNet via VNet-peering ([service ketening](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Van een VNet naar uw on-premises netwerk via een VPN-gateway.

Veel ondernemingen hebben strenge vereisten voor beveiliging en naleving waarvoor een on-premises inspectie van alle netwerk pakketten is vereist om specifieke beleids regels af te dwingen. Azure biedt een mechanisme met [](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) de naam geforceerde tunneling die verkeer van de virtuele machines naar de on-premises routeert door een aangepaste route of [Border Gateway Protocol BGP-](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) advertenties te maken via ExpressRoute of VPN.

Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes (UDR). Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de Azure VPN-gateway.

Het volgende gedeelte bevat de huidige beperking van de tabel en routes de routering voor een Azure-netwerk:

- Elk virtueel netwerksubnet heeft een ingebouwde, systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:

  -  **Lokale VNet-routes:** Rechtstreeks naar de doel-Vm's in hetzelfde virtuele netwerk

  - **On-premises routes:** Naar de Azure VPN-gateway

  -  **Standaard route:** Rechtstreeks op internet. Pakketten die bestemd zijn voor de privé-IP-adressen die niet worden gedekt door de vorige twee routes, worden verwijderd.

- Met de release van door de gebruiker gedefinieerde routes kunt u een routerings tabel maken om een standaard route toe te voegen en vervolgens de routerings tabel koppelen aan uw VNet-subnet om geforceerde Tunneling in te scha kelen voor deze subnetten.

- U moet een 'standaard-site"tussen de cross-premises lokale sites die zijn verbonden met het virtuele netwerk instellen.

- Geforceerde tunneling moet worden gekoppeld aan een VNet met een VPN-gateway voor dynamische routering (niet een statische gateway genoemd).

- ExpressRoute geforceerde tunneling is niet geconfigureerd via dit mechanisme, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies.

> [!Note]
> Zie de [ExpressRoute-documentatie](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

#### <a name="network-security-appliances"></a>Netwerk beveiligings apparaten
Hoewel netwerk beveiligings groepen en door de gebruiker gedefinieerde routes een zekere mate van netwerk beveiliging kunnen bieden op het netwerk en de transport lagen van het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), zijn er situaties waarin u wilt dat u de beveiliging op een hoger niveau van de netwerk stack. In dergelijke situaties wordt u aangeraden virtuele netwerk beveiligings apparaten te implementeren die worden meegeleverd door Azure-partners.

![Netwerk beveiligings apparaten](./media/network-security/azure-network-security-fig-10.png)

Azure-netwerk beveiligings apparaten verbeteren VNet-beveiliging en netwerk functies, en ze zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com)van talloze leveranciers. Deze virtuele beveiligings apparaten kunnen worden geïmplementeerd om het volgende te bieden:

-   Maxi maal beschik bare firewalls

-   Inbreuk op voor komen

-   Inbraak detectie

-   Firewalls voor webtoepassingen (Waf's)

-   WAN-optimalisatie

-   Routering

-   Taakverdeling

-   VPN

-   Certificaatbeheer

-   Active Directory

-   Meervoudige verificatie

#### <a name="application-gateway"></a>Toepassingsgateway

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) is een speciaal virtueel apparaat dat een ADC (Application Delivery controller) als een service biedt.

 ![Application Gateway](./media/network-security/azure-network-security-fig-11.png)

Met Application Gateway kunt u de prestaties en beschik baarheid van webfarms optimaliseren door de CPU-intensieve SSL-beëindiging te offloaden naar de Application Gateway (SSL-offloading). Het biedt ook andere Layer 7-routerings mogelijkheden, waaronder:

-   Round robin-distributie van binnenkomend verkeer

-   Sessie affiniteit op basis van cookies

-   Op URL-pad gebaseerde route ring

-   De mogelijkheid om meerdere websites achter één Application Gateway te hosten


Er wordt ook een [Web Application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) als onderdeel van de toepassings gateway gegeven. Dit biedt beveiliging voor webtoepassingen van veelvoorkomende beveiligings problemen en aanvallen. Application Gateway kunnen worden geconfigureerd als een Internet gateway, alleen interne gateway, of een combi natie van beide.

Application Gateway WAF kan worden uitgevoerd in de detectie-of preventie modus. Een veelvoorkomende use-case is dat beheerders in de detectie modus worden uitgevoerd om verkeer voor schadelijke patronen te observeren. Als er mogelijke aanvallen worden gedetecteerd, blokkeert de preventie modus verdacht uitgaand binnenkomend verkeer.

 ![Application Gateway](./media/network-security/azure-network-security-fig-12.png)

Daarnaast helpt Application Gateway WAF u bij het bewaken van webtoepassingen tegen aanvallen met behulp van een real-time WAF-logboek dat is geïntegreerd met [Azure monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) en [Azure Security Center](https://azure.microsoft.com/services/security-center/) voor het volgen van WAF-waarschuwingen en om eenvoudig trends te bewaken.

Het logboek met JSON-indeling gaat rechtstreeks naar het opslag account van de klant. U hebt volledige controle over deze logboeken en u kunt uw eigen Bewaar beleid Toep assen.

U kunt deze logboeken ook opnemen in uw eigen Analytics-systeem met behulp van [Azure log Integration](https://aka.ms/AzLog). WAF-logboeken zijn ook geïntegreerd met [Azure monitor](/azure/log-analytics/log-analytics-overview) -logboeken, zodat u Azure monitor-Logboeken kunt gebruiken om geavanceerde verfijnde query's uit te voeren.

#### <a name="azure-web-application-firewall-waf"></a>Azure Web Application Firewall (WAF)

Webtoepassingen zijn in toenemende mate doelwit van kwaad aardige aanvallen die gebruikmaken van veelvoorkomende bekende beveiligings problemen, zoals SQL-injectie, cross-site scripting aanvallen en andere aanvallen die worden weer gegeven in de [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Voor komen dat dergelijke exploits in de toepassing een strenge onderhoud, patching en bewaking op meerdere lagen van de toepassings topologie vereist.

 ![Azure Web Application firewall (WAF)](./media/network-security/azure-network-security-fig-13.png)

Een gecentraliseerde [Web Application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) kan worden beveiligd tegen aanvallen op internet en vereenvoudigt het beveiligings beheer zonder dat er wijzigingen in de toepassing zijn vereist.

Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

#### <a name="network-availability-controls"></a>Besturings elementen voor netwerk beschikbaarheid

Er zijn verschillende opties voor het distribueren van netwerkverkeer met behulp van Microsoft Azure. Deze opties werken verschillend. Ze hebben een eigen functieset en ondersteunen verschillende scenario's. Ze kunnen elk afzonderlijk worden gebruikt, maar ook worden gecombineerd.

Hieronder ziet u de besturings elementen voor netwerk beschikbaarheid:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure Load Balancer**

Biedt hoge Beschik baarheid en netwerk prestaties voor uw toepassingen. Het is een laag 4 (TCP, UDP) load balancer waarmee binnenkomend verkeer wordt verdeeld over in orde zijnde instanties van services die zijn gedefinieerd in een set met gelijke taak verdeling.

 ![Azure Load Balancer](./media/network-security/azure-network-security-fig-14.png)


Azure Load Balancer kunnen worden geconfigureerd voor het volgende:

-   Taak verdeling van binnenkomend Internet verkeer naar virtuele machines. Deze configuratie wordt ook wel [Internet gerichte taak verdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview)genoemd.

-   Taak verdeling van verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in Cloud Services of tussen lokale computers en virtuele machines in een cross-premises virtueel netwerk. Deze configuratie wordt ook wel [interne taak verdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview)genoemd.

-   Extern verkeer door sturen naar een specifieke virtuele machine.

Alle resources in de Cloud moeten een openbaar IP-adres hebben dat bereikbaar is vanaf internet. De Cloud infrastructuur in azure maakt gebruik van niet-Routeer bare IP-adressen voor de bijbehorende resources. Azure gebruikt Network Address Translation (NAT) met open bare IP-adressen om te communiceren met internet.

 **Toepassings gateway**

 Application Gateway werkt in de toepassingslaag (laag 7 in de referentie stack van het OSI-netwerk). Deze service fungeert als een omgekeerde proxyservice, beëindigt de clientverbinding en stuurt aanvragen door naar back-endeindpunten.

 **Traffic Manager**

Met Microsoft Azure Traffic Manager kunt u de distributie van gebruikers verkeer voor service-eind punten in verschillende data centers beheren. Service-eind punten die worden ondersteund door Traffic Manager zijn onder andere Azure-Vm's, Web Apps en Cloud Services. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn.

Traffic Manager gebruikt de Domain Name System (DNS) om client aanvragen door te sturen naar het meest geschikte eind punt op basis van een [routerings methode voor verkeer](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) en de status van de eind punten. Traffic Manager biedt een reeks methoden voor het routeren van verkeer voor verschillende toepassings behoeften, [controle](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)van de eindpunt status en automatische failover. Traffic Manager is bestand tegen storingen, waaronder het uitvallen van een hele Azure-regio.

Met Azure Traffic Manager kunt u de distributie van verkeer over de eind punten van uw toepassing beheren. Een eindpunt is een internetgerichte service die binnen of buiten Azure wordt gehost.

Traffic Manager biedt twee belang rijke voor delen:

-   Distributie van verkeer op basis van een van de verschillende [methoden voor het routeren van verkeer](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Voortdurende bewaking van de status van het eind punt](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) en automatische failover wanneer eind punten mislukken.

Wanneer een client verbinding probeert te maken met een service, moet de DNS-naam van de service eerst worden omgezet in een IP-adres. De client maakt vervolgens verbinding met dat IP-adres om toegang te krijgen tot de service. Traffic Manager DNS gebruikt om clients te leiden naar specifieke service-eind punten op basis van de regels van de methode voor het routeren van verkeer. Clients maken rechtstreeks verbinding met het geselecteerde eind punt. Traffic Manager is geen proxy of gateway. Het verkeer dat door de client en de service wordt door Traffic Manager wordt niet weer gegeven.

### <a name="azure-network-validation"></a>Azure-netwerk validatie

Azure-netwerk validatie is om ervoor te zorgen dat het Azure-netwerk werkt omdat het is geconfigureerd en validatie kan worden uitgevoerd met behulp van de services en functies die beschikbaar zijn voor het bewaken van het netwerk. Met Azure Network Watcher hebt u toegang tot een verdwaald van logboek registratie-en diagnostische mogelijkheden waarmee u inzicht krijgt in de prestaties en status van uw netwerk. Deze mogelijkheden zijn toegankelijk via de portal, Power shell, CLI, rest API en SDK.

Azure Operational Security heeft betrekking op de services, besturings elementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere assets in Microsoft Azure. Azure Operational Security is gebaseerd op een Framework met de kennis die is opgedaan via een aantal mogelijkheden die uniek zijn voor micro soft, waaronder micro soft Security Development Lifecycle (SDL), het micro soft Security Response Center-programma en dieper in op het landschap van de Cyber beveiliging bedreigingen.

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure Resource Manager

De personen en processen die Microsoft Azure gebruiken, zijn wellicht de belangrijkste beveiligings functie van het platform. In deze sectie worden de functies beschreven van de wereld wijde datacenter infrastructuur van micro soft waarmee u de beveiliging, continuïteit en privacy kunt verbeteren en onderhouden.

De infrastructuur voor uw toepassing bestaat meestal uit veel onderdelen, zoals een virtuele machine, een opslagaccount en een virtueel netwerk, of een webtoepassing, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. Met Azure Resource Manager kunt u met de resources als groep in uw oplossing werken.

U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

**De voor delen van het gebruik van Resource Manager**

Resource Manager biedt diverse voordelen:

-   U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

-   U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

-   U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

-   U kunt de afhankelijkheden tussen resources definiëren, zodat deze in de juiste volg orde worden geïmplementeerd.

-   U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.

-   U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

-   U kunt de facturering van uw organisatie verduidelijken door de kosten voor een groep met resources delen weer te geven.

> [!Note]
> Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u het eerdere implementatiemodel hebt gebruikt en meer te weten wilt komen over de wijzigingen, leest u [Resource Manager-implementatie en klassieke implementatie begrijpen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Logboek registratie en controle van Azure-netwerken

Azure biedt veel hulpprogram ma's om netwerk beveiligings gebeurtenissen te bewaken, te voor komen, te detecteren en erop te reageren. Enkele van de krach tigste hulpprogram ma's die voor u beschikbaar zijn, zijn onder andere:

-   Network Watcher

-   Bewaking van netwerk resource niveau

-   Azure Monitor-logboeken

### <a name="network-watcher"></a>Netwerk-watcher

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) op scenario's gebaseerde bewaking wordt met de functies in Network Watcher gegeven. Deze service omvat pakket opname, volgende hop, IP-stroom controleren, beveiligings groep weer geven, NSG stroom Logboeken. Bewaking op scenario niveau biedt een end-to-end weer gave van netwerk bronnen in tegens telling tot afzonderlijke netwerk bron bewaking.

 ![Network Watcher](./media/network-security/azure-network-security-fig-15.png)

Network Watcher is een regionale service waarmee u voor waarden kunt controleren en diagnosticeren op het niveau van een netwerk scenario in, naar en Azure. Met behulp van de hulpprogram ma's voor netwerk diagnose en visualisatie die beschikbaar zijn bij Network Watcher, kunt u uw netwerk in azure begrijpen, vaststellen en er inzicht in krijgen.

Network Watcher heeft momenteel de volgende mogelijkheden:

#### <a name="topology"></a>Topologie

De [topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) retourneert een grafiek met netwerk bronnen in een virtueel netwerk. In de grafiek ziet u de onderlinge verbinding tussen de bronnen die het end-to-end-netwerk connectiviteit vertegenwoordigen. In de portal retourneert topologie de resource-objecten op basis van het virtuele netwerk. De relaties worden weer gegeven op basis van lijnen tussen de resources buiten de Network Watcher regio, zelfs als deze niet in de resource groep worden getoond. De resources die in de portal weergave worden geretourneerd, zijn een subset van de netwerk onderdelen die in de grafiek worden weer gegeven. Als u de volledige lijst met netwerk bronnen wilt zien, kunt u [Power shell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) of [rest](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest)gebruiken.

Als bronnen worden geretourneerd, wordt de verbinding tussen de resources gemodelleerd onder twee relaties.

- **Containment** -Virtual Network bevat een subnet dat een NIC bevat.

- **Gekoppeld** : een NIC is gekoppeld aan een virtuele machine.

#### <a name="variable-packet-capture"></a>Variabele pakketopname

Met Network Watcher-functie voor het [vastleggen](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) van pakketten kunt u pakket opname sessies maken om het verkeer van en naar een virtuele machine bij te houden. Met pakket opname kunt u netwerk afwijkingen zowel reactief als in proactiviteit vaststellen. Andere gebruiken zijn onder andere het verzamelen van netwerk statistieken, het verkrijgen van informatie over inbreuken op het netwerk, het opsporen van fouten in client-server communicatie en nog veel meer.

Pakket opname is een extensie voor virtuele machines die extern wordt gestart via Network Watcher. Deze mogelijkheid vereenvoudigt de belasting van het hand matig uitvoeren van een pakket opname op de gewenste virtuele machine, waardoor kost bare tijd wordt bespaard. Pakket opname kan worden geactiveerd via de portal, Power shell, CLI of REST API. Een voor beeld van hoe pakket opname kan worden geactiveerd, is met waarschuwingen voor virtuele machines.

#### <a name="ip-flow-verify"></a>IP-stroom controleren

[IP-stromen controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) controleert of een pakket wordt toegestaan of geweigerd op basis van 5-tuple-gegevens op een virtuele machine. Deze informatie bestaat uit de richting, het Protocol, het lokale IP, het externe IP-adres, de lokale poort en de externe poort. Als het pakket wordt geweigerd door een beveiligings groep, wordt de naam van de regel die het pakket heeft geweigerd, geretourneerd. Hoewel elke bron-of doel-IP kan worden gekozen, helpt beheerders met deze functie om snel problemen met de connectiviteit op te sporen vanuit of naar het internet en van of naar de on-premises omgeving.

IP-stromen controleren is gericht op een netwerk interface van een virtuele machine. De verkeers stroom wordt vervolgens geverifieerd op basis van de geconfigureerde instellingen van of naar die netwerk interface. Deze mogelijkheid is handig om te controleren of een regel in een netwerk beveiligings groep binnenkomend of uitgaand verkeer naar of van een virtuele machine blokkeert.

#### <a name="next-hop"></a>Volgende hop

Bepaalt de [volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) voor pakketten die worden gerouteerd in de Azure-netwerk infrastructuur, zodat u niet-geconfigureerde, door de gebruiker gedefinieerde routes kunt vaststellen. Verkeer van een virtuele machine wordt verzonden naar een bestemming op basis van de efficiënte routes die zijn gekoppeld aan een NIC. Met de volgende hop wordt het type van de volgende hop en het IP-adres van een pakket van een specifieke virtuele machine en NIC opgehaald. Dit helpt om te bepalen of het pakket wordt omgeleid naar de bestemming of dat het verkeer zwart wordt gegatd.

De volgende hop retourneert ook de route tabel die is gekoppeld aan de volgende hop. Bij het uitvoeren van een query op een volgende hop als de route wordt gedefinieerd als een door de gebruiker gedefinieerde route, wordt die route geretourneerd. Anders wordt ' systeem route ' door de volgende hop geretourneerd.

#### <a name="security-group-view"></a>Weergave van de beveiligingsgroep

Hiermee worden de effectief en toegepaste beveiligings regels opgehaald die worden toegepast op een virtuele machine. Netwerk beveiligings groepen zijn gekoppeld aan een subnetniveau of op een NIC-niveau. Wanneer deze is gekoppeld aan een subnetniveau, is dit van toepassing op alle VM-exemplaren in het subnet. De [weer gave netwerk beveiligings groep](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) retourneert alle geconfigureerde nsg's en regels die zijn gekoppeld aan een NIC-en subnetniveau voor een virtuele machine die inzicht biedt in de configuratie. Daarnaast worden de meest efficiënte beveiligings regels geretourneerd voor elk van de Nic's in een VM. Met de weer gave netwerk beveiligings groep kunt u een VM evalueren voor netwerk problemen, zoals open poorten. U kunt ook controleren of uw netwerk beveiligings groep werkt zoals verwacht op basis van een [vergelijking tussen de geconfigureerde en de effectief beveiligings regels](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>Logboek registratie voor NSG-stroom

 Met stroom logboeken voor netwerk beveiligings groepen kunt u Logboeken vastleggen die betrekking hebben op verkeer dat is toegestaan of geweigerd door de beveiligings regels in de groep. De stroom wordt gedefinieerd door een gegevens van vijf Tuples: bron-IP, doel-IP, bron poort, doel poort en protocol.

[Stroom logboeken van netwerk beveiligings groepen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van Network Watcher waarmee u informatie kunt bekijken over binnenkomend en IP-verkeer via een netwerk beveiligings groep.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Probleem oplossing voor virtuele netwerk gateway en verbinding

Network Watcher biedt veel mogelijkheden voor het koppelen van uw netwerk bronnen in Azure. Een van deze mogelijkheden is het oplossen van problemen met resources. Het [oplossen van resources](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) kan worden aangeroepen door Power shell, CLI of rest API. Als Network Watcher wordt aangeroepen, wordt de status van een Virtual Network gateway of een verbinding gecontroleerd en worden de bevindingen daarvan geretourneerd.

In deze sectie vindt u informatie over de verschillende beheer taken die momenteel beschikbaar zijn voor het oplossen van problemen met resources.

-   [Problemen met een Virtual Network gateway oplossen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Problemen met een verbinding oplossen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Limieten voor netwerk abonnementen

De limieten voor [netwerk abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) bieden u meer informatie over het gebruik van elk van de netwerk bronnen in een abonnement in een regio, vergeleken met het maximum aantal beschik bare resources.

#### <a name="configuring-diagnostics-log"></a>Diagnostische logboek configureren

Network Watcher biedt een [](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) weer gave van Diagnostische logboeken. Deze weer gave bevat alle netwerk bronnen die ondersteuning bieden voor diagnostische logboek registratie. In deze weer gave kunt u netwerk bronnen gemakkelijk en snel en eenvoudig inschakelen en uitschakelen.

### <a name="network-resource-level-monitoring"></a>Bewaking van netwerk resource niveau

De volgende functies zijn beschikbaar voor bewaking op resource niveau:

#### <a name="audit-log"></a>Auditlogboek

Bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken, worden vastgelegd. Deze audit logboeken zijn essentieel om verschillende nalevingen te bepalen. Deze logboeken kunnen worden weer gegeven in de Azure Portal of worden opgehaald met micro soft-hulpprogram ma's, zoals Power BI of hulpprogram ma's van derden. Audit logboeken zijn beschikbaar via de portal, Power shell, CLI en rest-API.

> [!Note]
> Zie [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)voor meer informatie over audit Logboeken.
Audit logboeken zijn beschikbaar voor bewerkingen die op alle netwerk bronnen worden uitgevoerd.


#### <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn prestatie metingen en tellers die gedurende een bepaalde periode zijn verzameld. Metrieken zijn momenteel beschikbaar voor Application Gateway. Metrische gegevens kunnen worden gebruikt om waarschuwingen te activeren op basis van de drempel waarde. Azure-toepassing gateway controleert standaard de status van alle resources in de back-end-groep en verwijdert automatisch alle bronnen die worden beschouwd als een slechte status van de groep. Application Gateway blijft de beschadigde instanties bewaken en voegt deze weer toe aan de gezonde back-end-pool zodra deze beschikbaar komen en reageren op status controles. Application Gateway verzendt de status tests met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen. Deze configuratie zorgt ervoor dat de test wordt uitgevoerd op dezelfde poort die door klanten zou worden gebruikt om verbinding te maken met de back-end.

> [!Note]
> Zie [Application Gateway diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) om te zien hoe metrieken kunnen worden gebruikt om waarschuwingen te maken.

#### <a name="diagnostic-logs"></a>Diagnostische logboeken

Periodieke en spontane gebeurtenissen worden gemaakt door netwerk bronnen en geregistreerde opslag accounts, verzonden naar een event hub of Azure Monitor Logboeken. Deze logboeken bieden inzicht in de status van een resource. Deze logboeken kunnen worden weer gegeven in hulpprogram ma's als Power BI en Azure Monitor-Logboeken. Ga naar [Azure monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)-logboeken voor meer informatie over het weer geven van Diagnostische logboeken.

Diagnostische logboeken zijn beschikbaar voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [netwerk beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), routes en [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Network Watcher biedt een weer gave van Diagnostische logboeken. Deze weer gave bevat alle netwerk bronnen die ondersteuning bieden voor diagnostische logboek registratie. In deze weer gave kunt u netwerk bronnen gemakkelijk en snel en eenvoudig inschakelen en uitschakelen.

### <a name="azure-monitor-logs"></a>Azure Monitor-logboeken

[Azure monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) -Logboeken is een service in azure die uw Cloud-en on-premises omgevingen bewaakt om hun Beschik baarheid en prestaties te behouden. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

Azure Monitor-logboeken bieden de volgende oplossingen voor het bewaken van uw netwerken:

-   Netwerkprestatiemeter (NPM)

-   Azure-toepassing gateway-analyse

-   Analyse van Azure-netwerk beveiligings groep

#### <a name="network-performance-monitor-npm"></a>Netwerk prestatie meter (NPM)
De oplossing voor het beheer van [Netwerkprestatiemeter](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) is een netwerk bewakings oplossing waarmee de status, Beschik baarheid en de bereikbaar van netwerken worden bewaakt.

Het wordt gebruikt voor het bewaken van de connectiviteit tussen:

-   open bare Cloud en on-premises

-   data centers en gebruikers locaties (filialen)

-   subnetten die als host fungeren voor verschillende lagen van een toepassing met meerdere lagen.


#### <a name="azure-application-gateway-analytics-in-azure-monitor-logs"></a>Azure Application Gateway Analytics in Azure Monitor-logboeken

De volgende logboeken worden ondersteund voor toepassings gateways:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

De volgende metrische gegevens worden ondersteund voor toepassings gateways:

-   door Voer van 5 minuten

#### <a name="azure-network-security-group-analytics-in-azure-monitor-logs"></a>Analyse van Azure-netwerk beveiligings groep in Azure Monitor-logboeken

De volgende logboeken worden ondersteund voor [netwerk beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** Bevat vermeldingen waarvoor NSG-regels worden toegepast op Vm's en instantie rollen op basis van een MAC-adres. De status voor deze regels wordt elke 60 seconden verzameld.

- **NetworkSecurityGroupRuleCounter:** Bevat vermeldingen voor het aantal keren dat elke NSG regel wordt toegepast om verkeer te weigeren of toe te staan.

## <a name="next-steps"></a>Volgende stappen
Lees meer over beveiliging door enkele van onze diep gaande onderwerpen over beveiliging te lezen:

-   [Azure Monitor logboeken voor netwerk beveiligings groepen (Nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Netwerk innovaties waarmee de Cloud wordt verstoord](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC De software voor netwerk switch die de wereld wijde cloud van micro soft activeert](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Hoe micro soft het snelle en betrouw bare wereld wijde netwerk bouwt](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Netwerk innovatie verlichten](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
