---
title: Azure-beveiliging en naleving blauwdruk - gegevensanalyse voor SP NIST 800-171
description: Azure-beveiliging en naleving blauwdruk - gegevensanalyse voor SP NIST 800-171
services: security
author: jomolesk
ms.assetid: ca75d2a8-4e20-489b-9a9f-3a61e74b032d
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 02db944e96c74d865026a17a3871dbad2835cf1d
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056071"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Azure-beveiliging en naleving blauwdruk - gegevensanalyse voor SP NIST 800-171

## <a name="overview"></a>Overzicht
[NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) bevat richtlijnen voor het beveiligen van de gecontroleerde niet-geclassificeerde informatie (CUI) die deel uitmaakt van nonfederal informatiesystemen en organisaties. SP NIST 800-171 stelt 14 families van de beveiligingsvereisten voor het beveiligen van de vertrouwelijkheid van CUI.

Deze Azure-beveiliging en naleving blauwdruk biedt hulp bij het implementeren van een data analytics-architectuur in Azure waarmee een subset van SP NIST 800-171-besturingselementen klanten. Deze oplossing toont manieren waarin klanten kunnen voldoen aan bepaalde beveiligings- en nalevingsvereisten. Het fungeert ook als basis voor klanten om te bouwen en configureren van hun eigen data analytics-oplossingen in Azure.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke wensen. Ze al dan niet mogen worden gebruikt als-is in een productieomgeving. Er is onvoldoende om volledig te voldoen aan de vereisten van SP NIST 800-171 implementeren van deze architectuur zonder aanpassingen. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die gebruikmaakt van deze architectuur. Vereisten kunnen verschillen op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing biedt een platform voor streaminganalyse waarop klanten hun eigen analysehulpmiddelen kunnen bouwen. De referentiearchitectuur bevat een overzicht van een algemene use case. Klanten kunnen deze gebruiken voor het invoeren van gegevens via invoer van grote hoeveelheden gegevens door de beheerder van de SQL-gegevens. Ook kan worden gebruikt voor het invoeren van gegevens door middel van operationele Gegevensupdates via een operationele gebruiker. Beide IORP gebruikmaken van Azure Functions voor het importeren van gegevens in Azure SQL Database. Azure Functions moeten worden geconfigureerd door de klant via de Azure-portal voor het afhandelen van de importtaken unieke aan de vereisten voor de analyse van de klant.

Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing maakt gebruik van Azure Machine Learning-services en SQL-Database snel door gegevens bladeren en snellere resultaten via slimmer modelleren van gegevens leveren. Machine Learning is bedoeld om de snelheid van de query verhogen door het detecteren van nieuwe relaties tussen gegevenssets. Gegevens wordt in eerste instantie wordt getraind door verschillende statistische functies. Maximaal zeven extra query kunnen toepassingen daarna worden gesynchroniseerd met de dezelfde tabellaire modellen om werkbelasting van query's te verspreiden en responstijden te verminderen. De klantserver zorgt voor het totaal van de query-pools in acht.

Voor geavanceerde analyse en rapportage, kan de SQL-Database worden geconfigureerd met columnstore-indexen. Machine Learning en SQL-Database kan omhoog of omlaag geschaald of volledig afgesloten in reactie op klanten te worden gebruikt. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice aangeraden het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

Nadat de gegevens worden geüpload naar de SQL-database en getraind door Machine Learning, wordt deze ontsloten door de operationele gebruiker en de beheerder van de SQL-gegevens met Power BI. Power BI gegevens intuïtief worden weergegeven en informatie verzamelt over meerdere gegevenssets te tekenen meer inzicht krijgen. Power BI heeft een hoge mate van aanpasbaarheid en eenvoudig worden geïntegreerd met SQL-Database. Klanten kunnen configureren voor het afhandelen van een breed scala aan scenario's die voor hun bedrijfsbehoeften vereist zijn.

De volledige oplossing is gebouwd op Azure-opslag, waarmee klanten vanuit de Azure portal configureren. Opslag worden alle gegevens met Storage Service Encryption voor het onderhouden van de vertrouwelijkheid van gegevens in rust versleuteld. Geografisch redundante opslag zorgt ervoor dat een negatieve gebeurtenis in het primaire datacenter van de klant niet tot verlies van gegevens leiden. Een tweede kopie wordt opgeslagen op een afzonderlijke locatie honderden van mijl weg.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory (Azure AD)-op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor het beheren van toegang tot geïmplementeerde resources. Deze bronnen omvatten klant sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt door Azure Security Center en Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken. Status van het bestandssysteem wordt weergegeven in één dashboard dat is eenvoudig te gebruiken.

