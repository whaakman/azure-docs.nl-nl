---
title: Azure Application Insights gebruik Impact | Microsoft docs
description: Analyseren hoe verschillende eigenschappen mogelijk invloed conversie tarieven voor onderdelen van uw apps.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Impactanalyse met Application Insights

Impact analyseert hoe laadtijden en andere eigenschappen conversie tarieven voor verschillende onderdelen van uw app beïnvloeden. Als u deze preciezer, detecteert hoe **elke dimensie** van een **paginaweergave**, **aangepaste gebeurtenis**, of **aanvraag** is van invloed op het gebruik van een andere **paginaweergave** of **aangepaste gebeurtenis**. 

![Impact-hulpprogramma](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Nog niet zeker Impact betekenis?

Een manier om na te denken van de Impact is als de ultieme hulpprogramma voor het vereffenen van argumenten met iemand van uw team over hoe traagheid in bepaalde aspecten van uw site is die betrekking hebben op of gebruikers hangen. Hoewel gebruikers een bepaalde hoeveelheid traagheid tolereren kunnen, kunt u de Impact inzicht in de beste manier om te worden verdeeld optimalisatie en prestaties van de gebruiker conversie maximaliseren.

Maar prestaties analyseren is slechts een subset van de Impact van mogelijkheden. Aangezien Impact aangepaste gebeurtenissen en dimensies ondersteunt, zijn beantwoorden van vragen zoals hoe gebruiker browser keuze met verschillende snelheden van conversie correleren een paar muisklikken.

![Schermopname conversie door browsers](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> Uw Application Insights-resource moet bevatten paginaweergaven of aangepaste gebeurtenissen met het hulpprogramma Impact. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](app-insights-javascript.md). Houd ook rekening mee dat sindsdien analyseren van de correlatie voorbeeld grootte belangrijk is.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Is paginalaadtijd die invloed hebben op hoeveel mensen op de pagina converteren?

Kies een weergave van de eerste pagina, aangepaste gebeurtenis of aanvraag om te beginnen met het beantwoorden van vragen met het hulpprogramma Impact.

![Impact-hulpprogramma](./media/app-insights-usage-impact/0002-dropdown.png)

1. Selecteer de paginaweergave van een in de **voor de paginaweergave** vervolgkeuzelijst.
2. Laat de **analyseren hoe de** vervolgkeuzelijst op de standaardselectie van **duur** (In deze context **duur** is een alias voor **Paginalaadtijd**.)
3. Voor de **heeft impact op het gebruik van** vervolgkeuzelijst selecteert u een aangepaste gebeurtenis. Deze gebeurtenis moet overeenkomen met een UI-element op de weergave van de pagina die u hebt geselecteerd in stap 1.

![Schermopname van resultaten](./media/app-insights-usage-impact/0003-results.png)

In dit exemplaar als **productpagina** laadtijd verhoogt de conversie tot **Product aanschaffen geklikt** uitvalt. Op basis van de bovenstaande distributie, kan de duur van een optimale pagina laden van 3.5 seconden zodat een potentiële 55% conversieverhouding worden gericht. Verdere voor verbeterde prestaties verminderen laadtijd hieronder 3.5 seconden kunnen momenteel niet correleert met extra conversie voordelen.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Wat gebeurt er als ik ben paginaweergaven bijhouden of tijden op aangepaste manieren laden?

Impact ondersteunt zowel standaard als aangepaste eigenschappen en metingen. Wat u wilt gebruiken Gebruik in plaats van de duur van filters op de primaire en secundaire gebeurtenissen specifiekere ophalen.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Converteer gebruikers uit verschillende landen of regio's met verschillende snelheden?

1. Selecteer de paginaweergave van een in de **voor de paginaweergave** vervolgkeuzelijst.
2. Kies "Land of regio" in **analyseren hoe de** vervolgkeuzelijst
3. Voor de **heeft impact op het gebruik van** vervolgkeuzelijst een aangepaste gebeurtenis die overeenkomt met een UI-element op de paginaweergave u in stap 1 hebt gekozen.

In dit geval passen de resultaten niet langer in een continue x-as model zoals in het eerste voorbeeld. In plaats daarvan wordt een vergelijkbaar met een gesegmenteerde trechter visualisatie weergegeven. Sorteren op **gebruik** om weer te geven van de variatie aan van de conversie naar uw aangepaste gebeurtenis op basis van het land.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Hoe wordt het hulpprogramma voor Impact de tarieven van deze conversie berekend?

Achter de schermen het hulpprogramma Impact is afhankelijk van de [Pearson correlatie coefficient] (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Resultaten worden berekend tussen 1 en 1-1 die nul correlatie en 1 die vertegenwoordigt een positieve correlatie vertegenwoordigt.

De basic uitsplitsing van de werking van impactanalyse is als volgt:

Laat _A_ = de hoofdpagina weergeven/aangepaste gebeurtenis/aanvraag u de eerste vervolgkeuzelijst selecteert. (**Voor de paginaweergave**).

Laat _B_ = de secundaire pagina weergeven/aangepaste gebeurtenis die u selecteert (**heeft impact op het gebruik van**).

Impact gekeken naar een steekproef van alle sessies van gebruikers in de geselecteerde periode. Voor elke sessie, wordt er gezocht naar elk exemplaar van _A_.

Sessies vervolgens zijn onderverdeeld in twee verschillende typen van _subsessions_ op basis van twee omstandigheden:

- Een geconverteerde koppeling bestaat uit een sessie eindigt met een _B_ gebeurtenis en omvat alle _A_ gebeurtenissen die vóór plaatsvinden _B_.
- Een niet-geconverteerde koppeling Deze gebeurtenis treedt op wanneer alle _A_de optreden, zonder een terminal _B_.

Hoe Impact uiteindelijk wordt berekend afhankelijk van of we door metrische gegevens of dimensie analyseren. Voor alle metrieken _A_het in een koppeling gemiddelde wordt berekend. Dat dimensies voor de waarde van elk _A_ bijdraagt _1/N_ op de waarde die is toegewezen aan _B_ waar _N_ is het aantal _A_de in de koppeling.

## <a name="next-steps"></a>Volgende stappen

- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)