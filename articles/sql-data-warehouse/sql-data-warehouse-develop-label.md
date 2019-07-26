---
title: Labels gebruiken voor instrument query's in SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van labels voor instrument query's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: ee991fdfcd93ea064d1205d61d07adf377cce667
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480034"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Labels gebruiken voor instrument query's in Azure SQL Data Warehouse
Tips voor het gebruik van labels voor instrument query's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.


## <a name="what-are-labels"></a>Wat zijn labels?
SQL Data Warehouse ondersteunt een concept met de naam query labels. We kijken naar een voor beeld voordat u naar een andere diepte gaat:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Met de laatste regel wordt de teken reeks ' mijn query label ' gelabeld in de query. Deze tag is vooral nuttig omdat het label query's kan uitvoeren via de Dmv's. Het uitvoeren van query's voor labels biedt een mechanisme voor het vinden van probleem query's en het identificeren van de voortgang van een ELT-uitvoering.

Een goede naam Conventie is echt handig. Als u bijvoorbeeld het label met het PROJECT, de PROCEDURE, de instructie of de opmerking start, wordt de query op een unieke manier geïdentificeerd voor alle code in broncode beheer.

De volgende query gebruikt een dynamische beheer weergave om te zoeken op label.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Het is essentieel om rechte haken of dubbele aanhalings tekens rond het woord label te plaatsen tijdens het uitvoeren van query's. Label is een gereserveerd woord dat een fout veroorzaakt wanneer het niet wordt gescheiden. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie [ontwikkelings overzicht](sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkel aars.


