---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FedRAMP
description: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FedRAMP
services: security
documentationcenter: na
author: jomolesk
manager: barbkess
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: daa81f4ec14a0e022ea22948324478b8e9cc3ce3
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247355"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Azure-beveiliging en naleving blauwdruk: IaaS-webtoepassing voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov) US government-is een programma is waarmee een gestandaardiseerde benadering voor de beveiligingsbeoordeling, de autorisatie en voortdurende bewaking van cloudproducten en Services. Deze Azure-beveiliging en naleving blauwdruk Automation biedt richtlijnen voor de implementatie van een FedRAMP-compatibele infrastructuur als een geschikt is voor een eenvoudige webtoepassing voor de internetgerichte service (IaaS)-omgeving. Deze oplossing automatiseert de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur, aan te tonen manieren waarin klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving en fungeert als een basis voor klanten om te bouwen en Configureer hun eigen oplossingen op Azure. De oplossing implementeert een subset van de besturingselementen van de FedRAMP High-basislijn, op basis van SP NIST 800-53. Zie voor meer informatie over de vereisten van de FedRAMP- en deze oplossing de [naleving documentatie](#compliance-documentation).
> [!NOTE]
> Deze oplossing wordt geïmplementeerd naar Azure Government.

Deze Azure-beveiliging en naleving blauwdruk Automation automatisch wordt geïmplementeerd met een referentiearchitectuur IaaS web application met vooraf geconfigureerde beveiligingsmaatregelen aan klanten helpen de naleving van FedRAMP-vereisten. De oplossing bestaat uit een Azure Resource Manager-sjablonen en PowerShell-scripts die resource-implementatie en configuratie begeleiden.

Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving. Implementeren van een toepassing in deze omgeving zonder aanpassing is niet voldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP High. Houd rekening met het volgende:
- Deze architectuur biedt een basislijn om klanten Azure gebruiken op een manier die compatibel is met FedRAMP te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en beoordeling van naleving van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren afhankelijk van de aard van de uitvoering van elke klant.

