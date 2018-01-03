---
title: Vervangend sleutels maken met behulp van de identiteit | Microsoft Docs
description: Informatie over het gebruik van identiteit vervangende om sleutels te maken voor tabellen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e10b58743fad5f7c2c4f00b51f06d4ec9bcb6768
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Vervangend sleutels maken met behulp van identiteit
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Gegevenstypen][Data Types]
> * [Distribueren][Distribute]
> * [Index][Index]
> * [Partitie][Partition]
> * [Statistieken][Statistics]
> * [Tijdelijke][Temporary]
> * [Identiteit][Identity]
> 
> 

Veel gegevens modelers zoals vervangende om sleutels te maken op de tabellen bij het ontwerpen van datawarehouse-modellen. U kunt de IDENTITY-eigenschap te realiseren eenvoudig en effectief zonder laadprestaties gebruiken. 

## <a name="get-started-with-identity"></a>Aan de slag met de identiteit
Een tabel kunt u definiëren als de IDENTITY-eigenschap hebben wanneer u eerst de tabel maken met behulp van de syntaxis die vergelijkbaar is met de volgende instructie:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Vervolgens kunt u `INSERT..SELECT` voor het vullen van de tabel.

## <a name="behavior"></a>Gedrag
De IDENTITY-eigenschap is ontworpen voor zonder laadprestaties uitbreiden in de distributies in het datawarehouse. Daarom is de implementatie van de identiteit is gericht op deze doelstellingen te bereiken. Deze sectie worden de nuances van de implementatie kunt u ze beter te begrijpen.  

### <a name="allocation-of-values"></a>Toewijzing van waarden
De volgorde waarin de vervangende waarden worden toegewezen, dat overeenkomt met het gedrag van SQL Server en Azure SQL Database niet worden gegarandeerd dat de IDENTITY-eigenschap. Echter, in Azure SQL Data Warehouse het ontbreken van een garantie groter is. 

Het volgende voorbeeld wordt een afbeelding:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

In het voorgaande voorbeeld is twee rijen bevindt zich in distributie 1. De eerste rij de vervangende waarde heeft van 1 in kolom `C1`, en de tweede rij heeft de vervangende waarde 61. Van deze waarden zijn gegenereerd door de IDENTITY-eigenschap. De toewijzing van de waarden is echter niet aaneengesloten. Dit gedrag is standaard.

### <a name="skewed-data"></a>Vertekende gegevens 
Het bereik van waarden voor het gegevenstype gelijkmatig verdeeld over de distributies. Als een gedistribueerde tabel leidt dit tot slechtere van vertekende gegevens, kan klikt u vervolgens het bereik van waarden die beschikbaar zijn voor het gegevenstype worden uitgeput voortijdig. Bijvoorbeeld, als alle gegevens eindigt in een enkel distributiepunt, heeft vervolgens effectief de tabel toegang tot slechts één zestigste van de waarden van het gegevenstype. Daarom moet de IDENTITY-eigenschap is beperkt tot `INT` en `BIGINT` alleen gegevenstypen.

### <a name="selectinto"></a>SELECTEREN... IN
Wanneer een bestaande id-kolom is geselecteerd in een nieuwe tabel, neemt de nieuwe kolom die de IDENTITY-eigenschap, tenzij een van de volgende voorwaarden voldaan wordt:
- De SELECT-instructie bevat een join.
- Meerdere SELECT-instructies worden gekoppeld met behulp van de SAMENVOEGING.
- De identiteitskolom staat meer dan één keer in de selectielijst.
- De identiteitskolom maakt deel uit van een expressie.
    
Als een van deze voorwaarden voldaan wordt, wordt de kolom geen NULL in plaats van de identiteitseigenschap overneemt gemaakt.

### <a name="create-table-as-select"></a>TABLE AS SELECT MAKEN
Maak tabel AS selecteren (CTAS) volgt hetzelfde gedrag voor SQL Server die wordt beschreven voor SELECT... IN. Echter, u een IDENTITY-eigenschap niet opgeven in de definitie van de kolom van de `CREATE TABLE` deel uit van de instructie. Ook in functie IDENTITY niet gebruiken de `SELECT` deel uit van de CTAS. Om te voorzien van een tabel, moet u gebruiken `CREATE TABLE` voor het definiëren van de tabel die wordt gevolgd door `INSERT..SELECT` het vullen.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Expliciet waarden invoegen in een identiteitskolom 
Biedt ondersteuning voor SQL Data Warehouse `SET IDENTITY_INSERT <your table> ON|OFF` syntaxis. Deze syntaxis kunt u expliciet waarden invoegen in de identiteitskolom.

