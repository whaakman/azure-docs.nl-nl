---
title: bestand opnemen
description: bestand opnemen
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 1e147e8bd9260cd1ece60b70641968a229995ec1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176414"
---
U kunt meerdere services binnen een abonnement maken. Elke service kan worden ingericht op een specifieke laag. U bent alleen beperkt door het aantal services die op elke laag worden toegestaan. U kunt bijvoorbeeld maximaal 12 services op de Basic-laag en een andere 12 services op de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over Prijscategorieën [SKU of laag kiezen voor Azure Search](../articles/search/search-sku-tier.md).

Maximale Servicelimieten kunnen worden verhoogd op aanvraag. Als u meer services binnen hetzelfde abonnement nodig hebt, neem dan contact op met ondersteuning voor Azure.

| Resource            | Gratis<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Maximum aantal services    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Maximale schaal in search-eenheden (SU)<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> vrij is op basis van gedeelde, niet-toegewezen resources. Scale-up wordt niet ondersteund op gedeelde bronnen.

<sup>2</sup> zoekeenheden factureren eenheden, die als hetzij een *replica* of een *partitie*. U moet beide resources voor opslag, indexeren en querybewerkingen. Zie voor meer informatie over het SU-berekeningen, [schalen resource niveaus voor werkbelastingen voor query's en indexen](../articles/search/search-capacity-planning.md). 