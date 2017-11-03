---
title: Labels op instrument query's in SQL Data Warehouse gebruiken | Microsoft Docs
description: Tips voor het gebruik van labels op instrument query's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Labels op instrument query's in SQL Data Warehouse gebruiken
SQL Data Warehouse ondersteunt een concept query labels genoemd. Bekijk voordat u doorgaat naar eventuele diepte gaan we een voorbeeld van een:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Deze regel geeft de tekenreeks 'Mijn Query Label' aan de query. Dit is vooral handig zijn, zoals het label query kunnen via de DMV's is. Dit biedt ons een mechanisme voor het opsporen van probleem query's en om te identificeren uitgevoerd via een ETL uitvoeren.

Een goede naamgevingsconventie echt helpt hier. Bijvoorbeeld als ' PROJECT: PROCEDURE: instructie: Opmerking ' kan helpen om de query in onder de code in broncodebeheer uniek te identificeren.

U kunt de volgende query die gebruikmaakt van de dynamische beheerweergaven gebruiken om te zoeken op label:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Het is essentieel dat u vierkante haken of dubbele aanhalingstekens rond het word-label langs tijdens het opvragen van. Label is een gereserveerd woord en wordt een fout veroorzaakt als deze niet zijn gescheiden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
