---
title: Exemplaar van SQL Server migreren naar Azure SQL Database Managed Instance | Microsoft Docs
description: Leer hoe u een exemplaar van SQL Server migreren naar Azure SQL Database Managed Instance.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 09/26/2018
ms.openlocfilehash: 7653ce7b0823b4e91685e77701a307370261f7e6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394050"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server-exemplaar migratie naar Azure SQL Database Managed Instance

In dit artikel hebt u meer informatie over de methoden voor het migreren van een SQL Server 2005 of latere versie exemplaar [Azure SQL Database Managed Instance](sql-database-managed-instance.md).

Op hoog niveau, is het proces van de database-migratie ziet eruit zoals:

![Migratieproces](./media/sql-database-managed-instance-migration/migration-process.png)

- [Beoordeling van compatibiliteit met Managed Instance](#assess-managed-instance-compatibility)
- [App-connectiviteit kiezen](sql-database-managed-instance-connect-app.md)
- [Implementeren naar een optimaal grootte beheerd exemplaar](#deploy-to-an-optimally-sized-managed-instance)
- [Kies migratiemethode en migreren](#select-migration-method-and-migrate)
- [Toepassingen bewaken](#monitor-applications)

> [!NOTE]
> Zie voor het migreren van een individuele database in een individuele database of elastische pool, [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Beoordeling van compatibiliteit met Managed Instance

Bepaal eerst of Managed Instance compatibel met de vereisten voor de database van uw toepassing is. Beheerd exemplaar is zodanig ontworpen dat eenvoudige lift- and -shift-migratie voor de meeste van bestaande toepassingen die gebruikmaken van on-premises SQL Server of op virtuele machines. Echter, u hebt mogelijk soms functies of mogelijkheden die nog niet worden ondersteund en de kosten van de implementatie van een tijdelijke oplossing zijn te hoog. 

Gebruik [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) voor het detecteren van potentieel compatibiliteit die invloed hebben op database-functionaliteit op Azure SQL Database. DMA biedt nog geen ondersteuning voor beheerd exemplaar als doel van de migratie, maar het wordt aanbevolen om analyse uitvoeren op Azure SQL-Database en zorgvuldig lijst gerapporteerde functiepariteit en compatibiliteitsproblemen op basis van de productdocumentatie. Zie [Azure SQL Database-functies](sql-database-features.md) om te controleren of er zijn enkele gerapporteerde belemmeringen dat niet blockers in het beheerde exemplaar, omdat de meeste van de blokkering problemen te voorkomen dat een migratie naar Azure SQL Database zijn verwijderd met beheerde Het exemplaar. Voor het exemplaar, functies, zoals query's die databaseoverschrijdend, transacties tussen databases binnen dezelfde instantie, gekoppelde server met andere SQL bronnen, CLR, globale tijdelijke tabellen, exemplaar niveau weergaven, Service Broker en dergelijke zijn beschikbaar in beheerde instanties. 

Als er enkele blokkerende problemen die niet worden verwijderd in Azure SQL Database Managed Instance gerapporteerd zijn, moet u mogelijk rekening houden met een andere optie, zoals [SQL Server op virtuele Machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Hier volgen enkele voorbeelden:

- Als u directe toegang tot voor het besturingssysteem of het bestandssysteem, bijvoorbeeld voor installatie van derden of aangepaste agents op dezelfde virtuele machine met SQL Server vereist.
- Als u strikte afhankelijk zijn van functies die nog steeds niet worden ondersteund, zoals FileStream / bestandstabel, PolyBase en cross-instance-transacties.
- Als dit echt moet u blijven op een specifieke versie van SQL Server (2012, bijvoorbeeld).
- Als uw rekenvereisten veel lager zijn die Managed Instance biedt in openbare preview-versie (één vCore, bijvoorbeeld) en de consolidatie van de database is niet acceptabel optie.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementeren naar een optimaal grootte beheerd exemplaar

Beheerd exemplaar is die is ontworpen voor on-premises werkbelastingen die van plan bent om naar de cloud te verplaatsen. Dit introduceert een [nieuwe aankoopmodel](sql-database-service-tiers-vcore.md) die biedt meer flexibiliteit bij het selecteren van het juiste niveau van resources voor uw workloads. In de on-premises wereld bent u waarschijnlijk gewend zijn aan het formaat van deze werkbelastingen met behulp van fysieke kernen en i/o-bandbreedte. De nieuwe aankopen model voor Managed Instance is gebaseerd op virtuele kernen, of 'vCores', met extra opslagruimte en i/o-beschikbaar afzonderlijk. Het vCore-model is een eenvoudigere manier om te begrijpen van uw rekenvereisten in de cloud in plaats van wat u gebruikt on-premises vandaag nog. Dit nieuwe model kunt u de juiste grootte uw doelomgeving in de cloud.

Kunt u rekenkracht en opslagbronnen tijdens de implementatie tijd en daarna wijzigen zonder uitvaltijd voor uw toepassing met de [Azure-portal](sql-database-scale-resources.md):

![beheerd exemplaar grootte](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Zie voor meer informatie over het maken van de VNet-infrastructuur en een beheerd exemplaar, [maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Het is belangrijk dat u uw bestemming VNet en subnet altijd in overeenstemming met [vereisten voor beheerd exemplaar VNET](sql-database-managed-instance-vnet-configuration.md#requirements). Eventuele incompatibiliteit kunt voorkomen dat u uit het maken van nieuwe instanties of met behulp van die u al hebt gemaakt.

## <a name="select-migration-method-and-migrate"></a>Kies migratiemethode en migreren

Beheerd exemplaar doelen gebruikersscenario's massale databasemigratie van on-premises of IaaS-database-implementaties vereisen. Ze zijn beste keuze als u wilt lift- and -shift de back-end van de toepassingen die regelmatig instantieniveau gebruiken en / of functionaliteiten tussen meerdere databases. Als dit uw scenario, kunt u een volledige instantie verplaatsen naar een bijbehorende omgeving in Azure zonder te hoeven rearchitecture uw toepassingen. 

Voor het verplaatsen van SQL-exemplaren, moet u zorgvuldig plannen:

-   De migratie van alle databases die moeten worden CO-locaties (die zijn uitgevoerd op hetzelfde exemplaar)
-   De migratie van objecten op exemplaarniveau die uw toepassing afhankelijk is, met inbegrip van aanmeldingen, referenties, SQL Agent-taken en Operators en triggers voor server op. 

Managed Instance is een volledig beheerde service waarmee u een aantal van de reguliere DBA activiteiten voor het platform delegeren terwijl ze zijn ingebouwd in. Sommige niveau Instantiegegevens hoeft daarom niet worden gemigreerd, zoals onderhoudstaken voor regelmatige back-ups of de Always On configureren als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Beheerd exemplaar biedt ondersteuning voor de volgende database migratieopties (momenteel dit zijn de enige ondersteunde migratiemethoden):

- Azure Database migratieservice - migratie met bijna nul downtime,
- Systeemeigen `RESTORE DATABASE FROM URL` : maakt gebruik van systeemeigen back-ups van SQL Server en vereist enige uitvaltijd.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

De [Azure Database Migration Service (DMS)](../dms/dms-overview.md) is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure. Implementatie-opties op de openbare Preview opgenomen Azure SQL Database Managed Instance en SQL Server in een Azure Virtual machines. DMS is de aanbevolen methode van de migratie voor workloads van uw onderneming. 

Als u SQL Server Integration Services (SSIS) op uw SQL Server on-premises, DMS biedt nog geen ondersteuning voor migratie SSIS-catalogus (SSISDB) die SSIS-pakketten opslaat, maar u kunt Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) inrichten die wordt een nieuwe SSISDB maken in Azure SQL Database- / MI en vervolgens u kunt uw pakketten te implementeren, Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

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
|Herstellen van Azure-opslag naar beheerd exemplaar|[HERSTELLEN van URL met SAS-REFERENTIES](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Wanneer een database die is beveiligd door migreren [Transparent Data Encryption](transparent-data-encryption-azure-sql.md) naar Azure SQL Database Managed Instance met behulp van systeemeigen hersteloptie, het corresponderende certificaat uit het on-premises of IaaS SQL Server moet worden gemigreerd voordat u het terugzetten van de database. Zie voor gedetailleerde stappen [cert TDE migreren naar Managed Instance](sql-database-managed-instance-migrate-tde-certificate.md)
> - Herstellen van systeemdatabases wordt niet ondersteund. Exemplaar niveau om objecten te migreren (opgeslagen in de basispagina of msdb-databases), wordt aangeraden deze scripts en T-SQL-scripts uitvoeren op de doel-exemplaar.

Zie voor een snelstart van het back-up van een database herstellen naar een beheerd exemplaar met behulp van een SAS-referentie [herstellen vanuit back-up naar een beheerd exemplaar](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>Toepassingen bewaken

Gedrag van toepassingen en prestaties na de migratie bijhouden. In het beheerde exemplaar enkele wijzigingen zijn slechts één keer worden ingeschakeld de [databasecompatibiliteitsniveau is gewijzigd](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Het compatibiliteitsniveau van de oorspronkelijke blijft databasemigratie naar Azure SQL Database in de meeste gevallen. Als het compatibiliteitsniveau van een gebruikersdatabase 100 of hoger vóór de migratie is, blijft ongewijzigd na de migratie. Als het compatibiliteitsniveau van een gebruikersdatabase 90 vóór de migratie in de bijgewerkte database is is het compatibiliteitsniveau ingesteld op 100, dit het laagste ondersteunde compatibiliteitsniveau in het beheerde exemplaar is. Compatibiliteitsniveau van systeemdatabases is 140.

Migratierisico, verminderen het databasecompatibiliteitsniveau alleen na het bewaking van toepassingsprestaties te wijzigen. Gebruik Query Store als optimale hulpprogramma voor het ophalen van informatie over de prestaties van de werkbelasting voor en na database compatibility level wijzigen, zoals uitgelegd in [prestaties stabiliteit behouden tijdens de upgrade naar nieuwere versie van SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Wanneer u zich op een volledig beheerd platform, worden de voordelen die automatisch worden geleverd als onderdeel van de service SQL Database. Bijvoorbeeld, u hoeft te maken van back-ups op Managed Instance - de service uitvoert back-ups voor u automatisch. U moet niet meer zorgen over de planning, nemen en het beheer van back-ups. Beheerd exemplaar biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaarperiode periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Tijdens de openbare preview, is de bewaarperiode liggen vast in zeven dagen.
Bovendien hoeft niet te hoeven maken over het instellen van hoge beschikbaarheid als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Als u wilt de beveiliging versterken, kunt u overwegen sommige van de functies die beschikbaar zijn:
- Azure Active Directory-verificatie op databaseniveau
- Gebruik [geavanceerde beveiligingsfuncties](sql-database-security-overview.md) zoals [controle](sql-database-managed-instance-auditing.md), [detectie van bedreigingen](sql-advanced-threat-protection.md), [Row-Level Security](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), en [dynamische Gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ) voor het beveiligen van uw exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaren [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor een zelfstudie waarin een herstel vanuit back-up, [maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
- Zie voor een zelfstudie waarin migratie met behulp van DMS [Migrate uw on-premises database naar Managed Instance met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
