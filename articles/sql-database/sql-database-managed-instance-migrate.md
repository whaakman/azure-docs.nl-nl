---
title: Migreren van SQL Server-exemplaar naar beheerd exemplaar van Azure SQL Database | Microsoft Docs
description: Informatie over het migreren van een SQL Server-exemplaar naar beheerd exemplaar van Azure SQL Database.
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
ms.date: 02/11/2019
ms.openlocfilehash: 1460b595e8887fc932d5be335ae51b07a000b9fb
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098354"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Beheerd exemplaar van SQL Server-exemplaar migratie naar Azure SQL Database

In dit artikel hebt u meer informatie over de methoden voor het migreren van een SQL Server 2005 of latere versie exemplaar [Azure SQL Database managed instance](sql-database-managed-instance.md). Zie voor meer informatie over het migreren naar een individuele database of elastische pool [migreren naar een database van één of gegroepeerde](sql-database-cloud-migrate.md). Zie voor informatie over het migreren van andere platforms migratie [Azure handleiding voor databasemigratie](https://datamigration.microsoft.com/).

Op hoog niveau, is het proces van de database-migratie ziet eruit zoals:

![Migratieproces](./media/sql-database-managed-instance-migration/migration-process.png)

- [Beoordeling van compatibiliteit met beheerd exemplaar](#assess-managed-instance-compatibility)
- [App-connectiviteit kiezen](sql-database-managed-instance-connect-app.md)
- [Implementeren naar een optimaal grootte beheerd exemplaar](#deploy-to-an-optimally-sized-managed-instance)
- [Kies migratiemethode en migreren](#select-migration-method-and-migrate)
- [Toepassingen bewaken](#monitor-applications)

> [!NOTE]
> Als u wilt migreren van een individuele database in een individuele database of elastische pool, Zie [een SQL Server-database migreren naar Azure SQL Database](sql-database-single-database-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Beoordeling van compatibiliteit met beheerd exemplaar

Ga eerst na of het beheerde exemplaar is compatibel met de vereisten voor de database van uw toepassing. De Implementatieoptie voor het beheerde exemplaar is ontworpen voor eenvoudige lift- and -shift-migratie voor de meeste van bestaande toepassingen die gebruikmaken van on-premises SQL Server of op virtuele machines. Echter, u hebt mogelijk soms functies of mogelijkheden die nog niet worden ondersteund en de kosten van de implementatie van een tijdelijke oplossing zijn te hoog.

Gebruik [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) voor het detecteren van potentieel compatibiliteit die invloed hebben op database-functionaliteit op Azure SQL Database. DMA niet nog ondersteuning voor beheerd exemplaar als doel van de migratie, maar het wordt aanbevolen om analyse uitvoeren op Azure SQL-Database en zorgvuldig lijst gerapporteerde functiepariteit en compatibiliteitsproblemen op basis van de productdocumentatie. Zie [Azure SQL Database-functies](sql-database-features.md) om te controleren of er zijn enkele blokkerende problemen die niet blockers in beheerd exemplaar, omdat de meeste van de blokkerende problemen te voorkomen dat een migratie naar Azure SQL Database zijn verwijderd met beheerde gerapporteerd het exemplaar. Voor het exemplaar, functies, zoals query's die databaseoverschrijdend, transacties binnen hetzelfde exemplaar tussen meerdere databases, gekoppelde server met andere SQL bronnen, CLR, globale tijdelijke tabellen zijn exemplaar niveau weergaven, Service Broker en dergelijke beschikbaar in beheerde exemplaren.

Als er enkele blokkerende problemen die niet worden verwijderd met de Implementatieoptie voor beheerd exemplaar gerapporteerd zijn, moet u mogelijk rekening houden met een andere optie, zoals [SQL Server op Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/). Hier volgen enkele voorbeelden:

- Als u directe toegang tot voor het besturingssysteem of het bestandssysteem, bijvoorbeeld voor installatie van derden of aangepaste agents op dezelfde virtuele machine met SQL Server vereist.
- Als u strikte afhankelijk zijn van functies die nog steeds niet worden ondersteund, zoals FileStream / bestandstabel, PolyBase en cross-instance-transacties.
- Als dit echt moet u blijven op een specifieke versie van SQL Server (2012, bijvoorbeeld).
- Als uw rekenvereisten veel lager zijn die beheerd exemplaar biedt (één vCore, bijvoorbeeld) en de consolidatie van de database is niet acceptabel optie.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementeren naar een beheerd exemplaar voor optimale grootte

Beheerd exemplaar is die is ontworpen voor on-premises werkbelastingen die van plan bent om naar de cloud te verplaatsen. Dit introduceert een [nieuwe aankoopmodel](sql-database-service-tiers-vcore.md) die biedt meer flexibiliteit bij het selecteren van het juiste niveau van resources voor uw workloads. In de on-premises wereld bent u waarschijnlijk gewend zijn aan het formaat van deze werkbelastingen met behulp van fysieke kernen en i/o-bandbreedte. De aankopen model voor het beheerde exemplaar is gebaseerd op virtuele kernen, of 'vCores', met extra opslagruimte en i/o-beschikbaar afzonderlijk. Het vCore-model is een eenvoudigere manier om te begrijpen van uw rekenvereisten in de cloud in plaats van wat u gebruikt on-premises vandaag nog. Dit nieuwe model kunt u de juiste grootte uw doelomgeving in de cloud.

Kunt u rekenkracht en opslagbronnen tijdens de implementatie tijd en daarna wijzigen zonder uitvaltijd voor uw toepassing met de [Azure-portal](sql-database-scale-resources.md):

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

Gedrag van toepassingen en prestaties na de migratie bijhouden. In het beheerde exemplaar enkele wijzigingen zijn slechts één keer worden ingeschakeld de [databasecompatibiliteitsniveau is gewijzigd](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Het compatibiliteitsniveau van de oorspronkelijke blijft databasemigratie naar Azure SQL Database in de meeste gevallen. Als het compatibiliteitsniveau van een gebruikersdatabase 100 of hoger vóór de migratie is, blijft ongewijzigd na de migratie. Als het compatibiliteitsniveau van een gebruikersdatabase 90 vóór de migratie in de bijgewerkte database is is het compatibiliteitsniveau ingesteld op 100, dit het laagste ondersteunde compatibiliteitsniveau in beheerd exemplaar is. Compatibiliteitsniveau van systeemdatabases is 140.

Migratierisico, verminderen het databasecompatibiliteitsniveau alleen na het bewaking van toepassingsprestaties te wijzigen. Gebruik Query Store als optimale hulpprogramma voor het ophalen van informatie over de prestaties van de werkbelasting voor en na database compatibility level wijzigen, zoals uitgelegd in [prestaties stabiliteit behouden tijdens de upgrade naar nieuwere versie van SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Wanneer u zich op een volledig beheerd platform, worden de voordelen die automatisch worden geleverd als onderdeel van de service SQL Database. Bijvoorbeeld, u hoeft te maken van back-ups op beheerde exemplaar: de service uitvoert back-ups voor u automatisch. U moet niet meer zorgen over de planning, nemen en het beheer van back-ups. Beheerd exemplaar biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaarperiode periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Bovendien hoeft niet te hoeven maken over het instellen van hoge beschikbaarheid als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Als u wilt de beveiliging versterken, kunt u overwegen sommige van de functies die beschikbaar zijn:

- Azure Active Directory-verificatie op databaseniveau
- Gebruik [geavanceerde beveiligingsfuncties](sql-database-security-overview.md) zoals [controle](sql-database-managed-instance-auditing.md), [detectie van bedreigingen](sql-database-advanced-data-security.md), [beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), en [dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ) voor het beveiligen van uw exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaren [wat is er een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor een zelfstudie waarin een herstel vanuit back-up, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md).
- Zie voor een zelfstudie waarin migratie met behulp van DMS [uw on-premises database migreren naar beheerd exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