SQL-Database is meestal beheerd via SQL Server Management Studio. Deze wordt uitgevoerd op een lokale machine is geconfigureerd voor toegang tot de SQL-database via een VPN of een Azure ExpressRoute-verbinding. *Het is raadzaam dat u een VPN of ExpressRoute-verbinding configureren voor beheer en de gegevens importeren in de resourcegroep*.

![Data Analytics voor SP NIST 800-171 verwijzing Architectuurdiagram](images/nist171-analytics-architecture.png "gegevensanalyse voor Architectuurdiagram van SP NIST 800-171-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Zie voor meer informatie de [architectuur](#deployment-architecture) sectie.

- Application Insights
- Azure Active Directory
- Azure Data Catalog
- Azure Disk Encryption
- Azure Event Grid
- Azure Functions
- Azure Key Vault
- Azure Log Analytics
- Azure Machine Learning
- Azure Monitor
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Virtual Network
    - (1) /16 netwerk
    - (2) /24 netwerken
    - (2) de netwerkbeveiligingsgroepen
- Power BI Dashboard

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure Event Grid**: met [Event Grid](https://docs.microsoft.com/azure/event-grid/overview), klanten kunnen eenvoudig toepassingen met op gebeurtenissen gebaseerde architecturen. Gebruikers selecteren de ze abonneren willen op Azure-resource. Ze geven vervolgens de gebeurtenis-handler of webhook een eindpunt voor het verzenden van de gebeurtenis. Klanten kunnen webhook-eindpunten beveiligen door queryparameters toevoegen aan de webhook-URL bij het maken van een gebeurtenisabonnement. Event Grid ondersteunt alleen HTTPS-eindpunten-webhook. Met Event Grid, kunnen klanten de controle het niveau van toegang krijgen tot verschillende gebruikers verschillende beheerbewerkingen doen. Gebruikers kunnen gebeurtenisabonnementen lijst, nieuwe te maken en genereren van sleutels. Event Grid maakt gebruik van Azure RBAC.

**Azure Functions**: [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) is een serverloze rekenservice die code op aanvraag wordt uitgevoerd. U hebt geen expliciet inrichten of beheren van infrastructuur. Met Azure Functions kunt u een script of een stukje code uitvoeren in reactie op diverse gebeurtenissen.

**Azure Machine Learning**: [Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) is een techniek van gegevenswetenschap die computers kunnen bestaande gegevens om toekomstig gedrag, resultaten en trends prognoses te gebruiken.

**Azure Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) vergemakkelijkt gegevensbronnen detecteren en te begrijpen door de gebruikers die de gegevens beheren. Algemene gegevensbronnen kunnen worden geregistreerd, gelabeld en gezocht naar gegevens. De gegevens blijven in de bestaande locatie, maar een kopie van de metagegevens wordt toegevoegd aan Data Catalog. Een verwijzing naar de locatie van de gegevens is opgenomen. De metagegevens worden geïndexeerd zodat elke gegevensbron gemakkelijk detecteren via zoekopdrachten. Indexering ook is het begrijpen om de gebruikers die de gegevensbron detecteren.

### <a name="virtual-network"></a>Virtueel netwerk
Deze referentiearchitectuur definieert een particulier virtueel netwerk met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) bevatten toegangsbeheerlijsten die verkeer binnen een virtueel netwerk toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor Active Directory
  - Een NSG voor de werkbelasting

Elk van de nsg's heeft specifieke poorten en protocollen open zodat de oplossing kan veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en vanuit Azure-datacenters versleutelt Azure standaard. Alle transacties naar de opslag via de Azure-portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: om te voldoen aan vereisten voor versleutelde gegevens in rust, alle [opslag](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Deze functie helpt beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten door SP NIST 800-171 gedefinieerd.

