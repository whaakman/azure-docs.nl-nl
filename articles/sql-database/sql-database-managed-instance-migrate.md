---
title: Database van SQL Server-instantie migreren naar Azure SQL Database - beheerd exemplaar | Microsoft Docs
description: Meer informatie over het migreren van een database van SQL Server-exemplaar naar Azure SQL Database - beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
manager: craigg
ms.date: 11/07/2019
ms.openlocfilehash: 7cf54b79fac87905117e321574571890c59315e6
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827069"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Beheerd exemplaar van SQL Server-exemplaar migratie naar Azure SQL Database

In dit artikel hebt u meer informatie over de methoden voor het migreren van een SQL Server 2005 of latere versie exemplaar [Azure SQL Database managed instance](sql-database-managed-instance.md). Zie voor meer informatie over het migreren naar een individuele database of elastische pool [migreren naar een database van één of gegroepeerde](sql-database-cloud-migrate.md). Zie voor informatie over het migreren van andere platforms migratie [Azure handleiding voor databasemigratie](https://datamigration.microsoft.com/).

> [!NOTE]
> Als u wilt snel starten en probeer dan Managed Instance, wilt u mogelijk gaat u naar [snelstartgids](/sql-database-managed-instance-quickstart-guide.md) in plaats van deze pagina. 

Op hoog niveau, is het proces van de database-migratie ziet eruit zoals:

![Migratieproces](./media/sql-database-managed-instance-migration/migration-process.png)

- [Beoordeling van compatibiliteit met beheerd exemplaar](#assess-managed-instance-compatibility) waar u moet ervoor zorgen dat er geen blokkerende problemen die voorkomen uw migraties dat kunnen.
  - Deze stap omvat ook het maken van [basislijn van prestaties](#create-performance-baseline) om te bepalen van Resourcegebruik op de bron-SQL Server-exemplaar. Deze stap is nodig als u wilt dat o implementeren van de juiste omvang Managed Instance en controleer of de prestaties na de migratie worden niet beïnvloed.
- [Kies de app-connectiviteitsopties](sql-database-managed-instance-connect-app.md)
- [Implementeren naar een optimaal grootte beheerd exemplaar](#deploy-to-an-optimally-sized-managed-instance) waarin u technische kenmerken (aantal vCores, hoeveelheid geheugen) en prestatielaag (bedrijfskritiek, algemeen gebruik) van uw beheerde exemplaar wordt kiezen.
- [Kies migratiemethode en migreer](#select-migration-method-and-migrate) waar u uw databases met behulp van offline migratie (systeemeigen back-up/herstel, database importe/exporteren) of online migratie (Data Migration Service, transactionele replicatie) migreert.
- [Toepassingen bewaken](#monitor-applications) om ervoor te zorgen dat u prestaties hebt verwacht.

> [!NOTE]
> Als u wilt migreren van een individuele database in een individuele database of elastische pool, Zie [een SQL Server-database migreren naar Azure SQL Database](sql-database-single-database-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Beoordeling van compatibiliteit met beheerd exemplaar

Ga eerst na of het beheerde exemplaar is compatibel met de vereisten voor de database van uw toepassing. De Implementatieoptie voor het beheerde exemplaar is ontworpen voor eenvoudige lift- and -shift-migratie voor de meeste van bestaande toepassingen die gebruikmaken van on-premises SQL Server of op virtuele machines. Echter, u hebt mogelijk soms functies of mogelijkheden die nog niet worden ondersteund en de kosten van de implementatie van een tijdelijke oplossing zijn te hoog.

Gebruik [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) voor het detecteren van potentieel compatibiliteit die invloed hebben op database-functionaliteit op Azure SQL Database. DMA niet nog ondersteuning voor beheerd exemplaar als doel van de migratie, maar het wordt aanbevolen om analyse uitvoeren op Azure SQL-Database en zorgvuldig lijst gerapporteerde functiepariteit en compatibiliteitsproblemen op basis van de productdocumentatie. Zie [Azure SQL Database-functies](sql-database-features.md) om te controleren of er zijn enkele blokkerende problemen die niet blockers in beheerd exemplaar, omdat de meeste van de blokkerende problemen te voorkomen dat een migratie naar Azure SQL Database zijn verwijderd met beheerde gerapporteerd het exemplaar. Voor het exemplaar, functies, zoals query's die databaseoverschrijdend, transacties binnen hetzelfde exemplaar tussen meerdere databases, gekoppelde server met andere SQL bronnen, CLR, globale tijdelijke tabellen zijn exemplaar niveau weergaven, Service Broker en dergelijke beschikbaar in beheerde exemplaren.

Als er enkele blokkerende problemen die niet worden verwijderd met de Implementatieoptie voor beheerd exemplaar gerapporteerd zijn, moet u mogelijk rekening houden met een andere optie, zoals [SQL Server op Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/). Hier volgen enkele voorbeelden:

- Als u directe toegang tot voor het besturingssysteem of het bestandssysteem, bijvoorbeeld voor installatie van derden of aangepaste agents op dezelfde virtuele machine met SQL Server vereist.
- Als u strikte afhankelijk zijn van functies die nog steeds niet worden ondersteund, zoals FileStream / bestandstabel, PolyBase en cross-instance-transacties.
- Als dit echt nodig te blijven op een specifieke versie van SQL Server (2012, bijvoorbeeld).
- Als uw rekenvereisten veel lager zijn die beheerd exemplaar biedt (één vCore, bijvoorbeeld) en de consolidatie van de database is niet acceptabel optie.

Als u hebt opgelost, alle migratieblokkeringen geïdentificeerd en u doorgaat met de migratie naar Managed Instance, houd er rekening mee dat mogelijk enkele van de wijzigingen die invloed hebben op de prestaties van uw workload:
- Verplichte volledig-herstelmodel en reguliere geautomatiseerde back-upschema kunnen invloed hebben op prestaties van uw workload of onderhoud/ETL acties als u regelmatig gebruikt eenvoudige/bulksgewijs geregistreerde model of back-ups op aanvraag is gestopt.
- Andere server of database configuraties van het niveau, zoals traceermarkeringen of compatibiliteitsniveaus
- Nieuwe functies die u, zoals transparante Database Encryption (TDE) of automatische failover-groepen gebruikt kunnen CPU-en i/o-beïnvloeden.

Beheerd exemplaar gegarandeerd 99,99% beschikbaarheid, zelfs in de essentiële scenario's, waardoor overhead veroorzaakt door deze functies kan niet worden uitgeschakeld. Zie voor meer informatie, [de belangrijkste oorzaken die verschillende prestaties voor SQL Server en de Managed Instance kunnen veroorzaken](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/).

### <a name="create-performance-baseline"></a>Basislijn van prestaties maken

Als u vergelijken van de prestaties van uw workload op Managed Instance met de oorspronkelijke werkbelasting wordt uitgevoerd op SQL Server wilt, moet u zou maken van een basislijn van prestaties die wordt gebruikt voor de vergelijking. 

Basislijn van prestaties is een set parameters, zoals gemiddelde/max CPU-gebruik, gemiddelde/maximale schijf-i/o-latentie, doorvoer, IOP's, levensverwachting voor pagina van de gemiddelde/maximum, gemiddelde maximale grootte van tempdb. U wilt vergelijkbaar of nog beter parameters hebben na de migratie, dus is het belangrijk om te meten en noteer de basislijnwaarden voor deze parameters. Naast systeemparameters moet u zou het selecteren van een set van de representatieve query's of de belangrijkste query's in uw werkbelasting en meting gemiddelde-min./Max. duur, CPU-gebruik voor de geselecteerde query's. Deze waarden zou u kunnen de prestaties van de werkbelasting wordt uitgevoerd op Managed Instance met de oorspronkelijke waarden op de bron-SQL Server inschakelen.

Enkele van de parameters die u voor het meten van op uw SQL Server-exemplaar zou moeten zijn: 
- [CPU-gebruik op uw SQL Server-exemplaar bewaken](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) en noteer de gemiddelde en de piekvraag CPU-gebruik.
- [Geheugengebruik op uw SQL Server-exemplaar bewaken](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage) en bepaalt de hoeveelheid geheugen die wordt gebruikt door andere onderdelen, zoals de buffergroep, cache, kolom-archief van toepassingen plannen [In-memory OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017), enzovoort. Bovendien moet u waarden voor gemiddelde en piekuren van prestatiemeteritem levensverwachting voor pagina-geheugen.
- Schijf-i/o-gebruik op de bron SQL Server-exemplaar met controleren [sys.dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) weergeven of [prestatiemeteritems](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage).
- Werkbelasting en de queryprestaties of uw SQL Server-exemplaar bewaken via het onderzoeken van dynamische beheerweergaven of Query Store als u van SQL Server 2016 + versie migreert. Identificeer de gemiddelde duur en CPU-gebruik van de belangrijkste query's in uw werkbelasting om ze te vergelijken met de query's die worden uitgevoerd op het beheerde exemplaar.

> [!Note]
> Als u een probleem met uw werkbelasting op SQL Server, zoals hoog CPU-gebruik, constante geheugendruk, tempdb- of parametrization problemen ziet, moet u proberen deze kunnen worden opgelost op uw bron-SQL Server-exemplaar voordat u de basislijn en de migratie. Migreren van kent problemen naar een nieuw systeem migh leiden tot onverwachte resultaten en een vergelijking van de prestaties ongeldig te maken.

Als resultaat van deze activiteit moet u gedocumenteerde gemiddelde en maximale waarden voor CPU, geheugen en i/o-gebruik op uw bronsysteem, evenals gemiddelde en maximale duur en CPU-gebruik van de dominante en de meest kritieke query's in uw werkbelasting. U moet deze waarden later gebruiken om de prestaties van uw werkbelasting op Managed Instance met de basislijnprestaties van de werkbelasting op de bron-SQL Server.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementeren naar een beheerd exemplaar voor optimale grootte

Beheerd exemplaar is die is ontworpen voor on-premises werkbelastingen die van plan bent om naar de cloud te verplaatsen. Dit introduceert een [nieuwe aankoopmodel](sql-database-service-tiers-vcore.md) die biedt meer flexibiliteit bij het selecteren van het juiste niveau van resources voor uw workloads. In de on-premises wereld bent u waarschijnlijk gewend zijn aan het formaat van deze werkbelastingen met behulp van fysieke kernen en i/o-bandbreedte. De aankopen model voor het beheerde exemplaar is gebaseerd op virtuele kernen, of 'vCores', met extra opslagruimte en i/o-beschikbaar afzonderlijk. Het vCore-model is een eenvoudigere manier om te begrijpen van uw rekenvereisten in de cloud in plaats van wat u gebruikt on-premises vandaag nog. Dit nieuwe model kunt u de juiste grootte uw doelomgeving in de cloud. Hier worden enkele algemene richtlijnen die u helpen kunnen bij het kiezen van de juiste service-laag en de kenmerken beschreven:
- Op basis van de basislijn CPU-gebruik kunt u een beheerd exemplaar die overeenkomt met het aantal kernen dat u op de SQL Server gebruikt inrichten, hoeven er rekening mee dat CPU-kenmerken moet mogelijk worden geschaald zodat deze overeenkomt met [waar Managed Instance is VM-eigenschappen geïnstalleerd](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
- Op basis van het geheugengebruik van de basislijn kiezen [de servicelaag waarvoor overeenkomende geheugen](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics). De hoeveelheid geheugen kan niet rechtstreeks worden gekozen, zodat u moet de Managed Instance met de hoeveelheid vCores met overeenkomende geheugen (bijvoorbeeld 5.1 GB/vCore in Gen5) selecteren. 
- Gebaseerd op de basislaag i/o latentie van het subsysteem bestand kiezen tussen algemeen gebruik (latentie is groter dan 5 ms) en bedrijfskritiek Servicelagen (latentie van minder dan 3 ms).
- Op basis van basislijn doorvoer vooraf toewijzen van de grootte van gegevens of logboekbestanden om op te halen waarvan wordt verwacht i/o-prestaties.

Kunt u rekenkracht en opslagbronnen op implementatie tijd en daarna wijzigen zonder uitvaltijd voor uw toepassing met de [Azure-portal](sql-database-scale-resources.md):

![beheerd exemplaar grootte](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Zie voor meer informatie over het maken van de VNet-infrastructuur en een beheerd exemplaar, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Het is belangrijk dat u uw bestemming VNet en subnet altijd in overeenstemming met [managed instance VNet vereisten](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Eventuele incompatibiliteit kunt voorkomen dat u uit het maken van nieuwe instanties of met behulp van die u al hebt gemaakt. Meer informatie over [maken van nieuwe](sql-database-managed-instance-create-vnet-subnet.md) en [configureren van bestaande](sql-database-managed-instance-configure-vnet-subnet.md) netwerken.

## <a name="select-migration-method-and-migrate"></a>Kies migratiemethode en migreren

De beheerde instantie optie doelen gebruiker implementatiescenario's vereisen massale databasemigratie van on-premises of IaaS-implementaties database. Ze zijn beste keuze als u wilt lift- and -shift de back-end van de toepassingen die regelmatig instantieniveau gebruiken en / of functionaliteiten tussen meerdere databases. Als dit uw scenario, kunt u een volledige-exemplaar kunt verplaatsen naar een bijbehorende omgeving in Azure zonder de noodzaak opnieuw ontwerpen van uw toepassingen.

Voor het verplaatsen van SQL-exemplaren, moet u zorgvuldig plannen:

- De migratie van alle databases die moeten worden CO-locaties (die zijn uitgevoerd op hetzelfde exemplaar)
- De migratie van objecten op exemplaarniveau die uw toepassing afhankelijk is, met inbegrip van aanmeldingen, referenties, SQL Agent-taken en Operators en triggers voor server op.

Beheerd exemplaar is een beheerde service waarmee u een aantal van de reguliere DBA activiteiten voor het platform delegeren terwijl ze zijn ingebouwd in. Sommige niveau Instantiegegevens hoeft daarom niet worden gemigreerd, zoals onderhoudstaken voor regelmatige back-ups of de Always On configureren als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Beheerd exemplaar biedt ondersteuning voor de volgende database migratieopties (momenteel dit zijn de enige ondersteunde migratiemethoden):

- Azure Database migratieservice - migratie met bijna nul downtime,
- Systeemeigen `RESTORE DATABASE FROM URL` : maakt gebruik van systeemeigen back-ups van SQL Server en vereist enige uitvaltijd.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

De [Azure Database Migration Service (DMS)](../dms/dms-overview.md) is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure. Implementatie-opties op de openbare preview zijn onder meer databases in Azure SQL Database en SQL Server-databases in een Azure Virtual machines. DMS is de aanbevolen methode van de migratie voor workloads van uw onderneming.

Als u SQL Server Integration Services (SSIS) op uw SQL Server on-premises, DMS biedt nog geen ondersteuning voor migratie SSIS-catalogus (SSISDB) die SSIS-pakketten opslaat, maar u kunt Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) inrichten die wordt een nieuwe SSISDB in een beheerd exemplaar maken en vervolgens kunt u uw pakketten implementeren, Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

Zie voor meer informatie over dit scenario en configuratiestappen voor DMS [uw on-premises database migreren naar beheerd exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>Systeemeigen terugzetten vanuit URL

HERSTELLEN van systeemeigen back-ups (.bak-bestanden) die van SQL Server on-premises of [SQL Server op virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/), beschikbaar op [Azure Storage](https://azure.microsoft.com/services/storage/), is een van de belangrijkste mogelijkheden van de implementatie van het beheerde exemplaar de optie waarmee u snel en eenvoudig offline databasemigratie.

Het volgende diagram biedt een overzicht op hoog niveau van het proces:

![migratie-stroom](./media/sql-database-managed-instance-migration/migration-flow.png)

De volgende tabel vindt u meer informatie over de methoden die u kunt gebruiken, afhankelijk van de bron SQL Server-versie wordt uitgevoerd:

|Stap|SQL-Engine en versie|Back-up en herstellen van de methode|
|---|---|---|
|Back-up naar Azure Storage plaatsen|Eerdere SQL 2012 SP1 CU2|Uploaden van bestand met .bak rechtstreeks naar Azure storage|
||2012 SP1 CU2 - 2016|Directe back-up met afgeschaft [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) syntaxis|
||2016 en hoger|Directe back-up met [met SAS-REFERENTIES](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Herstellen van Azure-opslag naar beheerd exemplaar|[HERSTELLEN van URL met SAS-REFERENTIES](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Wanneer een database die is beveiligd door migreren [Transparent Data Encryption](transparent-data-encryption-azure-sql.md) naar een beheerd exemplaar met behulp van de optie voor systeemeigen terugzetten, het corresponderende certificaat uit het on-premises of IaaS SQL Server moet worden gemigreerd voordat de database herstellen. Zie voor gedetailleerde stappen [cert TDE migreren naar beheerd exemplaar](sql-database-managed-instance-migrate-tde-certificate.md)
> - Herstellen van systeemdatabases wordt niet ondersteund. Exemplaar niveau om objecten te migreren (opgeslagen in de basispagina of msdb-databases), wordt aangeraden deze scripts en T-SQL-scripts uitvoeren op de doel-exemplaar.

Zie voor een snelstart van het back-up van een database herstellen naar een beheerd exemplaar met behulp van een SAS-referentie [herstellen vanuit back-up naar een beheerd exemplaar](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]


## <a name="monitor-applications"></a>Toepassingen bewaken

Nadat u de migratie naar Managed Instance hebt voltooid, moet u de werking van de toepassing en de prestaties van uw workload bijhouden. Dit proces omvat de volgende activiteiten:
- [Prestaties van de werkbelasting wordt uitgevoerd op het beheerde exemplaar](#compare-performance-with-the-baseline) met de [basislijn van prestaties die u hebt gemaakt op de bron-SQL Server](#create-performance-baseline).
- Continu [bewaken van prestaties van uw workload](#monitor-performance) om potentiële problemen en verbetering van de gebruikerservaring te identificeren.

### <a name="compare-performance-with-the-baseline"></a>Prestaties met de basislijn vergelijken

De eerste activiteit die u gaat direct na de migratie is kunnen de prestaties van de werkbelasting met de basislijnprestaties van de werkbelasting. Het doel van deze activiteit is om te bevestigen dat de werkbelastingsprestaties op uw beheerde exemplaar aan uw behoeften voldoet. 

Databasemigratie naar Managed Instance is bedoeld om database-instellingen en de oorspronkelijke compatibiliteitsniveau in de meeste gevallen. De oorspronkelijke instellingen blijven behouden waar mogelijk om het risico van sommige prestaties degradations ten opzichte van de bron-SQL Server. Als het compatibiliteitsniveau van een gebruikersdatabase 100 of hoger vóór de migratie is, blijft ongewijzigd na de migratie. Als het compatibiliteitsniveau van een gebruikersdatabase 90 vóór de migratie in de bijgewerkte database is is het compatibiliteitsniveau ingesteld op 100, dit het laagste ondersteunde compatibiliteitsniveau in beheerd exemplaar is. Compatibiliteitsniveau van systeemdatabases is 140. Aangezien migreren naar Managed Instance feitelijk naar de nieuwste versie van SQL Server Database Engine migreert is, moet u rekening houden dat u wilt opnieuw test de prestaties van uw workload om sommige verrassende prestatieproblemen te voorkomen.

Als een vereiste, zorg dat u de volgende activiteiten hebt:
- Uw instellingen op Managed Instance met de instellingen van de bron-SQL Server-exemplaar in overeenstemming gebracht voor het onderzoeken van verschillende exemplaren, databases, temdb instellingen en configuraties. Zorg ervoor dat u instellingen, zoals compatibiliteitsniveaus of versleuteling niet hebt gewijzigd voordat u de prestaties van een vergelijking uitvoeren of accepteer het risico dat enkele van de nieuwe functies die u hebt ingeschakeld invloed zijn op enkele query's. Migratierisico, verminderen het databasecompatibiliteitsniveau alleen na het bewaking van toepassingsprestaties te wijzigen.
- Implementeer [opslag richtlijnen voor aanbevolen procedures voor algemeen gebruik](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525) zoals vooraf toewijzen van de grootte van de bestanden naar de betere prestaties.
- Meer informatie over de [omgeving verschillen die tot de prestatieverschillen tussen Managed Instance en SQL Server leiden-sleutel]( https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) en de risico's die mogelijk van invloed zijn op de prestaties te identificeren.
- Zorg ervoor dat u ingeschakelde Query Store en automatisch afstemmen op uw beheerde exemplaar houden. Deze functies kunt u voor het meten van prestaties van de werkbelastingen en automatisch de mogelijke prestatieproblemen oplossen. Leer hoe u Query Store als een optimale hulpprogramma voor het ophalen van informatie over de prestaties van de werkbelasting voor en na het niveau wijzigen voor de database-compatibiliteit, wordt gebruikt, zoals wordt beschreven [prestaties stabiliteit behouden tijdens de upgrade naar nieuwere versie van SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).
Nadat u hebt voorbereid op de omgeving die vergelijkbaar is zo veel mogelijk is met uw on-premises-omgeving, u kunt uw workload uitgevoerd en meten van prestaties. Proces van de meting moet bevatten de dezelfde parameters die u gemeten [bij het maken van basislijnprestaties van uw werkbelasting metingen op de bron-SQL Server](#create-performance-baseline).
Als gevolg hiervan moet u prestatieparameters met de basislijn voor vergelijken en identificeren van belangrijke verschillen.

> [!NOTE]
> In veel gevallen kunt zou u niet mogelijk om de exact overeenkomende prestaties op Managed Instance en SQL Server. Voor het beheerde exemplaar is een SQL Server-database-engine, maar infrastructuur en configuratie van hoge beschikbaarheid in het beheerde exemplaar kunnen leiden tot enige verschil. U zou verwachten dat sommige query's sneller zijn zou terwijl een ander mogelijk langzamer. Het doel van de vergelijking is om te controleren of de prestaties van de werkbelastingen in het beheerde exemplaar overeenkomt met de prestaties van SQL Server (in gemiddelde), en identificeren zijn er geen kritieke query's met de prestaties die niet overeenkomen met de prestaties van uw oorspronkelijke.

Het resultaat van de vergelijking van de prestaties mogelijk:
- Prestaties van de werkbelastingen op Managed Instance wordt uitgelijnd of beter dat de prestaties van de werkbelastingen op de SQL Server. U hebt in dit geval is vastgesteld dat de migratie voltooid is.
- Het merendeel van de prestatieparameters en de query's in de workload werken prima, met enkele uitzonderingen met verminderde prestaties. In dit geval moet u om de verschillen en het belang ervan te identificeren. Als er enkele belangrijke query's met verminderde prestaties zijn, moet u onderzoeken de onderliggende SQL-abonnementen worden gewijzigd of de query's de enige bron limiet hebt bereikt. Risicobeperking kan in dit geval worden toe te passen sommige hints op de kritieke query's (bijvoorbeeld gewijzigde compatibiliteitsniveau, verouderde kardinaliteitsschatter) hetzij rechtstreeks of met behulp van planrichtlijnen, opnieuw samenstellen of statistieken en indexen die invloed kunnen hebben op de abonnementen maken. 
- De meeste van de query's zijn langzamer zijn op Managed Instance in vergelijking met de bron-SQL Server. In dit geval probeert te identificeren van de belangrijkste oorzaken van het verschil zoals [sommige resourcelimiet bereikt]( sql-database-managed-instance-resource-limits.md#instance-level-resource-limits) zoals i/o-limieten, geheugenlimiet, exemplaar log-frequentielimiet, enzovoort. Als er geen resourcelimieten voor de die ertoe leiden dat het verschil zijn kunnen, probeert te wijzigen van het compatibiliteitsniveau van de database of de database-wijzigingsinstellingen, zoals verouderde kardinaliteitsschatting en de test opnieuw te starten. Bekijk de aanbevelingen voor Managed Instance of Query Store weergaven die voor het identificeren van de query's die prestaties is teruggedraaid.

> [!IMPORTANT]
> Beheerd exemplaar beschikt over ingebouwde automatische plan correctie-functie die standaard is ingeschakeld. Deze functie zorgt ervoor dat query's die goed zijn uitgevoerd in de plakken niet in de toekomst zou afnemen. Zorg ervoor dat deze functie is ingeschakeld en dat u de werkbelasting lang genoeg hebt uitgevoerd met de oude instellingen voordat u nieuwe instellingen wijzigen om in te schakelen beheerd exemplaar voor meer informatie over de basislijnprestaties en plannen.

De wijziging van de parameters of upgrade van service-lagen te convergeren naar de optimale configuratie, totdat u de prestaties van de werkbelastingen die past bij uw behoeften.

### <a name="monitor-performance"></a>Prestaties bewaken

Beheerd exemplaar biedt veel geavanceerde hulpprogramma's voor controle en probleemoplossing en moet u deze gebruiken voor de bewaking van prestaties van uw exemplaar. Sommige van de parameters die uw moet controleren zijn:
- CPU-gebruik op het exemplaar om te bepalen, is het aantal vCores op dat u hebt ingericht is de juiste overeenkomende reeks voor uw workload.
- Pagina-levensverwachting op uw beheerde exemplaar om te bepalen [moet u extra geheugen](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444).
- Wachtstatistieken zoals `INSTANCE_LOG_GOVERNOR` of `PAGEIOLATCH` vertelt die u hebt opslag i/o-problemen, met name op de categorie Algemeen gebruik waar u mogelijk het toewijzen van bestanden voor betere i/o-prestaties.

## <a name="leverage-advanced-paas-features"></a>Maak gebruik van geavanceerde functies voor PaaS

Zodra u zich op een volledig beheerd platform en u hebt gecontroleerd dat werkbelasting prestaties zijn die overeenkomen met u de prestaties van de SQL Server-werkbelastingen, neemt u de voordelen die automatisch worden geleverd als onderdeel van de service SQL Database. 

Zelfs als u geen enkele wijzigingen in beheerde exemplaar tijdens de migratie aanbrengen, zijn er hoge kans dat u op een aantal nieuwe functies inschakelen wilt terwijl u uw exemplaar als u wilt een profiteren van de meest recente verbeteringen van de database-engine werkt. Sommige wijzigingen zijn slechts eenmaal ingeschakeld de [databasecompatibiliteitsniveau is gewijzigd](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database).


Bijvoorbeeld, u hoeft te maken van back-ups op beheerde exemplaar: de service uitvoert back-ups voor u automatisch. U moet niet meer zorgen over de planning, nemen en het beheer van back-ups. Beheerd exemplaar biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaarperiode periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Bovendien hoeft niet te hoeven maken over het instellen van hoge beschikbaarheid als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Als u wilt de beveiliging versterken, kunt u overwegen [Azure Active Directory-verificatie](sql-database-security-overview.md), [controle](sql-database-managed-instance-auditing.md), [detectie van bedreigingen](sql-database-advanced-data-security.md), [beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), en [dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ).

Naast de geavanceerde beheer- en beveiligingsfuncties, Managed Instance biedt een set geavanceerde hulpprogramma's waarmee u [controleren en afstemmen van uw workload](sql-database-monitor-tune-overview.md). [Azure SQL analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) kunt u een groot aantal beheerde instanties bewaken en centraliseren bewaking van een groot aantal exemplaren en -databases. [Automatisch afstemmen](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) in het beheerde exemplaar continu bewaken van prestaties van uw SQL-plan tot uitvoering van statistieken en de van geïdentificeerde prestatieproblemen automatisch oplossen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaren [wat is er een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor een zelfstudie waarin een herstel vanuit back-up, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md).
- Zie voor een zelfstudie waarin migratie met behulp van DMS [uw on-premises database migreren naar beheerd exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
