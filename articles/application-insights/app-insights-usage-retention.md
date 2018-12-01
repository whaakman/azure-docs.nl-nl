---
title: Analyse van de gebruikers bewaren voor web-apps met Azure Application Insights | Microsoft docs
description: Hoeveel gebruikers terugkomen naar uw app?
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/03/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 85fd0131da6d67cb8001c4b3de25af0c4575a64d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721983"
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Analyse van de gebruikers bewaren voor web-apps met Application Insights

De functie bewaren in [Azure Application Insights](app-insights-overview.md) helpt u hoeveel gebruikers terug naar uw app, en hoe vaak ze specifieke taken uitvoeren of bereiken van de doelstellingen analyseren. Als u een game site uitvoert, kan u bijvoorbeeld de nummers van gebruikers die naar de site terugkeert na het verlies van een spel met het nummer en nadat de winnende retourneren vergelijken. Aan de hand van deze kennis kunt u zowel uw gebruikerservaring en uw strategie voor zakelijke verbeteren.

## <a name="get-started"></a>Aan de slag

Als er geen gegevens in het programma retentie in de Application Insights-portal nog [meer informatie over het aan de slag met de hulpprogramma's voor gebruik](app-insights-usage-overview.md).

## <a name="the-retention-tool"></a>Het programma retentie

![Retentie-informatie](./media/app-insights-usage-retention/retention.png)

1. De werkbalk kan gebruikers nieuwe bewaarperiode rapporten maken, bestaande retentie rapporten openen, huidige bewaarperiode rapport opslaan of opslaan als, wijzigingen aangebracht in de opgeslagen rapporten ongedaan maken, gegevens in het rapport vernieuwen, rapport via e-mail of een directe koppeling delen en toegang tot de documentatie de pagina. 
2. Standaard toont de bewaarperiode alle gebruikers die u hebt vervolgens zijn teruggekomen en hebt iets anders gedurende een periode. U kunt andere combinatie van gebeurtenissen naar toespitsen op specifieke gebruikersactiviteiten selecteren.
3. Voeg een of meer filters op Eigenschappen. Bijvoorbeeld, kunt u zich richten op gebruikers in een bepaald land of regio. Klik op **Update** na het instellen van de filters. 
4. De grafiek met totale retentie bevat een overzicht van gebruikersretentie in de geselecteerde periode. 
5. Het raster bevat het aantal gebruikers op basis van de opbouwfunctie voor query's in #2 wordt bewaard. Elke rij vertegenwoordigt een cohort van gebruikers die een gebeurtenis in de weergegeven tijd uitgevoerd. Elke cel in de rij laat zien hoe vaak deze cohort geretourneerd ten minste één keer in een latere periode. Sommige gebruikers kunnen in meer dan één punt retourneren. 
6. De insights-kaarten weergeven top vijf initiërende gebeurtenissen en de top vijf geretourneerde gebeurtenissen, zodat gebruikers een beter begrip van hun rapport bewaren. 

![Aanwijzen met de muis bewaren](./media/app-insights-usage-retention/hover.png)

Gebruikers kunnen Beweeg de muisaanwijzer over de cellen in het programma retentie voor toegang tot de analytics-knop en knopinfo zijn waarin wordt uitgelegd wat de cel betekent. De knop Analytics leidt gebruikers naar het Analytics-hulpprogramma met een vooraf gevulde query voor het genereren van gebruikers van de cel. 

## <a name="use-business-events-to-track-retention"></a>Gebruik zakelijke gebeurtenissen bijhouden bewaren

Als u de handigste retentie-analyse, meten gebeurtenissen die staan voor belangrijke zakelijke activiteiten. 

Veel gebruikers mogelijk een pagina bijvoorbeeld geopend in uw app zonder het spel dat wordt weergegeven. Alleen de paginaweergaven bijhouden, zou een onjuiste schatting van hoeveel mensen keer terug naar het spel spelen na eerder genieten daarom bieden. Als u een helder beeld van de spelers, moet uw app een aangepaste gebeurtenis wanneer een gebruiker daadwerkelijk wordt afgespeeld verzenden.  

Het is raadzaam om Codeer aangepaste gebeurtenissen die staan voor belangrijke zakelijke acties en gebruik deze voor uw analyse bewaren. Voor het vastleggen van het spel resultaat, moet u een regel code voor het verzenden van een aangepaste gebeurtenis naar Application Insights schrijven. Als u dit in de code van de webpagina's of in Node.JS schrijven, wordt deze ziet er als volgt uit:

```JavaScript
    appinsights.trackEvent("won game");
```

Of in de servercode ASP.NET:

```csharp
   telemetry.TrackEvent("won game");
```

[Meer informatie over het schrijven van aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)


