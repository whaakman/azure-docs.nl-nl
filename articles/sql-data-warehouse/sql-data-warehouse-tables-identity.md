---
title: Vervangend sleutels - Azure SQL Data Warehouse maken met behulp van identiteit | Microsoft Docs
description: Aanbevelingen en voorbeelden voor het gebruik van de eigenschap id voor het maken van vervangend sleutels in tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: abe245e6174cb947e78252941c71ce6857b77f77
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306788"
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>Vervangend sleutels in Azure SQL Data Warehouse maken met behulp van identiteit
Aanbevelingen en voorbeelden voor het gebruik van de eigenschap id voor het maken van vervangend sleutels in tabellen in Azure SQL Data Warehouse.

## <a name="what-is-a-surrogate-key"></a>Wat is een surrogaatsleutel?
Een surrogaatsleutel in een tabel is een kolom met een unieke id voor elke rij. De sleutel is niet gegenereerd op basis van gegevens in de tabel. Gegevensmodellen zoals surrogate om sleutels te maken voor de tabellen bij het ontwerpen van datawarehouse-modellen. U kunt de identiteitseigenschap te realiseren eenvoudige en doeltreffende wijze zonder laadprestaties gebruiken.  

## <a name="creating-a-table-with-an-identity-column"></a>Het maken van een tabel met een id-kolom
De id-eigenschap is ontworpen om uit te schalen in de distributies in het datawarehouse zonder sneller kan worden geladen. De uitvoering van de identiteit is daarom gericht op deze doelstellingen te bereiken. 

U kunt een tabel definiëren die de identiteitseigenschap wanneer u eerst de tabel maakt met behulp van de syntaxis die vergelijkbaar is met de volgende instructie:

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

Dit rest van deze sectie ziet u de aspecten van de implementatie kunt u beter te begrijpen.  

### <a name="allocation-of-values"></a>Toewijzing van waarden
De volgorde waarin de vervangende waarden zijn toegewezen, dat overeenkomt met het gedrag van SQL Server en Azure SQL Database biedt geen garantie voor de identiteitseigenschap. Echter, in Azure SQL Data Warehouse, de afwezigheid van een groter is. 

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

In het voorgaande voorbeeld is twee rijen bevindt zich in de distributie van 1. De eerste rij heeft de vervangende waarde van 1 in kolom `C1`, en de tweede rij heeft de vervangende waarde 61. Beide van deze waarden zijn gegenereerd door de identiteitseigenschap. De toewijzing van de waarden is echter niet aaneengesloten. Dit gedrag is standaard.

### <a name="skewed-data"></a>Asymmetrische gegevens 
Het bereik van waarden voor het gegevenstype gelijkmatig verdeeld over de distributies. Als een distributietabel te lijden heeft ongelijke gegevens, klikt u vervolgens het bereik van waarden beschikbaar zijn voor het gegevenstype kunt uitgeput raken voortijdig gestopt. Bijvoorbeeld, als alle gegevens eindigt om in een enkel distributiepunt, heeft klikt u vervolgens effectief in de tabel toegang tot slechts één zestigste van de waarden van het gegevenstype. Om deze reden de identiteitseigenschap is beperkt tot `INT` en `BIGINT` gegevenstypen alleen.

### <a name="selectinto"></a>SELECTEREN... IN
Wanneer een bestaande id-kolom is geselecteerd in een nieuwe tabel, neemt de nieuwe kolom die de identiteitseigenschap, tenzij een van de volgende voorwaarden voldaan wordt:
- De SELECT-instructie bevat een join.
- Meerdere SELECT-instructies worden gekoppeld met behulp van de SAMENVOEGING.
- De id-kolom is meer dan één keer in de selectielijst worden weergegeven.
- De id-kolom is onderdeel van een expressie.
    
Als een van deze voorwaarden voldaan wordt, wordt de kolom niet null zijn in plaats van de identiteitseigenschap overneemt gemaakt.

### <a name="create-table-as-select"></a>TABLE AS SELECT MAKEN
CREATE TABLE AS SELECT (CTAS) volgt hetzelfde gedrag voor SQL Server die wordt beschreven in selecteren... IN. U kan niet een eigenschap echter opgeven in de definitie van de kolom van de `CREATE TABLE` deel uitmaakt van de instructie. Ook de functie IDENTITY niet gebruiken de `SELECT` deel uitmaakt van de CTAS. Om in te vullen in een tabel, moet u gebruiken `CREATE TABLE` voor het definiëren van de tabel die wordt gevolgd door `INSERT..SELECT` voor het vullen van het.

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Expliciet invoegen waarden in een id-kolom 
SQL Data Warehouse ondersteunt `SET IDENTITY_INSERT <your table> ON|OFF` syntaxis. Deze syntaxis kunt u expliciet waarden invoegen in de identiteitskolom.

Veel gegevensmodellen zoals met vooraf gedefinieerde negatieve waarden voor bepaalde rijen in hun dimensies. Een voorbeeld is de -1 of 'onbekende lid' rij. 

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

De aanwezigheid van de eigenschap identiteit heeft enkele gevolgen voor uw code laden van gegevens. In deze sectie worden enkele eenvoudige patronen voor het laden van gegevens in tabellen met behulp van identiteit. 

Voor het laden van gegevens in een tabel en een surrogaatsleutel genereren met behulp van identiteit, maak de tabel en gebruik vervolgens invoegen... Selecteer of invoegen... De waarden uit te voeren van de belasting.

Het volgende voorbeeld wordt het algemene patroon:
 
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
> Het is niet mogelijk te maken van tabel AS SELEC gebruiken ' momenteel bij het laden van gegevens naar een tabel met een id-kolom.
> 

Zie voor meer informatie over het laden van gegevens, [ontwerpen uitpakken, Load en Transform (ELT) voor Azure SQL Data Warehouse](design-elt-data-loading.md) en [het laden van aanbevolen procedures](guidance-for-loading-data.md).


## <a name="system-views"></a>Systeemweergaven
U kunt de [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) catalogusweergave om een kolom waarvoor de eigenschap id te identificeren.

Als u wilt, kunt u beter begrip van het databaseschema in dit voorbeeld laat zien hoe sys.identity_column integreren is met andere systeemcatalogusweergaven:

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
De id-eigenschap kan niet worden gebruikt:
- Wanneer het gegevenstype van kolom is niet INT- of BIGINT
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

Deze sectie bevat een voorbeeld van code die kunt u algemene taken uitvoert wanneer u met de id-kolommen werkt. 

Kolom C1 is de identiteit in de volgende taken.
 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Zoek de hoogste toegewezen waarde voor een tabel
Gebruik de `MAX()` functie om te bepalen van de hoogste waarde voor een distributietabel toegewezen:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>De seed- en increment vinden voor de identiteitseigenschap
U kunt de catalogusweergaven gebruiken voor het detecteren van de identiteit verhogings-seed configuratiewaarden voor een tabel met behulp van de volgende query uit: 

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

* Voor meer informatie over het ontwikkelen van tabellen, het overzicht [tabel] [Overview].  

