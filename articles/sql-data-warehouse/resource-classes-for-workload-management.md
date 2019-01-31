---
title: Resourceklassen voor het beheer van de werkbelasting - Azure SQL Data Warehouse | Microsoft Docs
description: Richtlijnen voor het gebruik van resource-klassen voor het beheren van gelijktijdigheid van taken en rekenresources voor query's in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/26/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 0dab8204450da60a6c1e46c8363e8d21dc307589
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453991"
---
# <a name="workload-management-with-resource-classes-in-azure-sql-data-warehouse"></a>Beheer van de werkbelasting met resourceklassen in Azure SQL Data Warehouse
Richtlijnen voor het gebruik van resource-klassen voor het beheren van geheugen en gelijktijdigheid voor query's in uw Azure SQL Data Warehouse.  
 
## <a name="what-is-workload-management"></a>Wat is beheer van de werkbelasting?
Beheer van de werkbelasting is de mogelijkheid om de algehele prestaties van alle query's optimaliseren. Een goed afgestemd werkbelasting wordt uitgevoerd voor query's en bewerkingen efficiënt, ongeacht of ze rekenintensieve of i/o-intensieve zijn laden.  SQL Data Warehouse biedt functionaliteit voor werkbelasting voor omgevingen met meerdere gebruikers. Een datawarehouse is niet bedoeld voor workloads met meerdere tenants.

De capaciteit van de prestaties van een datawarehouse wordt bepaald door de [datawarehouse-eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md). 

- De limieten voor geheugen en gelijktijdigheid voor alle profielen van de prestaties, Zie [limieten voor geheugen en gelijktijdigheid](memory-and-concurrency-limits.md).
- Als u wilt aanpassen prestaties capaciteit, kunt u [omhoog of omlaag schalen](quickstart-scale-compute-portal.md).

De capaciteit van de prestaties van een query wordt bepaald door de resourceklasse van de query. De rest van dit artikel wordt uitgelegd wat de resource-klassen zijn en hoe u deze kunt aanpassen.

## <a name="what-are-resource-classes"></a>Wat zijn resourceklassen?
De capaciteit van de prestaties van een query wordt bepaald door de resourceklasse van de gebruiker.  Resourceklassen worden vooraf bepaald resourcebeperkingen in Azure SQL Data Warehouse voor rekenresources en gelijktijdigheid van taken voor het uitvoeren van query's. Resourceklassen kunt u beperkingen instellen voor het aantal query's die gelijktijdig worden uitgevoerd en de compute-resources toegewezen aan elke query voor het beheren van uw workload. Is er een aandelentransactie uitgeschakeld tussen het geheugen en gelijktijdigheid.

- Kleinere resourceklassen verminderen de maximale hoeveelheid geheugen per query, maar tegelijkertijd verwerken.
- Grotere resourceklassen verhoogt de maximale hoeveelheid geheugen per query, maar beperkt. 

Er zijn twee soorten resourceklassen:

- Statische resources-klassen die zijn geschikt voor hogere gelijktijdigheid op de grootte van een gegevensset die is opgelost.
- Dynamische resourceklassen, die geschikt voor gegevenssets die zijn groeiende in grootte en prestaties verbeteren zijn als het serviceniveau omhoog wordt geschaald.   

