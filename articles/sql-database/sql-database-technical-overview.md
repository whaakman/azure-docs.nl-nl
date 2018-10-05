---
title: Wat is de service Azure SQL Database? | Microsoft Docs
description: 'Maak kennis met SQL-Database: technische informatie en mogelijkheden van het relationele database beheersysteem (RDBMS) in de cloud van Microsoft.'
keywords: inleiding in sql,intro in sql,wat is sql-database
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: dc5147ff2ebd7bdd6782d92fab33a08b62d854d2
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48804120"
---
# <a name="what-is-the-azure-sql-database-service"></a>Wat is de service Azure SQL Database? 

SQL Database is een algemene, beheerde relationele databaseservice in Microsoft Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON, ruimtelijke gegevens en XML. SQL Database levert dynamisch schaalbare prestaties binnen twee verschillende aankoopmodellen: een [aankoopmodel op basis van vCore](sql-database-service-tiers-vcore.md) en een [aankoopmodel op basis van DTU](sql-database-service-tiers-dtu.md). SQL Database biedt ook opties zoals [columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) voor krachtige analyses en rapportages, en [in-memory OLTP](sql-database-in-memory.md) voor veeleisende transactieverwerking. Microsoft verzorgt op naadloze wijze alle patching en updating van de SQL-codebasis en heeft het beheer van de onderliggende infrastructuur volledig weggewerkt. 

Azure SQL Database biedt de volgende implementatieopties voor een Azure SQL-database:
- Als een individuele database met een eigen set resources die wordt beheerd via een logische server 
- Als een gegroepeerde database in een [elastische pool](sql-database-elastic-pool.md) met een gedeelde set resources die wordt beheerd via een logische server
- Als onderdeel van een verzameling van databases, ook wel een [beheerd exemplaar](sql-database-managed-instance.md) dat systeem en gebruikersdatabases en delen van een set met resources bevat

In de volgende afbeelding worden deze implementatieopties weergegeven:

![implementatieopties](./media/sql-database-technical-overview/deployment-options.png) 

SQL Database deelt de codebasis met de [database-engine van Microsoft SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). In het kader van de cloudstrategie van Microsoft worden nieuwe mogelijkheden van SQL Server eerst uitgebracht in SQL Database en vervolgens in SQL Server zelf. Dankzij deze aanpak kunt u beschikken over de nieuwste mogelijkheden van SQL Server zonder overhead voor patching en upgrading, en zijn de nieuwe functies getest op miljoenen databases. Ga voor informatie over aangekondigde nieuwe mogelijkheden naar:

