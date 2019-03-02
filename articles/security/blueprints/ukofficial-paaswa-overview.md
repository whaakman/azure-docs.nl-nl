---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing die als host fungeert voor de officiële Workloads groot-Brittannië
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing die als host fungeert voor de officiële Workloads groot-Brittannië
services: security
author: jomolesk
ms.assetid: 446105ad-a863-44f5-a964-6ead1dac4787
ms.service: security
ms.topic: article
ms.date: 07/13/2018
ms.author: jomolesk
ms.openlocfilehash: 7f67b7363719f544c485503e0f8207bb1f1078ce
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244703"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-hosting-for-uk-official-workloads"></a>Azure-beveiliging en naleving blauwdruk: PaaS-webtoepassing die als host fungeert voor de officiële Workloads groot-Brittannië

## <a name="azure-security-and-compliance-blueprints"></a>Blauwdrukken voor Azure-beveiliging en naleving

Azure blauwdrukken bestaan uit richtlijnen documenten en automation-sjablonen die voor het implementeren van cloud-gebaseerde architecturen voor oplossingen voor scenario's waarvoor accreditatie-en nalevingsvereisten. Azure blauwdrukken zijn verzamelingen van richtlijnen en automation-sjabloon waardoor Microsoft Azure-klanten kunnen Versnel de levering van hun zakelijke doelstellingen die via het inrichten van een foundation-architectuur die kan worden uitgebreid om te voldoen aan eventuele aanvullende vereisten.