Veel gegevens modelers zoals met vooraf gedefinieerde negatieve waarden voor bepaalde rijen in hun dimensies. Een voorbeeld is de -1 of 'onbekend lid' rij. 

Het volgende script laat zien hoe deze rij expliciet toevoegen met behulp van IDENTITY_INSERT instellen:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Gegevens laden in een tabel met de identiteit

De aanwezigheid van de IDENTITY-eigenschap heeft een aantal gevolgen toe aan uw code laden van gegevens. Deze sectie worden enkele basispatronen die voor het laden van gegevens in tabellen met behulp van de identiteit. 

### <a name="load-data-with-polybase"></a>Gegevens laden met PolyBase
Gegevens laden in een tabel en een vervangend sleutel genereren met behulp van de identiteit, de tabel maken en vervolgens met INSERT... Selecteer of INSERT... WAARDEN voor het uitvoeren van de belasting.

Het volgende voorbeeld illustreert het basispatroon:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Het is niet mogelijk om te gebruiken `CREATE TABLE AS SELECT` momenteel bij het laden van gegevens naar een tabel met een identiteitskolom.
> 

Zie de volgende artikelen voor meer informatie over het laden van gegevens met behulp van het hulpprogramma voor bulksgewijs kopiëren (BCP)-programma's:

- [Laden met PolyBase][]
- [Aanbevolen procedures van PolyBase][]

### <a name="load-data-with-bcp"></a>Gegevens laden met BCP
BCP is een opdrachtregelprogramma waarmee u gebruiken kunt om gegevens te laden in SQL Data Warehouse. Een van de parameters (-E) bepaalt het gedrag van BCP bij het laden van gegevens naar een tabel met een identiteitskolom. 

Wanneer -E wordt opgegeven, worden de waarden die zijn ondergebracht in het bestand voor invoer voor de kolom met de identiteit behouden. Als E - *niet* opgegeven, worden de waarden in deze kolom worden genegeerd. Als de identiteitskolom niet opgenomen is, is de gegevens die normaal werken geladen. De waarden worden gegenereerd volgens het beleid verhoging en seed van de eigenschap.

Zie voor meer informatie over het laden van gegevens met behulp van BCP, de volgende artikelen:

- [Laden met BCP][]
- [BCP in MSDN][]

## <a name="catalog-views"></a>Catalogusweergaven
SQL Data Warehouse ondersteunt de `sys.identity_columns` catalogusweergave. Deze weergave kan worden gebruikt om een kolom waarvoor de eigenschap identiteit te identificeren.

Als u het databaseschema beter te begrijpen, in dit voorbeeld laat zien hoe integreren `sys.identity_columns` met andere weergaven van de catalogus system:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Beperkingen
De IDENTITY-eigenschap kan niet worden gebruikt in de volgende scenario's:
- Waar het gegevenstype van kolom is geen INT of BIGINT
- De kolom is waar ook de distributiesleutel
- Waar de tabel is een externe tabel 

De volgende gerelateerde functies worden niet ondersteund in SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Taken

Deze sectie bevat enkele veelvoorkomende taken uitvoeren wanneer u met de id-kolommen werkt kunt u voorbeeldcode.

> [!NOTE] 
> Kolom C1 is de identiteit in de volgende taken.
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>De hoogste toegewezen waarde vinden voor een tabel
Gebruik de `MAX()` functie om te bepalen van de hoogste waarde die is toegewezen aan een gedistribueerde tabel:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>De seed- en increment vinden voor de IDENTITY-eigenschap
U kunt de catalogusweergaven gebruiken voor het detecteren van de identiteit increment- en seed configuratiewaarden voor een tabel met behulp van de volgende query: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het ontwikkelen van tabellen, [tabel overzicht][Overview], [tabel gegevenstypen][Data Types], [distribueren van een tabel][Distribute], [indexeren van een tabel][Index], [partitie van een tabel][Partition], en [tijdelijke tabellen][Temporary]. 
* Zie voor meer informatie over best practices [aanbevolen procedures voor SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[Laden met bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Laden met PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[Aanbevolen procedures van PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP in MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
