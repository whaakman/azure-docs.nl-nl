---
title: "Planningsoverwegingen voor het Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Meer informatie over wat u kunt doen om nu plannen en voorbereiden voor Azure-Stack met meerdere knooppunten.
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 90f8fa1a-cace-4bfa-852b-5abe2b307615
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: twooley
ms.openlocfilehash: 8484f7947f23a00c05b34babf13cd75f9d227740
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Planningsoverwegingen voor het Azure-Stack geïntegreerd systemen

*Van toepassing op: Azure Stack geïntegreerd systemen*

Als u geïnteresseerd in een Azure-Stack geïntegreerd systeem bent, moet u enkele van de belangrijke planningsoverwegingen rond de implementatie en hoe het systeem past in uw datacenter te begrijpen. Dit onderwerp bevat een overzicht van deze overwegingen.

Als u besluit om aan te schaffen een geïntegreerd systeem, helpt uw oorspronkelijke leveranciers (OEM) hardwareleverancier begeleiden u bij veel van de planning in meer detail. Ze worden ook de werkelijke implementatie uitvoeren.

## <a name="management-considerations"></a>Beheeroverwegingen

Azure-Stack is een verzegeld systeem waar de infrastructuur is vergrendeld zowel vanuit een machtigingen en netwerk-perspectief. Netwerk toegangsbeheerlijsten (ACL's) worden toegepast voor het blokkeren van inkomend verkeer van alle niet-geautoriseerde en alle onnodige communicatie tussen onderdelen van de infrastructuur. Dit maakt het moeilijk voor onbevoegde gebruikers toegang tot het systeem.

Voor het dagelijkse beheer en bewerkingen is er geen onbeperkte beheerderstoegang tot de infrastructuur. Azure Stack-operators moeten het systeem via de beheerdersportal of via Azure Resource Manager (via PowerShell of de REST-API) te beheren. Er is geen toegang tot het systeem door andere beheerprogramma's zoals Hyper-V-beheer of Failover Cluster Manager. Ter bescherming van het systeem kan niet binnen de onderdelen van de infrastructuur van Azure-Stack software van derden (bijvoorbeeld agents) worden geïnstalleerd. Interoperabiliteit met externe software voor beheer en beveiliging vindt plaats via PowerShell of de REST-API.

Wanneer een hoger niveau van toegang nodig is voor het oplossen van problemen die door waarschuwing uitoefenen stappen niet zijn opgelost, moet u werken met ondersteuning. Dankzij de ondersteuning is er een methode voor tijdelijke volledige beheerderstoegang tot het systeem meer geavanceerde bewerkingen uit te voeren. 

## <a name="deploy-connected-or-disconnected"></a>Verbonden of niet-verbonden implementeren
 
U kunt voor het implementeren van Azure-Stack is verbonden met internet (en naar Azure) of de verbinding verbroken. Als u optimaal wilt profiteren van Azure-Stack, waaronder hybride scenario's tussen Azure-Stack en Azure, zou u wenst te implementeren met Azure bent verbonden. De volgende tabel kunt geven een overzicht van de belangrijkste verschillen tussen de implementatiemodi.

| Onderwerp | Verbonden modus | Modus zonder verbinding |
| -------- | ------------- | ----------|
| Identiteitsprovider | Azure Active Directory (Azure AD) of Active Directory Federatieservices (AD FS) | Alleen AD FS |
| Marketplace-syndicatie | Items rechtstreeks downloaden vanuit Azure Marketplace voor de marketplace in Azure-Stack | Handmatige beheer van de marketplace in Azure-Stack is vereist |
| Licentiemodel | Betalen-als-gebruik of op basis van capaciteit | Op basis van capaciteit alleen|
| Patch en bij te werken  | Updatepakketten downloaden rechtstreeks naar de Azure-Stack | Verwisselbare media en een afzonderlijke aangesloten apparaat is vereist |
| | | |

U kunt de implementatiemodus zonder volledige systeem opnieuw implementeren gaande later niet wijzigen.

## <a name="identity-considerations"></a>Identiteit overwegingen

### <a name="choose-identity-provider"></a>Kies de identiteitsprovider

U moet rekening houden met welke id-provider die u wilt gebruiken voor Azure-Stack-implementatie, Azure AD of AD FS. U kunt geen id-providers overschakelen na implementatie zonder volledige systeem opnieuw installeren.

Uw keuze van de provider identiteit heeft geen invloed op de virtuele machines van tenants, de van identiteitssysteem en accounts die ze gebruiken, of ze kunnen deelnemen aan een Active Directory-domein, enzovoort. Dit is een afzonderlijke.

**Redenen om met behulp van Azure AD**

- U hebt al bestaande investeringen in Azure AD (zoals Azure of Office 365).
- U wilt gebruiken dezelfde identiteit tussen Azure en Azure Stack clouds.
- Wilt u ondersteuning van multi-tenancymodus scenario's waar u verschillende organisaties op hetzelfde exemplaar van Azure-Stack kan hosten.
- U wilt gebruiken op basis van REST Directorybeheer via Azure AD Graph inrichten gebruikers, groepen, enzovoort via API's.

> [!NOTE]
> U kunt de implementatie van een Azure-Stack kan geen verbinding met zowel een Azure AD-exemplaar als een bestaand exemplaar van AD FS op hetzelfde moment. Als u met Azure AD implementeren en u wilt een bestaand exemplaar van AD FS gebruiken, kunt u uw on-premises federeren exemplaar van AD FS met Azure AD.

**Redenen om met AD FS**

- Er is geen of slechts gedeeltelijk verbinding met internet.
- Er zijn regelgeving/onafhankelijkheid vereisten.
- Wilt u uw eigen identiteitssysteem (zoals uw zakelijk Active Directory) gebruiken voor de operator en gebruikersaccounts. Om dit te doen, kunt u communiceren met een bestaand exemplaar van AD FS (op Windows Server 2012, 2012 R2 of 2016) dat wordt ondersteund door Active Directory.
- U hebt geen Azure-investeringen en u niet wilt gebruiken van Azure AD.

> [!NOTE]
> U kunt Azure-Stack federeren alleen met een ander exemplaar van AD FS dat wordt ondersteund door Active Directory. Andere id-providers worden niet ondersteund. Als u een andere id-providers die u wilt gebruiken met Azure-Stack hebt, kunt u overwegen Azure AD gebaseerde implementatie in plaats daarvan.

### <a name="ad-fs-and-graph-integration"></a>Integratie van AD FS en grafiek

Als u kiest voor het implementeren van Azure-Stack AD FS gebruikt als de id-provider, moet u het exemplaar van AD FS op Azure-Stack integreren met een bestaand exemplaar van AD FS via een federatieve vertrouwensrelatie. Hierdoor identiteiten in een bestaand Active Directory-forest om te verifiëren met resources in Azure-Stack.

U kunt ook de grafiek service in Azure-Stack integreren met bestaande Active Directory. Hiermee kunt u voor het beheren van op rollen gebaseerde toegangsbeheer (RBAC) in Azure-Stack. Wanneer u toegang tot een resource wordt overgedragen, zoekt het onderdeel van de grafiek het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

Het volgende diagram toont de geïntegreerde grafiek en AD FS-netwerkverkeer.
![Diagram van de AD FS en grafiek netwerkverkeer](media/azure-stack-planning-considerations/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Licentiemodel

U moet beslissen welke licentiemodel die u wilt gebruiken. Voor een implementatie verbonden kunt u betalen-als-gebruik of op basis van capaciteit-licentieverlening. Betalen-als-gebruik vereist een verbinding naar Azure te rapporteren over het gebruik, die vervolgens wordt gefactureerd via Azure commerce. Alleen capaciteit op basis van licentieverlening wordt ondersteund als u implementeert de internetverbinding verbroken. Zie voor meer informatie over de licentiemodellen [Microsoft Azure-Stack verpakken en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Beslissingen voor de naamgeving

U moet u bedenken hoe wilt u van plan bent uw Azure-Stack-naamruimte, met name de regio, en externe domeinnaam. De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie voor openbare eindpunten is de combinatie van deze twee namen &lt; *regio*&gt;&lt;*external_FQDN*  &gt;, bijvoorbeeld *east.cloud.fabrikam.com*. In dit voorbeeld zou de Azure-Stack-portals zijn beschikbaar op de volgende URL's:

- https://Portal.East.cloud.Fabrikam.com
- https://adminportal.East.cloud.Fabrikam.com

De volgende tabel geeft een overzicht van deze domain naming beslissingen.

| Naam | Beschrijving | 
| -------- | ------------- | 
|Regionaam | De naam van uw eerste Azure-Stack-regio. Deze naam wordt gebruikt als onderdeel van de FQDN-naam voor de openbare virtuele IP-adressen (VIP's) die Azure-Stack beheert. De regionaam wordt normaal gesproken een fysieke locatie-id, zoals de locatie van een datacenter. | 
| Externe domeinnaam | De naam van de zone System DNS (Domain Name) voor eindpunten met extern gerichte VIP's. In de FQDN-naam gebruikt voor deze openbare VIP's. | 
| Domeinnaam van persoonlijke (intern) | De naam van het domein (en interne DNS-zone) gemaakt op Azure-Stack voor beheer van de infrastructuur. 
| | |

## <a name="certificate-requirements"></a>Certificaatvereisten

Voor implementatie moet u Secure Sockets Layer (SSL) om certificaten te verlenen voor openbare eindpunten. Op een hoog niveau hebben certificaten de volgende vereisten:

> [!IMPORTANT]
> De certificaatgegevens in dit artikel is opgegeven als een algemene richtlijn. Voordat u certificaten voor Azure-Stack aanschaft, kunt u samenwerken met uw OEM-hardware-partner. Geven ze meer gedetailleerde certificaat richtlijnen en vereisten.

- U kunt een enkel jokertekencertificaat of gebruiken van een set specifieke certificaten en het gebruik van jokertekens alleen voor eindpunten zoals opslag en Sleutelkluis.
- Certificaten moeten worden uitgegeven door een openbaar vertrouwde certificeringsinstantie (CA) of een CA door de klant beheerd.
 
De volgende tabel ziet u de services en het aantal openbare eindpunten die certificaten voor de eerste Azure-Stack-implementatie vereisen. 

| Gebruikt voor | Eindpunt 
| -------- | ------------- | 
| Azure Resource Manager (beheerder) | adminmanagment. [regio]. [external_domain] |
| Azure Resource Manager (gebruiker) | beheer. [regio]. [external_domain] |
| Portal (beheerder) | adminportal. [regio]. [external_domain] |
| Portal (gebruiker) | Portal. [regio]. [external_domain] |
| Sleutelkluis (gebruiker) | &#42;. kluis. [regio]. [external_domain] |
| Sleutelkluis (beheerder) | &#42;. adminvault. [regio]. [external_domain] |
| Storage | &#42;. BLOB. [regio]. [external_domain]<br>&#42;. de tabel. [regio]. [external_domain]<br>&#42;. wachtrij. [regio]. [external_domain]  |
| Grafiek ** | grafiek. [regio]. [external_domain] |
| AD FS ** | AD FS. [regio]. [external_domain] |
| | |

** Certificaten voor de grafiek en AD FS-eindpunten zijn alleen nodig voor AD FS-implementaties.

Als u een jokertekencertificaat één gebruiken wilt, moet u een totaal van zes alternatieve namen van certificaathouders (SAN's) voor de initiële implementatie van Azure-Stack. Deze SAN's zijn: 

- &#42;. [regio]. [external_domain]
- &#42;. kluis. [regio]. [external_domain]
- &#42;. adminvault. [regio]. [external_domain]
- &#42;. BLOB. [regio]. [external_domain]
- &#42;. de tabel. [regio]. [external_domain] 
- &#42;. wachtrij. [regio]. [external_domain]

## <a name="time-synchronization"></a>Tijdsynchronisatie

De Azure-Stack-time-server moet u synchroniseren met een externe tijd-server die kan worden omgezet via IP-adres. Een tijdserver op het bedrijfsnetwerk is vereist voor de implementatie van een niet-verbonden.

## <a name="network-connectivity"></a>Netwerkverbinding

### <a name="dns-integration"></a>DNS-integratie

Om op te lossen externe DNS-namen uit Azure-Stack (bijvoorbeeld www.bing.com), moet u DNS-servers die Azure-Stack gebruiken kunt voor het doorsturen van DNS-aanvragen waarvoor Azure-Stack niet gemachtigd is bieden. Als invoer voor implementatie, moet u ten minste twee servers als DNS-doorstuurservers wilt gebruiken voor fouttolerantie opgeven.

U kunt oplossen door DNS-namen van eindpunten Azure Stack uit externe Azure-Stack (bijvoorbeeld van het zakelijke forest), moet u de DNS-servers die de externe DNS-zone met de DNS-servers die als host fungeren van de bovenliggende zone die u wilt gebruiken als host voor Azure-Stack integreren. Hiervoor moet de DNS-naamomzetting tussen Azure-Stack en bestaande DNS-zones in het datacenter. Hiervoor gebruikt u methoden zoals voorwaardelijk doorsturen of zone-overdracht. Het is raadzaam voorwaardelijk doorsturen, hebt u direct controle over de DNS-servers die als host fungeren voor de bovenliggende zone voor uw Azure-Stack externe DNS-naamruimte. (Als de externe Azure-Stack-DNS-zone wordt weergegeven als een onderliggend domein van uw zakelijke domeinnaam (bijvoorbeeld azurestack.contoso.com en contoso.com), moet u zoneoverdracht in plaats daarvan.

### <a name="network-infrastructure"></a>Netwerkinfrastructuur

De netwerkinfrastructuur voor Azure-Stack bestaat uit verschillende logische netwerken die zijn geconfigureerd op de switches. Het volgende diagram toont deze logische netwerken, en hoe ze integreren met de top-of-rack (TOR), BMC (baseboard management controller) en (klant) netwerkswitches rand.

![Logisch netwerk en de switch-verbindingen](media/azure-stack-planning-considerations/NetworkDiagram.png)

De volgende tabel ziet u de logische netwerken en bijbehorende IPv4-subnet bereiken die u moet plannen.

| Logisch netwerk | Beschrijving | Grootte | 
| -------- | ------------- | ------------ | 
| Openbare VIP | Openbare IP-adressen voor een kleine set van Stack Azure-services, met de rest gebruikt door virtuele machines van tenants. 32-adressen binnen dit netwerk maakt gebruik van de Azure-Stack-infrastructuur. Als u van plan bent om App Service en de SQL-resourceproviders te gebruiken, moet deze 7 meer gebruikt. | / 26 (62 hosts) - /22 (1022 hosts)<br><br>Aanbevolen = /24 (254 hosts) | 
| Switch-infrastructuur | Point-to-Point IP-adressen voor de routering, toegewezen switch beheerinterfaces en loopback-adressen toegewezen aan de switch. | /26 | 
| Infrastructuur | Gebruikt voor interne Azure Stack-onderdelen communiceren. | /24 |
| Privé | Gebruikt voor het opslagnetwerk en persoonlijke VIP's. | /24 | 
| BMC | Gebruikt voor communicatie met de BMC's op de fysieke hosts. | /27 | 
| | | |

### <a name="uplink-to-border-switches"></a>Uplinkpoortprofiel met rand switches

U moet een uplinkpoortprofiel die zijn geconfigureerd op de rand switches in uw datacenter. U kunt versturen deze Layer 3-verkeer van de top-of-rack (TOR)-switches die deel uitmaken van een Azure-Stack geïntegreerd systeem met een van de volgende methoden:

- Border Gateway Protocol (BGP) 
- statische routering

BGP wordt aangeraden omdat hij Hiermee het automatisch bijwerken van routes die zijn gepubliceerd door de Software Load Balancing Multiplexer (SLB MUX) met externe netwerken. Dit is belangrijk als u openbare IP-adresbereiken na de implementatie toevoegen. Als u BGP-peering van dit is de TOR schakelt over naar de cumulatieve switch of de TOR-switches zijn verbonden met, openbare IP-adresbereiken dat u toevoegt na de implementatie automatisch worden geadverteerd naar de cumulatieve switch uitvoeren zonder handmatige tussenkomst. Als u statische routering, moet u de routes voor de nieuwe bereiken handmatig bijwerken telkens wanneer u een openbaar IP-adresblok toevoegen.

### <a name="proxy-server"></a>Proxyserver

Azure-Stack ondersteunt alleen transparentproxy servers. Een transparentproxy onderschept aanvragen op de netwerklaag zonder speciale clientconfiguratie.

### <a name="publish-azure-stack-services"></a>Azure-Stack services publiceren

U moet Azure Stack services beschikbaar stellen aan gebruikers van buiten Azure-Stack. Azure Stack stelt u verschillende eindpunten voor de functies van de infrastructuur. Deze eindpunten toegewezen VIP's uit de openbare IP-adresgroep. Een DNS-vermelding wordt voor elk eindpunt voor de externe DNS-zone, is opgegeven tijdens de implementatie gemaakt. De gebruikersportal krijgt bijvoorbeeld de DNS-host-vermelding van '-portal. <*regio*>. <*external_FQDN*>. " 

#### <a name="ports-and-urls"></a>Poorten en URL 's

Om ervoor te Stack Azure-services (zoals de portals Azure Resource Manager, DNS, etc.) beschikbaar met externe netwerken, moet u binnenkomend verkeer op deze eindpunten toestaan voor specifieke URL's, poorten en protocollen.
 
In een implementatie waarbij een transparentproxy uplinkpoortprofiel met een traditioneel proxyserver, moet u toestaan bepaalde poorten en URL's voor uitgaande communicatie. Deze omvatten poorten en URL's voor identiteit, marketplace syndication, patch en update, registratie en gebruiksgegevens.

Zie voor meer informatie:
- [Poorten voor inkomend verkeer en protocollen](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Uitgaande poorten en URL 's](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Verbinding maken met Azure Stack naar Azure

Voor hybride cloud-scenario's moet u plannen hoe u wilt verbinding maken met Azure-Stack naar Azure. Er zijn twee manieren verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure: 
- **Site-naar-site**. Een virtueel particulier netwerk (VPN)-verbinding via IPsec (IKE v1 en IKE v2). Dit type verbinding vereist een VPN-apparaat of Routing and Remote Access Service (RRAS). Zie voor meer informatie over VPN-gateways in Azure [over VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). De communicatie via deze tunnel wordt gecodeerd en beveiligd. Bandbreedte wordt echter wel beperkt door de maximale doorvoer van de tunnel (100-200 Mbps).
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
| Multitenant Azure Stack, implementatie op een intranet | Site-naar-site VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | In bedrijfsimplementaties waar sommige tenantverkeer moeten van andere tenants worden beveiligd. |
| Eén tenant Stack in Azure, internet-implementatie | Uitgaande NAT | Grotere bandbreedte voor snellere overdrachten. | Verkeer dat niet is versleuteld. Er is geen isolatie- of -versleuteling buiten de TOR. | Hostingscenario's waarin de tenant opgehaald hun eigen Azure Stack-implementatie en een speciaal circuit aan de Stack van Azure-omgeving. ExpressRoute en Multiprotocol Label overschakelen (MPLS).
| Multitenant-Stack voor Azure, internet-implementatie | Site-naar-site VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | Hostingscenario's waarin de provider wil bieden een multitenant-cloud, waar de tenants niet vertrouwt elkaar en verkeer moeten worden versleuteld.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Met behulp van ExpressRoute

U kunt Azure-Stack verbinden met Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) voor één tenant intranet en scenario's voor meerdere tenants. U moet een ExpressRoute-circuit via ingerichte [een connectiviteitsprovider](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Het volgende diagram toont ExpressRoute voor een scenario voor één tenant (waarbij 'Verbinding van de klant' is van het ExpressRoute-circuit).

![Diagram waarin één tenant ExpressRoute scenario](media/azure-stack-planning-considerations/ExpressRouteSingleTenant.PNG)

Het volgende diagram toont ExpressRoute voor een scenario met meerdere tenants.

![Diagram waarin meerdere tenants ExpressRoute scenario](media/azure-stack-planning-considerations/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externe controle
Ophalen van één enkele weergave van alle waarschuwingen van uw Azure-Stack-implementatie en apparaten en waarschuwingen integreren met bestaande werkstromen voor IT-service voor tickets, kunt u Azure-Stack integreren met externe datacenter bewakingsoplossingen.

De host van de levenscyclus van hardware is opgenomen met de Azure-Stack-oplossing, is een computer buiten een Azure-Stack die wordt uitgevoerd door OEM geleverde leverancier beheerhulpprogramma's voor hardware. U kunt deze hulpprogramma's of andere oplossingen die rechtstreeks worden geïntegreerd met bestaande bewakingsoplossingen in uw datacenter.

De volgende tabel geeft een overzicht van de lijst met beschikbare opties.

| Onderwerp | Oplossing voor externe controle |
| -- | -- |
| Azure Stack-software. | - [Azure Stack Management Pack voor Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Invoegtoepassing Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST gebaseerde API-aanroepen | 
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
