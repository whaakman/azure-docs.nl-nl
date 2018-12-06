---
title: Aandachtspunten voor algemene datacenter-integratie voor Azure Stack-geïntegreerde systemen | Microsoft Docs
description: Meer informatie over wat u kunt doen om nu plannen en voorbereiden voor de datacenter-integratie met meerdere knooppunten Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 3cbe7c1d458e95aa27c51f0d1c9f61326888a5ab
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962678"
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Datacenter-integratie-overwegingen voor Azure Stack-geïntegreerde systemen
Als u geïnteresseerd in een geïntegreerde Azure Stack-systeem bent, moet u enkele van de belangrijkste Beschouwingen over planning om de implementatie en hoe het systeem past in uw datacenter te begrijpen. In dit artikel bevat een overzicht van deze overwegingen om u te helpen u belangrijke infrastructuur beslissingen voor uw Azure Stack-systeem met meerdere knooppunten. Een goed begrip van deze overwegingen helpt bij het werken met de leverancier van de OEM-hardware omdat ze Azure Stack in uw datacenter implementeren.  

> [!NOTE]
> Azure Stack-systemen met meerdere knooppunten kunnen alleen worden gekocht van hardwareleveranciers geautoriseerde. 

Voor het implementeren van Azure Stack, moet u planningsinformatie bieden aan uw solution provider voordat implementatie is gestart om u te helpen bij het proces gaat u snel en goed. Deze informatie nodig bereiken voor netwerken, beveiliging en identiteitsgegevens met veel belangrijke beslissingen waarvoor kennis van veel verschillende gebieden en besluitvormers. U moet daarom halen mensen van meerdere teams in uw organisatie om ervoor te zorgen dat u alle vereiste informatie bij de hand hebt voordat de implementatie begint. Het kan helpen om te communiceren met uw hardwareleverancier bij het verzamelen van deze informatie, zoals hebben ze mogelijk advies handig om uw beslissingen.

Bij het onderzoeken en de vereiste gegevens verzamelen, moet u mogelijk enkele wijzigingen in de configuratie van vóór de implementatie aanbrengen in uw netwerkomgeving. Dit omvat bijvoorbeeld reserveren van IP-adresruimten voor de Azure Stack-oplossing, configureren van uw routers, switches en firewalls om voor te bereiden voor de verbinding met de nieuwe schakelopties voor Azure Stack-oplossing. Zorg ervoor dat u hebt het onderwerp gebied expert kan samenwerken tot helpt u bij de planning.

## <a name="capacity-planning-considerations"></a>Overwegingen voor capaciteitsplanning
Bij het evalueren van een Azure Stack-oplossing voor de aanschaf van moeten hardware-configuratie-opties worden gemaakt die een directe invloed hebben op de totale capaciteit van de Azure Stack-oplossing. Het gaat hierbij om de klassieke keuzes van CPU, geheugen-dichtheid opslagconfiguratie en algehele oplossing schalen (bijvoorbeeld aantal servers). De eenvoudige rekenkundige van deze onderdelen om te bepalen aan bruikbare capaciteit in tegenstelling tot een traditionele virtualisatieoplossing, is niet van toepassing. De eerste reden is dat Azure Stack is ontworpen voor het hosten van de infrastructuur of management onderdelen binnen de oplossing zelf. De tweede reden is dat een aantal van de oplossing capaciteit wordt gereserveerd ter ondersteuning van tolerantie; het bijwerken van de software van de oplossing op een manier die wordt geminimaliseerd tot een onderbreking van tenantwerkbelastingen. 

