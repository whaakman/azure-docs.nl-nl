---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 037996385f34c5037e0386686e3bdf8dc1b7a37a
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219831"
---
Zone-redundante opslag (ZRS) repliceert uw gegevens synchroon tussen drie opslagclusters in één regio. Elke opslagcluster fysiek gescheiden van de andere en bevindt zich in een eigen binnen een beschikbaarheidszone (AZ). Elke beschikbaarheidszone&mdash;en het cluster ZRS binnen het&mdash;autonome en is voorzien van afzonderlijke hulpprogramma's en functies voor netwerkbeheer.

Wanneer u uw gegevens in een opslagaccount met behulp van ZRS-replicatie opslaat, kunt u blijven toegang tot uw gegevens en beheren als een beschikbaarheidszone beschikbaar is. ZRS biedt uitstekende prestaties en lage latentie. ZRS biedt dezelfde [schaalbaarheidsdoelen](../articles/storage/common/storage-scalability-targets.md) als [lokaal redundante opslag (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Denk aan ZRS voor scenario's waarin consistentie, duurzaamheid en hoge beschikbaarheid. Zelfs als een stroomstoring of een natuurlijke ramp rendert een beschikbaarheidszone is niet beschikbaar, ZRS biedt duurzaamheid voor opslagobjecten van ten minste 99,9999999999% (12 9's) gedurende een bepaald jaar.

Zie voor meer informatie over beschikbaarheidszones [overzicht van Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview).