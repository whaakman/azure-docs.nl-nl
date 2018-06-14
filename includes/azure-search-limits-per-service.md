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
ms.openlocfilehash: b4062aab5a453505ef4586f422a124d4bbf715cb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30844060"
---
Opslag wordt begrensd door schijfruimte of een vaste limiet op de *maximumaantal* van indexen, document of andere bronnen op hoog niveau, afhankelijk van wat zich het eerste voordoet. De volgende tabel worden de opslaglimieten. Zie voor bovengrenzen voor indexen, documenten en andere objecten, [limieten door resource](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Resource | Gratis | Basic&nbsp;<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>2</sup> |
| -------- | --- | --- | --- | --- | --- | --- |
| Service Level Agreement (SLA) <sup>3</sup>  |Nee |Ja |Ja |Ja |Ja |Ja |
| Opslag per partitie |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partities per service |N/A |1 |12 |12 |12 |3 |
| Partitiegrootte |N/A |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replica's |N/A |3 |12 |12 |12 |12 |

<sup>1</sup> basic heeft een vaste partitie. Bij deze laag worden aanvullende SUs gebruikt voor het toewijzen van meer replica's voor query hogere werkbelastingen.

<sup>2</sup> S3 HD heeft een vaste limiet van 3 partities. Dit is lager dan de partitielimiet voor S3. De partitielimiet is lager doordat S3 HD aanzienlijk meer indexen heeft. Omdat er servicelimieten bestaan voor zowel rekenresources (opslag en verwerking) als inhoud (indexen en documenten), wordt de limiet voor de inhoud het eerst bereikt.

<sup>3</sup> Service level agreements (Sla's) worden aangeboden voor factureerbare services op toegewijde bronnen. Gratis services en de preview-functies hebben geen SLA. Voor factureerbare services Sla's van kracht als u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de SLA voor query (gelezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen / schrijven). Het aantal partities is niet een SLA-overweging. 