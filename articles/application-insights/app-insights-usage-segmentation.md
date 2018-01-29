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
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 1a5380cac08ab32cfea4cf457aed1fb1510099ed
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analyse van de gebruikers, sessies en gebeurtenissen in Application Insights

Nagaan wanneer mensen uw web-app welke pagina's die ze zijn het meest geïnteresseerd gebruiken in, waarbij uw gebruikers zich bevinden en welke browsers en besturingssystemen die ze gebruiken. Bedrijfs- en gebruiksgegevens telemetrie analyseren met behulp van [Azure Application Insights](app-insights-overview.md).

![Schermopname van Application Insights-gebruikers](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>Aan de slag

Als er geen gegevens in de gebruikers, sessies of blades gebeurtenissen in de Application Insights-portal nog [informatie over het aan de slag met het gebruik van hulpprogramma's voor](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Het hulpprogramma van de segmentering van gebruikers, sessies en gebeurtenissen

Drie van de blades gebruik hetzelfde hulpprogramma gebruiken om te opdelen telemetrie van uw web-app uit drie perspectieven. Door het filteren en de gegevens worden gesplitst, kunt u inzicht in het relatieve gebruik van verschillende pagina's en onderdelen onthullen.

* **Gebruikers hulpprogramma**: hoeveel mensen uw app en de bijbehorende functies gebruikt.  Gebruikers worden met behulp van anonieme id's die zijn opgeslagen in de browsercookies geteld. Verschillende browsers of machines met één persoon wordt geteld als meer dan één gebruiker.
* **Sessies hulpprogramma**: het aantal sessies van gebruikersactiviteit bepaalde pagina's en functies van uw app hebt opgenomen. Een sessie worden geteld na half uur gebruiker inactief of na 24 uur onafgebroken gebruik.
* **Gebeurtenissen hulpprogramma**: hoe vaak bepaalde pagina's en functies van uw app worden gebruikt. Een paginaweergave geteld als een pagina in een browser wordt geladen vanuit uw app, mits u [geïnstrumenteerd deze](app-insights-javascript.md). 

    Een aangepaste gebeurtenis vertegenwoordigt een exemplaar van dat er iets gebeurt in uw app, vaak een gebruikersinteractie, zoals een knop klikt u op of de voltooiing van een taak. Voeg in uw app code [aangepaste gebeurtenissen genereren](app-insights-api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Uitvoeren van query's voor bepaalde gebruikers

Leer de verschillende groepen gebruikers door de query-opties aan de bovenkant van het hulpprogramma gebruikers aan te passen:

* Weergeven: Kies een cohort van gebruikers te analyseren.
* Wie gebruikt: Kies aangepaste gebeurtenissen en paginaweergaven.
* Tijdens: Kies een tijdsbereik.
* Door: Kiezen hoe de gegevens laden door een bepaalde periode of door een andere eigenschap zoals browser of een plaats.
* Splitsen: Kies een eigenschap waarmee te splitsen of segment de gegevens. 
* Filters toevoegen: De query beperken tot bepaalde gebruikers, sessies of gebeurtenissen op basis van hun eigenschappen, zoals de browser of een plaats. 
 
## <a name="saving-and-sharing-reports"></a>Opslaan en delen van rapporten 
U kunt gebruikers rapporten, persoonlijke aan u in de sectie Mijn rapporten of gedeeld met iedereen met toegang tot deze Application Insights-resource in de sectie gedeelde rapporten anders opslaan.

Voor het delen van een koppeling naar een rapport gebruikers, sessies of gebeurtenissen; Klik op **Share** Kopieer de koppeling in de werkbalk.

Voor het delen van een kopie van de gegevens in een rapport gebruikers, sessies of gebeurtenissen; Klik op **Share** op de werkbalk, klik vervolgens op de **Word-pictogram** voor het maken van een Word-document met de gegevens. Of klik op de **Word-pictogram** boven het belangrijkste diagram.

## <a name="meet-your-users"></a>Voldoen aan uw gebruikers

De **voldoen aan uw gebruikers** sectie wordt informatie weergegeven over vijf voorbeeldgebruikers overeenkomt met de huidige query. Overweegt en verkennen van het gedrag van personen naast statistische functies, krijgt u inzicht in hoe mensen uw app daadwerkelijk gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)