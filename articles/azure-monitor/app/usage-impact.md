---
title: Azure Application Insights gebruik Impact | Microsoft docs
description: Analyseren hoe verschillende eigenschappen mogelijk van invloed zijn op conversieverhoudingen van onderdelen van uw apps.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/25/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 023793cc699849eec4e1c6d8cbe08259d984ac8d
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052867"
---
# <a name="impact-analysis-with-application-insights"></a>Impactanalyse met Application Insights

Impact analyseren hoe laadtijden en andere eigenschappen van invloed zijn op conversieverhoudingen van verschillende onderdelen van uw app. Voor nauwkeuriger wordt geplaatst, worden ontdekt hoe **elke dimensie** van een **paginaweergave**, **aangepaste gebeurtenis**, of **aanvraag** is van invloed op het gebruik van een verschillende **paginaweergave** of **aangepaste gebeurtenis**. 

![Hulpprogramma voor impact](./media/usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Nog steeds niet zeker wat gevolgen voor moet doet?

Een manier om na te denken van de Impact is als de ultieme hulpprogramma voor het vereffenen van argumenten met iemand in uw team over hoe traagheid in een bepaald aspect van uw site is die betrekking hebben op of gebruikers blijven. Hoewel gebruikers een bepaalde hoeveelheid traagheid tolereren kunnen, kunt u Impact inzicht in de beste manier om te verdelen optimalisatie en prestaties te maximaliseren gebruikersconversie.

Maar alleen een subset van de mogelijkheden van de Impact analyseren van prestaties is. Aangezien Impact biedt ondersteuning voor aangepaste gebeurtenissen en dimensies, zijn beantwoorden van vragen zoals hoe is de keuze van de gebruiker-browser gerelateerd met verschillende snelheden van conversie slechts een paar muisklikken.

![Schermafbeelding conversie door browsers](./media/usage-impact/0004-browsers.png)

> [!NOTE]
> Uw Application Insights-resource moet paginaweergaven of aangepaste gebeurtenissen met het hulpprogramma Impact bevatten. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](../../azure-monitor/app/javascript.md). Houd er rekening mee dat omdat u bij het analyseren van CORRELATIE, voorbeeld grootte is ook belangrijk.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Laadtijd van browserpagina beïnvloedt hoeveel mensen op de pagina Mijn converteren?

Kies een initiële paginaweergave, aangepaste gebeurtenis of aanvraag om te beginnen met het beantwoorden van vragen met het hulpprogramma Impact.

![Hulpprogramma voor impact](./media/usage-impact/0002-dropdown.png)

1. Selecteer de paginaweergave van een in de **voor de paginaweergave** vervolgkeuzelijst.
2. Laat de **analyseren hoe de** vervolgkeuzelijst op de standaardselectie van **duur** (In deze context **duur** is een alias voor **laadtijd van browserpagina**.)
3. Voor de **heeft gevolgen voor het gebruik van** vervolgkeuzelijst, selecteert u een aangepaste gebeurtenis. Deze gebeurtenis moet overeenkomen met een UI-element op de weergave van de pagina die u hebt geselecteerd in stap 1.

![Schermafbeelding van de resultaten](./media/usage-impact/0003-results.png)

In dit geval als **productpagina** laadtijd verhoogt de conversieverhouding te **aankoop-Product hebt geklikt** uitvalt. Op basis van de distributie hierboven, kan de duur van een optimale pagina laden van 3,5 seconden voor het bereiken van een mogelijke 55% conversieverhouding worden gericht. Verdere voor verbeterde prestaties verminderen laadtijd hieronder 3,5 seconden doen op dit moment niet correleert met de conversie van extra voordelen.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Wat gebeurt er als ik ben paginaweergaven bijhouden of tijden op aangepaste manieren laden?

Impact ondersteunt zowel standaard als aangepaste eigenschappen en metingen. Gebruik wat u wilt. Gebruik in plaats van de duur van filters op de primaire en secundaire gebeurtenissen om meer specifieke.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Converteer gebruikers uit verschillende landen of regio's met verschillende snelheden?

1. Selecteer de paginaweergave van een in de **voor de paginaweergave** vervolgkeuzelijst.
2. Kies 'Land of regio' **analyseren hoe de** vervolgkeuzelijst
3. Voor de **heeft gevolgen voor het gebruik van** vervolgkeuzelijst, selecteer een aangepaste gebeurtenis die overeenkomt met een UI-element op de paginaweergave u in stap 1 hebt gekozen.

In dit geval passen de resultaten niet meer in het model van een doorlopende x-as als in het eerste voorbeeld. In plaats daarvan wordt een vergelijkbaar met een gesegmenteerde trechter visualisatie weergegeven. Sorteren op **gebruik** om de variatie van de conversie naar uw aangepaste gebeurtenissen op basis van land weer te geven.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Hoe wordt het hulpprogramma gevolgen deze conversieverhoudingen berekend?

Achter de schermen, het hulpprogramma Impact is afhankelijk van de [Pearson correlatiecoëfficiënt](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Resultaten worden berekend tussen 1 en 1-1 die nul correlatie en 1 die vertegenwoordigt een positieve correlatie vertegenwoordigt.

De uitsplitsing van de basis van de werking van impactanalyse is als volgt:

Laat _A_ = de hoofdpagina weergave/aangepaste gebeurtenis/aanvraag u in de eerste vervolgkeuzelijst selecteert. (**Voor de paginaweergave**).

Laat _B_ = de secundaire pagina weergave/aangepaste gebeurtenis die u selecteert (**heeft gevolgen voor het gebruik van**).

Impact kijkt naar een voorbeeld van alle sessies van gebruikers in het geselecteerde tijdsbereik. Voor elke sessie, wordt er gezocht naar elk exemplaar van _A_.

Sessies vervolgens zijn onderverdeeld in twee verschillende soorten van _subsessions_ op basis van een van twee voorwaarden:

- Een geconverteerde koppeling bestaat uit een sessie eindigt met een _B_ gebeurtenis en omvat alle _A_ gebeurtenissen die vóór plaatsvinden _B_.
- Een niet-geconverteerde koppeling vindt plaats wanneer alle _A_de optreden, zonder een terminal _B_.

Hoe Impact uiteindelijk wordt berekend is afhankelijk van of we door metrische gegevens of dimensie analyseren. Voor alle metrische gegevens _A_het in een koppeling wordt het gemiddelde genomen. Terwijl afmetingen voor de waarde van elk _A_ bijdraagt _1/N_ op de waarde die is toegewezen aan _B_ waar _N_ is het aantal _A_de in de koppeling.

## <a name="next-steps"></a>Volgende stappen

- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Trechters](usage-funnels.md)
    - [Retentie](usage-retention.md)
    - [Gebruikersstromen](usage-flows.md)
    - [Werkmappen](../../azure-monitor/app/usage-workbooks.md)
    - [Gebruikerscontext toevoegen](usage-send-user-context.md)