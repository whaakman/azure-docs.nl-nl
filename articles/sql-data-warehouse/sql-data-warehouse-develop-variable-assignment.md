---
title: Toewijzen van variabelen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 012bc76950f212d69d26607c666e878b22015e70
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Variabelen in Azure SQL Data Warehouse toewijzen
Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="setting-variables-with-declare"></a>Variabelen met DECLARE instellen
Variabelen in SQL Data Warehouse worden ingesteld met de `DECLARE` instructie of de `SET` instructie. Tijdens de initialisatie van variabelen met DECLARE, is een van de meest flexibele manieren voor het instellen van een variabele waarde in SQL Data Warehouse.

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

U kunt geen initialiseren of een variabele gebruiken in dezelfde instructie DECLARE. Ter illustratie van het punt het volgende voorbeeld is **niet** toegestaan sinds @p1 is geïnitialiseerd en gebruikt in dezelfde instructie DECLARE. Het volgende voorbeeld wordt een fout opgetreden.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Waarden van instellingen is ingesteld
Is een algemene methode voor het instellen van een enkele variabele ingesteld.

De volgende instructies zijn allemaal geldig manieren voor het instellen van een variabele is ingesteld:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt alleen een variabele instellen op een tijdstip is ingesteld. Samengestelde operators zijn echter wel toegestaan.

## <a name="limitations"></a>Beperkingen
U kunt selecteren of UPDATE niet gebruiken voor variabele toewijzing.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

