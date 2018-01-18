---
title: Gebruiksanalyse in Azure Application Insights oplossen
description: Gids voor probleemoplossing - analyse van site- en app-gebruik met Application Insights.
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/16/2018
ms.author: mbullwin
ms.openlocfilehash: cb5f3052301b23eb10cd6b84ab6fae98bcc7ea18
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-usage-analytics-in-application-insights"></a>Gebruiksanalyse in Application Insights oplossen
Vragen hebt over de [gebruik analytics hulpprogramma's in Application Insights](app-insights-usage-overview.md): [gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md), [schoorstenen](usage-funnels.md), [gebruiker loopt](app-insights-usage-flows.md), [Bewaren](app-insights-usage-retention.md), of cohorten? Hier volgen enkele antwoorden.

## <a name="counting-users"></a>Telling van gebruikers
**De hulpprogramma's weergeven dat mijn app een gebruikerssessie heeft, maar ik mijn app weet gebruiksanalyse heeft veel gebruikers/sessies. Hoe kan ik deze onjuiste aantallen oplossen?**

Alle telemetriegebeurtenissen in Application Insights hebben een [anonieme gebruikers-ID](application-insights-data-model-context.md) en een [sessie-ID](application-insights-data-model-context.md) als twee van de standaardeigenschappen. Standaard tellen alle van de hulpprogramma's voor webanalyse gebruik gebruikers en sessies op basis van deze id. Als deze standaard eigenschappen worden niet met de unieke id's voor elke gebruiker en de sessie van uw app wordt ingevuld, ziet u een onjuiste telling van gebruikers en sessies in de hulpprogramma's voor webanalyse gebruik.

Als u een web-app controleren bent, de gemakkelijke oplossing is het toevoegen van de [Application Insights JavaScript SDK](app-insights-javascript.md) naar uw app en zorg ervoor dat het script codefragment is geladen op elke pagina die u wilt bewaken. De JavaScript SDK automatisch genereert anonieme gebruiker en sessie-id's en telemetrische gebeurtenissen in deze id wordt gevuld als dat van uw app lukt.

Als u bent een webservice (geen gebruikersinterface) bewaken [maken van een initialisatiefunctie voor telemetrie die de anonieme gebruiker-ID en sessie-ID-eigenschappen vult](app-insights-usage-send-user-context.md) volgens uw service begrippen unieke gebruikers en sessies.

Als uw app verzendt [geverifieerde gebruikers-id's](app-insights-api-custom-events-metrics.md#authenticated-users), kunt u op basis van geverifieerde gebruikers-id's in het hulpprogramma gebruikers tellen. Kies in de vervolgkeuzelijst 'Weergeven', 'Geverifieerde gebruikers'.

De hulpprogramma's voor webanalyse gebruik ondersteund niet tellen gebruikers of sessies op basis van eigenschappen dan anonieme gebruikers-ID of ID van de geverifieerde gebruiker sessie-ID.

## <a name="naming-events"></a>Naamgeving van gebeurtenissen
**Mijn app heeft duizenden verschillende paginaweergave en aangepaste gebeurtenisnamen. Het is moeilijk onderscheid maken tussen deze en de hulpprogramma's voor webanalyse gebruik vaak niet meer reageren. Hoe kan ik deze naming problemen oplossen?**

Paginaweergave en aangepaste gebeurtenisnamen worden gebruikt in de hulpprogramma's voor webanalyse gebruik. Gebeurtenissen ook naamgeving is essentieel voor het ophalen van de waarde van deze hulpprogramma's. Het doel is een evenwicht tussen met te weinig, overmatig algemene namen ('knop geklikt') en met te veel, overmatig specifieke namen ('knop bewerken op http://www.contoso.com/index geklikt').

Als u alle wijzigingen naar de paginaweergave en de namen van aangepaste gebeurtenis van uw app verzenden, die u wilt wijzigen van de broncode van uw app en de implementatie opnieuw uit. **Alle telemetrie in Application Insights gedurende 90 dagen wordt opgeslagen en kan niet worden verwijderd**, dus u wijzigt de gebeurtenisnamen van de is 90 dagen aan het licht volledig. De negentig dagen na het aanbrengen van naamswijzigingen van wordt zowel de gebeurtenisnamen van de oude en nieuwe weergegeven in uw telemetrie, dus past query's en communiceer deze binnen uw teams dienovereenkomstig.

Als uw app te veel namen van de pagina weergeven verzendt, controleert u of deze pagina weergeven namen handmatig worden opgegeven in de code en als dat automatisch door de Application Insights JavaScript SDK lukt:

* Als de namen van de weergave pagina handmatig worden opgegeven bij het gebruik van code de [ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md), wijzigt u de naam moet minder specifiek. Veelvoorkomende fouten, zoals het plaatsen van de URL van de naam van de paginaweergave vermijden. Gebruik in plaats daarvan de URL-parameter van de `trackPageView` API. Andere details van de naam van de pagina verplaatsen naar aangepaste eigenschappen.

* Als de Application Insights JavaScript SDK automatisch pagina weergavenamen verzendt, kunt u uw pagina's titels wijzigen of overschakelen naar de pagina weergavenamen handmatig te verzenden. De SDK verzendt de [titel](https://developer.mozilla.org/docs/Web/HTML/Element/title) van elke pagina als de naam van de pagina, standaard. U kunt uw titels om algemenere zijn, maar houd ook rekening met Zoekmachineoptimalisatie en andere effecten die deze wijziging kan hebben wijzigen. Paginaweergave handmatig opgeven met de namen van de `trackPageView` API overschrijft de namen van de automatisch verzameld zodat u meer algemene namen in telemetrie verzenden kan zonder te wijzigen van pagina's.   

Als uw app te veel aangepaste gebeurtenisnamen verzendt, wijzigt u de naam in de volgende code worden minder specifiek. Geen opnieuw, URL's en andere per pagina of dynamische gegevens rechtstreeks in de namen van aangepaste providers. In plaats daarvan deze gegevens naar een aangepaste eigenschappen van de aangepaste gebeurtenis met de `trackEvent` API. Bijvoorbeeld, in plaats van `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`, het is raadzaam dat lijkt op `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van gebruik analytics](app-insights-usage-overview.md)

## <a name="get-help"></a>Help opvragen
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