Gelijktijdigheidssleuven resourceklassen gebruiken voor het meten van gebruik van resources.  [Gelijktijdigheidssleuven](#concurrency-slots) verderop in dit artikel worden beschreven. 

- Als u het Resourcegebruik voor de resourceklassen, Zie [limieten voor geheugen en gelijktijdigheid](memory-and-concurrency-limits.md#concurrency-maximums).
- Als u wilt de resourceklasse aanpassen, kunt u de query onder een andere gebruiker uitvoeren of [wijzigen van de huidige gebruiker resourceklasse](#change-a-users-resource-class) lidmaatschap. 

### <a name="static-resource-classes"></a>Statische resourceklassen
Statische resourceklassen toewijzen dezelfde hoeveelheid geheugen, ongeacht het huidige prestatieniveau, die worden gemeten in [datawarehouse-eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md). Omdat de query's krijgen dezelfde toewijzing van het geheugen, ongeacht het prestatieniveau [geschaalde uitbreiding van het datawarehouse](quickstart-scale-compute-portal.md) kunt u meer query's uitgevoerd binnen een resourceklasse.  Statische resourceklassen zijn ideaal als het gegevensvolume bekend is en constante.

De statische resourceklassen worden geïmplementeerd met deze vooraf gedefinieerde databaserollen:

- staticrc10
- staticrc20
- staticrc30
- staticrc40
- staticrc50
- staticrc60
- staticrc70
- staticrc80

### <a name="dynamic-resource-classes"></a>Dynamische resourceklassen
Dynamische Resourceklassen toewijzen een wisselende hoeveelheid geheugen, afhankelijk van het huidige serviceniveau. Hoewel de statische resourceklassen zijn nuttig voor hogere gelijktijdigheid en statische gegevensvolumes, zijn dynamische resourceklassen beter geschikt voor een groeiende of variabele hoeveelheid gegevens.  Wanneer u omhoog naar een grotere serviceniveau schalen, krijgt uw query's automatisch meer geheugen.  

De dynamische resourceklassen worden geïmplementeerd met deze vooraf gedefinieerde databaserollen:

- smallrc
- mediumrc
- largerc
- xlargerc 

### <a name="gen2-dynamic-resource-classes-are-truly-dynamic"></a>De dynamische resourceklassen Gen2 zijn echt dynamisch
Wanneer op de details van dynamische resourceklassen op Gen1 bestuderen, zijn er enkele gegevens die extra complexiteit toevoegen tot het begrijpen van hun gedrag:

- De klasse van de resources smallrc werkt met een vaste geheugenmodel als een statische resourceklasse.  Query's Smallrc krijg dynamisch niet meer geheugen als het serviceniveau van de wordt verhoogd.
- Als het niveau van service wijzigt, kan de gelijktijdigheid van de beschikbare query omhoog of omlaag gaan.
- Serviceniveaus schalen biedt geen een proportionele wijziging het geheugen toegewezen aan de dezelfde resourceklassen.

Op **Gen2 alleen**, dynamische resourceklassen worden echt dynamische adressen van de hierboven genoemde punten.  De nieuwe regel is 3-10-22-70 voor percentage geheugentoewijzingen voor kleine-medium-groot-xlarge resourceklassen, **ongeacht serviceniveau**.  De onderstaande tabel bevat de geconsolideerde details van het geheugen toegewezen percentages en het minimum aantal gelijktijdige query's die worden uitgevoerd, ongeacht het serviceniveau van de.

| Resourceklasse | Percentage geheugen | Min gelijktijdige query 's |
|:--------------:|:-----------------:|:----------------------:|
| smallrc        | 3%                | 32                     |
| mediumrc       | 10%               | 10                     |
| largerc        | 22%               | 4                      |
| xlargerc       | 70%               | 1                      |


### <a name="default-resource-class"></a>Standaard resourceklasse
Standaard is elke gebruiker lid is van de dynamische resourceklasse **smallrc**. 

De resourceklasse van de service-beheerder is vastgesteld op smallrc en kan niet worden gewijzigd.  De servicebeheerder is de gebruiker heeft gemaakt tijdens het inrichtingsproces.  De servicebeheerder in deze context is de aanmelding die is opgegeven voor 'serverbeheerder' wanneer een nieuw exemplaar van SQL Data Warehouse maken met een nieuwe server.

> [!NOTE]
> Gebruikers of groepen die zijn gedefinieerd als Active Directory-beheerder zijn ook servicebeheerders.
>
>

## <a name="resource-class-operations"></a>Klasse resourcebewerkingen

Resource-klassen zijn ontworpen voor betere prestaties voor activiteiten voor het beheren en manipuleren van gegevens. Complexe query's kunnen ook profiteren van die wordt uitgevoerd onder een grote resourceklasse. Bijvoorbeeld, queryprestaties voor grote samenvoegingen en sorteert kunnen verbeteren wanneer de resourceklasse groot genoeg is is voor het inschakelen van de query uit te voeren in het geheugen.

### <a name="operations-governed-by-resource-classes"></a>Bewerkingen die zijn onderworpen aan resource-klassen

Deze bewerkingen zijn onderworpen aan resource-klassen:

* INSERT-SELECT, UPDATE, DELETE
* Selecteer (bij het opvragen van gebruikerstabellen)
* ALTER INDEX - opnieuw samenstellen of REORGANIZE
* ALTER TABLE OPNIEUW MAKEN
* INDEX MAKEN
* GECLUSTERDE COLUMNSTORE-INDEX MAKEN
* CREATE TABLE AS SELECT (CTAS)
* Gegevens laden
* Bewerkingen voor gegevensverplaatsing die wordt uitgevoerd door de Data Movement Service (DMS)

> [!NOTE]  
> Selecteer instructies op de dynamische beheerweergave (DMV's) of ander systeem weergaven niet worden geregeld door een van de gelijktijdigheidslimieten. U kunt het systeem, ongeacht het aantal query's worden uitgevoerd op deze bewaken.
> 
> 

### <a name="operations-not-governed-by-resource-classes"></a>Bewerkingen niet worden geregeld door de resource-klassen
Aantal query's worden altijd uitgevoerd in de resourceklasse smallrc zelfs als de gebruiker lid van een grotere resourceklasse is. Deze uitgesloten query's tellen niet mee voor de limiet voor gelijktijdigheid. Bijvoorbeeld, als de limiet voor gelijktijdigheid 16 is, kunnen de veel gebruikers worden selecteren in systeemweergaven zonder gevolgen voor de beschikbare gelijktijdigheidssleuven in beslag.

De volgende instructies zijn vrijgesteld van resource-klassen en altijd in smallrc uitgevoerd:

* MAKEN of DROP TABLE
* DE TABEL WIJZIGEN... SWITCH, splitsen of samenvoegen partitie
* ALTER INDEX UITSCHAKELEN
* INDEX VERWIJDEREN
* MAKEN, bijwerken of DROP STATISTICS
* TRUNCATE TABLE
* ALTER AUTORISATIE
* AANMELDING MAKEN
* CREATE, ALTER of DROP USER
* CREATE, ALTER of DROP PROCEDURE
* MAKEN of de weergave niet verwijderen
* WAARDEN INVOEGEN
* Selecteer in systeemweergaven en DMV 's
* EXPLAIN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="concurrency-slots"></a>Gelijktijdigheidssleuven
Gelijktijdigheidssleuven zijn een handige manier om bij te houden van de beschikbare resources voor het uitvoeren van query's. Ze zijn, zoals tickets die u koopt als u wilt reserveren van plaatsen op een concert omdat zitplaatsen beperkt is. Het totale aantal gelijktijdigheidssleuven per datawarehouse wordt bepaald door het serviceniveau van de. Voordat een query kunt uitvoeren, moet het mogelijk zijn onvoldoende gelijktijdigheidssleuven reserveren. Wanneer een query is voltooid, worden de gelijktijdigheidssleuven vrijgegeven.  

- Een query uitvoeren met 10 gelijktijdigheidssleuven in beslag, hebben toegang tot 5 keer meer compute-bronnen dan query's met een met 2 gelijktijdigheidssleuven.
- Elke query is 10 gelijktijdigheidssleuven vereist en er nog 40 gelijktijdigheidssleuven, kunnen gelijktijdig worden alleen 4 query's uitgevoerd.
 
Alleen query's resourcegestuurd gelijktijdigheidssleuven in beslag nemen. Systeem-query's en sommige trivial query's meetellen niet als sleuven. Het exacte aantal gelijktijdigheidssleuven verbruikt, wordt bepaald door de resourceklasse van de query.

## <a name="view-the-resource-classes"></a>De resourceklassen weergeven

Resourceklassen worden geïmplementeerd als vooraf gedefinieerde databaserollen. Er zijn twee soorten resourceklassen: dynamische en statische. Een lijst van de resourceklassen wilt weergeven, gebruikt u de volgende query:

```sql
SELECT name 
FROM   sys.database_principals
WHERE  name LIKE '%rc%' AND type_desc = 'DATABASE_ROLE';
```

## <a name="change-a-users-resource-class"></a>De resourceklasse van een gebruiker wijzigen

Resourceklassen worden geïmplementeerd door het toewijzen van gebruikers aan de databaserollen. Wanneer een gebruiker een query uitvoert, wordt de query wordt uitgevoerd met de resourceklasse van de gebruiker. Bijvoorbeeld, wanneer een gebruiker lid van de databaserol smallrc of staticrc10 is, hun query's worden uitgevoerd met een kleine hoeveelheid geheugen. Wanneer een gebruiker van een database een lid van de databaserollen xlargerc of staticrc80 is, wordt de query's uitvoeren met grote hoeveelheden geheugen. 

Als u wilt vergroten de resourceklasse van een gebruiker, gebruikt u de opgeslagen procedure [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). 

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Als u wilt de resourceklasse verlagen, gebruik [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql).  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

## <a name="resource-class-precedence"></a>Prioriteit van de resource-klasse
Gebruikers kunnen lid zijn van meerdere resourceklassen. Wanneer een gebruiker behoort tot meer dan één resourceklasse:

- Dynamische resourceklassen hebben voorrang op de statische resourceklassen. Bijvoorbeeld, als een gebruiker is lid van zowel mediumrc(dynamic) en staticrc80 (statisch), query's worden uitgevoerd met mediumrc.
- Grotere resourceklassen hebben voorrang op kleinere resourceklassen. Bijvoorbeeld, als een gebruiker lid van mediumrc en largerc is, query's worden uitgevoerd met largerc. Op dezelfde manier als een gebruiker lid van zowel staticrc20 en statirc80 is, query's worden uitgevoerd met staticrc80 resourcetoewijzingen.

## <a name="recommendations"></a>Aanbevelingen
We raden u aan een gebruiker die is toegewezen aan het uitvoeren van een specifiek type query of load-bewerkingen. Vervolgens geven dat de gebruiker een permanente resource-klasse in plaats van de resourceklasse regelmatig te wijzigen. Gezien het feit dat statische resourceklassen meer algemene controle over de werkbelasting bieden ook raadzaam om met behulp van de eerste voordat dynamische resourceklassen.

### <a name="resource-classes-for-load-users"></a>Resourceklassen voor het load-gebruikers
`CREATE TABLE` Standaard maakt gebruik van geclusterde columnstore-indexen. Comprimeren van gegevens in een columnstore index is een geheugenintensieve bewerking en geheugendruk de kwaliteit van de index kan verminderen. Daarom bent u waarschijnlijk een hogere resourceklasse vereisen wanneer het laden van gegevens. Om ervoor te zorgen belastingen over voldoende geheugen beschikt, kunt u een gebruiker die is ingesteld voor het uitvoeren van loads maken en die gebruiker toewijzen aan een hogere resourceklasse.

Het geheugen dat nodig is voor het verwerken van belastingen efficiënt is afhankelijk van de aard van de tabel geladen en de grootte van de gegevens. Zie voor meer informatie over geheugenvereisten [rijgroep kwaliteit maximaliseren](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Nadat u de geheugenvereiste hebt bepaald, kiest u of de load-gebruiker toewijzen aan een statische of dynamische resourceklasse.

- Gebruik een statische resourceklasse wanneer geheugenvereisten van de tabel binnen een bepaald bereik vallen. Belastingen uitgevoerd met het juiste geheugen. Wanneer u het datawarehouse, nodig de belasting niet meer geheugen. Met behulp van een statische resourceklasse, blijven de geheugentoewijzingen constante. Deze consistentie geheugen te besparen en kunt u meer query's gelijktijdig worden uitgevoerd. Het is raadzaam dat nieuwe oplossingen maken gebruik van de statische resourceklassen eerst deze kunnen beheersen.
- Gebruik een dynamische resourceklasse wanneer sterk geheugenvereisten van de tabel verschilt. Belasting mogelijk meer geheugen dan de huidige DWU of cDWU niveau biedt. Daarom schalen van het datawarehouse meer geheugen aan toegevoegd load-bewerkingen, waardoor loads naar sneller.

### <a name="resource-classes-for-queries"></a>Resourceklassen voor query 's

Aantal query's zijn rekenintensieve en andere niet.  

- Kies een dynamische resourceklasse wanneer query's complexe zijn, maar geen hoge gelijktijdigheid hoeft.  Genereren van rapporten dagelijks of wekelijks is bijvoorbeeld een incidentele nodig voor resources. Als de rapporten grote hoeveelheden gegevens verwerken worden, vindt het datawarehouse schalen u meer geheugen aan de bestaande resourceklasse van de gebruiker.
- Kies een statische resourceklasse wanneer resource verwachtingen gedurende de dag varieert. Bijvoorbeeld, een statische resourceklasse is geschikt als het datawarehouse is opgevraagd door veel mensen. Wanneer u het datawarehouse, wordt de hoeveelheid geheugen toegewezen aan de gebruiker niet wijzigen. Meer query's kunnen als gevolg daarvan kunnen parallel op het systeem worden uitgevoerd.

Selecteren van een juiste geheugentoekenning is afhankelijk van verschillende factoren, zoals de hoeveelheid gegevens die zijn opgevraagd, de aard van de tabel schema's en verschillende join, selecteren en predikaten groeperen. In het algemeen toewijzen van meer geheugen kunt u query's sneller zijn voltooid, maar verlaagt de algehele gelijktijdigheid. Als u niet de gelijktijdigheid is een probleem, schaadt toewijzen van te veel geheugen niet doorvoer. 

Voor het afstemmen van prestaties, andere resource-klassen te gebruiken. De volgende sectie biedt een opgeslagen procedure waarmee u de beste resourceklasse te achterhalen.

## <a name="example-code-for-finding-the-best-resource-class"></a>Voorbeeldcode voor het vinden van de beste resourceklasse
 
U kunt de volgende opgeslagen procedure op **Gen1 alleen** om te achterhalen gelijktijdigheid en geheugen verlenen per resourceklasse op een bepaalde SLO en de dichtstbijzijnde aanbevolen resourceklasse voor het geheugen intensieve CCI bewerkingen op CCI niet-gepartitioneerde tabel een opgegeven resourceklasse:

Dit is het doel van deze opgeslagen procedure:  
1. De gelijktijdigheid van taken en voor geheugentoewijzing in batchmodus per resourceklasse op een bepaalde SLO zien. Gebruiker moet null zijn voor schema- en tablename bieden, zoals wordt weergegeven in dit voorbeeld.  
2. Om te zien van de dichtstbijzijnde aanbevolen resourceklasse voor het CCI geheugenintensieve bewerkingen (load, tabel, kopiëren, opnieuw index, enzovoort) op niet-gepartitioneerde CCI tabel in een opgegeven resourceklasse. De opgeslagen procedure maakt gebruik van tabelschema om de vereiste geheugentoekenning erachter te komen.

### <a name="dependencies--restrictions"></a>Afhankelijkheden en beperkingen:
- Deze opgeslagen procedure is niet ontworpen voor het berekenen van de geheugenvereiste voor een gepartitioneerde cci-tabel.    
- Deze opgeslagen procedure neemt de geheugenvereiste in aanmerking voor de component SELECT van CTAS of INSERT-te selecteren en wordt ervan uitgegaan dat het een SELECT is.
- Deze opgeslagen procedure maakt gebruik van een tijdelijke tabel, die beschikbaar is in de sessie waarin deze opgeslagen procedure is gemaakt.    
- Deze opgeslagen procedure, is afhankelijk van de huidige aanbiedingen (bijvoorbeeld hardwareconfiguratie, DMS-configuratie) en als een van die wordt gewijzigd vervolgens deze opgeslagen procedure werkt niet correct.  
- Deze opgeslagen procedure, is afhankelijk van de limiet voor de bestaande aangeboden gelijktijdigheid en als wijzigingen vervolgens deze opgeslagen procedure zou werken niet goed.  
- Deze opgeslagen procedure, is afhankelijk van bestaande resource klasse aanbiedingen en als die wordt gewijzigd vervolgens deze opgeslagen procedure niet werkt goed.  

>  [!NOTE]  
>  Als er uitvoer na het uitvoeren van opgeslagen procedure met opgegeven parameters, kan er twee mogelijke situaties. <br />1. Een van beide DW-Parameter bevat een ongeldige SLO-waarde <br />2. Of er is geen overeenkomende resourceklasse voor de bewerking CCI op de tabel. <br />Op DW100, bijvoorbeeld, is de hoogste geheugentoekenning beschikbaar 400 MB, en als tabelschema is breed genoeg om te passeren van de vereiste van 400 MB.
      
### <a name="usage-example"></a>Voorbeeld van gebruik:
Syntaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU: Ofwel een NULL-parameter moet worden opgehaald van de huidige DWU uit de DW-database of geef een ondersteunde DWU in de vorm van 'DW100' opgeven
2. @SCHEMA_NAME: Geef een schemanaam van de tabel
3. @TABLE_NAME: Geef een tabelnaam op van het belang van de

Voorbeelden voor het uitvoeren van deze opgeslagen procedure:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```
> [!NOTE]
> De waarden die zijn gedefinieerd in deze versie van de opgeslagen procedure zijn alleen van toepassing op Gen1.
>
>

De volgende instructie maakt Tabel1 die wordt gebruikt in de voorgaande voorbeelden.
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
Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Zie voor meer informatie over hoe grotere resourceklassen geclusterde columnstore-index kwaliteit verbeteren kan, [geheugenoptimalisaties voor columnstore-compressie](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
