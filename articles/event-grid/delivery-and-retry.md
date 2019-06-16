---
title: Azure Event Grid-levering en probeer het opnieuw
description: Hierin wordt beschreven hoe Azure Event Grid gebeurtenissen biedt en hoe deze omgaat met niet-bezorgde berichten.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: spelluru
ms.openlocfilehash: b4bfdd3e9cdf99314dc55907ba163adc6cd39423
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952881"
---
# <a name="event-grid-message-delivery-and-retry"></a>Levering van berichten van Event Grid en probeer het opnieuw

Dit artikel wordt beschreven hoe Azure Event Grid omgaat met gebeurtenissen bij levering is niet bevestigd.

Event Grid biedt duurzame levering. Het biedt een elk bericht ten minste eenmaal voor elk abonnement. Gebeurtenissen worden onmiddellijk verzonden naar het eindpunt van de geregistreerde van elk abonnement. Als een eindpunt niet de ontvangst van een gebeurtenis bevestigt, pogingen Event Grid levering van de gebeurtenis.

Op dit moment Event Grid elke gebeurtenis afzonderlijk naar abonnees worden verzonden. De abonnee ontvangt een matrix met een eenmalige gebeurtenis.

## <a name="retry-schedule-and-duration"></a>Schema voor nieuwe pogingen en de duur

Event Grid wacht 30 seconden voor een reactie na het leveren van een bericht. Na 30 seconden, als het eindpunt nog niet is gereageerd, het bericht is in de wachtrij voor opnieuw proberen. Event Grid maakt gebruik van een beleid voor exponentieel uitstel opnieuw proberen voor de bezorging van gebeurtenissen. Event Grid pogingen levering op het volgende schema beste vermogen:

- 10 seconden
- 30 seconden
- 1 minuut
- 5 minuten
- 10 minuten
- 30 minuten
- 1 uur
- Per uur voor maximaal 24 uur

Als het eindpunt binnen drie minuten reageert, Event Grid wordt geprobeerd de gebeurtenis verwijderen uit de wachtrij voor nieuwe pogingen op beste vermogen maar duplicaten kunnen nog steeds worden ontvangen.

Event Grid een kleine willekeurige toegevoegd aan alle stappen van de nieuwe pogingen en kunt u alleen bepaalde nieuwe pogingen overslaan als een eindpunt consistent beschadigd, omlaag gedurende een lange periode is of lijkt te worden geconfronteerd.

Voor het gedrag van deterministische, stelt time to live gebeurtenis en maximale levering probeert de [beleid voor opnieuw proberen abonnement](manage-event-delivery.md).

Event Grid verloopt standaard alle gebeurtenissen die niet worden bezorgd binnen 24 uur. U kunt [aanpassen van het beleid voor opnieuw proberen](manage-event-delivery.md) bij het maken van een gebeurtenisabonnement. U opgeven dat het maximum aantal bezorgingspogingen (de standaardwaarde is 30) en de gebeurtenis time-to-live (standaard is 1440 minuten).

## <a name="dead-letter-events"></a>Dead-letter uitvoeren voor gebeurtenissen

Wanneer een gebeurtenis kan niet van Event Grid leveren, kan de niet-bezorgde gebeurtenis verzenden naar een opslagaccount. Dit proces staat bekend als onbestelbare. Standaard inschakelen Event Grid onbestelbare niet. Als u wilt inschakelen, moet u een opslagaccount voor niet-bezorgde gebeurtenissen bij het maken van het gebeurtenisabonnement. U gebeurtenissen van dit opslagaccount wordt gebruikt om op te lossen leveringen op te halen.

Wanneer u probeert alle van de nieuwe pogingen, Event Grid een gebeurtenis verzendt naar de dead-letter-locatie. Als de Event Grid een 400 (ongeldige aanvraag) of 413 (aanvragen aanvraagentiteit te groot) antwoordcode ontvangt, wordt onmiddellijk de gebeurtenis verzonden naar de dead-letter-eindpunt. Deze responscodes geven aan de bezorging van de gebeurtenis nooit slaagt.

Er is een vertraging van vijf minuten tussen de laatste poging tot het leveren van een gebeurtenis en wanneer deze naar de dead-letter-locatie is geleverd. Deze vertraging is bedoeld om te beperken van het aantal bewerkingen van de Blob-opslag. Als de locatie van de dead-letter uitvoeren voor de vier uur niet beschikbaar is, wordt de gebeurtenis is verwijderd.

Voordat u de locatie van de dead-letter uitvoeren, moet u een opslagaccount met een container hebt. U kunt het eindpunt voor deze container opgeven bij het maken van het gebeurtenisabonnement. Het eindpunt is in de indeling van: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Het is raadzaam om te worden geïnformeerd wanneer een gebeurtenis is verzonden naar de dead-letter-locatie. Event Grid gebruiken om te reageren op gebeurtenissen die door niet-bezorgde [een gebeurtenisabonnement maken](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) voor de dead-letter uitvoeren voor blob-opslag. Telkens wanneer de dead-letter uitvoeren voor blob-opslag een niet-bezorgde gebeurtenis ontvangt, meldt Event Grid de handler. De handler reageert met de acties die u wilt deelnemen aan voor het afhandelen van niet-bezorgde gebeurtenissen.

Zie voor een voorbeeld van het instellen van een locatie dead-letter [dode letter en beleid voor opnieuw proberen](manage-event-delivery.md).

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
- 413 Aanvraagentiteit te groot
- 414 URI te lang
- 429 te veel aanvragen
- 500 Interne serverfout
- 503 Service niet beschikbaar
- 504 Time-out van gateway

## <a name="next-steps"></a>Volgende stappen

* De status van de gebeurtenis leveringen, Zie [Monitor Event Grid berichtbezorging](monitor-event-delivery.md).
* Zie voor het aanpassen van de gebeurtenis Bezorgingsopties [dode letter en beleid voor opnieuw proberen](manage-event-delivery.md).