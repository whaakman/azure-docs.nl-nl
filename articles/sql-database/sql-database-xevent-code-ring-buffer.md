---
title: XEvent-ringbuffer code voor SQL Database | Microsoft Docs
description: Bevat een voorbeeld van Transact-SQL-code die eenvoudig en snel door gebruik van het doel ringbuffer in Azure SQL Database is gemaakt.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 0da4626943a7223b24baac80121b5da0062c0d73
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237074"
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Doelcode ringbuffer voor uitgebreide gebeurtenissen in SQL-Database

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Wilt u een codevoorbeeld voor de eenvoudigste snelle manier om gegevens vastleggen en het rapport voor een uitgebreide gebeurtenis tijdens een test. De eenvoudigste doel voor de gegevens van uitgebreide gebeurtenis is de [ringbuffer doel](https://msdn.microsoft.com/library/ff878182.aspx).

In dit onderwerp wordt een voorbeeld van de Transact-SQL-code die:

1. Maakt een tabel met gegevens om te demonstreren met.
2. Hiermee maakt u een sessie voor een bestaande uitgebreide gebeurtenis, namelijk **sqlserver.sql_statement_starting**.
   
   * De gebeurtenis is beperkt tot de SQL-instructies met een bepaalde Update-tekenreeks: **instructie als '% UPDATE tabEmployee %'**.
   * De uitvoer van de gebeurtenis verzenden naar een doel van het type ringbuffer, namelijk wil **package0.ring_buffer**.
3. Start de gebeurtenissessie.
4. Problemen met een aantal eenvoudige UPDATE van de SQL-instructies.
5. Problemen met een SQL SELECT-instructie om op te halen van gebeurtenis-uitvoer van de ringbuffer.
   
   * **sys.dm_xe_database_session_targets** en lid zijn van andere dynamische beheerweergave (DMV's).
6. Stopt de gebeurtenissessie.
7. Komt het doel ringbuffer, om de resources vrij te geven.
8. Komt de gebeurtenissessie en de demo-tabel.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account en -abonnement. U  kunt zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). 
* Elke database die kunt u een tabel in.
  
  * U kunt eventueel [maken een **AdventureWorksLT** demonstratiedatabase](sql-database-get-started.md) in minuten.
* SQL Server Management Studio (ssms.exe), in het ideale geval de meest recente maandelijkse updateversie. 
  U kunt de meest recente ssms.exe van downloaden:
  
  * Onderwerp [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx).
  * [Een directe koppeling naar de download.](https://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Codevoorbeeld

Met zeer kleine wijziging, kan het volgende voorbeeld ringbuffer op Azure SQL Database of Microsoft SQL Server worden uitgevoerd. Het verschil is de aanwezigheid van het knooppunt '_database' naam van een dynamische beheerweergave (DMV's), die wordt gebruikt in de component FROM in stap 5. Bijvoorbeeld:

* sys.dm_xe **_database**_session_targets
* sys.dm_xe_session_targets

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a>Ring Buffer inhoud

We ssms.exe gebruikt voor het uitvoeren van de voorbeeldcode.

U kunt de resultaten, we de cel onder de kop van de kolom hebt geklikt **target_data_XML**.

Klik in het deelvenster met resultaten we de cel onder de kop van de kolom hebt geklikt **target_data_XML**. Klikt u op een ander tabblad van het bestand in ssms.exe waarin de inhoud van de resultaatcel wordt weergegeven, als XML gemaakt.

De uitvoer wordt weergegeven in het volgende blok. Het lijkt veel, maar dit is slechts twee **<event>** elementen.

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Release van resources die zijn ondergebracht door uw ringbuffer

Wanneer u klaar bent met uw ringbuffer, u kunt deze verwijderen en de daarbij behorende bronnen uitgeven van de release een **ALTER** als volgt uit:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


De definitie van de gebeurtenissessie is bijgewerkt, maar niet verwijderd. U kunt later een ander exemplaar van de ringbuffer toevoegen aan de gebeurtenissessie:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Meer informatie

Het primaire onderwerp voor uitgebreide gebeurtenissen op Azure SQL Database is:

* [Extended event-overwegingen in SQL Database](sql-database-xevent-db-diff-from-svr.md), die in tegenstelling tot bepaalde aspecten van uitgebreide gebeurtenissen die tussen Azure SQL Database versus Microsoft SQL Server verschillen.

Andere onderwerpen voor het voorbeeld van code voor uitgebreide gebeurtenissen zijn beschikbaar op de volgende koppelingen. U moet echter routinematig voorbeelden om te zien of het voorbeeld is gericht op Microsoft SQL Server en Azure SQL Database controleren. U kunt vervolgens beslissen of kleine wijzigingen nodig zijn om uit te voeren van het voorbeeld.

* Codevoorbeeld voor Azure SQL Database: [gebeurtenisbestand doel code voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
