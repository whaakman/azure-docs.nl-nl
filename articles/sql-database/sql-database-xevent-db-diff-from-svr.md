---
title: Uitgebreide gebeurtenissen in SQL Database | Microsoft Docs
description: Beschrijving van extended events (XEvents) in Azure SQL Database, en hoe gebeurtenissessies enigszins afwijken van de event-sessies in Microsoft SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 482faaea7089e095da13a2bae5f5937e20d50616
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246749"
---
# <a name="extended-events-in-sql-database"></a>Uitgebreide gebeurtenissen in SQL-Database
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

In dit onderwerp wordt uitgelegd hoe de implementatie van uitgebreide gebeurtenissen in Azure SQL Database is enigszins in vergelijking met uitgebreide gebeurtenissen in Microsoft SQL Server.

- SQL Database V12 opgedaan de functie voor uitgebreide gebeurtenissen in de tweede helft van de agenda 2015.
- SQL Server heeft uitgebreide gebeurtenissen sinds 2008.
- De functieset uitgebreide gebeurtenissen op SQL-Database is een robuuste subset van de functies van SQL Server.

*XEvents* is een informeel bijnaam die soms wordt gebruikt voor 'uitgebreide gebeurtenissen' in de blogs en andere informele locaties.

Meer informatie over uitgebreide gebeurtenissen voor Azure SQL Database en Microsoft SQL Server, is beschikbaar op:

