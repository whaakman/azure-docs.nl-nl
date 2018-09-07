---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4a43966180850645584043690b1be9d6ae232f6e
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027444"
---
Zone-redundante opslag (ZRS) repliceert uw gegevens synchroon tussen drie opslagclusters in één regio. Elke opslagcluster fysiek gescheiden van de andere en bevindt zich in een eigen binnen een beschikbaarheidszone (AZ). Elke beschikbaarheidszone en wordt het cluster ZRS locatiemogelijkheid is autonome met afzonderlijke hulpprogramma's en netwerkmogelijkheden.

Wanneer uw gegevens opslaat in een ZRS-account, zorgt ervoor dat u wel toegang krijgen tot en beheer van uw gegevens in het geval dat een zone niet beschikbaar. ZRS biedt uitstekende prestaties en lage latentie. ZRS biedt dezelfde [schaalbaarheidsdoelen](../articles/storage/common/storage-scalability-targets.md) als [lokaal redundante opslag (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Denk aan ZRS voor scenario's waarin grote consistentie, sterk duurzaamheid en hoge beschikbaarheid, zelfs als een stroomstoring of een natuurlijke ramp Hiermee maakt u een zonegebonden datacenter is niet beschikbaar. ZRS biedt duurzaamheid voor opslagobjecten van ten minste 99,9999999999% (12 9's) gedurende een bepaald jaar.

Zie voor meer informatie over beschikbaarheidszones [overzicht van Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview).