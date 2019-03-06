---
title: Azure SQL Data Warehouse opmerkingen bij de Release mei 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 45a391f45d11d968818bafc97a705411a133b273
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57430846"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Mei 2018 
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in mei 2018. 

## <a name="gen-2-instances"></a>Gen 2 exemplaren
![ALT](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) laag van Azure SQL Data Warehouse Compute geoptimaliseerd Gen2 stelt nieuwe prestatienormen voor cloud-gegevensopslag. Klanten nu krijgen tot vijf keer betere prestaties van query's, vier keer meer gelijktijdigheid en vijf keer hoger verwerkingsvermogen vergeleken met de huidige generatie. Deze kan nu voorzien 128 gelijktijdige query's uit een cluster, de hoogste van elke service gegevensopslag in de cloud.

Zie de [Turbocharge cloud analytics met Azure SQL Data Warehouse](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) blogbericht van Rohan Kumar, Corporate Vice President, Azure Data.

## <a name="auto-statistics"></a>Automatische statistieken
Statistieken zijn essentieel voor het genereren van de query-plan in moderne kostengebaseerd-optimalisatieprogramma's, zoals de engine van SQL Data Warehouse te optimaliseren. Wanneer alle query's die vooraf bekend zijn, is waarmee wordt bepaald wat statistieken objecten moeten worden gemaakt een haalbare taak. Wanneer het systeem wordt geconfronteerd met ad hoc en willekeurige query's, dit is normaal dat voor de datawarehousing-workloads, systeembeheerders mogelijk moeilijk te voorspellen welke statistieken moeten echter worden gemaakt die moeten leiden tot uitvoering van potentieel suboptimale queryplannen en meer reactietijden van de query. Er is een manier om dit probleem verhelpen statistieken om objecten te maken voor alle tabelkolommen vooraf. Maar dit proces wordt geleverd met een boete omdat statistieken objecten worden onderhouden tijdens tabel tijdens het laden moeten, waardoor langere tijden voor het laden.

SQL Data Warehouse biedt nu ondersteuning voor automatisch maken van statistieken objecten zorgt voor meer flexibiliteit, productiviteit en gebruiksgemak voor systeembeheerders en ontwikkelaars, en ervoor te zorgen dat het systeem blijft kwaliteit uitvoering plannen en de beste bieden reactietijden.

Als u wilt in- of uitschakelen van het maken van automatische statistieken in SQL Data Warehouse, voert u de volgende instructie uit:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Als een best practice en richtlijnen, is het raadzaam instellen `AUTO_CREATE_STATISTICS` optie naar `ON`.

> [!NOTE]
> Het maken van automatische statistiek is *standaard ingeschakeld* voor alle nieuwe datawarehouses.
>  

Zie de [ALTER DATABASE SET Options](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) artikel voor meer informatie.

## <a name="rejected-row-support"></a>Ondersteuning voor rij geweigerd
Klanten gebruiken vaak [PolyBase (externe tabellen) om gegevens te laden](design-elt-data-loading.md) in SQL Data Warehouse vanwege de hoge prestaties, parallelle aard van het laden van gegevens. PolyBase is het standaardmodel laden bij het laden van gegevens via [Azure Data Factory](http://azure.com/adf) ook. 

SQL Data Warehouse voegt de mogelijkheid voor het definiëren van een geweigerde rij locatie via de `REJECTED_ROW_LOCATION` parameter met de [CREATE EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) instructie. Na het uitvoeren van een [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) rijen met kunnen niet worden geladen vanuit de externe tabel worden opgeslagen in een bestand in de buurt van de bron voor verder onderzoek. 

Zie de [laden met vertrouwen met SQL Data Warehouse PolyBase geweigerd rij locatie](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) blog voor meer informatie over het gedrag van de rij geweigerd.

Het volgende voorbeeld ziet u de nieuwe syntaxis voor het opgeven van geweigerde rijen.

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

## <a name="alter-view"></a>ALTER VIEW
[ALTER VIEW](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) kan een gebruiker een eerder gemaakte weergave wijzigen zonder dat u de weergave hoeft te verwijderen/maken en machtigingen toepassen. 

Het volgende voorbeeld wijzigt een eerder gemaakte weergave.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

## <a name="concatws"></a>CONCAT_WS
De [CONCAT_WS()](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) functie retourneert een tekenreeks die voortvloeien uit de samenvoeging van twee of meer waarden in een end-to-end-manier. De samengevoegde waarden worden gescheiden met het scheidingsteken dat is opgegeven in het eerste argument. De `CONCAT_WS` functie is handig voor het genereren van uitvoer Comma-Separated waarde (CSV).

Het volgende voorbeeld ziet een set waarden met een komma int samenvoegen.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
De instructie retourneert het volgende resultaat:
```
result
---------
1,2,3
```
Het volgende voorbeeld ziet een set met gemengde gegevenstypen met een komma samenvoegen.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
De instructie retourneert het volgende resultaat:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```

## <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
De [sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) systeem opgeslagen procedure informatie over de gegevens die worden ondersteund door de huidige omgeving retourneert. Dit wordt doorgaans gebruikt door hulpprogramma's verbinding maken via ODBC-verbindingen voor onderzoek naar het type van gegevens.

Het volgende voorbeeld haalt de details voor alle gegevens die worden ondersteund door SQL Data Warehouse.

```sql
EXEC sp_datatype_info
```

## <a name="select-into-with-order-by-behavior-change"></a>Selecteer INTO met ORDER BY-gedrag wijzigen
SQL Data Warehouse worden nu geblokkeerd `SELECT INTO` query's met een `ORDER BY` component. Met deze bewerking zou eerder mislukt door eerst het bestellen van de gegevens in het geheugen en klik vervolgens in de doeltabel opnieuw ordenen van de gegevens zodat deze overeenkomt met de tabelvorm invoegen.

### <a name="previous-behavior"></a>Gedrag van het vorige
De volgende instructie zou slagen met extra verwerking overhead.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

### <a name="current-behavior"></a>Gedrag van het huidige
De volgende instructie genereert een fout die aangeeft de `ORDER BY` component wordt niet ondersteund een `SELECT INTO` instructie.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
De instructie fout geretourneerd:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

## <a name="set-parseonly-on-query-status-behavior-change"></a>PARSEONLY ingesteld op de status van de (gedrag wijzigen)
Met behulp van de `SET PARSEONLY ON` syntaxis kan gebruikers de SQL Data Warehouse-engine de syntaxis van elke T-SQL-instructie controleren en eventuele foutberichten geretourneerd zonder te compileren of uitvoeren van de instructie. Eerder in de [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) systeemweergave, de status van deze instructies blijft in de `Running` staat. De `sys.dm_pdw_exec_requests` weergave wordt nu de status retourneren `Complete`.

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
