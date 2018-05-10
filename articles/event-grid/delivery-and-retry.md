---
title: Azure Event raster levering en probeer het opnieuw
description: Beschrijft hoe Azure gebeurtenis raster gebeurtenissen biedt en de manier waarop niet-bezorgde berichten worden verwerkt.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: db16a4ba2177e92fa4500af0969c44471004ba73
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Gebeurtenis raster berichtbezorging en probeer het opnieuw 

Dit artikel wordt beschreven hoe Azure gebeurtenis raster omgaat met gebeurtenissen bij levering is niet bevestigd.

Gebeurtenis raster bevat duurzame levering. Elk bericht ten minste eenmaal voor elk abonnement levert. Gebeurtenissen worden verzonden naar de geregistreerde webhook van elk abonnement onmiddellijk. Als een webhook Bevestig de ontvangst van een gebeurtenis niet binnen 60 seconden na de eerste poging voor levering, opnieuw gebeurtenis raster levering van de gebeurtenis. 

Op dit moment gebeurtenis raster elke gebeurtenis afzonderlijk naar abonnees worden verzonden. De abonnee ontvangt een matrix met één gebeurtenis.

## <a name="message-delivery-status"></a>Bericht bezorgingsstatus

Gebeurtenis raster maakt gebruik van HTTP-antwoordcodes ontvangst te bevestigen van gebeurtenissen. 

### <a name="success-codes"></a>Geslaagd-codes

De volgende codes voor HTTP-antwoord geven aan dat een gebeurtenis met succes is bezorgd bij uw webhook. Gebeurtenis raster beschouwt levering voltooid.

- 200 OK
- 202 geaccepteerd

### <a name="failure-codes"></a>Fout-codes

De volgende codes voor HTTP-antwoord geven aan dat een gebeurtenis bezorging is mislukt. 

- 400 onjuiste aanvraag
- 401-niet toegestaan
- 404 – Niet gevonden
- Time-out voor 408 aanvragen
- 414 URI te lang
- 500 Interne serverfout
- 503 Service niet beschikbaar
- 504 Time-out van gateway

Als gebeurtenis raster een fout die aangeeft dat het eindpunt is niet beschikbaar ontvangt, probeert het opnieuw om de gebeurtenis te verzenden. 

## <a name="retry-intervals-and-duration"></a>Intervallen voor nieuwe pogingen en duur

Een beleid voor opnieuw proberen van exponentieel uitstel gebruikt voor gebeurtenis raster voor de levering van de gebeurtenis. Als uw webhook niet reageert of een foutcode retourneert, opnieuw gebeurtenis raster levering op het volgende schema:

1. 10 seconden
2. 30 seconden
3. 1 minuut
4. 5 minuten
5. 10 minuten
6. 30 minuten
7. 1 uur

Gebeurtenis raster voegt een kleine willekeurige toe aan alle intervallen voor opnieuw proberen. Levering van de gebeurtenis wordt opnieuw geprobeerd eens per uur na een uur.

Gebeurtenis raster verloopt standaard alle gebeurtenissen die niet worden bezorgd binnen 24 uur.

## <a name="next-steps"></a>Volgende stappen

* De status van de gebeurtenis leveringen Zie [Monitor gebeurtenis raster berichtbezorging](monitor-event-delivery.md).
* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).