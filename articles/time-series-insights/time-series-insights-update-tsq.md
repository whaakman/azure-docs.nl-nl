---
title: Azure Time Series Insights (preview) gegevens opvragen | Microsoft Docs
description: Azure Time Series Insights (preview) gegevens uitvoeren van query 's
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 00ef6eed23d1645320c28123d6670230cdd725c9
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964914"
---
# <a name="data-querying"></a>Query's uitvoeren op gegevens

Azure Time Series Insights (TSI) kunt gegevens uitvoeren van query's op gebeurtenissen en metagegevens die zijn opgeslagen in de omgeving via openbare surface API's. Deze API's worden ook gebruikt in de [TSI-Verkenner](./time-series-insights-update-explorer.md).

Er zijn drie primaire categorieën van de API die beschikbaar in Azure TSI zijn:

* Omgeving API's kunnen uitvoeren van query's de TSI-omgeving zelf, zoals de lijst met omgevingen aanroeper heeft toegang tot de metagegevens van de omgeving, enzovoort.

* Time series model-query (TSM-Q) API's inschakelen maken, lezen, bijwerken en verwijderen van bewerkingen voor metagegevens die zijn opgeslagen in de omgeving van de time series-model. Als exemplaren, typen, hiërarchieën, enz.

* Time series-query (TSQ) API's inschakelen voor het ophalen van gegevens van statuswijzigingsgebeurtenissen van de provider van de gegevensbron is geregistreerd, of bewerkingen voor het transformeren, combineren en uitvoeren van berekeningen op time series-gegevens kunt uitvoeren.

De [Time Series-expressietaal](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) (TSX) is een krachtige, vierde, categorie. Time Series modellen (TSM) wordt gebruikt om in te schakelen van de samenstelling van de geavanceerde berekening.

## <a name="azure-time-series-insights-core-apis"></a>Azure Time Series Insights core API 's

Hieronder vindt u de core API's wordt ondersteund.

![tsq][1]

### <a name="the-environment-apis"></a>De omgeving-API 's

Hier volgen de beschikbare omgeving API's:

* [Ophalen van de omgeving API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): de lijst met omgevingen retourneert die de aanroeper is geautoriseerd voor toegang tot.
* [Ophalen van de omgeving beschikbaarheid API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): de distributie van het aantal gebeurtenissen geretourneerd via de tijdstempel van de gebeurtenis `$ts`. Deze API kunt u bepalen of er geen gebeurtenissen in het tijdstempel zijn door het aantal gebeurtenissen als bestaan.
* [Ophalen van de gebeurtenis Schema API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): retourneert de metagegevens van de gebeurtenis schema voor een reeks opgegeven zoekopdracht. Deze API helpt bij het ophalen van alle metagegevens/eigenschappen die beschikbaar zijn in het schema voor de opgegeven zoekopdracht-reeks.

### <a name="time-series-model-query-tsm-q-apis"></a>Time Series Model-Query (TSM-Q) API 's

Hier volgen de tijd reeks Model-Query-API's beschikbaar:

* [Instellingen API model](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): kunt ophalen en hierop patches toepassen op het type en de naam van het model van de omgeving.
* [API van het type](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): CRUD kunnen op de typen van de Time-Series en hun bijbehorende variabelen.
* [Hiërarchieën API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): kunt CRUD op Time Series-hiërarchieën en hun bijbehorende veld paden.
* [API-exemplaren](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): kunt CRUD op Time Series-instanties en de bijbehorende velden bijbehorende exemplaar.

### <a name="the-time-series-query-tsq-apis"></a>De Time Series Query (TSQ) API 's

Hier volgen de Time Series Query beschikbare API's:

* [Ophalen van gebeurtenissen API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): de ophalen gebeurtenissen-API kunt query's en voor het ophalen van gegevens van TSI van gebeurtenissen, zoals ze worden geregistreerd in Azure TSI van de provider van de gegevensbron.

* [Ophalen van de serie API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): Hiermee query's en voor het ophalen van gegevens van de Azure TSI van vastgelegde gebeurtenissen door gebruik te maken van gegevens die zijn geregistreerd op de kabel met behulp van de variabelen definiëren in het model of de opgegeven inline.

    >[!NOTE]
    > De aggregatie-component is genegeerd, zelfs als opgegeven in een model of inline opgegeven.

  De reeks ophalen API retourneert een TSV (tijd reeks waarde van een indeling TSI wordt gebruikt voor JSON-uitvoer van de query) voor elke variabele voor elk interval, op basis van de opgegeven **Time Series-ID** en de set beschikbare variabelen.

* [Reeks API aggregeren](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): Hiermee query's en voor het ophalen van gegevens van TSI van vastgelegde gebeurtenissen door steekproeven en aggregeren van gegevens vastgelegd.

  De totale reeks-API retourneert een TSV voor elke variabele voor elk interval, op basis van de opgegeven **Time Series-ID** en de set beschikbare variabelen. De totale reeks API realiseert verminderen door gebruik te maken van variabelen die zijn opgeslagen in TSM of inline aggregate of voorbeeld gegevens opgegeven.

  Cumulatieve typen ondersteund: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Volgende stappen

Lees de [opslag van Azure TSI (Preview) en Ingress](./time-series-insights-update-storage-ingress.md).

Meer informatie over [gegevensmodellering](./time-series-insights-update-tsm.md).

Meer informatie over [aanbevolen procedures bij het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/tsq.png
