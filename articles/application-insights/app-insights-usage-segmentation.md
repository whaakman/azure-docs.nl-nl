---
title: Gebruiker, sessie en gebeurtenis analyse in Azure Application Insights | Microsoft docs
description: Demografische analyse van de gebruikers van uw web-app.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 16795ce1fd53b9634dbc2ff7513d1c39752b8848
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53974587"
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analyse van gebruikers, sessies en gebeurtenissen in Application Insights

Ontdek wanneer mensen uw web-app, welke pagina's die ze zijn het meest geïnteresseerd in, waarbij uw gebruikers zich bevinden, en welke browsers en besturingssystemen die ze gebruiken. Bedrijven en gebruikstelemetrie analyseren met behulp van [Azure Application Insights](app-insights-overview.md).

![Schermopname van Application Insights-gebruikers](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>Aan de slag

Als er geen gegevens in de gebruikers, sessies of gebeurtenissen blades in de Application Insights-portal nog [meer informatie over het aan de slag met de hulpprogramma's voor gebruik](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Het hulpprogramma van de segmentering van gebruikers, sessies en gebeurtenissen

Drie van de blades gebruik hetzelfde hulpprogramma gebruiken om te opdelen telemetrie van uw web-app uit drie perspectieven. Door te filteren en de gegevens worden gesplitst, kunt u inzichten over het relatieve gebruik van verschillende pagina's en onderdelen.

* **Hulpprogramma gebruikers**: Hoeveel mensen uw app en de functies gebruikt.  Gebruikers worden met behulp van anonieme id's die zijn opgeslagen in de browsercookies geteld. Gebruik van verschillende browsers of machines één persoon wordt geteld als meer dan één gebruiker.
* **Programma sessies**: Het aantal sessies van gebruikersactiviteit hebt opgenomen bepaalde pagina's en functies van uw app. Een sessie wordt geteld na een halfuur van inactiviteit van de gebruiker, of na 24 uur continu gebruik.
* **Gebeurtenissen hulpprogramma**: Hoe vaak bepaalde pagina's en functies van uw app worden gebruikt. De weergave van een pagina wordt geteld als een pagina in een browser wordt geladen in uw app, mits u [geïnstrumenteerd deze](../azure-monitor/app/javascript.md). 

    Een aangepaste gebeurtenis vertegenwoordigt één exemplaar van dat er iets gebeurt in uw app, vaak een tussenkomst van de gebruiker, zoals een knop klikt u op of de voltooiing van een taak. U code invoegen in uw app [aangepaste gebeurtenissen genereren](../azure-monitor/app/api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Query's uitvoeren voor bepaalde gebruikers

Verken de verschillende groepen gebruikers door de opties voor query's aan de bovenkant van het hulpprogramma gebruikers aan te passen:

* Weergeven: Kies een cohort van gebruikers om te analyseren.
* Die gebruik hebben gemaakt: Aangepaste gebeurtenissen en paginaweergaven kiezen.
* Tijdens de: Kies een tijdsbereik.
* Door: Kiezen hoe u aan de bucket van de gegevens door een bepaalde periode of door een ander domein zoals browser of een plaats.
* Splitsen op basis van: Kies een eigenschap waarmee te splitsen of een segment de gegevens. 
* Filters toevoegen: De query beperken tot bepaalde gebruikers, sessies of gebeurtenissen op basis van hun eigenschappen, zoals de browser of een plaats. 
 
## <a name="saving-and-sharing-reports"></a>Opslaan en delen van rapporten 
U kunt gebruikers rapporten, exclusief voor u in de sectie Mijn rapporten of gedeeld met iedereen met toegang tot deze Application Insights-resource in de sectie gedeelde rapporten opslaan.

Voor het delen van een koppeling naar een rapport gebruikers, sessies of gebeurtenissen. Klik op **Share** in de werkbalk, kopieer vervolgens de koppeling.

Voor het delen van een kopie van de gegevens in een rapport gebruikers, sessies of gebeurtenissen. Klik op **Share** in de werkbalk en klik op de **Word pictogram** te maken van een Word-document met de gegevens. Of klik op de **Word pictogram** boven de belangrijkste grafiek.

## <a name="meet-your-users"></a>Voldoen aan uw gebruikers

De **voldoen aan uw gebruikers** sectie wordt informatie weergegeven over vijf voorbeeldgebruikers overeenkomt met de huidige query. Overweegt en verkennen van het gedrag van personen, naast de statistische functies, krijgt u inzicht in hoe mensen uw app daadwerkelijk gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)