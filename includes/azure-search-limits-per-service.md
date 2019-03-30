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
ms.openlocfilehash: 30c6fc189ebcd497a214828f65213a55cefdf03f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632862"
---
Opslag wordt beperkt door de schijfruimte of door een vaste limiet op de *maximum aantal* van indexen, document of andere bronnen op hoog niveau, afhankelijk van wat het eerste komt. De volgende tabel worden de opslaglimieten. Zie voor de maximumlimieten op indexen, documenten en andere objecten, [limieten per resource](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Resource | Gratis | Basic<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD<sup>2</sup> | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Serviceovereenkomst (SLA)<sup>3</sup>  |Nee |Ja |Ja |Ja |Ja |Ja |Ja |Ja |
| Opslag per partitie |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Partities per service |N/A |1 |12 |12 |12 |3 |12 |12 |
| Partitiegrootte |N/A |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Replica's |N/A |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> basic heeft een vaste partitie. Op deze laag, worden extra search-eenheden voor het toewijzen van meer replica's voor hogere querywerkbelastingen gebruikt.

<sup>2</sup> S3 HD heeft een vaste limiet van drie partities, dit is lager dan de partitielimiet voor S3. De partitielimiet is lager doordat S3 HD aanzienlijk meer indexen heeft. Omdat er servicelimieten bestaan voor zowel rekenresources (opslag en verwerking) als inhoud (indexen en documenten), wordt de limiet voor de inhoud het eerst bereikt.

<sup>3</sup> serviceovereenkomsten voor factureerbare services op specifieke resources worden aangeboden. Gratis services en preview-functies hebben geen SLA. Voor factureerbare services, sla's van kracht wanneer u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de query (lezen) Sla's. Drie of meer replica's zijn vereist voor query's en indexering van SLA's (lezen-schrijven). Het aantal partities is niet een aandachtspunt SLA. 