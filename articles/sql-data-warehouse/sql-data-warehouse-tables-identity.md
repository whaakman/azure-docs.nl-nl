---
title: IDENTITEIT gebruiken voor het maken van sleutels vervangende - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en voorbeelden voor het gebruik van de IDENTITY-eigenschap vervangende om sleutels te maken van tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ab028705f5af7c37017d2e697240b7d3436f5f71
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526981"
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>Met behulp van de identiteit vervangende om sleutels te maken in Azure SQL Data Warehouse
Aanbevelingen en voorbeelden voor het gebruik van de IDENTITY-eigenschap vervangende om sleutels te maken van tabellen in Azure SQL Data Warehouse.

## <a name="what-is-a-surrogate-key"></a>Wat is de sleutel van een vervangende?
Een vervangende sleutel in een tabel is een kolom met een unieke identificatie voor elke rij. De sleutel wordt niet afgeleid van gegevens in de tabel. Gegevens modelers zoals vervangende om sleutels te maken op de tabellen bij het ontwerpen van datawarehouse-modellen. U kunt de IDENTITY-eigenschap te realiseren eenvoudig en effectief zonder laadprestaties gebruiken.  

## <a name="creating-a-table-with-an-identity-column"></a>Maakt een tabel met een id-kolom
De IDENTITY-eigenschap is ontworpen voor zonder laadprestaties uitbreiden in de distributies in het datawarehouse. Daarom is de implementatie van de identiteit is gericht op deze doelstellingen te bereiken. 

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

Deze rest van deze sectie worden de nuances van de implementatie voor deze uitgebreider inzicht gemarkeerd.  

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

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Expliciet invoegen waarden in een identiteitskolom 
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

## <a name="loading-data"></a>Gegevens laden

De aanwezigheid van de IDENTITY-eigenschap heeft een aantal gevolgen toe aan uw code laden van gegevens. Deze sectie worden enkele basispatronen die voor het laden van gegevens in tabellen met behulp van de identiteit. 

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
> Het is niet mogelijk te maken tabel AS SELEC gebruiken is momenteel bij het laden van gegevens naar een tabel met een identiteitskolom.
> 

Zie voor meer informatie over het laden van gegevens, [ontwerpen extraheren, laden en transformeren (ELT) voor Azure SQL Data Warehouse](design-elt-data-loading.md) en [bij het laden van aanbevolen procedures](guidance-for-loading-data.md).


## <a name="system-views"></a>Systeemweergaven
U kunt de [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) catalogusweergave om een kolom waarvoor de eigenschap identiteit te identificeren.

Als u het databaseschema beter te begrijpen, dit voorbeeld ziet u het integreren van sys.identity_column' met andere weergaven van de catalogus system:

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
De IDENTITY-eigenschap kan niet worden gebruikt:
- Wanneer het gegevenstype van kolom is geen INT of BIGINT
- Als de kolom is ook de distributiesleutel
- Als de tabel is een externe tabel 

De volgende gerelateerde functies worden niet ondersteund in SQL Data Warehouse:

- [IDENTITY()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [SCOPE_IDENTITY](/sql/t-sql/functions/scope-identity-transact-sql)
- [IDENT_CURRENT](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)
- [DBCC CHECK_IDENT()](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)

## <a name="common-tasks"></a>Algemene taken

Deze sectie bevat enkele veelvoorkomende taken uitvoeren wanneer u met de id-kolommen werkt kunt u voorbeeldcode. 

Kolom C1 is de identiteit in de volgende taken.
 
 
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

* Zie voor meer informatie over het ontwikkelen van tabellen, [tabel overview] [Overview].  

