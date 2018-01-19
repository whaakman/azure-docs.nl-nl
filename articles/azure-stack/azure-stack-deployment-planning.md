---
title: "Planningsoverwegingen voor het Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Meer informatie over wat u kunt doen om nu plannen en voorbereiden voor Azure-Stack met meerdere knooppunten.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: cd14f0e5259e5c0b6cbf11790bbdf08164267ffa
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Planningsoverwegingen voor het Azure-Stack geïntegreerd systemen
Als u geïnteresseerd in een Azure-Stack geïntegreerd systeem bent, moet u enkele van de belangrijke planningsoverwegingen rond de implementatie en hoe het systeem in uw datacenter past begrijpen. Dit artikel bevat een overzicht van deze overwegingen bij de infrastructuur van belangrijke beslissingen voor uw Azure-Stack-systeem met meerdere knooppunten. Een goed begrip van deze overwegingen helpt bij het werken met uw hardwareleverancier OEM als ze Azure Stack op uw datacenter implementeren.  

> [!NOTE]
> Azure Stack systemen met meerdere knooppunten kunnen alleen worden gekocht bij geautoriseerde hardwareleveranciers. 

Voor het implementeren van Azure-Stack er een aantal beslissingen die u moet zijn aanbrengen in goed Azure Stack integreren in uw omgeving. U moet deze informatie geven aan uw solution provider tijdens het planningsproces en gereed zijn voor de hardwareleverancier voordat implementatie begint te helpen bij het proces gaat soepel en snel.

Deze informatie nodig bereiken over netwerken-, beveiligings- en identiteitsgegevens met veel belangrijke beslissingen waarvoor kennis van veel verschillende gebieden en besluitvormers. U moet daarom ophalen van de mensen van meerdere teams in uw organisatie om ervoor te zorgen dat u alle vereiste informatie bij de hand hebt voordat de implementatie begint. Kunt u contact opnemen met uw hardwareleverancier bij het verzamelen van deze informatie zoals ze advies die nuttig zijn hebben wellicht voor uw beslissingen.

Tijdens het onderzoeken en de vereiste gegevens verzamelen, moet u mogelijk enkele vóór implementatie configuratiewijzigingen aanbrengen in uw netwerkomgeving. Het kan hierbij reserveren van IP-adresruimten voor de Azure-Stack-oplossing en uw routers, switches en firewalls om voor te bereiden voor de verbinding met de nieuwe Azure-Stack oplossing switches configureren. Zorg ervoor dat u hebt het onderwerp gebied expert uitgelijnd en betrokken tijdens de implementatieproject voor hulp bij de planning.

