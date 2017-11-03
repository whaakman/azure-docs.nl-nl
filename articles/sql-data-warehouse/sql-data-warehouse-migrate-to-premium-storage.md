---
title: Uw bestaande Azure datawarehouse migreren naar premium-opslag | Microsoft Docs
description: Instructies voor het migreren van een bestaand datawarehouse naar premium-opslag
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 860e50b532b4b0a21d3be54f087730070b0e56bb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a>Uw datawarehouse migreren naar premium-opslag
Azure SQL Data Warehouse onlangs geïntroduceerd [premium-opslag voor groter prestaties, voorspelbaarheid][premium storage for greater performance predictability]. Bestaande datawarehouses momenteel op standaardopslag kunnen nu worden gemigreerd naar de premium-opslag. U kunt profiteren van de automatische migratie van of als u liever om te beheren wanneer voor het migreren van (dit heeft betrekking op enige downtime), kunt u de migratie zelf doen.

Als u meer dan een datawarehouse hebt, gebruikt u de [schema voor automatische migratie] [ automatic migration schedule] om te bepalen wanneer dit wordt ook worden gemigreerd.

## <a name="determine-storage-type"></a>Opslagtype bepalen
Als u een datawarehouse hebt gemaakt voordat de volgende datums, u standaardopslag momenteel gebruikt.

| **Regio** | **Datawarehouse gemaakt voor deze datum** |
|:--- |:--- |
| Australië - oost |Premium-opslag is nog niet beschikbaar |
| China - oost |1 november 2016 |
| China - noord |1 november 2016 |
| Duitsland - centraal |1 november 2016 |
| Duitsland - noordoost |1 november 2016 |
| India - west |Premium-opslag is nog niet beschikbaar |
| Japan - west |Premium-opslag is nog niet beschikbaar |
| Noord-centraal VS |10 november 2016 |

## <a name="automatic-migration-details"></a>Details van de automatische migratie
Standaard gaan we uw database voor u tussen 18:00 uur en 6:00 uur in uw regio lokale tijd tijdens de [schema voor automatische migratie][automatic migration schedule]. Uw bestaande datawarehouse kan niet worden gebruikt tijdens de migratie. De migratie duurt ongeveer één uur per terabyte van opslag per datawarehouse. U wordt niet in rekening gebracht tijdens een gedeelte van de automatische migratie.

> [!NOTE]
> Wanneer de migratie voltooid is, wordt uw datawarehouse worden weer online is en kan worden gebruikt.
>
>

Microsoft neemt de volgende stappen uit om de migratie (deze hoeven niet alle betrokkenheid van uw kant) te voltooien. Stel in dit voorbeeld of uw bestaande datawarehouse op een standard-opslag is momenteel met de naam 'MyDW'.

1. Microsoft wijzigt de naam 'MyDW' naar 'MyDW_DO_NOT_USE_ [tijdstempel]'.
2. Microsoft pauzeert 'MyDW_DO_NOT_USE_ [tijdstempel]'. Gedurende deze tijd is een back-up gemaakt. Mogelijk ziet u meerdere onderbroken en hervat als er problemen ondervindt tijdens dit proces.
3. Microsoft maakt een nieuw datawarehouse met de naam 'MyDW' op premium-opslag van de back-up gemaakt in stap 2. 'MyDW' worden niet weergegeven tot na het herstellen voltooid is.
4. Nadat het herstel voltooid is 'MyDW' retourneert met dezelfde magazijn eenheden en status (onderbroken of actieve) is dat deze vóór de migratie.
5. Nadat de migratie voltooid is, verwijdert Microsoft 'MyDW_DO_NOT_USE_ [tijdstempel]'.

> [!NOTE]
> De volgende instellingen uitvoeren als onderdeel van de migratie niet via:
>
> * Controle op het databaseniveau van de moet opnieuw worden ingeschakeld.
> * Firewall-regels op het databaseniveau van de moeten opnieuw worden toegevoegd. Firewallregels op serverniveau worden niet getroffen.
>
>

### <a name="automatic-migration-schedule"></a>Planning voor automatische migratie
Automatische migraties optreden tussen 18:00 uur en 6:00 uur (lokale tijd per regio) tijdens het volgende schema van de storing.

