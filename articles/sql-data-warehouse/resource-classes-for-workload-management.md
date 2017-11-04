---
title: Resource-klassen voor het beheer van de werkbelasting - Azure SQL Data Warehouse | Microsoft Docs
description: Richtlijnen voor het gebruik van resource klassen gelijktijdigheid van taken beheren en bronnen berekenen voor query's in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 122646f73b6e4e7c62eb0e6d4b6672b603d8acb2
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="resource-classes-for-workload-management"></a>Resource-klassen voor het beheer van de werkbelasting
Richtlijnen voor het gebruik van resource-klassen voor het beheren van het aantal gelijktijdige query's die gelijktijdig worden uitgevoerd en bronnen berekenen voor query's in Azure SQL Data Warehouse.
 
## <a name="what-is-workload-management"></a>Wat is werkbelasting management?
Beheer van de werkbelasting is de mogelijkheid om de algehele prestaties van alle query's optimaliseren. Een goed afgestemd werkbelasting voert query's en bewerkingen efficiënt ongeacht of ze rekenintensieve of i/o-intensieve zijn laden. 

SQL Data Warehouse levert werkbelasting voor omgevingen met meerdere gebruikers. Een datawarehouse is niet bedoeld voor werkbelastingen van meerdere tenants.

## <a name="what-are-resource-classes"></a>Wat zijn de klassen resource?
Resource-klassen zijn vooraf bepaalde limieten voor uitvoering van de query. SQL Data Warehouse beperkt de rekenresources voor elke query volgens de bronklasse. 

Resource klassen u helpt de algehele prestaties van uw datawarehouse-workload. Met resource klassen effectief kunt u beperkingen instellen voor het aantal query's die gelijktijdig worden uitgevoerd en de rekenresources toegewezen aan elke query voor het beheren van uw workload. 

- Kleinere resource klassen gebruiken minder rekenresources maar groter algemene query gelijktijdigheid inschakelen
- Grotere resource klassen bieden meer bronnen berekenen maar gelijktijdigheid van de query beperken

Resource-klassen zijn ontworpen voor activiteiten voor het beheer en manipuleren van gegevens. Sommige zeer complexe query's ook profiteren wanneer er grote joins en sorteert deze zodat het systeem wordt de query uitgevoerd in het geheugen in plaats van naar schijf lopen.

De volgende bewerkingen worden bepaald door de klassen van de resource:

* INSERT-SELECT-, UPDATE, DELETE
* Selecteer (tijdens het opvragen van gebruikerstabellen)
* ALTER INDEX - REBUILD of REORGANIZE
* ALTER TABLE REBUILD
* INDEX MAKEN
* GECLUSTERDE COLUMNSTORE-INDEX MAKEN
* TABLE AS SELECT (CTAS) MAKEN
* Gegevens laden
* Data movement-bewerkingen die worden uitgevoerd door de Data Movement Service (DMS)

