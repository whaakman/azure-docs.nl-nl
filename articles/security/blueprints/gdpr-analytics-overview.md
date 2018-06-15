---
title: Azure-beveiliging en naleving blauwdruk - analyse voor GDPR
description: Azure-beveiliging en naleving blauwdruk - analyse voor GDPR
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 9114dea1f95198b5ff8dc1bf759be5d1179885fd
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162181"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk: Analytics voor GDPR

## <a name="overview"></a>Overzicht
De algemene gegevens beveiliging regelgeving (GDPR) bevat veel vereisten over het verzamelen, opslaan en gebruiken van persoonlijke gegevens, inclusief hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aangepast aan de vereisten voor transparantie, detecteren en rapporteren persoonlijke gegevens inbreuk en train privacy personeel en andere werknemers. De GDPR personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen in organisaties die verzamelen, verwerkt of persoonlijke gegevens te analyseren. De GDPR legt nieuwe regels voor organisaties die goederen bieden en services naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar een organisatie zich bevinden.

Microsoft heeft Azure met toonaangevende beveiligingsmaatregelen en het privacybeleid ter bescherming van gegevens in de cloud, met inbegrip van de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR ontworpen. Microsoft [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft door te voeren op de vereisten van de processors.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een analytics-architectuur van de gegevens in Azure die bij de vereisten van de GDPR helpt. Deze oplossing laat zien manieren waarin klanten kunnen voldoen aan bepaalde beveiligings- en nalevingsvereisten en fungeert als basis voor klanten om te bouwen en configureren van hun eigen gegevens analytics-oplossingen in Azure. Klanten kunnen gebruikmaken van deze verwijzende architectuur en voer de Microsoft [vier stappen proces](https://aka.ms/gdprebook) in hun reis GDPR naleving:
1. Detecteren: Identificeer welke persoonsgegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonlijke gegevens wordt gebruikt en gebruikt.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt detecteren, voorkomen van en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en meldingen schenden.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een GDPR-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing biedt een analytics platform waarop klanten hun eigen analytics-hulpprogramma's kunnen maken. De referentiearchitectuur bevat een overzicht van een algemene gebruiksvoorbeeld waar klanten via bulkimport gegevens door de beheerder van SQL-gegevens of door middel van operationele Gegevensupdates via een gebruiker met operationele gegevens invoeren. Beide streams werk gebruikmaken van Azure Functions voor het importeren van gegevens in Azure SQL Database. Azure Functions moeten worden geconfigureerd door de klant via de Azure portal voor het afhandelen van de taken importeren uniek voor elke klant van eigen analytics vereisten.

Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing bevat echter Azure Machine Learning-services in combinatie met Azure SQL Database snel bladeren door de gegevens en snellere resultaten via slimmer modellering van klantgegevens leveren. Azure Machine Learning is een vorm van machine learning bedoeld query snelheden verbeteren door het detecteren van nieuwe relaties tussen gegevenssets. Nadat de gegevens is getraind via verschillende statistische functies, tot 7 extra query kunnen pools (totale met inbegrip van de klantserver 8) worden gesynchroniseerd met de dezelfde modellen in tabelvorm om verspreiden workload van query's en responstijden te verminderen.

Voor geavanceerde analyse en rapportage, worden Azure SQL-Databases geconfigureerd met columnstore-indexen. Zowel Azure Machine Learning en Azure SQL-Databases kunnen worden geschaald omhoog of omlaag of volledig afgesloten in reactie op gebruik van de klant. Alle SQL-verkeer is versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice adviseert Azure het gebruik van een vertrouwde certificeringsinstantie voor een betere beveiliging.

Wanneer gegevens worden geüpload naar de Azure SQL Database en getraind door Azure Machine Learning, is het verwerkte door de operationele gebruiker en de beheerder van de SQL-gegevens met Power BI. Power BI intuïtief gegevens worden weergegeven en informatie verzamelt over meerdere gegevenssets beter inzicht wordt getekend. De hoge mate van aanpassingsvermogen en eenvoudige integratie met Azure SQL Database zorgt ervoor dat klanten configureren kunnen voor het afhandelen van een breed scala aan scenario's zoals vereist door hun zakelijke behoeften.

De volledige oplossing is gebaseerd op Azure Storage dat klanten via de Azure portal configureert. Azure Storage versleutelt alle gegevens met Storage-Service Encryption vertrouwelijkheid van gegevens in rust te waarborgen. Geografisch redundante opslag (GRS) zorgt ervoor dat een nadelige gebeurtenis in het primaire datacenter van de klant niet in een verlies van gegevens resulteren zal als een tweede kopie wordt opgeslagen in een andere locatie honderden van mijl verwijderd.

Deze architectuur beheert voor een betere beveiliging resources met Azure Active Directory en Azure Sleutelkluis. Systeemstatus wordt bewaakt door Operations Management Suite (OMS) en Azure-Monitor. Klanten configureren beide bewaking services logboeken vastleggen en systeemstatus wordt weergegeven in een enkele, gemakkelijk navigeerbaar dashboard.

Azure SQL-Database wordt meestal beheerd via SQL Server Management Studio (SSMS), die wordt uitgevoerd van een lokale machine is geconfigureerd voor toegang tot de Azure SQL Database via een veilige VPN- of ExpressRoute-verbinding. **Azure raadt configureren van een VPN- of ExpressRoute-verbinding voor het beheer en de gegevens importeren in de resourcegroep van de referentie-architectuur**.

![Analytics voor GDPR verwijzen naar Architectuurdiagram](images/gdpr-analytics-architecture.png?raw=true "Analytics voor GDPR verwijzen naar Architectuurdiagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie zijn de [architectuur voor implementatie](#deployment-architecture) sectie.

- Azure Functions
- Azure SQL Database
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure Monitor
- Azure Storage
- Power BI Dashboard
- Azure Data Catalog
- Azure Security Center
- Application Insights
- Azure Event Grid
- netwerkbeveiligingsgroepen

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de implementatie en de implementatie-elementen.

**Azure Event raster**
[Azure gebeurtenis raster](https://docs.microsoft.com/en-us/azure/event-grid/overview) kunnen klanten eenvoudig om toepassingen te bouwen met architectuur op basis van gebeurtenissen. Gebruikers selecteren de Azure-resource die ze graag willen abonneren op en geef de gebeurtenis-handler of webhook een eindpunt voor het verzenden van de gebeurtenis te. Klanten kunnen webhook-eindpunten beveiligen door queryparameters toevoegen aan de webhook-URL bij het maken van een gebeurtenisabonnement. Azure Event raster ondersteunt alleen HTTPS-webhook-eindpunten. Azure Event raster kunnen klanten bepalen het niveau van toegang krijgen tot andere gebruikers kunnen verschillende bewerkingen zoals de lijst met abonnementen doen, maakt u nieuwe en genereren van sleutels. Gebeurtenis raster gebaseerd toegangsbeheer (RBAC) wordt gebruikt.

**Azure Functions**
[Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) is een server minder compute-service waarmee gebruikers code op aanvraag uitvoeren zonder expliciet inrichten of infrastructuur beheren. Met Azure Functions kunt u een script of een stukje code uitvoeren in reactie op diverse gebeurtenissen.

**Azure Machine Learning**
[Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/preview/) een techniek voor gegevens wetenschappelijke waarmee computers kunnen bestaande gegevens worden gebruikt voor het voorspellen van toekomstige gedrag, resultaten en trends.

**Azure Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) gegevensbronnen gemakkelijk kunnen worden gedetecteerd en begrijpelijk maakt voor de gebruikers die de gegevens te beheren. Algemene gegevensbronnen kunnen worden geregistreerd, label en gezocht naar persoonlijke gegevens. De gegevens blijven in de bestaande locatie, maar een kopie van de metagegevens wordt toegevoegd aan Data Catalog, samen met een verwijzing naar de locatie van de gegevensbron. Deze metagegevens worden ook geïndexeerd zodat elke gegevensbron gemakkelijk kan worden gedetecteerd via zoekopdrachten, en begrijpelijk is voor gebruikers die de gegevensbron detecteren.

### <a name="virtual-network"></a>Virtueel netwerk
Deze verwijzende architectuur definieert een persoonlijke VNet met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die het verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer op een subnet of afzonderlijke VM-niveau te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor Active Directory
  - Een NSG voor de werkbelasting

Elk van de nsg's een bepaalde poorten en protocollen open zodat de oplossing kunt veilig en correct werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Logboeken met diagnostische gegevens en gebeurtenissen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en opgeslagen in een opslagaccount
  - OMS Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-in-transit"></a>Gegevens die onderweg
Azure versleutelt alle communicatie naar en van Azure-datacenters standaard. Alle transacties naar Azure Storage via de Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt de gegevens in rust via versleuteling, database, controle en andere maatregelen.

**Azure Storage** versleutelde data-at-rest-vereisten voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption). Hierdoor kunnen beveiligen en bescherming van persoonlijke gegevens ter ondersteuning van de beveiliging van de organisatie verbintenissen en nalevingsvereisten gedefinieerd door de GDPR.

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
- [SQL dynamische gegevens maskeren (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling gevoelige, persoonlijke gegevens wordt beperkt door het maskeren van de gegevens voor onbevoegde gebruikers of toepassingen. DDM kan automatisch detecteren van mogelijk gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor GDPR beveiliging, en voor het beperken van toegang zodat deze niet de database via toegang door onbevoegden wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheer van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers die toegang tot Azure SQL Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database AAD voor verificatie van de toepassing met Azure SQL Database gebruikt. Zie voor meer informatie hoe [gevoelige gegevens in SQL-Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders voor het definiëren van verfijnde toegangsmachtigingen voor de hoeveelheid toegang verlenen dat gebruikers moeten uitvoeren van hun taken. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, kunnen beheerders toestaan om alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnement toegang is beperkt tot abonnementsbeheerder.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten Beperk het aantal gebruikers die toegang tot bepaalde gegevens zoals persoonlijke gegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, Beperk en bewaak bevoegde identiteiten en de toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just in time beheerderstoegang wanneer deze nodig is.
-   [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen management** de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Sleutelkluis waarmee klanten kunnen persoonlijke gegevens en toegang tot deze gegevens te beveiligen:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van de nodig.
- Sleutelkluis-beleid worden met de minimaal vereiste machtigingen aan sleutels en geheimen gedefinieerd.
- Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum.
- Alle sleutels in de Sleutelkluis worden beschermd door speciale hardware security modules (HSM's). Het sleuteltype is een HSM beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten krijgen minimaal vereiste machtigingen met RBAC.
- Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) kunnen klanten verkeer bewaken, verzamelen van Logboeken en gegevensbronnen voor bedreigingen analyseren. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure services voor configuratie en serviceaanbevelingen om te helpen verbeteren beveiligingspostuur en persoonlijke gegevens te beschermen. Azure Security Center bevat een [dreiging intelligence-rapport](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) voor elk gedetecteerd threat om te helpen respons op incidenten teams onderzoeken en bedreigingen te herstellen.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken zijn Windows-gebeurtenislogboeken system en Azure Blob storage, tabellen en logboeken van de wachtrij.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering met een gedefinieerde bewaarperiode van 2 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien zijn de volgende OMS-oplossingen opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.
-   [Het bijhouden van](https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

**Monitor voor Azure**
[Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) helpt klanten prestaties, beveiliging te handhaven en trends voor doordat organisaties om te controleren, waarschuwingen maken en gegevens, zoals het bijhouden van de API te archiveren aanroepen in Azure-resources van klanten.

**Application Insights**
[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars op meerdere platforms. Gebruik dit voor het bewaken van live-webtoepassing. Deze detecteert afwijkingen en analyses krachtige hulpprogramma's om u te helpen bij het analyseren van problemen en om te begrijpen wat gebruikers daadwerkelijk doen met de app bevat. Het is ontworpen om gebruikers continu prestaties en bruikbaarheid verbeteren.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/gdprAnalyticsdfd) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![Analytics voor GDPR dreiging model](images/gdpr-analytics-threat-model.png?raw=true "Analytics voor GDPR dreiging model")

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk – GDPR klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en de processortijd verantwoordelijkheden voor alle GDPR artikelen bevat. Houd er rekening mee dat voor de Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - GDPR gegevens Analytics implementatie Matrix](https://aka.ms/gdprAnalytics) bevat informatie over welke GDPR artikelen worden verholpen door de architectuur voor analyses, met inbegrip van gedetailleerde beschrijvingen van hoe de implementatie voldoet aan de vereisten van elk gedekte artikel.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN- en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor het opzetten van een verbinding met de resources die zijn geïmplementeerd als onderdeel van deze gegevens veilig analytics verwijzen naar architectuur. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een laag van beveiliging voor gegevens onderweg.

Door het implementeren van een beveiligde VPN-tunnel met Azure kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via het Internet en kan klanten veilig 'tunnel' gegevens in een versleutelde koppeling tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. De [IPsec-tunnelmodus](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt gebruikt bij deze optie als een mechanisme voor versleuteling.

Omdat het verkeer binnen de VPN-tunnel Internet met een site-naar-site VPN passeren, biedt Microsoft met een andere, zelfs veiliger verbindingsoptie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant telecommunicatie provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot het.

Aanbevolen procedures voor het implementeren van een beveiligde hybride netwerk die uitgebreider is dan een on-premises netwerk naar Azure [beschikbaar](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Proces extract-transformatie-Load (ETL)
[PolyBase](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in Azure SQL Database zonder de noodzaak van een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via een T-SQL-query's. Microsoft business intelligence en analyse stack, evenals hulpprogramma's van derden die compatibel is met SQL Server kunnen worden gebruikt met PolyBase.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kan worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) ook koppelen aan een bestaand AAD door het maken van de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
