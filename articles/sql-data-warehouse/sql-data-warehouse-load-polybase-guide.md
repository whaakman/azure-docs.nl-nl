---
title: Handleiding voor het gebruik van PolyBase in SQL Data Warehouse | Microsoft Docs
description: Richtlijnen en aanbevelingen voor het gebruik van PolyBase in SQL Data Warehouse-scenario's.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 9/13/2017
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: e8ae0eb96200c167a8758df4ce20b51452cc59a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Handleiding voor het gebruik van PolyBase in SQL Data Warehouse
Deze handleiding biedt praktische informatie voor het gebruik van PolyBase in SQL Data Warehouse.

Om te beginnen, Zie de [gegevens laden met PolyBase] [ Load data with PolyBase] zelfstudie.

## <a name="rotating-storage-keys"></a>Opslagsleutels draaien
Van tijd tot tijd wilt u de toegangssleutel wijzigen in de blob-opslag om veiligheidsredenen.

De meest elegante manier om deze taak uitvoeren is een proces genaamd 'roteren van de sleutels' volgen. U mogelijk opgevallen dat er twee opslagsleutels voor blob storage-account. Dit is zodat kunt u overstappen

Roteren van uw sleutels van Azure storage-account is een proces eenvoudige drie stappen

1. Tweede-scoped databasereferentie op basis van de toegangssleutel voor secundaire opslag maken
2. Tweede externe gegevensbron gebaseerd op deze nieuwe referentie maken
3. Verwijderen en de externe tabel(len) die verwijst naar de externe gegevensbron maken

Wanneer u hebt gemigreerd alle externe tabellen met de externe gegevensbron en vervolgens kunt u het opschonen taken uitvoeren:

1. Eerste externe gegevensbron verwijderen
2. Uitgevallen eerste database-scoped referentie op basis van de toegangssleutel voor primaire opslag
3. Meld u aan bij Azure en gereed voor de volgende keer dat de primaire toegangssleutel genereren



## <a name="load-data-with-external-tables"></a>Gegevens laden met externe tabellen
In dit voorbeeld worden gegevens geladen uit Azure blob storage met SQL Data Warehouse-database.

Gegevens rechtstreeks op te slaan, verwijdert de gegevens overdrachtstijd voor query's. Opslaan van gegevens met een columnstore-index verbetert de prestaties van query's voor analyse van query's door maximaal 10 x.

In dit voorbeeld gebruikt de instructie CREATE TABLE AS SELECT om gegevens te laden. De nieuwe tabel neemt over van de kolommen met de naam in de query. De gegevenstypen van die kolommen neemt over van de definitie van de externe tabel.