- [Quick Start: Uitgebreide gebeurtenissen in SQL Server](https://msdn.microsoft.com/library/mt733217.aspx)
- [Uitgebreide gebeurtenissen](https://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Vereisten

In dit onderwerp wordt ervan uitgegaan dat u al enige kennis hebt van:

- [Azure SQL Database-service](https://azure.microsoft.com/services/sql-database/).
- [Uitgebreide gebeurtenissen](https://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.

- Het grootste deel van onze documentatie over uitgebreide gebeurtenissen geldt voor zowel SQL Server en SQL-Database.

Eerdere blootstelling aan de volgende items is handig bij het kiezen van de gebeurtenisbestand als de [doel](#AzureXEventsTargets):

- [Azure Storage-service](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Azure PowerShell gebruiken met Azure Storage](../storage/common/storage-powershell-guide-full.md) -bevat uitgebreide informatie over PowerShell en de Azure Storage-service.

## <a name="code-samples"></a>Codevoorbeelden

Verwante onderwerpen bieden twee voorbeelden van code:


- [Doelcode ringbuffer voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-ring-buffer.md)
    - Korte eenvoudige Transact-SQL-script.
    - We in het code-voorbeeld-onderwerp dat, wanneer u klaar bent met een doel ringbuffer, u de bronnen vrijgeven moet door het uitvoeren van een alter neerzetten benadrukken `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` instructie. U kunt later een ander exemplaar van ringbuffer door toevoegen `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Doelcode gebeurtenisbestand voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-event-file.md)
    - Fase 1 is PowerShell voor het maken van een Azure Storage-container.
    - Fase 2 is Transact-SQL die gebruikmaakt van de Azure Storage-container.

## <a name="transact-sql-differences"></a>Verschillen Transact-SQL


- Wanneer u uitvoert het [maken GEBEURTENIS sessie](https://msdn.microsoft.com/library/bb677289.aspx) opdracht op de SQL Server, gebruikt u de **op SERVER** component. Maar op de SQL-Database die u gebruikt de **op DATABASE** component in plaats daarvan.


- De **op DATABASE** component is ook van toepassing op de [ALTER GEBEURTENIS sessie](https://msdn.microsoft.com/library/bb630368.aspx) en [neerzetten GEBEURTENIS sessie](https://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-opdrachten.


- Er is een best practice om op te nemen van de gebeurtenissessieoptie van **STARTUP_STATE = ON** in uw **maken GEBEURTENIS sessie** of **ALTER GEBEURTENIS sessie** instructies.
    - De **= ON** waarde biedt ondersteuning voor automatisch opnieuw opstarten na een herconfiguratie van de logische-database vanwege een failover.

## <a name="new-catalog-views"></a>Nieuwe catalogusweergaven

De uitgebreide gebeurtenissen-functie wordt ondersteund door verschillende [weergaven van catalogus](https://msdn.microsoft.com/library/ms174365.aspx). Catalogusweergaven vertellen u *metagegevens of definities* van de gebruiker gemaakte event-sessies in de huidige database. De weergaven retourneren geen informatie over exemplaren van actieve gebeurtenissessies.

| Naam van<br/>Catalogusweergave | Beschrijving |
|:--- |:--- |
| **sys.database_event_session_actions** |Retourneert een rij voor elke actie die bij elke gebeurtenis van een event-sessie. |
| **sys.database_event_session_events** |Retourneert een rij voor elke gebeurtenis in een event-sessie. |
| **sys.database_event_session_fields** |Retourneert een rij voor elke aanpassen kan kolom die expliciet is ingesteld op gebeurtenissen en -doelen. |
| **sys.database_event_session_targets** |Retourneert een rij voor elke gebeurtenisdoel voor de gebeurtenissessie van een. |
| **sys.database_event_sessions** |Retourneert een rij voor elke gebeurtenissessie in de database van SQL-Database. |

In Microsoft SQL Server, vergelijkbare catalogusweergaven hebben namen die zijn *.server\_*  in plaats van *.database\_*. Het naampatroon is, zoals **sys.server_event_%**.

## <a name="new-dynamic-management-views-dmvshttpsmsdnmicrosoftcomlibraryms188754aspx"></a>Nieuwe dynamische beheerweergaven [(DMV's)](https://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL-Database heeft [dynamische beheerweergave (DMV's)](https://msdn.microsoft.com/library/bb677293.aspx) die ondersteuning bieden voor uitgebreide gebeurtenissen. DMV's vertellen u *active* event-sessies.

| Naam van de DMV | Beschrijving |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Retourneert informatie over gebeurtenisacties sessie. |
| **sys.dm_xe_database_session_events** |Retourneert informatie over sessiegebeurtenissen. |
| **sys.dm_xe_database_session_object_columns** |Geeft de configuratiewaarden voor objecten die zijn gebonden aan een sessie. |
| **sys.dm_xe_database_session_targets** |Retourneert informatie over de sessie-doelen. |
| **sys.dm_xe_database_sessions** |Retourneert een rij voor elke gebeurtenissessie die is afgestemd op de huidige database. |

In Microsoft SQL Server, catalogusweergaven vergelijkbaar zijn met de naam zonder de  *\_database* gedeelte van de naam, zoals:

- **sys.dm_xe_sessions**, in plaats van naam<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>Zowel DMV 's
Er zijn aanvullende DMV's die gemeenschappelijk voor zowel de Azure SQL Database en de Microsoft SQL Server zijn voor uitgebreide gebeurtenissen:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Zoek de beschikbare uitgebreide gebeurtenissen, acties en doelen

U kunt een eenvoudige SQL uitvoeren **Selecteer** verkrijgen van een lijst van de beschikbare gebeurtenissen, acties en doel.

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Doelen voor uw SQL-Database-event-sessies

Hier vindt u doelen die de resultaten van de event-sessies op SQL-Database kunnen vastleggen:

- [Doelringbuffer](https://msdn.microsoft.com/library/ff878182.aspx) -kort bevat gebeurtenisgegevens in het geheugen.
- [Gebeurtenis teller doel](https://msdn.microsoft.com/library/ff878025.aspx) -telt alle gebeurtenissen die zich tijdens een sessie met uitgebreide gebeurtenissen voordoen.
- [Doelgebeurtenisbestand](https://msdn.microsoft.com/library/ff878115.aspx) -volledige buffers schrijft naar een Azure Storage-container.

De [Event Tracing voor Windows (ETW)](https://msdn.microsoft.com/library/ms751538.aspx) API is niet beschikbaar voor uitgebreide gebeurtenissen op SQL-Database.

## <a name="restrictions"></a>Beperkingen

Er zijn een paar beveiligingsgerelateerde verschillen befitting de cloudomgeving van SQL Database:

- Uitgebreide gebeurtenissen zijn gevonden op het isolatiemodel met één tenant. De gebeurtenissessie van een in de ene database geen toegang tot gegevens of gebeurtenissen uit een andere database.
- U kunt geen opdracht een **maken GEBEURTENIS sessie** -instructie in de context van de **master** database.

## <a name="permission-model"></a>Machtigingsmodel

U moet hebben **besturingselement** machtiging voor de database om uit te geven een **maken GEBEURTENIS sessie** instructie. De database-eigenaar (dbo) heeft **besturingselement** machtiging.

### <a name="storage-container-authorizations"></a>Storage-container autorisaties

De SAS-token genereren voor uw Azure Storage-container moet opgeven **rwl** voor de machtigingen. De **rwl** waarde biedt de volgende machtigingen:

- Lezen
- Schrijven
- Lijst

## <a name="performance-considerations"></a>Prestatieoverwegingen

Er zijn scenario's waarbij intensief gebruik van uitgebreide gebeurtenissen meer actieve geheugen verzamelen dan voor het algehele systeem in orde is. Daarom de Azure SQL Database-systeem dynamisch wordt ingesteld en beperkingen met betrekking tot het bedrag van het actieve geheugen die kan worden samengevoegd door een gebeurtenissessie past. Veel factoren gaat u naar de dynamische berekening.

Als u een foutmelding krijgen dat een maximum geheugen is afgedwongen ontvangt, zijn sommige corrigerende acties die u kunt uitvoeren:

- Minder gelijktijdige event-sessies worden uitgevoerd.
- Via uw **maken** en **ALTER** -instructies voor sessies van evenementen, verminderen de hoeveelheid geheugen die u opgeeft op de **MAX\_geheugen** component.

### <a name="network-latency"></a>Netwerklatentie

De **gebeurtenisbestand** doel tegenkomen netwerklatentie of fouten tijdens het vastleggen van gegevens naar Azure Storage-blobs. Andere gebeurtenissen in SQL-Database kunnen zijn vertraagd, terwijl ze wachten tot de netwerkcommunicatie om te voltooien. Deze vertraging kan vertragen uw workload.

- Als u wilt deze prestaties risico te voorkomen dat de instelling de **EVENT_RETENTION_MODE** optie naar **NO_EVENT_LOSS** in de definities van de event-sessie.

## <a name="related-links"></a>Verwante koppelingen

- [Azure PowerShell gebruiken met Azure Storage](../storage/common/storage-powershell-guide-full.md).
- [Azure Storage-Cmdlets](https://docs.microsoft.com/powershell/module/Azure.Storage)
- [Azure PowerShell gebruiken met Azure Storage](../storage/common/storage-powershell-guide-full.md) -bevat uitgebreide informatie over PowerShell en de Azure Storage-service.
- [Het Blob storage gebruiken met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/ms189522.aspx)
- [EVENT-sessie (Transact-SQL) maken](https://msdn.microsoft.com/library/bb677289.aspx)
- [Jonathan Kehayias blogberichten over uitgebreide gebeurtenissen in Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- De Azure *Service-Updates* webpagina teruggebracht door parameter naar Azure SQL Database:
    - [https://azure.microsoft.com/updates/?service=sql-database](https://azure.microsoft.com/updates/?service=sql-database)


Andere onderwerpen voor het voorbeeld van code voor uitgebreide gebeurtenissen zijn beschikbaar op de volgende koppelingen. U moet echter routinematig voorbeelden om te zien of het voorbeeld is gericht op Microsoft SQL Server en Azure SQL Database controleren. U kunt vervolgens beslissen of kleine wijzigingen nodig zijn om uit te voeren van het voorbeeld.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
