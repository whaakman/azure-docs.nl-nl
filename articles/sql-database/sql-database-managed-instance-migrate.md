---
title: SQL Server-instantie migreren naar Azure SQL Database-beheerd instantie | Microsoft Docs
description: Informatie over het migreren van een exemplaar van SQL Server naar Azure SQL Database-beheerd instantie.
keywords: databasemigratie, sql server-databasemigratie, hulpprogramma's voor databasemigratie, database migreren, sql-database migreren
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 04/10/2018
ms.author: bonova
ms.openlocfilehash: 5b8a2ec7e0401ac239acdefdd77a13b522f73960
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server-exemplaar migratie naar Azure SQL Database-beheerd instantie

In dit artikel leert u de methoden voor het migreren van een SQL Server 2005 of een latere versie naar Azure SQL Database beheerd-instantie (preview). 

SQL Database Managed Instance is een uitbreiding van de bestaande SQL Database-service en biedt naast databases en elastische pools een derde implementatieoptie.  Het is bedoeld voor database lift-en-verschuiving naar een volledig beheerde PaaS zonder de toepassing opnieuw. SQL Database Managed Instance biedt extra compatibiliteit met het on-premises SQL Server-programmeermodel en gebruiksklare ondersteuning voor de meeste functies van SQL Server en de bijbehorende hulpprogramma's en services.

Op het hoogste niveau ziet migratieproces toepassing er in het volgende diagram:

![migratieproces](./media/sql-database-managed-instance-migration/migration-process.png)

