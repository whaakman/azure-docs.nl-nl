---
title: Variabelen toewijzen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 6c943478f3904aac17a572f012f2b2b69ffa2223
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479550"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Variabelen toewijzen in Azure SQL Data Warehouse

Tips voor het toewijzen van T-SQL-variabelen in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen.

## <a name="setting-variables-with-declare"></a>Variabelen instellen met DECLAReren

Variabelen in SQL Data Warehouse worden ingesteld met behulp van `DECLARE` de `SET` instructie of de instructie. Initialisatie van variabelen met DECLAReren is een van de meest flexibele manieren om een variabele waarde in SQL Data Warehouse in te stellen.

```sql
DECLARE @v  int = 0
;
```

U kunt DECLARe ook gebruiken om meer dan één variabele per keer in te stellen. U kunt niet selecteren of bijwerken gebruiken om het volgende te doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kunt een variabele niet initialiseren en gebruiken in dezelfde DECLARe-instructie. Om het punt te illustreren, is het volgende voor beeld **niet** toegestaan @p1 omdat het is geïnitialiseerd en wordt gebruikt in dezelfde Declare-instructie. In het volgende voor beeld wordt een fout weer geven.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Waarden instellen met SET

SET is een gemeen schappelijke methode voor het instellen van één variabele.

De volgende instructies zijn allemaal geldige manieren om een variabele in te stellen met SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt slechts één variabele tegelijk instellen met SET. Samengestelde Opera tors zijn echter wel toegestaan.

## <a name="limitations"></a>Beperkingen

U kunt UPDATE niet gebruiken voor het toewijzen van variabelen.

## <a name="next-steps"></a>Volgende stappen

Zie [ontwikkelings overzicht](sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkel aars.
