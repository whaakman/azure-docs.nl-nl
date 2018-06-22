---
title: Maken en beheren van Azure SQL elastische Database taken met Transact-SQL (T-SQL) | Microsoft Docs
description: Scripts uitvoeren tussen meerdere databases met elastische Database-taak agent met Transact-SQL (T-SQL).
services: sql-database
author: jaredmoo
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 06/14/2018
ms.author: jaredmoo
ms.openlocfilehash: fb6e4ebd635d8afa8e679ee5bb0f5646f28f887b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313337"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Transact-SQL (T-SQL) gebruiken om te maken en beheren van taken voor elastische Database

Dit artikel bevat veel voorbeeldscenario's werken met elastische taken aan de slag met T-SQL.

De voorbeelden gebruiken de [opgeslagen procedures](#job-stored-procedures) en [weergaven](#job-views) beschikbaar in de [ *taak database*](elastic-jobs-overview.md#job-database).

Transact-SQL (T-SQL) wordt gebruikt voor het maken, configureren, uitvoeren en beheren van taken. Maken van de agent elastische taak wordt niet ondersteund in T-SQL, dus moet u eerst maken een *elastische taak agent* via de portal of [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).


## <a name="create-a-credential-for-job-execution"></a>Een referentie voor de taakuitvoering van een maken

De referentie wordt gebruikt om verbinding maken met de doeldatabases voor uitvoering van het script. De referentie moet de juiste machtigingen van de databases die zijn opgegeven door de doelgroep voor het script kunnen uitvoeren. Wanneer u een server en/of het doel een lid van groep van toepassingen, ten zeerste aangeraden wordt een master-referentie voor vernieuwen van de referentie voor uitbreiding van de server en/of de groep van toepassingen tijdens het uitvoeren van taak maken. De-scoped databasereferentie wordt gemaakt op de database van de agent taak. De dezelfde referenties moet worden gebruikt voor het *een aanmelding maken* en *maken van een gebruiker van aanmelding bij de aanmelding databasemachtigingen verlenen* op de doeldatabases.


```sql
--Connect to the job database specified when creating the job agent

-- Create a db master key if one does not already exist, using your own password.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';  
  
-- Create a database scoped credential.  
CREATE DATABASE SCOPED CREDENTIAL myjobcred WITH IDENTITY = 'jobcred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO

-- Create a database scoped credential for the master database of server1.
CREATE DATABASE SCOPED CREDENTIAL mymastercred WITH IDENTITY = 'mastercred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO
```

## <a name="create-a-target-group-servers"></a>Maken van een doelgroep (servers)

Het volgende voorbeeld laat zien hoe een taak op alle databases in een server uitvoeren.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:


```sql
-- Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group 'ServerGroup1'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
'ServerGroup1',
@target_type = 'SqlServer',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net'

--View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name='ServerGroup1';
SELECT * FROM jobs.target_group_members WHERE target_group_name='ServerGroup1';
```


## <a name="exclude-a-single-database"></a>Uitsluiten van een individuele database

Het volgende voorbeeld ziet u het uitvoeren van een taak op alle databases in een server, met uitzondering van de database met de naam *MappingDB*.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group = N'ServerGroup'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name='server2.database.windows.net'
GO

--Excude a database target member from the server target group
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@membership_type = N'Exclude',
@target_type = N'SqlDatabase',
@server_name = N'server1.database.windows.net',
@database_name =N'MappingDB'
GO

--View the recently created target group and target group members
SELECT * FROM [jobs].target_groups WHERE target_group_name = N'ServerGroup';
SELECT * FROM [jobs].target_group_members WHERE target_group_name = N'ServerGroup';
```


## <a name="create-a-target-group-pools"></a>Maken van een doelgroep (groepen)

Het volgende voorbeeld laat zien hoe toe te passen van alle databases in een of meer elastische pools.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing pool(s)
EXEC jobs.sp_add_target_group 'PoolGroup'

-- Add an elastic pool(s) target member
EXEC jobs.sp_add_target_group_member
'PoolGroup',
@target_type = 'SqlElasticPool',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net',
@elastic_pool_name='ElasticPool-1'

-- View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name = N'PoolGroup';
SELECT * FROM jobs.target_group_members WHERE target_group_name = N'PoolGroup';
```


## <a name="deploy-new-schema-to-many-databases"></a>Nieuw schema te veel databases implementeren

Het volgende voorbeeld ziet het nieuwe schema implementeren op alle databases.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:


```sql
--Connect to the job database specified when creating the job agent

--Add job for create table
EXEC jobs.sp_add_job @job_name='CreateTableTest', @description='Create Table Test'

-- Add job step for create table
EXEC jobs.sp_add_jobstep @job_name='CreateTableTest',
@command=N'IF NOT EXISTS (SELECT * FROM sys.tables 
            WHERE object_id = object_id(''Test''))
CREATE TABLE [dbo].[Test]([TestId] [int] NOT NULL);',
@credential_name='myjobcred',
@target_group_name='PoolGroup'
```


## <a name="data-collection-using-built-in-parameters"></a>Verzamelen van gegevens met behulp van ingebouwde parameters

In veel gegevens verzameling scenario's, kan het handig zijn om op te nemen van sommige scripting variabelen om te na de resultaten van de taak te verwerken.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Bijvoorbeeld: als u wilt groeperen alle resultaten van de samen dezelfde taak wordt uitgevoerd, gebruiken de *$(job_execution_id)* zoals weergegeven in de volgende opdracht:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Databaseprestaties bewaken

Het volgende voorbeeld wordt een nieuwe taak voor het verzamelen van prestatiegegevens uit meerdere databases.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdrachten:

```sql
--Connect to the job database specified when creating the job agent

-- Add a job to collect perf results
EXEC jobs.sp_add_job @job_name ='ResultsJob', @description='Collection Performance data from all customers'

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsJob',
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());',
@credential_name='myjobcred',
@target_group_name='PoolGroup',
@output_type='SqlDatabase',
@output_credential_name=’myjobcred’,
@output_server_name=’server1.database.windows.net',
@output_database_name=’<resultsdb>',
@output_table_name='<resutlstable>'
Create a job to monitor pool performance
--Connect to the job database specified when creating the job agent

-- Add a target group containing master database
EXEC jobs.sp_add_target_group 'MasterGroup'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name='MasterGroup',
@target_type='SqlDatabase',
@server_name='server1.database.windows.net',
@database_name='master'

-- Add a job to collect perf results
EXEC jobs.sp_add_job
@job_name='ResultsPoolsJob',
@description='Demo: Collection Performance data from all pools',
@schedule_interval_type='Minutes',
@schedule_interval_count=15

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsPoolsJob',
@command=N'declare @now datetime
DECLARE @startTime datetime
DECLARE @endTime datetime
DECLARE @poolLagMinutes datetime
DECLARE @poolStartTime datetime
DECLARE @poolEndTime datetime
SELECT @now = getutcdate ()
SELECT @startTime = dateadd(minute, -15, @now)
SELECT @endTime = @now
SELECT @poolStartTime = dateadd(minute, -30, @startTime)
SELECT @poolEndTime = dateadd(minute, -30, @endTime)

SELECT elastic_pool_name , end_time, elastic_pool_dtu_limit, avg_cpu_percent, avg_data_io_percent, avg_log_write_percent, max_worker_percent, max_session_percent,
        avg_storage_percent, elastic_pool_storage_limit_mb FROM sys.elastic_pool_resource_stats
        WHERE end_time > @poolStartTime and end_time <= @poolEndTime;
'),
@credential_name='myjobcred',
@target_group_name='MasterGroup',
@output_type='SqlDatabase',
@output_credential_name='myjobcred',
@output_server_name=’server1.database.windows.net',
@output_database_name=’resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>Definities van de taak weergeven

Het volgende voorbeeld laat zien hoe om huidige taakdefinities weer te geven.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- View all jobs
SELECT * FROM jobs.jobs

-- View the steps of the current version of all jobs
SELECT js.* FROM jobs.jobsteps js
JOIN jobs.jobs j 
  ON j.job_id = js.job_id AND j.job_version = js.job_version

-- View the steps of all versions of all jobs
select * from jobs.jobsteps
```


## <a name="begin-ad-hoc-execution-of-a-job"></a>Ad-hoc-uitvoering van een taak begint

Het volgende voorbeeld ziet hoe u een taak onmiddellijk starten.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- Execute the latest version of a job
EXEC jobs.sp_start_job 'CreateTableTest'

-- Execute the latest version of a job and receive the execution id
declare @je uniqueidentifier
exec jobs.sp_start_job 'CreateTableTest', @job_execution_id = @je output
select @je

select * from jobs.job_executions where job_execution_id = @je

-- Execute a specific version of a job (e.g. version 1)
exec jobs.sp_start_job 'CreateTableTest', 1
```


## <a name="schedule-execution-of-a-job"></a>De uitvoering van een taak plannen

Het volgende voorbeeld laat zien hoe een taak voor toekomstige uitvoering plannen.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>Uitvoeringsstatus van taak bewaken

Het volgende voorbeeld laat zien hoe om details van de uitvoering van status voor alle taken weer te geven.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named ‘ResultsPoolJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named ‘ResultsPoolsJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Een taak annuleren

Het volgende voorbeeld laat zien hoe een taak annuleren.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- View all active executions to determine job execution id
SELECT * FROM jobs.job_executions 
WHERE is_active = 1 AND job_name = 'ResultPoolsJob'
ORDER BY start_time DESC
GO

-- Cancel job execution with the specified job execution id
EXEC jobs.sp_stop_job '01234567-89ab-cdef-0123-456789abcdef'
```


## <a name="delete-old-job-history"></a>Oude Taakgeschiedenis verwijderen

Het volgende voorbeeld laat zien hoe Taakgeschiedenis voorafgaand aan een specifieke datum te verwijderen.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Een taak en de taakgeschiedenis verwijderen

Het volgende voorbeeld laat zien hoe een taak te verwijderen en alle gerelateerde Taakgeschiedenis.  
Verbinding maken met de [ *taak database* ](elastic-jobs-overview.md#job-database) en voer de volgende opdracht:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>Taak opgeslagen procedures

De volgende opgeslagen procedures zijn in de [taken database](elastic-jobs-overview.md#job-database).



|Opgeslagen procedure  |Beschrijving  |
|---------|---------|
|[sp_add_job](#spaddjob)     |     Hiermee wordt een nieuwe taak toegevoegd.    |
|[sp_update_job ](#spupdatejob)    |      Een bestaande taak bijgewerkt.   |
|[sp_delete_job](#spdeletejob)     |      Hiermee verwijdert u een bestaande taak.   |
|[sp_add_jobstep](#spaddjobstep)    |    Hiermee voegt u een stap toe aan een job.     |
|[sp_update_jobstep](#spupdatejobstep)     |     Een taakstap-updates.    |
|[sp_delete_jobstep](#spdeletejobstep)     |     Hiermee verwijdert u een taakstap.    |
|[sp_start_job](#spstartjob)    |  Start een taak wordt uitgevoerd.       |
|[sp_stop_job](#spstopjob)     |     Hiermee stopt het uitvoeren van een taak.   |
|[sp_add_target_group](#spaddtargetgroup)    |     Voegt een doelgroep.    |
|[sp_delete_target_group](#spdeletetargetgroup)     |    Hiermee verwijdert u een doelgroep.     |
|[sp_add_target_group_member](#spaddtargetgroupmember)     |    Hiermee voegt u een database of een groep met databases toe aan een doelgroep.     |
|[sp_delete_target_group_member](#spdeletetargetgroupmember)     |     Hiermee verwijdert u een lid van een doel van een doelgroep.    |
|[sp_purge_jobhistory ](#sppurgejobhistory)    |    Hiermee verwijdert u de geschiedenisrecords voor een taak.     |





### <a name="spaddjob"></a>sp_add_job

Hiermee wordt een nieuwe taak toegevoegd. 
  
#### <a name="syntax"></a>Syntaxis  
  

```sql
[jobs].sp_add_job [ @job_name = ] 'job_name'  
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
    [ , [ @job_id = ] job_id OUTPUT ]
```

  
#### <a name="arguments"></a>Argumenten  

[  **@job_name =** ] 'job_name'  
De naam van de taak. De naam moet uniek zijn en het %-teken niet bevatten. job_name is nvarchar(128) met geen standaardwaarde.

[  **@description =** ] 'description'  
De beschrijving van de taak. Beschrijving is nvarchar(512) met een standaard van NULL. Als de beschrijving wordt weggelaten, wordt een lege tekenreeks wordt gebruikt.

[  **@enabled =** ] ingeschakeld  
Hiermee wordt aangegeven of de planning van de taak is ingeschakeld. Ingeschakeld is bits met een standaardwaarde van 0 (uitgeschakeld). Als u 0 opgeeft, wordt de taak is niet ingeschakeld en kan niet worden uitgevoerd volgens schema; echter, het kan handmatig worden uitgevoerd. Als dit 1 is, wordt de taak wordt uitgevoerd volgens schema en kan ook handmatig worden uitgevoerd.

[  **@schedule_interval_type =**] schedule_interval_type  
De waarde geeft aan wanneer de taak wordt uitgevoerd. schedule_interval_type is nvarchar(50) met een standaardwaarde van één keer, en een van de volgende waarden:
- 'Eenmaal'
- 'Minutes'
- 'Hours'
- "Dagen"
- 'Weken'
- 'Maanden'

[  **@schedule_interval_count =** ] schedule_interval_count  
Het aantal perioden schedule_interval_count optreden tussen de uitvoering van de taak. schedule_interval_count is int, met een standaardwaarde van 1. De waarde moet groter zijn dan of gelijk aan 1 zijn.

[  **@schedule_start_time =** ] schedule_start_time  
De datum op welke taak uitvoering kan beginnen. schedule_start_time is DATETIME2, met de standaardwaarde van 01-01-0001 00:00:00.0000000.

[  **@schedule_end_time =** ] schedule_end_time  
De datum op welke taak uitvoering kunt stoppen. schedule_end_time DATETIME2, is de standaard 9999-12-31 11:59:59.0000000. 

[  **@job_id =** ] job_id uitvoer  
Het id-nummer toegewezen aan het project als gemaakt. job_id is een uitvoervariabele van het type uniqueidentifier.

#### <a name="return-code-values"></a>Codewaarden retourneren

0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
sp_add_job moet worden uitgevoerd vanaf de taak agent database die is opgegeven bij het maken van de agent van de taak.
Nadat sp_add_job om toe te voegen van een taak is uitgevoerd, kunt sp_add_jobstep worden gebruikt voor het toevoegen van stappen die de activiteiten worden uitgevoerd voor de taak. Nummer van de initiële versie van de taak is 0, 1 wordt verhoogd wanneer de eerste stap wordt toegevoegd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spupdatejob"></a>sp_update_job

Een bestaande taak bijgewerkt.

#### <a name="syntax"></a>Syntaxis

```sql
[jobs].sp_update_job [ @job_name = ] 'job_name'  
    [ , [ @new_name = ] 'new_name' ]
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak moet worden bijgewerkt. job_name is nvarchar(128).

[  **@new_name =** ] 'nieuwe_naam'  
De nieuwe naam van de taak. nieuwe_naam is nvarchar(128).

[  **@description =** ] 'description'  
De beschrijving van de taak. de beschrijving is nvarchar(512).

[  **@enabled =** ] ingeschakeld  
Hiermee bepaalt u of het taakschema ingeschakeld (1) of niet (0) is ingeschakeld. Ingeschakeld bit is.

[  **@schedule_interval_type=** ] schedule_interval_type  
De waarde geeft aan wanneer de taak wordt uitgevoerd. schedule_interval_type kan nvarchar(50) is en een van de volgende waarden:

- 'Eenmaal'
- 'Minutes'
- 'Hours'
- "Dagen"
- 'Weken'
- 'Maanden'

[  **@schedule_interval_count=** ] schedule_interval_count  
Het aantal perioden schedule_interval_count optreden tussen de uitvoering van de taak. schedule_interval_count is int, met een standaardwaarde van 1. De waarde moet groter zijn dan of gelijk aan 1 zijn.

[  **@schedule_start_time=** ] schedule_start_time  
De datum op welke taak uitvoering kan beginnen. schedule_start_time is DATETIME2, met de standaardwaarde van 01-01-0001 00:00:00.0000000.

[  **@schedule_end_time=** ] schedule_end_time  
De datum op welke taak uitvoering kunt stoppen. schedule_end_time DATETIME2, is de standaard 9999-12-31 11:59:59.0000000. 

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Nadat sp_add_job om toe te voegen van een taak is uitgevoerd, kunt sp_add_jobstep worden gebruikt voor het toevoegen van stappen die de activiteiten worden uitgevoerd voor de taak. Nummer van de initiële versie van de taak is 0, 1 wordt verhoogd wanneer de eerste stap wordt toegevoegd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.



### <a name="spdeletejob"></a>sp_delete_job

Hiermee verwijdert u een bestaande taak.

#### <a name="syntax"></a>Syntaxis

```sql
[jobs].sp_delete_job [ @job_name = ] 'job_name'
    [ , [ @force = ] force ]
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak moet worden verwijderd. job_name is nvarchar(128).

[  **@force =** ] forceren  
Geeft aan of verwijderen als er uitvoeringen van de taak is uitgevoerd en alle lopende uitvoeringen (1) of mislukken te annuleren als u een taak uitvoeringen worden uitgevoerd (0). Force is bits.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Taakgeschiedenis wordt automatisch verwijderd wanneer een taak wordt verwijderd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.



### <a name="spaddjobstep"></a>sp_add_jobstep

Hiermee voegt u een stap toe aan een job.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_add_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] step_name ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_subscription_id = ] 'output_subscription_id' ]   
     [ , [ @output_resource_group_name = ] 'output_resource_group_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Argumenten

[  **@job_name =** ] 'job_name'  
De naam van de taak waarop u wilt toevoegen, de stap. job_name is nvarchar(128).

[  **@step_id =** ] step_id  
De reeks-id-nummer voor de taakstap. Stap identificatienummers begint bij 1 en verhogen zonder onderbrekingen. Als een bestaande stap al deze id, de klikt u vervolgens dat stap en alle volgende stappen uit hun id hebben verhoogd zodat deze nieuwe stap kan worden ingevoegd in de reeks. Als niet wordt opgegeven, wordt de step_id automatisch toegewezen naar de laatste in de volgorde van stappen. step_id is een int.

[  **@step_name =** ] step_name  
De naam van de stap. Moet worden opgegeven, met uitzondering van de eerste stap van een taak die (voor uw gemak bedoeld;) een standaardnaam van de 'taakstap heeft'. step_name is nvarchar(128).

[  **@command_type =** ] 'command_type'  
Het type van de opdracht die door deze taakstap is uitgevoerd. command_type nvarchar(50) met TSql, wat betekent dat de waarde van de standaardwaarde is de @command_type parameter is een T-SQL-script.

Indien opgegeven, is de waarde moet TSql.

[  **@command_source =** ] 'command_source'  
Het type van de locatie waar de opdracht is opgeslagen. command_source nvarchar(50) met Inline, wat betekent dat de waarde van de standaardwaarde is de @command_source parameter is de letterlijke tekst van de opdracht.

Indien opgegeven, is de waarde moet Inline.

[  **@command =** ] 'command'  
De opdracht moet geldige T-SQL-script en vervolgens door deze taakstap wordt uitgevoerd. opdracht is nvarchar(max) met een standaard van NULL.

[  **@credential_name =** ] 'credential_name'  
De naam van de database-scoped referentie opgeslagen in de database van deze taak-besturingselement dat wordt gebruikt voor het verbinding maken met elk van de doeldatabases binnen de doelgroep wanneer deze stap wordt uitgevoerd. credential_name is nvarchar(128).

[  **@target_group_name =** ] 'target-groepsnaam'  
De naam van de doelgroep met de doeldatabases die de taakstap wordt uitgevoerd op. target_group_name is nvarchar(128).

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
De wachttijd voordat de eerste poging, als de taakstap bij de uitvoering van de eerste poging mislukt. initial_retry_interval_seconds is int, met de standaardwaarde van 1.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
De maximale vertraging tussen nieuwe pogingen. Als de vertraging tussen pogingen groter is dan deze waarde toenemen zou, is het in plaats daarvan naar deze waarde beperkt. maximum_retry_interval_seconds is int, met de standaardwaarde van 120.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
De vermenigvuldigingsfactor toepassen op de tijd tussen elke poging als meerdere taak uitvoering van stap probeert mislukken. Bijvoorbeeld als de eerste poging had een vertraging van vijf seconden en de vermenigvuldiger backoff 2.0 is, klikt u vervolgens de tweede poging een vertraging van 10 seconden hebben en het derde opnieuw heeft een vertraging van 20 seconden. retry_interval_backoff_multiplier zijn echte, met de standaardwaarde van 2.0.

[  **@retry_attempts =** ] retry_attempts  
Het aantal keren opnieuw kan worden uitgevoerd als de eerste poging mislukt. Bijvoorbeeld, als de waarde retry_attempts 10, wordt er 1 eerste nieuwe poging en 10 pogingen, geeft een totaal van 11 pogingen. Als de laatste nieuwe poging is mislukt, wordt de taak wordt uitgevoerd beëindigd met een levenscyclus mislukt. retry_attempts is int, met de standaardwaarde van 10.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
De maximale hoeveelheid tijd die zijn toegestaan voor de stap uit te voeren. Als deze tijd wordt overschreden, wordt aan een levenscyclus van een time-out opgetreden bij het uitvoeren van de taak beëindigd. step_timeout_seconds is int, met de standaardwaarde van 43.200 seconden (12 uur).

[  **@output_type =** ] 'output_type'  
Als dat niet null is, het type van de bestemming van de opdracht eerste resultaat ingesteld naar worden geschreven. output_type is nvarchar(50) met een standaard van NULL.

Als u opgeeft, moet de waarde SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Als deze niet leeg is, de naam van de database-scoped referentie die wordt gebruikt voor het verbinding maken met de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. output_credential_name is nvarchar(128) met een standaardwaarde NULL.

[  **@output_subscription_id =** ] 'output_subscription_id'  
Moet een beschrijving.

[  **@output_resource_group_name =** ] 'output_resource_group_name'  
Moet een beschrijving.

[  **@output_server_name =** ] 'output_server_name'  
Als dat niet null, de volledig gekwalificeerde DNS-naam van de server waarop de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. output_server_name is nvarchar(256) met een standaard van NULL.

[  **@output_database_name =** ] 'output_database_name'  
Als dat niet null, de naam van de database waarin de uitvoertabel van de bestemming. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. output_database_name is nvarchar(128) met een standaard van NULL.

[  **@output_schema_name =** ] 'output_schema_name'  
Als dat niet null, de naam van het SQL-schema met de uitvoertabel van de bestemming. Als output_type gelijk is aan SqlDatabase, is de standaardwaarde dbo. output_schema_name is nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Als dat niet null is, wordt de naam van de tabel die de eerste resultaat van de opdracht ingesteld naar worden geschreven. Als de tabel niet al bestaat, wordt deze gemaakt op basis van het schema van de resultatenset geretourneerd. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. output_table_name is nvarchar(128) met een standaardwaarde NULL.

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

[  **@max_parallelism =** ] max_parallelism uitvoer  
Het maximumniveau van parallelle uitvoering per elastische pool. Als de set en vervolgens de taakstap beperkt worden tot alleen uitvoeren op een maximum van die veel databases per elastische pool. Dit geldt voor elke elastische pool die ofwel rechtstreeks is opgenomen in de doelgroep of deel uitmaakt van een server die is opgenomen in de doelgroep. max_parallelism is int.


#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Als sp_add_jobstep is geslaagd, wordt het huidige versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt. Als de taak wordt momenteel uitgevoerd, die kan worden uitgevoerd, hebben geen nieuwe stap.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:  

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.



### <a name="spupdatejobstep"></a>sp_update_jobstep

Een taakstap-updates.

#### <a name="syntax"></a>Syntaxis

```sql
[jobs].sp_update_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @new_id = ] new_id ]   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak die de stap behoort. job_name is nvarchar(128).

[  **@step_id =** ] step_id  
De id-nummer voor de taakstap moet worden gewijzigd. Step_id of step_name moet worden opgegeven. step_id is een int.

[  **@step_name =** ] 'step_name'  
De naam van de stap worden gewijzigd. Step_id of step_name moet worden opgegeven. step_name is nvarchar(128).

[  **@new_id =** ] new_id  
De nieuwe volgorde id-nummer voor de taakstap. Stap identificatienummers begint bij 1 en verhogen zonder onderbrekingen. Als een stap in de volgorde is gewijzigd, klikt u vervolgens andere stappen zal worden automatisch opnieuw genummerd.

[  **@new_name =** ] 'nieuwe_naam'  
De nieuwe naam van de stap. nieuwe_naam is nvarchar(128).

[  **@command_type =** ] 'command_type'  
Het type van de opdracht die door deze taakstap is uitgevoerd. command_type nvarchar(50) met TSql, wat betekent dat de waarde van de standaardwaarde is de @command_type parameter is een T-SQL-script.

Indien opgegeven, is de waarde moet TSql.

[  **@command_source =** ] 'command_source'  
Het type van de locatie waar de opdracht is opgeslagen. command_source nvarchar(50) met Inline, wat betekent dat de waarde van de standaardwaarde is de @command_source parameter is de letterlijke tekst van de opdracht.

Indien opgegeven, is de waarde moet Inline.

[  **@command =** ] 'command'  
De opdrachten zijn moet geldige T-SQL-script en vervolgens door deze taakstap wordt uitgevoerd. opdracht is nvarchar(max) met een standaard van NULL.

[  **@credential_name =** ] 'credential_name'  
De naam van de database-scoped referentie opgeslagen in de database van deze taak-besturingselement dat wordt gebruikt voor het verbinding maken met elk van de doeldatabases binnen de doelgroep wanneer deze stap wordt uitgevoerd. credential_name is nvarchar(128).

[  **@target_group_name =** ] 'target-groepsnaam'  
De naam van de doelgroep met de doeldatabases die de taakstap wordt uitgevoerd op. target_group_name is nvarchar(128).

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
De wachttijd voordat de eerste poging, als de taakstap bij de uitvoering van de eerste poging mislukt. initial_retry_interval_seconds is int, met de standaardwaarde van 1.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
De maximale vertraging tussen nieuwe pogingen. Als de vertraging tussen pogingen groter is dan deze waarde toenemen zou, is het in plaats daarvan naar deze waarde beperkt. maximum_retry_interval_seconds is int, met de standaardwaarde van 120.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
De vermenigvuldigingsfactor toepassen op de tijd tussen elke poging als meerdere taak uitvoering van stap probeert mislukken. Bijvoorbeeld als de eerste poging had een vertraging van vijf seconden en de vermenigvuldiger backoff 2.0 is, klikt u vervolgens de tweede poging een vertraging van 10 seconden hebben en het derde opnieuw heeft een vertraging van 20 seconden. retry_interval_backoff_multiplier zijn echte, met de standaardwaarde van 2.0.

[  **@retry_attempts =** ] retry_attempts  
Het aantal keren opnieuw kan worden uitgevoerd als de eerste poging mislukt. Bijvoorbeeld, als de waarde retry_attempts 10, wordt er 1 eerste nieuwe poging en 10 pogingen, geeft een totaal van 11 pogingen. Als de laatste nieuwe poging is mislukt, wordt de taak wordt uitgevoerd beëindigd met een levenscyclus mislukt. retry_attempts is int, met de standaardwaarde van 10.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
De maximale hoeveelheid tijd die zijn toegestaan voor de stap uit te voeren. Als deze tijd wordt overschreden, wordt aan een levenscyclus van een time-out opgetreden bij het uitvoeren van de taak beëindigd. step_timeout_seconds is int, met de standaardwaarde van 43.200 seconden (12 uur).

[  **@output_type =** ] 'output_type'  
Als dat niet null is, het type van de bestemming van de opdracht eerste resultaat ingesteld naar worden geschreven. Als u wilt de waarde van output_type opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_type is nvarchar(50) met een standaard van NULL.

Als u opgeeft, moet de waarde SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Als deze niet leeg is, de naam van de database-scoped referentie die wordt gebruikt voor het verbinding maken met de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. Als u wilt de waarde van output_credential_name opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_credential_name is nvarchar(128) met een standaardwaarde NULL.

[  **@output_server_name =** ] 'output_server_name'  
Als dat niet null, de volledig gekwalificeerde DNS-naam van de server waarop de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. Als u wilt de waarde van output_server_name opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_server_name is nvarchar(256) met een standaard van NULL.

[  **@output_database_name =** ] 'output_database_name'  
Als dat niet null, de naam van de database waarin de uitvoertabel van de bestemming. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. Als u wilt de waarde van output_database_name opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_database_name is nvarchar(128) met een standaard van NULL.

[  **@output_schema_name =** ] 'output_schema_name'  
Als dat niet null, de naam van het SQL-schema met de uitvoertabel van de bestemming. Als output_type gelijk is aan SqlDatabase, is de standaardwaarde dbo. Als u wilt de waarde van output_schema_name opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_schema_name is nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Als dat niet null is, wordt de naam van de tabel die de eerste resultaat van de opdracht ingesteld naar worden geschreven. Als de tabel niet al bestaat, wordt deze gemaakt op basis van het schema van de resultatenset geretourneerd. Moet worden opgegeven als output_type SqlDatabase gelijk is aan. Als u wilt de waarde van output_server_name opnieuw instellen op NULL, stelt u de waarde van deze parameter in op '' (lege tekenreeks). output_table_name is nvarchar(128) met een standaardwaarde NULL.

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

[  **@max_parallelism =** ] max_parallelism uitvoer  
Het maximumniveau van parallelle uitvoering per elastische pool. Als de set en vervolgens de taakstap beperkt worden tot alleen uitvoeren op een maximum van die veel databases per elastische pool. Dit geldt voor elke elastische pool die ofwel rechtstreeks is opgenomen in de doelgroep of deel uitmaakt van een server die is opgenomen in de doelgroep. Als u wilt de waarde van max_parallelism instellen op null, instellen van de parameter op-1. max_parallelism is int.


#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Uitvoeringen in uitvoering van de taak wordt niet beïnvloed. Als sp_update_jobstep is geslaagd, wordt het versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers




### <a name="spdeletejobstep"></a>sp_delete_jobstep

Verwijdert een taakstap van een taak.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak van waaruit u de stap wordt verwijderd. job_name is nvarchar(128) met geen standaardwaarde.

[  **@step_id =** ] step_id  
De id-nummer voor de taakstap moet worden verwijderd. Step_id of step_name moet worden opgegeven. step_id is een int.

[  **@step_name =** ] 'step_name'  
De naam van de stap moet worden verwijderd. Step_id of step_name moet worden opgegeven. step_name is nvarchar(128).

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Uitvoeringen in uitvoering van de taak wordt niet beïnvloed. Als sp_update_jobstep is geslaagd, wordt het versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt.

De andere taakstappen wordt automatisch opnieuw genummerd zodat de tussenruimte door de verwijderde taakstap.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.






### <a name="spstartjob"></a>sp_start_job

Start een taak wordt uitgevoerd.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_start_job [ @job_name = ] 'job_name'   
     [ , [ @job_execution_id = ] job_execution_id OUTPUT ]   
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak van waaruit u de stap wordt verwijderd. job_name is nvarchar(128) met geen standaardwaarde.

[  **@job_execution_id =** ] job_execution_id uitvoer  
De uitvoerparameter die de taakuitvoering van de-id wordt toegewezen. job_version is uniqueidentifier.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spstopjob"></a>sp_stop_job

Hiermee stopt het uitvoeren van een taak.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Argumenten
[  **@job_execution_id =** ] job_execution_id  
De id-nummer van de taak wordt uitgevoerd om te stoppen. job_execution_id is uniqueidentifier met standaard van NULL.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.


### <a name="spaddtargetgroup"></a>sp_add_target_group

Voegt een doelgroep.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Argumenten
[  **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep te maken. target_group_name is nvarchar(128) met geen standaardwaarde.

[  **@target_group_id =** ] target_group_id uitvoer het doel groep-id-nummer toegewezen aan het project als gemaakt. target_group_id is een uitvoervariabele van het type uniqueidentifier, met een standaard van NULL.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Doelgroepen zijn een gemakkelijke manier om een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spdeletetargetgroup"></a>sp_delete_target_group

Hiermee verwijdert u een doelgroep.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Argumenten
[  **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep te verwijderen. target_group_name is nvarchar(128) met geen standaardwaarde.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spaddtargetgroupmember"></a>sp_add_target_group_member

Hiermee voegt u een database of een groep met databases toe aan een doelgroep.

#### <a name="syntax"></a>Syntaxis

```sql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] ‘membership_type’ ]   
        [ , [ @target_type = ] ‘target_type’ ]   
        [ , [ @refresh_credential_name = ] ‘refresh_credential_name’ ]   
        [ , [ @server_name = ] ‘server_name’ ]   
        [ , [ @database_name = ] ‘database_name’ ]   
        [ , [ @elastic_pool_name = ] ‘elastic_pool_name’ ]   
        [ , [ @shard_map_name = ] ‘shard_map_name’ ]   
        [ , [ @target_id = ] ‘target_id’ OUTPUT ]
```

#### <a name="arguments"></a>Argumenten
[  **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep waarop het lid wordt toegevoegd. target_group_name is nvarchar(128) met geen standaardwaarde.

[  **@membership_type =** ] 'membership_type'  
Hiermee geeft u aan als lid van de doel-worden opgenomen of uitgesloten. target_group_name is nvarchar(128) met standaard van 'Include'. Geldige waarden voor target_group_name zijn 'Include' of 'Uitsluiten'.

[  **@target_type =** ] 'target_type'  
Het type van de doeldatabase of -verzameling van databases met inbegrip van alle databases in een server, alle databases in een elastische pool, alle databases in een shard-kaart of een individuele database. target_type is nvarchar(128) met geen standaardwaarde. Geldige waarden voor target_type zijn 'SqlServer', 'SqlElasticPool', 'SqlDatabase' of 'SqlShardMap'. 

[  **@refresh_credential_name =** ] 'refresh_credential_name'  
De naam van de logische server. refresh_credential_name is nvarchar(128) met geen standaardwaarde.

[  **@server_name =** ] 'servernaam'  
De naam van de logische server die moet worden toegevoegd aan de opgegeven doelgroep. servernaam moet worden opgegeven wanneer target_type 'SqlServer'. servernaam is nvarchar(128) met geen standaardwaarde.

[  **@database_name =** ] 'database_name'  
De naam van de database die moet worden toegevoegd aan de opgegeven doelgroep. databasenaam moet worden opgegeven wanneer target_type "SqlDatabase". databasenaam is nvarchar(128) met geen standaardwaarde.

[  **@elastic_pool_name =** ] 'elastic_pool_name'  
De naam van de elastische groep die moet worden toegevoegd aan de opgegeven doelgroep. elastic_pool_name moet worden opgegeven wanneer target_type 'SqlElasticPool'. elastic_pool_name is nvarchar(128) met geen standaardwaarde.

[  **@shard_map_name =** ] 'shard_map_name'  
De naam van de adresgroep van de shard-toewijzing die moet worden toegevoegd aan de opgegeven doelgroep. elastic_pool_name moet worden opgegeven wanneer target_type 'SqlSqlShardMap'. shard_map_name is nvarchar(128) met geen standaardwaarde.

[  **@target_id =** ] target_group_id uitvoer  
Het doel-id-nummer toegewezen aan het doel-groepslid als gemaakt toegevoegd aan de doelgroep. target_id is een uitvoervariabele van het type uniqueidentifier, met een standaard van NULL.
Code retourwaarden 0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Een taak wordt uitgevoerd op alle databases in een server of elastische pool op moment van uitvoering, wanneer een logische server of de elastische groep is opgenomen in de doelgroep.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
Het volgende voorbeeld wordt alle databases in de Londen en NewYork-servers toegevoegd aan de groep Servers onderhouden klantgegevens. U moet verbinding met de opgegeven bij het maken van de agent taak in dit geval ElasticJobs taken-database.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group @target_group_name =  N'Servers Maintaining Customer Information'
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net' ;
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'NewYork.database.windows.net' ;
GO

--View the recently added members to the target group
SELECT * FROM [jobs].target_group_members WHERE target_group_name= N'Servers Maintaining Customer Information';
GO
```

### <a name="spdeletetargetgroupmember"></a>sp_delete_target_group_member

Hiermee verwijdert u een lid van een doel van een doelgroep.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_delete_target_group_member [ @target_group_name = ] 'target_group_name'
        [ , [ @target_id = ] ‘target_id’]
```



Argumenten [ @target_group_name =] 'target_group_name'  
De naam van de doelgroep van waaruit het lid van de groep doel te verwijderen. target_group_name is nvarchar(128) met geen standaardwaarde.

[ @target_id =] target_id  
 De doel-id-nummer toegewezen aan het doel-groepslid moet worden verwijderd. target_id is een uniqueidentifier, met een standaard van NULL.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Doelgroepen zijn een gemakkelijke manier om een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
Het volgende voorbeeld wordt de Londen-server verwijderd uit de groep Servers onderhouden klantgegevens. U moet verbinding met de opgegeven bij het maken van de agent taak in dit geval ElasticJobs taken-database.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Retrieve the target_id for a target_group_members
declare @tid uniqueidentifier
SELECT @tid = target_id FROM [jobs].target_group_members WHERE target_group_name = 'Servers Maintaining Customer Information' and server_name = 'London.database.windows.net'

-- Remove a target group member of type server
EXEC jobs.sp_delete_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_id = @tid
GO
```

### <a name="sppurgejobhistory"></a>sp_purge_jobhistory

Hiermee verwijdert u de geschiedenisrecords voor een taak.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_purge_jobhistory [ @job_name = ] 'job_name'   
      [ , [ @job_id = ] job_id ]
      [ , [ @oldest_date = ] oldest_date []
```

#### <a name="arguments"></a>Argumenten
[  **@job_name =** ] 'job_name'  
De naam van de taak waarvoor geschiedenisrecords te verwijderen. job_name is nvarchar(128) met een standaard van NULL. Job_id of job_name moet worden opgegeven, maar kunnen niet beide worden opgegeven.

[  **@job_id =** ] job_id  
 De taak-id-nummer van de taak voor de records moeten worden verwijderd. job_id is uniqueidentifier met een standaard van NULL. Job_id of job_name moet worden opgegeven, maar kunnen niet beide worden opgegeven.

[  **@oldest_date =** ] oldest_date  
 De oudste record in de geschiedenis moet worden bewaard. oldest_date is DATETIME2, met een standaard van NULL. Wanneer oldest_date wordt opgegeven, worden de records die ouder dan de opgegeven waarde zijn sp_purge_jobhistory alleen verwijderd.

#### <a name="return-code-values"></a>Codewaarden retourneren
0 (geslaagd) of 1 (fout) opmerkingen doelgroepen bieden een eenvoudige manier om het doel van een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Het beperken van een gebruiker alleen mogelijk voor het bewaken van taken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de agent taak verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
Het volgende voorbeeld wordt alle databases in de Londen en NewYork-servers toegevoegd aan de groep Servers onderhouden klantgegevens. U moet verbinding met de opgegeven bij het maken van de agent taak in dit geval ElasticJobs taken-database.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>Taak weergaven

De volgende weergaven zijn beschikbaar in de [taken database](elastic-jobs-overview.md#job-database).


|Weergave  |Beschrijving  |
|---------|---------|
|[jobs_executions](#jobsexecutions-view)     |  Toont Taakgeschiedenis worden uitgevoerd.      |
|[Taken](#jobs-view)     |   Bevat alle taken.      |
|[job_versions](#jobversions-view)     |   Bevat alle versies van de taak.      |
|[taakstappen](#jobsteps-view)     |     Bevat alle stappen in de huidige versie van elke taak.    |
|[jobstep_versions](#jobstepversions-view)     |     Bevat alle stappen in alle versies van elke taak.    |
|[target_groups](#targetgroups-view)     |      Bevat alle doelgroepen.   |
|[target_group_members](#targetgroups-view)     |   Bevat alle leden van alle doelgroepen.      |


### <a name="jobsexecutions-view"></a>jobs_executions weergeven

[taken]. [jobs_executions]

Toont Taakgeschiedenis worden uitgevoerd.


|Kolomnaam|   Gegevenstype   |Beschrijving|
|---------|---------|---------|
|**job_execution_id**   |uniqueidentifier|  Unieke ID van een exemplaar van een taak kan worden uitgevoerd.
|**job_name**   |nvarchar(128)  |De naam van de taak.
|**job_id** |uniqueidentifier|  Unieke ID van de taak.
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt wanneer die de taak is gewijzigd).
|**step_id**    |int|   (Voor deze taak) de unieke id voor de stap. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**is_active**| bits |Hiermee wordt aangegeven of informatie actief of inactief is. 1 geeft aan dat actieve taken en 0 geeft aan dat niet-actief.
|**Levenscyclus**| nvarchar(50)|Waarde die de status van de taak aangeeft: 'Gemaakt', wordt uitgevoerd, 'Mislukt', 'Geslaagd', 'Overgeslagen', 'SucceededWithSkipped'|
|**create_time**|   datetime2(7)|   De datum en tijd waarop die de taak is gemaakt.
|**start_time** |datetime2(7)|  Datum en tijd heeft de uitvoering van de taak gestart. NULL als de taak is nog niet uitgevoerd.
|**end_time**|  datetime2(7)    |Datum en tijd die de taak voltooid kan worden uitgevoerd. NULL als de taak is nog niet uitgevoerd of geen heeft nog voltooid kan worden uitgevoerd.
|**current_attempts**   |int    |Het aantal keren dat de stap opnieuw is geprobeerd. Bovenliggende taak zijn 0, onderliggende taak uitvoeringen is 1 of hoger op basis van het uitvoeringsbeleid.
|**current_attempt_start_time** |datetime2(7)|  Datum en tijd heeft de uitvoering van de taak gestart. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**last_message**   |nvarchar(max)| Geschiedenis-bericht, taak of -stap. 
|**target_type**|   nvarchar(128)   |Type van de doeldatabase of -verzameling van databases met inbegrip van alle databases in een server, alle databases in een elastische groep of een database. Geldige waarden voor target_type zijn 'SqlServer', 'SqlElasticPool' of 'SqlDatabase'. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**target_id**  |uniqueidentifier|  Unieke ID van het groepslid doel.  NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**target_group_name**  |nvarchar(128)  |Naam van de doelgroep. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**target_server_name**|    nvarchar(256)|  Naam van de logische server die is opgenomen in de doelgroep. Alleen opgegeven als target_type 'SqlServer' is. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.
|**target_database_name**   |nvarchar(128)| De naam van de database die is opgenomen in de doelgroep. Opgegeven alleen als target_type "SqlDatabase" is. NULL geeft aan dat dit de bovenliggende taak wordt uitgevoerd.


### <a name="jobs-view"></a>taken weergeven

[taken]. [taken]

Bevat alle taken.

|Kolomnaam|   Gegevenstype|  Beschrijving|
|------|------|-------|
|**job_name**|  nvarchar(128)   |De naam van de taak.|
|**job_id**|    uniqueidentifier    |Unieke ID van de taak.|
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt wanneer die de taak is gewijzigd).|
|**Beschrijving**    |nvarchar(512)| Beschrijving voor de taak. ingeschakelde bit geeft aan of de taak is ingeschakeld of uitgeschakeld. 1 geeft aan dat de ingeschakelde taken en 0 geeft aan dat taken uitgeschakeld.|
|**schedule_interval_type** |nvarchar(50)   |Waarde die aangeeft wanneer de taak wordt uitgevoerd: 'Eenmaal', 'Minuten', 'Uren', ' dagen', 'Weken', 'Maanden'
|**schedule_interval_count**|   int|    Het aantal perioden schedule_interval_type optreden tussen de uitvoering van de taak.|
|**schedule_start_time**    |datetime2(7)|  Datum en tijd waarop die de taak laatste uitvoering is gestart is.|
|**schedule_end_time**| datetime2(7)|   De datum en tijd waarop die de taak is de laatste voltooide uitvoering.|


### <a name="jobversions-view"></a>job_versions weergeven

[taken]. [job_verions]

Bevat alle versies van de taak.

|Kolomnaam|   Gegevenstype|  Beschrijving|
|------|------|-------|
|**job_name**|  nvarchar(128)   |De naam van de taak.|
|**job_id**|    uniqueidentifier    |Unieke ID van de taak.|
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt wanneer die de taak is gewijzigd).|


### <a name="jobsteps-view"></a>taakstappen weergeven

[taken]. [taakstappen]

Bevat alle stappen in de huidige versie van elke taak.

|Kolomnaam    |Gegevenstype| Beschrijving|
|------|------|-------|
|**job_name**   |nvarchar(128)| De naam van de taak.|
|**job_id** |uniqueidentifier   |Unieke ID van de taak.|
|**job_version**|   int|    De versie van de taak (automatisch bijgewerkt wanneer die de taak is gewijzigd).|
|**step_id**    |int    |(Voor deze taak) de unieke id voor de stap.|
|**step_name**  |nvarchar(128)  |(Voor deze taak) de unieke naam voor de stap.|
|**command_type**   |nvarchar(50)   |Het type van de opdracht wordt uitgevoerd in de taakstap. Voor v1, waarde moet gelijk zijn aan en wordt standaard ingesteld op 'TSql'.|
|**command_source** |nvarchar(50)|  Locatie van de opdracht. Voor v1, 'Inline' is de standaardinstelling en alleen geaccepteerd waarde.|
|**opdracht**|   nvarchar(max)|  De opdrachten door elastische taken via command_type moet worden uitgevoerd.|
|**credential_name**|   nvarchar(128)   |Naam van de-scoped databasereferentie gebruikt tot het uitvoeren van de taak.|
|**target_group_name**| nvarchar(128)   |Naam van de doelgroep.|
|**target_group_id**|   uniqueidentifier|   Unieke ID van de doelgroep.|
|**initial_retry_interval_seconds**|    int |De wachttijd voordat de eerste nieuwe poging. Standaardwaarde is 1.|
|**maximum_retry_interval_seconds** |int|   De maximale vertraging tussen nieuwe pogingen. Als de vertraging tussen pogingen groter is dan deze waarde toenemen zou, is het in plaats daarvan naar deze waarde beperkt. Standaardwaarde is 120.|
|**retry_interval_backoff_multiplier**  |echte|  De vermenigvuldigingsfactor toepassen op de tijd tussen elke poging als meerdere taak uitvoering van stap probeert mislukken. Standaardwaarde is 2.0.|
|**retry_attempts** |int|   Het aantal nieuwe poging probeert te gebruiken als deze stap mislukt. De standaardwaarde van 10, waarmee wordt aangegeven geen nieuwe pogingen.|
|**step_timeout_seconds**   |int|   De hoeveelheid tijd in minuten tussen nieuwe pogingen. De standaardwaarde is 0, waarmee wordt aangegeven van een interval van 0 minuten.|
|**output_type**    |nvarchar(11)|  Locatie van de opdracht. In de huidige preview 'Inline' is de standaardinstelling en alleen geaccepteerd waarde.|
|**output_credential_name**|    nvarchar(128)   |Naam van de referenties die worden gebruikt voor verbinding met de doelserver voor het opslaan van de resultaten ingesteld.|
|**output_subscription_id**|    uniqueidentifier|   Unieke ID van het abonnement van de server\database bestemming voor de resultaten die zijn ingesteld op basis van de uitvoering van de query.|
|**output_resource_group_name** |nvarchar(128)| Naam van resourcegroep waar de doelserver zich bevindt.|
|**output_server_name**|    nvarchar(256)   |De naam van de doelserver voor de resultatenset.|
|**output_database_name**   |nvarchar(128)| De naam van de doeldatabase voor de resultatenset.|
|**output_schema_name** |nvarchar(max)| Naam van de doelschema. De standaardwaarde dbo, indien niet opgegeven.|
|**output_table_name**| nvarchar(max)|  De naam van de tabel voor het opslaan van de resultaten die zijn ingesteld op basis van de queryresultaten. Tabel wordt gemaakt op basis van het schema van de resultaten die zijn ingesteld als deze niet al bestaat automatisch. Schema moet overeenkomen met het schema van de resultatenset.|
|**max_parallelism**|   int|    Het maximale aantal databases per elastische groep die de taakstap op tegelijk worden uitgevoerd. De standaardwaarde is NULL, wat betekent dat er geen limiet. |


### <a name="jobstepversions-view"></a>jobstep_versions weergeven

[taken]. [jobstep_versions]

Bevat alle stappen in alle versies van elke taak. Het schema is identiek aan [taakstappen](#jobsteps-view).

### <a name="targetgroups-view"></a>target_groups weergeven

[taken]. [target_groups]

Geeft een lijst van alle doelgroepen.

|Kolomnaam|Gegevenstype| Beschrijving|
|-----|-----|-----|
|**target_group_name**| nvarchar(128)   |De naam van de doelgroep, een verzameling van databases. 
|**target_group_id**    |uniqueidentifier   |Unieke ID van de doelgroep.

### <a name="targetgroupsmembers-view"></a>target_groups_members weergeven

[taken]. [target_groups_members]

Bevat alle leden van alle doelgroepen.

|Kolomnaam|Gegevenstype| Beschrijving|
|-----|-----|-----|
|**target_group_name**  |nvarchar (128|De naam van de doelgroep, een verzameling van databases. |
|**target_group_id**    |uniqueidentifier   |Unieke ID van de doelgroep.|
|**membership_type**    |int|   Hiermee geeft u aan als lid van de doelserver is opgenomen of in de doelgroep uitgesloten. Geldige waarden voor target_group_name zijn 'Include' of 'Uitsluiten'.|
|**target_type**    |nvarchar(128)| Type van de doeldatabase of -verzameling van databases met inbegrip van alle databases in een server, alle databases in een elastische groep of een database. Geldige waarden voor target_type zijn 'SqlServer', 'SqlElasticPool', 'SqlDatabase' of 'SqlShardMap'.|
|**target_id**  |uniqueidentifier|  Unieke ID van het groepslid doel.|
|**refresh_credential_name**    |nvarchar(128)  |Naam van de database-scoped referentie gebruikt voor verbinding met het groepslid doel.|
|**subscription_id**    |uniqueidentifier|  Unieke ID van het abonnement.|
|**resource_group_name**    |nvarchar(128)| Naam van de resourcegroep waarin het doel-groepslid zich bevindt.|
|**servernaam**    |nvarchar(128)  |Naam van de logische server die is opgenomen in de doelgroep. Alleen opgegeven als target_type 'SqlServer' is. |
|**Databasenaam**  |nvarchar(128)  |De naam van de database die is opgenomen in de doelgroep. Opgegeven alleen als target_type "SqlDatabase" is.|
|**elastic_pool_name**  |nvarchar(128)| Naam van de elastische groep is opgenomen in de doelgroep. Opgegeven alleen als target_type 'SqlElasticPool' is.|
|**shard_map_name** |nvarchar(128)| Naam van de shard-toewijzing die zijn opgenomen in de doelgroep. Opgegeven alleen als target_type 'SqlShardMap' is.|


## <a name="resources"></a>Resources

 - ![Onderwerp-koppelingspictogram](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "onderwerp koppelingspictogram") [conventies van Transact-SQL-syntaxis](/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="next-steps"></a>Volgende stappen

- [Maken en beheren van elastische taken met behulp van PowerShell](elastic-jobs-powershell.md)
- [Autorisatie en machtigingen voor SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
  