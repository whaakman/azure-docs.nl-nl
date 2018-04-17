---
title: Wat is de service Azure SQL Database? | Microsoft Docs
description: 'Maak kennis met SQL-Database: technische informatie en mogelijkheden van het relationele database beheersysteem (RDBMS) in de cloud van Microsoft.'
keywords: inleiding in sql,intro in sql,wat is sql-database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.topic: overview
ms.date: 03/07/2018
ms.author: carlrab
ms.openlocfilehash: a3b703c96e309294e5327fb7fb013cbf28c369e4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="what-is-the-azure-sql-database-service"></a>Wat is de service Azure SQL Database? 

SQL Database is een algemene, beheerde relationele databaseservice in Microsoft Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON, ruimtelijke gegevens en XML. SQL Database biedt beheerde [enkelvoudige SQL-databases](sql-database-servers-databases.md), beheerde SQL-databases in een [elastische pool](sql-database-elastic-pool.md) en SQL [Managed Instances](sql-database-managed-instance.md)(in openbare preview). De service biedt [dynamisch schaalbare prestaties](sql-database-service-tiers.md) en opties zoals [columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) voor krachtige analyses en rapportages, en [in-memory OLTP](sql-database-in-memory.md) voor veeleisende transactieverwerking. Microsoft verzorgt op naadloze wijze alle patching en updating van de SQL-codebasis en heeft het beheer van de onderliggende infrastructuur volledig weggewerkt. 

SQL Database deelt de codebasis met de [database-engine van Microsoft SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). In het kader van de cloudstrategie van Microsoft worden nieuwe mogelijkheden van SQL Server eerst uitgebracht in SQL Database en vervolgens in SQL Server zelf. Dankzij deze aanpak kunt u beschikken over de nieuwste mogelijkheden van SQL Server zonder overhead voor patching en upgrading, en zijn de nieuwe functies getest op miljoenen databases. Ga voor informatie over aangekondigde nieuwe mogelijkheden naar:

