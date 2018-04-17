---
title: Met labels op instrument query's in SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van labels op instrument query's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 48fe625573639c0ec98e672f02a35e4a9ae268e8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Met behulp van de labels op instrument query's in Azure SQL Data Warehouse
Tips voor het gebruik van labels op instrument query's in Azure SQL Data Warehouse om oplossingen te ontwikkelen.


## <a name="what-are-labels"></a>Wat zijn labels?
SQL Data Warehouse ondersteunt een concept query labels genoemd. Voordat u doorgaat naar eventuele diepte, bekijk een voorbeeld:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

De laatste regel labels van de tekenreeks 'Mijn Query Label' aan de query. Deze code is vooral handig zijn, omdat het label query kunnen via de DMV's. Een query uitvoert voor labels biedt een mechanisme voor probleem query's te zoeken en helpen identificeren door een ELT uitvoeren.

Een goede naamgevingsconventie echt helpt. Bijvoorbeeld, helpt vanaf het label PROJECT kunt PROCEDURE, instructie of opmerking om de query onder de code in broncodebeheer uniek te identificeren.

De volgende query maakt gebruik van een dynamische Beheerweergave om te zoeken op label.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Het is belangrijk om de vierkante haken of dubbele aanhalingstekens rond het word-label worden geplaatst bij het opvragen van. Label is een gereserveerd woord en veroorzaakt een fout bij het niet worden gescheiden. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).