> [!NOTE]  
> Selecteer vermeldingen op dynamische beheerweergaven (DMV's) of ander systeem weergaven niet worden geregeld door een van de limieten gelijktijdigheid van taken. U kunt het systeem ongeacht het aantal query's worden uitgevoerd op deze bewaken.
> 
> 

## <a name="static-and-dynamic-resource-classes"></a>Statische en dynamische resource klassen

Er zijn twee soorten klassen resource: dynamische en statische.

- **Statische Resource klassen** toewijzen dezelfde hoeveelheid geheugen ongeacht de huidige serviceniveau wordt gemeten in [datawarehouse eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md). Dit statische toewijzing betekent op grotere serviceniveaus dat kunt u meer query's uitvoeren in de resourceklasse van elke.  De statische resource klassen zijn benoemde staticrc10, staticrc20 staticrc30, staticrc40, staticrc50, staticrc60, staticrc70 en staticrc80. Deze resource-klassen zijn meest geschikt is voor oplossingen die bronklasse om extra rekenresources te vergroten.

- **Dynamische Resource klassen** toewijzen van een variabele hoeveelheid geheugen, afhankelijk van het huidige serviceniveau. Als u naar een grotere service opschalen, wordt in uw query's automatisch meer geheugen krijgt. De dynamische Bronklassen zijn benoemde smallrc, mediumrc largerc en xlargerc. Deze resource-klassen zijn meest geschikt is voor oplossingen welke toename compute scale voor aanvullende bronnen. 

De [prestatielagen](performance-tiers.md) dezelfde resourcegroep klassennamen gebruiken, maar hebben verschillende [geheugen en gelijktijdigheid specificaties](performance-tiers.md). 


## <a name="assigning-resource-classes"></a>Toewijzen van de resource-klassen

Resource-klassen worden geïmplementeerd door het toewijzen van gebruikers aan databaserollen. Wanneer een gebruiker een query uitvoert, wordt de query wordt uitgevoerd van de gebruiker bronklasse. Bijvoorbeeld, wanneer een gebruiker lid is van de databaserol smallrc of staticrc10, voer de query's met kleine hoeveelheden geheugen. Wanneer een databasegebruiker lid van de databaserollen xlargerc of staticrc80 is, wordt de query's uitvoeren met grote hoeveelheden geheugen. 

Gebruik de opgeslagen procedure voor een verhoging van een gebruiker bronklasse [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). 

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Als u wilt de bronklasse verlagen, gebruiken [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql).  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

De resourceklasse van de servicebeheerder is vast en kan niet worden gewijzigd.  De servicebeheerder is de gebruiker die is gemaakt tijdens het inrichtingsproces.

### <a name="default-resource-class"></a>Standaard-bronklasse
Standaard elke gebruiker is lid van de resourceklasse kleine **smallrc**. 

### <a name="resource-class-precedence"></a>Prioriteit van de resource-klasse
Gebruikers kunnen lid zijn van meerdere resource-klassen. Wanneer een gebruiker behoort tot meer dan één resourceklasse:

- Dynamische Bronklassen hebben voorrang op statische resource klassen. Bijvoorbeeld, als een gebruiker lid is van zowel mediumrc(dynamic) en staticrc80 (statische), query's worden uitgevoerd met mediumrc.
- Grotere resource klassen hebben voorrang op kleinere resource klassen. Bijvoorbeeld, als een gebruiker lid is van mediumrc en largerc, query's worden uitgevoerd met largerc. Op dezelfde manier als een gebruiker lid is van zowel staticrc20 en statirc80, query's worden uitgevoerd met staticrc80 resourcetoewijzingen.

### <a name="queries-exempt-from-resource-classes"></a>Query's is uitgesloten van resource-klassen
Sommige query's worden altijd uitgevoerd in de resource smallrc klasse ondanks dat de gebruiker lid van een grotere bronklasse is. Deze uitgesloten query's meetellen niet voor de limiet voor gelijktijdigheid van taken. Bijvoorbeeld, als de limiet voor gelijktijdige 16, kunnen veel gebruikers selecteren van systeemweergaven zonder enige impact op de sleuven beschikbaar gelijktijdigheid van taken.

De volgende instructies zijn uitgesloten van resource-klassen en altijd uitgevoerd in smallrc:

* MAKEN of DROP TABLE
* ALTER TABLE... SWITCH, GESPLITSTE of partitie samenvoegen
* ALTER INDEX UITSCHAKELEN
* INDEX VERWIJDEREN
* CREATE, UPDATE of DROP STATISTICS
* TABEL AFKAPPEN
* ALTER AUTORISATIE
* AANMELDING MAKEN
* CREATE, ALTER of DROP USER
* CREATE, ALTER of DROP PROCEDURE
* MAKEN of weergave verwijderen
* WAARDEN INVOEGEN
* Selecteer in het systeemweergaven en DMV 's
* UITLEGGEN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="recommendations"></a>Aanbevelingen
We raden aan een gebruiker die is toegewezen voor het uitvoeren van een specifiek type query maken of laden van bewerkingen. Vervolgens geeft die gebruiker een permanente bronklasse in plaats van de bronklasse regelmatig wijzigen. Gezien het feit dat statische resource klassen meer algemene controle van de werkbelasting bieden ook raadzaam die eerst met voordat het overwegen van dynamische Bronklassen.

### <a name="resource-classes-for-load-users"></a>Resource-klassen voor de load-gebruikers
`CREATE TABLE`Standaard maakt gebruik van geclusterde columnstore-indexen. Comprimeren van gegevens in een columnstore index is een geheugenintensieve bewerking en geheugendruk de kwaliteit van de index kan verminderen. Daarom bent u waarschijnlijk een hogere bronklasse vereist bij het laden van gegevens. U kunt maken van een gebruiker die is aangewezen voor het uitvoeren van de belasting en die gebruiker toewijzen aan een hogere bronklasse zodat hoeveelheden over voldoende geheugen beschikt.

De hoeveelheid geheugen die nodig zijn voor de belasting efficiënt verwerken, is afhankelijk van de aard van de tabel die is geladen en de omvang van gegevens. Zie voor meer informatie over geheugenvereisten [rijgroep kwaliteit maximaliseren](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Als u de geheugenvereiste hebt vastgesteld, kies of u de load-gebruiker toewijzen aan een statisch of dynamisch bronklasse.

- Gebruik een statisch bronklasse wanneer geheugenvereisten van de tabel binnen een bepaald bereik vallen. Belastingen uitgevoerd met de juiste geheugen. Wanneer u het datawarehouse schaalt, nodig de belasting niet meer geheugen. Met behulp van een statische bronklasse, blijven de geheugentoewijzing constante. Deze consistentie bespaart geheugen en kunt u meer query's gelijktijdig worden uitgevoerd. Het is raadzaam dat nieuwe oplossingen de klassen statische resource gebruiken eerst deze grotere controle bieden.
- Gebruik een dynamische bronklasse wanneer geheugenvereisten van de tabel uiteenlopen. Belasting mogelijk meer geheugen dan de huidige DWU of cDWU niveau bevat. Daarom voegt schalen van het datawarehouse meer geheugen toe aan load-bewerkingen, waarmee de belasting sneller uitvoeren.

### <a name="resource-classes-for-queries"></a>Resource-klassen voor query 's

Sommige query's zijn rekenintensieve en andere niet.  

- Kies een dynamische bronklasse wanneer query's complex zijn, maar geen hoge gelijktijdigheid van taken hoeft.  Dagelijks of wekelijks rapporten genereren is bijvoorbeeld een incidentele nodig voor bronnen. Als de rapporten grote hoeveelheden gegevens verwerken worden, biedt het schalen van het datawarehouse meer geheugen aan de bestaande resource-klasse van de gebruiker.
- Kies een statische bronklasse wanneer resource verwachtingen gedurende de dag variëren. Een statische bronklasse is bijvoorbeeld geschikt als het datawarehouse wordt opgevraagd door veel mensen. Tijdens het schalen van het datawarehouse, wordt de hoeveelheid geheugen toegewezen aan de gebruiker niet wijzigen. Als gevolg daarvan kunnen meer query's worden uitgevoerd op het systeem parallel.

Selecteren van een juiste geheugentoekenning is afhankelijk van veel factoren af zoals de hoeveelheid gegevens die zijn opgevraagd, de aard van het tabelschema en verschillende join selecteert en groepeert u predicaten. In het algemeen toewijzen van meer geheugen kunt u query's sneller zijn voltooid, maar verlaagt de algehele gelijktijdigheid van taken. Als geen gelijktijdigheid van taken een probleem is, komt te veel toewijzen van geheugen niet schadelijk zijn voor doorvoer. 

Gebruik andere resource klassen voor het afstemmen van de prestaties. De volgende sectie geeft een opgeslagen procedure waarmee u het beste bronklasse te achterhalen.

## <a name="example-code-for-finding-the-best-resource-class"></a>Voorbeeld van code voor de beste bronklasse zoeken
 
U kunt de volgende opgeslagen procedure gebruiken om te achterhalen gelijktijdigheid van taken en het geheugen verlenen per bronklasse op een gegeven SLO en de dichtstbijzijnde best bronklasse voor geheugen rekenintensieve CCI bewerkingen op niet-gepartitioneerde CCI tabel op een bepaalde bron-klasse:

Dit is het doel van deze opgeslagen procedure:  
1. Voor een overzicht van de gelijktijdigheid van taken en het geheugen per bronklasse op een gegeven SLO verlenen. Gebruiker moet NULL opgeven voor schema- en tablename in dit voorbeeld wordt getoond.  
2. Om te zien de dichtstbijzijnde best bronklasse voor het CCI geheugenintensieve tabel operations (belasting, tabel kopiëren, opnieuw opbouwen index, enz.) op niet-gepartitioneerde CCI op een bepaalde bron-klasse. De opgeslagen procedure gebruikt tabelschema om erachter te komen de vereiste geheugentoekenning.

### <a name="dependencies--restrictions"></a>Afhankelijkheden en beperkingen:
- Deze opgeslagen procedure is niet ontworpen voor het berekenen van de geheugenvereiste voor een cci gepartitioneerde tabel.    
- Deze opgeslagen procedure neemt geheugenvereiste in aanmerking voor de component SELECT van INSERT-CTAS-selecteren en wordt ervan uitgegaan dat het een SELECT.
- Deze opgeslagen procedure maakt gebruik van een tijdelijke tabel die beschikbaar is in de sessie waarin deze opgeslagen procedure is gemaakt.    
- Deze opgeslagen procedure is afhankelijk van de huidige aanbiedingen (bijvoorbeeld hardwareconfiguratie, DMS config) en als een van die verandert vervolgens deze opgeslagen procedure werkt niet correct.  
- Deze opgeslagen procedure is afhankelijk van bestaande aangeboden gelijktijdigheid limiet en als die wordt gewijzigd vervolgens deze opgeslagen procedure werkt niet correct.  
- Deze opgeslagen procedure is afhankelijk van bestaande resource klasse aanbiedingen en als die wordt gewijzigd vervolgens deze opgeslagen procedure werkt niet correct.  

>  [!NOTE]  
>  Als u geen uitvoer krijgt na uitvoering van opgeslagen procedure met parameters die worden geleverd, kan er twee gevallen. <br />1. Beide DW-Parameter bevat een ongeldige SLO waarde <br />2. Of er is geen overeenkomende resourceklasse voor de CCI-bewerking op de tabel. <br />Op DW100, bijvoorbeeld, is de hoogste geheugentoekenning beschikbaar 400 MB, en als tabelschema smal overschrijden de vereiste van 400 MB.
      
### <a name="usage-example"></a>Voorbeeld van gebruik:
Syntaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:Ofwel een NULL-parameter voor de huidige DWU extraheren uit de DW-database of geef een ondersteunde DWU in de vorm van 'DW100' opgeven
2. @SCHEMA_NAME:Geef een schemanaam van de tabel
3. @TABLE_NAME:Geef de tabelnaam van een met het belang

Voorbeelden voor het uitvoeren van deze opgeslagen procedure:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

De volgende instructie maakt Table1 die wordt gebruikt in de voorgaande voorbeelden.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>Definitie van de opgeslagen procedure

```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temp table (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```




## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Zie voor meer informatie over hoe groter resource klassen kan worden verbeterd geclusterde columnstore-index kwaliteit [geheugen optimalisaties voor de compressie columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
