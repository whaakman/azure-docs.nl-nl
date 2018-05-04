---
title: Dynamische SQL met in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 604074e0a645918f7033360b79a1b7cad050c9e4
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamische SQL in SQL datawarehouse
Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="dynamic-sql-example"></a>Dynamische SQL-voorbeeld

Bij het ontwikkelen van toepassingscode voor SQL Data Warehouse, moet u wellicht dynamische sql om flexibele, algemene en modulair oplossingen te gebruiken. SQL Data Warehouse biedt geen ondersteuning voor blob-gegevenstypen op dit moment. Ondersteunt geen blob-gegevenstypen mogelijk beperkt de omvang van uw tekenreeksen sinds de blob-gegevenstypen zijn zowel varchar(max) en nvarchar(max) typen. Als u deze typen in uw toepassingscode gebruikt hebt om grote tekenreeksen samen te stellen, moet u de code in segmenten opsplitsen en gebruik in plaats daarvan de instructie EXEC.

Een eenvoudig voorbeeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de tekenreeks kort is, kunt u [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) die normaal werken.

> [!NOTE]
> Instructies die worden uitgevoerd als dynamische SQL nog steeds onderworpen aan alle TSQL-validatieregels.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

