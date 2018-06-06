---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor FedRAMP
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor FedRAMP
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: 20aa842fb8168bc28a388c817f4e4eedbdd63ebd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727775"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Azure-beveiliging en naleving blauwdruk: PaaS-webtoepassing voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal risico en autorisatie Management-programma (FedRAMP)](https://www.fedramp.gov/) is een Verenigde Staten government wide programma waarmee een gestandaardiseerde aanpak voor beoordeling van de veiligheid, autorisatie en doorlopende bewaking voor cloud producten en services. Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het leveren van een Microsoft Azure-platform als een service (PaaS)-architectuur waarmee een subset van hoge FedRAMP besturingselementen implementeren. Deze oplossing biedt richtlijnen voor de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur manieren waarin klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving te demonstreren en fungeert als basis voor klanten bouw en hun eigen oplossingen configureren in Azure.

Deze referentiearchitectuur, gekoppelde besturingselement implementatiehandleidingen en threat modellen zijn bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Een toepassing in deze omgeving zonder aanpassingen implementeren is onvoldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP hoog. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een FedRAMP-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De webtoepassing wordt gehost in een geïsoleerde Azure App Service-omgeving, die een persoonlijke, toegewezen omgeving in een Azure-datacenter. De belasting van de omgeving hoeveelheid verkeer voor de webtoepassing over virtuele machines die worden beheerd door Azure. Deze architectuur tevens netwerkbeveiligingsgroepen, een Application Gateway Azure DNS en Load Balancer. Operations Management Suite biedt bovendien realtime analyses van systeemstatus en beveiliging. **Azure raadt aan om een VPN- of ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![PaaS-webtoepassing voor FedRAMP reference architecture-diagram](images/fedramp-paaswa-architecture.png?raw=true) 'PaaS-webtoepassing voor FedRAMP verwijzing Architectuurdiagram'

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

- Azure Active Directory
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) web Application Firewall
        - firewallmodus: voorkomen
        - Regelset: OWASP 3.0
        - listener: poort 443
- Azure Virtual Network
- Netwerkbeveiligingsgroepen
- Azure DNS
- Azure Storage
- Operations Management Suite
- Azure Monitor
- App Service-omgeving v2
- Azure Load Balancer
- Azure Web App
- Azure Resource Manager

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de implementatie en de implementatie-elementen.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunnen klanten de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten met een sjabloon voor implementatie en dat de sjabloon kan werken voor verschillende omgevingen zoals testen, Faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die waarmee klanten kunnen hun resources beheren na implementatie.

