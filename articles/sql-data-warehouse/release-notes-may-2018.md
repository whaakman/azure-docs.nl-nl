---
title: Azure SQL Data Warehouse releaseopmerkingen mei 2018 | Microsoft Docs
description: Releaseopmerkingen voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 843621a8f6e08b2b51d4b7abd05d0ae6c3393fe1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726030"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse (mei 2018)?
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. In dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in mei 2018. 

## <a name="gen-2-instances"></a>Gen 2 exemplaren
![ALT](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) Azure SQL Data Warehouse Compute geoptimaliseerd Gen2 laag stelt nieuwe prestatienormen voor cloud datawarehousing. Klanten ontvangen vervolgens maximaal vijf keer betere prestaties van query's, vier keer meer gelijktijdigheid van taken en vijf keer hoger rekenkracht vergeleken met de huidige generatie. Deze kan nu 128 gelijktijdige query's van één cluster, de hoogste waarde van een cloudservice voor gegevens magazijnbeheer dienen.

Zie de [Turbocharge cloud analytics met Azure SQL Data Warehouse](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) blog aankondiging van Rohan Kumar, zakelijke Vice-directeur Azure-gegevens.

## <a name="features"></a>Functies

### <a name="auto-statistics"></a>Automatische statistieken
Statistieken zijn essentieel voor het optimaliseren van queryplan genereren in moderl kosten gebaseerde zowel zoals de engine in SQL Data Warehouse. Alle query's zijn vooraf bekend, is het bepalen van statistieken objecten moeten worden gemaakt een haalbare taak. Echter, wanneer het systeem wordt geconfronteerd met ad-hoc en willekeurige query's die typisch zijn voor de datawarehouse-werkbelastingen, systeembeheerders mogelijk is het moeilijk te voorspellen statistieken moeten ertoe leidt dat mogelijk suboptimale queryplannen voor uitvoering worden gemaakt en langere tijden voor query-antwoord. Er is een manier om dit probleem verhelpen statistieken om objecten te maken voor alle tabelkolommen van tevoren. Echter dat proces wordt geleverd met een zoals statistieken objecten worden gehandhaafd tijdens tabel laadproces moeten, waardoor langere tijden voor laden.

SQL Data Warehouse biedt nu ondersteuning voor het automatisch maken van statistieken-objecten bieden meer flexibiliteit, productiviteit en eenvoudig te gebruiken voor systeembeheerders en ontwikkelaars, terwijl dat het systeem blijft bieden uitvoering plannen en beste reactietijden.

Als u wilt in- of uitschakelen van automatische statistieken maken in SQL Data Warehouse, voer de volgende instructie:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Als een best practice en richtlijnen aangeraden instelling `AUTO_CREATE_STATISTICS` optie naar `ON`.

> [!NOTE]
> Het maken van automatische statistiek is *standaard ingeschakeld* voor alle nieuwe datawarehouses.
>  

Zie de [ALTER DATABASE SET Options](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) artikel voor meer informatie.

### <a name="rejected-row-support"></a>Afgewezen rij-ondersteuning
Klanten gebruiken vaak [PolyBase (externe tabellen) om gegevens te laden](design-elt-data-loading.md) in SQL Data Warehouse vanwege de hoge prestaties, parallelle aard van het laden van gegevens. PolyBase is het standaard laden-model bij het laden van gegevens via [Azure Data Factory](http://azure.com/adf) ook. 

SQL Data Warehouse voegt de mogelijkheid voor het definiëren van een locatie geweigerde rij via de `REJECTED_ROW_LOCATION` parameter met de [externe tabel maken](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) instructie. Na de uitvoering van een [maken tabel AS selecteren (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) rijen die niet kan worden geladen vanaf de externe tabel wordt opgeslagen in een bestand in de buurt van de bron voor verder onderzoek. 

Zie de [probleemloos laden met SQL Data Warehouse PolyBase afgewezen rij locatie](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) blog voor meer informatie over het gedrag van de rij geweigerd.

Het volgende voorbeeld ziet u de nieuwe syntaxis voor het opgeven van rijen geweigerd.

```sql
CREATE EXTERNAL TABLE [dbo].[Reject_Example]
(
    ...
)
WITH
(
    ...
    ,REJECTED_ROW_LOCATION=‘/Reject_Directory'
)
```

### <a name="alter-view"></a>WEERGAVE WIJZIGEN
[ALTER VIEW](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) kan een gebruiker een eerder gemaakte weergave wijzigen zonder te verwijderen/maken met de weergave en het opnieuw. 

Het volgende voorbeeld wijzigt een eerder gemaakte weergave.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

### <a name="concatws"></a>CONCAT_WS
[De CONCAT_WS()](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) functie retourneert een tekenreeks die voortvloeien uit de samenvoeging van twee of meer waarden in een end-to-end-manier. De samengevoegde waarden worden gescheiden met het scheidingsteken dat is opgegeven in het eerste argument. De `CONCAT_WS` functie is handig voor het genereren van uitvoer Comma-Separated waarde (CSV).

Het volgende voorbeeld ziet een set waarden met een komma int cookievalidatie.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
De instructie retourneert het resultaat van de volgende:
```
result
---------
1,2,3
```
Het volgende voorbeeld ziet een reeks gemengde gegevenstypen met een komma cookievalidatie.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
De instructie retourneert het resultaat van de volgende:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```
### <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
De [sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) systeem opgeslagen procedure informatie over de gegevens die worden ondersteund door de huidige omgeving retourneert. Dit wordt doorgaans gebruikt door hulpprogramma's die verbinding maken via een ODBC-verbindingen voor onderzoek naar het type van gegevens.

Het volgende voorbeeld worden details voor alle gegevens die worden ondersteund door SQL Data Warehouse opgehaald.

```sql
EXEC sp_datatype_info
```

## <a name="behavior-changes"></a>Gedragswijzigingen
### <a name="select-into-with-order-by"></a>SELECT INTO met ORDER BY
SQL Data Warehouse wordt nu geblokkeerd `SELECT INTO` query's met een `ORDER BY` component. Deze bewerking zou eerder mislukt door de eerste ordening van de gegevens in het geheugen en klik vervolgens in de doeltabel volgorde van de gegevens zodat deze overeenkomen met de tabelvorm invoegen.

#### <a name="previous-behavior"></a>Vorige gedrag
De volgende instructie zou slagen met extra verwerking overhead.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

#### <a name="current-behavior"></a>Huidige gedrag
De volgende instructie genereert een fout die aangeeft de `ORDER BY` component wordt niet ondersteund een `SELECT INTO` instructie.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
De instructie fout geretourneerd:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

### <a name="set-parseonly-on-query-status"></a>PARSEONLY ingesteld op de status opvragen
Met behulp van de `SET PARSEONLY ON` syntaxis kan een gebruiker beschikken over de SQL Data Warehouse-engine onderzoekt de syntaxis van elke T-SQL-instructie en eventuele foutberichten geretourneerd zonder te compileren of uitvoeren van de instructie. In de [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) systeemweergave, de status van deze instructies blijft de `Running` staat. De `sys.dm_pdw_exec_requests` weergave wordt nu de status retourneren `Complete`.