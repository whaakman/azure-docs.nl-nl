---
title: Definieert gegevenstypen - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen voor het definiëren van gegevenstypes tabel in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: eb469e6a654414b0411f8c45b73658f99a383751
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306580"
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Gegevenstypes tabel in Azure SQL Data Warehouse
Aanbevelingen voor het definiëren van gegevenstypes tabel in Azure SQL Data Warehouse. 

## <a name="what-are-the-data-types"></a>Wat zijn de gegevenstypen?

SQL Data Warehouse ondersteunt de meest gebruikte gegevenstypen. Zie voor een lijst van de ondersteunde gegevenstypen, [gegevenstypen](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) in de instructie CREATE TABLE. 

## <a name="minimize-row-length"></a>Rijlengte minimaliseren
Minimaliseren van de grootte van gegevenstypen verkort de rijlengte, die tot betere prestaties van query's leiden. Gebruik het kleinste gegevenstype die geschikt is voor uw gegevens. 

- Vermijd tekenkolommen met een grote standaardlengte definiëren. Bijvoorbeeld, als de langste waarde 25 tekens is, definieert u uw kolom als VARCHAR(25). 
- Vermijd het gebruik van [NVARCHAR] [NVARCHAR] als u VARCHAR maar nodig hebt.
- Gebruik zo mogelijk NVARCHAR(4000) of VARCHAR(8000) in plaats van NVARCHAR(MAX) of VARCHAR(MAX).

Als u van PolyBase externe tabellen gebruikmaakt laden van de tabellen, kan de opgegeven lengte van een rij in de tabel kan niet groter zijn dan 1 MB. Wanneer een rij met gegevens met variabele lengte is groter dan 1 MB, kunt u de rij met BCP, maar niet met PolyBase te laden.

## <a name="identify-unsupported-data-types"></a>Niet-ondersteunde gegevenstypen identificeren
Als u uw database vanaf een andere SQL-database migreert, kunt u de gegevenstypen die worden niet ondersteund in SQL Data Warehouse kunt tegenkomen. Gebruik deze query voor het detecteren van niet-ondersteunde gegevenstypen in uw bestaande SQL-schema.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Tijdelijke oplossingen voor niet-ondersteunde gegevenstypen

De volgende lijst bevat de gegevenstypen die in SQL Data Warehouse biedt geen ondersteuning voor en biedt alternatieven die u in plaats van de niet-ondersteunde gegevenstypen gebruiken kunt.

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
| [Tijdstempel](/sql/t-sql/data-types/date-and-time-types) |Bijwerken van de code voor het gebruik van [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) en de [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) functie. Alleen constanten worden ondersteund als standaardwaarden, daarom current_timestamp kan niet worden gedefinieerd als een default-beperking. Als u nodig hebt voor het migreren van versie rijwaarden uit een getypeerde timestamp-kolom, gebruikt u [binaire](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) of [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) rij Versiewaarden voor het niet leeg of null zijn. |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [de gebruiker gedefinieerd type](/sql/relational-databases/native-client/features/using-user-defined-types) |Converteren naar het oorspronkelijke gegevenstype indien mogelijk. |
| standaardwaarden | Standaardwaarden ondersteunen letterlijke tekenreeksen zijn en alleen constanten. |


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwikkelen van tabellen [tabeloverzicht](sql-data-warehouse-tables-overview.md).
