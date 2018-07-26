---
title: Exemplaar van SQL Server migreren naar Azure SQL Database Managed Instance | Microsoft Docs
description: Leer hoe u een exemplaar van SQL Server migreren naar Azure SQL Database Managed Instance.
keywords: databasemigratie, sql server-databasemigratie, hulpprogramma's voor databasemigratie, database migreren, sql-database migreren
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: bonova
ms.openlocfilehash: a9a02f9007c174024028305746682f9ac07dab22
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247207"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server-exemplaar migratie naar Azure SQL Database Managed Instance

In dit artikel leert u over de methoden voor het migreren van een SQL Server 2005 of latere versie-exemplaar naar Azure SQL Database Managed Instance (preview). 

SQL Database Managed Instance is een uitbreiding van de bestaande SQL Database-service en biedt naast databases en elastische pools een derde implementatieoptie.  Het is ontworpen om in te schakelen database lift-and-shift naar een volledig beheerde PaaS zonder het ontwerp van de toepassing. SQL Database Managed Instance biedt extra compatibiliteit met het on-premises SQL Server-programmeermodel en gebruiksklare ondersteuning voor de meeste functies van SQL Server en de bijbehorende hulpprogramma's en services.

Op hoog niveau lijkt het migratieproces toepassing:

![Migratieproces](./media/sql-database-managed-instance-migration/migration-process.png)

