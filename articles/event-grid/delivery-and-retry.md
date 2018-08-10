---
title: Azure Event Grid-levering en probeer het opnieuw
description: Hierin wordt beschreven hoe Azure Event Grid gebeurtenissen biedt en hoe deze omgaat met niet-bezorgde berichten.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: tomfitz
ms.openlocfilehash: b34386a7b416d6f7d8b008a9cb5ef142948a370f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005392"
---
# <a name="event-grid-message-delivery-and-retry"></a>Levering van berichten van Event Grid en probeer het opnieuw 

Dit artikel wordt beschreven hoe Azure Event Grid omgaat met gebeurtenissen bij levering is niet bevestigd.

Event Grid biedt duurzame levering. Het biedt een elk bericht ten minste eenmaal voor elk abonnement. Gebeurtenissen worden onmiddellijk verzonden naar de geregistreerde webhook van elk abonnement. Als een webhook niet Bevestig de ontvangst van een gebeurtenis binnen 60 seconden na de eerste poging voor de levering, pogingen Event Grid levering van de gebeurtenis. 

Op dit moment Event Grid elke gebeurtenis afzonderlijk naar abonnees worden verzonden. De abonnee ontvangt een matrix met een eenmalige gebeurtenis.

## <a name="message-delivery-status"></a>Status van de levering van bericht

Event Grid maakt gebruik van HTTP-responscodes ontvangst te bevestigen van gebeurtenissen. 

### <a name="success-codes"></a>Succescodes

De volgende HTTP-responscodes geven aan dat een gebeurtenis met succes is bezorgd bij de webhook. Event Grid beschouwt levering voltooid.

- 200 OK
- 202 geaccepteerd

### <a name="failure-codes"></a>Foutcodes

De volgende HTTP-responscodes geven aan dat een gebeurtenis levering is mislukt. 

- 400-Ongeldige aanvraag
- 401-niet toegestaan
- 404 – Niet gevonden
- 408 time-out van aanvraag
- 414 URI te lang
- 429 te veel aanvragen
- 500 Interne serverfout
- 503 Service niet beschikbaar
- 504 Time-out van gateway

Als Event Grid ontvangt een foutbericht weergegeven dat het eindpunt is tijdelijk niet beschikbaar of een aanvraag voor toekomstige kan het wel lukken, wordt opnieuw geprobeerd om de gebeurtenis te verzenden. Als de Event Grid ontvangt een foutbericht weergegeven dat de levering nooit worden uitgevoerd en een [dead-letter-eindpunt is geconfigureerd](manage-event-delivery.md), wordt de gebeurtenis verzonden naar de dead-letter-eindpunt. 

## <a name="retry-intervals-and-duration"></a>Interval tussen nieuwe pogingen en de duur

Event Grid maakt gebruik van een beleid voor exponentieel uitstel opnieuw proberen voor de bezorging van gebeurtenissen. Als uw webhook reageert niet of een foutcode retourneert, pogingen Event Grid levering op het volgende schema:

1. 10 seconden
2. 30 seconden
3. 1 minuut
4. 5 minuten
5. 10 minuten
6. 30 minuten
7. 1 uur

Event Grid wordt toegevoegd een kleine willekeurige bij alle intervallen. Levering van gebeurtenissen wordt eenmaal per uur opnieuw uitgevoerd na een uur.

Event Grid verloopt standaard alle gebeurtenissen die niet worden bezorgd binnen 24 uur. U kunt [aanpassen van het beleid voor opnieuw proberen](manage-event-delivery.md) bij het maken van een gebeurtenisabonnement. U opgeven dat het maximum aantal bezorgingspogingen (de standaardwaarde is 30) en de gebeurtenis time-to-live (standaard is 1440 minuten).

## <a name="dead-letter-events"></a>Dead-letter uitvoeren voor gebeurtenissen

Wanneer een gebeurtenis kan niet van Event Grid leveren, kan de niet-bezorgde gebeurtenis verzenden naar een opslagaccount. Dit proces staat bekend als onbestelbare. Niet-bezorgde om gebeurtenissen te bekijken, kunt u deze ophalen van de locatie van de dead-letter uitvoeren. Zie voor meer informatie, [dode letter en beleid voor opnieuw proberen](manage-event-delivery.md).

## <a name="next-steps"></a>Volgende stappen

* De status van de gebeurtenis leveringen, Zie [Monitor Event Grid berichtbezorging](monitor-event-delivery.md).
* Zie voor het aanpassen van de gebeurtenis Bezorgingsopties [beheren Event Grid bezorgingsinstellingen](manage-event-delivery.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).