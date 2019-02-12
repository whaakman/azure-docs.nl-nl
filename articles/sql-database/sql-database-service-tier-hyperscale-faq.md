---
title: Azure SQL Database grootschalige Veelgestelde vragen over | Microsoft Docs
description: Antwoorden op algemene vragen klanten vragen over een Azure SQL-database in de servicelaag grootschalige - vaak een grote database met de naam.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/06/2019
ms.openlocfilehash: 16d536ff5bd1f7d9a3427877a93036d9f711564e
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099770"
---
# <a name="faq-about-azure-sql-hyperscale-databases"></a>Veelgestelde vragen over zeer grootschalige Azure-SQL-databases

In dit artikel vindt u antwoorden op veelgestelde vragen voor klanten die u overweegt een database in de zeer grootschalige Azure SQL Database-servicelaag, vaak een grote database met de naam (momenteel in openbare preview-versie). In dit artikel beschrijft de scenario's die ondersteuning biedt voor grootschalig en de cross-functie-services zijn compatibel met SQL Database grootschalige in het algemeen.

- Deze Veelgestelde vragen is bedoeld voor lezers die een korte begrip van de servicelaag grootschalige hebt en wilt hebben hun specifieke vragen en problemen beantwoord.
- Deze Veelgestelde vragen is niet bedoeld om te worden van een gids voor paraatheid of antwoorden op vragen over het gebruik van een SQL-Database grootschalige-database. Voor die, raden we u verwijzen naar de [Azure SQL Database grootschalige](sql-database-service-tier-hyperscale.md) documentatie.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-a-hyperscale-database"></a>Wat is een zeer grootschalige-database

Een zeer grootschalige-database is een Azure SQL-database in de servicelaag van grootschalige die wordt ondersteund door de scale-out opslag grootschalig technologie. Een grote database biedt ondersteuning voor maximaal 100 TB aan gegevens en biedt hoge doorvoer en prestaties, evenals snelle schaling aan te passen aan de vereisten voor de werkbelasting. Schalen is transparant voor de toepassing – connectiviteit, verwerking van query's, enzovoort, werk, zoals een andere SQL-database.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Welke resourcetypen en aankopen modellen ondersteunen grootschalige

De servicelaag grootschalige is alleen beschikbaar voor individuele databases met behulp van het op vCore gebaseerde aankoopmodel in Azure SQL Database.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Hoe verschilt de grootschalige servicelaag van de Servicelagen voor algemeen gebruik en bedrijfskritiek

Het vCore-gebaseerde service lagen voornamelijk worden onderscheiden, is afhankelijk van beschikbaarheid, opslagtype en IOP's.

- De categorie Algemeen gebruik-service is geschikt voor de meeste zakelijke workloads, biedt een set met gelijke taakverdeling van reken- en opties voor i/o-latentie of failover-tijden wanneer niet de prioriteit zijn.
- De servicelaag grootschalige is geoptimaliseerd voor zeer grote databaseworkloads.
- De laag bedrijfskritiek-service is geschikt voor workloads van bedrijven waar i/o-latentie is een prioriteit.

