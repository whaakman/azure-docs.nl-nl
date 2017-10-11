---
title: Analyse van de gebruiker, sessie en gebeurtenissen in Azure Application Insights | Microsoft docs
description: Demografische analyse van gebruikers van uw web-app.
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
ms.author: bwren
ms.openlocfilehash: b154a01d1690bff4950ebc1ff5a5b89894d4d111
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analyse van de gebruikers, sessies en gebeurtenissen in Application Insights

Weten wanneer mensen uw web-app gebruikt, welke pagina's die ze zijn het meest geïnteresseerd, waar uw gebruikers zich bevinden, welke browsers en besturingssystemen die ze gebruiken. Bedrijfs- en gebruiksgegevens telemetrie analyseren met behulp van [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Aan de slag

Als er geen gegevens in de gebruikers, sessies of blades gebeurtenissen in de Application Insights-portal nog [informatie over het aan de slag met het gebruik van hulpprogramma's voor](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Het hulpprogramma van de segmentering van gebruikers, sessies en gebeurtenissen

Drie van de blades gebruik hetzelfde hulpprogramma gebruiken om te opdelen telemetrie van uw web-app uit drie perspectieven. Door het filteren en de gegevens worden gesplitst, kunt u inzicht in het relatieve gebruik van verschillende pagina's en onderdelen onthullen.

* **Gebruikers hulpprogramma**: hoeveel mensen uw app en de bijbehorende functies gebruikt.  Gebruikers worden met behulp van anonieme id's die zijn opgeslagen in de browsercookies geteld. Verschillende browsers of machines met één persoon wordt geteld als meer dan één gebruiker.
* **Sessies hulpprogramma**: het aantal sessies van gebruikersactiviteit bepaalde pagina's en functies van uw app hebt opgenomen. Een sessie worden geteld nadat half uur gebruiker inactief of nadat continue 24 uur van gebruik.
* **Gebeurtenissen hulpprogramma**: hoe vaak bepaalde pagina's en functies van uw app worden gebruikt. Een paginaweergave geteld als een pagina in een browser wordt geladen vanuit uw app, mits u [geïnstrumenteerd deze](app-insights-javascript.md). 

    Een aangepaste gebeurtenis vertegenwoordigt een exemplaar van dat er iets gebeurt in uw app, vaak een gebruikersinteractie, zoals een knop klikt u op of de voltooiing van een taak. Voeg in uw app code [aangepaste gebeurtenissen genereren](app-insights-api-custom-events-metrics.md#trackevent).

![Gebruik hulpprogramma](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Uitvoeren van query's voor bepaalde gebruikers 

Leer de verschillende groepen gebruikers door de query-opties aan de bovenkant van het hulpprogramma gebruikers aan te passen: 

* Wie gebruikt: Kies aangepaste gebeurtenissen en paginaweergaven. 
* Tijdens: Kies een tijdsbereik. 
* Door: Kiezen hoe de gegevens laden door een bepaalde periode of door een andere eigenschap zoals browser of een plaats. 
* Splitsen: Kies een eigenschap waarmee te splitsen of segment de gegevens. 
* Filters toevoegen: De query beperken tot bepaalde gebruikers, sessies of gebeurtenissen op basis van hun eigenschappen, zoals de browser of een plaats. 
 
## <a name="saving-and-sharing-reports"></a>Opslaan en delen van rapporten 
U kunt gebruikers rapporten, persoonlijke aan u in de sectie Mijn rapporten of gedeeld met iedereen met toegang tot deze Application Insights-resource in de sectie gedeelde rapporten anders opslaan.  
 
Tijdens het opslaan van een rapport of de eigenschappen bewerken, kiest u 'Actueel relatief tijdsbereik' voor het opslaan van een rapport wordt voortdurend vernieuwd gegevens, een vaste hoeveelheid tijd als u terugkeert.  
 
Kies 'Huidig absoluut tijdsbereik' een rapport opslaan met een vaste set van gegevens. Houd er rekening mee dat de gegevens in Application Insights alleen worden opgeslagen voor 90 dagen, dus als er meer dan 90 dagen zijn verstreken sinds een rapport met een absoluut tijdsbereik is opgeslagen, wordt het rapport leeg, weergegeven. 
 
## <a name="example-instances"></a>Voorbeeld-exemplaren

De voorbeeld-exemplaren in deze sectie bevat informatie over een aantal afzonderlijke gebruikers, sessies of gebeurtenissen die door de huidige query worden vergeleken. Overweegt en verkennen van het gedrag van personen naast statistische functies, krijgt u inzicht in hoe mensen uw app daadwerkelijk gebruiken. 
 
## <a name="insights"></a>Inzichten 

De zijbalk Insights bevat grote clusters van gebruikers die algemene eigenschappen voor delen. Deze clusters kunnen onthullen verrassend trends in hoe mensen uw app gebruiken. Als bijvoorbeeld 40% van alle over het gebruik van uw app afkomstig is van mensen met een enkele functie.  


## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)