CREATE TABLE AS SELECT is een zeer zodat Transact-SQL-instructie die de gegevens in de rekenknooppunten van uw SQL Data Warehouse parallel worden geladen.  Het oorspronkelijk is ontwikkeld voor de engine massively parallelle processing (MPP) in Analytics Platform System en bevindt zich nu in SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Zie [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

> [!NOTE]
> Een werklast met behulp van een externe tabel mislukken met de fout *'Query afgebroken--de maximale afwijzen drempelwaarde werd bereikt tijdens het lezen van een externe bron'*. Dit geeft aan dat de externe gegevens bevat *dirty* records. Een record wordt beschouwd als 'vervuild' als de werkelijke gegevens typen of aantal kolommen komen niet overeen met de kolomdefinities van de externe tabel of als de gegevens komt niet met de externe bestandsindeling overeen. U herstelt dit door ervoor te zorgen dat uw externe tabel en het externe bestand indeling definities juist zijn en de externe gegevens aan deze definities voldoet. Als een subset van externe gegevensrecords zijn gewijzigd, kunt u deze records voor uw query's met behulp van de opties reject in externe tabel-DDL maken afwijzen.
> 
> 


## <a name="create-statistics-on-newly-loaded-data"></a>Statistieken maken voor zojuist geladen gegevens
Azure SQL Data Warehouse bevat nog geen functionaliteit voor het automatisch maken of bijwerken van statistieken.  Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd.  Zie het onderwerp [Statistieken][Statistics] in de groep onderwerpen voor ontwikkelaars voor gedetailleerde uitleg van statistieken.  Hieronder vindt u een kort voorbeeld van het maken van statistieken over de tabellen die zijn geladen in dit voorbeeld.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-with-external-tables"></a>Gegevens exporteren met de externe tabellen
Deze sectie wordt beschreven hoe gegevens uit SQL Data Warehouse exporteren naar Azure blob storage met externe tabellen. In dit voorbeeld maakt gebruik van CREATE externe TABLE AS SELECT die een zeer zodat Transact-SQL-instructie de gegevens parallel te exporteren uit de rekenknooppunten.

Het volgende voorbeeld wordt een externe tabel Weblogs2014 met behulp van de kolomdefinities en gegevens van dbo. Weblogboeken tabel. Definitie van de externe tabel is opgeslagen in SQL Data Warehouse en de resultaten van de SELECT-instructie worden geëxporteerd naar de map '/ archiveren/log2014 /' onder de blob-container die is opgegeven door de gegevensbron. De gegevens worden in de opgegeven tekst-indeling geëxporteerd.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Het laden van gebruikers isoleren
Het is vaak een moet zijn van meerdere gebruikers die gegevens in een SQL DW laden kunnen. Omdat de [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] vereist machtigingen voor beheer van de database, verschijnt met meerdere gebruikers met toegangsbeheer via alle schema's. Als u wilt dit beperken, kunt u de instructie BESTURINGSELEMENT weigeren.

Voorbeeld: Overweeg database schema's schema_A voor afdeling A en schema_B voor afdeling B laten database gebruikers user_A en user_B worden gebruikers voor PolyBase laden in afdeling A en B, respectievelijk. Ze hebben gekregen BESTURINGSELEMENT databasemachtigingen.
De auteurs van A en B nu de schemavergrendeling omlaag hun schema's met behulp van weigeren:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 Met dit moeten user_A en user_B nu worden vergrendeld uit de andere afdeling schema.
 
## <a name="polybase-performance-optimizations"></a>PolyBase-prestatieoptimalisaties
Om te zorgen voor optimale prestaties met PolyBase laden we raden het volgende:
- Splits grote gecomprimeerde bestanden in kleinere gecomprimeerde bestanden. De vandaag ondersteund compressietypen zijn niet splittable. Als gevolg hiervan zullen prestaties worden beïnvloed door het laden van een enkele groot bestand.
- Laden in een round_robin, heap faseringstabel voor snelheid van de snelste laden. Dit is de meest efficiënte manier voor het verplaatsen van de gegevens van de opslaglaag naar het datawarehouse.
- Alle bestandsindelingen hebben verschillende prestatiekenmerken. Voor de snelste belasting gescheiden gebruik gecomprimeerd tekstbestanden. Het verschil tussen UTF-8 en UTF-16-prestaties is minimaal.
- Dezelfde plaatsen uw storage-laag en het datawarehouse naar Latentie minimaliseren
- Uw datawarehouse opschalen als u verwacht de taak van een grote laden dat.

## <a name="polybase-limitations"></a>PolyBase-beperkingen
PolyBase in SQL DW heeft de volgende beperkingen die in aanmerking worden genomen moeten bij het ontwerpen van een taak laden:
- Een enkele rij kan niet groter zijn dan 1.000.000 bytes zijn. Dit geldt ongeacht het tabelschema gedefinieerd.
- Uitvoer van de gegevens in een bestandsindeling ORC van SQL Server of Azure SQL Data Warehouse zware tekstkolommen kan wanneer worden beperkt tot slechts 50 kolommen vanwege java buiten geheugenfouten. U kunt dit probleem omzeilen slechts een subset van de kolommen te exporteren.





## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het verplaatsen van gegevens naar SQL Data Warehouse, de [Migratieoverzicht][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
