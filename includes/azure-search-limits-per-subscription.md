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
ms.openlocfilehash: 8a4f794c8ef24a90498954629c131904621c5b43
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30844080"
---
U kunt meerdere services binnen een abonnement, elk criterium ingericht op een specifieke laag, alleen beperkt door het aantal services dat is toegestaan op elke laag maken. U kunt bijvoorbeeld maximaal 12 services op de basisstaffel en een andere 12 services op de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over categorieën [een SKU of laag kiezen voor Azure Search](../articles/search/search-sku-tier.md).

Maximale Servicelimieten kunnen op verzoek worden verhoogd. Neem contact op met ondersteuning van Azure als u meer services die zich binnen hetzelfde abonnement nodig.

| Resource            | Gratis&nbsp;<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Maximale services    |1     | 12    | 12  | 6  | 6  | 6     |
| Maximale schaal in SU&nbsp;<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> vrij is op basis van gedeelde, niet-toegewezen bronnen. Scale-up wordt niet ondersteund op gedeelde bronnen.

<sup>2</sup> search-eenheden (SU) zijn facturering eenheden, toegewezen als hetzij een *replica* of een *partitie*. U moet beide resources voor opslag, indexeren en querybewerkingen. Zie voor meer informatie over SU berekeningen, [schalen niveaus van de bron voor query's en indexen werkbelastingen](../articles/search/search-capacity-planning.md). 