De [Azure Stack capaciteit planner werkblad](https://aka.ms/azstackcapacityplanner) helpt u geïnformeerd beslissingen te nemen met betrekking tot het plannen van capaciteit op twee manieren: ofwel de door het selecteren van een hardware-aanbieding en wilt aanpassen aan een combinatie van resources of door te definiëren de workload die door Azure Stack is bedoeld om uit te voeren als u wilt weergeven van de beschikbare hardware-SKU's dat kan worden ondersteund. Ten slotte is het werkblad bedoeld als richtlijn om te helpen bij het nemen van beslissingen met betrekking tot Azure Stack-planning en configuratie. 

Het werkblad is niet bedoeld als vervanging voor uw eigen onderzoek en analyse.  Microsoft geeft geen verklaringen en garanties, expliciet of impliciet, met betrekking tot de informatie in het werkblad.



## <a name="management-considerations"></a>Overwegingen met betrekking tot
Azure Stack is een gesloten systeem, waarbij de infrastructuur is vergrendeld in een machtigingen en perspectief. Netwerk-toegangsbeheerlijsten (ACL's) worden toegepast voor het blokkeren van inkomend verkeer van alle niet-geautoriseerde en alle onnodige communicatie tussen onderdelen van de infrastructuur. Dit maakt het moeilijk voor onbevoegde gebruikers toegang tot het systeem.

Voor het dagelijkse beheer en bewerkingen is er geen onbeperkte beheerderstoegang tot de infrastructuur. Azure Stack-operators moeten het systeem via het administrator-portal of via Azure Resource Manager (via PowerShell of de REST-API) beheren. Er is geen toegang tot het systeem door andere beheerprogramma's zoals Hyper-V-beheer of Failover-clusterbeheer. Ter bescherming van het systeem, kan niet software van derden (bijvoorbeeld agenten) worden geïnstalleerd in de onderdelen van de Azure Stack-infrastructuur. Interoperabiliteit met extern beheer en beveiliging software vindt plaats via PowerShell of de REST-API.

Wanneer een hoger niveau van toegang nodig is voor het oplossen van problemen die niet worden opgelost door waarschuwing uitoefenen stappen, moet u werken met Microsoft Support. Door middel van ondersteuning is er een methode voor tijdelijke volledige beheerderstoegang tot het systeem meer geavanceerde bewerkingen uit te voeren. 

## <a name="identity-considerations"></a>Overwegingen met betrekking tot identiteit

### <a name="choose-identity-provider"></a>Kies de id-provider
U moet rekening houden met welke id-provider die u wilt gebruiken voor Azure Stack-implementatie, Azure AD of AD FS. U kunt niet de id-providers overstappen na de implementatie zonder het volledige systeem opnieuw implementeren. Als u geen eigenaar van de Azure AD-account en een account dat aan u geleverd door uw Cloudserviceprovider, en als u wilt overschakelen van de provider en gebruiken van een andere Azure AD-account, moet op dit moment u contact op met uw oplossingsprovider voor het implementeren van de f-oplossing of u uw kosten.



Uw keuze van id-provider heeft geen gevolgen voor virtuele machines van tenants, identiteitssysteem en accounts die ze gebruiken, of ze kunnen deelnemen aan een Active Directory-domein, enzovoort. Dit staat los.

U kunt meer informatie over het kiezen van een id-provider in de [Azure Stack geïntegreerde systemen verbinding modellen artikel](./azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>AD FS en Graph-integratie
Als u ervoor Azure Stack als id-provider met behulp van AD FS implementeren kiest, moet u het exemplaar van AD FS in Azure Stack integreren met een bestaande AD FS-exemplaar via een federatieve vertrouwensrelatie. Hiermee wordt de identiteiten in een bestaand Active Directory-forest om te verifiëren met resources in Azure Stack.

U kunt de Graph-service in Azure Stack ook integreren met bestaande Active Directory. Hiermee kunt u voor het beheren van rollen gebaseerd toegangsbeheer (RBAC) in Azure Stack. Wanneer toegang tot een resource wordt overgedragen, wordt de Graph-component zoekt u het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

Het volgende diagram toont de geïntegreerde verkeersstroom voor AD FS en Graph.
![Diagram van AD FS en een graaf verkeersstroom](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Licentiemodel
U moet beslissen welke licentiemodel die u wilt gebruiken. De beschikbare opties is afhankelijk van of u Azure Stack is verbonden met internet implementeren:
- Voor een [verbonden implementatie](azure-stack-connected-deployment.md), kunt u betalen als u-gebruik of op basis van capaciteit-licentieverlening. Betalen als u-gebruik moet worden verbonden met Azure rapporteren over het gebruik, die vervolgens via Azure commerce worden gefactureerd. 
- Alleen op basis van capaciteit licentieverlening wordt ondersteund als u [implementeren verbroken](azure-stack-disconnected-deployment.md) vanaf het internet. 

Zie voor meer informatie over de licentiemodellen [Microsoft Azure Stack verpakking en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Beslissingen voor de naamgeving

U moet denken over de manier waarop u wilt plannen van uw Azure Stack-naamruimte, met name de regionaam en de externe naam. De externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie voor openbare eindpunten is de combinatie van deze twee namen: &lt; *regio*&gt;.&lt; *fqdn*&gt;. Bijvoorbeeld, *east.cloud.fabrikam.com*. In dit voorbeeld zou de Azure Stack-portals zijn beschikbaar op de volgende URL's:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> De regionaam die u voor uw Azure Stack-implementatie kiest moet uniek zijn en wordt weergegeven in de portal-adressen. 

De volgende tabel geeft een overzicht van deze domein naming beslissingen.

| Naam | Beschrijving | 
| -------- | ------------- | 
|Regionaam | De naam van uw eerste Azure Stack-regio. Deze naam wordt gebruikt als onderdeel van de FQDN-naam voor de openbare virtuele IP-adressen (VIP's) die Azure Stack wordt beheerd. De regionaam van de is meestal een fysieke locatie-id, zoals de locatie van een datacenter.<br><br>De regionaam van de moet bestaan uit alleen letters en getallen tussen 0-9. Er zijn geen speciale tekens zoals "-" of "#", enzovoort zijn toegestaan.| 
| De naam van het externe domein | De naam van de Domain Name System (DNS)-zone voor eindpunten met extern gerichte VIP's. In de FQDN-naam gebruikt voor deze openbare VIP's. | 
| Domeinnaam van persoonlijke (intern) | De naam van het domein (en interne DNS-zone) gemaakt in Azure Stack voor beheer van infrastructuur. 
| | |

## <a name="certificate-requirements"></a>Vereisten voor certificaten

Voor de implementatie moet u voor Secure Sockets Layer (SSL)-certificaten voor openbare eindpunten. Op hoog niveau hebben certificaten de volgende vereisten:

- U kunt een certificaat voor één jokerteken gebruiken of u kunt gebruikmaken van een set van specifieke certificaten en jokertekens gebruiken alleen voor eindpunten, zoals opslag en Key Vault.
- Certificaten kunnen worden uitgegeven door een vertrouwde openbare certificeringsinstantie (CA) of een door de klant beheerde CA.

Voor meer informatie over welke PKI certificaten zijn vereist voor het implementeren van Azure Stack en over het verkrijgen van deze kunt zien, [certificaatvereisten voor Azure Stack Public Key Infrastructure](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> De gegevens van het opgegeven PKI-certificaat moet worden gebruikt als algemene richtlijn. Voordat u een PKI-certificaten voor Azure Stack aanschaffen, werkt u samen met uw OEM-hardware-partner. Ze biedt meer gedetailleerde certificaat richtlijnen en vereisten.


## <a name="time-synchronization"></a>Tijdsynchronisatie
U moet een specifiek tijdstip-server met wordt gebruikt om te synchroniseren van Azure Stack.  Tijd symbolization is essentieel voor Azure Stack en de infrastructuur functies, zoals het wordt gebruikt voor het genereren van Kerberos-tickets die worden gebruikt voor verificatie van interne services met elkaar.

U moet dat een IP-adres voor de tijd synchronisatie-server, hoewel de meeste van de onderdelen van de infrastructuur van een URL kunnen omzetten sommige biedt alleen ondersteuning voor IP-adressen. Als u bent zijn met behulp van de niet-verbonden Implementatieoptie wordt gebruikt, moet u een tijdserver in uw bedrijfsnetwerk dat u ervoor dat kan worden bereikt vanaf het infrastructuurnetwerk in Azure Stack.

## <a name="connect-azure-stack-to-azure"></a>Azure Stack verbinden met Azure

Voor hybride cloud-scenario's moet u plannen hoe u wilt verbinding maken met Azure Stack in Azure. Er zijn twee ondersteunde manieren verbinding maken van virtuele netwerken in Azure Stack met virtuele netwerken in Azure: 
- **Site-naar-site**. Een virtueel particulier netwerk (VPN)-verbinding via IPsec (IKE v1 en IKE v2). Dit type verbinding is vereist voor een VPN-apparaat of Routing and Remote Access Service (RRAS). Zie voor meer informatie over VPN-gateways in Azure, [over VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). De communicatie via deze tunnel wordt gecodeerd en beveiligd. Bandbreedte wordt echter beperkt door de maximale doorvoer van de tunnel (100-200 Mbps).
- **Uitgaande NAT**. Standaard hebben alle virtuele machines in Azure Stack verbinding met externe netwerken via uitgaande NAT. Elk virtueel netwerk dat is gemaakt in Azure Stack Hiermee haalt u een openbare IP-adres is toegewezen. Of de virtuele machine direct een openbaar IP-adres wordt toegewezen of achter een load balancer met een openbaar IP-adres is, is er uitgaande toegang via de uitgaande NAT met behulp van het VIP van het virtuele netwerk. Dit werkt alleen voor communicatie die is gestart door de virtuele machine en dat is bestemd voor externe netwerken (internet of intranet). Het kan niet worden gebruikt om te communiceren met de virtuele machine van buiten.

### <a name="hybrid-connectivity-options"></a>Hybride connectiviteitsopties

Voor hybride verbindingen is het belangrijk te bedenken wat voor soort implementatie die u wilt aanbieden en waarop deze wordt geïmplementeerd. U moet overwegen of u nodig hebt om te isoleren netwerkverkeer per tenant, en of u hebt een intranet of internet-implementatie.

- **Azure Stack voor één tenant**. Een Azure Stack-implementatie die er, ten minste vanuit het netwerk oogpunt, uitziet alsof het één tenant. Kunnen er dat veel abonnementen tenant, maar zoals een intranet-service al het verkeer wordt verzonden via dezelfde netwerken. Netwerkverkeer van één abonnement verloopt via de dezelfde netwerkverbinding als een ander abonnement en hoeft niet te worden geïsoleerd via een gecodeerde tunnel.

- **Azure Stack voor meerdere tenants**. Een Azure Stack-implementatie waarbij elk tenantabonnement van de-verkeer dat gekoppeld aan netwerken die zich buiten Azure Stack geïsoleerd van andere tenants netwerkverkeer zijn moet.
 
- **Implementatie op een intranet**. Een Azure Stack-implementatie die bevindt zich op een bedrijfsintranet, meestal op privé-IP-adresruimte en achter een firewall voor een of meer. Het openbare IP-adressen zijn niet echt openbaar, als ze rechtstreeks via het openbare internet kunnen niet worden doorgestuurd.

- **Implementatie van Internet**. Een Azure Stack-implementatie die verbonden met het openbare internet en maakt gebruik van internet routeerbare openbare IP-adressen voor het openbare VIP-adresbereik. De implementatie kan nog steeds zitten achter een firewall bevindt, maar het openbare VIP-adresbereik is rechtstreeks bereikbaar is vanaf het openbare internet en Azure.
 
De volgende tabel geeft een overzicht van de hybride verbinding scenario's, met de professionals, nadelen en use-cases.

| Scenario | Verbindingsmethode | Professionals | Nadelen | Goed voor |
| -- | -- | --| -- | --|
| Eén tenant Azure Stack, implementatie op een intranet | Uitgaande NAT | Grotere bandbreedte voor snellere bestandsoverdrachten. Eenvoudig te implementeren; Er is geen gateways dat nodig is. | Verkeer dat niet is versleuteld. Er is geen isolatie- of -versleuteling buiten de stack. | Enterprise-implementaties waarin alle tenants gelijkmatig vertrouwd worden.<br><br>Ondernemingen die u een Azure ExpressRoute-circuit naar Azure hebt. |
| Multitenant Azure Stack, implementatie op een intranet | Site-naar-site-VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | Enterprise-implementaties waarin bepaalde-verkeer tenant moeten worden beveiligd van andere tenants. |
| Eén tenant Azure Stack, internet-implementatie | Uitgaande NAT | Grotere bandbreedte voor snellere bestandsoverdrachten. | Verkeer dat niet is versleuteld. Er is geen isolatie- of -versleuteling buiten de stack. | Hostingscenario's waarin de tenant hun eigen Azure Stack-implementatie en een speciale circuit met de Azure Stack-omgeving haalt. Bijvoorbeeld, ExpressRoute en Multiprotocol Label Switching (MPLS).
| Multitenant Azure Stack, internet-implementatie | Site-naar-site-VPN | Verkeer van de tenant VNet naar de bestemming is beveiligd. | Bandbreedte wordt beperkt door de site-naar-site VPN-tunnel.<br><br>Vereist een gateway in het virtuele netwerk en een VPN-apparaat op het doelnetwerk. | Hostingscenario's waarin de provider wil bieden een multitenant-cloud, waar de tenants niet vertrouwt elkaar en het verkeer moeten worden versleuteld.
|  |  |  |  |  |

### <a name="using-expressroute"></a>Met behulp van ExpressRoute

U kunt Azure Stack verbinding maken met Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) voor één tenant intranet- en scenario's met meerdere tenants. U moet een ingerichte ExpressRoute-circuit via [een connectiviteitsprovider](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Het volgende diagram toont ExpressRoute voor een scenario met één tenant (waarbij 'Verbinding van de klant' is van het ExpressRoute-circuit).

![Diagram waarin één tenant ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Het volgende diagram toont ExpressRoute voor een scenario met meerdere tenants.

![Diagram waarin meerdere tenants ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externe bewaking
Een weergave van alle waarschuwingen ophalen van uw Azure Stack-implementatie en de apparaten en waarschuwingen integreren met bestaande IT service management-werkstromen voor tickets, kunt u [Integreer Azure Stack met externe datacenter bewakingsoplossingen](azure-stack-integrate-monitor.md).

De host van de levenscyclus van hardware is opgenomen in de Azure Stack-oplossing, is een computer buiten Azure Stack die wordt uitgevoerd door OEM geleverde leverancier-beheerhulpprogramma's voor hardware. U kunt deze hulpprogramma's of andere oplossingen die rechtstreeks worden geïntegreerd met bestaande bewakingsoplossingen in uw datacenter.

De volgende tabel geeft een overzicht van de lijst met beschikbare opties.

| Onderwerp | Oplossing voor externe controle |
| -- | -- |
| Azure Stack-software | [Azure Stack Managementpack voor Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios-invoegtoepassing](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>Op basis van REST API-aanroepen | 
| Fysieke servers (bmc's via IPMI) | OEM-hardware - leverancier van managementpack van Operations Manager<br>OEM-hardware leverancier verstrekte-oplossing<br>Hardwareleverancier Nagios-invoegtoepassingen | OEM-partner ondersteund voor controle (opgenomen) | 
| Netwerkapparaten (SNMP) | Operations Manager de detectie van netwerkapparaten<br>OEM-hardware leverancier verstrekte-oplossing<br>Nagios-switch invoegtoepassing |
| Statuscontrole voor tenant-abonnement | [System Center Management Pack voor Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Houd rekening met de volgende vereisten:
- De oplossing die u gebruikt moet worden zonder agent. U kunt agents in Azure Stack-onderdelen van derden niet installeren. 
- Als u gebruiken van System Center Operations Manager wilt, is Operations Manager 2012 R2 of Operations Manager 2016 vereist.

## <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen

Planning voor back-up en herstel na noodgevallen omvat het plannen van zowel de onderliggende Azure Stack-infrastructuur die als host fungeert voor virtuele machines van IaaS en PaaS-services en voor tenant-toepassingen en gegevens. U moet afzonderlijk voor deze plannen.

### <a name="protect-infrastructure-components"></a>Onderdelen van de infrastructuur beveiligen

U kunt [maakt u een back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md) infrastructuuronderdelen naar een SMB-share die u opgeeft:

- U moet een externe SMB-bestandsshare op een bestaande Windows-bestandsserver of een apparaat van derden.
- U moet deze dezelfde share voor de back-up van de netwerkswitches en de host van de levenscyclus van hardware. De leverancier van de OEM-hardware kunnen bieden van richtlijnen voor back-up en herstel van deze onderdelen als deze zich buiten Azure Stack. U bent verantwoordelijk voor het uitvoeren van de back-werkstromen op basis van de aanbeveling van de OEM-leverancier.

Als onherstelbaar gegevensverlies optreedt, kunt u de infrastructuur voor back-up om een seedbewerking uit implementatiegegevens zoals de implementatie-invoer en id's, service-accounts, CA-basiscertificaat, federatieve bronnen (in niet-verbonden implementaties), plannen, aanbiedingen, te gebruiken abonnementen, quota, toewijzingen van beleid en de rol RBAC en Key Vault-geheimen.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Tenant-toepassingen op virtuele IaaS-machines beveiligen

Azure Stack maakt geen back-up van de tenant-toepassingen en gegevens. U moet plannen voor back-ups en disaster recovery-beveiliging met een doel buiten Azure Stack. Tenant-beveiliging is een tenant gebaseerde-activiteit. Tenants kunnen in de Gast-technologieën gebruiken om mappen, toepassingsgegevens en de systeemstatus te beveiligen voor virtuele machines van IaaS. Als een enterprise- of -service-provider kunt u echter te bieden een oplossing back-up en herstel in hetzelfde datacenter of extern in een cloud.

Als u wilt back-up van Linux of Windows IaaS virtuele machines, moet u back-producten met toegang tot het gastbesturingssysteem gebruiken om te beveiligen bestand, map, staat van besturingssysteem en toepassingsgegevens. U kunt Azure back-up, System Center Data Center Protection Manager, of producten van derden wordt ondersteund.

Als u wilt repliceren van gegevens naar een secundaire locatie en failover van de toepassing indelen als zich een noodgeval voordoet, kunt u Azure Site Recovery of ondersteunde producten van derden gebruiken. Toepassingen die ondersteuning bieden voor systeemeigen replicatie, zoals Microsoft SQL Server, kunnen ook gegevens repliceren naar een andere locatie waar de toepassing wordt uitgevoerd.

## <a name="learn-more"></a>Meer informatie

- Zie voor informatie over gebruiksvoorbeelden, aanschaffen, partners en leveranciers van OEM-hardware, de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Voor informatie over de roadmap en geo-beschikbaarheid voor Azure Stack geïntegreerde systemen, Zie het technische document: [Azure Stack: een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Volgende stappen
[Azure Stack-verbinding implementatiemodellen](azure-stack-connection-models.md)
