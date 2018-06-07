---
title: Azure Event raster berichtbezorging bewaken
description: Beschrijft hoe de bezorging van Azure Event raster berichten bewaken.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: tomfitz
ms.openlocfilehash: 625f3e228bb28c85e68fb592914fb2191baf3e4e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626986"
---
# <a name="monitor-event-grid-message-delivery"></a>Gebeurtenis raster berichtbezorging bewaken 

In dit artikel wordt beschreven hoe de portal gebruiken om de status van de gebeurtenis leveringen te bekijken.

Gebeurtenis raster bevat duurzame levering. Elk bericht ten minste eenmaal voor elk abonnement levert. Gebeurtenissen worden verzonden naar de geregistreerde webhook van elk abonnement onmiddellijk. Als een webhook niet Bevestig de ontvangst van een gebeurtenis binnen 60 seconden na de eerste poging voor levering, opnieuw gebeurtenis raster levering van de gebeurtenis.

Voor informatie over de levering van de gebeurtenis en nieuwe pogingen, [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Levering metrische gegevens

De portal wordt weergegeven metrische gegevens voor de status van het leveren van gebeurtenisberichten.

Voor onderwerpen zijn de metrische gegevens:

* **Publiceren is voltooid**: gebeurtenis is verzonden naar het onderwerp en verwerkt met een antwoord 2xx.
* **Publiceren mislukt**: gebeurtenis verzonden naar het onderwerp, maar geweigerd met een foutcode.
* **Niet-overeenkomende**: gebeurtenis naar het onderwerp gepubliceerd, maar niet overeenkomt met een gebeurtenisabonnement. De gebeurtenis is verwijderd.

Voor abonnementen zijn de metrische gegevens:

* **Levering geslaagd**: gebeurtenis is geleverd aan het eindpunt van het abonnement en 2xx antwoord ontvangen.
* **Levering mislukt**: gebeurtenis verzonden naar het eindpunt van abonnement, maar geen 4xx of 5xx-antwoord ontvangen.
* **Gebeurtenissen verlopen**: gebeurtenis is niet bezorgd en alle nieuwe pogingen is verzonden. De gebeurtenis is verwijderd.
* **Komt overeen met gebeurtenissen**: gebeurtenis in het onderwerp is overeenkomt met het gebeurtenisabonnement.

## <a name="event-subscription-status"></a>De status van de gebeurtenis-abonnement

Overzicht van metrische gegevens voor een gebeurtenisabonnement, kunt u ofwel door abonnementstype of abonnementen voor een specifieke bron zoeken.

Selecteer om te zoeken op abonnement gebeurtenistype, **alle services**.

![Selecteer alle services](./media/monitor-event-delivery/all-services.png)

Zoeken naar **gebeurtenis raster** en selecteer **gebeurtenis raster abonnementen** uit de beschikbare opties.

![Zoek naar gebeurtenisabonnementen](./media/monitor-event-delivery/search-and-select.png)

Filteren op het type gebeurtenis, het abonnement en locatie. Selecteer **metrische gegevens** voor het abonnement om weer te geven.

![Abonnementen voor gebeurtenissen filteren](./media/monitor-event-delivery/filter-events.png)

De metrische gegevens voor het onderwerp over de gebeurtenis en een abonnement weergeven.

![Metrische gegevens weergeven gebeurtenis](./media/monitor-event-delivery/subscription-metrics.png)

Ga voor de metrische gegevens voor een specifieke bron, bron te selecteren. Selecteer **gebeurtenissen**.

![Gebeurtenissen voor een bron selecteren](./media/monitor-event-delivery/select-events.png)

U ziet de metrische gegevens voor abonnementen voor die bron.

## <a name="custom-event-status"></a>Status van de aangepaste gebeurtenis

Als u een aangepaste onderwerp hebt gepubliceerd, kunt u de metrische gegevens weergeven voor. Selecteer de resourcegroep voor het onderwerp en het onderwerp selecteren.

![Aangepaste onderwerp selecteren](./media/monitor-event-delivery/select-custom-topic.png)

De metrische gegevens voor de aangepaste gebeurtenisonderwerp weergeven.

![Metrische gegevens weergeven gebeurtenis](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de levering van de gebeurtenis en nieuwe pogingen, [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
