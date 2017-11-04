---
title: Azure Application Insights voor ASP.NET Core | Microsoft Docs
description: Bewaken van webtoepassingen voor beschikbaarheid, prestaties en het gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 74f99dd6f31ecff7c838d8f710a7fe4279ce0ea9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights voor ASP.NET Core
[Application Insights](app-insights-overview.md) kunt u uw webtoepassing voor beschikbaarheid, prestaties en gebruik te bewaken. Op basis van de feedback die u krijgt over de prestaties en de effectiviteit van uw app tijdens het gebruik, kunt u weldoordachte beslissingen nemen over de richting van het ontwerp in elke fase van de ontwikkelingslevenscyclus.

![Voorbeeld](./media/app-insights-asp-net-core/sample.png)

U hebt een abonnement met [Microsoft Azure](http://azure.com). Meld u aan met een Microsoft-account, dat u mogelijk al hebt voor Windows, XBox Live of andere Microsoft-cloudservices. Uw team wellicht een organisatie-abonnement op Azure: vraag de eigenaar u toevoegen met behulp van uw Microsoft-account.

## <a name="getting-started"></a>Aan de slag

* In Visual Studio Solution Explorer met de rechtermuisknop op uw project en selecteer **Configure Application Insights**, of **toevoegen > Application Insights**. [Meer informatie](app-insights-asp-net.md).
* Als u deze opdrachten niet ziet, volgt u de [handmatige aan de slag](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Wellicht moet u dit doen als uw project is gemaakt met een versie van Visual Studio voordat 2017.

## <a name="using-application-insights"></a>Application Insights gebruiken
Meld u aan bij de [Microsoft Azure-portal](https://portal.azure.com), selecteer **alle Resources** of **Application Insights**, en selecteer vervolgens de resource die u hebt gemaakt om het bewaken van uw app.

Gebruik uw app een tijdje in een apart browservenster. Hier ziet u gegevens verschijnen in de Application Insights-grafieken. (U wellicht klikt u op vernieuwen.) Er is een kleine hoeveelheid gegevens terwijl u ontwikkelt, maar deze grafieken echt tot leven komen wanneer u uw app publiceren en veel gebruikers hebben. 

De overzichtspagina toont de prestatie-grafieken: serverreactietijd, paginalaadtijd en tellingen van mislukte aanvragen. Klik op een grafiek om meer grafieken en gegevens te bekijken.

Weergaven in de portal kunnen worden onderverdeeld in drie hoofdcategorieën:

* [Metrics Explorer](app-insights-metrics-explorer.md) grafieken en tabellen van de metrische gegevens en aantallen zoals reactietijden, uitvalpercentage of met zelfgemaakt metrische gegevens ziet de [API](app-insights-api-custom-events-metrics.md). Filteren en de gegevens door de waarden van eigenschappen voor een beter inzicht in uw app en de gebruikers te segmenteren.
* [Zoek Explorer](app-insights-diagnostic-search.md) geeft een lijst van afzonderlijke gebeurtenissen, zoals een specifieke aanvragen, uitzonderingen, logboektraceringen of gebeurtenissen die u zelf met gemaakt de [API](app-insights-api-custom-events-metrics.md). Filteren en zoeken in de gebeurtenissen en navigeren tussen gerelateerde gebeurtenissen om problemen te onderzoeken.
* [Analytics](app-insights-analytics.md) kunt u SQL-achtige query's uitvoeren via uw Telemetrie en is een krachtig analytische en diagnostische hulpprogramma.

## <a name="alerts"></a>Waarschuwingen
* Automatisch [proactieve diagnostische waarschuwingen](app-insights-proactive-diagnostics.md) waarmee wordt aangegeven dat u over afwijkende wijzigingen in uitvalpercentage en andere metrische gegevens.
* Instellen van [beschikbaarheidstests](app-insights-monitor-web-app-availability.md) voor het testen van uw website voortdurend vanaf locaties wereldwijd en e-mailberichten krijgen zodra een mislukt test.
* Instellen van [metrische waarschuwingen](app-insights-monitor-web-app-availability.md) weten als metrische gegevens zoals reactietijden of uitzondering tarieven buiten acceptabele grenzen gaat.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Open source
[Lees- en bijdragen aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Volgende stappen
* [Voeg telemetrie toe aan uw webpagina's](app-insights-javascript.md) monitor pagina informatie over het gebruik en prestaties.
* [Afhankelijkheden bewaken](app-insights-asp-net-dependencies.md) om te zien als REST, SQL of andere externe bronnen u vertragen.
* [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een meer gedetailleerde weergave van de prestaties en het gebruik van uw app.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) controleren van uw app continu uit de hele wereld. 