Voor een snel overzicht van de werking van deze oplossing, bekijk deze [video](https://aka.ms/fedrampblueprintvideo) worden uitgelegd en gedemonstreerd van de implementatie.

Klik op [hier](https://aka.ms/fedrampblueprintrepo) voor implementatie-instructies.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing wordt geïmplementeerd voor een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Application Gateway en Load Balancer. Virtuele machines die zijn geïmplementeerd voor de web- en gegevenslagen zijn geconfigureerd in een Beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid. Virtuele machines zijn domein en Active Directory-groepsbeleid worden gebruikt om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Een bastionhost biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. **Azure wordt aanbevolen voor het configureren van een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van referentie-architectuur.**

![IaaS-webtoepassing voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-iaaswa-architecture.png?raw=true "IaaS-webtoepassing voor Architectuurdiagram van FedRAMP-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Details van de implementatiearchitectuur bevinden zich in de [architectuur](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) bastionhost (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-clusterknooppunt (SQL Server 2017 voor Windows Server 2016)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Beschikbaarheidssets
    - (1) active Directory-domeincontrollers
    - (1) SQL-clusterknooppunten
    - (1) web/IIS
- Azure Virtual Network
    - (1) /16 virtuele netwerken
    - (5) /24 subnetten
    - DNS-instellingen zijn ingesteld op beide domeincontrollers
- Azure Load Balancer
- Azure Application Gateway
    - (1) de WAF-toepassingsgateway ingeschakeld
        - Firewallmodus: preventie
        - regel is ingesteld: OWASP 3.0
        - listener: poort 443
- Azure Storage
    - (7) Geo-redundant storage-accounts
- Azure-Cloud-Witness
- Recovery Services-kluis
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Azure Monitor (Logboeken)

## <a name="deployment-architecture"></a>Implementatie-architectuur

De volgende sectie bevat de elementen van de ontwikkeling en implementatie.

**Bastionhost**: De bastionhost is de enkel ingangspunt die een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources biedt. De bastionomgeving van host NSG kunt verbindingen alleen op TCP-poort 3389 voor RDP. Klanten kunnen de bastionhost om te voldoen aan de organisatie system beperkingsvereisten voor verdere configureren.

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: Deze oplossing wordt geïmplementeerd resources in een architectuur met een afzonderlijke subnet, database-subnet, Active Directory-subnet en beheersubnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door regels voor netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten te beperken het verkeer tussen subnetten die alleen dat nodig is voor het systeem en de beheerfunctionaliteit.

Raadpleeg de configuratie voor [netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) met deze oplossing wordt geïmplementeerd. Klanten kunnen netwerkbeveiligingsgroepen configureren door het bewerken van het bestand hoger via [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciale netwerkbeveiligingsgroep (NSG):
- 1 NSG voor Application Gateway (LBNSG)
- 1 NSG voor bastionhost (MGTNSG)
- 1 NSG voor primaire en back-domeincontrollers (ADNSG)
- 1 NSG voor SQL-Servers (SQLNSG)
- 1 NSG voor weblaag (WEBNSG)

**Subnetten**: Elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt data-at-rest met behulp van verschillende versleuteling metingen.

**Azure Storage**: Om te voldoen aan vereisten voor de versleuteling van data-at-rest, het gebruik van alle opslagaccounts [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**: SQL Server is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), die voert realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.

Klanten kunnen ook de volgende metingen van de SQL Server-beveiliging configureren:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of appservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
-   [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) kan worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moet aanpassen, dynamische gegevensmaskering instellingen om te voldoen aan het schema van de database.

**Azure Disk Encryption**: Azure Disk Encryption wordt gebruikt om versleutelde Windows IaaS VM-schijven. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing is geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) is van Microsoft met meerdere tenants cloudgebaseerde directory service- en identiteit.
- Verificatie van een klant geïmplementeerde web-App kan worden uitgevoerd met behulp van Azure AD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure Role-based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) maakt nauwkeurig beheer gerichte toegang voor Azure. Abonnementstoegang is beperkt tot de beheerder van het abonnement en toegang tot bronnen worden beperkt op basis van gebruikersrol.
- Een geïmplementeerde IaaS Active Directory-exemplaar biedt identiteitsbeheer op besturingssysteemniveau voor geïmplementeerde virtuele machines van IaaS.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Azure Key Vault helpt bij het beheer van IaaS VM-schijfversleuteling sleutels en geheimen voor deze referentiearchitectuur.

**Patchbeheer**: Windows virtuele machines geïmplementeerd met deze Azure-beveiliging en naleving blauwdruk Automation zijn standaard automatisch updates ontvangen van Windows Update-Service geconfigureerd. Deze oplossing implementeert ook de Azure Automation-oplossing waarmee Update-implementaties kunnen worden gemaakt voor het implementeren van patches voor Windows-servers wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt realtime-beveiliging-functie die helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software, met configureerbare meldingen wanneer bekende schadelijke of ongewenste software wil installeren of uitvoeren op de beveiligde virtuele machines.

**Application Gateway**: De architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall (WAF) en de OWASP ruleset ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-modus)
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA. De oplossing wordt geïmplementeerd voor alle weblaag en de gegevens laag virtuele machines in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets zorgen ervoor dat de virtuele machines worden verdeeld over meerdere geïsoleerde hardwareclusters beschikbaarheid verbeteren. Daarnaast wordt deze oplossing implementeert de virtuele machines met SQL Server in een Beschikbaarheidsset als een [AlwaysOn-beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). De Always On availability group-functie voorziet in mogelijkheden voor hoge beschikbaarheid en herstel na noodgevallen.

**Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld.

**Cloud-Witness**: [Cloud-Witness](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een type failovercluster-quorumwitness in Windows Server 2016 die gebruikmaakt van Azure als arbitragepunt. De Cloud-Witness, net als elke andere quorumwitness een stem opgehaald en kunt deelnemen aan de quorumberekeningen, maar hierbij de standaard openbare Azure Blob Storage. Dit elimineert de noodzaak extra onderhoud van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Logboeken in Azure Monitor biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. De [logboeken van Azure Monitor](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) oplossing verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.

- **Activiteitenlogboeken:**  [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Logboeken met diagnostische gegevens:**  [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure-opslaglogboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken.
- **Logboek archiveren:**  Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen. Deze logboeken verbinden met Azure Monitor-logboeken voor verwerking, opslag en -dashboardrapporten.

Bovendien worden de volgende bewakingsoplossingen geïnstalleerd als onderdeel van deze architectuur. Houd er rekening mee dat het verantwoordelijkheid van de klant het configureren van deze oplossingen aan te passen aan FedRAMP beveiligingscontroles is:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): De Antimalware-oplossing rapporteert over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): De oplossing Azure Automation opgeslagen, wordt uitgevoerd en runbooks worden beheerd.
-   [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Het dashboard beveiliging en controle biedt een hoogwaardig inzicht in de beveiligingsstatus van resources, dankzij metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, bedreigingsinformatie en algemene Beveiligingsquery's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): De oplossing Update Management kunt Klantenbeheer van updates voor besturingssysteem, met inbegrip van de status van de beschikbare updates en het installatieproces van vereiste updates.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.
-   [Wijzigingen bijhouden](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing wijzigingen bijhouden kan klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, beveiliging en trends te identificeren doordat organisaties om te controleren, waarschuwingen maken en archiveren van gegevens, zoals het bijhouden van API-aanroepen in klanten Azure-resources.

## <a name="threat-model"></a>Risicomodel
Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/fedrampWAdfd) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![IaaS-webtoepassing voor FedRAMP risicomodel](images/fedramp-iaaswa-threat-model.png?raw=true "IaaS-webtoepassing voor risicomodel FedRAMP")

## <a name="compliance-documentation"></a>Naleving-documentatie

De [Azure-beveiliging en naleving blauwdruk - FedRAMP hoog klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP High. De matrix geeft aan of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en blauwdruk voor naleving - FedRAMP IaaS Web Application hoge besturingselement implementatie Matrix](https://aka.ms/blueprintwacim) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP High. De matrix bevat informatie waarop besturingselementen worden gedekt door de IaaS architectuur voor webtoepassingen, waaronder gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Deze Azure-beveiliging en naleving blauwdruk Automation bestaat uit JSON-configuratiebestanden en PowerShell-scripts die worden verwerkt door Azure Resource Manager API-service om resources binnen Azure te implementeren. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Deze oplossing wordt geïmplementeerd naar Azure Government.

#### <a name="quickstart"></a>Snelstartgids
1. Klonen of downloaden [dit](https://aka.ms/fedrampblueprintrepo) GitHub-opslagplaats naar uw lokale werkstation.

2. Voer de PowerShell-script voor vóór de implementatie: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Klik op de knop hieronder, meld u aan bij de Azure-portal, voer de vereiste parameters voor ARM-sjabloon en klikt u op **aankoop**.

    [![Implementeren in Azure](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur IaaS-webtoepassing. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kan klanten veilig 'tunnel' gegevens in een gecodeerde verbinding tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een andere verbinding nog veiliger optie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een rechtstreekse verbinding van de klant telecommunicatie-provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.  
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.  
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.  
- Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.  
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
