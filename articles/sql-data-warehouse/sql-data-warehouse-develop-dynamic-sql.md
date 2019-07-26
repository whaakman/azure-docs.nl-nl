---
title: Dynamische SQL gebruiken in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 4454b1d44d0be61dca8571e86c73e09a9527d1eb
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479663"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamische SQL in SQL Data Warehouse
Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.

## <a name="dynamic-sql-example"></a>Voor beeld van dynamische SQL

Bij het ontwikkelen van toepassings code voor SQL Data Warehouse moet u mogelijk dynamische SQL gebruiken om flexibele, algemene en modulaire oplossingen te leveren. SQL Data Warehouse biedt op dit moment geen ondersteuning voor BLOB-gegevens typen. Het ondersteunen van BLOB-gegevens typen kan de grootte van uw teken reeksen beperken omdat de BLOB-gegevens typen zowel varchar (max) als nvarchar (max)-typen bevatten. Als u deze typen in de toepassings code hebt gebruikt om grote teken reeksen te maken, moet u de code in segmenten afsplitsen en in plaats daarvan de instructie EXEC gebruiken.

Een eenvoudig voor beeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de teken reeks kort is, kunt u [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) als normaal gebruiken.

> [!NOTE]
> Voor de instructies die worden uitgevoerd als dynamische SQL zijn alle TSQL-validatie regels nog steeds van toepassing.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie [ontwikkelings overzicht](sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkel aars.