- **[Azure-roadmap voor SQL Database](https://azure.microsoft.com/roadmap/?category=databases)**:hier kunt u kunt ontdekken wat er nieuw is en wat er op komst is. 
- **[Azure SQL Database-blog](https://azure.microsoft.com/blog/topics/database)**: hier plaatsen de leden van het SQL Server-productteam berichten met het laatste nieuws over SQL Database en informatie over nieuwe functies. 

> [!IMPORTANT]
> Zie [SQL-functies](sql-database-features.md) voor informatie over de functieverschillen tussen SQL Database en SQL Server. 

SQL Database levert voorspelbare prestaties op meerdere serviceniveaus met dynamische schaalbaarheid zonder uitvaltijd, ingebouwde intelligente optimalisatie, schaalbaarheid en beschikbaarheid op wereldwijde schaal en geavanceerde beveiligingsmogelijkheden. Bovendien hoeft u vrijwel geen tijd te besteden aan beheer. Dankzij deze mogelijkheden kunt u zich richten op het sneller ontwikkelen en op de markt brengen van apps, in plaats van kostbare tijd en middelen in te zetten voor het beheer van virtuele machines en infrastructuur. De SQL Database-service is momenteel beschikbaar in 38 datacenters over de hele wereld, en er komen er regelmatig meer bij. Dat betekent dat u uw database kunt uitvoeren in een datacenter bij u in de buurt.

> [!IMPORTANT]
> SQL Database Managed Instance is momenteel in preview en is slechts op één serviceniveau beschikbaar. Zie [SQL Database Managed Instance](sql-database-managed-instance.md) voor meer informatie.
>

## <a name="scalable-performance-and-pools"></a>Schaalbare prestaties en pools

Met SQL Database zijn alle databases volledig geïsoleerd en draagbaar, met elk een eigen [servicelaag](sql-database-service-tiers.md) en een gegarandeerd prestatieniveau. SQL Database biedt verschillende prestatieniveaus voor verschillende behoeften. Door de mogelijkheid om databasepools te maken kunt u het gebruik van resources maximaliseren en geld besparen.

Met SQL Database Managed Instance wordt elk exemplaar geïsoleerd van andere exemplaren om zo gegarandeerde resources te kunnen toewijzen. Zie [SQL Database Managed Instance](sql-database-managed-instance.md) voor meer informatie. 

### <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en schaal aanpassen zonder uitvaltijd

Microsoft SQL Database biedt een [op DTU gebaseerd aankoopmodel](sql-database-service-tiers.md#dtu-based-purchasing-model) of het [op vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview). 
- Het op DTU gebaseerde aankoopmodel biedt een combinatie van berekenen, geheugen en i/o-resources in drie servicelagen voor lichte tot zware workloads van databases: Basic, Standard en Premium. Prestatieniveaus binnen elke laag bieden een andere combinatie van deze resources, waaraan u extra opslagbronnen kunt toevoegen.
- Met het op vCore gebaseerde aankoopmodel kunt u het aantal vCores, de hoeveelheid of het geheugen, en de hoeveelheid en de snelheid van de opslag kiezen.

U kunt uw eerste app ontwikkelen op één enkele kleine database voor slechts enkele euro's per maand. Vervolgens kunt u op elk gewenst moment handmatig of programmatisch de servicelaag wijzigen, zodat deze beter past bij de behoeften van uw oplossing. U kunt het prestatieniveau aanpassen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.

   ![DTU schalen](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

SQL Database Managed Instance is in preview en biedt momenteel maar één servicelaag. Zie [SQL Database Managed Instance](sql-database-managed-instance.md) voor meer informatie.

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastische pools voor optimaal resourcegebruik

Voor veel bedrijven en toepassingen is het kunnen maken van enkele databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. [Elastische pools](sql-database-elastic-pool.md) zijn ontworpen om dit probleem te verhelpen. Het concept is eenvoudig. U wijst prestatieresources toe aan een pool in plaats van aan een individuele database en betaalt voor de collectieve prestatieresources van de pool in plaats van voor de prestaties van een individuele database. 

   ![elastische pools](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Met elastische pools hoeft u zich niet bezig te houden met het verhogen en verlagen van de databaseprestaties als de vraag naar resources fluctueert. De gepoolde databases maken naar behoefte gebruik van de prestatieresources van de elastische pool. Gepoolde databases tellen mee voor het verbruik tot het maximum voor de pool is bereikt. Zo blijven uw kosten voorspelbaar, ook al is uw gebruik van de individuele database dat niet. Bovendien kunt u [databases aan de groep toevoegen of eruit verwijderen](sql-database-elastic-pool-manage-portal.md). Zo kan uw app kan worden opgeschaald van een handjevol databases naar duizenden databases, allemaal binnen het budget dat u zelf bepaalt. U kunt ook de minimale en maximale beschikbare resources die voor databases beschikbaar zijn in de pool beheren om ervoor te zorgen dat er geen database in de pool is die alle poolresources gebruikt en dat elke gepoolde database een gegarandeerd minimum aan resources heeft. Zie [Design Patterns for Multi-tenant SaaS Applications with SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Ontwerppatronen voor multitenant SaaS-toepassingen met behulp van SQL Database) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

> [!IMPORTANT]
> SQL Database Managed Instance biedt geen ondersteuning voor elastische pools.

### <a name="blend-single-databases-with-pooled-databases"></a>Individuele databases combineren met gepoolde databases

Waarvoor u ook kiest - individuele databases of elastische pools - u zit er niet aan vast. U kunt individuele databases combineren met elastische pools en de servicelagen van individuele databases en elastische pools snel en eenvoudig aanpassen aan de situatie. Dankzij de kracht en het bereik van Azure kunt u andere Azure-services combineren en integreren met SQL Database om te voldoen aan de behoeften voor uw unieke app-ontwerp, kosten te besparen en resources efficiënt te beheren. Daarnaast kunt u nieuwe zakelijke verkoopkansen creëren.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Uitgebreide mogelijkheden voor bewaking en waarschuwingen

Maar hoe kunt u de relatieve prestaties van individuele databases en elastische pools vergelijken? Hoe weet u wanneer u moet stoppen met omhoog of omlaag schalen? U maakt gebruik van de [ingebouwde hulpprogramma's voor prestatiebewaking](sql-database-performance.md) en [waarschuwingen](sql-database-insights-alerts-portal.md) in combinatie met de prestatiebeoordelingen. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal op basis van uw huidige prestatiebehoeften of de prestatiebehoeften van uw project. Zie [SQL Database-opties en prestaties: wat is er beschikbaar in elke servicelaag](sql-database-service-tiers.md) voor meer informatie.

Daarnaast kan SQL Database [metrische gegevens en diagnostische logboeken verzenden](sql-database-metrics-diag-logging.md) die de bewaking vergemakkelijken. U kunt SQL Database configureren voor het opslaan van resourcegebruik, werkrollen en sessies, en connectiviteit in een van deze Azure-resources:

- **Azure Storage**: voor het archiveren van grote hoeveelheden telemetriegegevens voor een lage prijs
- **Azure Event Hub**: voor het integreren van SQL Database-telemetrie in uw eigen bewakingsoplossing of actieve pijplijnen
- **Azure Log Analytics**: voor een ingebouwde bewakingsoplossing met functionaliteit voor rapportages, waarschuwingen en risicobeperking

    ![architectuur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Beschikbaarheid

De toonaangevende serviceovereenkomst [(SLA)](http://azure.microsoft.com/support/legal/sla/) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt u ervoor dat uw app continu (24 uur per dag, 7 dagen per week) in de lucht blijft. Daarnaast biedt SQL Database ingebouwde functies voor [bedrijfscontinuïteit en wereldwijde schaalbaarheid](sql-database-business-continuity.md), zoals:

- **[Automatische back-ups](sql-database-automated-backups.md)**: SQL Database maakt automatische volledige en differentiële back-ups en back-ups van transactielogboeken.
- **[Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md)**: SQL Database ondersteunt het herstel naar ieder bepaald tijdstip dankzij de automatische bewaarperiode voor back-ups.
- **[Actieve geo-replicatie](sql-database-geo-replication-overview.md)**: met SQL Database kunt u maximaal vier leesbare secundaire databases configureren in hetzelfde Azure-datacenter of in wereldwijd gedistribueerde datacenters.  Als u bijvoorbeeld een SaaS-toepassing hebt met een catalogusdatabase met een groot volume aan gelijktijdige alleen-lezen transacties, kunt u actieve geo-replicatie gebruiken om het lezen wereldwijd te schalen en knelpunten weg te nemen die ontstaan door de lees-workloads op de primaire database. 
- **[Failover-groepen](sql-database-geo-replication-overview.md)**: SQL Database biedt hoge beschikbaarheid en taakverdeling op wereldwijde schaal, inclusief transparante geo-replicatie en failover van omvangrijke sets databases en elastische pools. Dankzij failover-groepen en actieve geo-replicatie kunt u wereldwijd gedistribueerde SaaS-toepassingen maken met een minimale overhead aan beheer, doordat u alle complexe bewaking, routering en failover-indeling overlaat aan SQL Database.
- **[Zone-redundante databases](sql-database-high-availability.md)**: met SQL Database kunt u Premium-databases of Bedrijfskritieke (preview) databases of elastische pools inrichten in meerdere beschikbaarheidszones. Omdat deze databases en elastische pools meerdere redundante replica's hebben om een hoge beschikbaarheid te garanderen, biedt het plaatsen van deze replica's in meerdere beschikbaarheidszones betere weerbaarheid, inclusief de mogelijkheid om zonder verlies van gegevens automatisch te herstellen van schalingsfouten in het datacenter. Deze functie is momenteel beschikbaar als preview-product. 

## <a name="built-in-intelligence"></a>Ingebouwde intelligentie

Met SQL Database beschikt u over ingebouwde intelligentie waarmee u de kosten van het uitvoeren en beheren van databases drastisch verlaagt en de prestaties en beveiliging van uw toepassing optimaliseert. SQL Database verwerkt 24 uur per dag miljoenen workloads van klanten en verzamelt en verwerkt daarbij enorme hoeveelheden telemetriegegevens, waarbij de privacy van klanten te allen tijde volledig wordt gerespecteerd. Er worden verschillen de algoritmen gebruikt om de telemetriegegevens continu te evalueren zodat de service zich kan aanpassen aan uw toepassing. Op basis van deze analyses stelt de service aanbevelingen op voor het verbeteren van de prestaties, afgestemd op uw specifieke workloads. 

### <a name="automatic-performance-monitoring-and-tuning"></a>Automatisch bewaking en afstemming van prestaties

SQL Database biedt gedetailleerde informatie over de query’s die u wilt bewaken. SQL Database leert uw databasepatronen zodat u uw databaseschema kunt aanpassen aan uw workload. SQL Database geeft [aanbevelingen voor het afstemmen van de prestaties](sql-database-advisor.md), waarmee u de aanbevolen acties kunt weergeven en uitvoeren. 

Maar doorlopende databasebewaking is een moeilijke, tijdrovende taak, zeker wanneer het om vele databases gaat. Met [Intelligent Insights](sql-database-intelligent-insights.md) wordt deze taak automatisch voor u uitgevoerd door de prestaties van SQL Database op schaal automatisch te bewaken en u te informeren over problemen met prestatievermindering. Hierbij wordt de hoofdoorzaak van het probleem aangegeven. Indien mogelijk worden er ook aanbevelingen voor prestatieverbetering geboden.

Het is vrijwel onmogelijk om het beheer van enorme hoeveelheden databases op een efficiënte manier uit te voeren, zelfs met de hulpprogramma’s en rapporten van SQL Database en Azure Portal. In plaats van het handmatig bewaken en afstemmen van uw database, kunt u overwegen sommige van deze taken over te laten aan SQL Database, met de functie voor [automatisch afstemmen](sql-database-automatic-tuning.md). Aanbevolen acties worden automatisch door SQL Database uitgevoerd, getest en geverifieerd om te garanderen dat de prestaties optimaal blijven. Op die manier past SQL Database zich automatisch op een gecontroleerde en veilige manier aan uw workloads aan. Automatische afstemming betekent dat de prestaties van uw database zorgvuldig worden bewaakt en vergeleken voor en na elke afstemactie. Als de prestaties niet zijn verbeterd, wordt de betreffende actie teruggedraaid.

Veel van onze partners die [multitenant SaaS-apps](sql-database-design-patterns-multi-tenancy-saas-applications.md) uitvoeren op SQL Database, vertrouwen al op deze automatische afstemming, zodat de prestaties van hun toepassingen altijd stabiel en voorspelbaar zijn. Dankzij deze functie wordt de kans op prestatieproblemen (op welk moment dan ook) aanzienlijk kleiner. En omdat een deel van hun klanten ook werkt met SQL Server, maken ze gebruik van dezelfde indexeringsaanbevelingen van SQL Database om hun klanten met SQL Server te helpen.

Er zijn twee automatisch afstemmingsmethoden [in SQL Database](sql-database-automatic-tuning.md):

- **Automatisch indexbeheer**: hiermee worden indexen geïdentificeerd die moeten worden toegevoegd aan of verwijderd uit uw database.
- **Automatische abonnementcorrectie**: hiermee worden abonnementen met problemen geïdentificeerd en prestatieproblemen met SQL-abonnementen opgelost (binnenkort beschikbaar, nu al beschikbaar in SQL Server 2017).

### <a name="adaptive-query-processing"></a>Verwerking van adaptieve query’s

Er is ook een reeks functies voor de [verwerking van adaptieve query’s](/sql/relational-databases/performance/adaptive-query-processing) toegevoegd aan SQL Database, waaronder interleave-uitvoering van tabelfuncties met meerdere instructies, feedback over geheugentoekenning in batchmodus en adaptieve samenvoegingen in batchmodus. Elk van deze verwerkingsfuncties voor adaptieve query’s werkt met vergelijkbare technieken voor ‘leren en aanpassen’, voor het oplossen van prestatieproblemen die het gevolg zijn hardnekkige problemen met query-optimalisatie.

### <a name="intelligent-threat-detection"></a>Intelligente detectie van bedreigingen

 [SQL Threat Detection](sql-database-threat-detection.md) maakt gebruik van [SQL Database Auditing](sql-database-auditing.md) om Azure SQL-databases doorlopend te controleren op mogelijk schadelijke pogingen om toegang te krijgen tot gevoelige gegevens. SQL-bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten potentiële bedreigingen kunnen detecteren en erop reageren zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers ontvangen waarschuwingen bij verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang. De SQL-beveiligingswaarschuwingen bieden details over verdachte activiteiten en geven aanbevelingen voor het onderzoeken en tegenhouden ervan. Gebruikers kunnen de verdachte gebeurtenissen onderzoeken om te bepalen of die het gevolg zijn van pogingen om toegang te krijgen tot, of misbruik te maken van, gegevens in de database. Met bedreigingsdetectie is het eenvoudig om potentiële bedreigingen voor de database weg te nemen zonder de hulp van een beveiligingsexpert of het moeten beheren van geavanceerde bewakingssystemen.

## <a name="advanced-security-and-compliance"></a>Geavanceerde beveiliging en naleving van voorschriften

SQL Database biedt een reeks [ingebouwde functies voor beveiliging en naleving](sql-database-security-overview.md) zodat uw toepassing voldoet aan diverse vereisten op het gebied van beveiliging en de naleving van voorschriften. 

### <a name="auditing-for-compliance-and-security"></a>Controles voor naleving en beveiliging

Met [Azure SQL Database Auditing](sql-database-auditing.md) worden databasegebeurtenissen bijgehouden en naar een auditlogboek in uw Azure Storage-account geschreven. Dankzij controles kunt u zorgen voor naleving van wet- en regelgeving, krijgt u inzicht in de activiteit in uw database en in de afwijkingen en discrepanties die kunnen wijzen op problemen voor het bedrijf of vermoedelijke schendingen van de beveiliging.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens

De [transparante gegevensversleuteling](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) van SQL Database beschermt gegevens tegen schadelijke activiteiten met behulp van real-time versleuteling en ontsleuteling van de database, gekoppelde back-ups en inactieve transactielogboeken, zonder dat er wijzigingen in de toepassing nodig zijn. Met ingang van mei 2017 worden alle nieuw gemaakte Azure-database automatisch beschermd met transparante gegevensversleuteling (TDE). TDE is een beproefde technologie voor versleuteling van inactieve gegevens, die verplicht is volgens veel nalevingsstandaarden voor de bescherming tegen diefstal van opslagmedia. Klanten kunnen Azure Key Vault gebruiken voor het beheren van hun TDE-versleutelingssleutels en andere geheimen, op een veilig manier die voldoet aan de voorschriften.

### <a name="data-encryption-in-motion"></a>Versleuteling van gegevens in beweging

SQL is het enige databasesysteem dat met [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) functionaliteit biedt voor de versleuteling van gevoelige gegevens in beweging, inactieve gegevens en gegevens die worden verwerkt in een query. Het unieke Always Encrypted biedt ongeëvenaarde beveiliging tegen diefstal van kritieke gegevens. Met Always Encrypted worden bijvoorbeeld de creditcardnummers van klanten altijd versleuteld opgeslagen in de database, ook tijdens queryverwerking. Ontsleuteling vindt plaats op het moment dat de gegevens moeten worden gebruikt en kan alleen worden uitgevoerd door gemachtigde medewerkers of toepassingen die de gegevens moeten verwerken.

### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering

Met [dynamische gegevensmaskering in SQL Database](sql-database-dynamic-data-masking-get-started.md) wordt de blootstelling van gevoelige gegevens beperkt door deze gegevens te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering helpt onbevoegde toegang tot gevoelige gegevens te voorkomen, doordat klanten kunnen aangeven hoeveel van de gevoelige gegevens mag worden vrijgegeven, met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.

### <a name="row-level-security"></a>Beveiliging op rijniveau

Met [beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (zoals groepslidmaatschap of uitvoeringscontext). Beveiliging op rijniveau (RLS) vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. U kunt bijvoorbeeld bepalen dat werkrollen alleen toegang hebben tot de rijen met gegevens die relevant zijn voor hun afdeling, of de toegang van klanten beperken tot de gegevens die relevant zijn voor hun bedrijf.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-integratie en meervoudige verificatie

Dankzij [Azure Active Directory-integratie](sql-database-aad-authentication.md) kunt u in SQL Database de identiteit van databasegebruikers en andere Microsoft-services centraal beheren. Deze mogelijkheid vereenvoudigt het beheer van machtigingen en verbetert de beveiliging. Azure Active Directory ondersteunt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md) (MFA) voor betere beveiliging van gegevens en toepassingen, en ondersteunt ook een proces voor eenmalige aanmelding.

### <a name="compliance-certification"></a>Nalevingscertificering

SQL Database wordt regelmatig gecontroleerd en is gecertificeerd volgens diverse nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://azure.microsoft.com/support/trust-center/services/) vindt, voor meer informatie.

## <a name="easy-to-use-tools"></a>Gebruiksvriendelijke hulpprogramma’s

SQL Database maakt het bouwen en onderhouden van toepassingen makkelijker en productiever. Met SQL Database kunt u zich concentreren op dat waar u het beste in bent: fantastische apps bouwen. In SQL Database kunt u beheren en ontwikkelen met de hulpprogramma’s en vaardigheden die u al hebt.

- **[Azure Portal](https://portal.azure.com/)**: een webtoepassing voor het beheren van alle Azure-services 
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)**: een gratis te downloaden clienttoepassing voor het beheren van elke SQL-infrastructuur, van SQL Server tot SQL Database
- **[SQL Server Data Tools in Visual Studio](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)**: een gratis te downloaden clienttoepassing voor het ontwikkelen van relationele SQL Server-databases, Azure SQL-databases, Integration Services-pakketten, Analysis Services-gegevensmodellen en Reporting Services-rapporten.
- **[Visual Studio Code](https://code.visualstudio.com/docs)**: een gratis te downloaden opensource code-editor voor Windows, macOS en Linux die ondersteuning biedt voor extensies, waaronder de [mssql-extensie](https://aka.ms/mssql-marketplace) voor het uitvoeren van query's in Microsoft SQL Server, Azure SQL Database en SQL Data Warehouse.

SQL Database ondersteunt het maken van toepassingen met Python, Java, Node.js, PHP, Ruby en .NET op macOS, Linux en Windows. SQL Database ondersteunt dezelfde [verbindingsbibliotheken](sql-database-libraries.md) als SQL Server.

## <a name="engage-with-the-sql-server-engineering-team"></a>Contact met het technische team van SQL Server

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): stel hier uw vragen over databasebeheer
- [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-server): stel hier uw vragen over ontwikkeling
- [MSDN-forums](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): stel hier uw technische vragen
- [Feedback](http://aka.ms/sqlfeedback): hier kunt u bugs rapporteren en functies aanvragen
- [Reddit](https://www.reddit.com/r/SQLServer/): vragen en antwoorden voor SQL Server

## <a name="next-steps"></a>Volgende stappen

- Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/) voor hulpprogramma's voor het berekenen en vergelijken van de kosten voor individuele databases en elastische pools.

- Raadpleeg deze snelstarts om snel aan de slag te gaan:

  - [Een SQL-database maken in Azure Portal](sql-database-get-started-portal.md)  
  - [Een SQL-database maken met de Azure CLI](sql-database-get-started-cli.md)
  - [Een SQL-database maken met PowerShell](sql-database-get-started-powershell.md)

- Zie de volgende artikelen voor een reeks Azure CLI- en PowerShell-voorbeelden:
  - [Azure CLI-voorbeelden voor SQL Database](sql-database-cli-samples.md)
  - [Azure PowerShell-voorbeelden voor SQL Database](sql-database-powershell-samples.md)