## <a name="overview"></a>Overzicht

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen en automation-scripts voor het leveren van een Microsoft Azure [platform als een service (PaaS)](https://azure.microsoft.com/overview/what-is-paas/) gehost architectuur voor webtoepassingen geschikt is voor het verwerken van workloads die zijn geclassificeerd Als [UK-OFFICIAL](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/715778/May-2018_Government-Security-Classifications-2.pdf). Deze beveiligingsclassificatie omvat het merendeel van de gegevens die zijn gemaakt of verwerkt via de openbare sector. Dit omvat routinematige zakelijke activiteiten en -services, welke indien verloren, gestolen of gepubliceerd in de media, waarvan sommige kan schadelijke gevolgen hebben. De typische bedreigingsprofiel voor de officiële classificatie is bijna hetzelfde als een privé-onderneming die waardevolle informatie en services bevat. Groot-Brittannië officiële verwacht de noodzaak om te beschermen van gegevens van de Britse overheid of services op basis van een dreiging of een inbreuk op door aanvallers met gebonden mogelijkheden en resources, zoals (maar is niet beperkt tot) hactivists, single-issue druk te verlichten groepen, resulterend journalisten bevoegde afzonderlijke hackers, en het merendeel van strafrechtelijke personen en groepen.

Deze blauwdruk is gecontroleerd door de UK nationale Cyber Security Center (NCSC) en overeenstemt met de NCSC 14 Cloud Security Principles.

De architectuur maakt gebruik van Azure [platform als een service](https://azure.microsoft.com/overview/what-is-paas/) onderdelen voor het leveren van een omgeving waarmee klanten om te voorkomen dat de kosten en complexiteit van het aanschaffen van softwarelicenties, van het beheer van de onderliggende infrastructuur van toepassingen en middleware of de ontwikkelhulpprogramma's en andere bronnen. Klanten beheren de toepassingen en services die hij ontwikkelt, ligt de focus op het leveren van de waarde voor het bedrijf, terwijl Microsoft Azure de andere Azure-resources zoals virtuele machines, opslag beheert en netwerken, plaatsen van meer van de [deling van verantwoordelijkheid](https://docs.microsoft.com/azure/security/security-paas-deployments#division-of-responsibility) voor infrastructuurbeheer u aan bij de Azure-platform. [Azure App Services](https://azure.microsoft.com/services/app-service/) automatisch schalen, hoge beschikbaarheid biedt, biedt ondersteuning voor Windows en Linux en maakt automatische implementaties vanuit GitHub, Azure DevOps of een gitopslagplaats als standaard-services mogelijk. Door het gebruik van App Services, ontwikkelaars zich concentreren op het leveren van de waarde voor het bedrijf zonder de overhead van het beheer van infrastructuur. Het is mogelijk ontwikkel nieuwe toepassingen voor Java, PHP, Node.js, Python, HTML of C# web of ook voor het migreren van bestaande cloud of op lokale webtoepassingen voor Azure App Services (Hoewel grondige vanwege de zorgvuldigheid en testen om te bevestigen prestaties is vereist).

Deze blauwdruk is gericht op het inrichten van een veilige basis [platform als een service](https://azure.microsoft.com/overview/what-is-paas/) webinterface voor openbare en ook back-office-gebruikers. In dit scenario van blauwdruk ontwerp rekening gehouden met het gebruik van Azure die worden gehost op basis van web services waar een openbare gebruiker kunt veilig indienen, weergeven en beheren van gevoelige gegevens; ook dat een back-office of government-operator de gevoelige gegevens die de openbare gebruiker heeft ingediend veilig kunt verwerken. Use cases voor dit scenario kunnen zijn:

- Een gebruiker een belastingaangifte indienen met behulp van een government-operator verwerken van de inzending;
- Een gebruiker die een service via een webgebaseerde toepassing aanvragen met een back-office-gebruiker valideren en leveren van de service. of
- Een gebruiker om te zoeken en weergeven van openbare domein help-informatie over een government-service.

Met behulp van [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) sjablonen en Azure Command Line Interface scripts, de blauwdruk implementeert u een omgeving die wordt uitgelijnd op het Verenigd Koninkrijk nationale Cyber beveiliging centrum (NCSC) 14 [Cloud Security Principles](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) en het Center for Internet Security (CIS) [kritieke beveiligingscontroles](https://www.cisecurity.org/critical-controls.cfm). De NCSC raadt aan om te evalueren van de eigenschappen voor de beveiliging van de service en voor meer informatie over de verdeling van de verantwoordelijkheid tussen de klant en de leverancier van de Cloud Security Principles door klanten worden gebruikt. Microsoft heeft verstrekt informatie met betrekking tot elk van deze principes voor meer informatie over de splitsing van verantwoordelijkheden. Deze architectuur en de bijbehorende Azure Resource Manager-sjablonen worden ondersteund door de Microsoft-whitepaper [14 Cloud beveiligingsmaatregelen voor het Verenigd Koninkrijk in de cloud met behulp van Microsoft Azure](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Deze architectuur is gecontroleerd door de NCSC en het Verenigd Koninkrijk NCSC 14 Cloud beveiligingsprincipes, waardoor de publieke sector organisaties de mogelijkheid om te voldoen aan verplichtingen met cloud-gebaseerde services wereldwijd en in het Verenigd Koninkrijk in fast track conform de Microsoft Azure-cloud. Deze sjabloon implementeert de infrastructuur voor de werkbelasting. Toepassingscode en de ondersteunende bedrijfslaag en software van gegevens van de laag moeten worden geïnstalleerd en geconfigureerd door klanten. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukofficial-paaswa-repo/).

Deze blauwdruk is een foundation-architectuur. Onze klanten kunnen deze blauwdruk gebruiken als basis voor de officiële classificatie web gebaseerde workloads en vouw op de sjablonen en resources met hun eigen vereisten. Deze blauwdruk bouwt voort op de beginselen van het [UK officiële drie lagen IaaS-webtoepassingen blauwdruk](https://aka.ms/ukofficial-iaaswa) te bieden van onze klanten [infrastructure-as-a-service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/) en PaaS-implementatie-opties voor het hosten van web gebaseerde workloads.

Voor het implementeren van deze blauwdruk, is een Azure-abonnement vereist. Als u een Azure-abonnement hebt, kunt u kosteloos zich om snel en eenvoudig: Aan de slag met Azure. Klik op [hier](https://aka.ms/ukofficial-paaswa-repo/) voor implementatie-instructies.

## <a name="architecture-and-components"></a>Architectuur en onderdelen

Deze blauwdruk biedt een webtoepassing die als host fungeert oplossing in een Azure-cloud-omgeving die ondersteuning biedt voor UK-OFFICIAL werkbelastingen. De architectuur biedt een beveiligde omgeving die gebruikmaakt van Azure-platform als een servicemogelijkheden. Binnen de omgeving zijn twee App Service-web-apps geïmplementeerd (één voor gebruikers) en één voor back-office-gebruikers, met een API-App-laag voor de zakelijke services voor de web-front-end. Een Azure SQL Database wordt geïmplementeerd als een beheerde relationele gegevensopslag voor de toepassing. Verbinding met deze onderdelen van buiten het platform en tussen al deze onderdelen worden versleuteld via TLS 1.2 om te controleren of gegevens in transport privacy, die toegang heeft geverifieerd door Azure Active Directory.

![PaaS Web Hosting van toepassingen voor UK officiële Workloads verwijzing Architectuurdiagram](images/ukofficial-paaswa-architecture.png?raw=true "PaaS Web Hosting van toepassingen voor UK officiële Workloads verwijzing Architectuurdiagram")

Als onderdeel van de implementatie-architectuur, veilige opslag inrichten, bewaking & logboekregistratie, geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging en management worden mogelijkheden ook geïmplementeerd om ervoor te zorgen dat klanten alle hulpprogramma's die zijn vereist om te hebben Beveilig en controleren van hun omgeving voor deze oplossing.

Deze oplossing maakt gebruik van de volgende Azure-services. Informatie van de implementatiearchitectuur vindt u in de [architectuur](#deployment-architecture) sectie.

- Azure Active Directory
- App Service
- Web-app
- API-app
- Azure DNS
- Key Vault
- Azure Monitor (Logboeken)
- Application Insights
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage

## <a name="deployment-architecture"></a>Implementatie-architectuur

De volgende sectie bevat de elementen van de implementatie en uitvoering.

### <a name="security"></a>Beveiliging

#### <a name="identity-and-authentication"></a>Identiteit en verificatie

Deze blauwdruk zorgt ervoor dat de toegang tot resources wordt beschermd met directory-en identiteitsbeheerservices. Deze architectuur maakt volledig gebruik van [identiteit als de beveiligingsperimeter](https://docs.microsoft.com/azure/security/security-paas-deployments). 

De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) is van Microsoft met meerdere tenants cloudgebaseerde directory service- en identiteit. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, met inbegrip van gebruikers met toegang tot de SQL-Database.
- Verificatie van de operator gerichte web-App- en toegangsbeheer voor het beheer van de Azure-resources wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- Versleuteling van de kolom database maakt gebruik van Azure AD om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie, [altijd versleuteld: Bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- Voor de burgers bestemde webtoepassing is geconfigureerd voor openbare toegang. Om toe te staan voor het maken van accounts en -verificatie via active directory of sociale id-providers [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) indien nodig kan worden geïntegreerd.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen en risico's accounts bevat aanbevelingen voor het verbeteren van de beveiligingsstatus van de identiteiten van uw organisatie, configureert u automatische antwoorden gedetecteerd verdachte activiteit met betrekking tot de identiteiten van uw organisatie, en verdachte incidenten onderzoekt het probleem en wordt de juiste actie op te lossen.
- [Azure Role-based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) maakt nauwkeurig beheer gerichte toegang voor Azure. Abonnementstoegang is beperkt tot de beheerder van het abonnement en Azure Key Vault toegang is beperkt tot gebruikers die Sleutelbeheer toegang nodig hebben.
- Door gebruik te maken van [Azure Active Directory voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) klanten aanvullende beveiligingscontroles op toegang tot apps of gebruikers in hun omgeving op basis van specifieke voorwaarden, zoals locatie, apparaat, status en meld u kunnen afdwingen in de risico's.
- [Azure DDoS Protection](https://docs.microsoft.com/azure/security/security-paas-deployments#security-advantages-of-a-paas-cloud-service-model) gecombineerd met aanbevolen procedures voor ontwerp van toepassingen, biedt beveiliging tegen DDoS-aanvallen, met het verkeer altijd bewaken, en realtime beperken van de algemene op netwerkniveau aanvallen. Met een architectuur voor PaaS, platform-niveau van DDoS protection is transparant voor de klant en opgenomen in het platform, maar het is belangrijk te weten dat de toepassing security ontwerp verantwoordelijkheid wordt veroorzaakt door de klant.

#### <a name="data-in-transit"></a>Gegevens die onderweg zijn

Gegevens zijn van doorvoer van buiten en tussen Azure-onderdelen worden beschermd met behulp van [Transport Layer Security/Secure Sockets Layer (TLS/SSL)](https://www.microsoft.com/TrustCenter/Security/Encryption), waarbij symmetrische cryptographie op basis van een gedeeld geheim voor het versleutelen van communicatie als ze worden gebruikt via het netwerk worden verzonden. Standaard wordt het netwerkverkeer beveiligd met behulp van TLS 1.2.

#### <a name="security-and-malware-protection"></a>Beveiliging en malware protection

[Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt een gecentraliseerde weergave van de beveiligingsstatus van al uw Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u kunt snel zien of er resources die uw aandacht vereisen.

[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) is een persoonlijke cloudconsultant die u helpt volgen aanbevolen procedures voor het optimaliseren van uw Azure-implementaties. Het analyseert uw resourceconfiguratie en gebruikstelemetrie, en raadt vervolgens oplossingen aan die u kunnen helpen de kosteneffectiviteit, prestaties, hoge beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) is een realtime-beveiliging-functie waarmee u kunt herkennen en verwijderen van virussen, spyware en andere schadelijke software. Dit wordt standaard op de onderliggende infrastructuur van de PaaS-virtuele machine is geïnstalleerd en wordt beheerd door de Azure-infrastructuur transparant voor de klant, ervoor te zorgen dat

### <a name="paas-services-in-this-blueprint"></a>PaaS-services in deze blauwdruk

#### <a name="azure-app-service"></a>Azure App Service

Azure App Service biedt een volledig beheerde hostomgeving voor web-App ontwikkeld in Java, PHP, Node.js, Python, HTML en C# zonder om infrastructuur te beheren. Het biedt automatisch schalen en hoge beschikbaarheid ondersteunt zowel Windows als Linux en maakt automatische implementaties van [Azure DevOps](https://azure.microsoft.com/services/visual-studio-team-services/) of een willekeurige op basis van een Git-repo.

App Service is [ISO, SOC en PCI-](https://www.microsoft.com/TrustCenter/) en verificatie van gebruikers met [Azure Active Directory](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) of via sociaal aanmelden ([Google](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-google), [Facebook](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-facebook), [Twitter](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-twitter), en [Microsoft authentication](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-microsoft).

Basic-, Standard en Premium-abonnementen zijn voor productieworkloads en draaien op specifieke virtuele Machine-instanties. Elke instantie kan meerdere toepassingen en domeinen ondersteunen. App-services ook ondersteuning voor [IP-adresbeperkingen](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions) voor het beveiligen van verkeer naar de goedgekeurde IP-adressen, indien nodig, evenals [beheerde identiteiten voor een Azure-resources](https://docs.microsoft.com/azure/app-service/overview-managed-identity) voor beveiligde verbinding met andere PaaS-services zoals [voor Key Vault](https://azure.microsoft.com/services/key-vault/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Extra beveiliging is vereist als host fungeert voor uw apps in een persoonlijke, exclusieve Azure-omgeving en is ideaal voor apps waarvoor een veilige verbindingen met uw on-premises netwerk, of extra prestaties en schaal onze Isolated-abonnement.

Deze sjabloon implementeert de volgende functies van App Service:

- [Standard](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) laag voor App Service-Plan
- Meerdere App Service [implementatiesites](https://docs.microsoft.com/azure/app-service/deploy-staging-slots): Ontwikkel, Preview, QA, UAT en natuurlijk productie (standaard sleuf).
- [Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/app-service/overview-managed-identity) verbinding maken met [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) (dit kan ook worden gebruikt voor toegang tot [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 
- Integratie met [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps) om prestaties te bewaken
- [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) 
- Metrische gegevens [waarschuwingen](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) 
- [Azure API Apps](https://azure.microsoft.com/services/app-service/api/) 

#### <a name="azure-sql-database"></a>Azure SQL Database

SQL Database is een algemene, beheerde relationele databaseservice in Microsoft Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON, ruimtelijke gegevens en XML. SQL Database biedt beheerde één SQL-databases, beheerde SQL-databases in een [elastische pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool), en SQL [beheerde instanties](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) (in openbare preview). Het biedt een [dynamisch schaalbare prestaties])https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) en opties zoals [columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) voor extreme analytische analyse en rapportage, en [in-memory OLTP](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory) voor extreme transactionele verwerking. Microsoft verzorgt op naadloze wijze alle patching en updating van de SQL-codebasis en heeft het beheer van de onderliggende infrastructuur volledig weggewerkt.

Azure SQL-Database in deze blauwdruk

De Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:

- [Firewallregels op serverniveau en databaseniveau](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure), of via [Virtual Network-Service-eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) met behulp van [virtuele netwerkregels](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview).
- [Transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) helpt Azure SQL Database en Azure Data Warehouse beschermen tegen de dreiging van schadelijke activiteiten. Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing.
- [Azure AD-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication), kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt één plek voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen.
- Gebruik van Azure Active Directory voor het databasebeheer van de
- [Auditlogboeken](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) naar storage-accounts
- Metrische gegevens [waarschuwingen](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) voor mislukte DB-verbindingen
- [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Altijd versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

### <a name="azure-storage"></a>Azure Storage

Microsoft [Azure Storage](https://azure.microsoft.com/services/storage/) is een Microsoft beheerde cloudservice waarmee opslag die maximaal beschikbaar, beveiligde, duurzaam, schaalbaar en redundant is. Azure Storage bestaat uit Blob Storage, File Storage en Queue Storage.

#### <a name="azure-storage-in-this-blueprint"></a>Azure Storage in deze blauwdruk

Deze sjabloon maakt gebruik van de volgende onderdelen van Azure Storage:

- [Storage-Serviceversleuteling](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) 
- Alleen HTTPS-verbindingen toestaan

#### <a name="data-at-rest"></a>Data-at-rest

Via [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) alle gegevens die zijn geschreven naar Azure Storage worden versleuteld met 256-bits AES-versleuteling, een van de krachtigste blokversleutelingsmethoden die. U kunt Microsoft beheerde sleutels gebruiken met SSE of kunt u [uw eigen versleutelingssleutels](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Storage-accounts kunnen worden beveiligd [Virtual Network-Service-eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) met behulp van [virtuele netwerkregels](https://docs.microsoft.com/azure/storage/common/storage-network-security).

Gedetailleerde informatie over het beveiligen van Azure Storage vindt u de [beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide).


### <a name="secrets-management"></a>Geheimen beheren

#### <a name="azure-key-vault"></a>Azure Key Vault

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) wordt gebruikt voor het beveiligen van de toepassingssleutels en geheimen om ervoor te zorgen dat ze niet toegankelijk zijn door derde partijen. Key Vault is niet bedoeld als archief voor wachtwoorden van gebruikers. Hiermee kunt u meerdere beveiligde containers, die kluizen worden genoemd maken. Deze kluizen worden ondersteund door hardware security modules (HSM's). Kluizen verminderen de kans op onbedoeld verlies van beveiligingsinformatie door de opslag van toepassingsgeheimen te centraliseren. Sleutelkluizen regelen en registreren ook de toegang tot alles wat erin is opgeslagen. Azure Key Vault kan het aanvragen en vernieuwen van Transport Layer Security-certificaten (TLS) verwerken, met behulp van de functies die vereist zijn voor een robuuste oplossing voor het beheren van de levenscyclus van certificaten.

#### <a name="azure-key-vault-in-this-blueprint"></a>Azure Key Vault in deze blauwdruk

- Bevat de toegangssleutel voor opslag met leestoegang verleend aan de [beheerde identiteit](https://docs.microsoft.com/azure/app-service/overview-managed-identity) van de klant gerichte web-app
- Het wachtwoord van de SQL Server DBA bevat (in een afzonderlijke kluis)
- Logboekregistratie van diagnostische gegevens

### <a name="monitoring-logging-and-audit"></a>Bewaking, logboekregistratie en controle

#### <a name="azure-monitor-logs"></a>Logboeken in Azure Monitor

[Logboeken in Azure Monitor](https://azure.microsoft.com/services/log-analytics/) is een service in Azure waarmee u kunt verzamelen en analyseren van gegevens die worden gegenereerd door resources in uw cloud en on-premises omgevingen.

#### <a name="azure-monitor-logs-in-this-blueprint"></a>Azure Monitor-Logboeken in deze blauwdruk

- SQL-evaluatie
- Diagnostische gegevens van Key Vault
- Verbinding met Application Insights
- Azure-activiteitenlogboek

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars op meerdere platforms. Gebruikt voor het bewaken van live web-apps het wordt automatisch detecteren van afwijkende prestaties, prestaties analyseren en een diagnose stellen problemen en om te begrijpen hoe gebruikers interacteren met de app. Application Insights kunnen worden geïmplementeerd op platforms, waaronder .NET, Node.js en Java EE, on-premises gehost of in de cloud. De service kan ook worden geïntegreerd met uw DevOps-proces en bevat verbindingspunten naar verschillende hulpmiddelen voor ontwikkelaars.

#### <a name="application-insights-in-this-blueprint"></a>Application Insights in deze blauwdruk

Deze sjabloon maakt gebruik van de volgende Application Insights-onderdelen:

- Application Insights-dashboard per site (Operator, klanten en API)

#### <a name="azure-activity-logs"></a>Azure-activiteitenlogboeken

[Azure-activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs#what-you-can-do-with-the-activity-log) controlelaag gebeurtenissen voor uw abonnementen voert een controle uit. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen.

#### <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) kunt kernbewaking voor Azure-services doordat het verzamelen van metrische gegevens, activiteitenlogboeken en diagnostische logboeken. Azure Monitor biedt metrische infrastructuurgegevens en logboeken op basisniveau voor de meeste services in Microsoft Azure.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/ukofficial-paaswa-tm) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS Web Hosting van toepassingen voor UK officiële Workloads risicomodel](images/ukofficial-paaswa-threat-model.png?raw=true "PaaS Web Hosting van toepassingen voor risicomodel UK officiële Workloads")

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC Cloud Security Principles naleving documentatie

De kroon commerciële Service (een Bureau die geschikt is voor commerciële en aankopen activiteit verbeteren door de overheid) vernieuwd de classificatie van Microsoft binnen de regeling vallen enterprise cloudservices aan v6 G-Cloud, die betrekking hebben op het aanbod op het OFFICIAL-niveau. Details van Azure en G-Cloud kunnen u vinden in de [Azure Verenigd Koninkrijk G-Cloud security evaluatieoverzicht](https://www.microsoft.com/trustcenter/compliance/uk-g-cloud).

Deze blauwdruk overeenstemt met de 14 cloud security-principes die worden beschreven in de NCSC [Cloud Security Principles](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) om te zorgen voor een omgeving die ondersteuning biedt voor werkbelastingen die zijn geclassificeerd als groot-Brittannië officiële.

De [Azure-beveiliging en naleving blauwdruk - groot-Brittannië officiële klant verantwoordelijkheid Matrix](https://aka.ms/ukofficial-crm) (Excel-werkmap) geeft een lijst van alle 14 cloud security principles en geeft voor elk principe (of principe subdeel), of het principe implementatie is de verantwoordelijkheid van Microsoft, de klant, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor UK officiële principe implementatie Matrix](https://aka.ms/ukofficial-paaswa-pim) (Excel-werkmap) geeft een lijst van alle 14 cloud security principles en geeft voor elk principe (of principe subdeel) die de verantwoordelijkheid van een klant in de Matrix van de verantwoordelijkheden van de klant wordt gebruikt (1) als de blauwdruk implementeert het principe en 2) een beschrijving van hoe de toepassing wordt uitgelijnd met de principal-vereisten.  

De Cloud Security Alliance (CSA) gepubliceerd bovendien de Cloud Control Matrix ter ondersteuning van klanten in de evaluatie van cloudproviders en om te vragen die moeten worden beantwoord voordat u doorgaat naar de cloud services te identificeren. Antwoord, Microsoft Azure beantwoord de CSA Consensus evaluatie Initiative Questionnaire ([CSA CAIQ](https://www.microsoft.com/TrustCenter/Compliance/CSA)), waarin wordt beschreven hoe Microsoft de principes voor de voorgestelde adressen.

## <a name="third-party-assessment"></a>Evaluatie van derden

Deze blauwdruk is gecontroleerd door de UK nationale Cyber Security Center (NCSC) en overeenstemt met de NCSC 14 Cloud Security Principles

De automation-sjablonen zijn getest door het team UK klant succes eenheid Azure Cloud Solution Architect en onze partner van Microsoft, [Ampliphae](http://www.ampliphae.com/).


## <a name="deploy-the-solution"></a>De oplossing implementeren

Deze Azure-beveiliging en naleving blauwdruk Automation bestaat uit JSON-configuratiebestanden en PowerShell-scripts die worden verwerkt door Azure Resource Manager API-service om resources binnen Azure te implementeren. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/ukofficial-paaswa-repo).

Drie manieren is te vinden voor de implementatie van; Een eenvoudige 'express' [Azure CLI 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) geschikt is voor het snel ontwikkelen van een test-omgeving; een geparameteriseerde [2 van Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) aanpak biedt meer configuratie voor werkbelasting omgevingen; en een Azure-portal op basis van een implementatie waarbij de operator de implementatieparameters via Azure portal kunt opgeven. 

1.  Klonen of downloaden [dit](https://aka.ms/ukofficial-paaswa-repo) GitHub-opslagplaats naar uw lokale werkstation.
2.  Beoordeling [methode 1: Azure CLI-2 (Express-versie)](https://aka.ms/ukofficial-paaswa-repo/#method-1-azure-cli-2-express-version) en de opgegeven opdrachten uit te voeren.
3.  Beoordeling [methode 1a: Azure CLI-2 (configureren van de implementatie via scriptargumenten)](https://aka.ms/ukofficial-paaswa-repo/#method-1a-azure-cli-2-configuring-the-deployment-via-script-arguments) en voert u de opgegeven opdrachten
4.  Beoordeling [methode 2: Azure-portal implementatieproces](https://aka.ms/ukofficial-paaswa-repo/#method-2-azure-portal-deployment-process) en de vermelde opdrachten uitvoeren

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="api-management"></a>API Management

[Met Azure API Management](https://azure.microsoft.com/services/api-management/) kan worden gebruikt voor de API-App-Service voor extra lagen van beveiliging, beperking en besturingselementen beschikbaar, proxy en API's beveiligen.

### <a name="azure-b2c"></a>Azure B2C

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) kan worden geïmplementeerd als een besturingselement waarmee gebruikers zich registreren, een identiteit maken en inschakelen autorisatie en toegangsbeheer voor de openbare web-App.

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
- Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