| | Resourcetype | Algemeen doel |  Hyperscale | Bedrijfskritiek |
|:---|:---:|:---:|:---:|:---:|:---:|
| **Het meest geschikt voor** |Alle|  Meeste zakelijke workloads. Aanbiedingen budget documentgeoriënteerde met gelijke taakverdeling reken- en opties. | Data-toepassingen met capaciteitsvereisten voor grote hoeveelheden gegevens en de mogelijkheid om opslag van automatisch schalen en schaal compute soepel. | OLTP-toepassingen met hoge Transactiesnelheid en de laagste latentie i/o. Biedt de hoogste veerkracht tijdens storingen met behulp van verschillende geïsoleerde replica's.|
|  **Resourcetype** ||Individuele database / elastische pool / beheerd exemplaar | Individuele database | Individuele database / elastische pool / beheerd exemplaar |
| **COMPUTE-grootte**|Individuele database / elastische pool * | 1 en 80 vCores | 1 en 80 vCores * | 1 en 80 vCores |
| |Beheerd exemplaar | 8, 16, 24 uur per dag, 32, 40, 64, 80 vCores | N/A | 8, 16, 24 uur per dag, 32, 40, 64, 80 vCores |
| **Opslagtype** | Alle |Externe Premium-opslag (per exemplaar) | Ongedaan maken gekoppelde opslag met lokale SSD-cache (per exemplaar) | Zeer snelle lokale SSD-opslag (per exemplaar) |
| **Opslaggrootte** | Individuele database / elastische pool | 5 GB – 4 TB | Maximaal 100 TB | 5 GB – 4 TB |
| | Beheerd exemplaar  | 32 GB – 8 TB | N/A | 32 GB – 4 TB |
| **I/o-doorvoer** | Één database ** | 500 IOP's per vCore met 7000 maximale IOPS | Nog onbekend | 5000 IOP's met 200.000 maximale IOPS|
| | Beheerd exemplaar | Afhankelijk van de grootte van bestand | N/A | Beheerd exemplaar: Afhankelijk van de grootte van bestand|
|**Beschikbaarheid**|Alle|1 replica, geen leesschaal, geen lokale cache | Meerdere replica's, maximaal 15 leesschaal, gedeeltelijke lokale cache | 3 replica's, 1 leesschaal, zone-redundante HA, volledige lokale cache |
|**Back-ups**|Alle|RA-GRS, 7 en 35 dagen (7 dagen standaard)| RA-GRS, 7 en 35 dagen (7 dagen standaard), constante tijd punt in0time recovery (PITR) | RA-GRS, 7 en 35 dagen (7 dagen standaard) |

\* Elastische pools niet ondersteund in de servicelaag grootschalige

### <a name="who-should-use-the-hyperscale-service-tier"></a>Wie moet de servicelaag grootschalige gebruiken

De servicelaag van grootschalige is voornamelijk bedoeld voor klanten die hebben grote on-premises SQL Server-databases en willen hun toepassingen moderniseren door naar de cloud te verplaatsen of voor klanten die al gebruikmaken van Azure SQL Database en wilt aanzienlijk uitbreiden de mogelijkheden voor databasegroei van de. Zeer grootschalige is ook bedoeld voor klanten die hoge prestaties en hoge schaalbaarheid zoeken. Met zeer grootschalige profiteert u van:

- Ondersteuning voor maximaal 100 TB grootte van de database
- Snelle database back-ups, ongeacht de grootte van de database (back-ups zijn gebaseerd op bestand momentopnamen)
- Snelle database wordt hersteld, ongeacht de grootte van de database (uit bestand momentopnamen zijn herstelbewerkingen)
- Hogere log doorvoer resulteert in snelle transactie doorvoeren tijden, ongeacht de grootte van de database
- Read scale-out op een of meer alleen-lezen knooppunten voor de offload van uw workload voor lezen en voor ' hot '-stand-bys.
- Snelle omhoog schalen van compute, in de constante tijd moet krachtigere om te voorzien in de zware werkbelasting en schaal omlaag, constante tijdstip. Dit is vergelijkbaar met omhoog en omlaag schalen tussen een P6 naar een P11, bijvoorbeeld, maar veel sneller als dit is niet een grootte van gegevens.

### <a name="what-regions-currently-support-hyperscale"></a>Welke regio's ondersteunen momenteel zeer grootschalige

Zeer grootschalige is momenteel beschikbaar voor individuele databases in de volgende regio's:  West US1, West vs2, US1 VS-Oost, VS-midden, West-Europa, Noord-Europa, UK-West, Zuidoost-Azië, Japan-Oost, Korea-centraal, Australië-Zuidoost en Australië-Oost.

### <a name="can-i-create-multiple-hyperscale-databases-per-sql-database-server"></a>Ik kan meerdere grootschalige databases per SQL-databaseserver maken

Ja. Zie voor meer informatie en beperkingen met betrekking tot het aantal databases per SQL-databaseserver grootschalige [SQL Database-resourcebeperkingen voor één en gepoolde databases op een SQL-databaseserver](sql-database-resource-limits-database-server.md).

### <a name="what-are-the-performance-characteristic-of-a-hyperscale-database"></a>Wat zijn de prestatiekenmerk van een grootschalige-database

De SQL-Database grootschalige architectuur biedt hoge prestaties en doorvoer ondersteuning te bieden voor grote databases. De precieze prestatieprofiel en -kenmerken is niet beschikbaar tijdens de openbare preview.

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Wat is de schaalbaarheid van een grootschalige-database