## <a name="management-considerations"></a>Beheeroverwegingen
Azure-Stack is een verzegeld systeem waar de infrastructuur is vergrendeld zowel vanuit een machtigingen en netwerk-perspectief. Netwerk toegangsbeheerlijsten (ACL's) worden toegepast voor het blokkeren van inkomend verkeer van alle niet-geautoriseerde en alle onnodige communicatie tussen onderdelen van de infrastructuur. Dit maakt het moeilijk voor onbevoegde gebruikers toegang tot het systeem.

Voor het dagelijkse beheer en bewerkingen is er geen onbeperkte beheerderstoegang tot de infrastructuur. Azure Stack-operators moeten het systeem via de beheerdersportal of via Azure Resource Manager (via Azure CLI, PowerShell of de REST-API) te beheren. Er is geen toegang tot het systeem door andere beheerprogramma's zoals Hyper-V-beheer of Failover Cluster Manager. Ter bescherming van het systeem kan niet binnen de onderdelen van de infrastructuur van Azure-Stack software van derden (bijvoorbeeld agents) worden geïnstalleerd. Interoperabiliteit met extern beheer- en beveiligingssoftware vindt plaats via Azure CLI, PowerShell of de REST-API.

Wanneer een hoger niveau van toegang nodig is voor het oplossen van problemen die door waarschuwing uitoefenen stappen niet zijn opgelost, moet u werken met ondersteuning. Dankzij de ondersteuning is er een methode voor tijdelijke volledige beheerderstoegang tot het systeem meer geavanceerde bewerkingen uit te voeren. 

## <a name="identity-considerations"></a>Identiteit overwegingen

### <a name="choose-identity-provider"></a>Kies de identiteitsprovider
U moet rekening houden met welke id-provider die u wilt gebruiken voor Azure-Stack-implementatie, Azure AD of AD FS. U kunt geen id-providers overschakelen na implementatie zonder volledige systeem opnieuw installeren.

Uw keuze van de provider identiteit heeft geen invloed op virtuele machines van tenants, identiteitssysteem en de accounts die ze gebruiken, of ze kunnen deelnemen aan een Active Directory-domein, enzovoort. Dit is een afzonderlijke.

U kunt meer informatie over het kiezen van een id-provider in de [beslissingen voor de implementatie voor Azure-Stack geïntegreerde systemen artikel](.\azure-stack-deployment-decisions.md).

### <a name="ad-fs-and-graph-integration"></a>Integratie van AD FS en grafiek
Als u kiest voor het implementeren van Azure-Stack AD FS gebruikt als de id-provider, moet u het exemplaar van AD FS op Azure-Stack integreren met een bestaand exemplaar van AD FS via een federatieve vertrouwensrelatie. Hierdoor identiteiten in een bestaand Active Directory-forest om te verifiëren met resources in Azure-Stack.

U kunt ook de grafiek service in Azure-Stack integreren met bestaande Active Directory. Hiermee kunt u voor het beheren van op rollen gebaseerde toegangsbeheer (RBAC) in Azure-Stack. Wanneer u toegang tot een resource wordt overgedragen, zoekt het onderdeel van de grafiek het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

Het volgende diagram toont de geïntegreerde grafiek en AD FS-netwerkverkeer.
![Diagram van de AD FS en grafiek netwerkverkeer](media/azure-stack-deployment-planning/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Licentiemodel

U moet beslissen welke licentiemodel die u wilt gebruiken. Voor een implementatie verbonden kunt u betalen-als-gebruik of op basis van capaciteit-licentieverlening. Betalen-als-gebruik vereist een verbinding naar Azure te rapporteren over het gebruik, die vervolgens wordt gefactureerd via Azure commerce. Alleen capaciteit op basis van licentieverlening wordt ondersteund als u implementeert de internetverbinding verbroken. Zie voor meer informatie over de licentiemodellen [Microsoft Azure-Stack verpakken en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Beslissingen voor de naamgeving

U moet u bedenken hoe wilt u van plan bent uw Azure-Stack-naamruimte, met name de regio, en externe domeinnaam. De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie voor openbare eindpunten is de combinatie van deze twee namen &lt; *regio*&gt;&lt;*external_FQDN*  &gt;, bijvoorbeeld *east.cloud.fabrikam.com*. In dit voorbeeld zou de Azure-Stack-portals zijn beschikbaar op de volgende URL's:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

De volgende tabel geeft een overzicht van deze domain naming beslissingen.

| Naam | Beschrijving | 
| -------- | ------------- | 
|Regionaam | De naam van uw eerste Azure-Stack-regio. Deze naam wordt gebruikt als onderdeel van de FQDN-naam voor de openbare virtuele IP-adressen (VIP's) die Azure-Stack beheert. De regionaam wordt normaal gesproken een fysieke locatie-id, zoals de locatie van een datacenter. | 
| Externe domeinnaam | De naam van de zone System DNS (Domain Name) voor eindpunten met extern gerichte VIP's. In de FQDN-naam gebruikt voor deze openbare VIP's. | 
| Domeinnaam van persoonlijke (intern) | De naam van het domein (en interne DNS-zone) gemaakt op Azure-Stack voor beheer van de infrastructuur. 
| | |

## <a name="certificate-requirements"></a>Certificaatvereisten

Voor implementatie moet u Secure Sockets Layer (SSL) om certificaten te verlenen voor openbare eindpunten. Op een hoog niveau hebben certificaten de volgende vereisten:

- U kunt een enkel jokertekencertificaat of gebruiken van een set specifieke certificaten en het gebruik van jokertekens alleen voor eindpunten zoals opslag en Sleutelkluis.
- Certificaten kunnen worden uitgegeven door een openbaar vertrouwde certificeringsinstantie (CA) of een CA door de klant beheerd.

Voor meer informatie over welke PKI certificaten zijn vereist voor het implementeren van Azure-netwerkstack en hoe deze verkrijgen, zien, [Azure Stack Public Key Infrastructure certificaatvereisten](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> De gegevens van het opgegeven PKI-certificaat moet worden gebruikt als een algemene richtlijn. Voordat u een PKI-certificaten voor Azure-Stack aanschaft, kunt u samenwerken met uw OEM-hardware-partner. Geven ze meer gedetailleerde certificaat richtlijnen en vereisten.



## <a name="time-synchronization"></a>Tijdsynchronisatie
U kunt een specifiek tijdstip server met wordt gebruikt voor het synchroniseren van Azure-Stack moet kiezen.  Tijdsynchronisatie is essentieel voor de Azure-Stack en de bijbehorende rollen infrastructuur, zoals het wordt gebruikt voor het genereren van Kerberos-tickets die worden gebruikt voor verificatie van interne services met elkaar.

U moet een IP-adres voor de server van de synchronisatie tijd als Hoewel de meeste van de onderdelen in de infrastructuur kan een URL oplossen kunt, sommige biedt slechts ondersteuning voor IP-adressen opgeven. Als u klaar bent met de optie voor de verbinding is verbroken, moet u een tijd-server in uw bedrijfsnetwerk dat u ervoor dat kan worden bereikt vanaf het netwerk van de infrastructuur in Azure-Stack. Hiervoor moet mogelijk extra aandacht tijdens de planning van het netwerk integratie-gedeelte van het implementatieproject uit.


## <a name="network-connectivity"></a>Netwerkverbinding
Deze sectie bevat de Azure-netwerkstack informatie over de infrastructuur waarmee u belangrijke beslissingen over het beste Azure Stack integreren in uw bestaande netwerkomgeving maken. 

> [!NOTE]
> Om op te lossen externe DNS-namen uit Azure-Stack (bijvoorbeeld www.bing.com), moet u DNS-servers die Azure-Stack gebruiken kunt voor het doorsturen van DNS-aanvragen waarvoor Azure-Stack niet gemachtigd is bieden. Zie voor meer informatie over Azure Stack DNS vereisten, [Stack Azure datacenter integratie - DNS-](azure-stack-integrate-dns.md).

### <a name="physical-network-design"></a>Fysieke netwerkontwerp
De Azure-Stack-oplossing vereist een robuuste en maximaal beschikbare fysieke infrastructuur ter ondersteuning van de toepassing en services. Hieronder vindt u een diagram van onze aanbevolen ontwerp:

![Aanbevolen ontwerp voor Azure-Stack-netwerk](media/azure-stack-deployment-planning/recommended-design.png)


### <a name="logical-networks"></a>Logische netwerken
Logische netwerken vertegenwoordigen een abstractie van de onderliggende fysieke netwerkinfrastructuur. Ze worden gebruikt voor het ordenen en vereenvoudigen Netwerktoewijzingen voor hosts, virtuele machines en services. Als onderdeel van het maken van logische netwerken, netwerksites gemaakt voor het definiëren van de virtuele LAN (VLAN's), IP-subnetten en IP-subnet/VLAN-paren die gekoppeld aan het logische netwerk op elke fysieke locatie zijn.
De netwerkinfrastructuur voor Azure-Stack maakt gebruik van de volgende logische netwerken die zijn geconfigureerd op de switches:

### <a name="network-infrastructure"></a>Netwerkinfrastructuur
De netwerkinfrastructuur voor Azure-Stack bestaat uit verschillende logische netwerken die zijn geconfigureerd op de switches. Het volgende diagram toont deze logische netwerken, en hoe ze integreren met de top-of-rack (TOR), BMC (baseboard management controller) en (klant) netwerkswitches rand.

![Logisch netwerk en de switch-verbindingen](media/azure-stack-deployment-planning/NetworkDiagram.png)

#### <a name="bmc-network"></a>BMC-netwerk
Dit netwerk is op het verbinden van alle de baseboard management controllers (ook wel bekend als serviceprocessors, bijvoorbeeld iDRAC, iLO, iBMC, enz.) toegewezen aan het beheernetwerk. Indien aanwezig, wordt de hardware-lifecycle-host (HLH) bevindt zich op dit netwerk en OEM-specifieke software voor onderhoud van hardware-en/of bewaking kan bieden. 

#### <a name="private-network"></a>Privénetwerk
Deze /24 (254 host IP-adressen) netwerk is gebonden aan de Stack van Azure-regio (wordt niet uitgebreid dan de apparaten van de switch rand van de Stack van Azure-regio) en is onderverdeeld in twee subnetten:

- **Opslagnetwerk**. Een /25 (126 host IP-adressen) netwerk dat wordt gebruikt ter ondersteuning van het gebruik van opslagruimten Direct (S2D) en Server Message Block (SMB)-opslagverkeer en livemigratie van virtuele machine. 
- **Interne virtuele IP-netwerk**. A/25 netwerk gereserveerd voor intern gebruik bestemde VIP's voor de software load balancer (SLB).

#### <a name="azure-stack-infrastructure-network"></a>Netwerk van Azure Stack-infrastructuur
Dit/24 netwerk is toegewezen aan interne Azure Stack-onderdelen zodat ze kunnen communiceren en uitwisselen van gegevens onderling. Dit subnet routeerbare IP-adressen vereist, maar worden privé gehouden met de oplossing met behulp van toegangsbeheerlijsten (ACL's).  Er wordt niet verwacht te routeren afgezien van de rand switches, met uitzondering van een zeer kleine bereik overeen in grootte met een/27 netwerk, waardoor wordt gebruikt door sommige van deze services wanneer ze toegang tot externe bronnen en/of internet vereist. 

#### <a name="public-infrastructure-network"></a>Infrastructuur voor openbare netwerk
Dit/27 netwerk is het zeer kleine bereik van het Azure-Stack subnet eerder vermeld, vereist geen openbare IP-adressen, maar hoeft toegang tot internet via een NAT of een transparentproxy. Dit netwerk wordt toegewezen voor de Emergency Recovery-Console (ERCS). De VM ERCS hiervoor is internettoegang vereist tijdens de registratie in Azure en moet routeerbaar zijn met uw beheernetwerk voor het oplossen van problemen.

#### <a name="public-vip-network"></a>Openbare VIP-netwerk
Het openbare VIP-netwerk is toegewezen aan de netwerkcontroller in Azure-Stack. Het is niet een logisch netwerk op de switch. De SLB maakt gebruik van de groep met adressen en wijst/32 voor bedrijfstoepassingen huurder netwerken. Deze 32 IP-adressen zijn op de switch-routeringstabel geadverteerd als een beschikbare route via BGP. Dit netwerk bevat de externe toegankelijk of openbare IP-adressen. De infrastructuur van Azure-Stack maakt gebruik van ten minste 8 adressen van deze openbare VIP-netwerk terwijl de rest wordt gebruikt door de tenant-VM's. De netwerkgrootte op dit subnet kan variëren van een minimum van /26 (64-hosts) tot maximaal /22 (1022 hosts), is het raadzaam dat u van plan bent voor een/24 netwerk.

#### <a name="switch-infrastructure-network"></a>Switch-infrastructuurnetwerk
Dit/26 netwerk is het subnet met subnetten van het point-to-point routeerbare IP-/ 30 (2 host IP van) en de loopbacks die zijn toegewezen/32 subnetten voor in-band overschakelen beheer- en BGP-router-ID. Dit bereik van IP-adressen moet routeerbaar extern van de Azure-Stack-oplossing voor uw datacenter en ze kunnen persoonlijke of openbare IP-adressen zijn. Bijvoorbeeld, binnen een multi-verpachte serviceprovider scenario die openbare IP-adressen mogelijk vereist, terwijl in een strak beheerd implementatie voor ondernemingen, persoonlijke IP-adressen mogelijk voorkeur.

#### <a name="switch-management-network"></a>Switch-management-netwerk
Deze slechts/29 (6 host IP-adressen) netwerk is toegewezen aan de poorten van de schakelopties verbinding te maken. Het biedt out-of-band-toegang voor implementatie, beheer en probleemoplossing. Het is berekend op basis van de hierboven genoemde infrastructuurnetwerk in de switch.

### <a name="transparent-proxy"></a>TRANSPARENTPROXY
De Azure-Stack-oplossing biedt geen ondersteuning voor normale webproxy. Als het datacentrum al het verkeer vereist naar een proxy gebruikt, moet u een transparentproxy voor het verwerken van al het verkeer van het rek volgens het beleid, verwerkt; het scheiden van verkeer tussen de zones in uw netwerk. Een transparentproxy (ook wel bekend als een onderschept, inline of geforceerde proxy) onderschept normale communicatie op de netwerklaag zonder speciale clientconfiguratie. Clients moeten niet worden op de hoogte van de aanwezigheid van de proxy.

### <a name="publish-azure-stack-services"></a>Azure-Stack services publiceren

U moet Azure Stack services beschikbaar stellen aan gebruikers van buiten Azure-Stack. Azure Stack stelt u verschillende eindpunten voor de functies van de infrastructuur. Deze eindpunten toegewezen VIP's uit de openbare IP-adresgroep. Een DNS-vermelding wordt voor elk eindpunt voor de externe DNS-zone, is opgegeven tijdens de implementatie gemaakt. De gebruikersportal is bijvoorbeeld toegewezen dat de host DNS-vermelding van de portal.  *&lt;regio >.&lt; FQDN-naam >*.

#### <a name="ports-and-urls"></a>Poorten en URL 's

Om ervoor te Stack Azure-services (zoals de portals Azure Resource Manager, DNS, etc.) beschikbaar met externe netwerken, moet u binnenkomend verkeer op deze eindpunten toestaan voor specifieke URL's, poorten en protocollen.
 
In een implementatie waarbij een transparentproxy uplinkpoortprofiel met een traditioneel proxyserver, moet u toestaan bepaalde poorten en URL's voor uitgaande communicatie. Deze omvatten poorten en URL's voor identiteit, marketplace syndication, patch en update, registratie en gebruiksgegevens.

Zie voor meer informatie:
- [Poorten voor inkomend verkeer en protocollen](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Uitgaande poorten en URL 's](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Verbinding maken met Azure Stack naar Azure

Voor hybride cloud-scenario's moet u plannen hoe u wilt verbinding maken met Azure-Stack naar Azure. Er zijn twee manieren verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure: 
- **Site-to-site**. Een virtueel particulier netwerk (VPN)-verbinding via IPsec (IKE v1 en IKE v2). Dit type verbinding vereist een VPN-apparaat of Routing and Remote Access Service (RRAS). Zie voor meer informatie over VPN-gateways in Azure [over VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). De communicatie via deze tunnel wordt gecodeerd en beveiligd. Bandbreedte wordt echter wel beperkt door de maximale doorvoer van de tunnel (100-200 Mbps).
- **Uitgaande NAT**. Standaard hebben alle virtuele machines in Azure Stack verbinding met externe netwerken via uitgaande NAT bevinden. Elk virtueel netwerk dat is gemaakt in Azure Stack Hiermee haalt u een openbaar IP-adres toegewezen. Of de virtuele machine direct een openbaar IP-adres wordt toegewezen of zich achter een load balancer met een openbaar IP-adres, heeft dit uitgaande toegang via de uitgaande NAT het VIP van het virtuele netwerk gebruiken. Dit werkt alleen voor communicatie die is gestart door de virtuele machine en bestemd is voor externe netwerken (internet of intranet). Deze kan niet worden gebruikt om te communiceren met de virtuele machine vanuit buiten.

#### <a name="hybrid-connectivity-options"></a>Opties voor hybride verbindingen

Voor hybride verbindingen is het belangrijk te overwegen wat voor soort implementatie die u wilt aanbieden en waar deze wordt geïmplementeerd. U moet overwegen of u moet voor het isoleren van netwerkverkeer per tenant en of hebt u de implementatie van een intranet of internet.

- **Azure Stack voor één tenant**. Een Azure-Stack-implementatie die eruit, ten minste van een netwerk perspectief ziet alsof het één tenant. Er kan dat veel abonnementen tenant, maar zoals een intranet-service al het verkeer via dezelfde netwerken gezonden. Netwerkverkeer van één abonnement overdracht via dezelfde netwerkverbinding als een ander abonnement en hoeven niet te worden geïsoleerd via een versleutelde-tunnel.

- **Multitenant Azure Stack**. De implementatie van een Azure-Stack waar elke tenantabonnement verkeer dat gekoppeld aan de netwerken die extern voor Azure-Stack zijn geïsoleerd van andere tenants netwerkverkeer worden moet.
 
- **Implementatie op een intranet**. Een Azure-Stack-implementatie bevindt zich op een bedrijfsintranet doorgaans op particuliere IP-adresruimte en achter een firewall in een of meer. Het openbare IP-adressen zijn niet volledig openbaar, als ze rechtstreeks via het openbare internet kunnen niet worden gerouteerd.

- **Implementatie van Internet**. Een Azure-Stack-implementatie die verbonden met het openbare internet en maakt gebruik van internet routeerbare openbare IP-adressen voor het openbare VIP-adresbereik. De implementatie kan nog steeds bevinden zich achter een firewall, maar het openbare VIP-adresbereik is rechtstreeks bereikbaar is vanaf het openbare internet en Azure.
 
De volgende tabel geeft een overzicht van de hybride verbinding scenario's met professionals, nadelen en gebruiksvoorbeelden.

| Scenario | Connectiviteit methode | Professionals | Nadelen | Goede voor |
| -- | -- | --| -- | --|
| Eén tenant Azure Stack, implementatie op een intranet | Uitgaande NAT | Grotere bandbreedte voor snellere overdrachten. Eenvoudig te implementeren; Er zijn geen gateways dat nodig is. | Verkeer dat niet is versleuteld. Er is geen isolatie- of -versleuteling buiten de TOR. | In bedrijfsimplementaties waar alle huurders evenveel vertrouwd zijn.<br><br>Ondernemingen die u een Azure ExpressRoute-circuit naar Azure hebt. |
| Multitenant Azure Stack, implementatie op een intranet | Site-to-site VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | In bedrijfsimplementaties waar sommige tenantverkeer moeten van andere tenants worden beveiligd. |
| Eén tenant Stack in Azure, internet-implementatie | Uitgaande NAT | Grotere bandbreedte voor snellere overdrachten. | Verkeer dat niet is versleuteld. Er is geen isolatie- of -versleuteling buiten de TOR. | Hostingscenario's waarin de tenant opgehaald hun eigen Azure Stack-implementatie en een speciaal circuit aan de Stack van Azure-omgeving. ExpressRoute en Multiprotocol Label overschakelen (MPLS).
| Multitenant-Stack voor Azure, internet-implementatie | Site-to-site VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | Hostingscenario's waarin de provider wil bieden een multitenant-cloud, waar de tenants niet vertrouwt elkaar en verkeer moeten worden versleuteld.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Met behulp van ExpressRoute

U kunt Azure-Stack verbinden met Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) voor één tenant intranet en scenario's voor meerdere tenants. U moet een ExpressRoute-circuit via ingerichte [een connectiviteitsprovider](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Het volgende diagram toont ExpressRoute voor een scenario voor één tenant (waarbij 'Verbinding van de klant' is van het ExpressRoute-circuit).

![Diagram waarin één tenant ExpressRoute scenario](media/azure-stack-deployment-planning/ExpressRouteSingleTenant.PNG)

Het volgende diagram toont ExpressRoute voor een scenario met meerdere tenants.

![Diagram waarin meerdere tenants ExpressRoute scenario](media/azure-stack-deployment-planning/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externe controle
Ophalen van één enkele weergave van alle waarschuwingen van uw Azure-Stack-implementatie en apparaten en waarschuwingen integreren met bestaande werkstromen voor IT-service voor tickets, kunt u Azure-Stack integreren met externe datacenter bewakingsoplossingen.

Opgenomen met de Azure-Stack-oplossing, is de hardware-lifecycle-host (HLH) een computer buiten een Azure-Stack die wordt uitgevoerd door OEM geleverde leverancier beheerhulpprogramma's voor hardware. U kunt deze hulpprogramma's of andere oplossingen die rechtstreeks worden geïntegreerd met bestaande bewakingsoplossingen in uw datacenter.

De volgende tabel geeft een overzicht van de lijst met beschikbare opties.

| Onderwerp | Oplossing voor externe controle |
| -- | -- |
| Azure Stack-software. | - [Azure Stack Management Pack voor Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Nagios plug-in](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST gebaseerde API-aanroepen | 
| Fysieke servers (bmc's via IPMI) | -Operations Manager-leverancier van management pack<br>-OEM leverancier geleverde hardwareoplossing<br>-Hardwareleverancier Nagios-invoegtoepassingen | OEM-partner ondersteund bewakingsoplossing (opgenomen) | 
| Netwerkapparaten (SNMP) | -Detectie van netwerkapparaten operations Manager<br>-OEM leverancier geleverde hardwareoplossing<br>-Nagios switch-invoegtoepassing |
| Statuscontrole voor tenant-abonnement | - [System Center Management Pack voor Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Houd rekening met de volgende vereisten:
- De oplossing die u gebruikt moet worden zonder agent. U kunt van derden agents binnen Azure Stack-componenten niet installeren. 
- Als u gebruiken van System Center Operations Manager wilt, moet hiervoor Operations Manager 2012 R2 of Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen

Planning voor back-up en herstel na noodgevallen omvat het plannen voor zowel de onderliggende Azure Stack infrastructuur die als host fungeert voor IaaS virtuele machines en PaaS-services en voor de tenant-toepassingen en gegevens. U moet plannen voor deze afzonderlijk.

### <a name="protect-infrastructure-components"></a>Onderdelen van de infrastructuur beveiligen

Azure Stack back-ups van onderdelen van de infrastructuur tot een share die u opgeeft.

- U moet een externe SMB-bestandsshare op een bestaande Windows-bestandsserver of een apparaat van derden.
- U moet deze dezelfde share voor de back-up van netwerkswitches en de host van de levenscyclus van hardware. Uw hardwareleverancier OEM helpt richtlijnen te bieden voor back-up en herstel van deze onderdelen wanneer deze externe naar Azure-Stack. U bent verantwoordelijk voor het uitvoeren van de back-werkstromen op basis van de OEM-leverancier aanbeveling.

Als onherstelbaar gegevensverlies optreedt, kunt u de infrastructuur back-up seedbewerking implementatiegegevens, zoals implementatie ingangen en id's, service-accounts CA-basiscertificaat, federatieve resources (in niet-verbonden implementaties), plannen, aanbiedingen, abonnementen, quota, toewijzingen voor het beleid en de rol van RBAC en geheimen van de Sleutelkluis.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Tenant-toepassingen op IaaS virtuele machines beveiligen

Azure Stack maakt geen back-up van de tenant-toepassingen en gegevens. U moet plannen voor back-up en noodherstel recovery-beveiliging met een externe naar Azure Stack doel. Beveiliging van de tenant is een activiteit tenant op basis van beschikbare resources. Voor IaaS virtuele machines, kunnen tenants in de Gast technologieën gebruiken om mappen, toepassingsgegevens en systeemstatus te beveiligen. Als een enterprise- of service-provider kunt u echter bieden een oplossing back-up en herstel in hetzelfde datacenter of extern in een cloud.

Als u wilt back-up van Linux of Windows IaaS virtuele machines, moet u back-producten met toegang tot het gastbesturingssysteem bestand, map, de status van het besturingssysteem en toepassingsgegevens te beveiligen. U kunt Azure back-up, System Center Data Center Protection Manager of producten van derden ondersteund.

Als u wilt repliceren van gegevens naar een secundaire locatie en stroomlijnen de failover van de toepassing als er een ramp optreedt, kunt u Azure Site Recovery of ondersteunde producten van derden gebruiken. (Op de eerste release van geïntegreerde systemen Azure Site Recovery niet wordt ondersteund failback. Echter, u kunt bereiken failback via een handmatig proces.) Bovendien kunnen toepassingen die ondersteuning bieden voor systeemeigen replicatie (zoals Microsoft SQL Server) gegevens repliceren naar een andere locatie waar de toepassing wordt uitgevoerd.

> [!IMPORTANT]
> Op de eerste release van geïntegreerde systemen door ons ondersteund beveiligingstechnologieën die op de het gastniveau van de virtuele machine voor IaaS werkt. U kunt geen agents installeren op de onderliggende infrastructuurservers.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over gebruiksvoorbeelden, kopen, partners en hardwareleveranciers OEM de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Voor informatie over de roadmap en geo-beschikbaarheid voor Azure-Stack geïntegreerde systemen, Zie het white paper: [Azure Stack: een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
