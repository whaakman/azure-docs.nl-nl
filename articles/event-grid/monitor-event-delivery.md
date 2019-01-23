---
title: Levering van Azure Event Grid berichten bewaken
description: Beschrijft hoe u voor het bewaken van de bezorging van berichten van de Azure Event Grid.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: spelluru
ms.openlocfilehash: fdd18b833794c25cb90188ba8bc418d4785492ba
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464802"
---
# <a name="monitor-event-grid-message-delivery"></a>Levering van berichten van Event Grid bewaken 

In dit artikel wordt beschreven hoe u de portal gebruiken om te zien van de status van de gebeurtenis leveringen.

Event Grid biedt duurzame levering. Het biedt een elk bericht ten minste eenmaal voor elk abonnement. Gebeurtenissen worden onmiddellijk verzonden naar de geregistreerde webhook van elk abonnement. Als een webhook niet Bevestig de ontvangst van een gebeurtenis binnen 60 seconden na de eerste poging voor de levering, pogingen Event Grid levering van de gebeurtenis.

Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Levering van metrische gegevens

De portal wordt weergegeven metrische gegevens voor de status van het leveren van gebeurtenisberichten.

Onderwerpen worden de metrische gegevens:

* **Publicatie is voltooid**: Gebeurtenis is verzonden naar het onderwerp en verwerkt met een 2xx-antwoord.
* **Publiceren is mislukt**: Gebeurtenis verzonden naar het onderwerp, maar geweigerd met een foutcode.
* **Niet-overeenkomende**: Gebeurtenis is gepubliceerd naar het onderwerp, maar niet overeenkomen met een gebeurtenisabonnement. De gebeurtenis is verwijderd.

Voor abonnementen worden de metrische gegevens:

* **Levering is voltooid**: Gebeurtenis is geleverd aan het eindpunt van het abonnement en 2xx antwoord ontvangen.
* **Levering mislukt**: Gebeurtenis verzonden naar het eindpunt van het abonnement, maar 4xx of 5xx antwoord ontvangen.
* **Gebeurtenissen verlopen**: Gebeurtenis is niet bezorgd en alle nieuwe pogingen zijn verzonden. De gebeurtenis is verwijderd.
* **Overeenkomende gebeurtenissen**: Gebeurtenis in het onderwerp overeen met het gebeurtenisabonnement.

## <a name="event-subscription-status"></a>Status van abonnement

Overzicht van metrische gegevens voor een gebeurtenisabonnement, kunt u ofwel zoeken door het abonnementstype of abonnementen voor een specifieke resource.

Selecteer om te zoeken op abonnement gebeurtenistype, **alle services**.

![Alle services selecteren](./media/monitor-event-delivery/all-services.png)

Zoeken naar **gebeurtenisraster** en selecteer **Event Grid-abonnementen** uit de beschikbare opties.

![Zoeken naar gebeurtenisabonnementen](./media/monitor-event-delivery/search-and-select.png)

Filteren op het type gebeurtenis, het abonnement en locatie. Selecteer **metrische gegevens** voor het abonnement om weer te geven.

![Abonnementen filteren](./media/monitor-event-delivery/filter-events.png)

Bekijk de metrische gegevens voor het onderwerp over de gebeurtenis en het abonnement.

![Metrische gegevens weergeven-gebeurtenis](./media/monitor-event-delivery/subscription-metrics.png)

U vindt de metrische gegevens voor een specifieke resource, selecteert u die bron. Selecteer **gebeurtenissen**.

![Gebeurtenissen voor een resource selecteren](./media/monitor-event-delivery/select-events.png)

Ziet u de metrische gegevens voor abonnementen voor die bron.

## <a name="custom-event-status"></a>Status van het aangepaste gebeurtenis

Als u een aangepast onderwerp hebt gepubliceerd, kunt u de metrische gegevens weergeven voor deze. Selecteer de resourcegroep voor het onderwerp en selecteert u het onderwerp.

![Aangepaste onderwerp selecteren](./media/monitor-event-delivery/select-custom-topic.png)

Bekijk de metrische gegevens voor het onderwerp van het aangepaste gebeurtenis.

![Metrische gegevens weergeven-gebeurtenis](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
