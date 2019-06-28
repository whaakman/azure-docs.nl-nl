---
title: De component OFFSET LIMIET in Azure Cosmos DB
description: Meer informatie over de component OFFSET LIMIET voor Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 60ac28c80e9f7cc72f4d6005c12cb5f68671341e
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342457"
---
# <a name="offset-limit-clause"></a>De component OFFSET LIMIET

De component OFFSET LIMIET is een optionele component overslaan en vervolgens een aantal waarden van de query uitvoeren. Het aantal OFFSET en het maximum aantal zijn vereist in de component OFFSET LIMIET.

Wanneer de LIMIET van OFFSET wordt gebruikt in combinatie met een component ORDER BY, wordt de resultatenset wordt geproduceerd door te doen overslaan en de geordende waarden ondernemen. Als er geen component ORDER BY wordt gebruikt, wordt deze resulteren in een deterministische volgorde van waarden.

## <a name="syntax"></a>Syntaxis
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumenten

- `<offset_amount>`

   Hiermee geeft u het geheel getal van de items die de resultaten van de query moeten worden overgeslagen.

- `<limit_amount>`
  
   Hiermee geeft u het geheel getal van de items die de queryresultaten bevatten

## <a name="remarks"></a>Opmerkingen
  
  Zowel de verschuiving van het aantal en het maximum aantal zijn vereist in de component OFFSET LIMIET. Als een optionele `ORDER BY` component wordt gebruikt, wordt de resultatenset wordt geproduceerd door de overslaan doen via de geordende waarden. Anders retourneert de query een vaste volgorde van waarden. Momenteel deze component wordt ondersteund voor query's in één partitie, partitie-overkoepelende query's niet nog ondersteund.

## <a name="examples"></a>Voorbeelden

Bijvoorbeeld, als volgt een query die de eerste waarde overslaat en retourneert de tweede waarde (in volgorde van de naam van het residente stad):

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

De resultaten zijn:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Hier volgt een query die de eerste waarde overslaat en de tweede waarde retourneert (zonder volgorde):

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

De resultaten zijn:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [SELECT-component](sql-query-select.md)
- [ORDER BY-component](sql-query-order-by.md)