- **[Azure-roadmap voor SQL Database](https://azure.microsoft.com/roadmap/?category=databases)**:hier kunt u kunt ontdekken wat er nieuw is en wat er op komst is. 
- **[Azure SQL Database-blog](https://azure.microsoft.com/blog/topics/database)**: hier plaatsen de leden van het SQL Server-productteam berichten met het laatste nieuws over SQL Database en informatie over nieuwe functies. 

> [!IMPORTANT]
> Zie [SQL-functies](sql-database-features.md) voor informatie over de functieverschillen tussen SQL Database en SQL Server. 

SQL Database biedt voorspelbare prestaties met meerdere resourcetypen, service-lagen en compute-grootten die dynamische schaalbaarheid geen uitvaltijd, ingebouwde intelligente optimalisatie, wereldwijde schaalbaarheid en beschikbaarheid en geavanceerde beveiliging biedt opties, allemaal met praktisch zonder beheer. Dankzij deze mogelijkheden kunt u zich richten op het sneller ontwikkelen en op de markt brengen van apps, in plaats van kostbare tijd en middelen in te zetten voor het beheer van virtuele machines en infrastructuur. De SQL Database-service is momenteel beschikbaar in 38 datacenters over de hele wereld, en er komen er regelmatig meer bij. Dat betekent dat u uw database kunt uitvoeren in een datacenter bij u in de buurt.

## <a name="scalable-performance-and-pools"></a>Schaalbare prestaties en pools

Met SQL Database, elke database is geïsoleerd van elkaar en draagbaar, elk met een eigen service tier binnen de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) of [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) en een gegarandeerde COMPUTE-grootte. SQL Database biedt verschillende grootten voor verschillende behoeften en mogelijkheid om databasepools te maken het gebruik van resources maximaliseren en geld te besparen.

- Met [SQL Database Managed Instance](sql-database-managed-instance.md), elk exemplaar is geïsoleerd van andere exemplaren om zo gegarandeerde resources. Zie [SQL Database Managed Instance](sql-database-managed-instance.md) voor meer informatie.
- Met de [grootschalige servicelaag](sql-database-hyperscale.md) (preview) in de vCore model aanschaffen, kunt u schalen tot 100 TB met snelle back-up en herstellen van de mogelijkheden.

### <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en schaal aanpassen zonder uitvaltijd

Microsoft SQL Database biedt een [op DTU gebaseerd aankoopmodel](sql-database-service-tiers-dtu.md) of het [op vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). 
- Het op DTU gebaseerde aankoopmodel biedt een combinatie van berekenen, geheugen en i/o-resources in drie servicelagen voor lichte tot zware workloads van databases: Basic, Standard en Premium. COMPUTE-grootten in elke laag bieden een andere combinatie van deze resources, waaraan u extra opslagbronnen kunt toevoegen.
- Met het op vCore gebaseerde aankoopmodel kunt u het aantal vCores, de hoeveelheid of het geheugen, en de hoeveelheid en de snelheid van de opslag kiezen.

U kunt uw eerste app ontwikkelen op een kleine, één database tegen lage kosten per maand in de categorie Algemeen gebruik-service en wijzig vervolgens de servicelaag handmatig of via een programma op elk gewenst moment in de servicelaag van zakelijke essentieel om te voldoen aan de behoeften van uw oplossing. U kunt het prestatieniveau aanpassen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.

> [!IMPORTANT]
> De [grootschalige servicelaag](sql-database-hyperscale.md) is momenteel in openbare preview. Wordt niet aanbevolen om nog een productieworkload uitvoert in grootschalige databases. U kunt een grootschalige-database niet bijwerken naar een andere service-laag. Voor test-doeleinden, wordt aangeraden maakt u een kopie van de huidige database, en het exemplaar aan grootschalige servicelaag bijwerken...

Dynamische schaalbaarheid is iets anders dan automatisch schalen. Automatisch schalen vindt plaats wanneer een service automatisch wordt geschaald op basis van criteria, terwijl u met dynamische schaalbaarheid handmatig kunt schalen zonder uitvaltijd. Eén Azure SQL Database ondersteunt handmatige dynamische schaalbaarheid, maar niet automatisch schalen. Voor een meer *automatische* ervaring zou u elastische pools kunnen gebruiken. Hiermee kunnen databases resources in een pool delen op basis van afzonderlijke databasebehoeften. Er zijn echter scripts die kunnen helpen bij het automatiseren van schaalbaarheid voor één Azure SQL Database. Zie [PowerShell gebruiken voor het controleren en schalen van één Azure SQL-database](scripts/sql-database-monitor-and-scale-database-powershell.md) voor een voorbeeld. 

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastische pools voor optimaal resourcegebruik

Voor veel bedrijven en toepassingen is het kunnen maken van enkele databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. [Elastische pools](sql-database-elastic-pool.md) zijn ontworpen om dit probleem te verhelpen. Het concept is eenvoudig. U wijst prestatieresources toe aan een pool in plaats van aan een individuele database en betaalt voor de collectieve prestatieresources van de pool in plaats van voor de prestaties van een individuele database. 

   ![elastische pools](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Met elastische pools hoeft u zich niet bezig te houden met het verhogen en verlagen van de databaseprestaties als de vraag naar resources fluctueert. De gepoolde databases maken naar behoefte gebruik van de prestatieresources van de elastische pool. Gepoolde databases tellen mee voor het verbruik tot het maximum voor de pool is bereikt. Zo blijven uw kosten voorspelbaar, ook al is uw gebruik van de individuele database dat niet. Bovendien kunt u [databases aan de groep toevoegen of eruit verwijderen](sql-database-elastic-pool-manage-portal.md). Zo kan uw app kan worden opgeschaald van een handjevol databases naar duizenden databases, allemaal binnen het budget dat u zelf bepaalt. U kunt ook de minimale en maximale beschikbare resources die voor databases beschikbaar zijn in de pool beheren om ervoor te zorgen dat er geen database in de pool is die alle poolresources gebruikt en dat elke gepoolde database een gegarandeerd minimum aan resources heeft. Zie [Design Patterns for Multi-tenant SaaS Applications with SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Ontwerppatronen voor multitenant SaaS-toepassingen met behulp van SQL Database) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen. 

Scripts kunnen helpen bij het bewaken en schalen van elastische pools. Zie [PowerShell gebruiken voor het controleren en schalen van een elastische SQL-pool in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md) voor een voorbeeld

> [!IMPORTANT]
> SQL Database Managed Instance biedt geen ondersteuning voor elastische pools.

### <a name="blend-single-databases-with-pooled-databases"></a>Individuele databases combineren met gepoolde databases

Waarvoor u ook kiest - individuele databases of elastische pools - u zit er niet aan vast. U kunt individuele databases combineren met elastische pools en de servicelagen van individuele databases en elastische pools snel en eenvoudig aanpassen aan de situatie. Dankzij de kracht en het bereik van Azure kunt u andere Azure-services combineren en integreren met SQL Database om te voldoen aan de behoeften voor uw unieke app-ontwerp, kosten te besparen en resources efficiënt te beheren. Daarnaast kunt u nieuwe zakelijke verkoopkansen creëren.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Uitgebreide mogelijkheden voor bewaking en waarschuwingen

Maar hoe kunt u de relatieve prestaties van individuele databases en elastische pools vergelijken? Hoe weet u wanneer u moet stoppen met omhoog of omlaag schalen? U maakt gebruik van de [ingebouwde hulpprogramma's voor prestatiebewaking](sql-database-performance.md) en [waarschuwingen](sql-database-insights-alerts-portal.md) in combinatie met de prestatiebeoordelingen. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal op basis van uw huidige prestatiebehoeften of de prestatiebehoeften van uw project. Zie [Aankoopmodel op basis van DTU](sql-database-service-tiers-dtu.md) en [Aankoopmodel op basis van vCore](sql-database-service-tiers-vcore.md) voor meer informatie.

Daarnaast kan SQL Database [metrische gegevens en diagnostische logboeken verzenden](sql-database-metrics-diag-logging.md) die de bewaking vergemakkelijken. U kunt SQL Database configureren voor het opslaan van resourcegebruik, werkrollen en sessies, en connectiviteit in een van deze Azure-resources:

- **Azure Storage**: voor het archiveren van grote hoeveelheden telemetriegegevens voor een lage prijs
- **Azure Event Hub**: voor het integreren van SQL Database-telemetrie in uw eigen bewakingsoplossing of actieve pijplijnen
- **Azure Log Analytics**: voor een ingebouwde bewakingsoplossing met functionaliteit voor rapportages, waarschuwingen en risicobeperking. Azure Log Analytics is een functie van de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md)

    ![architectuur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Beschikbaarheid

De toonaangevende serviceovereenkomst [(SLA)](http://azure.microsoft.com/support/legal/sla/) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt u ervoor dat uw app continu (24 uur per dag, 7 dagen per week) in de lucht blijft. In het Azure-platform wordt elk exemplaar van Azure SQL Database volledig beheerd en zijn geen verlies en een hoge beschikbaarheid van gegevens gegarandeerd. Patches, back-ups, replicatie, foutdetectie, onderliggende potentiële hardware-, software- of netwerkfouten, de implementatie van bugfixes, failovers, database-upgrades en andere onderhoudstaken worden in Azure automatisch afgehandeld. Standaard-beschikbaarheid wordt bereikt door de lagen voor berekeningen en opslag te scheiden. Premium-beschikbaarheid wordt bereikt door de lagen voor berekeningen en opslag te integreren op één knooppunt voor hogere prestaties en vervolgens AlwaysOn-beschikbaarheidsgroepen te implementeren. Zie voor een volledige beschrijving van de mogelijkheden voor hoge beschikbaarheid van Azure SQL Database, [beschikbaarheid van de SQL-Database](sql-database-high-availability.md). Daarnaast biedt SQL Database ingebouwde functies voor [bedrijfscontinuïteit en wereldwijde schaalbaarheid](sql-database-business-continuity.md), zoals:

- **[Automatische back-ups](sql-database-automated-backups.md)**: SQL Database maakt automatische volledige en differentiële back-ups en back-ups van transactielogboeken.
- **[Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md)**: SQL Database ondersteunt het herstel naar ieder bepaald tijdstip dankzij de automatische bewaarperiode voor back-ups.
- **[Actieve geo-replicatie](sql-database-geo-replication-overview.md)**: met SQL Database kunt u maximaal vier leesbare secundaire databases configureren in hetzelfde Azure-datacenter of in wereldwijd gedistribueerde datacenters.  Als u bijvoorbeeld een SaaS-toepassing hebt met een catalogusdatabase met een groot volume aan gelijktijdige alleen-lezen transacties, kunt u actieve geo-replicatie gebruiken om het lezen wereldwijd te schalen en knelpunten weg te nemen die ontstaan door de lees-workloads op de primaire database. 
- **[Failover-groepen](sql-database-geo-replication-overview.md)**: SQL Database biedt hoge beschikbaarheid en taakverdeling op wereldwijde schaal, inclusief transparante geo-replicatie en failover van omvangrijke sets databases en elastische pools. Dankzij failover-groepen en actieve geo-replicatie kunt u wereldwijd gedistribueerde SaaS-toepassingen maken met een minimale overhead aan beheer, doordat u alle complexe bewaking, routering en failover-indeling overlaat aan SQL Database.
- **[Zone-redundante databases](sql-database-high-availability.md)**: met SQL Database kunt u Premium-databases of Bedrijfskritieke databases of elastische pools inrichten in meerdere beschikbaarheidszones. Omdat deze databases en elastische pools meerdere redundante replica's hebben om een hoge beschikbaarheid te garanderen, biedt het plaatsen van deze replica's in meerdere beschikbaarheidszones betere weerbaarheid, inclusief de mogelijkheid om zonder verlies van gegevens automatisch te herstellen van schalingsfouten in het datacenter.  

## <a name="built-in-intelligence"></a>Ingebouwde intelligentie

Met SQL Database beschikt u over ingebouwde intelligentie waarmee u de kosten van het uitvoeren en beheren van databases drastisch verlaagt en de prestaties en beveiliging van uw toepassing optimaliseert. SQL Database verwerkt 24 uur per dag miljoenen workloads van klanten en verzamelt en verwerkt daarbij enorme hoeveelheden telemetriegegevens, waarbij de privacy van klanten te allen tijde volledig wordt gerespecteerd. Er worden verschillen de algoritmen gebruikt om de telemetriegegevens continu te evalueren zodat de service zich kan aanpassen aan uw toepassing. Op basis van deze analyses stelt de service aanbevelingen op voor het verbeteren van de prestaties, afgestemd op uw specifieke workloads. 

### <a name="automatic-performance-monitoring-and-tuning"></a>Automatisch bewaking en afstemming van prestaties

SQL Database biedt gedetailleerde informatie over de query’s die u wilt bewaken. SQL Database leert uw databasepatronen zodat u uw databaseschema kunt aanpassen aan uw workload. SQL Database geeft [aanbevelingen voor het afstemmen van de prestaties](sql-database-advisor.md), waarmee u de aanbevolen acties kunt weergeven en uitvoeren. 

Maar doorlopende databasebewaking is een moeilijke, tijdrovende taak, zeker wanneer het om vele databases gaat. Met [Intelligent Insights](sql-database-intelligent-insights.md) wordt deze taak automatisch voor u uitgevoerd door de prestaties van SQL Database op schaal automatisch te bewaken en u te informeren over problemen met prestatievermindering. Hierbij wordt de hoofdoorzaak van het probleem aangegeven. Indien mogelijk worden er ook aanbevelingen voor prestatieverbetering geboden.

Het is vrijwel onmogelijk om het beheer van enorme hoeveelheden databases op een efficiënte manier uit te voeren, zelfs met de hulpprogramma’s en rapporten van SQL Database en de Azure-portal. In plaats van het handmatig bewaken en afstemmen van uw database, kunt u overwegen sommige van deze taken over te laten aan SQL Database, met de functie voor [automatisch afstemmen](sql-database-automatic-tuning.md). Aanbevolen acties worden automatisch door SQL Database uitgevoerd, getest en geverifieerd om te garanderen dat de prestaties optimaal blijven. Op die manier past SQL Database zich automatisch op een gecontroleerde en veilige manier aan uw workloads aan. Automatische afstemming betekent dat de prestaties van uw database zorgvuldig worden bewaakt en vergeleken voor en na elke afstemactie. Als de prestaties niet zijn verbeterd, wordt de betreffende actie teruggedraaid.

Veel van onze partners die [multitenant SaaS-apps](sql-database-design-patterns-multi-tenancy-saas-applications.md) uitvoeren op SQL Database, vertrouwen al op deze automatische afstemming, zodat de prestaties van hun toepassingen altijd stabiel en voorspelbaar zijn. Dankzij deze functie wordt de kans op prestatieproblemen (op welk moment dan ook) aanzienlijk kleiner. En omdat een deel van hun klanten ook werkt met SQL Server, maken ze gebruik van dezelfde indexeringsaanbevelingen van SQL Database om hun klanten met SQL Server te helpen.

Er zijn twee automatisch afstemmingsmethoden [in SQL Database](sql-database-automatic-tuning.md):

- **Automatisch indexbeheer**: hiermee worden indexen geïdentificeerd die moeten worden toegevoegd aan of verwijderd uit uw database.
- **Automatische abonnementcorrectie**: hiermee worden abonnementen met problemen geïdentificeerd en prestatieproblemen met SQL-abonnementen opgelost (binnenkort beschikbaar, nu al beschikbaar in SQL Server 2017).

### <a name="adaptive-query-processing"></a>Verwerking van adaptieve query’s

Er is ook een reeks functies voor de [verwerking van adaptieve query’s](/sql/relational-databases/performance/adaptive-query-processing) toegevoegd aan SQL Database, waaronder interleave-uitvoering van tabelfuncties met meerdere instructies, feedback over geheugentoekenning in batchmodus en adaptieve samenvoegingen in batchmodus. Elk van deze verwerkingsfuncties voor adaptieve query’s werkt met vergelijkbare technieken voor ‘leren en aanpassen’, voor het oplossen van prestatieproblemen die het gevolg zijn hardnekkige problemen met query-optimalisatie.

## <a name="advanced-security-and-compliance"></a>Geavanceerde beveiliging en naleving van voorschriften

SQL Database biedt een reeks [ingebouwde functies voor beveiliging en naleving](sql-database-security-overview.md) zodat uw toepassing voldoet aan diverse vereisten op het gebied van beveiliging en de naleving van voorschriften. 

### <a name="advance-threat-protection"></a>Advanced Threat Protection

SQL Advanced Threat Protection is een uniform pakket voor geavanceerde mogelijkheden voor SQL-beveiliging. Het bevat functionaliteit voor het detecteren en classificeren van gevoelige gegevens, het beheren van beveiligingsproblemen in uw database, en het detecteren van afwijkende activiteiten die kunnen duiden op een bedreiging van de database. Het is tevens een centraal punt voor het inschakelen en beheren van deze mogelijkheden.

- [Gegevensdetectie en -classificatie](sql-database-data-discovery-and-classification.md) (momenteel een preview-versie) biedt ingebouwde mogelijkheden in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Het kan worden gebruikt voor het zichtbaar maken van de classificatiestatus van gegevens in uw database, en het traceren van de toegang tot gevoelige gegevens binnen en buiten de database.
- [Evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) is een eenvoudig te configureren service waarmee u potentiële zwakke plekken in de beveiliging van de database kunt detecteren, volgen en verhelpen. Deze service biedt u inzicht in de status van de beveiliging en bruikbare stappen om beveiligingsproblemen op te lossen en de beveiliging van uw database te verbeteren.
- [Detectie van bedreigingen](sql-database-threat-detection.md) detecteert vreemde activiteiten die duiden op ongebruikelijke en mogelijk schadelijke pogingen toegang te verkrijgen tot of aanvallen uit te voeren op uw database. Hiermee wordt uw database continu gecontroleerd op verdachte activiteiten en wordt u onmiddellijk gewaarschuwd bij mogelijke beveiligingsproblemen, SQL-injectieaanvallen en afwijkende databasetoegangspatronen. De waarschuwingen bevatten detailinformatie over verdachte activiteiten en aanbevelingen voor het onderzoeken en tegenhouden ervan.

### <a name="auditing-for-compliance-and-security"></a>Controles voor naleving en beveiliging

Met [Azure SQL Database Auditing](sql-database-auditing.md) worden databasegebeurtenissen bijgehouden en naar een auditlogboek in uw Azure Storage-account geschreven. Dankzij controles kunt u zorgen voor naleving van wet- en regelgeving, krijgt u inzicht in de activiteit in uw database en in de afwijkingen en discrepanties die kunnen wijzen op problemen voor het bedrijf of vermoedelijke schendingen van de beveiliging.

### <a name="data-encryption"></a>Gegevensversleuteling

Uw gegevens worden beveiligd met SQL Database via gegevensversleuteling. Voor gegevens die in beweging zijn, wordt [Transport Layer Security](https://support.microsoft.com/kb/3135244) gebruikt, voor data-at-rest [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) en voor gebruikte gegevens [Altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine).

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-integratie en meervoudige verificatie

Dankzij [Azure Active Directory-integratie](sql-database-aad-authentication.md) kunt u in SQL Database de identiteit van databasegebruikers en andere Microsoft-services centraal beheren. Deze mogelijkheid vereenvoudigt het beheer van machtigingen en verbetert de beveiliging. Azure Active Directory ondersteunt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md) (MFA) voor betere beveiliging van gegevens en toepassingen, en ondersteunt ook een proces voor eenmalige aanmelding.

### <a name="compliance-certification"></a>Nalevingscertificering

SQL Database wordt regelmatig gecontroleerd en is gecertificeerd volgens diverse nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://azure.microsoft.com/support/trust-center/services/) vindt, voor meer informatie.

## <a name="easy-to-use-tools"></a>Gebruiksvriendelijke hulpprogramma’s

SQL Database maakt het bouwen en onderhouden van toepassingen makkelijker en productiever. Met SQL Database kunt u zich concentreren op dat waar u het beste in bent: fantastische apps bouwen. In SQL Database kunt u beheren en ontwikkelen met de hulpprogramma’s en vaardigheden die u al hebt.

- **[De Azure-portal](https://portal.azure.com/)**: een webtoepassing voor het beheren van alle Azure-services 
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

- Zie deze Quick Starts om snel aan de slag te gaan:

  - [Een SQL-database maken in Azure Portal](sql-database-get-started-portal.md)  
  - [Een SQL-database maken met de Azure CLI](sql-database-get-started-cli.md)
  - [Een SQL-database maken met PowerShell](sql-database-get-started-powershell.md)

- Zie de volgende artikelen voor een reeks Azure CLI- en PowerShell-voorbeelden:
  - [Azure CLI-voorbeelden voor SQL Database](sql-database-cli-samples.md)
  - [Azure PowerShell-voorbeelden voor SQL Database](sql-database-powershell-samples.md)
