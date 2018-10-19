---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing Avg
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing Avg
services: security
author: jomolesk
ms.assetid: 229759a1-f984-4985-a3fd-3719a7d1c8ff
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c51ce44d1f6c2dcacaed09a490e46ad3af1ec9dc
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406684"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing Avg

## <a name="overview"></a>Overzicht
De General Data Protection Regulation (GDPR) bevat veel vereisten voor het verzamelen, opslaan en het gebruik van persoonlijke gegevens, met inbegrip van hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aan transparantie te voldoen, detecteren en rapporteren schendingen van persoonlijke gegevens, en train privacy personeel en andere werknemers. De AVG personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen voor organisaties die verzameld, verwerkt of persoonlijke gegevens te analyseren. De AVG legt een nieuwe regels voor organisaties die goederen en services naar mensen in de Europese Unie (EU) of die verzamelen en analyseren van gegevens van inwoners van de EU. De AVG is van toepassing, ongeacht waar een organisatie zich bevindt.

Microsoft heeft Azure ontworpen met toonaangevende beveiligingsmaatregelen en privacybeleidsregels ter bescherming van de gegevens in de cloud, waaronder de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR. Microsofts [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft aan de vereisten van processors die zijn doorgevoerd.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een platform als een geschikt is voor een eenvoudige webtoepassing voor de internetgerichte service (PaaS)-omgeving. Deze oplossing laat zien waar klanten kunnen voldoen aan de specifieke vereisten voor beveiliging en naleving van de AVG manieren en fungeert als een basis voor klanten om te bouwen en configureren van hun eigen PaaS-oplossingen voor web-toepassing in Azure. Klanten kunnen gebruikmaken van deze referentiearchitectuur en gaat u als volgt Microsofts [vier stappen](https://aka.ms/gdprebook) in hun op weg naar de GDPR-naleving:
1. Detecteren: Identificeer welke persoonlijke gegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonsgegevens wordt gebruikt en geopend.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt voorkomen, detecteren en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en in strijd is met meldingen.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basislijn om workloads naar Azure te implementeren op een manier die compatibel met GDPR klanten te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De web-App wordt gehost in een geïsoleerde Azure App Service Environment, dit een exclusieve privéomgeving in een Azure-datacenter is. De omgeving verdeelt verkeer voor de web-App voor virtuele machines die worden beheerd door Azure. Deze architectuur bevat ook netwerkbeveiligingsgroepen, een Application Gateway, Azure DNS en Load Balancer. Bovendien biedt Azure Monitor realtime analyses van de systeemstatus. **Azure wordt aanbevolen voor het configureren van een VPN of ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van referentie-architectuur.**

![PaaS Web Applicaiton voor AVG verwijzing Architectuurdiagram](images/gdpr-paaswa-architecture.png?raw=true "PaaS Web Applicaiton voor Architectuurdiagram van GDPR-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Details van de implementatiearchitectuur bevinden zich in de [architectuur](#deployment-architecture) sectie.

- Azure Active Directory (AAD)
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) de WAF-toepassingsgateway ingeschakeld
        - firewallmodus: preventie
        - Regelset: OWASP 3.0
        - listener: poort 443
- Azure Virtual Network
- netwerkbeveiligingsgroepen
- Azure DNS
- Azure Storage
- Azure Monitor
- Application Insights
- Azure Security Center
- App Service Environment v2
- Azure Load Balancer
- Azure Web App
- Azure Resource Manager

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunnen klanten voor het werken met de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten een sjabloon voor implementatie gebruiken en deze sjabloon kunt werken voor verschillende omgevingen, zoals testen, fasering en productie. Resource Manager biedt beveiliging, controle en tagfuncties die helpen bij klanten die hun resources beheren na de implementatie.

**App Service Environment v2**: de [Azure App Service Environment](https://docs.microsoft.com/azure/app-service/environment/intro) is een App Service-functie waarmee een volledig geïsoleerde en toegewezen omgeving voor App Service-toepassingen op een grote schaal veilig kunnen worden uitgevoerd.

As-omgevingen zijn geïsoleerd om één klant toepassingen alleen worden uitgevoerd en worden altijd geïmplementeerd in een virtueel netwerk. Klanten hebben meer controle over zowel toepassing binnenkomend en uitgaand netwerkverkeer en toepassingen zeer snelle, veilige verbindingen kunnen maken via virtuele netwerken tot on-premises bedrijfsresources.

Gebruik van de as-omgevingen voor deze architectuur zijn toegestaan voor de volgende besturingselementen/configuraties:

- Hosten in een beveiligde Virtueelnetwerk van Azure en netwerkbeveiligingsregels
- As-omgeving geconfigureerd met de zelf-ondertekend ILB-certificaat voor HTTPS-communicatie
- [Interne Load Balancing modus](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modus 3)
- Uitschakelen [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Wijziging [TLS-codering](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [WAF-gegevens beperken](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Toestaan dat [verkeer van de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure-Web-App**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) kunnen klanten bouwen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder het beheren van infrastructuur. Het biedt automatisch schalen en een hoge beschikbaarheid, ondersteuning voor zowel Windows als Linux en maakt automatische implementaties mogelijk vanuit GitHub, Azure DevOps of een willekeurige Git-repo.

### <a name="virtual-network"></a>Virtual Network
De architectuur van definieert een persoonlijke VNet met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
- 1 NSG voor Application Gateway
- 1 NSG voor App Service-omgeving
- 1 NSG voor Azure SQL Database

Elk van de nsg's zijn bepaalde poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

**Azure DNS**: de Domain Name System- of DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Azure-infrastructuur biedt. Door domeinen te hosten in Azure, kunnen gebruikers de DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services beheren. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kunnen klanten hun toepassingen schalen en hoge beschikbaarheid voor services. Load Balancer ondersteunt zowel binnenkomende als uitgaande-scenario's, en lage latentie, hoge doorvoer, en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en van Azure-datacenters versleutelt Azure standaard. Alle transacties met Azure Storage via Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt te beschermen en beveiligen van persoonlijke gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door de GDPR.

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   Azure SQL Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden ter bescherming van gegevens op over de rest. TDE biedt de zekerheid dat persoonlijke gegevens die zijn opgeslagen is niet onderhevig aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
- [Uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kunnen worden gebruikt om te stoppen met de verwerking van de betrokkenen, zoals gebruikers kunnen aangepaste eigenschappen toevoegen aan database-objecten en gegevens markeren als 'Discontinued' voor de ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van die zijn gekoppeld persoonlijke gegevens.
- [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers voor het definiëren van beleid om toegang te beperken tot gegevens te verwerken.
- [SQL Database Dynamic Data Masking (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige, persoonlijke gegevens door het maskeren van de gegevens naar niet-gemachtigde gebruikers of toepassingen. DDM kan automatisch detecteren van potentieel gevoelige gegevens en stel de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor de beveiliging van de AVG en voor het beperken van toegang, zodat deze niet de database via niet-geautoriseerde toegang wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) is van Microsoft met meerdere tenants cloudgebaseerde directory service- en identiteit. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers met toegang tot de Azure SQL Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van AAD om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten voor het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens, zoals persoonsgegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die betrekking hebben op van een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's en verdachte onderzoekt het probleem incidenten, onderneem gepaste actie op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen Management** maakt gebruik van de oplossing [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault zodat klanten bescherming van persoonlijke gegevens en toegang tot deze gegevens:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault worden beschermd door gespecialiseerde hardware security modules (HSM's). Het sleuteltype is een HSM beveiligd 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met RBAC worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kunnen klanten om verkeer te controleren, logboeken te verzamelen en analyseren van gegevensbronnen voor bedreigingen. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en persoonlijke gegevens beschermen. Azure Security Center bevat een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) voor elke bedreigingen gedetecteerde te helpen incident response teams onderzoeken en bedreigingen verhelpen.

**Application Gateway** de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met Web Application Firewall (WAF) en de OWASP ruleset ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-modus)
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure Monitor biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. Deze verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure Storage-Logboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslag en -dashboardrapporten.

Bovendien is de volgende bewakingsoplossingen, opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory statuscontrole oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de antimalwareoplossing rapporten over malware, bedreigingen en bescherming van de status.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van Application Insights en Azure SQL Database.
-   [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiliging en controle dashboard biedt een hoogwaardig inzicht in de beveiligingsstatus van resources, dankzij metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, bedreigingsinformatie en algemene Beveiligingsquery's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): statuscontrole van de SQL-oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): de updatebeheer-oplossing maakt Klantenbeheer van de beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het installatieproces van vereiste updates.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing status van de Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de Activity Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.
-   [Wijzigingen bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking): de functie Wijzigingen bijhouden kan klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, beveiliging en trends te identificeren doordat organisaties om te controleren, waarschuwingen maken en archiveren van gegevens, zoals het bijhouden van API-aanroepen in klanten Azure-resources.

**Application Insights**
[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars op meerdere platforms. Application Insights detecteert afwijkende prestaties en klanten deze kunnen gebruiken voor het bewaken van de live web-App. Het bevat krachtige analysehulpmiddelen om te helpen klanten problemen identificeren en te begrijpen wat gebruikers daadwerkelijk doen met de app. Het is ontworpen om u te helpen klanten prestaties en bruikbaarheid continu te verbeteren.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/gdprPaaSdfd) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS Web Applicaiton voor AVG risicomodel](images/gdpr-paaswa-threat-model.png?raw=true "PaaS Web Applicaiton voor risicomodel Avg")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk – AVG klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en processor verantwoordelijkheden voor alle AVG artikelen bevat. Houd er rekening mee dat voor Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - AVG PaaS Web Application implementatie Matrix](https://aka.ms/gdprPaaSWeb) bevat informatie over welke AVG artikelen worden verholpen door de PaaS architectuur voor webtoepassingen, waaronder gedetailleerde beschrijvingen van hoe is de implementatie voldoet aan de vereisten van elk artikel vallen.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur van PaaS web toepassing. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

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
