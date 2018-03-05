---
title: "Algemene datacenter Integratieoverwegingen voor het Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Meer informatie over wat u kunt doen om nu plan en bereid voor datacenter-integratie met meerdere knooppunten Azure Stack.
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
ms.date: 03/02/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 25ef6ba9ff105486f39cee8b6181a8c63e64ec13
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Aandachtspunten voor Datacenter-integratie voor Azure-Stack geïntegreerd systemen
Als u geïnteresseerd in een Azure-Stack geïntegreerd systeem bent, moet u enkele van de belangrijke planningsoverwegingen rond de implementatie en hoe het systeem in uw datacenter past begrijpen. Dit artikel bevat een overzicht van deze overwegingen bij de infrastructuur van belangrijke beslissingen voor uw Azure-Stack-systeem met meerdere knooppunten. Een goed begrip van deze overwegingen helpt bij het werken met uw hardwareleverancier OEM als ze Azure Stack op uw datacenter implementeren.  

> [!NOTE]
> Azure Stack systemen met meerdere knooppunten kunnen alleen worden gekocht bij geautoriseerde hardwareleveranciers. 

U moet voor het implementeren van Azure-Stack planningsinformatie bieden aan uw solution provider voordat implementatie begint te helpen bij het proces gaat soepel en snel. Deze informatie nodig bereiken over netwerken-, beveiligings- en identiteitsgegevens met veel belangrijke beslissingen waarvoor kennis van veel verschillende gebieden en besluitvormers. U moet daarom ophalen van de mensen van meerdere teams in uw organisatie om ervoor te zorgen dat u alle vereiste informatie bij de hand hebt voordat de implementatie begint. Kunt u contact opnemen met uw hardwareleverancier bij het verzamelen van deze informatie zoals ze advies die nuttig zijn hebben wellicht voor uw beslissingen.

Tijdens het onderzoeken en de vereiste gegevens verzamelen, moet u mogelijk enkele vóór implementatie configuratiewijzigingen aanbrengen in uw netwerkomgeving. Dit omvat het reserveren van IP-adresruimten voor de Azure-Stack-oplossing, uw routers, switches en firewalls om voor te bereiden voor de verbinding met de nieuwe Azure-Stack oplossing switches configureren. Zorg ervoor dat u hebt het onderwerp gebied expert die maximaal helpt u bij de planning.

## <a name="capacity-planning-considerations"></a>Overwegingen bij capaciteitsplanning
Bij het evalueren van een Azure-Stack-oplossing voor overname moeten configuratiemogelijkheden hardware worden gemaakt die een directe invloed hebben op de totale capaciteit van de Azure-Stack-oplossing. Het gaat hierbij om klassieke keuzes CPU, geheugen dichtheid, opslagconfiguratie en de algehele oplossing voor scale (bijvoorbeeld het aantal servers). De eenvoudige rekenkundige van deze onderdelen bruikbare capaciteit bepalen in tegenstelling tot een traditionele virtualisatieoplossing is niet van toepassing. De eerste reden is dat Azure-Stack is ontworpen voor het hosten van de infrastructuur of management componenten binnen de oplossing zelf. De tweede reden is dat enkele van de capaciteit van de oplossing is gereserveerd ter ondersteuning van tolerantie; het bijwerken van de oplossing software op een manier die onderbreking van de tenantwerkbelastingen minimaliseert. 

De [Azure Stack capaciteit planner werkblad](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) helpt u geïnformeerd beslissingen te nemen met betrekking tot het plannen van capaciteit op twee manieren: ofwel de door een hardware-aanbieding selecteren en aanpassen aan een combinatie van bronnen of probeert door te definiëren de werkbelasting die Azure-Stack is bedoeld om uit te voeren om weer te geven van de beschikbare hardware-SKU's dat kan worden ondersteund. Ten slotte is het werkblad bedoeld als richtlijn bij het nemen van besluiten betrekking op Azure-Stack planning en configuratie hebben. 

Het werkblad is niet bedoeld om te worden vervangen voor uw eigen onderzoek en analyse.  Microsoft geeft geen enkele expliciete of impliciete met betrekking tot de informatie in het werkblad.



