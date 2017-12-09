---
title: Migratie van de SQL Server-database naar Azure SQL Database | Microsoft Docs
description: Lees meer over SQL Server-databasemigratie naar Azure SQL Database in de cloud.
keywords: databasemigratie, sql server-databasemigratie, hulpprogramma's voor databasemigratie, database migreren, sql-database migreren
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 11/07/2017
ms.author: carlrab
ms.openlocfilehash: 8a31ed948fe9387720db61018e0edded530cd900
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server-databasemigratie naar SQL Database in de cloud
In dit artikel leest u over de twee primaire methoden voor het migreren van een SQL Server 2005 of latere database naar Azure SQL Database. De eerste methode is eenvoudiger, maar leidt wel tot enige uitvaltijd tijdens de migratie. In bepaalde gevallen kan deze uitvaltijd aanzienlijk zijn. De tweede methode is complexer, maar veroorzaakt tijdens de migratie veel minder uitvaltijd.

In beide gevallen moet u ervoor zorgen dat de brondatabase compatibel met Azure SQL Database met behulp van is de [gegevens migratie-assistent (DMA)](https://www.microsoft.com/download/details.aspx?id=53595). SQL Database V12 nadert [functie pariteit](sql-database-features.md) met SQL Server dan problemen met het niveau van de server en meerdere databases. Bij databases en toepassingen die afhankelijk zijn van [slechts gedeeltelijk of in het geheel niet ondersteunde functies](sql-database-transact-sql-information.md), is een zekere mate van [herstructurering vereist om compatibiliteitsproblemen](sql-database-cloud-migrate.md#resolving-database-migration-compatibility-issues) op te lossen voordat de SQL Server-database kan worden gemigreerd.

> [!NOTE]
> Voor het migreren van een SQL Server-database, met inbegrip van Microsoft Access, Sybase, MySQL Oracle en DB2, naar Azure SQL Database raadpleegt u [SQL Server Migration Assistant](https://blogs.msdn.microsoft.com/datamigration/2017/09/29/release-sql-server-migration-assistant-ssma-v7-6/) (Migratieassistent voor SQL Server).
> 

## <a name="method-1-migration-with-downtime-during-the-migration"></a>Methode 1: Migratie die gepaard gaat met uitvaltijd

 Gebruik deze methode als u zich enige uitvaltijd kunt permitteren of als u met het oog op een latere migratie een testmigratie van een productiedatabase uitvoert. Zie voor een zelfstudie [migreren van een SQL Server-database](sql-database-migrate-your-sql-server-database.md).

De volgende lijst bevat de algemene werkstroom voor de migratie van een SQL Server-database met deze methode.

  ![Diagram van VSSSDT-migratie](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. [Beoordelen](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) de database voor compatibiliteit met behulp van de nieuwste versie van de [gegevens migratie-assistent (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).
2. Bereid alle benodigde fixes voor als Transact-SQL-scripts.
3. Maak een transactioneel consistent kopie van de brondatabase wordt gemigreerd - en zorg ervoor dat er geen verdere wijzigingen worden aangebracht voor de database (of kunt u deze wijzigingen handmatig toepassen nadat de migratie is voltooid). Er zijn veel methoden om een database stil te leggen, van het uitschakelen van de clientconnectiviteit tot het maken van een [databasemomentopname](https://msdn.microsoft.com/library/ms175876.aspx).
4. Implementeer de Transact-SQL-scripts om de fixes toe te passen op de databasekopie.
5. [Migreren](https://docs.microsoft.com/sql/dma/dma-migrateonpremsql) het database-exemplaar naar een nieuwe Azure SQL Database met behulp van de gegevens migratie-assistent.

### <a name="optimizing-data-transfer-performance-during-migration"></a>De prestaties van de gegevensoverdracht tijdens de migratie optimaliseren 

Hieronder leest u enkele aanbevelingen voor het behalen van optimale prestaties tijdens het importeren.

* Kies voor optimale overdrachtprestaties het hoogste serviceniveau en de hoogste prestatielaag die binnen uw budget mogelijk zijn. Na voltooiing van de migratie kunt u weer omlaag schalen om geld te besparen. 
* De afstand tussen het Bacpac-bestand en het doel-Datacenter minimaliseren.
* Schakel automatische statistieken tijdens de migratie uit.
* Partitioneer tabellen en indexen.
* Verwijder geïndexeerde weergaven en maak ze opnieuw nadat de migratie is voltooid.
* Verplaats zelden opgevraagde historische gegevens naar een andere database en migreer ze vervolgens naar een afzonderlijke Azure SQL Database. U kunt dan [elastische query's](sql-database-elastic-query-overview.md) uitvoeren om eventueel benodigde historische gegevens op te vragen.

### <a name="optimize-performance-after-the-migration-completes"></a>Prestaties optimaliseren nadat de migratie is voltooid

[Werk uw statistieken bij](https://msdn.microsoft.com/library/ms187348.aspx) met een volledige scan nadat de migratie is voltooid.

## <a name="method-2-use-transactional-replication"></a>Methode 2: Transactionele replicatie gebruiken

Als u het zich niet kunt permitteren om tijdens de migratie uw SQL Server-database uit de productieomgeving te verwijderen, kunt u als migratieoplossing transactionele replicatie voor SQL Server gebruiken. Voor het gebruik van deze methode moet de brondatabase voldoen aan de [vereisten voor transactionele replicatie](https://msdn.microsoft.com/library/mt589530.aspx) en compatibel zijn met Azure SQL Database. Zie voor meer informatie over SQL-replicatie met AlwaysOn [replicatie configureren voor AlwaysOn-beschikbaarheidsgroepen (SQL Server)](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server).

Als u deze oplossing wilt gebruiken, configureert u uw Azure SQL Database als abonnee voor de SQL Server-instantie die u wilt migreren. De distributeur voor transactionele replicatie synchroniseert dan de gegevens uit de database die moeten worden gesynchroniseerd (de uitgever) terwijl er nieuwe transacties blijven binnenkomen. 

Met transactionele replicatie komen alle wijzigingen aan uw gegevens of schema in uw Azure SQL Database terecht. Nadat de synchronisatie is voltooid en u klaar bent om te migreren, wijzigt u de verbindingstekenreeks in uw toepassingen zodanig dat deze naar uw Azure SQL Database verwijst. Wanneer door toepassing van transactionele replicatie alle wijzigingen die nog in uw brondatabase aanwezig zijn, zijn overgenomen en al uw toepassingen naar Azure DB verwijzen, kunt u de functie voor transactionele replicatie verwijderen. Uw Azure SQL Database is nu uw productiesysteem.

 ![SeedCloudTR-diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> U kunt transactionele replicatie ook gebruiken voor het migreren van een subset van uw brondatabase. De publicatie die u naar Azure SQL Database repliceert, kan worden beperkt tot een subset van de tabellen in de betreffende database. Voor elke tabel die wordt gerepliceerd, kunt u de gegevens beperken tot een subset van de rijen en/of een subset van de kolommen.
>

### <a name="migration-to-sql-database-using-transaction-replication-workflow"></a>Migratie naar SQL Database met de transactiereplicatiewerkstroom

> [!IMPORTANT]
> Gebruik altijd de nieuwste versie van SQL Server Management Studio om gesynchroniseerd te blijven met updates voor Microsoft Azure en SQL Database. In oudere versies van SQL Server Management Studio kunt u SQL Database niet instellen als abonnee. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 

1. Distributie instellen
   -  [SQL Server Management Studio (SSMS) gebruiken](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Transact-SQL gebruiken](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Publicatie maken
   -  [SQL Server Management Studio (SSMS) gebruiken](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Transact-SQL gebruiken](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Abonnement maken
   -  [SQL Server Management Studio (SSMS) gebruiken](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Transact-SQL gebruiken](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

### <a name="some-tips-and-differences-for-migrating-to-sql-database"></a>Enkele tips en verschillen met betrekking tot migratie naar SQL Database

1. Gebruik een lokale distributeur. 
   - In dat geval zorgt ervoor dat invloed op de prestaties op de server. 
   - Als de prestatie-impact onacceptabel is, kunt u een andere server gebruiken, maar dit verhoogt wel de complexiteit van het beheer.
2. Als u een map met momentopnamen selecteert, moet u ervoor zorgen dat de map die u selecteert, groot genoeg is voor een BCP van elke tabel die u wilt repliceren. 
3. Als u een momentopname maakt, worden de gekoppelde tabellen vergrendeld totdat het proces is voltooid. Het is dus belangrijk dat u het maken van momentopnamen goed plant. 
4. In Azure SQL Database worden alleen push-abonnementen ondersteund. U kunt alleen abonnees toevoegen vanuit de brondatabase.

## <a name="resolving-database-migration-compatibility-issues"></a>Compatibiliteitsproblemen met databasemigratie oplossen
Er zijn veel verschillende compatibiliteitsproblemen die zich kunnen voordoen. Dit is afhankelijk van de versie van SQL Server in de brondatabase en de complexiteit van de database die u migreert. Oudere versies van SQL Server hebben vaker te maken met compatibiliteitsproblemen. Gebruik de volgende resources en voer ook op internet een gerichte zoekopdracht uit (met uw zoekmachine):

* [SQL Server-databasefuncties die niet worden ondersteund in Azure SQL Database](sql-database-transact-sql-information.md)
* [Database Engine-functionaliteit in SQL Server 2016 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Database Engine-functionaliteit in SQL Server 2014 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Database Engine-functionaliteit in SQL Server 2012 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Database Engine-functionaliteit in SQL Server 2008 R2 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Database Engine-functionaliteit in SQL Server 2005 stopgezet](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

In aanvulling op deze resources en uw zoekopdrachten op internet kunt u ook de [MSDN SQL Server-communityforums](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) of [StackOverflow](http://stackoverflow.com/) raadplegen.

## <a name="next-steps"></a>Volgende stappen
* Gebruik het script dat beschikbaar is op de Azure SQL EMEA Engineers-blog [om het tempdb-gebruik tijdens de migratie te bewaken](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/).
* Gebruik het script dat beschikbaar is op de Azure SQL EMEA Engineers-blog [om de transactielogboekruimte van de database te bewaken terwijl de migratie bezig is](https://blogs.msdn.microsoft.com/azuresqlemea/2016/10/31/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database/0).
* Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
* Meer informatie over hoe u na de migratie met de UTC-tijd werkt, vindt u in [Modifying the default time zone for your local time zone](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/) (De standaardtijdzone voor uw lokale tijdzone wijzigen).
* Meer informatie over hoe u na de migratie de standaardtaal van een database wijzigt, vindt u in [How to change the default language of Azure SQL Database](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/) (De standaardtaal van Azure SQL Database wijzigen).