SQL Database grootschalige biedt snelle schaalbaarheid op basis van de vraag van uw workload.

- **Omhoog/omlaag schalen**

  U kunt met zeer grootschalige, schalen van de grootte van de primaire compute in termen van resources, zoals CPU, geheugen en schaal omlaag, constante tijdstip. Omdat de opslag wordt gedeeld, omhoog en omlaag schalen is niet een grootte van gegevens.  
- **Schalen In/uit**

  Met zeer grootschalige, moet u ook de mogelijkheid voor het inrichten van een of meer extra rekenknooppunten die u gebruiken kunt voor het bieden van uw leesaanvragen ophalen. Dit betekent dat u deze extra compute-knooppunten kunt gebruiken als alleen-lezen-knooppunten voor de offload van uw workload lezen van de primaire Computing. In aanvulling om alleen-lezen, dat deze knooppunten ook dienen als hot stand-by-de van in het geval van een failover via van de primaire.

  Inrichting van elk van deze extra compute-knooppunten kunnen worden gedaan in de constante tijd en is een online-bewerking. U kunt verbinding maken met deze aanvullende alleen-lezen rekenknooppunten door in te stellen de `ApplicationIntent` argument op de verbindingsreeks naar `read_only`. Alle verbindingen die zijn gemarkeerd met `read-only` worden automatisch doorgestuurd naar een van de aanvullende alleen-lezen-compute-knooppunten.

## <a name="deep-dive-questions"></a>Duik de diepte vragen

### <a name="can-i-mix-hyperscale-and-single-databases-a-my-sql-database-server"></a>Kan ik grootschalig en individuele databases combineren een mijn SQL Database-server

Ja, dat is mogelijk.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Vereist grootschalige mijn toepassing-programmeermodel om te wijzigen

Nee, blijft het programmeermodel van uw toepassing is. U gebruikt de verbindingsreeks als normaal en de normale modus om te communiceren met uw Azure SQL database.

### <a name="what-transaction-isolation-levels-are-going-to-be-default-on-sql-database-hyperscale-database"></a>Welke transactieniveaus voor isolatie gebruikt gaan worden standaard op Hyperschaal voor SQL Database-database

Op het primaire knooppunt wordt de transactie-isolatieniveau RCSI (lezen doorgevoerd Snapshot-isolatie). Op de secundaire knooppunten leesbewerkingen wordt het isolatieniveau Snapshot.

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-sql-database-hyperscale"></a>Ik kan mijn on-premises of IaaS SQL Server-licentie overbrengen naar SQL Database grootschalige

Ja, [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) is beschikbaar voor grootschalige. Elke SQL Server Standard kern kunt toewijzen aan 1 grootschalige vCores. Elke SQL Server Enterprise-kern kunt toewijzen aan 4 zeer grootschalige vCores. U hoeft geen SQL-licentie voor secundaire replica's. De prijs van Azure Hybrid Benefit wordt automatisch worden toegepast op leesschaal (secundair) replica's.

### <a name="what-kind-of-workloads-is-sql-database-hyperscale-designed-for"></a>Wat voor soort workloads is SQL Database grootschalige ontworpen voor

SQL Database grootschalige biedt ondersteuning voor alle SQL Server-workloads, maar is voornamelijk geoptimaliseerd voor OLTP. U kunt hybride en analytische (datamart) werkbelastingen voor brengen.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-sql-database-hyperscale"></a>Hoe kan ik kiezen tussen Azure SQL Data Warehouse en zeer grootschalige SQL-Database

Als u momenteel uitvoeren van interactieve analyses query's met behulp van SQL Server als een datawarehouse grootschalig voor SQL-Database is een goede optie omdat u relatief klein datawarehouses (zoals een paar TB tot 10's van TB hosten kunt) tegen een lagere kosten en kunt u uw gegevens migreren  datawarehouse-workload naar SQL Database grootschalige zonder codewijzigingen T-SQL.

Als u een data-analyse uitvoeren op grote schaal met complexe query's en via Parallel Data Warehouse (PDW), Teradata of andere Massively Parallel-Processor (MPP)), SQL Data Warehouse voor datawarehouses mogelijk de beste keuze.
  
