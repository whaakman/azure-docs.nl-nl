---
title: Azure SQL-Database beheerd exemplaar T-SQL verschillen | Microsoft Docs
description: Dit artikel worden de T-SQL-verschillen tussen beheerde exemplaar van Azure SQL Database en SQL Server.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab, bonova
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 95eca05d695e039f59b71caa4d730f4e1f84fc97
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337945"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Azure SQL Database beheerd exemplaar T-SQL-verschillen met SQL Server 

Azure SQL Database beheerd-instantie (preview) biedt hoge compatibiliteit met lokale SQL Server Database Engine. De meeste van de SQL Server Database Engine-functies worden ondersteund in Managed-exemplaar. Omdat er nog een paar verschillen in de syntaxis en het gedrag, wordt in dit artikel bevat een overzicht van en worden deze verschillen uitgelegd.
 - [T-SQL-verschillen en niet-ondersteunde functies](#Differences)
 - [Functies die verschillend gedrag in beheerde exemplaar hebt](#Changes)
 - [Bekende problemen en tijdelijke beperkingen](#Issues)

## <a name="Differences"></a> De verschillen T-SQL met SQL Server 

Deze sectie bevat een overzicht van de belangrijkste verschillen in T-SQL-syntaxis en het gedrag tussen exemplaar beheerd en lokale SQL Server Database Engine, evenals de niet-ondersteunde functies.

### <a name="always-on-availability"></a>Altijd op beschikbaarheid

[Hoge beschikbaarheid](sql-database-high-availability.md) is ingebouwd in beheerde exemplaar en niet door gebruikers worden beheerd. De volgende instructies worden niet ondersteund:
 - [EINDPUNT MAKEN... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
 - [BESCHIKBAARHEIDSGROEP MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
 - [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
 - [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
 - [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) -component van de [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) instructie

### <a name="auditing"></a>Controleren 
 
De belangrijkste verschillen tussen de SQL-Audit op beheerde exemplaar, Azure SQL Database en SQL Server on-premises zijn:
- Beheerde exemplaar SQL Audit werkt op het serverniveau en de winkels `.xel` bestanden op Azure blob storage-account.  
- In Azure SQL Database werkt SQL Audit op databaseniveau.
- In SQL Server on-premises, / virtuele machine, SQL Audit werkt op serverniveau, maar gebeurtenissen op system/windows-gebeurtenislogboeken bestanden opslaat.  
  
XEvent-controle in beheerde exemplaar ondersteunt Azure blob storage-doelen. Bestands- en windows logboeken worden niet ondersteund.    
 
De sleutel verschillen de `CREATE AUDIT` syntaxis voor controle naar Azure blob storage zijn:
- Een nieuwe syntaxis `TO URL` wordt aangeboden en kunt u de URL van de Azure blob Storage-container op te geven waar `.xel` bestanden kunnen worden geplaatst 
- De syntaxis `TO FILE` wordt niet ondersteund omdat het beheerde-exemplaar heeft geen toegang tot Windows-bestandsshares. 
 
Zie voor meer informatie:  
- [CONTROLE VAN DE SERVER MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql) 
- [Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)     

### <a name="backup"></a>Backup 

Beheerde exemplaar automatische back-ups en kunnen gebruikers volledige database maken `COPY_ONLY` back-ups. Differentiële, het logboek en bestand momentopnameback-ups worden niet ondersteund.  
- Beheerde exemplaar kan back-up van een database alleen aan een Azure Blob Storage-account: 
 - Alleen `BACKUP TO URL` wordt ondersteund 
 - `FILE`, `TAPE`, en back-upapparaten worden niet ondersteund.  
- De meeste van de algemene `WITH` opties worden ondersteund 
 - `COPY_ONLY` is verplicht
 - `FILE_SNAPSHOT` Niet ondersteund  
 - Tapeopties: `REWIND`, `NOREWIND`, `UNLOAD`, en `NOUNLOAD` worden niet ondersteund 
 - Logboek-specifieke opties: `NORECOVERY`, `STANDBY`, en `NO_TRUNCATE` worden niet ondersteund 
 
Beperkingen:  
- Beheerde exemplaar back-up een database naar een back-up met maximaal 32 striping die voldoende voor de databases is maximaal 4 TB als back-compressie wordt gebruikt.
- Maximum aantal back-stripe-grootte is 195 GB (maximum aantal blobgrootte). Het aantal striping in de opdracht backup afzonderlijke stripe verkleinen en blijven binnen deze limiet verhogen. 

> [!TIP]
> Deze beperking on-premises back-up naar omzeilen `DISK` in plaats van de back-up naar `URL`, back-upbestand voor zet vervolgens blob uploaden. Biedt ondersteuning voor grotere bestanden niet herstellen omdat een andere blob-type wordt gebruikt.  

Zie voor meer informatie over back-ups met T-SQL [back-up](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>De buffergroepuitbreiding 
 
- [Uitbreiding van de buffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wordt niet ondersteund.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wordt niet ondersteund. Zie [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql). 
 
### <a name="bulk-insert--openrowset"></a>Bulksgewijs invoegen / openrowset

Beheerde exemplaar heeft geen toegang tot bestandsshares en Windows-mappen, zodat de bestanden moeten worden geïmporteerd uit Azure blob-opslag.
- `DATASOURCE` is vereist in `BULK INSERT` opdracht tijdens het importeren van bestanden uit Azure blob-opslag. Zie [BULKSGEWIJS invoegen](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` is vereist in `OPENROWSET` wanneer u een inhoud van een bestand te lezen uit Azure blob-opslag. Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
 
### <a name="certificates"></a>Certificaten 

Beheerde exemplaar heeft geen toegang tot bestandsshares en Windows-mappen, zodat de volgende beperkingen toepassen: 
- `CREATE FROM`/`BACKUP TO` bestand wordt niet ondersteund voor certificaten
- `CREATE`/`BACKUP` certificaat van `FILE` / `ASSEMBLY` wordt niet ondersteund. Bestanden met persoonlijke sleutel kunnen niet worden gebruikt.  
 
Zie [maken certificaat](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) en [back-certificaat](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
> [!TIP]
> Tijdelijke oplossing: script certificaat en persoonlijke sleutel, als .sql-bestand opslaan en maken van binaire gegevens: 
> 
> ``` 
CREATE CERTIFICATE  
 FROM BINARY = asn_encoded_certificate    
WITH PRIVATE KEY ( <private_key_options> ) 
>```   
 
### <a name="clr"></a>CLR 

Beheerde exemplaar heeft geen toegang tot bestandsshares en Windows-mappen, zodat de volgende beperkingen toepassen: 
- Alleen `CREATE ASSEMBLY FROM BINARY` wordt ondersteund. Zie [ASSEMBLY maken van BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` wordt niet ondersteund. Zie [ASSEMBLY maken vanuit bestand](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kan niet verwijzen naar bestanden. Zie [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).
 
### <a name="compatibility-levels"></a>Compatibiliteitsniveaus 
 
- Ondersteunde compatibiliteitsniveaus zijn: 100, 110, 120, 130, 140  
- Compatibiliteitsniveaus onder de 100 worden niet ondersteund. 
- Het compatibiliteitsniveau van de standaardwaarde voor nieuwe databases is 140. Voor de herstelde databases blijft compatibiliteitsniveau ongewijzigd als deze 100 en hoger.

Zie [ALTER databasecompatibiliteitsniveau](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).
 
### <a name="credential"></a>Referentie 
 
Alleen Azure Sleutelkluis en `SHARED ACCESS SIGNATURE` identiteiten worden ondersteund. Windows-gebruikers worden niet ondersteund.
 
Zie [referentie maken](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) en [ALTER referentie](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql). 
 
### <a name="cryptographic-providers"></a>Cryptografische providers

Beheerde exemplaar heeft geen toegang tot bestanden zodat cryptografieproviders kunnen niet worden gemaakt:
- `CREATE CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [maken CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [ALTER CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql). 

### <a name="collation"></a>Sortering 
 
De serversortering van de wordt `SQL_Latin1_General_CP1_CI_AS` en kan niet worden gewijzigd. Zie [sorteringen](https://docs.microsoft.com/sql/t-sql/statements/collations).
 
### <a name="database-options"></a>Database-opties 
 
- Meerdere logboekbestanden worden niet ondersteund. 
- Objecten in het geheugen worden niet ondersteund in de servicetier voor algemene doeleinden.  
- Er is een limiet van 280 bestanden per exemplaar: max 280 bestanden per database voor de overdracht. Gegevens en de logboekbestanden tellen mee in deze limiet.  
- Database kan geen bestandsgroepen met filestream-gegevens bevatten.  Herstel mislukt als .bak bevat `FILESTREAM` gegevens.  
- Elk bestand is in Azure Premium-opslag geplaatst. I/o en doorvoer per bestand afhankelijk van de grootte van elk afzonderlijk bestand op dezelfde manier als bij Azure Premium-opslag-schijven. Zie [schijfprestaties Azure Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  
 
#### <a name="create-database-statement"></a>Instructie CREATE DATABASE

Hieronder vindt u `CREATE DATABASE` beperkingen: 
- Bestanden en bestandsgroepen kan niet worden gedefinieerd.  
- `CONTAINMENT` optie wordt niet ondersteund.  
- `WITH`opties worden niet ondersteund.  
   > [!TIP]
   > Gebruik als tijdelijke oplossing `ALTER DATABASE` nadat `CREATE DATABASE` databaseopties bestanden toe te voegen of containment instellen instellen.  

- `FOR ATTACH` optie wordt niet ondersteund 
- `AS SNAPSHOT OF` optie wordt niet ondersteund 

Zie voor meer informatie [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instructie ALTER DATABASE

Sommige eigenschappen van het bestand kunnen niet worden ingesteld of gewijzigd:
- Het pad kan niet worden opgegeven `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL-instructie. Verwijder `FILENAME` plaatst de bestanden uit het script omdat beheerde automatisch-exemplaar.  
- Bestandsnaam kan niet worden gewijzigd met `ALTER DATABASE` instructie.

De volgende opties worden standaard ingesteld en kunnen niet worden gewijzigd: 
- `MULTI_USER` 
- `ENABLE_BROKER ON` 
- `AUTO_CLOSE OFF` 

De volgende opties kunnen niet worden gewijzigd: 
- `AUTO_CLOSE` 
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)` 
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)` 
- `DISABLE_BROKER` 
- `EMERGENCY` 
- `ENABLE_BROKER` 
- `FILESTREAM` 
- `HADR`   
- `NEW_BROKER` 
- `OFFLINE` 
- `PAGE_VERIFY` 
- `PARTNER` 
- `READ_ONLY` 
- `RECOVERY BULK_LOGGED` 
- `RECOVERY_SIMPLE` 
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER` 
- `SINGLE_USER` 
- `WITNESS`

Wijzig de naam wordt niet ondersteund.

Zie voor meer informatie [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Databasespiegeling

Databasespiegeling wordt niet ondersteund.
 - `ALTER DATABASE SET PARTNER` en `SET WITNESS` opties worden niet ondersteund.
 - `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wordt niet ondersteund.

Zie voor meer informatie [ALTER DATABASE SET PARTNER en SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) en [ENDPOINT maken... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC 
 
Niet-gedocumenteerde DBCC-instructies die zijn ingeschakeld in SQL Server worden niet ondersteund in Managed-exemplaar.
- `Trace Flags` worden niet ondersteund. Zie [Trace-vlaggen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` wordt niet ondersteund. Zie [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` wordt niet ondersteund. Zie [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Gedistribueerde transacties

Geen van beide MSDTC noch [elastische transacties](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) worden momenteel ondersteund in Managed-exemplaar.

### <a name="extended-events"></a>Uitgebreide gebeurtenissen 

Sommige Windows-specifieke doelen voor XEvents worden niet ondersteund:
- `etw_classic_sync target` wordt niet ondersteund. Store `.xel` bestanden op Azure-blobopslag. Zie [etw_classic_sync doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target). 
- `event_file target`wordt niet ondersteund. Store `.xel` bestanden op Azure-blobopslag. Zie [event_file doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Externe bibliotheken

In-database R- en Python externe bibliotheken worden nog niet ondersteund. Zie [SQL Server-Machine Learning-Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM en bestandstabel

- FILESTREAM-gegevens wordt niet ondersteund. 
- Database mag niet bestandsgroepen met `FILESTREAM` gegevens
- `FILETABLE` wordt niet ondersteund
- Kunnen geen tabellen hebben `FILESTREAM` typen
- De volgende functies worden niet ondersteund:
 - `GetPathLocator()` 
 - `GET_FILESTREAM_TRANSACTION_CONTEXT()` 
 - `PathName()` 
 - `GetFileNamespacePath()` 
 - `FileTableRootPath()` 

Zie voor meer informatie [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) en [Bestandstabellen](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantische zoekopdracht in volledige tekst

[Semantische zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) wordt niet ondersteund.

### <a name="linked-servers"></a>Gekoppelde servers
 
Gekoppelde servers in beheerde exemplaar ondersteunen een beperkt aantal doelen: 
- Doelen ondersteund: SQL Server en SQL-Database
- Niet ondersteund doelen: bestanden, Analysis Services en andere RDBMS.

Bewerkingen

- Cross-instance schrijftransacties worden niet ondersteund.
- `sp_dropserver` wordt ondersteund voor het verwijderen van een gekoppelde server. Zie [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` de functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren (ofwel beheerd, on-premises of in virtuele Machines). Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` de functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren (ofwel beheerd, on-premises of in virtuele machines). Alleen `SQLNCLI`, `SQLNCLI11`, en `SQLOLEDB` waarden als provider worden ondersteund. Bijvoorbeeld: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Zie [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
 
### <a name="logins--users"></a>Aanmeldingen / gebruikers 

- SQL-aanmeldingen gemaakt `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, en `FROM SID` worden ondersteund. Zie [maken aanmelding](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Windows-aanmeldingen die zijn gemaakt met `CREATE LOGIN ... FROM WINDOWS` syntaxis worden niet ondersteund.
- Azure Active Directory (Azure AD) gebruiker van het exemplaar gemaakt is [beheerdersbevoegdheden onbeperkte](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Azure Active Directory (Azure AD) databaseniveau andere gebruikers dan beheerders kunnen worden gemaakt met `CREATE USER ... FROM EXTERNAL PROVIDER` syntaxis. Zie [gebruiker maken... VAN DE EXTERNE PROVIDER](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)
 
### <a name="polybase"></a>PolyBase

Externe tabellen die verwijzen naar de HDFS- of Azure blob-opslag-bestanden worden niet ondersteund. Zie voor meer informatie over Polybase [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicatie 
 
Replicatie is nog niet ondersteund. Zie voor meer informatie over replicatie [SQL Server-replicatie](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication).
 
### <a name="restore-statement"></a>De instructie RESTORE 
 
- Ondersteunde syntaxis  
   - `RESTORE DATABASE` 
   - `RESTORE FILELISTONLY ONLY` 
   - `RESTORE HEADER ONLY` 
   - `RESTORE LABELONLY ONLY` 
   - `RESTORE VERIFYONLY ONLY` 
- Niet-ondersteunde syntaxis 
   - `RESTORE LOG ONLY` 
   - `RESTORE REWINDONLY ONLY`
- Bron  
 - `FROM URL` (Azure-blobopslag) is alleen ondersteunde optie.
 - `FROM DISK`/`TAPE`-back-upapparaat wordt niet ondersteund.
 - Back-upsets worden niet ondersteund. 
- `WITH` opties worden niet ondersteund (Nee `DIFFERENTIAL`, `STATS`, enz.)     
- `ASYNC RESTORE` -Herstel wordt voortgezet zelfs als de clientverbinding wordt verbroken. Als de verbinding wordt verbroken, kunt u controleren `sys.dm_operation_status` weergave van de status van een herstelbewerking (evenals voor database maken en verwijderen). Zie [sys.dm_operation_status bevat](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  
 
De volgende databaseopties zijn ingesteld/genegeerd en later kunnen niet worden gewijzigd:  
- `NEW_BROKER` (als de broker is niet ingeschakeld in .bak-bestand)  
- `ENABLE_BROKER` (als de broker is niet ingeschakeld in .bak-bestand)  
- `AUTO_CLOSE=OFF` (als een database in .bak-bestand heeft `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (als een database in .bak-bestand heeft `SIMPLE` of `BULK_LOGGED` herstelmodus)
- Geoptimaliseerd voor geheugen bestandsgroep is toegevoegd en XTP aangeroepen als deze niet in het bronbestand .bak  
- Eventuele bestaande geoptimaliseerd voor geheugen-bestandsgroep is gewijzigd in XTP  
- `SINGLE_USER` en `RESTRICTED_USER` opties worden geconverteerd naar `MULTI_USER`   
Beperkingen:  
- `.BAK` bestanden met meerdere back-upsets kunnen niet worden hersteld. 
- `.BAK` bestanden met meerdere logboekbestanden kunnen niet worden hersteld. 
- Herstel mislukt als .bak bevat `FILESTREAM` gegevens.
- Back-ups met databases die objecten van de actieve geheugen hebben kunnen niet op dit moment worden hersteld.  
- Back-ups met databases waar op een bepaald moment In-Memory objecten bestond kunnen niet momenteel worden hersteld.   
- Back-ups met databases in de modus alleen-lezen kunnen niet op dit moment worden hersteld. Deze beperking wordt binnenkort verwijderd.   
 
Zie voor meer informatie over herstel instructies [instructies RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service broker 
 
- Cross-instance service broker wordt niet ondersteund. 
 - `sys.routes` -Voorwaarde: adres selecteren in sys.routes. Adres moet lokaal op elke route. Zie [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
 - `CREATE ROUTE` -u kunt geen `CREATE ROUTE` met `ADDRESS` dan `LOCAL`. Zie [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
 - `ALTER ROUTE` kan geen `ALTER ROUTE` met `ADDRESS` dan `LOCAL`. Zie [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  
 
### <a name="service-key-and-service-master-key"></a>Hoofdsleutel voor key en service-service 
 
- [Back-up van de hoofdsleutel van](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (beheerd door de SQL Database-service) wordt niet ondersteund 
- [Restore master key](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (beheerd door de SQL Database-service) wordt niet ondersteund 
- [Back-up van service hoofdsleutel](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (beheerd door de SQL Database-service) wordt niet ondersteund 
- [Terugzetten van de hoofdsleutel van de service](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (beheerd door de SQL Database-service) wordt niet ondersteund 
 
### <a name="stored-procedures-functions-triggers"></a>Opgeslagen procedures, functies, triggers 
 
- `NATIVE_COMPILATION` wordt momenteel niet ondersteund. 
- De volgende [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) opties worden niet ondersteund: 
 - `allow polybase export` 
 - `allow updates` 
 - `filestream_access_level` 
 - `max text repl size` 
 - `remote data archive` 
 - `remote proc trans` 
- `sp_execute_external_scripts` wordt niet ondersteund. Zie [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` wordt niet ondersteund. Zie [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` worden niet ondersteund, met inbegrip van `sp_addextendedproc` en `sp_dropextendedproc`. Zie [uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, en `sp_detach_db` worden niet ondersteund. Zie [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), en [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` wordt niet ondersteund. Zie [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>SQL Server Agent 
 
- SQL Agent-instellingen zijn alleen-lezen. Procedure `sp_set_agent_properties` wordt niet ondersteund in Managed-exemplaar.  
- Taken - alleen de stappen van T-SQL-taak zijn momenteel ondersteunde (meer stappen worden toegevoegd tijdens de openbare preview).
 - SSIS is nog niet ondersteund. 
 - Replicatie wordt nog niet ondersteund.  
  - Logboeklezer voor de transactie is nog niet ondersteund.  
  - Momentopname is nog niet ondersteund.  
  - Distributor is nog niet ondersteund.  
  - Samenvoegen wordt niet ondersteund.  
  - De wachtrijweergave wordt niet ondersteund.  
 - Opdrachtshell is nog niet ondersteund. 
  - Beheerde exemplaar heeft geen toegang tot externe bronnen (bijvoorbeeld netwerkshares via robocopy).  
 - PowerShell is nog niet ondersteund.
 - Analyseservices worden niet ondersteund.  
- Meldingen worden gedeeltelijk ondersteund.
 - E-mailmelding wordt ondersteund, vereist een Database-e-mailprofiel te configureren. Er kan slechts één database-e-mailprofiel en moet worden aangeroepen `AzureManagedInstance_dbmail_profile` openbare Preview (tijdelijke beperking).  
 - Pager wordt niet ondersteund.  
 - Net Send wordt niet ondersteund. 
 - Waarschuwingen worden nog niet niet ondersteund.
 - Proxy's worden niet ondersteund.  
- Gebeurtenislogboek wordt niet ondersteund. 
 
De volgende functies worden momenteel niet ondersteund, maar in de toekomst worden ingeschakeld:  
- Proxy's
- Plannen van taken op niet-actieve CPU 
- Agent in-/ uitschakelen
- Waarschuwingen

Zie voor meer informatie over SQL Server Agent [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).
 
### <a name="tables"></a>Tabellen 

Het volgende worden niet ondersteund: 
- `FILESTREAM` 
- `FILETABLE` 
- `EXTERNAL TABLE` 
- `MEMORY_OPTIMIZED`  

Zie voor meer informatie over het maken en wijzigen van tabellen [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) en [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).
 
## <a name="Changes"></a> Gedragswijzigingen 
 
De volgende variabelen, taken en weergaven retourneren andere resultaten:  
- `SERVERPROPERTY('EngineEdition')` retourneert de waarde 8. Deze eigenschap is uniek exemplaar beheerd. Zie [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` retourneert NULL, omdat het concept van een exemplaar als deze bestaat voor SQL Server is niet van toepassing met beheerde-exemplaar. Zie [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` retourneert volledige DNS-'koppelbaar' naam, bijvoorbeeld Mijn-managed-instance.wcus17662feb9ce98.database.windows.net. Zie [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -retourneert volledige DNS-naam 'koppelbaar' zoals `myinstance.domain.database.windows.net` voor eigenschappen 'name' en 'data_source'. Zie [SYS. SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVICENAME` retourneert NULL, omdat het concept van een service die voor SQL Server niet van toepassing op beheerde exemplaar bestaat. Zie [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).   
- `SUSER_ID` wordt ondersteund. Retourneert NULL als AAD-aanmelding niet in sys.syslogins is. Zie [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` wordt niet ondersteund. Retourneert onjuiste gegevens (tijdelijke bekend probleem). Zie [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 
- `GETDATE()` en andere functies ingebouwde datum/tijd altijd retourneert tijd in UTC-tijdzone. Zie [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bekende problemen en beperkingen

### <a name="tempdb-size"></a>De grootte van TEMPDB

`tempdb` 12 opgesplitst bestanden met de maximale grootte 14 GB per bestand. Deze maximale grootte per bestand kan niet worden gewijzigd en nieuwe bestanden kunnen niet worden toegevoegd aan `tempdb`. Deze beperking wordt binnenkort verwijderd. Sommige query's mogelijk een fout geretourneerd als ze nodig hebben meer dan 168 GB in `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Meer dan opslagruimte met kleine databasebestanden

Elk exemplaar beheerd heeft naar 35 TB opslagruimte is gereserveerd voor de schijfruimte van Azure Premium en elk databasebestand op een afzonderlijke fysieke schijf is geplaatst. Schijfgrootte kunnen 128 GB, 256 GB, 512 GB, 1 TB of 4 TB zijn. Ongebruikte ruimte op schijf niet in rekening gebracht, maar de totale som van Azure Premium-schijfgrootte niet langer zijn dan 35 TB. In sommige gevallen kunt u een beheerde-exemplaar dat niet 8 TB in totaal hoeft de 35 TB Azure de limiet voor de grootte van de opslagruimte, vanwege de fragmentatie van de interne overschrijden. 

Een exemplaar beheerd kan bijvoorbeeld een bestand met 1,2 TB grootte die gebruikmaakt van een schijf 4 TB en 248 bestanden met 1 GB elk die worden geplaatst op 248 schijven met een grootte van 128 GB. In dit voorbeeld is de totale schijfgrootte opslag 1 x 4 TB + 248 x 128 GB = 35 TB. Totaal aantal gereserveerde exemplaargrootte voor databases is echter 1 x 1.2 TB + 248 x 1 GB = 1,4 TB. Dit ziet u dat onder bepaalde omstandigheden, als gevolg van een specifieke distributie van bestanden, een exemplaar beheerd Azure Premium-schijf opslaglimiet waar u niet verwacht het dat mogelijk mogelijk bereikt. 

Er is geen fout op bestaande databases en ze kunnen zonder probleem worden uitgebreid als nieuwe bestanden zijn niet toegevoegd, maar de nieuwe databases kunnen niet worden gemaakt of hersteld omdat er niet voldoende ruimte voor nieuwe schijfstations, zelfs als de totale grootte van alle databases t niet bereikt hij groottelimiet-exemplaar. De fout die wordt geretourneerd is in dat geval niet wissen.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Onjuiste configuratie van SAS-sleutel tijdens de database herstellen

`RESTORE DATABASE` die leest .bak-bestand mogelijk worden voortdurend wordt opnieuw geprobeerd te lezen .bak-bestand en de geretourneerde fout na lange periode als Shared Access Signature in `CREDENTIAL` is onjuist. UPSETS uitvoeren voordat het terugzetten van een database om er zeker van te zijn dat de SAS-sleutel juist is.
Zorg ervoor dat u voorloopspaties verwijdert `?` van de SAS-sleutel gegenereerd met behulp van Azure-portal.

### <a name="tooling"></a>Tooling

SQL Server Management Studio en SQL Server Data Tools wellicht enkele problemen bij het openen van beheerd exemplaar. Alle tooling problemen zullen worden besteed vóór het algemene beschikbaarheid.

### <a name="incorrect-database-names"></a>Onjuiste databasenamen

Beheerde exemplaar mogelijk guid-waarde in plaats van de databasenaam tijdens het terugzetten of in een aantal foutberichten weergeven. Deze problemen worden gecorrigeerd voordat de algemene beschikbaarheid.

### <a name="database-mail-profile"></a>Database-e-mailprofiel
Er kan slechts één database-e-mailprofiel en moet worden aangeroepen `AzureManagedInstance_dbmail_profile`. Dit is een tijdelijke beperking die snel worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaar [wat is er een exemplaar beheerd?](sql-database-managed-instance.md)
- Voor een functies en vergelijkingslijst Zie [algemene functies van SQL](sql-database-features.md).
- Zie voor een zelfstudie waarin u wordt getoond hoe u een nieuw exemplaar van beheerde [het maken van een exemplaar beheerd](sql-database-managed-instance-create-tutorial-portal.md).