## <a name="management-considerations"></a>Beheeroverwegingen
Azure-Stack is een verzegeld systeem waar de infrastructuur is vergrendeld zowel vanuit een machtigingen en netwerk-perspectief. Netwerk toegangsbeheerlijsten (ACL's) worden toegepast voor het blokkeren van inkomend verkeer van alle niet-geautoriseerde en alle onnodige communicatie tussen onderdelen van de infrastructuur. Dit maakt het moeilijk voor onbevoegde gebruikers toegang tot het systeem.

Voor het dagelijkse beheer en bewerkingen is er geen onbeperkte beheerderstoegang tot de infrastructuur. Azure Stack-operators moeten het systeem via de beheerdersportal of via Azure Resource Manager (via PowerShell of de REST-API) te beheren. Er is geen toegang tot het systeem door andere beheerprogramma's zoals Hyper-V-beheer of Failover Cluster Manager. Ter bescherming van het systeem kan niet binnen de onderdelen van de infrastructuur van Azure-Stack software van derden (bijvoorbeeld agents) worden geïnstalleerd. Interoperabiliteit met externe software voor beheer en beveiliging vindt plaats via PowerShell of de REST-API.

Wanneer een hoger niveau van toegang nodig is voor het oplossen van problemen die door waarschuwing uitoefenen stappen niet zijn opgelost, moet u samenwerken met Microsoft Support. Dankzij de ondersteuning is er een methode voor tijdelijke volledige beheerderstoegang tot het systeem meer geavanceerde bewerkingen uit te voeren. 

## <a name="identity-considerations"></a>Identiteit overwegingen

### <a name="choose-identity-provider"></a>Kies de identiteitsprovider
U moet rekening houden met welke id-provider die u wilt gebruiken voor Azure-Stack-implementatie, Azure AD of AD FS. U kunt geen id-providers overschakelen na implementatie zonder volledige systeem opnieuw installeren.

Uw keuze van de provider identiteit heeft geen invloed op de virtuele machines van tenants, de van identiteitssysteem en accounts die ze gebruiken, of ze kunnen deelnemen aan een Active Directory-domein, enzovoort. Dit is een afzonderlijke.

U kunt meer informatie over het kiezen van een id-provider in de [Azure Stack geïntegreerde systemen verbinding modellen artikel](.\azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>Integratie van AD FS en grafiek
Als u kiest voor het implementeren van Azure-Stack AD FS gebruikt als de id-provider, moet u het exemplaar van AD FS op Azure-Stack integreren met een bestaand exemplaar van AD FS via een federatieve vertrouwensrelatie. Hierdoor identiteiten in een bestaand Active Directory-forest om te verifiëren met resources in Azure-Stack.

U kunt ook de grafiek service in Azure-Stack integreren met bestaande Active Directory. Hiermee kunt u voor het beheren van op rollen gebaseerde toegangsbeheer (RBAC) in Azure-Stack. Wanneer u toegang tot een resource wordt overgedragen, zoekt het onderdeel van de grafiek het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

Het volgende diagram toont de geïntegreerde grafiek en AD FS-netwerkverkeer.
![Diagram van de AD FS en grafiek netwerkverkeer](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Licentiemodel
U moet beslissen welke licentiemodel die u wilt gebruiken. De beschikbare opties is afhankelijk van of u een Azure-Stack is verbonden met internet implementeren:
- Voor een [verbonden implementatie](azure-stack-connected-deployment.md), kunt u betalen-als-gebruik of op basis van capaciteit-licentieverlening. Betalen-als-gebruik vereist een verbinding naar Azure te rapporteren over het gebruik, die vervolgens wordt gefactureerd via Azure commerce. 
- Alleen op basis van capaciteit licentieverlening wordt ondersteund als u [implementeren verbroken](azure-stack-disconnected-deployment.md) vanaf internet. 

Zie voor meer informatie over de licentiemodellen [Microsoft Azure-Stack verpakken en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Beslissingen voor de naamgeving

U moet u bedenken hoe wilt u van plan bent uw Azure-Stack-naamruimte, met name de regio en externe domeinnaam. De externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie voor openbare eindpunten is de combinatie van deze twee namen: &lt; *regio*&gt;.&lt; *fqdn*&gt;. Bijvoorbeeld: *east.cloud.fabrikam.com*. In dit voorbeeld zou de Azure-Stack-portals zijn beschikbaar op de volgende URL's:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> De regionaam van de die u voor uw Azure-Stack-implementatie kiest moet uniek zijn en worden weergegeven in de portal adressen. 

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
U kunt een specifiek tijdstip server met wordt gebruikt voor het synchroniseren van Azure-Stack moet kiezen.  Tijd symbolization is essentieel voor de Azure-Stack en de bijbehorende rollen infrastructuur, zoals het wordt gebruikt voor het genereren van Kerberos-tickets die worden gebruikt voor verificatie van interne services met elkaar.

Geef dat een IP-adres voor de tijd synchronisatie-server, hoewel de meeste van de onderdelen in de infrastructuur van een URL kunnen omzetten sommige biedt slechts ondersteuning voor IP-adressen. Als u klaar bent met de optie voor de verbinding is verbroken, moet u een tijd-server in uw bedrijfsnetwerk dat u ervoor dat kan worden bereikt vanaf het netwerk van de infrastructuur in Azure-Stack.

## <a name="connect-azure-stack-to-azure"></a>Verbinding maken met Azure Stack naar Azure

Voor hybride cloud-scenario's moet u plannen hoe u wilt verbinding maken met Azure-Stack naar Azure. Er zijn twee manieren verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure: 
- **Site-to-site**. Een virtueel particulier netwerk (VPN)-verbinding via IPsec (IKE v1 en IKE v2). Dit type verbinding vereist een VPN-apparaat of Routing and Remote Access Service (RRAS). Zie voor meer informatie over VPN-gateways in Azure [over VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). De communicatie via deze tunnel wordt gecodeerd en beveiligd. Bandbreedte wordt echter wel beperkt door de maximale doorvoer van de tunnel (100-200 Mbps).
- **Uitgaande NAT**. Standaard hebben alle virtuele machines in Azure Stack verbinding met externe netwerken via uitgaande NAT bevinden. Elk virtueel netwerk dat is gemaakt in Azure Stack Hiermee haalt u een openbaar IP-adres toegewezen. Of de virtuele machine direct een openbaar IP-adres wordt toegewezen of zich achter een load balancer met een openbaar IP-adres, heeft dit uitgaande toegang via de uitgaande NAT het VIP van het virtuele netwerk gebruiken. Dit werkt alleen voor communicatie die is gestart door de virtuele machine en bestemd is voor externe netwerken (internet of intranet). Deze kan niet worden gebruikt om te communiceren met de virtuele machine vanuit buiten.

### <a name="hybrid-connectivity-options"></a>Opties voor hybride verbindingen

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

### <a name="using-expressroute"></a>Met behulp van ExpressRoute

U kunt Azure-Stack verbinden met Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) voor één tenant intranet en scenario's voor meerdere tenants. U moet een ExpressRoute-circuit via ingerichte [een connectiviteitsprovider](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Het volgende diagram toont ExpressRoute voor een scenario voor één tenant (waarbij 'Verbinding van de klant' is van het ExpressRoute-circuit).

![Diagram waarin één tenant ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Het volgende diagram toont ExpressRoute voor een scenario met meerdere tenants.

![Diagram waarin meerdere tenants ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externe controle
Ophalen van één enkele weergave van alle waarschuwingen van uw Azure-Stack-implementatie en apparaten en waarschuwingen integreren met bestaande werkstromen voor IT-service voor tickets, kunt u [Azure Stack integreren met externe datacenter bewakingsoplossingen](azure-stack-integrate-monitor.md).

De host van de levenscyclus van hardware is opgenomen met de Azure-Stack-oplossing, is een computer buiten een Azure-Stack die wordt uitgevoerd door OEM geleverde leverancier beheerhulpprogramma's voor hardware. U kunt deze hulpprogramma's of andere oplossingen die rechtstreeks worden geïntegreerd met bestaande bewakingsoplossingen in uw datacenter.

De volgende tabel geeft een overzicht van de lijst met beschikbare opties.

| Onderwerp | Oplossing voor externe controle |
| -- | -- |
| Azure Stack-software. | [Azure Stack Management Pack voor Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios plug-in](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>Op basis van REST-API-aanroepen | 
| Fysieke servers (bmc's via IPMI) | OEM-hardware - management pack voor Operations Manager-leverancier<br>Door OEM geleverde leverancier hardwareoplossing<br>Hardwareleverancier Nagios-invoegtoepassingen | OEM-partner ondersteund bewakingsoplossing (opgenomen) | 
| Netwerkapparaten (SNMP) | Detectie van netwerkapparaten Operations Manager<br>Door OEM geleverde leverancier hardwareoplossing<br>Invoegtoepassing Nagios-switch |
| Statuscontrole voor tenant-abonnement | [System Center Management Pack voor Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Houd rekening met de volgende vereisten:
- De oplossing die u gebruikt moet worden zonder agent. U kunt van derden agents binnen Azure Stack-componenten niet installeren. 
- Als u gebruiken van System Center Operations Manager wilt, is Operations Manager 2012 R2 of Operations Manager 2016 vereist.

## <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen

Planning voor back-up en herstel na noodgevallen omvat het plannen voor zowel de onderliggende Azure Stack infrastructuur die als host fungeert voor IaaS virtuele machines en PaaS-services en voor de tenant-toepassingen en gegevens. U moet plannen voor deze afzonderlijk.

### <a name="protect-infrastructure-components"></a>Onderdelen van de infrastructuur beveiligen

U kunt [back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md) infrastructuuronderdelen tot een SMB-share die u opgeeft:

- U moet een externe SMB-bestandsshare op een bestaande Windows-bestandsserver of een apparaat van derden.
- U moet deze dezelfde share voor de back-up van netwerkswitches en de host van de levenscyclus van hardware. Uw hardwareleverancier OEM helpt richtlijnen te bieden voor back-up en herstel van deze onderdelen wanneer deze externe naar Azure-Stack. U bent verantwoordelijk voor het uitvoeren van de back-werkstromen op basis van de OEM-leverancier aanbeveling.

Als onherstelbaar gegevensverlies optreedt, kunt u de infrastructuur back-up seedbewerking implementatiegegevens, zoals implementatie ingangen en id's, service-accounts CA-basiscertificaat, federatieve resources (in niet-verbonden implementaties), plannen, aanbiedingen, abonnementen, quota, toewijzingen voor het beleid en de rol van RBAC en geheimen van de Sleutelkluis.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Tenant-toepassingen op IaaS virtuele machines beveiligen

Azure Stack maakt geen back-up van de tenant-toepassingen en gegevens. U moet plannen voor back-up en noodherstel recovery-beveiliging met een externe naar Azure Stack doel. Beveiliging van de tenant is een activiteit tenant op basis van beschikbare resources. Voor IaaS virtuele machines, kunnen tenants in de Gast technologieën gebruiken om mappen, toepassingsgegevens en systeemstatus te beveiligen. Als een enterprise- of service-provider kunt u echter bieden een oplossing back-up en herstel in hetzelfde datacenter of extern in een cloud.

Als u wilt back-up van Linux of Windows IaaS virtuele machines, moet u back-producten met toegang tot het gastbesturingssysteem bestand, map, de status van het besturingssysteem en toepassingsgegevens te beveiligen. U kunt Azure back-up, System Center Data Center Protection Manager of producten van derden ondersteund.

Als u wilt repliceren van gegevens naar een secundaire locatie en stroomlijnen de failover van de toepassing als er een ramp optreedt, kunt u Azure Site Recovery of ondersteunde producten van derden gebruiken. (Op de eerste release van geïntegreerde systemen Azure Site Recovery niet wordt ondersteund failback. Echter, u kunt bereiken failback via een handmatig proces.) Bovendien kunnen toepassingen die ondersteuning bieden voor systeemeigen replicatie (zoals Microsoft SQL Server) gegevens repliceren naar een andere locatie waar de toepassing wordt uitgevoerd.

> [!IMPORTANT]
> Op de eerste release van geïntegreerde systemen door ons ondersteund beveiligingstechnologieën die op de het gastniveau van de virtuele machine voor IaaS werkt. U kunt geen agents installeren op de onderliggende infrastructuurservers.

## <a name="learn-more"></a>Meer informatie

- Zie voor meer informatie over gebruiksvoorbeelden, kopen, partners en hardwareleveranciers OEM de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Voor informatie over de roadmap en geo-beschikbaarheid voor Azure-Stack geïntegreerde systemen, Zie het white paper: [Azure Stack: een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Volgende stappen
[Azure Stack verbinding implementatiemodellen](azure-stack-connection-models.md)