## <a name="sql-database-hyperscale-compute-questions"></a>SQL Database grootschalige compute vragen

### <a name="can-i-pause-my-compute-at-any-time"></a>Ik kan mijn compute op elk gewenst moment onderbreken

Nee.

### <a name="can-i-provision-a-compute-with-extra-ram-for-my-memory-intensive-workload"></a>Ik kan een Computing met extra RAM-geheugen voor mijn werkbelasting geheugenintensieve inrichten

Nee. Als u meer RAM-geheugen, moet u een upgrade uitvoert naar een hogere compute-grootte. Gen4 hardware biedt meer RAM-geheugen in vergelijking met Gen5 hardware. Zie voor meer informatie, [grootschalige opslag- en rekencapaciteit grootten](sql-database-vcore-resource-limits-single-databases.md#hyperscale-service-tier-preview).

### <a name="can-i-provision-multiple-compute-nodes-of-different-sizes"></a>Ik kan meerdere compute-knooppunten van verschillende grootten inrichten

Nee.

### <a name="how-many-read-scale-replicas-are-supported"></a>Hoeveel leesschaal replica's worden ondersteund

In openbare preview-versie, worden de databases grootschalige standaard gemaakt met één leesschaal replica (twee replica's in totaal). Vul een ondersteuningsaanvraag via Azure portal als u wilt toevoegen of verwijderen van leesschaal replica's.

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-nodes"></a>Voor hoge beschikbaarheid, ik nodig om in te richten als u meer rekenknooppunten

In grote databases, wordt de hoge beschikbaarheid op het opslagniveau verstrekt. U hoeft slechts één replica voor een hoge beschikbaarheid. Wanneer de compute-replica niet actief is, wordt automatisch een nieuwe replica gemaakt met zonder verlies van gegevens.

Echter, als er slechts één replica, het duurt enige tijd om te maken van de lokale cache in de nieuwe replica na een failover. Tijdens de fase cache opnieuw is gebouwd haalt de-database gegevens rechtstreeks vanuit de pagina servers, leidt tot verminderde prestaties van IOP's en query's uitvoeren.

Voor bedrijfskritische apps die hoge beschikbaarheid nodig hebt, moet u ten minste 2 compute-knooppunten, met inbegrip van de primaire compute-knooppunt (standaard) inrichten. Op die manier is er een hot stand-by-beschikbaar in het geval van een failover.

## <a name="data-size-and-storage-questions"></a>Vragen over de grootte en de opslag van gegevens

### <a name="what-is-the-max-db-size-supported-with-sql-database-hyperscale"></a>Wat is de maximale databasegrootte ondersteund met SQL Database grootschalige

100 TB

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Wat is de grootte van het transactielogboek met grootschalige

Het transactielogboek met grootschalige is vrijwel oneindig. U hoeft niet te hoeven maken over het uitvoeren van de logboekruimte op een systeem met een hoge log-doorvoer. De snelheid voor het genereren van log kan echter worden beperkt voor continue agressief werkbelastingen. De maximale en gemiddelde Meld u generatie tarief is nog niet bekend zijn (nog steeds in Preview-versie).

### <a name="does-my-temp-db-scale-as-my-database-grows"></a>Mijn tijdelijke db schalen als mijn database groeit

Uw `tempdb` database bevindt zich op lokale SSD-opslag en is geconfigureerd op basis van de compute-grootte die u inricht. Uw `tempdb` is geoptimaliseerd en lay-out en biedt voordelen voor maximale prestaties. De `tempdb` grootte kan niet worden geconfigureerd en u wordt beheerd door de onderliggende opslagsysteem.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-the-data-files"></a>Wordt mijn grootte van de database automatisch vergroten of heb ik voor het beheren van de grootte van de gegevensbestanden

De databasegrootte van uw is automatisch neemt toe naarmate u invoegen/meer gegevens opnemen.

### <a name="what-is-the-smallest-database-size-that-sql-database-hyperscale-supports-or-starts-with"></a>Wat is de kleinste grootte van de database die zeer grootschalige SQL-Database ondersteunt of begint met

5 GB

### <a name="in-what-increments-does-my-database-size-grow"></a>In welke stappen de grootte van mijn database vergroten

1 GB

### <a name="is-the-storage-in-sql-database-hyperscale-local-or-remote"></a>Is de opslag in SQL Database grootschalige lokale of externe

In zeer grootschalige, worden gegevensbestanden opgeslagen in Azure standard-opslag. Gegevens is sterk in cache opgeslagen op lokale SSD-opslag, op virtuele machines dicht bij de rekenknooppunten. Compute-knooppunten hebben bovendien een cache op de lokale SSD en in-memory (buffergroep, enzovoort), te verminderen van de frequentie van de gegevens worden opgehaald van externe knooppunten.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Ik kan beheren of bestanden of bestandsgroepen met grootschalige definiëren

Nee
  
### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Ik kan een vaste bovengrens voor de groei van gegevens voor mijn database inrichten

Nee

### <a name="how-are-data-files-laid-out-with-sql-database-hyperscale"></a>Hoe worden gegevensbestanden lay-out met SQL Database grootschalige

De gegevensbestanden worden beheerd door de pagina servers. Als de grootte van de groeit, zijn de gegevensbestanden en bijbehorende pagina knooppunten worden toegevoegd.

### <a name="is-database-shrink-supported"></a>Verkleind database wordt ondersteund

Nee

### <a name="is-database-compression-supported"></a>Wordt de database compressie ondersteund

Ja

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Als ik een grote tabel hebt, Mijn tabelgegevens ophalen verspreid over meerdere gegevensbestanden

Ja. De data-pagina's die zijn gekoppeld aan een bepaalde tabel kunnen in meerdere gegevensbestanden, die allemaal deel uit van de dezelfde bestandsgroep terechtkomen. SQL Server gebruikmaakt van een [strategie voor evenredige opvulling](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) voor distributie van gegevens over gegevensbestanden.

## <a name="data-migration-questions"></a>Vragen over de migratie van gegevens

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>Ik kan mijn bestaande Azure SQL-databases verplaatsen naar de servicelaag grootschalige

Ja. U kunt uw bestaande Azure SQL-databases verplaatsen naar grootschalige. Dit is een eenrichtingsvertrouwensrelatie migratie in openbare preview. U kunt databases van grootschalige niet verplaatsen naar een andere servicelaag. U wordt aangeraden een kopie van uw productiedatabases en migreren naar grootschalige voor het testen van concepten (Proefverzamelingen).
  
### <a name="can-i-move-my-hyperscale-databases-to-other-editions"></a>Ik kan mijn grootschalige databases verplaatsen naar andere edities

Nee. In openbare preview, kunt u een grote database niet verplaatsen naar een andere servicelaag.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Ik alle functionaliteit en mogelijkheden na de migratie naar de servicelaag grootschalige verlies

Ja. Sommige functies van Azure SQL Database worden niet ondersteund in grootschalige tijdens de openbare preview, inclusief maar niet beperkt tot de TDE en lange termijn bewaren back-up. Nadat u uw databases naar grootschalige migreren, deze functies werken niet meer.

### <a name="can-i-move-my--on-premises-sql-server-database-or-my-sql-server-virtual-machine-database-to-hyperscale"></a>Ik kan mijn on-premises SQL Server-database of de SQL Server-database voor virtuele machine verplaatsen naar grootschalige

Ja. U kunt alle bestaande technologieën voor migratie gebruiken om te migreren naar grootschalige, met inbegrip van BACPAC, transactionele replicatie, logische het laden van gegevens. Zie ook de [Azure Database migratieservice](../dms/dms-overview.md).

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Wat is mijn uitvaltijd tijdens de migratie van een on-premises of virtuele-machineomgeving op Hyperschaal en hoe kan ik het te minimaliseren

Uitvaltijd is hetzelfde als de downtime wanneer u uw databases naar een individuele database in Azure SQL Database migreert. U kunt [transactionele replicatie](replication-to-sql-database.md#data-migration-scenario
) migratie voor databases tot enkele TB in grootte downtime minimaliseren. Voor zeer grote database (10 + TB), kunt u overwegen om gegevens met ADF, Spark of met andere technologieën voor het verplaatsen van gegevens te migreren.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-sql-database-hyperscale"></a>Hoeveel tijd het zou nemen het binnenhalen van X hoeveelheid gegevens naar SQL Database grootschalige

Nog niet bekend (nog steeds in de Preview-versie)

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-and-sql-data-warehouse"></a>Kan ik lezen gegevens uit blob-opslag en snel load (zoals Polybase en SQL Data Warehouse)

U kunt gegevens lezen uit Azure Storage en laden van gegevens te laden in een grootschalige-database (net zoals u met een reguliere individuele database doen kunt). Polybase is momenteel niet ondersteund op Azure SQL Database. U kunt doen met behulp van Polybase [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) of het uitvoeren van een Spark-taak in [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) met de [Spark-connector voor SQL](sql-database-spark-connector.md). De Spark-connector naar SQL biedt ondersteuning voor bulksgewijs invoegen.

Eenvoudig herstel of bulksgewijs logboekregistratie model wordt niet ondersteund in grootschalige. Model voor volledig herstel is vereist voor een hoge beschikbaarheid. Zeer grootschalige biedt echter een betere gegevensopname tarief ten opzichte van een individuele database vanwege de nieuwe log-architectuur.

### <a name="does-sql-database-hyperscale-allow-provisioning-multiple-nodes-for-ingesting-large-amounts-of-data"></a>SQL Database grootschalige staat inrichting van meerdere knooppunten voor het opnemen van grote hoeveelheden gegevens

Nee. Zeer grootschalige SQL-Database is een SMP-architectuur en is niet een asymmetrische multiprocessing of een architectuur met meerdere masters. U kunt alleen meerdere replica's uitschalen alleen-lezen-workloads maken.

### <a name="what-is-the-oldest-sql-server-version-will-sql-database-hyperscale-support-migration-from"></a>Wat is de oudste SQL-Server versie wordt ondersteuning voor migratie van SQL Database grootschalige

SQL Server 2005. Zie voor meer informatie, [migreren naar een enkele database of een gegroepeerde database](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database). Zie voor compatibiliteitsproblemen, [het oplossen van compatibiliteitsproblemen met databasemigratie](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues).

### <a name="does-sql-database-hyperscale-support-migration-from-other-data-sources-such-as-aurora-mysql-oracle-db2-and-other-database-platforms"></a>SQL Database grootschalige biedt ondersteuning voor migratie van andere gegevensbronnen, zoals Aurora, MySQL, Oracle, DB2 en andere databaseplatforms

Ja. SQL Server die afkomstig zijn van andere gegevensbronnen dan is logische migratie vereist. U kunt de [Azure Database Migration Service](../dms/dms-overview.md) voor een logische-migratie.

## <a name="business-continuity-and-disaster-recovery-questions"></a>Zakelijke continuïteit en noodherstel recovery vragen

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Wat SLA's zijn opgegeven voor een grote database

In het algemeen wordt geen Sla tijdens de openbare preview. Zeer grootschalige biedt echter dezelfde mate van hoge beschikbaarheid met aanbiedingen die momenteel SQL-database. Zie [SLA](https://azure.microsoft.com/support/legal/sla/).

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>De databaseback-ups worden beheerd voor mij door de service Azure SQL Database

Ja

### <a name="how-often-are-the-database-backups-taken"></a>Hoe vaak kan ik de databaseback-ups gemaakt

Er zijn geen traditionele volledige, differentiële en logboekback-ups voor databases die zeer grootschalige SQL-Database. In plaats daarvan er worden regelmatig momentopnamen van de gegevensbestanden en logboek dat wordt gegenereerd is gewoon bewaard als voor de bewaarperiode geconfigureerd of voor u beschikbaar is.

### <a name="does-sql-database-hyperscale-support-point-in-time-restore"></a>Wijst ondersteuning voor SQL Database grootschalige in Time Restore

Ja

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-with-backuprestore-in-sql-database-hyperscale"></a>Wat is de Recovery Point Objective (RPO) / Recovery Time Objective (RTO) met back-up/herstel in SQL Database grootschalige

De RPO is 0 min. Het doel RTO is minder dan 10 minuten, ongeacht de grootte van de database. Tijdens de openbare preview kan u echter langer hersteltijd optreden.

### <a name="do-backups-of-large-databases-affect-compute-performance-on-my-primary"></a>Back-ups van grote databases invloed hebben op prestaties op mijn primaire

Nee. Back-ups worden beheerd door het opslagsubsysteem en gebruikmaken van momentopnamen van het bestand. Ze doen geen invloed op de werkbelasting van de gebruiker op de primaire.

### <a name="can-i-perform-geo-restore-with-a-sql-database-hyperscale-database"></a>Ik kan geo-herstel met een SQL-Database grootschalige database uitvoeren

Nee, niet tijdens de openbare preview.

### <a name="can-i-setup-geo-replication-with-sql-database-hyperscale-database"></a>Ik kan Geo-replicatie instellen met SQL Database grootschalige database

Nee, niet tijdens de openbare preview.

### <a name="do-my-secondary-compute-nodes-get-geo-replicated-with-sql-database-hyperscale"></a>Mijn secundaire rekenknooppunten krijg geo-replicatie met SQL Database grootschalige

Nee, niet tijdens de openbare preview.

### <a name="can-i-take-a-sql-database-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-sql-server-in-vm"></a>Ik kan zet de database van een SQL-Database grootschalige back-up en mijn on-premises server of SQL Server in virtuele machine herstellen

Nee. De opslagindeling voor grote databases verschilt van traditionele SQL Server, en u geen back-ups beheren of hier toegang toe hebben. Als u uw gegevens uit de database van een SQL-Database zeer grootschalige, gebruikt u de export-service of scripts plus BCP gebruiken.

## <a name="cross-feature-questions"></a>Cross-functie vragen

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Ik alle functionaliteit en mogelijkheden na de migratie naar de servicelaag grootschalige verlies

Ja. Sommige functies van Azure SQL Database worden niet ondersteund in grootschalige tijdens de openbare preview, inclusief maar niet beperkt tot de TDE en lange termijn bewaren back-up. Nadat u uw databases naar grootschalige migreren, deze functies werken niet meer.

### <a name="will-polybase-work-with-sql-database-hyperscale"></a>Will Polybase in combinatie met SQL Database grootschalige

Nee. Polybase wordt niet ondersteund op Azure SQL Database.

### <a name="does-the-compute-have-support-for-r-and-python"></a>Beschikt over de compute-ondersteuning voor R en python

Nee. R en Python worden niet ondersteund in Azure SQL Database.

### <a name="are-the-compute-nodes-containerized"></a>Zijn de rekenknooppunten opgenomen in een container

Nee. Uw database zich bevindt op een compute-VM en niet een container.

## <a name="performance-questions"></a>Vragen over prestaties

### <a name="how-much-throughput-can-i-push-on-the-largest-sql-database-hyperscale-compute"></a>Hoeveel doorvoer kan ik push in de berekening van de grootste zeer grootschalige SQL-Database

Nog niet bekend (nog steeds in de Preview-versie)

### <a name="how-many-iops-do-i-get-on-the-largest-sql-database-hyperscale-compute"></a>Het aantal IOPS krijg ik in de berekening van de grootste zeer grootschalige SQL-Database

Nog niet bekend (nog steeds in de Preview-versie)

### <a name="does-my-throughput-get-affected-by-backups"></a>Mijn doorvoer ophalen beïnvloed door back-ups

Nee. COMPUTE is losgekoppeld van de opslaglaag impact op compute te voorkomen.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-nodes"></a>Mijn doorvoer krijgen ook van invloed kan ik aanvullende rekenknooppunten inrichten

Omdat de opslag wordt gedeeld en er is geen directe fysieke replicatie vindt plaats tussen primaire en secundaire rekenknooppunten in technisch opzicht kunnen worden de doorvoer voor het primaire knooppunt beïnvloed door leesschaal knooppunten toe te voegen. Echter, we continue agressief werkbelasting zodat log kan beperken toe te passen op secundaire knooppunten en pagina-servers voor meer informatie en onjuiste leesprestaties op secundaire knooppunten voorkomen.

## <a name="scalability-questions"></a>Vragen over schaalbaarheid

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-node"></a>Hoe lang duurt het op schaal omhoog en omlaag een compute-knooppunt

Enkele minuten

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Mijn database offline is, terwijl de schaal omhoog/omlaag bewerking uitgevoerd wordt

Nee. De schaal is omhoog en omlaag online.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Kan ik verwachten verbinding neerzetten wanneer de vergroten / verkleinen uitgevoerd worden

Schaal omhoog of omlaag resulteert in een bestaande verbindingen wanneer failover gebeurt er met de compute-knooppunt met de doelgrootte wordt verwijderd. Toevoegen van meer replica's resulteert niet in de verbinding wordt verbroken.

### <a name="is-the-scaling-up-and-down-of-compute-nodes-automatic-or-end-user-triggered-operation"></a>Is het omhoog en omlaag van compute-knooppunten automatisch of door eindgebruikers bewerking geactiveerd

Door eindgebruikers. Niet automatisch.  

### <a name="does-my-tempb-also-grow-as-the-compute-is-scaled-up"></a>Wordt mijn `tempb` ook groeien zolang er aan de rekenresources omhoog wordt geschaald

Ja. Tijdelijke db wordt automatisch omhoog schalen wanneer de compute groeit.  

### <a name="can-i-provision-multiple-primary-computes-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Ik kan meerdere primaire berekeningen zoals een meerdere masters systeem waarop meerdere primaire compute-koppen een hogere mate van gelijktijdigheid kunt station inrichten

Nee. Alleen het primaire knooppunt lezen/schrijven-aanvragen accepteert. Secundaire rekenknooppunten accepteert alleen alleen-lezen aanvragen.

## <a name="read-scale-questions"></a>Vragen over het schalen van lezen

### <a name="how-many-secondary-compute-nodes-can-i-provision"></a>Het aantal secundaire compute-knooppunten kan ik inrichten

In openbare preview-versie maken we 2 replica's voor grootschalig databases standaard. Vul een ondersteuningsaanvraag via Azure portal als u het aantal replica's aanpassen wilt.

### <a name="how-do-i-connect-to-these-secondary-compute-nodes"></a>Hoe maak ik verbinding met deze secundaire rekenknooppunten

U kunt verbinding maken met deze aanvullende alleen-lezen rekenknooppunten door in te stellen de `ApplicationIntent` argument op de verbindingsreeks naar `read_only`. Alle verbindingen die zijn gemarkeerd met `read-only` worden automatisch doorgestuurd naar een van de aanvullende alleen-lezen-compute-knooppunten.  

### <a name="can-i-create-a-dedicated-endpoint-for-the-read-scale-replica"></a>Ik kan een speciaal eindpunt voor de replica leesschaal maken

Nee. In de openbare preview, u kunt alleen verbinding maken met leesschaal replica door op te geven `ApplicationIntent=ReadOnly`.

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Het systeem doet intelligente taakverdeling van de workload voor lezen

Nee. Preview-versie is de werkbelasting voor lezen alleen opnieuw gerichte naar een willekeurige leesschaal-replica.

### <a name="can-i-scale-updown-the-secondary-compute-nodes-independently-of-the-primary-compute"></a>Ik kan omhoog/omlaag de secundaire rekenknooppunten onafhankelijk van de primaire compute schalen

Nee, niet tijdens de openbare preview.

### <a name="do-i-get-different-temp-db-sizing-for-my-primary-compute-and-my-additional-secondary-compute-nodes"></a>Krijg ik verschillende tijdelijke db-grootte voor mijn primaire reken- en Mijn aanvullende secundaire rekenknooppunten

Nee. Uw `tempdb` is geconfigureerd op basis van de inrichting van de compute grootte, tijdens de openbare preview, zijn uw secundaire rekenknooppunten even groot is als de primaire compute.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-nodes"></a>Kan ik de indexen en weergaven op mijn secundaire rekenknooppunten toevoegen

Nee. Zeer grootschalige databases gedeelde opslag, wat betekent dat alle compute-knooppunten, Zie de dezelfde tabellen, indexen en weergaven. Als u wilt dat aanvullende indexen die zijn geoptimaliseerd voor leesbewerkingen op secundaire server: u moet deze toevoegen op de primaire eerst.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-node"></a>Hoeveel vertraging is er nog tussen de primaire en secundaire compute-knooppunt

Vanaf het moment dat een transactie wordt doorgevoerd op de primaire, afhankelijk van de snelheid voor het genereren van logboek kan ofwel zijn onmiddellijk of in lage milliseconden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de servicelaag van grootschalige, [grootschalige servicelaag (preview)](sql-database-service-tier-hyperscale.md).
