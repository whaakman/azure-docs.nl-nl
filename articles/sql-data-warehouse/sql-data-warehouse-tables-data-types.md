---
title: Gegevenstypen - Azure SQL Data Warehouse definiëren | Microsoft Docs
description: Aanbevelingen voor de tabel gegevenstypen definiëren in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 4d8a222a6d4cfa4138fe833fb4e9cc895dbc5f65
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Gegevenstypen van de tabel in Azure SQL Data Warehouse
Aanbevelingen voor de tabel gegevenstypen definiëren in Azure SQL Data Warehouse. 

## <a name="what-are-the-data-types"></a>Wat zijn de gegevenstypen?

SQL Data Warehouse ondersteunt gebruikte de meest gegevenstypen. Zie voor een lijst van de ondersteunde gegevenstypen [gegevenstypen](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) in de instructie CREATE TABLE. 

## <a name="minimize-row-length"></a>Rijlengte minimaliseren
Voor het minimaliseren van de grootte van gegevenstypen verkort de rijlengte, wat tot betere prestaties van query's leidt. Gebruik de kleinste gegevenstype die geschikt is voor uw gegevens. 

- Vermijd tekenkolommen met een grote standaardlengte wordt vereist. Bijvoorbeeld, als de langste waarde bestaat uit 25 tekens, vervolgens definiëren de kolom als VARCHAR(25). 
- Vermijd het gebruik van [NVARCHAR] [NVARCHAR] als u alleen VARCHAR nodig.
- Gebruik zo mogelijk NVARCHAR(4000) of VARCHAR(8000) in plaats van NVARCHAR(MAX) of VARCHAR(MAX).

Als u van PolyBase externe tabellen gebruikmaakt laden van de tabellen, kan de gedefinieerde lengte van de tabelrij kan niet groter zijn dan 1 MB. Wanneer een rij met variabele lengte groter is dan 1 MB, kunt u de rij met BCP, maar niet met PolyBase laden.

## <a name="identify-unsupported-data-types"></a>Niet-ondersteunde gegevenstypen identificeren
Als u uw database uit een andere SQL-database migreert, kunt u de gegevenstypen die worden niet ondersteund in SQL Data Warehouse kunt tegenkomen. Gebruik deze query voor het detecteren van niet-ondersteunde gegevenstypen in uw bestaande SQL-schema.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Tijdelijke oplossingen voor niet-ondersteunde gegevenstypen

De volgende lijst bevat de gegevenstypen dat SQL Data Warehouse biedt geen ondersteuning voor en geeft alternatieven die u in plaats van de niet-ondersteunde gegevenstypen gebruiken kunt.

| Niet-ondersteund gegevenstype | Tijdelijke oplossing |
| --- | --- |
| [geometrie](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Geografie](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Tekst](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |Kolom splitsen in meerdere sterk getypeerde kolommen. |
| [Tabel](/sql/t-sql/data-types/table-transact-sql) |Converteren naar tijdelijke tabellen. |
| [tijdstempel](/sql/t-sql/data-types/date-and-time-types) |Bijwerken van code voor het gebruik [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) en de [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) functie. Alleen constanten worden ondersteund als standaardwaarden, daarom current_timestamp kan niet worden gedefinieerd als een default-beperking. Als u migreren versie rijwaarden uit een getypeerde timestamp-kolom wilt, gebruikt u [binaire](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) of [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) voor het niet leeg of NULL rij Versiewaarden. |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [de gebruiker gedefinieerd type](/sql/relational-databases/native-client/features/using-user-defined-types) |Terug te converteren naar het native gegevenstype indien mogelijk. |
| standaardwaarden | Standaardwaarden ondersteuning literals en alleen constanten. |


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwikkelen van tabellen [tabel overzicht](sql-data-warehouse-tables-overview.md).
