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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755659"
---
U kunt meerdere services binnen een abonnement, stuk ingericht op een bepaalde laag, alleen beperkt door het aantal services die zijn toegestaan op elke laag maken. U kunt bijvoorbeeld maximaal 12 services op de Basic-laag en een andere 12 services op de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over Prijscategorieën [een SKU of laag kiezen voor Azure Search](../articles/search/search-sku-tier.md).

Maximale Servicelimieten kunnen worden verhoogd op aanvraag. Neem contact op met ondersteuning van Azure als u meer services binnen hetzelfde abonnement nodig hebt.

| Resource            | Gratis&nbsp;<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Maximum aantal services    |1     | 12    | 12  | 6  | 6  | 6     |
| Maximale schaal in SU&nbsp;<sup>2</sup> |N/A |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> vrij is op basis van gedeelde, niet-toegewezen resources. Scale-up wordt niet ondersteund op gedeelde bronnen.

<sup>2</sup> search-eenheden (SU) factureren eenheden, die als hetzij een *replica* of een *partitie*. U moet beide resources voor opslag, indexeren en querybewerkingen. Zie voor meer informatie over het SU-berekeningen, [schalen resource niveaus voor werkbelastingen voor query's en indexen](../articles/search/search-capacity-planning.md). 