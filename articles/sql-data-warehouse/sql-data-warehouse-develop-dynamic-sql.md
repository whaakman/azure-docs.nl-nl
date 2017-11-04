---
title: Dynamische SQL in SQL datawarehouse | Microsoft Docs
description: Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29228676373aee8dbc7b1b2a7d92ffc978333804
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamische SQL in SQL datawarehouse
Bij het ontwikkelen van toepassingscode voor SQL Data Warehouse, moet u wellicht dynamische sql om flexibele, algemene en modulair oplossingen te gebruiken. SQL Data Warehouse biedt geen ondersteuning voor blob-gegevenstypen op dit moment. Dit beperkt mogelijk de grootte van uw tekenreeksen als blob typen zowel varchar(max) en nvarchar(max) typen bevatten. Als u deze typen in uw toepassingscode gebruikt hebt bij het bouwen van zeer grote tekenreeksen, moet u de code in segmenten opsplitsen en gebruik in plaats daarvan de instructie EXEC.

Een eenvoudig voorbeeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de tekenreeks kort is kunt u [sp_executesql] [ sp_executesql] die normaal werken.

> [!NOTE]
> Instructies die worden uitgevoerd als dynamische SQL nog steeds onderworpen aan alle TSQL-validatieregels.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