| **Regio** | **Geschatte begindatum** | **Geschatte einddatum** |
|:--- |:--- |:--- |
| Australië - oost |Nog niet worden vastgesteld |Nog niet worden vastgesteld |
| China - oost |9 januari 2017 |13 januari 2017 |
| China - noord |9 januari 2017 |13 januari 2017 |
| Duitsland - centraal |9 januari 2017 |13 januari 2017 |
| Duitsland - noordoost |9 januari 2017 |13 januari 2017 |
| India - west |Nog niet worden vastgesteld |Nog niet worden vastgesteld |
| Japan - west |Nog niet worden vastgesteld |Nog niet worden vastgesteld |
| Noord-centraal VS |9 januari 2017 |13 januari 2017 |

## <a name="self-migration-to-premium-storage"></a>Zelfstandige migratie naar de premium-opslag
Als u bepalen wilt wanneer uw uitvaltijd wordt uitgevoerd, kunt u de volgende stappen uit om te migreren van een bestaand datawarehouse op een standard-opslag naar de premium-opslag. Als u deze optie kiest, moet u de zelfstandige migratie voltooien voordat de automatische migratie in deze regio begint. Dit zorgt ervoor dat u het risico van de automatische migratie een conflict veroorzaken vermijden (Raadpleeg de [schema voor automatische migratie][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Zelfstandige migratie-instructies
Migreren van uw datawarehouse, gebruikt u de back-up en herstellen van functies. Het gedeelte voor herstel van de migratie is ongeveer één uur per terabyte van opslag verwacht per datawarehouse. Als u wilt behouden dezelfde naam na de migratie is voltooid, voert u de [stappen om te wijzigen tijdens de migratie][steps to rename during migration].

1. [Onderbreken] [ Pause] uw datawarehouse. Dit vindt een automatische back-up.
2. [Herstellen] [ Restore] van uw meest recente momentopname.
3. Verwijder het bestaande datawarehouse op een standard-opslag. **Als u niet in deze stap doet, wordt in rekening gebracht voor beide datawarehouses.**

> [!NOTE]
> De volgende instellingen uitvoeren als onderdeel van de migratie niet via:
>
> * Controle op het databaseniveau van de moet opnieuw worden ingeschakeld.
> * Firewall-regels op het databaseniveau van de moeten opnieuw worden toegevoegd. Firewallregels op serverniveau worden niet getroffen.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Wijzig de naam van datawarehouse tijdens de migratie (optioneel)
Twee databases op dezelfde logische server kunnen niet dezelfde naam hebben. SQL Data Warehouse ondersteunt nu de mogelijkheid om te wijzigen van een datawarehouse.

Stel in dit voorbeeld of uw bestaande datawarehouse op een standard-opslag is momenteel met de naam 'MyDW'.

1. Wijzig de naam 'MyDW' met behulp van de volgende opdracht ALTER DATABASE. (In dit voorbeeld we je Wijzig de naam 'MyDW_BeforeMigration'.)  Deze opdracht alle bestaande transacties wordt gestopt en moet worden uitgevoerd in de database master kan slagen.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Onderbreken] [ Pause] 'MyDW_BeforeMigration'. Dit vindt een automatische back-up.
3. [Herstellen] [ Restore] van de meest recente momentopname maken van een nieuwe database met de naam die wordt gebruikt om te worden (bijvoorbeeld 'MyDW').
4. Verwijderen van 'MyDW_BeforeMigration'. **Als u niet in deze stap doet, wordt in rekening gebracht voor beide datawarehouses.**


## <a name="next-steps"></a>Volgende stappen
Met het wijzigen naar premium-opslag hebt u ook een toenemend aantal blob-databasebestanden in de onderliggende architectuur van uw datawarehouse. Om de prestatievoordelen maximaliseren van deze wijziging, kunt u uw geclusterde columnstore-indexen met behulp van het volgende script opnieuw maken. Het script werkt door af te dwingen een deel van uw bestaande gegevens voor de aanvullende blobs. Als u geen actie onderneemt, wordt de gegevens natuurlijk na verloop van tijd distribueren, als u meer gegevens in de tabellen laden.

**Vereisten:**

- Het datawarehouse moet worden uitgevoerd met 1000 datawarehouse eenheden of hoger (Zie [scale rekenkracht][scale compute power]).
- De gebruiker uitvoeren van het script moet in de [mediumrc rol] [ mediumrc role] of hoger. Een gebruiker toevoegen aan deze rol, uitvoeren van het volgende:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table to control index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, the below can be re-run to restart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Als u problemen ondervindt met uw datawarehouse [Maak een ondersteuningsticket] [ create a support ticket] en verwijzing 'migratie naar de premium-opslag' als de mogelijke oorzaak.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
