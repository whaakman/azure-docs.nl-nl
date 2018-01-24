---
title: Analyse van de gebruiker bewaren voor webtoepassingen met Azure Application Insights | Microsoft docs
description: Hoeveel gebruikers terug naar uw app?
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: 03f8bea65e319f357be4ca05fd4aa40da77a3436
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Analyse van de gebruiker bewaren voor webtoepassingen met Application Insights

De functie bewaren in [Azure Application Insights](app-insights-overview.md) helpt u hoeveel gebruikers terug naar uw app en hoe vaak ze bepaalde taken uitvoeren of bereiken van doelen analyseren. Als u een game site uitvoert, kan u bijvoorbeeld de aantallen gebruikers die terug naar de site na het verlies van een game met het nummer en nadat winnen retourneren vergelijken. Deze informatie kunt u zowel uw gebruikerservaring en de bedrijfsstrategie verbeteren.

## <a name="get-started"></a>Aan de slag

Als er geen gegevens in het hulpprogramma bewaren in de Application Insights-portal nog [informatie over het aan de slag met het gebruik van hulpprogramma's voor](app-insights-usage-overview.md).

## <a name="the-retention-tool"></a>Het hulpprogramma bewaren

![Retentie-informatie](./media/app-insights-usage-retention/retention.png)

1. De werkbalk kan gebruikers maken van nieuwe bewaren, bestaande bewaren rapporten openen, huidige bewaren rapport opslaan of niet opslaan omdat wijzigingen aangebracht in de opgeslagen rapporten terug te draaien, gegevens in het rapport te vernieuwen, rapport via e-mail of directe koppeling te delen en toegang de documentatie tot pagina. 
2. Standaard bevat bewaren alle gebruikers die u hebt vervolgens terug documentatie en iets anders heeft gedurende een periode. U kunt andere combinatie van gebeurtenissen naar toespitsen op specifieke gebruikersactiviteiten selecteren.
3. Een of meer filters toevoegen op Eigenschappen. Bijvoorbeeld, kunt u zich richten op gebruikers in een bepaald land of regio. Klik op **Update** na het instellen van de filters. 
4. De algehele bewaren grafiek bevat een overzicht van de gebruiker bewaren in de geselecteerde periode. 
5. Het raster toont het aantal gebruikers dat volgens de opbouwfunctie voor query's in #2 wordt bewaard. Elke rij vertegenwoordigt een cohort van gebruikers die een gebeurtenis in de weergegeven tijd uitgevoerd. Elke cel in de rij bevat ten minste eenmaal in een latere periode hoeveel die cohort geretourneerd. Sommige gebruikers mogelijk geretourneerd uit meer dan één periode. 
6. De kaarten insights weergeven bovenste vijf gang worden gezet en bovenste vijf geretourneerde gebeurtenissen om gebruikers een beter begrip van de retentie-rapport 

![Bewaartermijn muis aanwijzen](./media/app-insights-usage-retention/hover.png)

Gebruikers kunnen de muisaanwijzer cellen in het hulpprogramma bewaren voor toegang tot het analytics-knop en knopinfo zijn waarin wordt uitgelegd wat de cel betekent. De knop Analytics, kunnen gebruikers voor het Analytics-hulpprogramma met een vooraf ingestelde query voor het genereren van gebruikers van de cel. 

## <a name="use-business-events-to-track-retention"></a>Gebruik zakelijke gebeurtenissen bijhouden bewaren

Als u de nuttigste retentie-analyse, meten gebeurtenissen waarbij aanzienlijke zakelijke activiteiten. 

Bijvoorbeeld: veel gebruikers mogelijk open een pagina in uw app zonder het spel dat wordt weergegeven. Alleen de paginaweergaven bijhouden, zou een onnauwkeurige schatting maken van hoeveel mensen terugkeren om in te spelen na eerder genieten daarom bieden. Als u een duidelijk beeld van spelers retourneren, moet uw app een aangepaste gebeurtenis wanneer een gebruiker daadwerkelijk speelt verzenden.  

Het is raadzaam om aangepaste gebeurtenissen die belangrijke zakelijke acties vertegenwoordigt code en dit beleid gebruiken voor uw analyse bewaren. Voor het vastleggen van de game uitkomst, moet u een regel van een aangepaste gebeurtenis verzenden naar Application Insights-code schrijven. Als u deze in de code van de webpagina's of in Node.JS schrijft, als volgt uitziet:

```JavaScript
    appinsights.trackEvent("won game");
```

Of in de servercode ASP.NET:

```csharp
   telemetry.TrackEvent("won game");
```

[Meer informatie over het schrijven van aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)


