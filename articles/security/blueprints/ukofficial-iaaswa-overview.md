---
title: Azure-beveiliging en naleving blauwdruk - webtoepassing IaaS met drie lagen voor UK-OFFICIAL
description: Azure-beveiliging en naleving blauwdruk - webtoepassing IaaS met drie lagen voor UK-OFFICIAL
services: security
author: jomolesk
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.topic: article
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: a7461f6160c4c848106b16b1a9eaacb96ddf7499
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55699139"
---
# <a name="azure-security-and-compliance-blueprint---three-tier-iaas-web-application-for-uk-official"></a>Azure-beveiliging en naleving blauwdruk - webtoepassing IaaS met drie lagen voor UK-OFFICIAL

## <a name="overview"></a>Overzicht

 Dit artikel bevat richtlijnen en automation-scripts voor het leveren van een architectuur geschikt is voor het verwerken van veel werkbelastingen die zijn geclassificeerd als officiële in het Verenigd Koninkrijk webtoepassingen met drie lagen op basis van Microsoft Azure.

 Met behulp van een infrastructuur als Code benadering, de set [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) sjablonen implementeren een omgeving die wordt uitgelijnd op het Verenigd Koninkrijk nationale Cyber beveiliging centrum (NCSC) 14 [Cloud Security Principles](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) en het Center for Internet Security (CIS) [kritieke beveiligingscontroles](https://www.cisecurity.org/critical-controls.cfm).

 De NCSC raden aan hun Cloud Security Principles door klanten worden gebruikt om te evalueren van de eigenschappen voor de beveiliging van de service en voor meer informatie over de verdeling van de verantwoordelijkheid tussen de klant en de leverancier. Bevat informatie voor elk van deze principes om te begrijpen van de splitsing van verantwoordelijkheden.

 Deze architectuur en de bijbehorende Azure Resource Manager-sjablonen worden ondersteund door de Microsoft-whitepaper [14 Cloud beveiligingsmaatregelen voor het Verenigd Koninkrijk in de cloud met behulp van Microsoft Azure](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Dit document catalogi hoe Azure-services zijn afgestemd op het Verenigd Koninkrijk NCSC 14 Cloud beveiligingsprincipes, waardoor u organisaties fast track hun mogelijkheden om te voldoen aan hun verplichtingen met behulp van cloud-gebaseerde services wereldwijd en in het Verenigd Koninkrijk op de Microsoft Azure cloud.

 Deze sjabloon implementeert de infrastructuur voor de werkbelasting. Toepassingscode en de ondersteunende bedrijfslaag en software van gegevens van de laag moeten worden geïnstalleerd en geconfigureerd. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukwebappblueprintrepo).

 Als u niet een Azure-abonnement hebt en u kunt zich aanmelden, snel en eenvoudig - [aan de slag met Azure](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

 De Azure-sjablonen bieden een toepassingsarchitectuur webtoepassingen met drie lagen in een Azure-cloud-omgeving die ondersteuning biedt voor UK-OFFICIAL werkbelastingen. De architectuur biedt een beveiligde hybride omgeving die uitgebreider is dan een on-premises netwerk met Azure zodat op basis van web-workloads veilig worden geopend door zakelijke gebruikers of via internet.

![IaaS-webtoepassing voor drie lagen voor UK-OFFICIAL verwijzing Architectuurdiagram](images/ukofficial-iaaswa-architecture.png?raw=true "IaaS-webtoepassing voor drie lagen voor UK-OFFICIAL Architectuurdiagram van verwijzing")

 Deze oplossing maakt gebruik van de volgende Azure-services. Details van de implementatiearchitectuur bevinden zich in de [architectuur](#deployment-architecture) sectie.

(1) /16 virtueel netwerk - operationele VNet
- (3) /24 subnetten - 3-laagse (Web, Biz, gegevens)
- (1) /27 subnet - wordt toegevoegd
- (1) /27 subnet - Gateway-Subnet
- (1) /29 subnet - Subnet voor Application Gateway
- Wordt standaard (door Azure geleverd) DNS
- Ingeschakeld om het beheer van VNet-peering
- Netwerkbeveiligingsgroep (NSG) voor het beheren van netwerkverkeer

(1) /24 virtueel netwerk - Management-VNet
- (1) /27 subnet
- Maakt gebruik van DNS voor (2) toegevoegd en (1) Azure DNS-vermeldingen
- Ingeschakeld om operationele VNet-peering
- Netwerkbeveiligingsgroep (NSG) voor het beheren van netwerkverkeer

(1) application Gateway
- WAF - ingeschakeld
- Modus van de WAF - preventie
- regel is ingesteld: OWASP 3.0
- HTTP-Listener op poort 80
- Connectiviteit/verkeer wordt geregeld via NSG
- Openbare IP-adres-eindpunt gedefinieerd (Azure)

(1) VPN - Route gebaseerde Site-2-Site IPSec VPN-tunnel
- Openbare IP-adres-eindpunt gedefinieerd (Azure)
- Connectiviteit/verkeer wordt geregeld via NSG
- (1) gateway het lokale netwerk (on-premises eindpunt)
- (1) azure-netwerkgateway (Azure-eindpunt)

(9) virtuele Machines - alle virtuele machines worden geïmplementeerd met Azure IaaS-Antimalware DSC-instellingen

- (2) Active Directory Domain Services Domain Controllers (Windows Server 2012 R2)
  - (2) DNS-Server-rollen - 1 per VM
  - (2) de NIC is verbonden met de operationele VNet - 1 per VM
  - Beide zijn toegevoegd aan het domein dat is gedefinieerd in de sjabloon-
    - Domein gemaakt als onderdeel van de implementatie


- (1) Jumpbox (bastionhost) Management VM
  - 1 NIC op het beheer van VNet met openbare IP-adres
    - NSG wordt gebruikt voor het beperken van verkeer (in/uit) naar specifieke bronnen
  - Niet-domein


- (2) VM's web-laag
  - (2) serverfuncties de IIS - 1 per VM
  - (2) de NIC is verbonden met de operationele VNet - 1 per VM
  - Niet-domein


- (2) biz laag virtuele machines
  - (2) de NIC is verbonden met de operationele VNet - 1 per VM
  - Niet-domein


- (2) gegevens laag virtuele machines
  - (2) de NIC is verbonden met de operationele VNet - 1 per VM
  - Niet-domein

Beschikbaarheidssets
- (1) active Directory-domeincontroller VM ingesteld - 2 virtuele machines
- (1) weblaag VM ingesteld - 2 virtuele machines
- (1) VM biz laag ingesteld - 2 virtuele machines
- (1) VM gegevens in laag ingesteld - 2 virtuele machines

Load Balancer
- (1) web laag Load Balancer
- (1) biz laag Load Balancer
- (1) gegevens laag Load Balancer

Storage
- (14) het totaal Storage-Accounts
  - Active Directory Domain Controller Availability Set
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnostische lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset wordt toegevoegd
  - Virtuele machine Jumpbox
    - (1) primaire lokaal redundante opslag (LRS)-account voor de Jumpbox-VM
    - (1) diagnostische lokaal redundante opslag (LRS)-account voor de Jumpbox-VM
  - Web-laag virtuele machines
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnostische lokaal redundante opslag (LRS)-account voor de Web-laag-Beschikbaarheidsset
  - Biz laag virtuele machines
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnostische lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset voor laag van Biz
  - Gegevens laag virtuele machines
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnostische lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset voor laag van gegevens

### <a name="deployment-architecture"></a>Implementatie-architectuur:

**On-Premises Network**: Een lokaal privénetwerk dat in een organisatie geïmplementeerd.

**Productie VNet**: De productie [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) (Virtueelnetwerk) als host fungeert voor de toepassing en andere operationele resources die worden uitgevoerd in Azure. Elk VNet bevat mogelijk meerdere subnetten die worden gebruikt voor het isoleren en beheren van netwerkverkeer.

**Web-laag**: Binnenkomende HTTP-aanvragen worden verwerkt. Antwoorden worden geretourneerd via deze laag.

**Bedrijfslaag**: Implementeert zakelijke processen en andere functionele logica voor het systeem.

**Database-laag**: Permanente gegevensopslag, met behulp van [SQL Server Always On Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) voor hoge beschikbaarheid. Klanten kunnen gebruiken [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) als een alternatief PaaS.

**Gateway**: De [VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) biedt connectiviteit tussen de routers in het on-premises netwerk en de productie-VNet.

**Internet-Gateway en openbare IP-adres**: Toepassingsservices voor gebruikers via het internet wordt aangegeven dat de internet-gateway. Verkeer toegang tot deze services is beveiligd met een [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) Layer 7-Routering en load balancing mogelijkheden met web application firewall (WAF) beveiliging bieden.

**Beheer van VNet**: Dit [VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) bevat resources die beheer- en controlemogelijkheden voor de werkbelasting die in de productie VNet implementeren.

**Jumpbox**: Ook wel een [bastionhost](https://en.wikipedia.org/wiki/Bastion_host), dit is een beveiligde virtuele machine op het netwerk die beheerders gebruiken voor verbinding met virtuele machines in de productie-VNet. De jumpbox heeft een netwerkbeveiligingsgroep die alleen extern verkeer vanaf openbare IP-adressen op een lijst met veilige adressen toelaat. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de oorzaak van het verkeer worden gedefinieerd in de NSG. Beheer van productieresources is via RDP met behulp van een beveiligde Jumpbox-VM.

**Gebruiker gedefinieerde Routes**: [Gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) worden gebruikt voor het definiëren van de stroom van IP-verkeer binnen Azure VNets.

**Netwerk gekoppelde VNETs**: De productie en het beheer van vnet's zijn verbonden via [VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).
Deze VNets worden nog steeds beheerd als afzonderlijke resources, maar worden weergegeven als een voor alle verbindingsdoeleinden voor deze virtuele machines. Deze netwerken communiceren met elkaar via privé-IP-adressen. VNet-peering zijn afhankelijk van de VNets in dezelfde Azure-regio.

**Netwerkbeveiligingsgroepen**: [Nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten Access Control Lists toestaan of weigeren van verkeer binnen een VNet. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen.

**Active Directory Domain Services (AD DS)**: Deze architectuur biedt een toegewezen [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) implementatie.

**Logboekregistratie en controle**: [Azure-activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) van schermopnamen worden bewerkingen uitgevoerd voor de resources in uw abonnement, zoals die de bewerking heeft gestart wanneer de bewerking is uitgevoerd, de status van de bewerking en de waarden van andere eigenschappen die u kunnen helpen bij onderzoek de de bewerking. Azure-activiteitenlogboek is een Azure-platform-service waarmee alle acties op een abonnement wordt vastgelegd. Logboeken kunnen worden gearchiveerd of geëxporteerd indien nodig.

**Netwerk voor bewaking en waarschuwingen**: [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) is een platformservice biedt netwerk pakketopname, stroomlogboeken, hulpprogramma's voor topologie en diagnostische gegevens voor netwerk-verkeer binnen uw vnet's.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: Server-workloads worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Deze configuratie zorgt ervoor dat tijdens gepland of ongepland onderhoud ten minste één virtuele machine beschikbaar is en voldoet aan de 99,95% Azure SLA.

### <a name="logging-and-audit"></a>Logboekregistratie en controle

**Bewaking**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) is de platformservice die één bron biedt voor het bewaken van het activiteitenlogboek, metrische gegevens en diagnostische logboeken van al uw Azure-resources. Azure Monitor kan worden geconfigureerd om te visualiseren, query's uitvoeren, routeren, archiveren en reageren op de metrische gegevens en logboeken van resources in Azure. Het verdient aanbeveling dat resources gebaseerd toegangsbeheer wordt gebruikt voor het beveiligen van de audittrail om ervoor te zorgen dat gebruikers de mogelijkheid om te wijzigen van de logboeken niet hebben.

**Activiteitenlogboeken**: Configureer [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) te bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.

**Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door een resource. Deze logboeken kunnen Windows-gebeurtenislogboeken system, blob, tabel en wachtrijlogboeken bevatten.

**Firewall-logboeken**: Application Gateway biedt volledige diagnostische gegevens en Logboeken. Er zijn firewalllogboeken beschikbaar voor Application Gateway-resources waarvoor WAF is ingeschakeld.

**Logboek archiveren**: Gegevensopslag logboek kan worden geconfigureerd voor het schrijven naar een gecentraliseerd Azure storage-account voor archivering en een gedefinieerde periode. Logboeken kunnen worden verwerkt met behulp van Azure Log Analytics of met SIEM-systemen van derden.

### <a name="identity"></a>Identiteit

**Active Directory Domain Services**: Deze architectuur biedt de implementatie van een Active Directory Domain Services in Azure. Zie de volgende artikelen voor specifieke aanbevelingen over het implementeren van Active Directory in Azure:

[Active Directory Domain Services (AD DS) naar Azure uitbreiden](https://docs.microsoft.com/azure/guidance/guidance-identity-adds-extend-domain).

[Richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Active Directory-integratie**: Als alternatief voor een specifieke AD DS-architectuur klanten beter gebruik van [Azure Active Directory](https://docs.microsoft.com/azure/guidance/guidance-ra-identity) integratie of [Active Directory in Azure die is gekoppeld aan een on-premises forest](https://docs.microsoft.com/azure/guidance/guidance-ra-identity).

### <a name="security"></a>Beveiliging

**Beheer en beveiliging**: Deze blauwdruk kan beheerders verbinding maken met het beheer van VNet en Jumpbox met behulp van RDP van een vertrouwde bron. Het netwerkverkeer voor het beheer van VNet wordt beheerd met behulp van nsg's. Toegang tot poort 3389 is beperkt tot verkeer van een vertrouwde IP-adresbereik dat toegang heeft tot het subnet met daarin de Jumpbox.

Klanten kunnen ook kunt u overwegen een [beheermodel voor verbeterde beveiliging](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) voor het beveiligen van de omgeving bij het verbinden met het beheer van VNet en de Jumpbox. Is het nodig dat voor een betere beveiliging klanten gebruiken een [Privileged Access Workstation](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) en RDGateway-configuratie. Het gebruik van network virtual appliances en openbare en persoonlijke DMZ's bieden meer verbeteringen voor de beveiliging.

**Het netwerk beveiligen**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) worden aanbevolen voor elk subnet voor een tweede beschermingsniveau tegen inkomend verkeer dat via een onjuist geconfigureerde of uitgeschakelde-gateway. Voorbeeld: [Resource Manager-sjabloon voor het implementeren van een NSG](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Openbare eindpunten beveiligen**: Toepassingsservices voor gebruikers via het internet wordt aangegeven dat de internet-gateway. Verkeer toegang tot deze services is beveiligd met een [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), die voorziet in een Web Application Firewall en HTTPS-protocol.

**IP-adresbereiken**: De IP-adresbereiken in de architectuur zijn voorgestelde bereiken. Klanten wordt aangeraden rekening houden met hun eigen omgeving en het gebruik van de juiste bereiken.

**Hybride connectiviteit**: De op basis van cloudwerkbelastingen zijn verbonden met de on-premises datacentrum via IPSEC-VPN met behulp van de Azure VPN-Gateway. Klanten moeten ervoor zorgen dat ze een juiste VPN-Gateway gebruiken om te verbinden met Azure. Voorbeeld: [VPN-Gateway Resource Manager-sjabloon](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Klanten uitvoeren van grootschalige, bedrijfskritieke met big data-vereisten kunnen desgewenst kunt u overwegen een hybrid network architecture [ExpressRoute](https://docs.microsoft.com/azure/guidance/guidance-hybrid-network-expressroute) privénetwerkverbinding met Microsoft-cloudservices.

**Scheiding van taken**: Deze referentiearchitectuur worden gescheiden van de VNets voor bewerkingen voor gegevensbeheer en zakelijke activiteiten. Afzonderlijke VNets en subnetten toestaan van verkeer management, met inbegrip van verkeer inkomend en uitgaand verkeer beperkingen, met behulp van NSGs tussen netwerksegmenten te volgen [Microsoft cloud services en -netwerkbeveiliging](https://docs.microsoft.com/azure/best-practices-network-security) aanbevolen procedures.

**Resourcebeheer**: Azure-resources zoals VM's, VNets en load balancers worden beheerd door ze te groeperen in [Azure-resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Resource-toegangsbeheer op basis van rollen kunnen vervolgens worden toegewezen aan elke resourcegroep om toegang te beperken tot alleen geautoriseerde gebruikers.

**Toegang tot toegangsbeperkingen**: Gebruik [Role-Based Access Control](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (RBAC) voor het beheren van de resources in uw toepassing met [aangepaste rollen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) RBAC kan worden gebruikt voor het beperken van de bewerkingen die DevOps op elke laag kan uitvoeren. Bij het verlenen van machtigingen, gebruiken de [principe van minimale bevoegdheden](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1). Registreer alle beheergerelateerde bewerkingen en controleer regelmatig of eventuele configuratiewijzigingen inderdaad zo waren gepland.

**Toegang tot Internet**: Deze referentiearchitectuur maakt gebruik van [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) als de internetgerichte gateway en de load balancer. Sommige klanten kunnen ook overwegen om virtuele netwerkapparatuur van derden voor extra verificatielagen netwerken beveiliging als alternatief voor de [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction).

**Azure Security Center**: De [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een centraal overzicht van de beveiligingsstatus van resources in het abonnement en aanbevelingen gedaan om te voorkomen van verdachte resources. Het kan ook worden gebruikt om in te schakelen gedetailleerdere beleid. Beleid kan bijvoorbeeld worden toegepast op bepaalde resourcegroepen, waardoor de onderneming om aan te passen de houding risico. Het verdient aanbeveling dat klanten Azure Security Center in hun Azure-abonnement inschakelen.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>Documentatie voor NCSC Cloud Security Principles naleving

De kroon commerciële Service (een Bureau die geschikt is voor commerciële en aankopen activiteit verbeteren door de overheid) vernieuwd de classificatie van Microsoft binnen de regeling vallen enterprise cloudservices aan v6 G-Cloud, die betrekking hebben op het aanbod op het OFFICIAL-niveau. Details van Azure en G-Cloud kunnen u vinden in de [Azure Verenigd Koninkrijk G-Cloud security evaluatieoverzicht](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud).

Deze blauwdruk overeenstemt met de 14 cloud security-principes die worden beschreven in de NCSC [Cloud Security Principles](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) om te zorgen voor een omgeving die ondersteuning biedt voor werkbelastingen die zijn geclassificeerd als UK-OFFICIAL.

De [klant verantwoordelijkheid Matrix](https://aka.ms/ukofficial-crm) (Excel-werkmap) geeft een lijst van alle 14 cloud security principles, en de matrix voor elk principe (of principe subdeel) aangeeft of de implementatie van het principe de verantwoordelijkheid van is Microsoft, de klant, of gedeeld tussen de twee.

De [principe implementatie Matrix](https://aka.ms/ukofficial-iaaswa-pim) (Excel-werkmap) een lijst met alle 14 cloud security principles, en de matrix geeft, voor elk principe (of principe subdeel) die de verantwoordelijkheid van een klant in de klant wordt gebruikt De verantwoordelijkheden van Matrix (1) als de blauwdruk automation implementeert het principe en 2) een beschrijving van hoe de toepassing wordt uitgelijnd met de principal-vereisten.

De Cloud Security Alliance (CSA) gepubliceerd bovendien de Cloud Control Matrix ter ondersteuning van klanten in de evaluatie van cloudproviders en om te vragen die moeten worden beantwoord voordat u doorgaat naar de cloud services te identificeren. Antwoord, Microsoft Azure beantwoord de CSA Consensus evaluatie Initiative Questionnaire ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)), waarin wordt beschreven hoe Microsoft de principes voor de voorgestelde adressen.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Er zijn twee methoden die implementatiegebruikers gebruiken mogelijk voor het implementeren van deze blauwdruk automatisering. De eerste methode maakt gebruik van een PowerShell-script dat de tweede methode maakt gebruik van de Azure-portal om de referentiearchitectuur te implementeren. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukofficial-iaaswa-repo).

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
