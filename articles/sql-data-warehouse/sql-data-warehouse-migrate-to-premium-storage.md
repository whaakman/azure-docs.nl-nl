---
title: Uw bestaande Azure datawarehouse migreren naar premium-opslag | Microsoft Docs
description: Instructies voor het migreren van een bestaand datawarehouse naar premium-opslag
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: ''
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 03/15/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 3b43bc17b7f9cf80a9520c5c573be3a48d82e4e7
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/14/2018
ms.locfileid: "31005635"
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a>Uw datawarehouse migreren naar premium-opslag
Azure SQL Data Warehouse onlangs geïntroduceerd [premium-opslag voor groter prestaties, voorspelbaarheid][premium storage for greater performance predictability]. Bestaande datawarehouses momenteel op standaardopslag kunnen nu worden gemigreerd naar de premium-opslag. U kunt profiteren van de automatische migratie van of als u liever om te beheren wanneer voor het migreren van (dit heeft betrekking op enige downtime), kunt u de migratie zelf doen.

Als u meer dan een datawarehouse hebt, gebruikt u de [schema voor automatische migratie] [ automatic migration schedule] om te bepalen wanneer dit wordt ook worden gemigreerd.

## <a name="determine-storage-type"></a>Opslagtype bepalen
Als u een datawarehouse hebt gemaakt voordat de volgende datums, u standaardopslag momenteel gebruikt.

| **Regio** | **Datawarehouse gemaakt voor deze datum** |
|:--- |:--- |
| Australië - oost |1 januari 2018 |
| China East |1 november 2016 |
| China - noord |1 november 2016 |
| Duitsland - centraal |1 november 2016 |
| Duitsland - noordoost |1 november 2016 |
| India - west |1 februari 2018 |
| Japan - west |1 februari 2018 |
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
| Australië - oost |19 maart 2018 |20 maart 2018 |
| China East |Al gemigreerd |Al gemigreerd |
| China - noord |Al gemigreerd |Al gemigreerd |
| Duitsland - centraal |Al gemigreerd |Al gemigreerd |
| Duitsland - noordoost |Al gemigreerd |Al gemigreerd |
| India - west |19 maart 2018 |20 maart 2018 |
| Japan - west |19 maart 2018 |20 maart 2018 |
| Noord-centraal VS |Al gemigreerd |Al gemigreerd |

## <a name="self-migration-to-premium-storage"></a>Zelfstandige migratie naar de premium-opslag
Als u bepalen wilt wanneer uw uitvaltijd wordt uitgevoerd, kunt u de volgende stappen uit om te migreren van een bestaand datawarehouse op een standard-opslag naar de premium-opslag. Als u deze optie kiest, moet u de zelfstandige migratie voltooien voordat de automatische migratie in deze regio begint. Dit zorgt ervoor dat u het risico van de automatische migratie een conflict veroorzaken vermijden (Raadpleeg de [schema voor automatische migratie][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Zelfstandige migratie-instructies
Migreren van uw datawarehouse, gebruikt u de back-up en herstellen van functies. Het gedeelte voor herstel van de migratie is ongeveer één uur per terabyte van opslag verwacht per datawarehouse. Als u wilt behouden dezelfde naam na de migratie is voltooid, voert u de [stappen om te wijzigen tijdens de migratie][steps to rename during migration].

1. [Onderbreken] [ Pause] uw datawarehouse. 
2. [Herstellen] [ Restore] van uw meest recente momentopname.
3. Verwijder het bestaande datawarehouse op een standard-opslag. **Als u niet in deze stap doet, wordt in rekening gebracht voor beide datawarehouses.**

> [!NOTE]
>
> Bij het herstellen van uw datawarehouse, moet u controleren dat de meest recente beschikbare herstelpunt vindt plaats nadat het datawarehouse is onderbroken.
>
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
2. [Onderbreken] [ Pause] 'MyDW_BeforeMigration'. 
3. [Herstellen] [ Restore] van de meest recente momentopname maken van een nieuwe database met de naam die wordt gebruikt om te worden (bijvoorbeeld 'MyDW').
4. Verwijderen van 'MyDW_BeforeMigration'. **Als u niet in deze stap doet, wordt in rekening gebracht voor beide datawarehouses.**


## <a name="next-steps"></a>Volgende stappen
Met het wijzigen naar premium-opslag hebt u ook een toenemend aantal blob-databasebestanden in de onderliggende architectuur van uw datawarehouse. Om de prestatievoordelen maximaliseren van deze wijziging, kunt u uw geclusterde columnstore-indexen met behulp van het volgende script opnieuw maken. Het script werkt door af te dwingen een deel van uw bestaande gegevens voor de aanvullende blobs. Als u geen actie onderneemt, wordt de gegevens natuurlijk na verloop van tijd distribueren, als u meer gegevens in de tabellen laden.

Als u problemen ondervindt met uw datawarehouse [Maak een ondersteuningsticket] [ create a support ticket] en verwijzing 'migratie naar de premium-opslag' als de mogelijke oorzaak.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: quickstart-scale-compute-portal.md
[mediumrc role]: resource-classes-for-workload-management.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