**App Service-omgeving v2**: de [Azure App Service omgeving (as-omgeving)](https://docs.microsoft.com/azure/app-service/environment/intro) is een App Service-functie een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van App Service-toepassingen op een grote schaal.

ASEs zijn geïsoleerd om alleen één klant toepassingen uitvoeren en worden altijd geïmplementeerd in een virtueel netwerk. Klanten hebben fijnmazig controle over beide-toepassing voor binnenkomend en uitgaand netwerkverkeer en toepassingen kunnen verbinding maken snelle beveiligde verbindingen via virtuele netwerken met lokale bedrijfsbronnen.

Gebruik van ASEs voor deze architectuur zijn toegestaan voor de volgende besturingselementen/configuraties:

- Hosten in een beveiligde virtuele Azure-netwerk en netwerk-beveiligingsregels voor verbindingen
- As-omgeving geconfigureerd met een zelf-ondertekend certificaat van de ILB voor HTTPS-communicatie
- [Interne Load Balancing-modus](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Schakel [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Wijziging [TLS-codering](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web Application Firewall – gegevens beperken](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Toestaan dat [Azure SQL Database-verkeer](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

De [richtlijnen en aanbevelingen](#guidance-and-recommendations) sectie bevat aanvullende informatie over ASEs.

**Azure-Web-App**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) kunnen klanten ontwikkelen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder infrastructuurbeheer. Het biedt automatisch schalen en een hoge beschikbaarheid, ondersteuning voor zowel Windows als Linux en maakt automatische implementaties mogelijk vanuit GitHub, Visual Studio Team Services of een willekeurige Git-repo.

### <a name="virtual-network"></a>Virtual Network
De architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) toegangsbeheerlijsten toestaan of weigeren van verkeer binnen een virtueel netwerk bevatten. Nsg's kunnen worden gebruikt om verkeer op een subnet of afzonderlijke VM-niveau te beveiligen. De volgende nsg's bestaan:
- 1 NSG voor Application Gateway
- 1 NSG voor App Service-omgeving
- 1 NSG voor Azure SQL Database

Elk van de nsg's een bepaalde poorten en protocollen open zodat de oplossing kunt veilig en correct werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Logboeken met diagnostische gegevens en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en opgeslagen in een opslagaccount
  - OMS Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

**Azure DNS**: de Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hosting service voor DNS-domeinen waarmee naamomzetting met Azure-infrastructuur. Gebruikers kunnen DNS-records met de dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services beheren door het hosten van de domeinen in Azure. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kunnen klanten hun toepassingen schalen en hoge beschikbaarheid voor services maken. Load Balancer ondersteunt zowel inkomende als uitgaande scenario's, en biedt lage latentie en hoge doorvoer en schaalt miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg
Azure versleutelt alle communicatie naar en van Azure-datacenters standaard. Alle transacties naar Azure Storage via de Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt de gegevens in rust via versleuteling, database, controle en andere maatregelen.

**Azure Storage**: om te voldoen aan de versleutelde gegevens op de rest-vereisten, alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor gegevensschijven. De oplossing integreert met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-instantie gebruikt de volgende veiligheidsmaatregelen voor database:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   Azure SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), uitvoert, wordt er realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om informatie op te beschermen rest.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Always-gecodeerd kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
- [Beveiliging](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers beleid om toegang te beperken voor het stoppen met het verwerken van gegevens definiëren.
- [SQL-Database dynamische-gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) kunnen worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moeten dynamische gegevensmaskering instellingen om te voldoen aan hun databaseschema aanpassen.

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers die toegang tot de Azure SQL Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure Active Directory om te verifiëren van de toepassing met Azure SQL Database. Zie voor meer informatie hoe [gevoelige gegevens in Azure SQL Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Op rollen gebaseerde toegangsbeheer van Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunt nauwkeurig gerichte toegangsbeheer voor Azure. Abonnement toegang is beperkt tot abonnementsbeheerder en toegang tot bronnen worden beperkt op basis van gebruikersrol.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten Beperk het aantal gebruikers die toegang tot bepaalde gegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, Beperk en bewaak bevoegde identiteiten en de toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just in time beheerderstoegang wanneer deze nodig is.
- [Azure Active Directory: Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen Management** de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Sleutelkluis waarmee klanten kunnen gegevens en toegang tot dergelijke gegevens beschermen:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van de nodig.
- Sleutelkluis-beleid worden met de minimaal vereiste machtigingen aan sleutels en geheimen gedefinieerd.
- Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum.
- Alle sleutels in de Sleutelkluis worden beschermd door speciale hardware security modules (HSM's). Het sleuteltype is een HSM beveiligde 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met behulp van toegangsbeheer op basis van rollen worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is.

**Toepassingsgateway** de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met Web Application Firewall en de ruleset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn:
- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Schakel [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle
Operations Management suite biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De Operations Management suite [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure Storage-logboeken Sleutelkluis controlelogboeken en Application Gateway toegang en firewall-Logboeken.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaartermijn is gebruiker configureerbare, up, 730 dagen om te voldoen aan vereisten voor de organisatie-specifieke bewaarperiode. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien zijn de volgende Operations Management suite-oplossingen opgenomen als onderdeel van deze architectuur:
-   [Active directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert. In deze oplossing te runbooks verzamelen van Logboeken van Application Insights en Azure SQL Database.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.
-   [Het bijhouden van](https://docs.microsoft.com/azure/automation/automation-change-tracking): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

**Monitor voor Azure**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers prestaties, beveiliging en trends voor doordat organisaties om te controleren, waarschuwingen maken en gegevens, zoals het bijhouden van de API-aanroepen te archiveren in klanten Azure-resources.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/fedrampPaaSWebAppDFD) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS-webtoepassing voor risicomodel FedRAMP](images/fedramp-paaswa-threat-model.png?raw=true "PaaS Web Applicaiton voor FedRAMP risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk - FedRAMP hoge klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix geeft aan of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - FedRAMP PaaS WebApp hoge besturingselement implementatie Matrix](https://aka.ms/fedrampPaaSWebAppCIM) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix bevat informatie waarop besturingselementen worden gedekt door de PaaS web application architectuur, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk gedekte besturingselement.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN- en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd om veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze PaaS web application verwijzende architectuur. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een laag van beveiliging voor gegevens onderweg.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een VPN-verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via het Internet en kan klanten veilig 'tunnel' gegevens in een versleutelde koppeling tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt gebruikt bij deze optie als een mechanisme voor versleuteling.

Omdat het verkeer binnen de VPN-tunnel Internet met een site-naar-site VPN passeren, biedt Microsoft met een andere, zelfs veiliger verbindingsoptie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant telecommunicatie provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot het.

Aanbevolen procedures voor het implementeren van een beveiligde hybride netwerk die uitgebreider is dan een on-premises netwerk naar Azure [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
