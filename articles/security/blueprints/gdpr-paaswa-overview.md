---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor GDPR
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor GDPR
services: security
author: jomolesk
ms.assetid: 229759a1-f984-4985-a3fd-3719a7d1c8ff
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c338fed118e330280824754277a2fc31a1eaa7ba
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor GDPR

## <a name="overview"></a>Overzicht
De algemene gegevens beveiliging regelgeving (GDPR) bevat veel vereisten over het verzamelen, opslaan en gebruiken van persoonlijke gegevens, inclusief hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aangepast aan de vereisten voor transparantie, detecteren en rapporteren persoonlijke gegevens inbreuk en train privacy personeel en andere werknemers. De GDPR personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen in organisaties die verzamelen, verwerkt of persoonlijke gegevens te analyseren. De GDPR legt nieuwe regels voor organisaties die goederen bieden en services naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar een organisatie zich bevinden.

Microsoft heeft Azure met toonaangevende beveiligingsmaatregelen en het privacybeleid ter bescherming van gegevens in de cloud, met inbegrip van de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR ontworpen. Microsoft [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft door te voeren op de vereisten van de processors.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een platform als een geschikt is voor een eenvoudige webtoepassing voor internetgerichte service (PaaS)-omgeving. Deze oplossing laat zien waarin klanten kunnen voldoen aan de specifieke vereisten voor de beveiliging en naleving van de GDPR manieren en fungeert als basis voor klanten om te bouwen en hun eigen oplossingen voor PaaS web toepassingen configureren in Azure. Klanten kunnen gebruikmaken van deze verwijzende architectuur en voer de Microsoft [vier stappen proces](https://aka.ms/gdprebook) in hun reis GDPR naleving:
1. Detecteren: Identificeer welke persoonsgegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonlijke gegevens wordt gebruikt en gebruikt.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt detecteren, voorkomen van en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en meldingen schenden.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een GDPR-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De webtoepassing wordt gehost in een geïsoleerde Azure App Service-omgeving, die een persoonlijke, toegewezen omgeving in een Azure-datacenter. De belasting van de omgeving hoeveelheid verkeer voor de webtoepassing over virtuele machines die worden beheerd door Azure. Deze architectuur tevens netwerkbeveiligingsgroepen, een Application Gateway Azure DNS en Load Balancer. Application Insights biedt bovendien realtime prestaties Toepassingsbeheer en analyses via Operations Management Suite (OMS). **Azure raadt aan om een VPN- of ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![PaaS Web Applicaiton voor GDPR verwijzing Architectuurdiagram](images/gdpr-paaswa-architecture.png?raw=true "PaaS Web Applicaiton voor GDPR reference architecture-diagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

- Azure Active Directory (AAD)
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) WAF Application Gateway is ingeschakeld
        - firewallmodus: voorkomen
        - Regelset: OWASP 3.0
        - listener: poort 443
- Azure Virtual Network
- netwerkbeveiligingsgroepen
- Azure DNS
- Azure Storage
- Operations Management Suite (OMS)
- Azure Monitor
- Application Insights
- Azure Security Center
- App Service-omgeving v2
- Azure Load Balancer
- Azure Web App
- Azure Resource Manager

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de implementatie en de implementatie-elementen.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) kunnen klanten de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten met een sjabloon voor implementatie en dat de sjabloon kan werken voor verschillende omgevingen zoals testen, Faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die waarmee klanten kunnen hun resources beheren na implementatie.

**App Service-omgeving v2**: de [Azure App Service-omgeving](https://docs.microsoft.com/en-us/azure/app-service/environment/intro) is een App Service-functie een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van App Service-toepassingen op een grote schaal.

ASEs zijn geïsoleerd om alleen één klant toepassingen uitvoeren en worden altijd geïmplementeerd in een virtueel netwerk. Klanten hebben fijnmazig controle over beide-toepassing voor binnenkomend en uitgaand netwerkverkeer en toepassingen kunnen verbinding maken snelle beveiligde verbindingen via virtuele netwerken met lokale bedrijfsbronnen.

Gebruik van ASEs voor deze architectuur zijn toegestaan voor de volgende besturingselementen/configuraties:

- Hosten in een beveiligde virtuele Azure-netwerk en netwerk-beveiligingsregels voor verbindingen
- As-omgeving geconfigureerd met een zelf-ondertekend certificaat van de ILB voor HTTPS-communicatie
- [Interne Load Balancing modus](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modus 3)
- Schakel [TLS 1.0](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Wijziging [TLS-codering](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [WAF – gegevens beperken](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Toestaan dat [Azure SQL Database-verkeer](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure-Web-App**: [Azure Web Apps](https://docs.microsoft.com/en-us/azure/app-service/) kunnen klanten ontwikkelen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder infrastructuurbeheer. Het biedt automatisch schalen en een hoge beschikbaarheid, ondersteuning voor zowel Windows als Linux en maakt automatische implementaties mogelijk vanuit GitHub, Visual Studio Team Services of een willekeurige Git-repo.

### <a name="virtual-network"></a>Virtual Network
De architectuur definieert een persoonlijke VNet met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die het verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer op een subnet of afzonderlijke VM-niveau te beveiligen. De volgende nsg's bestaan:
- 1 NSG voor Application Gateway
- 1 NSG voor App Service-omgeving
- 1 NSG voor Azure SQL Database

Elk van de nsg's een bepaalde poorten en protocollen open zodat de oplossing kunt veilig en correct werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Logboeken met diagnostische gegevens en gebeurtenissen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en opgeslagen in een opslagaccount
  - OMS Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

**Azure DNS**: de Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. [Azure DNS](https://docs.microsoft.com/en-us/azure/dns/dns-overview) is een hosting service voor DNS-domeinen waarmee naamomzetting met Azure-infrastructuur. Gebruikers kunnen DNS-records met de dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services beheren door het hosten van de domeinen in Azure. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) kunnen klanten hun toepassingen schalen en hoge beschikbaarheid voor services maken. Load Balancer ondersteunt zowel inkomende als uitgaande scenario's, en biedt lage latentie en hoge doorvoer en schaalt miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg
Azure versleutelt alle communicatie naar en van Azure-datacenters standaard. Alle transacties naar Azure Storage via de Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt de gegevens in rust via versleuteling, database, controle en andere maatregelen.

**Azure Storage**: om te voldoen aan de versleutelde gegevens op de rest-vereisten, alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption). Hierdoor kunnen beveiligen en bescherming van persoonlijke gegevens ter ondersteuning van de beveiliging van de organisatie verbintenissen en nalevingsvereisten gedefinieerd door de GDPR.

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor gegevensschijven. De oplossing integreert met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-instantie gebruikt de volgende veiligheidsmaatregelen voor database:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   Azure SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), uitvoert, wordt er realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om informatie op te beschermen rest. TDE biedt de zekerheid dat persoonlijke gegevens opgeslagen niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Always-gecodeerd kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
- [Uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kunnen worden gebruikt om te stoppen met het verwerken van de betrokkenen, zoals gebruikers worden aangepaste eigenschappen toevoegen aan de database-objecten en gegevens labelen als 'uit 'assortiment ter ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van die zijn gekoppeld persoonlijke gegevens.
- [Beveiliging](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers beleid om toegang te beperken voor het stoppen met het verwerken van gegevens definiëren.
- [SQL Database dynamische gegevens maskeren (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling gevoelige, persoonlijke gegevens wordt beperkt door het maskeren van de gegevens voor onbevoegde gebruikers of toepassingen. DDM kan automatisch detecteren van mogelijk gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor GDPR beveiliging, en voor het beperken van toegang zodat deze niet de database via toegang door onbevoegden wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheer van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers die toegang tot de Azure SQL Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database AAD voor verificatie van de toepassing met Azure SQL Database gebruikt. Zie voor meer informatie hoe [gevoelige gegevens in Azure SQL Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders voor het definiëren van verfijnde toegangsmachtigingen voor de hoeveelheid toegang verlenen dat gebruikers moeten uitvoeren van hun taken. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, kunnen beheerders toestaan om alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnement toegang is beperkt tot abonnementsbeheerder.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten Beperk het aantal gebruikers die toegang tot bepaalde gegevens zoals persoonlijke gegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, Beperk en bewaak bevoegde identiteiten en de toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just in time beheerderstoegang wanneer deze nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen Management** de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Sleutelkluis waarmee klanten kunnen persoonlijke gegevens en toegang tot deze gegevens te beveiligen:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van de nodig.
- Sleutelkluis-beleid worden met de minimaal vereiste machtigingen aan sleutels en geheimen gedefinieerd.
- Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum.
- Alle sleutels in de Sleutelkluis worden beschermd door speciale hardware security modules (HSM's). Het sleuteltype is een HSM beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten krijgen minimaal vereiste machtigingen met RBAC.
- Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) kunnen klanten verkeer bewaken, verzamelen van Logboeken en gegevensbronnen voor bedreigingen analyseren. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure services voor configuratie en serviceaanbevelingen om te helpen verbeteren beveiligingspostuur en persoonlijke gegevens te beschermen. Azure Security Center bevat een [dreiging intelligence-rapport](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) voor elk gedetecteerd threat om te helpen respons op incidenten teams onderzoeken en bedreigingen te herstellen.

**Toepassingsgateway** de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met Web Application Firewall (WAF) en de ruleset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Schakel [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF modus)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

OMS biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure Storage-logboeken Sleutelkluis controlelogboeken en Application Gateway toegang en firewall-Logboeken.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaartermijn is gebruiker configureerbare, up, 730 dagen om te voldoen aan vereisten voor de organisatie-specifieke bewaarperiode. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien zijn de volgende OMS-oplossingen opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert. In deze oplossing te runbooks verzamelen van Logboeken van Application Insights en Azure SQL Database.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.
-   [Het bijhouden van](https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

**Monitor voor Azure**
[Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) helpt gebruikers prestaties, beveiliging en trends voor doordat organisaties om te controleren, waarschuwingen maken en gegevens, zoals het bijhouden van de API-aanroepen te archiveren in klanten Azure-resources.

**Application Insights**
[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars op meerdere platforms. Application Insights afwijkingen detecteert en klanten kunnen gebruiken om te controleren van de live-webtoepassing. Dit omvat analytics krachtige hulpprogramma's waarmee klanten kunnen problemen vaststellen en om te begrijpen wat gebruikers daadwerkelijk met hun app doen. Het is bedoeld voor klanten die continu prestaties en bruikbaarheid verbeteren.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/gdprPaaSdfd) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS Web Applicaiton voor GDPR risicomodel](images/gdpr-paaswa-threat-model.png?raw=true "PaaS Web Applicaiton voor GDPR risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk – GDPR klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en de processortijd verantwoordelijkheden voor alle GDPR artikelen bevat. Houd er rekening mee dat voor de Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - GDPR PaaS Web Application-implementatie Matrix](https://aka.ms/gdprPaaSWeb) bevat informatie over welke GDPR artikelen worden verholpen door de PaaS web application-architectuur, met inbegrip van gedetailleerde beschrijvingen van hoe is de implementatie voldoet aan de vereisten van elk gedekte artikel.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN- en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd om veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze PaaS web application verwijzende architectuur. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een laag van beveiliging voor gegevens onderweg.

Door het implementeren van een beveiligde VPN-tunnel met Azure kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via het Internet en kan klanten veilig 'tunnel' gegevens in een versleutelde koppeling tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. De [IPsec-tunnelmodus](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt gebruikt bij deze optie als een mechanisme voor versleuteling.

Omdat het verkeer binnen de VPN-tunnel Internet met een site-naar-site VPN passeren, biedt Microsoft met een andere, zelfs veiliger verbindingsoptie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant telecommunicatie provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot het.

Aanbevolen procedures voor het implementeren van een beveiligde hybride netwerk die uitgebreider is dan een on-premises netwerk naar Azure [beschikbaar](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
