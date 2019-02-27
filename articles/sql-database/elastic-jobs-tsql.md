---
title: Maken en beheren van Azure SQL taken voor Elastic Database met behulp van Transact-SQL (T-SQL) | Microsoft Docs
description: Scripts uitvoeren voor verschillende databases met agent voor elastische Database-taken met behulp van Transact-SQL (T-SQL).
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
ms.author: jaredmoo
author: jaredmoo
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 56ad75bc0bed77c2107a49ab339368ab2a63c1fe
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56881257"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Transact-SQL (T-SQL) gebruiken om te maken en beheren van taken voor Elastic Database

Dit artikel bevat veel scenario's met voorbeelden aan de slag met elastische taken werken met T-SQL.

De voorbeelden gebruiken de [opgeslagen procedures](#job-stored-procedures) en [weergaven](#job-views) beschikbaar in de [ *taak database*](sql-database-job-automation-overview.md#job-database).

Transact-SQL (T-SQL) wordt gebruikt om te maken, configureren, uitvoeren en beheren van taken. Het maken van de agent voor elastische taken wordt niet ondersteund in T-SQL, dus moet u eerst maken een *agent voor elastische taken* met behulp van de portal of [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).


## <a name="create-a-credential-for-job-execution"></a>Maken van een referentie voor het uitvoeren van taak

De referentie op die wordt gebruikt om verbinding maken met de doeldatabase voor uitvoering van het script. De referentie moet de juiste machtigingen voor de databases die zijn opgegeven door de doelgroep voor het script is uitgevoerd. Wanneer u een server en/of lid van groep doel, wordt het ten zeerste aangeraden een master referentie om te vernieuwen van de referentie voor uitbreiding van de server en/of de groep van toepassingen tijdens het uitvoeren van taak maken. De database-scoped referentie is gemaakt op de database van de agent taak. De dezelfde referenties moet worden gebruikt om *een aanmelding maken* en *maken van een gebruiker van Meld u aan bij de aanmelding databasemachtigingen verlenen* op de doeldatabase.


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

Het volgende voorbeeld ziet hoe u het uitvoeren van een taak voor alle databases in een server.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:


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


## <a name="exclude-an-individual-database"></a>Uitsluiten van een individuele database

Het volgende voorbeeld laat zien hoe het uitvoeren van een taak voor alle databases in een SQL Database-server, met uitzondering van de database met de naam *MappingDB*.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group N'ServerGroup'
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

--Exclude a database target member from the server target group
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

Het volgende voorbeeld ziet hoe u richten op alle databases in een of meer elastische pools.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

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


## <a name="deploy-new-schema-to-many-databases"></a>Nieuw schema om veel databases te implementeren

Het volgende voorbeeld ziet hoe u nieuwe schema implementeert op alle databases.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:


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

In veel gegevens verzameling scenario's, kan het nuttig zijn om op te nemen van sommige van deze scripts variabelen om te na verwerking van de resultaten van de taak.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Bijvoorbeeld: als u wilt alle resultaten van de uitvoering van dezelfde taak van samen groeperen, gebruiken de *$(job_execution_id)* zoals wordt weergegeven in de volgende opdracht uit:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Databaseprestaties bewaken

Het volgende voorbeeld wordt een nieuwe taak voor het verzamelen van prestatiegegevens uit meerdere databases.

Standaard wordt de agent voor taken zoeken voor het maken van de tabel voor het opslaan van de geretourneerde resultaten in. Als gevolg hiervan moet de aanmelding die is gekoppeld aan de referentie die wordt gebruikt voor de referentie van de uitvoer voldoende rechten hebt voor dit uitvoeren. Als u handmatig wilt maken in de tabel vooraf moet deze de volgende eigenschappen hebben:
1. Kolommen met de juiste naam en gegevenstypen voor de resultatenset.
2. Extra kolom voor internal_execution_id met het gegevenstype uniqueidentifier.
3. Een niet-geclusterde index met de naam ' IX_<TableName>_Internal_Execution_ID ' op de kolom internal_execution_id.

Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdrachten uit:

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
@output_credential_name='myjobcred',
@output_server_name='server1.database.windows.net',
@output_database_name='<resultsdb>',
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
@output_server_name='server1.database.windows.net',
@output_database_name='resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>Definities van de taak weergeven

Het volgende voorbeeld laat zien hoe om huidige taakdefinities weer te geven.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

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


## <a name="begin-ad-hoc-execution-of-a-job"></a>Ad-hoc uitvoeren van een taak starten

Het volgende voorbeeld ziet hoe u een taak onmiddellijk worden gestart.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

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


## <a name="schedule-execution-of-a-job"></a>Uitvoering van een taak plannen

Het volgende voorbeeld ziet hoe u een taak voor de uitvoering van toekomstige plannen.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>Uitvoeringsstatus van de taak bewaken

Het volgende voorbeeld laat zien hoe om details van uitvoering van de status voor alle taken weer te geven.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named 'ResultsPoolJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named 'ResultsPoolsJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Een taak annuleren

Het volgende voorbeeld ziet hoe u een taak annuleren.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

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


## <a name="delete-old-job-history"></a>Verwijderen van oude Taakgeschiedenis

Het volgende voorbeeld laat zien hoe Taakgeschiedenis voorafgaand aan een specifieke datum verwijderen.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Een taak en de taakgeschiedenis verwijderen

Het volgende voorbeeld laat zien hoe om een taak te verwijderen en alle gerelateerde Taakgeschiedenis.  
Verbinding maken met de [ *taak database* ](sql-database-job-automation-overview.md#job-database) en voer de volgende opdracht uit:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>Taak opgeslagen procedures

De volgende opgeslagen procedures worden de [taken database](sql-database-job-automation-overview.md#job-database).



|Opgeslagen procedure  |Description  |
|---------|---------|
|[sp_add_job](#spaddjob)     |     Hiermee wordt een nieuwe taak toegevoegd.    |
|[sp_update_job](#spupdatejob)    |      Een bestaande taak voor het bijwerken.   |
|[sp_delete_job](#spdeletejob)     |      Hiermee verwijdert u een bestaande taak.   |
|[sp_add_jobstep](#spaddjobstep)    |    Voegt een stap toe aan een taak.     |
|[sp_update_jobstep](#spupdatejobstep)     |     Een stap in logboektaak-updates.    |
|[sp_delete_jobstep](#spdeletejobstep)     |     Hiermee verwijdert u een stap in logboektaak.    |
|[sp_start_job](#spstartjob)    |  Start een taak wordt uitgevoerd.       |
|[sp_stop_job](#spstopjob)     |     Hiermee stopt u het uitvoeren van een taak.   |
|[sp_add_target_group](#spaddtargetgroup)    |     Een doel-gebruikersgroep wordt toegevoegd.    |
|[sp_delete_target_group](#spdeletetargetgroup)     |    Hiermee verwijdert u een doelgroep.     |
|[sp_add_target_group_member](#spaddtargetgroupmember)     |    Een database of een groep databases toegevoegd aan een doelgroep.     |
|[sp_delete_target_group_member](#spdeletetargetgroupmember)     |     Hiermee verwijdert u een lid van een doel van een doelgroep.    |
|[sp_purge_jobhistory](#sppurgejobhistory)    |    Hiermee verwijdert u de geschiedenisrecords voor een taak.     |





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

[ **@job_name =** ] 'job_name'  
De naam van de taak. De naam moet uniek zijn en mag het percentage (%) teken. job_name is nvarchar(128) met geen standaardwaarde.

[  **@description =** ] 'description'  
De beschrijving van de taak. de beschrijving is nvarchar(512), met een standaard null. Als de beschrijving wordt weggelaten, wordt een lege tekenreeks wordt gebruikt.

[  **@enabled =** ] ingeschakeld  
Of de planning van de taak is ingeschakeld. Ingeschakeld is bits, met een standaardwaarde van 0 (uitgeschakeld). Als u 0 opgeeft, wordt de taak is niet ingeschakeld en wordt niet uitgevoerd volgens schema; maar kan deze handmatig worden uitgevoerd. Als 1, wordt de taak wordt uitgevoerd volgens schema, en kan ook handmatig worden uitgevoerd.

[ **@schedule_interval_type =**] schedule_interval_type  
De waarde geeft aan wanneer de taak wordt uitgevoerd. schedule_interval_type kan is nvarchar(50), met een standaardwaarde van één keer, en een van de volgende waarden:
- 'Eenmaal'
- 'Minutes',
- 'Uur',
- 'Days',
- 'Weken'.
- 'Maanden'

[  **@schedule_interval_count =** ] schedule_interval_count  
Het aantal perioden schedule_interval_count optreden tussen elke uitvoering van de taak. schedule_interval_count is int, met een standaardwaarde van 1. De waarde moet groter zijn dan of gelijk aan 1 zijn.

[  **@schedule_start_time =** ] schedule_start_time  
De datum op welke taak uitvoering kan beginnen. schedule_start_time is DATETIME2, met de standaardwaarde van 01-01-0001 00:00:00.0000000.

[  **@schedule_end_time =** ] schedule_end_time  
De datum op welke taak uitvoeren kunt stoppen. schedule_end_time DATETIME2, is met de standaardwaarde van 31-12-9999 11:59:59.0000000. 

[  **@job_id =** ] job_id uitvoer  
De taak id-nummer toegewezen aan de taak als is gemaakt. job_id is een uitvoervariabele van het type uniqueidentifier.

#### <a name="return-code-values"></a>Retourcode waarden

0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
sp_add_job moet worden uitgevoerd vanaf de taak agent database die is opgegeven bij het maken van de taakagent.
Nadat sp_add_job als u wilt toevoegen van een taak is uitgevoerd, kan het toevoegen van stappen die de activiteiten voor de taak uitvoeren sp_add_jobstep worden gebruikt. De eerste versienummer van de taak is 0, 1 wordt verhoogd wanneer de eerste stap is toegevoegd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spupdatejob"></a>sp_update_job

Een bestaande taak voor het bijwerken.

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
[ **@job_name =** ] 'job_name'  
De naam van de taak moet worden bijgewerkt. job_name is nvarchar(128).

[  **@new_name =** ] 'nieuwe_naam'  
De nieuwe naam van de taak. nieuwe_naam is nvarchar(128).

[  **@description =** ] 'description'  
De beschrijving van de taak. de beschrijving is nvarchar(512).

[  **@enabled =** ] ingeschakeld  
Geeft aan of het taakschema ingeschakeld (1) of niet is ingeschakeld (0). Ingeschakeld is bits.

[ **@schedule_interval_type=** ] schedule_interval_type  
De waarde geeft aan wanneer de taak wordt uitgevoerd. schedule_interval_type kan nvarchar(50) is en een van de volgende waarden:

- 'Eenmaal'
- 'Minutes',
- 'Uur',
- 'Days',
- 'Weken'.
- 'Maanden'

[  **@schedule_interval_count=** ] schedule_interval_count  
Het aantal perioden schedule_interval_count optreden tussen elke uitvoering van de taak. schedule_interval_count is int, met een standaardwaarde van 1. De waarde moet groter zijn dan of gelijk aan 1 zijn.

[  **@schedule_start_time=** ] schedule_start_time  
De datum op welke taak uitvoering kan beginnen. schedule_start_time is DATETIME2, met de standaardwaarde van 01-01-0001 00:00:00.0000000.

[  **@schedule_end_time=** ] schedule_end_time  
De datum op welke taak uitvoeren kunt stoppen. schedule_end_time DATETIME2, is met de standaardwaarde van 31-12-9999 11:59:59.0000000. 

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Nadat sp_add_job als u wilt toevoegen van een taak is uitgevoerd, kan het toevoegen van stappen die de activiteiten voor de taak uitvoeren sp_add_jobstep worden gebruikt. De eerste versienummer van de taak is 0, 1 wordt verhoogd wanneer de eerste stap is toegevoegd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
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
[ **@job_name =** ] 'job_name'  
De naam van de taak moet worden verwijderd. job_name is nvarchar(128).

[  **@force =** ] forceren  
Geeft aan of verwijderen als de taak uitvoeringen wordt uitgevoerd heeft en alle lopende uitvoeringen (1) of fouten worden geannuleerd als taakuitvoeringen uitgevoerd (0 worden). Force-is bits.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Taakgeschiedenis wordt automatisch verwijderd wanneer een taak wordt verwijderd.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.



### <a name="spaddjobstep"></a>sp_add_jobstep

Voegt een stap toe aan een taak.

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

[ **@job_name =** ] 'job_name'  
De naam van de taak waarnaar u wilt toevoegen van de stap. job_name is nvarchar(128).

[ **@step_id =** ] step_id  
De volgorde-id-nummer voor de taakstap. Stap identificatiecodes begint bij 1 en verhogen zonder onderbrekingen. Als een bestaande stap al deze id, de klikt u vervolgens dat stap en alle volgende stappen uit hun-id hebben verhoogd zodat deze nieuwe stap in de reeks kan worden ingevoegd. Indien niet opgegeven, wordt de step_id automatisch toegewezen naar de laatste in de volgorde van stappen. step_id is int.

[  **@step_name =** ] step_name  
De naam van de stap. Moet worden opgegeven, met uitzondering van de eerste stap van een taak die (voor uw gemak bedoeld;) een standaard-naam van de 'JobStep heeft'. step_name is nvarchar(128).

[ **@command_type =** ] 'command_type'  
Het type van de opdracht die wordt uitgevoerd door deze taakstap. command_type nvarchar(50) met TSql, wat betekent dat de waarde van de standaardwaarde is de @command_type parameter is een T-SQL-script.

Als u opgeeft, moet de waarde TSql.

[  **@command_source =** ] 'command_source'  
Het type van de locatie waar de opdracht is opgeslagen. command_source is nvarchar(50), met een standaardwaarde van Inline, wat betekent dat de waarde van de @command_source parameter is de letterlijke tekenreeks van de opdracht.

Als u opgeeft, is de waarde moet Inline.

[  **@command =** ] 'command'  
De opdracht moet geldige T-SQL-script en vervolgens door deze taakstap wordt uitgevoerd. opdracht is nvarchar(max), met een standaard null.

[  **@credential_name =** ] 'credential_name'  
De naam van de database-scoped referentie opgeslagen in deze taak control database die wordt gebruikt voor verbinding met elk van de doeldatabases in de doelgroep wanneer deze stap wordt uitgevoerd. credential_name is nvarchar(128).

[ **@target_group_name =** ] 'target-group_name'  
De naam van de doelgroep waarop zich de doeldatabases die de taakstap wordt uitgevoerd op. target_group_name is nvarchar(128).

[ **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
De vertraging voordat de eerste poging, als de taakstap mislukt bij de poging tot de eerste uitvoering. initial_retry_interval_seconds is int, met de standaardwaarde van 1.

[ **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
De maximale wachttijd tussen nieuwe pogingen. Als de vertraging tussen nieuwe pogingen kan groter zijn dan deze waarde, wordt deze het beste in plaats daarvan beperkt tot op deze waarde. maximum_retry_interval_seconds is int, met de standaardwaarde van 120.

[ **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
De vermenigvuldiger om toe te passen aan de elke poging als meerdere taak uitvoering van stap aanmeldingspogingen mislukt. Bijvoorbeeld, als de eerste poging had een vertraging van vijf seconden en de vermenigvuldiger uitstel 2.0 is, vervolgens de tweede nieuwe poging heeft een vertraging van 10 seconden en de derde opnieuw proberen heeft een vertraging van 20 seconden. retry_interval_backoff_multiplier zijn echte, met de standaardwaarde van 2.0.

[  **@retry_attempts =** ] retry_attempts  
Het aantal nieuwe pogingen worden uitgevoerd als de eerste poging is mislukt. Bijvoorbeeld, als de waarde retry_attempts 10, zijn er 1 eerste nieuwe poging en 10 pogingen, geeft een totaal van 11 pogingen. Als de laatste poging mislukt, wordt het uitvoeren van taak beëindigd met een levensduur van is mislukt. retry_attempts is int, met de standaardwaarde van 10.

[ **@step_timeout_seconds =** ] step_timeout_seconds  
De maximale hoeveelheid tijd die zijn toegestaan voor de stap uit te voeren. Als deze tijd wordt overschreden, wordt het uitvoeren van taak beëindigd met een levensduur van de time-out. step_timeout_seconds is int, met de standaardwaarde van 43.200 seconden (12 uur).

[ **@output_type =** ] 'output_type'  
Als dat niet null is, het type van de bestemming die eerste voor de resultatenset van de opdracht wordt geschreven naar. output_type is nvarchar(50), met een standaard null.

Als u opgeeft, moet de waarde SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Als deze niet leeg is, de naam van de database-scoped referentie die wordt gebruikt om te verbinden met de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk. output_credential_name is nvarchar(128), met een standaardwaarde van NULL.

[ **@output_subscription_id =** ] 'output_subscription_id'  
Beschrijving moet.

[  **@output_resource_group_name =** ] 'output_resource_group_name'  
Beschrijving moet.

[ **@output_server_name =** ] 'output_server_name'  
Als dat niet null, de volledig gekwalificeerde DNS-naam van de server die de doeldatabase uitvoer bevat. Moet worden opgegeven als output_type SqlDatabase gelijk. output_server_name is nvarchar(256), met een standaard null.

[  **@output_database_name =** ] 'output_database_name'  
Als dat niet null, de naam van de database die de doeltabel van de uitvoer bevat. Moet worden opgegeven als output_type SqlDatabase gelijk. output_database_name is nvarchar(128), met een standaard null.

[  **@output_schema_name =** ] 'output_schema_name'  
Als dat niet null, de naam van het SQL-schema met de uitvoertabel van de bestemming. Als output_type gelijk is aan SqlDatabase, is de standaardwaarde dbo. output_schema_name is nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Als dat niet null is, wordt de naam van de tabel die eerste voor de resultatenset van de opdracht om te worden geschreven. Als de tabel nog niet bestaat, wordt deze gemaakt op basis van het schema van de resultatenset geretourneerd. Moet worden opgegeven als output_type SqlDatabase gelijk. output_table_name is nvarchar(128), met een standaardwaarde van NULL.

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

[  **@max_parallelism =** ] max_parallelism uitvoer  
Het maximumniveau van parallelle uitvoering per elastische pool. Indien ingesteld, worden de taakstap beperkt is tot alleen worden uitgevoerd op een maximum van die veel databases per elastische groep zijn. Dit geldt voor elke elastische pool die ofwel rechtstreeks is opgenomen in de doelgroep of bevindt zich in een server die is opgenomen in de doelgroep. max_parallelism is int.


#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Als sp_add_jobstep is geslaagd, wordt het huidige versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt. Als de taak wordt momenteel uitgevoerd, bevat die tot uitvoering van de nieuwe stap niet.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:  

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.



### <a name="spupdatejobstep"></a>sp_update_jobstep

Een stap in logboektaak-updates.

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
[ **@job_name =** ] 'job_name'  
De naam van de taak die de stap behoort. job_name is nvarchar(128).

[ **@step_id =** ] step_id  
De id-nummer voor de taakstap worden gewijzigd. Step_id of step_name moet worden opgegeven. step_id is int.

[  **@step_name =** ] 'step_name'  
De naam van de stap om te worden gewijzigd. Step_id of step_name moet worden opgegeven. step_name is nvarchar(128).

[ **@new_id =** ] new_id  
De nieuwe volgorde id-nummer voor de taakstap. Stap identificatiecodes begint bij 1 en verhogen zonder onderbrekingen. Als een stap in de volgorde is gewijzigd, klikt u vervolgens andere stappen zal opnieuw worden automatisch genummerd.

[  **@new_name =** ] 'nieuwe_naam'  
De nieuwe naam van de stap. nieuwe_naam is nvarchar(128).

[ **@command_type =** ] 'command_type'  
Het type van de opdracht die wordt uitgevoerd door deze taakstap. command_type nvarchar(50) met TSql, wat betekent dat de waarde van de standaardwaarde is de @command_type parameter is een T-SQL-script.

Als u opgeeft, moet de waarde TSql.

[  **@command_source =** ] 'command_source'  
Het type van de locatie waar de opdracht is opgeslagen. command_source is nvarchar(50), met een standaardwaarde van Inline, wat betekent dat de waarde van de @command_source parameter is de letterlijke tekenreeks van de opdracht.

Als u opgeeft, is de waarde moet Inline.

[  **@command =** ] 'command'  
De opdrachten zijn moet geldige T-SQL-script en vervolgens door deze taakstap wordt uitgevoerd. opdracht is nvarchar(max), met een standaard null.

[  **@credential_name =** ] 'credential_name'  
De naam van de database-scoped referentie opgeslagen in deze taak control database die wordt gebruikt voor verbinding met elk van de doeldatabases in de doelgroep wanneer deze stap wordt uitgevoerd. credential_name is nvarchar(128).

[ **@target_group_name =** ] 'target-group_name'  
De naam van de doelgroep waarop zich de doeldatabases die de taakstap wordt uitgevoerd op. target_group_name is nvarchar(128).

[ **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
De vertraging voordat de eerste poging, als de taakstap mislukt bij de poging tot de eerste uitvoering. initial_retry_interval_seconds is int, met de standaardwaarde van 1.

[ **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
De maximale wachttijd tussen nieuwe pogingen. Als de vertraging tussen nieuwe pogingen kan groter zijn dan deze waarde, wordt deze het beste in plaats daarvan beperkt tot op deze waarde. maximum_retry_interval_seconds is int, met de standaardwaarde van 120.

[ **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
De vermenigvuldiger om toe te passen aan de elke poging als meerdere taak uitvoering van stap aanmeldingspogingen mislukt. Bijvoorbeeld, als de eerste poging had een vertraging van vijf seconden en de vermenigvuldiger uitstel 2.0 is, vervolgens de tweede nieuwe poging heeft een vertraging van 10 seconden en de derde opnieuw proberen heeft een vertraging van 20 seconden. retry_interval_backoff_multiplier zijn echte, met de standaardwaarde van 2.0.

[  **@retry_attempts =** ] retry_attempts  
Het aantal nieuwe pogingen worden uitgevoerd als de eerste poging is mislukt. Bijvoorbeeld, als de waarde retry_attempts 10, zijn er 1 eerste nieuwe poging en 10 pogingen, geeft een totaal van 11 pogingen. Als de laatste poging mislukt, wordt het uitvoeren van taak beëindigd met een levensduur van is mislukt. retry_attempts is int, met de standaardwaarde van 10.

[ **@step_timeout_seconds =** ] step_timeout_seconds  
De maximale hoeveelheid tijd die zijn toegestaan voor de stap uit te voeren. Als deze tijd wordt overschreden, wordt het uitvoeren van taak beëindigd met een levensduur van de time-out. step_timeout_seconds is int, met de standaardwaarde van 43.200 seconden (12 uur).

[ **@output_type =** ] 'output_type'  
Als dat niet null is, het type van de bestemming die eerste voor de resultatenset van de opdracht wordt geschreven naar. Als u wilt opnieuw de waarde van output_type instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_type is nvarchar(50), met een standaard null.

Als u opgeeft, moet de waarde SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Als deze niet leeg is, de naam van de database-scoped referentie die wordt gebruikt om te verbinden met de doeldatabase uitvoer. Moet worden opgegeven als output_type SqlDatabase gelijk. Als u wilt opnieuw de waarde van output_credential_name instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_credential_name is nvarchar(128), met een standaardwaarde van NULL.

[ **@output_server_name =** ] 'output_server_name'  
Als dat niet null, de volledig gekwalificeerde DNS-naam van de server die de doeldatabase uitvoer bevat. Moet worden opgegeven als output_type SqlDatabase gelijk. Als u wilt opnieuw de waarde van output_server_name instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_server_name is nvarchar(256), met een standaard null.

[  **@output_database_name =** ] 'output_database_name'  
Als dat niet null, de naam van de database die de doeltabel van de uitvoer bevat. Moet worden opgegeven als output_type SqlDatabase gelijk. Als u wilt opnieuw de waarde van output_database_name instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_database_name is nvarchar(128), met een standaard null.

[  **@output_schema_name =** ] 'output_schema_name'  
Als dat niet null, de naam van het SQL-schema met de uitvoertabel van de bestemming. Als output_type gelijk is aan SqlDatabase, is de standaardwaarde dbo. Als u wilt opnieuw de waarde van output_schema_name instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_schema_name is nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Als dat niet null is, wordt de naam van de tabel die eerste voor de resultatenset van de opdracht om te worden geschreven. Als de tabel nog niet bestaat, wordt deze gemaakt op basis van het schema van de resultatenset geretourneerd. Moet worden opgegeven als output_type SqlDatabase gelijk. Als u wilt opnieuw de waarde van output_server_name instellen op NULL, stelt u de waarde van van deze parameter in op ' (een lege tekenreeks). output_table_name is nvarchar(128), met een standaardwaarde van NULL.

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

[  **@max_parallelism =** ] max_parallelism uitvoer  
Het maximumniveau van parallelle uitvoering per elastische pool. Indien ingesteld, worden de taakstap beperkt is tot alleen worden uitgevoerd op een maximum van die veel databases per elastische groep zijn. Dit geldt voor elke elastische pool die ofwel rechtstreeks is opgenomen in de doelgroep of bevindt zich in een server die is opgenomen in de doelgroep. Als u de waarde van max_parallelism herstellen op null, waarde van deze parameter op-1. max_parallelism is int.


#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Alle uitvoeringen in uitvoering van de taak wordt niet beïnvloed. Als sp_update_jobstep is geslaagd, wordt het versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:

- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers




### <a name="spdeletejobstep"></a>sp_delete_jobstep

Hiermee verwijdert u een taakstap van een taak.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Argumenten
[ **@job_name =** ] 'job_name'  
De naam van de taak van waaruit de stap wordt verwijderd. job_name is nvarchar(128) met geen standaardwaarde.

[ **@step_id =** ] step_id  
De id-nummer voor de taakstap moet worden verwijderd. Step_id of step_name moet worden opgegeven. step_id is int.

[  **@step_name =** ] 'step_name'  
De naam van de stap moet worden verwijderd. Step_id of step_name moet worden opgegeven. step_name is nvarchar(128).

[  **@job_version =** ] job_version uitvoer  
Output-parameter die het nieuwe versienummer van de taak wordt toegewezen. job_version is int.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Alle uitvoeringen in uitvoering van de taak wordt niet beïnvloed. Als sp_update_jobstep is geslaagd, wordt het versienummer van de taak wordt verhoogd. De volgende keer dat de taak wordt uitgevoerd, wordt de nieuwe versie gebruikt.

De overige taakstappen wordt automatisch opnieuw genummerd om op te vullen van de tussenruimte door de verwijderde taakstap.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
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
[ **@job_name =** ] 'job_name'  
De naam van de taak van waaruit de stap wordt verwijderd. job_name is nvarchar(128) met geen standaardwaarde.

[  **@job_execution_id =** ] job_execution_id uitvoer  
Output-parameter die de taakuitvoering van de-id worden toegewezen. job_version is uniqueidentifier.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spstopjob"></a>sp_stop_job

Hiermee stopt u het uitvoeren van een taak.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Argumenten
[ **@job_execution_id =** ] job_execution_id  
De id-nummer van de taakuitvoering te stoppen. job_execution_id is uniqueidentifier met standaard null.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.
 
#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.


### <a name="spaddtargetgroup"></a>sp_add_target_group

Een doel-gebruikersgroep wordt toegevoegd.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Argumenten
[ **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep te maken. target_group_name is nvarchar(128) met geen standaardwaarde.

[  **@target_group_id =** ] target_group_id uitvoer het doel groep id-nummer toegewezen aan de taak als is gemaakt. target_group_id is een uitvoervariabele van het type uniqueidentifier, met een standaard null.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Doelgroepen bieden een eenvoudige manier om een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spdeletetargetgroup"></a>sp_delete_target_group

Hiermee verwijdert u een doelgroep.

#### <a name="syntax"></a>Syntaxis


```sql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Argumenten
[ **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep te verwijderen. target_group_name is nvarchar(128) met geen standaardwaarde.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Geen.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

### <a name="spaddtargetgroupmember"></a>sp_add_target_group_member

Een database of een groep databases toegevoegd aan een doelgroep.

#### <a name="syntax"></a>Syntaxis

```sql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] 'membership_type' ]   
        [ , [ @target_type = ] 'target_type' ]   
        [ , [ @refresh_credential_name = ] 'refresh_credential_name' ]   
        [ , [ @server_name = ] 'server_name' ]   
        [ , [ @database_name = ] 'database_name' ]   
        [ , [ @elastic_pool_name = ] 'elastic_pool_name' ]   
        [ , [ @shard_map_name = ] 'shard_map_name' ]   
        [ , [ @target_id = ] 'target_id' OUTPUT ]
```

#### <a name="arguments"></a>Argumenten
[ **@target_group_name =** ] 'target_group_name'  
De naam van de doelgroep waarop het lid wordt toegevoegd. target_group_name is nvarchar(128) met geen standaardwaarde.

[  **@membership_type =** ] 'membership_type'  
Hiermee geeft u aan als lid van de doel-worden opgenomen of uitgesloten. target_group_name is nvarchar(128) met standaard van 'Toevoegen'. Geldige waarden voor target_group_name zijn 'Toevoegen' of 'Uitsluiten'.

[ **@target_type =** ] 'target_type'  
Het type van de doeldatabase of -verzameling, inclusief alle databases in een server, alle databases in een elastische pool, alle databases in een shard-toewijzing of een afzonderlijke database. target_type is nvarchar(128) met geen standaardwaarde. Valid values for target_type are ‘SqlServer’, ‘SqlElasticPool’, ‘SqlDatabase’, or ‘SqlShardMap’. 

[  **@refresh_credential_name =** ] 'refresh_credential_name'  
De naam van de SQL Database-server. refresh_credential_name is nvarchar(128) met geen standaardwaarde.

[ **@server_name =** ] 'server_name'  
De naam van de SQL-Database-server die moet worden toegevoegd aan de opgegeven doelgroep. servernaam moet worden opgegeven wanneer target_type 'SqlServer'. servernaam is nvarchar(128) met geen standaardwaarde.

[  **@database_name =** ] 'database_name'  
De naam van de database die moet worden toegevoegd aan de opgegeven doelgroep. Database_Name moet worden opgegeven wanneer target_type "SqlDatabase". databasenaam is nvarchar(128) met geen standaardwaarde.

[ **@elastic_pool_name =** ] 'elastic_pool_name'  
De naam van de elastische groep die moet worden toegevoegd aan de opgegeven doelgroep. elastic_pool_name moet worden opgegeven wanneer target_type 'SqlElasticPool'. elastic_pool_name is nvarchar(128) met geen standaardwaarde.

[  **@shard_map_name =** ] 'shard_map_name'  
De naam van de groep van de shard-kaart die moet worden toegevoegd aan de opgegeven doelgroep. elastic_pool_name should be specified when target_type is ‘SqlSqlShardMap’. shard_map_name is nvarchar(128) met geen standaardwaarde.

[  **@target_id =** ] target_group_id uitvoer  
De doel-id-nummer toegewezen aan het lid van de doel-als gemaakt toegevoegd aan de doelgroep. target_id is een uitvoervariabele van het type uniqueidentifier, met een standaard null.
Geretourneerde waarden (geslaagd) 0 of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Een taak wordt uitgevoerd op alle individuele databases in een SQL Database-server of in een elastische pool op het moment van uitvoering, wanneer er een server met SQL-Database of elastische pool is opgenomen in de doelgroep.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
In het volgende voorbeeld wordt alle databases in de Londen en NewYork-servers toegevoegd aan de groep Servers onderhouden van klantgegevens. U moet verbinding maken met de opgegeven bij het maken van de taakagent in dit geval ElasticJobs taken-database.

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
        [ , [ @target_id = ] 'target_id']
```



Arguments [ @target_group_name = ] 'target_group_name'  
De naam van de doelgroep van waaruit het lid van de groep doel te verwijderen. target_group_name is nvarchar(128) met geen standaardwaarde.

[ @target_id = ] target_id  
 De doel-id-nummer toegewezen aan het lid van de doel moet worden verwijderd. target_id is een unieke id, met een standaard null.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout)

#### <a name="remarks"></a>Opmerkingen
Doelgroepen bieden een eenvoudige manier om een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
Het volgende voorbeeld wordt de Londen-server verwijderd uit de groep Servers onderhouden van klantgegevens. U moet verbinding maken met de opgegeven bij het maken van de taakagent in dit geval ElasticJobs taken-database.

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
[ **@job_name =** ] 'job_name'  
De naam van de taak waarvoor u de geschiedenisrecords te verwijderen. job_name is nvarchar(128), met een standaard null. Job_id of job_name moet worden opgegeven, maar beide kunnen niet worden opgegeven.

[ **@job_id =** ] job_id  
 De taak-id-nummer van de taak voor de records die moeten worden verwijderd. job_id is uniqueidentifier, met een standaard null. Job_id of job_name moet worden opgegeven, maar beide kunnen niet worden opgegeven.

[ **@oldest_date =** ] oldest_date  
 De oudste record die moet worden behouden in de geschiedenis. oldest_date is DATETIME2, met een standaard null. Wanneer oldest_date wordt opgegeven, verwijdert sp_purge_jobhistory alleen de records die ouder dan de opgegeven waarde zijn.

#### <a name="return-code-values"></a>Retourcode waarden
0 (geslaagd) of 1 (fout) opmerkingen doelgroepen bieden een eenvoudige manier om een taak op een verzameling van databases.

#### <a name="permissions"></a>Machtigingen
Leden van de vaste serverrol sysadmin kunnen deze opgeslagen procedure uitvoeren standaard. Ze een gebruiker alleen om te kunnen bewaken van taken beperken, kunt u de gebruiker als onderdeel van de volgende databaserol in de taak agent database die is opgegeven bij het maken van de taakagent verlenen:
- jobs_reader

Zie de sectie machtiging in dit document voor meer informatie over de machtigingen van deze rollen. Alleen leden van sysadmin kunnen deze opgeslagen procedure gebruiken om te bewerken van de kenmerken van de taken die eigendom zijn van andere gebruikers.

#### <a name="examples"></a>Voorbeelden
In het volgende voorbeeld wordt alle databases in de Londen en NewYork-servers toegevoegd aan de groep Servers onderhouden van klantgegevens. U moet verbinding maken met de opgegeven bij het maken van de taakagent in dit geval ElasticJobs taken-database.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>Taak weergaven

De volgende weergaven zijn beschikbaar in de [taken database](sql-database-job-automation-overview.md#job-database).


|Weergave  |Description  |
|---------|---------|
|[jobs_executions](#jobsexecutions-view)     |  Geeft de taakgeschiedenis worden uitgevoerd.      |
|[jobs](#jobs-view)     |   Geeft alle taken.      |
|[job_versions](#jobversions-view)     |   Bevat alle versies van de taak.      |
|[jobsteps](#jobsteps-view)     |     Ziet u alle stappen in de huidige versie van elke taak.    |
|[jobstep_versions](#jobstepversions-view)     |     Ziet u alle stappen in alle versies van elke taak.    |
|[target_groups](#targetgroups-view)     |      Geeft alle doelgroepen.   |
|[target_group_members](#targetgroups-view)     |   Ziet u alle leden van alle doelgroepen.      |


### <a name="jobsexecutions-view"></a>jobs_executions weergeven

[jobs].[jobs_executions]

Geeft de taakgeschiedenis worden uitgevoerd.


|Kolomnaam|   Gegevenstype   |Description|
|---------|---------|---------|
|**job_execution_id**   |uniqueidentifier|  De unieke ID van een exemplaar van een taak kan worden uitgevoerd.
|**job_name**   |nvarchar(128)  |Naam van de taak.
|**job_id** |uniqueidentifier|  De unieke ID van de taak.
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt telkens wanneer die de taak is gewijzigd).
|**step_id**    |int|   (Voor deze taak) de unieke id voor de stap. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**is_active**| bits |Geeft aan of informatie actief of inactief is. 1 geeft aan dat actieve taken en 0 geeft aan dat niet-actief.
|**lifecycle**| nvarchar(50)|Waarde die de status van de taak aangeeft: 'Gemaakt', 'Wordt uitgevoerd', 'Is mislukt', 'Geslaagd', 'Overgeslagen', 'SucceededWithSkipped'|
|**create_time**|   datetime2(7)|   De datum en tijd waarop die de taak is gemaakt.
|**start_time** |datetime2(7)|  Datum en tijd de uitvoering van de taak. NULL als de taak is nog niet uitgevoerd.
|**end_time**|  datetime2(7)    |Datum en tijd worden uitgevoerd door de taak is voltooid. NULL als de taak is nog niet uitgevoerd of niet is voltooid nog worden uitgevoerd.
|**current_attempts**   |int    |Aantal keren dat die de stap opnieuw wordt geprobeerd. Bovenliggende taak zal zijn ingesteld op 0, onderliggende taakuitvoeringen is 1 of hoger op basis van het uitvoeringsbeleid.
|**current_attempt_start_time** |datetime2(7)|  Datum en tijd de uitvoering van de taak. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**last_message**   |nvarchar(max)| Geschiedenis-bericht, taak of de stap. 
|**target_type**|   nvarchar(128)   |Het type van de doeldatabase of -verzameling van databases, met inbegrip van alle databases in een server, alle databases in een elastische pool of een database. Valid values for target_type are ‘SqlServer’, ‘SqlElasticPool’ or ‘SqlDatabase’. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**target_id**  |uniqueidentifier|  Unieke ID van het lid van de doel.  NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**target_group_name**  |nvarchar(128)  |De naam van de doelgroep. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**target_server_name**|    nvarchar(256)|  Naam van de SQL-Database-server die is opgenomen in de doelgroep. Alleen opgegeven als target_type 'SqlServer' is. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.
|**target_database_name**   |nvarchar(128)| Naam van de database die is opgenomen in de doelgroep. Opgegeven alleen als target_type "SqlDatabase" is. NULL geeft aan dat dit is het uitvoeren van de bovenliggende taak.


### <a name="jobs-view"></a>taken weergeven

[jobs].[jobs]

Geeft alle taken.

|Kolomnaam|   Gegevenstype|  Description|
|------|------|-------|
|**job_name**|  nvarchar(128)   |Naam van de taak.|
|**job_id**|    uniqueidentifier    |De unieke ID van de taak.|
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt telkens wanneer die de taak is gewijzigd).|
|**Beschrijving**    |nvarchar(512)| Beschrijving voor de taak. ingeschakelde bit geeft aan of de taak is ingeschakeld of uitgeschakeld. 1 geeft aan dat de ingeschakelde taken en 0 geeft aan dat de uitgeschakelde taken.|
|**schedule_interval_type** |nvarchar(50)   |Waarde die aangeeft wanneer de taak wordt uitgevoerd: 'Eenmaal', 'Minuten', 'Uur', ' dagen', 'Weken', 'Maanden'
|**schedule_interval_count**|   int|    Het aantal perioden schedule_interval_type optreden tussen elke uitvoering van de taak.|
|**schedule_start_time**    |datetime2(7)|  De datum en tijd waarop die de taak is de laatste uitvoering is gestart.|
|**schedule_end_time**| datetime2(7)|   De datum en tijd waarop die de taak laatste voltooide uitvoering is.|


### <a name="jobversions-view"></a>job_versions weergeven

[jobs].[job_versions]

Bevat alle versies van de taak.

|Kolomnaam|   Gegevenstype|  Description|
|------|------|-------|
|**job_name**|  nvarchar(128)   |Naam van de taak.|
|**job_id**|    uniqueidentifier    |De unieke ID van de taak.|
|**job_version**    |int    |De versie van de taak (automatisch bijgewerkt telkens wanneer die de taak is gewijzigd).|


### <a name="jobsteps-view"></a>taakstappen weergeven

[taken]. [taakstappen]

Ziet u alle stappen in de huidige versie van elke taak.

|Kolomnaam    |Gegevenstype| Description|
|------|------|-------|
|**job_name**   |nvarchar(128)| Naam van de taak.|
|**job_id** |uniqueidentifier   |De unieke ID van de taak.|
|**job_version**|   int|    De versie van de taak (automatisch bijgewerkt telkens wanneer die de taak is gewijzigd).|
|**step_id**    |int    |(Voor deze taak) de unieke id voor de stap.|
|**step_name**  |nvarchar(128)  |(Voor deze taak) de unieke naam voor de stap.|
|**command_type**   |nvarchar(50)   |Het type van de opdracht moet worden uitgevoerd in de taakstap. Voor v1, waarde moet gelijk zijn aan en standaard ingesteld op 'TSql'.|
|**command_source** |nvarchar(50)|  Locatie van de opdracht. Voor v1, 'Inline' is de standaardinstelling en alleen geaccepteerd waarde.|
|**Opdracht**|   nvarchar(max)|  De opdrachten die worden uitgevoerd met elastische taken via command_type.|
|**credential_name**|   nvarchar(128)   |Naam van de database-scoped referentie gebruikt met het uitvoeren van de taak.|
|**target_group_name**| nvarchar(128)   |De naam van de doelgroep.|
|**target_group_id**|   uniqueidentifier|   De unieke ID van de doelgroep.|
|**initial_retry_interval_seconds**|    int |De vertraging voor de eerste nieuwe poging. Standaardwaarde is 1.|
|**maximum_retry_interval_seconds** |int|   De maximale wachttijd tussen nieuwe pogingen. Als de vertraging tussen nieuwe pogingen kan groter zijn dan deze waarde, wordt deze het beste in plaats daarvan beperkt tot op deze waarde. Standaardwaarde is 120.|
|**retry_interval_backoff_multiplier**  |echte|  De vermenigvuldiger om toe te passen aan de elke poging als meerdere taak uitvoering van stap aanmeldingspogingen mislukt. Standaardwaarde is 2.0.|
|**retry_attempts** |int|   Het aantal nieuwe pogingen te gebruiken als deze stap mislukt. De standaardwaarde van 10, waarmee wordt aangegeven geen nieuwe pogingen.|
|**step_timeout_seconds**   |int|   De hoeveelheid tijd in minuten tussen nieuwe pogingen. De standaardwaarde is 0 geeft aan een interval van 0 minuten dat.|
|**output_type**    |nvarchar(11)|  Locatie van de opdracht. In de huidige Preview-versie, 'Inline' is de standaardinstelling en alleen geaccepteerd waarde.|
|**output_credential_name**|    nvarchar(128)   |Naam van de referenties die moeten worden gebruikt voor verbinding met de doelserver voor het opslaan van de resultaten ingesteld.|
|**output_subscription_id**|    uniqueidentifier|   Unieke ID van het abonnement van de server\database bestemming voor de resultaten van de queryuitvoering is ingesteld.|
|**output_resource_group_name** |nvarchar(128)| Resourcegroepnaam waarin de doel-server zich bevindt.|
|**output_server_name**|    nvarchar(256)   |De naam van de doelserver voor de resultatenset.|
|**output_database_name**   |nvarchar(128)| Naam van de doeldatabase voor de resultatenset.|
|**output_schema_name** |nvarchar(max)| Naam van de doelschema. Standaard dbo, indien niet opgegeven.|
|**output_table_name**| nvarchar(max)|  Naam van de tabel voor het opslaan van de resultaten instellen vanuit de resultaten van de query. Tabel wordt gemaakt automatisch op basis van het schema van de resultaten die zijn ingesteld als deze nog niet bestaat. Schema moet overeenkomen met het schema van de resultatenset.|
|**max_parallelism**|   int|    Het maximale aantal databases per elastische pool die de taakstap op wordt uitgevoerd op een tijdstip. De standaardwaarde is NULL, wat betekent dat er geen beperking. |


### <a name="jobstepversions-view"></a>jobstep_versions weergeven

[jobs].[jobstep_versions]

Ziet u alle stappen in alle versies van elke taak. Het schema is vrijwel identiek aan [taakstappen](#jobsteps-view).

### <a name="targetgroups-view"></a>target_groups weergeven

[jobs].[target_groups]

Geeft een lijst van alle doelgroepen.

|Kolomnaam|Gegevenstype| Description|
|-----|-----|-----|
|**target_group_name**| nvarchar(128)   |De naam van de doelgroep, een verzameling van databases. 
|**target_group_id**    |uniqueidentifier   |De unieke ID van de doelgroep.

### <a name="targetgroupsmembers-view"></a>target_groups_members weergeven

[jobs].[target_groups_members]

Ziet u alle leden van alle doelgroepen.

|Kolomnaam|Gegevenstype| Description|
|-----|-----|-----|
|**target_group_name**  |nvarchar(128|De naam van de doelgroep, een verzameling van databases. |
|**target_group_id**    |uniqueidentifier   |De unieke ID van de doelgroep.|
|**membership_type**    |int|   Hiermee geeft u aan als lid van het doel is opgenomen of in de doelgroep uitgesloten. Geldige waarden voor target_group_name zijn 'Toevoegen' of 'Uitsluiten'.|
|**target_type**    |nvarchar(128)| Het type van de doeldatabase of -verzameling van databases, met inbegrip van alle databases in een server, alle databases in een elastische pool of een database. Valid values for target_type are ‘SqlServer’, ‘SqlElasticPool’, ‘SqlDatabase’, or ‘SqlShardMap’.|
|**target_id**  |uniqueidentifier|  Unieke ID van het lid van de doel.|
|**refresh_credential_name**    |nvarchar(128)  |Naam van de database-scoped referentie gebruikt voor verbinding met het lid van de doel.|
|**subscription_id**    |uniqueidentifier|  De unieke ID van het abonnement.|
|**resource_group_name**    |nvarchar(128)| De naam van de resourcegroep waarin het lid van de doel-zich bevindt.|
|**server_name**    |nvarchar(128)  |Naam van de SQL-Database-server die is opgenomen in de doelgroep. Alleen opgegeven als target_type 'SqlServer' is. |
|**database_name**  |nvarchar(128)  |Naam van de database die is opgenomen in de doelgroep. Opgegeven alleen als target_type "SqlDatabase" is.|
|**elastic_pool_name**  |nvarchar(128)| De naam van de elastische groep die is opgenomen in de doelgroep. Opgegeven alleen als target_type 'SqlElasticPool' is.|
|**shard_map_name** |nvarchar(128)| De naam van de shard-toewijzing die is opgenomen in de doelgroep. Specified only when target_type is ‘SqlShardMap’.|


## <a name="resources"></a>Resources

 - ![Onderwerp koppelingspictogram](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "onderwerp koppelingspictogram") [conventies voor Transact-SQL-syntaxis](https://docs.microsoft.com/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql)  


## <a name="next-steps"></a>Volgende stappen

- [Elastische taken maken en beheren met PowerShell](elastic-jobs-powershell.md)
- [Autorisatie en machtigingen voor SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
  
