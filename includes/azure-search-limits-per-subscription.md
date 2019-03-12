---
title: bestand opnemen
description: bestand opnemen
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: a466ea29fa31c4c628724f3d5138a1612ef7a0f4
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553385"
---
U kunt meerdere services binnen een abonnement maken. Elke service kan worden ingericht op een specifieke laag. U bent alleen beperkt door het aantal services die op elke laag worden toegestaan. U kunt bijvoorbeeld maximaal 12 services op de Basic-laag en een andere 12 services op de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over Prijscategorieën [SKU of laag kiezen voor Azure Search](../articles/search/search-sku-tier.md).

Maximale Servicelimieten kunnen worden verhoogd op aanvraag. Als u meer services binnen hetzelfde abonnement nodig hebt, neem dan contact op met ondersteuning voor Azure.

| Resource            | Gratis<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Maximum aantal services    |1     | 12    | 12  | 6  | 6  | 6     |
| Maximale schaal in search-eenheden (SU)<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup>vrij is op basis van gedeelde, niet-toegewezen resources. Scale-up wordt niet ondersteund op gedeelde bronnen.

<sup>2</sup>zoekeenheden factureren eenheden, die als hetzij een *replica* of een *partitie*. U moet beide resources voor opslag, indexeren en querybewerkingen. Zie voor meer informatie over het SU-berekeningen, [schalen resource niveaus voor werkbelastingen voor query's en indexen](../articles/search/search-capacity-planning.md). 