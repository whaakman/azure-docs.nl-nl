---
title: "Blauwdruk Azure Automation - webtoepassingen met drie lagen voor VK officiële"
description: "Azure blauwdruk Automation & -webtoepassingen met drie lagen voor VK officiële"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 5f5694367d9be2ae66c7303cfea063b7f4979307
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-blueprint-automation-three-tier-web-applications-for-uk-official"></a>Blauwdruk Azure Automation: Drie lagen webtoepassingen voor VK officiële

## <a name="overview"></a>Overzicht

 Dit artikel bevat richtlijnen en automatisering scripts voor het leveren van een geschikt is voor het verwerken van veel werkbelastingen die zijn geclassificeerd als officiële in het Verenigd Koninkrijk voor architectuur van Microsoft Azure drie lagen op het web gebaseerd.

 Met behulp van een infrastructuur als Code benadert, de reeks [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) sjablonen (ARM) implementeert een omgeving die wordt uitgelijnd naar het Verenigd Koninkrijk nationale Cyberbeveiliging beveiliging centrum (NCSC) 14 [Cloud beveiligingsprincipes](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles)en Center voor de beveiliging van Internet (CIS) [kritieke beveiligingsmechanismen](https://www.cisecurity.org/critical-controls.cfm).

 De NCSC raden aan hun Cloud-beveiligingsprincipes door klanten worden gebruikt om te evalueren van de eigenschappen voor de beveiliging van de service, en voor meer informatie over de deling van verantwoordelijkheid tussen de klant en de leverancier. We bieden informatie met betrekking tot elk van deze principes om te begrijpen van de splitsing van verantwoordelijkheden.

 Deze architectuur en de bijbehorende ARM-sjablonen worden ondersteund door de whitepaper Microsoft [Azure blauwdruk voor de overheid VK](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Dit artikel catalogi hoe Azure-services zijn afgestemd op de VK NCSC 14 Cloud beveiligings-principals, waardoor u organisaties voor fast track-hun mogelijkheid om te voldoen aan hun verplichtingen met behulp van cloud-gebaseerde services globaal en in het Verenigd Koninkrijk in de Microsoft Azure cloud.

 Deze sjabloon wordt geïmplementeerd voor de infrastructuur voor de werkbelasting. Toepassingscode en de ondersteunende bedrijfslaag en software van de gegevens van laag moeten worden geïnstalleerd en geconfigureerd. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukwebappblueprintrepo).

 Als u geen Azure-abonnement hebt en u snel en eenvoudig aanmelden kunt - [aan de slag met Azure](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen

 De Azure-sjablonen bieden een toepassingsarchitectuur web drie lagen in een Azure-cloud-omgeving die ondersteuning biedt voor VK officiële werkbelastingen. De architectuur biedt een veilige hybride omgeving die uitgebreider is dan een on-premises netwerk Azure toestaan op het web gebaseerd werkbelastingen veilig worden geopend door zakelijke gebruikers of via internet.

![alternatieve tekst](images/diagram.png?raw=true "Azure VK officiële drie lagen architectuur")

 Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

(((1) /16 virtual Network - operationele VNet
- (3) /24 subnetten - 3-laagse (Web, Biz, gegevens)
- (1) /27 subnet - wordt toegevoegd
- (((1) /27 subnet - Gateway-Subnet
- (((1) slechts/29-subnet - Subnet toepassingsgateway
- Gebruikt standaard (Azure) DNS-
- Ingeschakeld om het Management-VNet-peering
- Netwerkbeveiligingsgroep (NSG) voor het beheren van netwerkverkeer

(((1) /24 virtual Network - Management-VNet
- (((1) /27 subnet
- Gebruikt (2) toegevoegd DNS en Azure DNS-vermeldingen (1)
- Ingeschakelde operationele vnet-peering
- Netwerkbeveiligingsgroep (NSG) voor het beheren van netwerkverkeer

(1) de toepassingsgateway
- WAF - ingeschakeld
- WAF modus - voorkomen
- Regelset: OWASP 3.0
- HTTP-Listener op poort 80
- Connectiviteit/verkeer wordt geregeld via het NSG
- Openbare IP-adres eindpunt gedefinieerd (Azure)

(1) VPN - Route gebaseerde Site-2-Site IPSec VPN-tunnel
- Openbare IP-adres eindpunt gedefinieerd (Azure)
- Connectiviteit/verkeer wordt geregeld via het NSG
- (1) de lokale netwerkgateway (lokaal eindpunt)
- (1) azure-netwerkgateway (Azure-eindpunt)

(9) virtuele Machines - alle virtuele machines zijn geïmplementeerd met Azure IaaS Antimalware DSC-instellingen

- (2) active Directory Domain Services-domeincontrollers (Windows Server 2012 R2)
  - (2) DNS-serverfuncties - 1 per VM
  - (2) de NIC die is verbonden met de operationele VNet - 1 per VM
  - Beide zijn met het domein is gekoppeld aan het domein dat is gedefinieerd in de sjabloon
    - Domein gemaakt als onderdeel van de implementatie


- (1) Jumpbox (Bastion Host) Management VM
  - 1 NIC op het VNet Management met openbare IP-adres
    - NSG wordt gebruikt voor het beperken van verkeer (in/out) naar specifieke bronnen
  - Niet-domein


- (2) virtuele machines web-laag
  - (2) serverfuncties de IIS - 1 per VM
  - (2) de NIC die is verbonden met de operationele VNet - 1 per VM
  - Niet-domein


- (2) biz laag virtuele machines
  - (2) de NIC die is verbonden met de operationele VNet - 1 per VM
  - Niet-domein


- (2) gegevens laag virtuele machines
  - (2) de NIC die is verbonden met de operationele VNet - 1 per VM
  - Niet-domein

Beschikbaarheidssets
- (1) active Directory-domeincontroller VM ingesteld - 2 virtuele machines
- (1) weblaag VM ingesteld - 2 virtuele machines
- (1) biz laag VM ingesteld - 2 virtuele machines
- (1) gegevenslaag VM ingesteld - 2 virtuele machines

Load Balancer
- (1) web laag Load Balancer
- (1) biz laag Load Balancer
- (1) gegevens laag Load Balancer

Storage
- (14) totaal Storage-Accounts
  - Active Directory Domain Controller Beschikbaarheidsset weergeven
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnose lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset wordt toegevoegd
  - Management Jumpbox VM
    - (1) primaire lokaal redundante opslag (LRS)-account voor de Jumpbox VM
    - (1) diagnose lokaal redundante opslag (LRS)-account voor de Jumpbox VM
  - Web-laag virtuele machines
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnose lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset voor laag van webservice
  - Virtuele machines Biz-laag
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnose lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset voor laag van Biz
  - Data-Tier virtuele machines
    - (2) primaire lokaal redundante opslag (LRS)-accounts: 1 voor elke virtuele machine  
    - (1) diagnose lokaal redundante opslag (LRS)-account voor de Beschikbaarheidsset voor laag van gegevens

### <a name="deployment-architecture"></a>Architectuur voor implementatie:

**On-Premises netwerk**: een particuliere LAN-netwerk dat is geïmplementeerd in een organisatie.

**Productie VNet**: de productie [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) (virtueel netwerk) als host fungeert voor de toepassing en andere operationele resources in Azure wordt uitgevoerd. Elke VNet kan verschillende subnetten die worden gebruikt bij het opsporen en beheren van netwerkverkeer bevatten.

**Web-laag**: binnenkomende HTTP-aanvragen worden verwerkt. Antwoorden worden geretourneerd door deze laag.

**Bedrijfslaag**: zakelijke processen en andere functionele logica voor het systeem worden geïmplementeerd.

**Laag van de database**: permanente gegevensopslag, met behulp van [SQL Server altijd op beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx) voor hoge beschikbaarheid. Klanten kunnen gebruiken [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) als een alternatief PaaS.

**Gateway**: de [VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) biedt connectiviteit tussen de routers in de on-premises netwerk en de productie VNet.

**Internet-Gateway en openbare IP-adres**: de internet-gateway beschrijft toepassingsservices voor gebruikers via het internet. Toegang tot deze services verkeer is beveiligd met een [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) laag 7 Routering en taakverdeling mogelijkheden met web application firewall (WAF) beveiliging bieden.

**Management VNet**: dit [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overviewcontains) bevat resources die beheer en controle van de mogelijkheden voor de werkbelastingen die worden uitgevoerd in de productie VNet te implementeren.

**Jumpbox**: ook wel een [bastion host](https://en.wikipedia.org/wiki/Bastion_host), dit is een beveiligde virtuele machine op het netwerk die beheerders gebruiken voor verbinding met virtuele machines in de productie VNet. De jumpbox heeft een NSG waarmee het externe verkeer alleen vanaf openbare IP-adressen op een veilige lijst. De bron van het verkeer moet worden gedefinieerd in de NSG zodat extern bureaublad (RDP)-verkeer. Beheer van productiebronnen is via RDP met behulp van een beveiligde Jumpbox VM.

**Gebruiker gedefinieerde Routes**: [de gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) worden gebruikt voor het definiëren van de stroom van IP-verkeer binnen Azure VNets.

**Netwerk brengen VNETs**: de productie- en Management VNets zijn verbonden via [VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).
Deze VNets nog steeds als afzonderlijke resources worden beheerd, maar als een voor alle verbindingen voor deze virtuele machines worden weergegeven. Deze netwerken communiceren met elkaar via privé IP-adressen. VNet-peering is onderworpen aan de VNets dat deze zich in hetzelfde Azure-gebied.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) Access Control Lists toestaan of weigeren van verkeer binnen een VNet bevatten. Nsg's kunnen worden gebruikt om verkeer op een subnet of afzonderlijke VM-niveau te beveiligen.

**Active Directory Domain Services (AD DS)**: deze architectuur biedt een speciaal [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) implementatie.

**Logboekregistratie en controle**: [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) worden vastgelegd in de resources in uw abonnement zoals wie de bewerking gestart wanneer de bewerking is opgetreden, de status van de bewerking en de waarden van uitgevoerde bewerkingen andere eigenschappen die u kunnen helpen onderzoek naar het opnieuw. Azure Activity Log is een Azure-platform-service waarmee alle acties voor een abonnement wordt vastgelegd. Logboeken worden gearchiveerd of geëxporteerd indien nodig.

**Bewaking en waarschuwen**: [Azure-netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) is een platformservice biedt pakketopname netwerk, stroom logboekregistratie, topologie hulpprogramma's en diagnostische gegevens voor netwerk traffics binnen uw vnet's.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: Server-werkbelastingen worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Deze configuratie zorgt ervoor dat tijdens gepland of ongepland onderhoud ten minste één virtuele machine beschikbaar is en voldoet aan de 99,95% Azure SLA.

### <a name="logging-and-audit"></a>Logboekregistratie en controle

**Bewaking**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) de platform-service die voorziet in één bron voor het bewaken van het activiteitenlogboek, meetgegevens en diagnostische logboeken van alle Azure-resources. Azure Monitor kan worden geconfigureerd om te visualiseren, te vragen, te routeren, archiveren en reageren op de metrische gegevens en de logboeken die afkomstig zijn van bronnen in Azure. Het verdient aanbeveling dat Resource gebaseerd toegangsbeheer wordt gebruikt voor het beveiligen van de audittrail om ervoor te zorgen dat gebruikers de mogelijkheid om te wijzigen van de logboeken niet hebben.

**Activiteitenlogboeken**: Configureer [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) te bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.

**Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door een resource. Deze logboeken kunnen bevatten Windows-gebeurtenislogboeken system, blob, table en logboeken van de wachtrij.

**Firewall-logboeken**: Application Gateway biedt volledige diagnostische gegevens en -Logboeken. Er zijn firewalllogboeken beschikbaar voor Application Gateway-resources waarvoor WAF is ingeschakeld.

**Meld u archiveren**: logboek opslag van gegevens kan worden geconfigureerd voor het schrijven naar een gecentraliseerde Azure storage-account voor archivering en een gedefinieerde bewaarperiode. Logboeken kunnen worden verwerkt met behulp van Azure-logboekanalyse of door de SIEM-systemen van derden.

### <a name="identity"></a>Identiteit

**Active Directory Domain Services**: deze architectuur biedt de implementatie van een Active Directory Domain Services in Azure. Zie de volgende artikelen voor specifieke aanbevelingen over het implementeren van Active Directory in Azure:

[Active Directory Domain Services (AD DS) naar Azure uitbreiden](https://docs.microsoft.com/azure/guidance/guidance-identity-adds-extend-domain).

[Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Active Directory-integratie**: als alternatief voor een toegewezen AD DS-architectuur klanten mogelijk wil gebruiken [Azure Active Directory](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-azure-active-directory) integratie of [Active Directory in Azure lid is van een on-premises forest](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-active-directory-in-azure-joined-to-an-on-premises-forest).

### <a name="security"></a>Beveiliging

**Beveiliging Management**: deze Azure blauwdruk kunnen beheerders verbinding maken met het beheer VNet en Jumpbox met RDP van een vertrouwde bron. Het netwerkverkeer voor de VNet wordt beheerd met behulp van NSGs management. Toegang tot poort 3389 is beperkt tot verkeer van een vertrouwde IP-adresbereik dat toegang heeft tot het subnet met de Jumpbox.

Klanten kunnen ook kunt u overwegen een [verbeterde beveiliging beheermodel](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) voor het beveiligen van de omgeving bij het verbinden met de management VNet en Jumpbox. Het wordt aangeraden voor een betere beveiliging klanten gebruiken een [bevoorrechte toegang werkstation](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) en RDGateway-configuratie. Het gebruik van virtuele netwerkapparaten en openbare en persoonlijke DMZ's bieden verdere verbeteringen in de beveiliging.

**Beveiligen van het netwerk**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) worden aanbevolen voor elk subnet naar een tweede niveau van bescherming tegen binnenkomend verkeer omzeilen van de gateway van een onjuist geconfigureerde of uitgeschakeld. Voorbeeld: [ARM-sjabloon voor het implementeren van een NSG](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Openbare eindpunten beveiligen**: de internet-gateway beschrijft toepassingsservices voor gebruikers via het internet. Toegang tot deze services verkeer is beveiligd met een [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), waarmee u een Web Application Firewall- en HTTPS-protocol-beheer.

**IP-adresbereiken**: het IP-adresbereiken in de architectuur zijn voorgestelde bereiken. Klanten wordt aangeraden rekening houden met hun eigen omgeving en de juiste bereiken gebruiken.

**Hybride verbindingen**: de werkbelastingen in de cloud zijn verbonden met het lokale datacentrum via IPSEC-VPN met behulp van de Azure VPN-Gateway. Klanten moeten ervoor zorgen dat ze een juiste VPN-Gateway gebruiken verbinding maken met Azure. Voorbeeld: [ARM-sjabloon voor VPN-Gateway](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Klanten met grootschalige bedrijfskritieke taken met big data-vereisten mogelijk wilt overwegen om een hybride netwerk architectuur [ExpressRoute](https://docs.microsoft.com/azure/guidance/guidance-hybrid-network-expressroute) particulier netwerkverbinding met Microsoft-cloudservices.

**Scheiding van zorgen**: deze verwijzende architectuur scheidt de VNets voor beheertaken uit te voeren en zakelijke activiteiten. Afzonderlijke vnet's en subnetten toestaan van beheer van verkeer, verkeer inkomende en uitgaande beperkingen, waaronder met behulp van NSGs tussen netwerksegmenten na [Microsoft cloud services en network security](https://docs.microsoft.com/azure/best-practices-network-security) aanbevolen procedures.

**Resourcebeheer**: Azure-resources, zoals virtuele machines, VNets en netwerktaakverdelers worden beheerd door ze te groeperen in [Azure-resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groupsresource). Resource toegangsbeheer op basis van rollen kunnen vervolgens worden toegewezen aan elke resourcegroep toegang te beperken tot alleen geautoriseerde gebruikers.

**Toegang tot de beperkingen voor toegangsbeheer**: Gebruik [toegangsbeheer op basis van rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) voor het beheren van de resources in uw toepassing met [aangepaste rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) RBAC kan worden gebruikt om de bewerkingen te beperken die DevOps kunt uitvoeren op elke laag. Bij het verlenen van machtigingen, gebruiken de [principe van minimale bevoegdheden](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1). Meld u alle beheerbewerkingen en uitvoeren van regelmatige controle om ervoor te zorgen dat configuratiewijzigingen zijn gepland.

**Toegang tot Internet**: maakt gebruik van deze verwijzende architectuur [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) als de gateway en de load balancer toegankelijk via internet. Sommige klanten kunnen ook overwegen om virtuele netwerkapparaten van derden voor extra verificatielagen networking beveiliging als alternatief voor het [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction).

**Azure Security Center**: de [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een centrale weergave van de beveiligingsstatus van resources in het abonnement en aanbevelingen die helpen te voorkomen dat resources waarmee is geknoeid. Het kan ook worden gebruikt om in te schakelen gedetailleerdere beleidsregels. Beleid kan bijvoorbeeld worden toegepast op bepaalde resourcegroepen, waardoor de onderneming dan de houding risico. Het verdient aanbeveling dat klanten Azure Security Center in de Azure-abonnement inschakelen.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC Cloud Security principes van de naleving documentatie

De domeinen commerciële-Service (een instantie werkt ter verbetering van de activiteit voor commerciële en aankopen door de overheid) vernieuwd de classificatie van Microsoft in bereik enterprise cloud-services voor v6 G-Cloud, die betrekking hebben op de offerings op het niveau van de officiële. Details van Azure en G-Cloud vindt u in de [Azure VK G-Cloud security assessment samenvatting](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud).

Deze oplossing VK officiële Azure blauwdruk wordt uitgelijnd naar de cloud 14 beveiligings-principals die zijn beschreven in de NCSC [beveiligingsprincipes Cloud](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) om te zorgen voor een omgeving die werkbelastingen die zijn geclassificeerd als VK officiële ondersteunt.

De [klant verantwoordelijkheid Matrix](https://aka.ms/blueprintuk-gcrm) (Excel-werkmap) ziet u alle 14 cloud-beveiligingsprincipes en de matrix geeft, voor elke principe (of principe subdeel), of de methode-implementatie de verantwoordelijkheid van is Microsoft, de klant of gedeeld tussen de twee.

De [principe implementatie Matrix](https://aka.ms/ukwebappblueprintpim) (Excel-werkmap) een lijst met alle 14 cloud-beveiligingsprincipes en de matrix geeft, voor elke principe (of principe subdeel) die de verantwoordelijkheid voor een klant in de klant wordt gebruikt De verantwoordelijkheden van Matrix (1) als de blauwdruk Azure Automation implementeert het principe en 2) een beschrijving van hoe de uitvoering wordt uitgelijnd met de vereisten van het principe. Deze inhoud is ook beschikbaar [hier](https://github.com/Azure/uk-official-three-tier-webapp/blob/master/principles-overview.md).

Bovendien Cloud Security Alliance (CSA) gepubliceerd voor de Cloud besturingselement Matrix ter ondersteuning van klanten in de evaluatie van cloudproviders en om te vragen die moeten worden beantwoord voordat u doorgaat met cloudservices te identificeren. In het antwoord, Microsoft Azure beantwoord de CSA Consensus Assessment initiatief vragenlijst ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)), waarin wordt beschreven hoe Microsoft de voorgestelde principes adressen.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Er zijn twee methoden die gebruikers van de implementatie gebruiken kunnen om deze oplossing Azure blauwdruk implementeren. De eerste methode maakt gebruik van een PowerShell-script dat de tweede methode maakt gebruik van Azure-Portal voor het implementeren van de referentiearchitectuur. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukwebappblueprintrepo).

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
