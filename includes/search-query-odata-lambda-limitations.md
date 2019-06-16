---
author: brjohnstmsft
ms.service: search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: b4d0bc73e652a1cd6ef59589318b92f63727c7f5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079637"
---
| Gegevenstype | Functies die zijn toegestaan in de lambda-expressies met `any` | Functies die zijn toegestaan in de lambda-expressies met `all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Alles behalve `search.ismatch` en `search.ismatchscoring` | Dezelfde |
| `Collection(Edm.String)` | Vergelijkingen met `eq` of `search.in` <br/><br/> De onderliggende expressies combineren met `or` | Vergelijkingen met `ne` of `not search.in()` <br/><br/> De onderliggende expressies combineren met `and` |
| `Collection(Edm.Boolean)` | Vergelijkingen met `eq` of `ne` | Dezelfde |
| `Collection(Edm.GeographyPoint)` | Met behulp van `geo.distance` met `lt` of `le` <br/><br/> `geo.intersects` <br/><br/> De onderliggende expressies combineren met `or` | Met behulp van `geo.distance` met `gt` of `ge` <br/><br/> `not geo.intersects(...)` <br/><br/> De onderliggende expressies combineren met `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | Met behulp van vergelijkingen `eq`, `ne`, `lt`, `gt`, `le`, of `ge` <br/><br/> Vergelijkingen combineren met andere onderliggende expressies gebruiken `or` <br/><br/> Vergelijkingen, behalve combineren `ne` met andere onderliggende expressies gebruiken `and` <br/><br/> Met combinaties van expressies `and` en `or` in [scheidende normaal formulier (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | Met behulp van vergelijkingen `eq`, `ne`, `lt`, `gt`, `le`, of `ge` <br/><br/> Vergelijkingen combineren met andere onderliggende expressies gebruiken `and` <br/><br/> Vergelijkingen, behalve combineren `eq` met andere onderliggende expressies gebruiken `or` <br/><br/> Met combinaties van expressies `and` en `or` in [Conjunctive normaal formulier (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) |