- [Beoordeling van compatibiliteit met Managed Instance](sql-database-managed-instance-migrate.md#assess-managed-instance-compatibility)
- [App-connectiviteit kiezen](sql-database-managed-instance-migrate.md#choose-app-connectivity-option)
- [Implementeren naar een optimaal grootte beheerd exemplaar](sql-database-managed-instance-migrate.md#deploy-to-an-optimally-sized-managed-instance)
- [Kies migratiemethode en migreren](sql-database-managed-instance-migrate.md#select-migration-method-and-migrate)
- [Toepassingen bewaken](sql-database-managed-instance-migrate.md#monitor-applications)

> [!NOTE]
> Zie voor het migreren van een individuele database in een individuele database of elastische pool, [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Beoordeling van compatibiliteit met Managed Instance

Bepaal eerst of Managed Instance compatibel met de vereisten voor de database van uw toepassing is. Beheerd exemplaar is zodanig ontworpen dat eenvoudige lift- and -shift-migratie voor de meeste van bestaande toepassingen die gebruikmaken van on-premises SQL Server of op virtuele machines. Echter, u hebt mogelijk soms functies of mogelijkheden die nog niet worden ondersteund en de kosten van de implementatie van een tijdelijke oplossing zijn te hoog. 

Gebruik [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) voor het detecteren van potentieel compatibiliteit die invloed hebben op database-functionaliteit op Azure SQL Database. DMA biedt nog geen ondersteuning voor beheerd exemplaar als doel van de migratie, maar het wordt aanbevolen om analyse uitvoeren op Azure SQL-Database en zorgvuldig lijst gerapporteerde functiepariteit en compatibiliteitsproblemen op basis van de productdocumentatie. De meeste van de blokkerende problemen te voorkomen dat een migratie naar Azure SQL Database zijn verwijderd met Managed Instance. Voor het exemplaar, functies, zoals query's die databaseoverschrijdend, transacties tussen databases binnen dezelfde instantie, gekoppelde server met andere SQL bronnen, CLR, globale tijdelijke tabellen, exemplaar niveau weergaven, Service Broker en dergelijke zijn beschikbaar in beheerde instanties. 

Er zijn echter enkele gevallen wanneer u moet rekening houden met een andere optie, zoals [SQL Server op virtuele Machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Hier volgen enkele voorbeelden:

- Als u directe toegang tot voor het besturingssysteem of het bestandssysteem, bijvoorbeeld voor installatie van derden of aangepaste agents op dezelfde virtuele machine met SQL Server vereist.
- Als u strikte afhankelijk zijn van functies die nog steeds niet worden ondersteund, zoals FileStream / bestandstabel, PolyBase en cross-instance-transacties.
- Als dit echt moet u blijven op een specifieke versie van SQL Server (2012, bijvoorbeeld).
- Als uw rekenvereisten veel lager zijn die Managed Instance biedt in openbare preview-versie (één vCore, bijvoorbeeld) en de consolidatie van de database is niet acceptabel optie.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementeren naar een optimaal grootte beheerd exemplaar

Beheerd exemplaar is die is ontworpen voor on-premises werkbelastingen die van plan bent om naar de cloud te verplaatsen. Dit introduceert een nieuwe aankopen model dat meer flexibiliteit biedt bij het selecteren van het juiste niveau van resources voor uw workloads. In de on-premises wereld bent u waarschijnlijk gewend zijn aan het formaat van deze werkbelastingen met behulp van fysieke kernen. De nieuwe aankopen model voor Managed Instance is gebaseerd op virtuele kernen, of 'vCores', met extra opslagruimte en i/o-beschikbaar afzonderlijk. Het vCore-model is een eenvoudigere manier om te begrijpen van uw rekenvereisten in de cloud in plaats van wat u gebruikt on-premises vandaag nog. Dit nieuwe model kunt u de juiste grootte uw doelomgeving in de cloud.

Kunt u rekenkracht en opslagbronnen op implementatie tijd en daarna wijzigen zonder uitvaltijd voor uw toepassing.

![beheerd exemplaar grootte](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Zie voor meer informatie over het maken van de VNet-infrastructuur en een beheerd exemplaar, [maken van een beheerd exemplaar](sql-database-managed-instance-create-tutorial-portal.md).

> [!IMPORTANT]
> Het is belangrijk dat u uw bestemming VNet en subnet altijd in overeenstemming met [vereisten voor beheerd exemplaar VNET](sql-database-managed-instance-vnet-configuration.md#requirements). Eventuele incompatibiliteit kunt voorkomen dat u uit het maken van nieuwe instanties of met behulp van die u al hebt gemaakt.

## <a name="select-migration-method-and-migrate"></a>Kies migratiemethode en migreren

Beheerd exemplaar doelen gebruikersscenario's massale databasemigratie van on-premises of IaaS-database-implementaties vereisen. Ze zijn beste keuze als u wilt lift- and -shift de back-end van de toepassingen die regelmatig instantieniveau gebruiken en / of functionaliteiten tussen meerdere databases. Als dit uw scenario, kunt u een volledige instantie verplaatsen naar een bijbehorende omgeving in Azure zonder te hoeven rearchitecture uw toepassingen. 

Voor het verplaatsen van SQL-exemplaren, moet u zorgvuldig plannen:

-   De migratie van alle databases die moeten worden CO-locaties (die zijn uitgevoerd op hetzelfde exemplaar)
-   De migratie van objecten op exemplaarniveau die uw toepassing afhankelijk is, met inbegrip van aanmeldingen, referenties, SQL Agent-taken en Operators en triggers voor server op. 

Managed Instance is een volledig beheerde service waarmee u een aantal van de reguliere DBA activiteiten voor het platform delegeren terwijl ze zijn ingebouwd in. Sommige niveau Instantiegegevens hoeft daarom niet worden gemigreerd, zoals onderhoudstaken voor regelmatige back-ups of de Always On configureren als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Beheerd exemplaar biedt ondersteuning voor de volgende database migratieopties (momenteel dit zijn de enige ondersteunde migratiemethoden):

- Azure Database migratieservice - migratie met bijna nul downtime
- Systeemeigen terugzetten vanuit URL - systeemeigen back-ups van SQL Server gebruikt en vereist enige uitvaltijd

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

De [Azure Database Migration Service (DMS)](../dms/dms-overview.md) is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure. Implementatie-opties op de openbare Preview opgenomen Azure SQL Database Managed Instance en SQL Server in een Azure Virtual machines. DMS is de aanbevolen methode van de migratie voor workloads van uw onderneming. 

Als u SQL Server Integration Services (SSIS) op uw SQL Server on-premises, DMS biedt nog geen ondersteuning voor migratie SSIS-catalogus (SSISDB) die SSIS-pakketten opslaat, maar u kunt Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) inrichten die wordt een nieuwe SSISDB maken in Azure SQL Database- / MI en vervolgens u kunt uw pakketten te implementeren, Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

Zie voor meer informatie over dit scenario en configuratiestappen voor DMS [Migrate uw on-premises database naar Managed Instance met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>Systeemeigen terugzetten vanuit URL

HERSTELLEN van systeemeigen back-ups (.bak-bestanden) die van SQL Server on-premises of [SQL Server op virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/), beschikbaar op [Azure Storage](https://azure.microsoft.com/services/storage/), is een van de belangrijkste mogelijkheden op SQL database Managed Instance die kunnen snel en eenvoudig offline migratie-database. 

Het volgende diagram biedt een overzicht op hoog niveau van het proces:

![migratie-stroom](./media/sql-database-managed-instance-migration/migration-flow.png)

De volgende tabel vindt u meer informatie over de methoden die u kunt gebruiken, afhankelijk van de bron SQL Server-versie wordt uitgevoerd:

|Stap|SQL-Engine en versie|Back-up en herstellen van de methode|
|---|---|---|
|Back-up naar Azure Storage plaatsen|Eerdere SQL 2012 SP1 CU2|Uploaden van bestand met .bak rechtstreeks naar Azure storage|
||2012 SP1 CU2 - 2016|Directe back-up met afgeschaft [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) syntaxis|
||2016 en hoger|Directe back-up met [met SAS-REFERENTIES](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Herstellen van Azure-opslag naar beheerd exemplaar|[HERSTELLEN van URL met SAS-REFERENTIES](sql-database-managed-instance-restore-from-backup-tutorial.md)|

> [!IMPORTANT]
> - Wanneer een database die is beveiligd met [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) wordt gemigreerd naar een met Azure SQL Database beheerd exemplaar met behulp van de optie voor systeemeigen herstel, moet het corresponderende certificaat van de on-premises of IaaS SQL-server worden gemigreerd voordat de database wordt hersteld. Zie voor gedetailleerde stappen [cert TDE migreren naar Managed Instance](sql-database-managed-instance-migrate-tde-certificate.md)
> - Herstellen van systeemdatabases wordt niet ondersteund. Exemplaar niveau om objecten te migreren (opgeslagen in de basispagina of msdb-databases), wordt aangeraden deze scripts en T-SQL-scripts uitvoeren op de doel-exemplaar.

Zie voor een volledige zelfstudie waarin de back-up van een database herstellen naar een beheerd exemplaar met behulp van een SAS-referenties, [herstellen vanuit back-up naar een beheerd exemplaar](sql-database-managed-instance-restore-from-backup-tutorial.md).

## <a name="monitor-applications"></a>Toepassingen bewaken

Gedrag van toepassingen en prestaties na de migratie bijhouden. In het beheerde exemplaar enkele wijzigingen zijn slechts één keer worden ingeschakeld de [databasecompatibiliteitsniveau is gewijzigd](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Het compatibiliteitsniveau van de oorspronkelijke blijft databasemigratie naar Azure SQL Database in de meeste gevallen. Als het compatibiliteitsniveau van een gebruikersdatabase 100 of hoger vóór de migratie is, blijft ongewijzigd na de migratie. Als het compatibiliteitsniveau van een gebruikersdatabase 90 vóór de migratie in de bijgewerkte database is is het compatibiliteitsniveau ingesteld op 100, dit het laagste ondersteunde compatibiliteitsniveau in het beheerde exemplaar is. Compatibiliteitsniveau van systeemdatabases is 140.

Migratierisico, verminderen het databasecompatibiliteitsniveau alleen na het bewaking van toepassingsprestaties te wijzigen. Gebruik Query Store als optimale hulpprogramma voor het ophalen van informatie over de prestaties van de werkbelasting voor en na database compatibility level wijzigen, zoals uitgelegd in [prestaties stabiliteit behouden tijdens de upgrade naar nieuwere versie van SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Wanneer u zich op een volledig beheerd platform, worden de voordelen die automatisch worden geleverd als onderdeel van de service SQL Database. Bijvoorbeeld, u hoeft te maken van back-ups op Managed Instance - de service uitvoert back-ups voor u automatisch. U moet niet meer zorgen over de planning, nemen en het beheer van back-ups. Beheerd exemplaar biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaarperiode periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Tijdens de openbare preview, is de bewaarperiode liggen vast in zeven dagen.
Bovendien hoeft niet te hoeven maken over het instellen van hoge beschikbaarheid als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Als u wilt de beveiliging versterken, kunt u overwegen sommige van de functies die beschikbaar zijn:
- Azure Active Directory-verificatie op databaseniveau
- Controle en detectie van bedreigingen voor het bewaken van activiteiten
- Beheren van toegang tot gevoelige en beschermde gegevens ([Row-Level Security](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) en [Dynamic Data Masking](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaren [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor een zelfstudie waarin een herstel vanuit back-up, [maken van een beheerd exemplaar](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor een zelfstudie waarin migratie met behulp van DMS [Migrate uw on-premises database naar Managed Instance met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
