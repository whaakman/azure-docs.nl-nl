---
title: Toewijzen van variabelen in SQL Data Warehouse | Microsoft Docs
description: Tips voor het toewijzen van Transact-SQL-variabelen in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 045d5148cd3f12dac63c961ccf7c953d355ed725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a>Variabelen in SQL Data Warehouse toewijzen
Variabelen in SQL Data Warehouse worden ingesteld met de `DECLARE` instructie of de `SET` instructie.

De volgende zijn geldige manieren voor het instellen van een variabele waarde:

## <a name="setting-variables-with-declare"></a>Variabelen met DECLARE instellen
Tijdens de initialisatie van variabelen met DECLARE, is een van de meest flexibele manieren voor het instellen van een variabele waarde in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

U kunt ook DECLARE meer dan één variabele instellen op een tijdstip. U kunt geen gebruiken `SELECT` of `UPDATE` om dit te doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kunt geen initialiseren of een variabele gebruiken in dezelfde instructie DECLARE. Ter illustratie van het punt het onderstaande voorbeeld is **niet** toegestaan als @p1 is geïnitialiseerd en gebruikt in dezelfde instructie DECLARE. Dit leidt tot een fout opgetreden.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Waarden van instellingen is ingesteld
Is een veelgebruikte manier voor het instellen van een enkele variabele ingesteld.

De volgende voorbeelden zijn geldige manieren voor het instellen van een variabele is ingesteld:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt alleen een variabele instellen op een tijdstip is ingesteld. Zoals hierboven beschreven zijn samengestelde operators toegestane.

## <a name="limitations"></a>Beperkingen
U kunt selecteren of UPDATE niet gebruiken voor variabele toewijzing.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