- [Beheerde exemplaar compatibiliteit evalueren](sql-database-managed-instance-migrate.md#assess-managed-instance-compatibility)
- [App-verbindingsoptie kiezen](sql-database-managed-instance-migrate.md#choose-app-connectivity-option)
- [Implementeren naar een optimaal grootte beheerde exemplaar](sql-database-managed-instance-migrate.md#deploy-to-an-optimally-sized-managed-instance)
- [Selecteer de migratiemethode en migreren](sql-database-managed-instance-migrate.md#select-migration-method-and-migrate)
- [Toepassingen bewaken](sql-database-managed-instance-migrate.md#monitor-applications)

> [!NOTE]
> Zie voor het migreren van een individuele database in een individuele database of een elastische pool, [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Beheerde exemplaar compatibiliteit evalueren

Bepaal eerst of beheerd exemplaar compatibel met de Databasevereisten van uw toepassing is. Beheerde exemplaar is ontworpen om gemakkelijk lift en shift migratie voor het merendeel van bestaande toepassingen die gebruikmaken van SQL Server on-premises, of op virtuele machines. U moet er soms functies of echter mogelijkheden die nog niet ondersteund en de kosten van de implementatie van een tijdelijke oplossing te hoog zijn. 

Gebruik [gegevens migratie-assistent (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) voor het detecteren van mogelijke compatibiliteit botsende databasefunctionaliteit op Azure SQL Database. DMA ondersteunt nog geen exemplaar worden beheerd als Migratiebestemming, maar het is raadzaam analyse uitvoeren op Azure SQL-Database en lijst met gerapporteerde functie pariteit en compatibiliteitsproblemen tegen productdocumentatie zorgvuldig te controleren. De meeste van de blokkerende problemen voorkomen van een migratie naar Azure SQL Database zijn met beheerde exemplaar verwijderd. Exemplaar, functies, zoals query's voor meerdere databases, transacties tussen meerdere databases binnen hetzelfde exemplaar, gekoppelde server aan andere SQL bronnen, de CLR, globale tijdelijke tabellen, zijn exemplaar niveau weergaven, Service Broker en dergelijke beschikbaar in exemplaren die worden beheerd. 

Er zijn echter enkele gevallen wanneer moet u rekening houden met een andere optie zoals [SQL Server op virtuele Machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Hier volgen enkele voorbeelden:

- Als u directe toegang voor het besturingssysteem of het bestandssysteem, bijvoorbeeld voor installatie van derden of aangepaste agents op dezelfde virtuele machine met SQL Server vereist.
- Als er strenge afhankelijkheid op functies die zijn nog steeds niet wordt ondersteund, zoals FileStream / bestandstabel, PolyBase en cross-instance transacties.
- Als dit echt moet u op een specifieke versie van SQL Server blijven (2012, bijvoorbeeld).
- Als uw compute-vereisten veel lager zijn dat exemplaar beheerd biedt openbare Preview (één vCore voor instantie) en de consolidatie van de database is niet acceptabel optie.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementeren naar een optimaal grootte beheerde exemplaar

Beheerde exemplaar is aangepast voor lokale werkbelastingen die u van plan bent te verplaatsen naar de cloud. Dit introduceert een nieuwe koopmodel waarin meer flexibiliteit biedt bij het selecteren van het juiste niveau van resources voor uw werkbelastingen. In de wereld lokale bent u waarschijnlijk gewend voor het formaat van deze werkbelastingen met behulp van de fysieke kernen. De nieuwe aankoopmodel voor beheerde exemplaar is gebaseerd op virtuele kernen, of 'vCores' met de extra opslag en i/o beschikbaar afzonderlijk. Het model vCore is een eenvoudigere manier om te begrijpen uw compute-vereisten in de cloud tegenover die u gebruikt lokale vandaag. Dit nieuwe model kunt u het juiste formaat uw omgeving bestemming in de cloud.

Kunt u compute en opslagbronnen op implementatie tijd en daarna wijzigen zonder uitvaltijd voor uw toepassing.

![beheerde exemplaar schaling](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Zie voor meer informatie over het maken van de VNet-infrastructuur en een exemplaar beheerd, [maken van een exemplaar beheerd](sql-database-managed-instance-create-tutorial-portal.md).

> [!IMPORTANT]
> Het is belangrijk om te houden van uw doel VNet en subnet altijd overeenkomstig [beheerd exemplaar VNET vereisten](sql-database-managed-instance-vnet-configuration.md#requirements). Elke incompatibiliteit kunt voorkomen dat u van nieuwe exemplaren maken of gebruiken die u al hebt gemaakt.

## <a name="select-migration-method-and-migrate"></a>Selecteer de migratiemethode en migreren

Beheerde exemplaar doelen gebruiker scenario's voor massaopslag databasemigratie van on-premises of IaaS database implementaties vereisen. Ze zijn beste keuze wanneer u moet lift- en back-end van de toepassingen die regelmatig niveau van het exemplaar gebruiken en / of functionaliteiten tussen meerdere databases worden verplaatst. Als dit uw scenario, kunt u een volledige instantie verplaatsen naar een overeenkomende omgeving in Azure zonder te hoeven rearchitecture uw toepassingen. 

U moet zorgvuldig plannen voor het verplaatsen van SQL-exemplaren:

-   De migratie van alle databases die moeten worden CO-locaties (die zijn uitgevoerd op hetzelfde exemplaar)
-   De migratie van instantieniveau-objecten die uw toepassing afhankelijk is, met inbegrip van aanmeldingen, referenties SQL Agent-taken en Operators en triggers voor server op. 

Beheerde exemplaar is een volledig beheerde service waarmee u sommige van de reguliere DBA activiteiten voor het platform delegeren, zoals ze zijn ingebouwd in. Sommige niveau exemplaargegevens hoeft daarom niet worden gemigreerd, zoals onderhoudstaken voor regelmatige back-ups of altijd op configuratie als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Beheerde exemplaar ondersteunt de volgende migratie-opties voor database is (deze zijn momenteel de enige ondersteunde migratiemethoden):

- Azure migratie databaseservice - migratie met vrijwel geen uitvaltijd
- Systeemeigen herstel van de URL - systeemeigen back-ups van SQL Server gebruikt en sommige te worden stilgelegd
- Migreren met behulp van Bacpac-bestand - Bacpac-bestand van SQL Server of SQL-Database wordt gebruikt en sommige te worden stilgelegd

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

De [Azure Database migratie Service (DMS)](../dms/dms-overview.md) is een volledig beheerde service die is ontworpen om in te schakelen naadloze migraties uit meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure. Implementatieopties voor op Public Preview zijn Azure SQL Database, exemplaar beheerd en SQL Server in een virtuele Machine van Azure. DMS is de aanbevolen methode van de migratie voor de werkbelasting van uw onderneming. 

Zie voor meer informatie over dit scenario en configuratiestappen voor DMS, [migreren uw on-premises database met beheerde-exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>Systeemeigen herstel van de URL

HERSTELLEN van systeemeigen back-ups (.bak-bestanden) die SQL Server on-premises of [SQL Server op virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/), beschikbaar op [Azure Storage](https://azure.microsoft.com/services/storage/), is een van de belangrijkste mogelijkheden op beheerde exemplaar van SQL DB die migratie van de database kunnen snel en eenvoudig offline. 

Het volgende diagram wordt het proces op hoog niveau beschreven:

![migratie-stroom](./media/sql-database-managed-instance-migration/migration-flow.png)

De volgende tabel biedt meer informatie over de methode die u kunt gebruiken, afhankelijk van de bron SQL Server-versie die u uitvoert:

|Stap|SQL-Engine en versie|Back-up / herstel methode|
|---|---|---|
|Back-up naar Azure Storage plaatsen|Eerdere SQL 2012 SP1 CU2|.Bak-bestand rechtstreeks uploaden naar Azure storage|
||2012 SP1 CU2 - 2016|Directe back-up met behulp van afgeschaft [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) syntaxis|
||2016 en hoger|Directe back-up met behulp van [met SAS-referentie](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Herstel naar Azure Storage op beheerde exemplaar|[HERSTELLEN van URL met SAS-referentie](sql-database-managed-instance-restore-from-backup-tutorial.md)|

> [!IMPORTANT]
> Herstel van systeemdatabases wordt niet ondersteund. Exemplaar niveau om objecten te migreren (opgeslagen in master of msdb-databases), wordt aangeraden script deze en T-SQL-scripts uitvoeren op de doel-exemplaar.

Zie voor een volledige zelfstudie waarin de back-up van een database herstellen naar een exemplaar beheerd met behulp van een SAS-referentie [back-up terugzetten naar een exemplaar beheerd](sql-database-managed-instance-restore-from-backup-tutorial.md).

### <a name="migrate-using-bacpac-file"></a>Migreren met behulp van Bacpac-bestand

U kunt importeren in Azure SQL Database en beheerde exemplaar van een maken een kopie van de oorspronkelijke database, met de gegevens, in een Bacpac-bestand. Zie [een Bacpac-bestand importeren in een nieuwe Azure SQL Database](sql-database-import.md).

## <a name="monitor-applications"></a>Toepassingen bewaken

Gedrag van toepassingen en prestaties na de migratie bijhouden. In beheerde exemplaar enkele wijzigingen zijn slechts één keer worden ingeschakeld de [databasecompatibiliteitsniveau is gewijzigd](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Databasemigratie naar Azure SQL Database blijft de oorspronkelijke compatibiliteitsniveau in de meeste gevallen. Als het compatibiliteitsniveau van een gebruikersdatabase 100 of hoger vóór de migratie, blijft hetzelfde na de migratie. Als het compatibiliteitsniveau van een gebruikersdatabase 90 vóór de migratie in de bijgewerkte database is is het compatibiliteitsniveau ingesteld op 100, waarmee de laagste ondersteunde compatibiliteitsniveau in beheerde exemplaar is. Het compatibiliteitsniveau van systeemdatabases is 140.

Wijzigen als u risico's voor migratie, het databasecompatibiliteitsniveau pas na het controleren van de prestaties. Gebruik Query Store als optimale hulpprogramma voor het ophalen van informatie over de prestaties van de werkbelasting voor en na compatibiliteit niveau wijziging van de database, zoals wordt beschreven in [prestaties stabiliteit behouden tijdens de upgrade naar een nieuwere versie van SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Wanneer u zich op een volledig beheerd platform, nemen de voordelen die automatisch worden geleverd als onderdeel van de service SQL-Database. Bijvoorbeeld, u hoeft te maken van back-ups op beheerde exemplaar: de service worden back-ups voor u automatisch. U moet niet langer hoeft over het plannen, maken en beheren van back-ups. Beheerde exemplaar biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaren periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). De bewaarperiode is tijdens de openbare preview opgelost in zeven dagen.
Bovendien hoeft niet te hoeven maken over het instellen van hoge beschikbaarheid als [hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in.

Versterking van beveiliging, kunt u overwegen bepaalde functies die beschikbaar zijn:
- Azure Active Directory-verificatie op het databaseniveau van de
- Controle en detectie van dreigingen voor het bewaken van activiteiten
- Beheren van toegang tot gevoelige en bevoorrechte gegevens ([beveiliging](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) en [dynamische-Gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over exemplaren beheerd [wat is er een exemplaar beheerd?](sql-database-managed-instance.md).
- Zie voor een zelfstudie waarin een herstel van back-up, [maken van een exemplaar beheerd](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor een zelfstudie waarin migratie met behulp van DMS [migreren uw on-premises database met beheerde-exemplaar met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
