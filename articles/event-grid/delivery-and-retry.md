---
title: Azure Event raster levering en probeer het opnieuw
description: Beschrijft hoe Azure gebeurtenis raster gebeurtenissen biedt en de manier waarop niet-bezorgde berichten worden verwerkt.
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/10/2018
ms.author: darosa
ms.openlocfilehash: fe9089334deceb38186add56ce3fb1d6ecc20363
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Gebeurtenis raster berichtbezorging en probeer het opnieuw 

Dit artikel wordt beschreven hoe Azure gebeurtenis raster omgaat met gebeurtenissen bij levering is niet bevestigd.

Gebeurtenis raster bevat duurzame levering. Elk bericht ten minste eenmaal voor elk abonnement levert. Gebeurtenissen worden verzonden naar de geregistreerde webhook van elk abonnement onmiddellijk. Als een webhook Bevestig de ontvangst van een gebeurtenis niet binnen 60 seconden na de eerste poging voor levering, opnieuw gebeurtenis raster levering van de gebeurtenis.

## <a name="message-delivery-status"></a>Bericht bezorgingsstatus

Gebeurtenis raster maakt gebruik van HTTP-antwoordcodes ontvangst te bevestigen van gebeurtenissen. 

### <a name="success-codes"></a>Geslaagd-codes

De volgende codes voor HTTP-antwoord geven aan dat een gebeurtenis met succes is bezorgd bij uw webhook. Gebeurtenis raster beschouwt levering voltooid.

- 200 OK
- 202 geaccepteerd

### <a name="failure-codes"></a>Fout-codes

De volgende codes voor HTTP-antwoord geven aan dat een gebeurtenis bezorging is mislukt. Gebeurtenis raster opnieuw geprobeerd om de gebeurtenis te verzenden. 

- 400 onjuiste aanvraag
- 401-niet toegestaan
- 404 – Niet gevonden
- Time-out voor 408 aanvragen
- 414 URI te lang
- Interne serverfout 500
- 503 Service niet beschikbaar
- 504 gateway Timeout

Andere antwoordcode of een gebrek aan een antwoord geeft een fout. Gebeurtenis raster pogingen levering. 

## <a name="retry-intervals"></a>Intervallen voor nieuwe pogingen

Een beleid voor opnieuw proberen van exponentieel uitstel gebruikt voor gebeurtenis raster voor de levering van de gebeurtenis. Als uw webhook niet reageert of een foutcode retourneert, opnieuw gebeurtenis raster levering op het volgende schema:

1. 10 seconden
2. 30 seconden
3. 1 minuut
4. 5 minuten
5. 10 minuten
6. 30 minuten
7. 1 uur

Gebeurtenis raster voegt een kleine willekeurige toe aan alle intervallen voor opnieuw proberen.

## <a name="retry-duration"></a>Probeer duur

Tijdens de preview verloopt Azure gebeurtenis raster alle gebeurtenissen die niet worden bezorgd binnen twee uur.

## <a name="next-steps"></a>Volgende stappen

* De status van de gebeurtenis leveringen Zie [Monitor gebeurtenis raster berichtbezorging](monitor-event-delivery.md).
* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).