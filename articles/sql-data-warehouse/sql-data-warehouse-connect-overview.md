<properties
   pageTitle="Verbinding maken met Azure SQL Data Warehouse | Microsoft Azure"
   description="Overzicht van verbindingen voor het maken van verbinding met Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/20/2016"
   ms.author="sonyama;barbkess"/>

# Verbinding maken met Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-connect-overview.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Stuurprogramma's](sql-data-warehouse-connection-strings.md)

Overzicht van verbinding maken met Azure SQL Data Warehouse. 

## Verbindingstekenreeks voor ontdekken in de portal

De SQL Data Warehouse wordt gekoppeld aan een Azure SQL Server. Om verbinding te maken hebt u de volledig gekwalificeerde naam van de server (**servernaam**.database.windows.net*) nodig.

Ga als volgt te werk om de volledig gekwalificeerde servernaam te vinden:

1. Ga naar de [Azure-portal][].
2. Klik op **SQL-databases** en vervolgens op de database waarmee u verbinding wilt maken. In dit voorbeeld wordt de voorbeelddatabase AdventureWorksDW gebruikt.
3. Zoek de volledige servernaam.

    ![Volledige servernaam][1]

## Verbindingsinstellingen
SQL Data Warehouse standaardiseert enkele instellingen tijdens het maken van de verbinding en het maken van objecten. Deze instellingen kunnen niet worden genegeerd.

| Database-instelling   | Waarde                        |
| :----------------- | :--------------------------- |
| ANSI_NULLS         | AAN                           |
| QUOTED_IDENTIFIERS | AAN                           |
| NO_COUNT           | UIT                          |
| DATEFORMAT         | mdy                          |
| DATEFIRST          | 7                            |
| Sortering van de database | SQL_Latin1_General_CP1_CI_AS |

## Sessies en aanvragen
Als er verbinding is gemaakt en er een sessie tot stand is gebracht, bent u klaar om query’s te schrijven naar en in te dienen bij SQL Data Warehouse.

Elke query wordt vertegenwoordigd door een of meer aanvraag-id’s. Alle query's die via die verbinding zijn ingediend, maken deel uit van één sessie en worden daarom vertegenwoordigd door een enkele sessie-id.

Aangezien SQL Data Warehouse een gedistribueerd MPP-systeem (Massively Parallel Processing) is, worden zowel de sessie-id’s als de aanvraag-id’s enigszins anders weergegeven in vergelijking tot SQL Server.

Sessies en aanvragen worden logisch vertegenwoordigd door de desbetreffende id’s.

| Id | Voorbeeldwaarde |
| :--------- | :------------ |
| Sessie-id | SID123456     |
| Aanvraag-id | QID123456     |

Merk op dat de sessie-id vooraf wordt gegaan door SID (de korte benaming voor sessie-id) en dat de aanvragen vooraf worden gegaan door QID (de korte benaming voor query-id).

U hebt deze informatie nodig om uw query te identificeren wanneer u de prestaties van uw query's controleert. U kunt de prestaties van uw query’s controleren met de [Azure-portal] en de dynamische beheerweergaven.

Deze query geeft uw huidige sessie aan.

```sql
SELECT SESSION_ID()
;
```

Als u alle query's wilt weergeven die worden uitgevoerd of onlangs zijn uitgevoerd bij uw datawarehouse, kunt u het volgende voorbeeld gebruiken. Hiermee wordt een weergave gemaakt en deze wordt vervolgens weergegeven.

```sql
CREATE VIEW dbo.vSessionRequests
AS
SELECT   s.[session_id]                                 AS Session_ID
        ,s.[status]                                     AS Session_Status
        ,s.[login_name]                                 AS Session_LoginName
        ,s.[login_time]                                 AS Session_LoginTime
        ,r.[request_id]                                 AS Request_ID
        ,r.[status]                                     AS Request_Status
        ,r.[submit_time]                                AS Request_SubmitTime
        ,r.[start_time]                                 AS Request_StartTime
        ,r.[end_compile_time]                           AS Request_EndCompileTime
        ,r.[end_time]                                   AS Request_EndTime
        ,r.[total_elapsed_time]                         AS Request_TotalElapsedDuration_ms
        ,DATEDIFF(ms,[submit_time],[start_time])        AS Request_InitiateDuration_ms
        ,DATEDIFF(ms,[start_time],[end_compile_time])   AS Request_CompileDuration_ms
        ,DATEDIFF(ms,[end_compile_time],[end_time])     AS Request_ExecDuration_ms
        ,[label]                                        AS Request_QueryLabel
        ,[command]                                      AS Request_Command
        ,[database_id]                                  AS Request_Database_ID
FROM    sys.dm_pdw_exec_requests r
JOIN    sys.dm_pdw_exec_sessions s  ON  r.[session_id] = s.[session_id]
WHERE   s.[session_id] <> SESSION_ID()
;

SELECT * FROM dbo.vSessionRequests;
```

## Volgende stappen

Zie [Query’s uitvoeren bij Visual Studio][] als u wilt beginnen met het uitvoeren van query’s bij uw datawarehouse met Visual Studio en andere toepassingen.


<!--Arcticles-->

[Query’s uitvoeren bij Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Azure-portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=ago16_HO4-->