**Azure Disk Encryption**: [schijfversleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de SQL-Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [Active Directory-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om gegevens in rust te beveiligen. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen. Het biedt beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang.
-   [Versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Nadat versleuteling van gegevens is ingeschakeld, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels kunnen toegang tot gegevens van de tekst zonder opmaak.
- [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens maskeren van de gegevens naar de replicagegevens gebruikers of toepassingen. Het kan automatisch detecteren van potentieel gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dynamische gegevensmaskering helpt bij de toegang beperken zodat gevoelige gegevens niet van de database via niet-gemachtigde toegang afsluiten. *Klanten zijn verantwoordelijk voor het aanpassen van de instellingen om te voldoen aan het schema van de database.*

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) is de Microsoft multitenant cloudgebaseerde directory en identity management-service. Alle gebruikers voor deze oplossing worden gemaakt in Azure AD en de gebruikers die toegang hebben tot de SQL-Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie over het [toepassingen integreren met Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Azure AD de versleuteling van de kolom database ook gebruikt voor verificatie van de toepassing met SQL-Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) door beheerders kan worden gebruikt om te voorzien in specifieke toegangsmachtigingen definiëren. Met, kunnen ze alleen de mate van toegang verlenen dat gebruikers moeten hun werk. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) door klanten kan worden gebruikt om het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens, zoals gegevens hebben. Beheerders kunnen Azure AD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed hebben op de identiteiten in een organisatie. Automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's configureren Ook onderzoekt het probleem verdachte incidenten passende actie op te lossen moet ondernemen.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Key Vault helpt Beveilig cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. De volgende mogelijkheden voor Key Vault zodat klanten gegevensbeveiliging:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is een hardware security module beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten zijn minimaal vereiste machtigingen verleend met behulp van RBAC.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Azure Security Center**: met [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Security Center heeft ook toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

 Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om te waarschuwen klanten van potentiële aanvallen die zijn gericht hun omgevingen. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) die worden geactiveerd wanneer een bedreiging of verdachte activiteit plaatsvindt. Klanten kunnen gebruikmaken van [aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

 Security Center biedt beveiligingswaarschuwingen en incidenten. Security Center kunt u eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde. Incident response teams kunnen de rapporten gebruiken wanneer ze onderzoeken en bedreigingen verhelpen.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, opslaglogboeken, auditlogboeken voor Key Vault en Azure Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. Gebruikers kunnen de bewaartermijn maximaal configureren tot 730 dagen, om te voldoen aan hun specifieke wensen.

**Log Analytics**: Logboeken worden geconsolideerd [Log Analytics](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Nadat gegevens zijn verzameld, onderverdeeld in afzonderlijke tabellen voor elk gegevenstype in Operations Management Suite-werkruimten. Op deze manier kunnen alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Security Center kan worden geïntegreerd met Log Analytics. Klanten kunnen Log Analytics-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Log Analytics [beheeroplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory statuscontrole oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Het biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): statuscontrole van de SQL-oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Daardoor kunnen klanten met een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing status van de Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling. Deze rapporten ook op het aantal agents zijn niet meer reageert en het aantal agents dat operationele gegevens verzendt.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de Activity Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van de SQL-Database. Klanten kunnen gebruikmaken van de automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing voor het herkennen van wijzigingen in de omgeving.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, veiligheid en trends te identificeren. Organisaties kunnen deze gebruiken om te controleren, waarschuwingen maken en archiveren van gegevens. Ze kunnen ook API-aanroepen in hun Azure-resources bijhouden.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) is een uitbreidbare Application Performance Management-service voor webontwikkelaars op meerdere platforms. Het detecteert afwijkende prestaties en bevat krachtige analysehulpmiddelen. De hulpprogramma's helpen problemen vaststellen en klanten inzicht krijgen wat gebruikers doen met de app te helpen. Het is ontworpen om u te helpen gebruikers prestaties en bruikbaarheid continu te verbeteren.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/nist171-analytics-tm) of kunt u hier vinden. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van system wanneer deze wijzigingen aanbrengt.

![Data Analytics voor SP NIST 800-171 risicomodel](images/nist171-analytics-threat-model.png "gegevensanalyse voor SP NIST 800-171 risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 klant verantwoordelijkheid Matrix](https://aka.ms/nist171-crm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor SP NIST 800-171. Deze matrix bevat of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 Data Analytics besturingselement implementatie Matrix](https://aka.ms/nist171-analytics-cim) bevat informatie over welke SP NIST 800-171-besturingselementen worden verholpen door de data analytics-architectuur. Dit omvat gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur voor analyse van gegevens. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet. Klanten kunnen de verbinding met een veilig 'tunnel' gegevens in een gecodeerde verbinding tussen hun netwerk en Azure gebruiken. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel over Internet met een site-naar-site-VPN wordt verzonden, biedt Microsoft een andere optie voor nog veiliger verbinding. ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. ExpressRoute-verbindingen rechtstreeks verbinding maken met een klant telecommunicatie-provider. Als gevolg hiervan de gegevens niet via Internet worden verzonden en wordt niet blootgesteld aan het. Deze verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gewone verbindingen. 

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Extract-transform-load-proces
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in SQL-Database zonder de noodzaak voor een afzonderlijke extract-transform-load of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via T-SQL-query's. De Microsoft business intelligence en analyse-stack en hulpprogramma's van derden die compatibel met SQL Server zijn kunnen worden gebruikt met PolyBase.

### <a name="azure-ad-setup"></a>Installatie van de Azure AD
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot medewerkers die met de omgeving werken. On-premises Active Directory kunnen worden geïntegreerd met Azure AD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen ook de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) naar Azure AD koppelen. Om dit te doen, moet u de geïmplementeerde Active Directory-infrastructuur een subdomein van een Azure AD-forest.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
