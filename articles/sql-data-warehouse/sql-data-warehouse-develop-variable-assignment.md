---
title: Toewijzen van variabelen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: ee97f7e5cda8b954fb697f73746e416d88d38c2d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401695"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Toewijzen van variabelen in Azure SQL Data Warehouse

Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="setting-variables-with-declare"></a>Variabelen met DECLARE instellen

Variabelen in SQL Data Warehouse worden ingesteld met behulp van de `DECLARE` instructie of de `SET` instructie. Tijdens de initialisatie van variabelen met DECLARE, is een van de meest flexibele manieren voor het instellen van een variabele waarde in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

U kunt ook DECLARE meer dan één variabele instellen op een tijdstip. U kunt selecteren of de UPDATE niet gebruiken het volgende doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kunt geen initialiseren en het gebruik van een variabele in dezelfde instructie DECLARE. Ter illustratie van de punt, het volgende voorbeeld is **niet** toegestaan omdat @p1 is geïnitialiseerd en gebruikt in dezelfde instructie DECLARE. Het volgende voorbeeld wordt een fout.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Instellingswaarden is ingesteld

Een veelgebruikte methode voor het instellen van een enkele variabele is ingesteld, is.

De volgende instructies zijn alle geldige manieren voor het instellen van een variabele is ingesteld:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt alleen een variabele instellen op een tijdstip is ingesteld. Samengestelde operators zijn echter toegestaan.

## <a name="limitations"></a>Beperkingen

U kunt UPDATE niet gebruiken voor variabele toewijzing.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).
