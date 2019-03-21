---
title: Gebruiker gedrag analyseprogramma's in Azure Application Insights oplossen
description: Gids voor probleemoplossing - site en het appgebruik met Application Insights te analyseren.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/11/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: eabc47c2acb33d8c6ee03477b5e8c7783edebbb7
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258770"
---
# <a name="troubleshoot-user-behavior-analytics-tools-in-application-insights"></a>Gebruiker gedrag analyseprogramma's in Application Insights oplossen
Hebt u vragen over de [gebruiker gedrag analyseprogramma's in Application Insights](usage-overview.md): [Gebruikers, sessies, gebeurtenissen](usage-segmentation.md), [Trechters](usage-funnels.md), [Gebruikersstromen](usage-flows.md), [retentie](usage-retention.md), of cohorten? Hier volgen enkele antwoorden.

## <a name="counting-users"></a>Telling van gebruikers
**De gebruiker gedrag analysehulpprogramma's laten zien dat mijn app een gebruikerssessie heeft, maar ik weet dat mijn app heeft veel gebruikers/sessies. Hoe kan ik deze onjuiste aantallen oplossen?**

Alle telemetriegebeurtenissen in Application Insights hebben een [anonieme gebruikers-ID](../../azure-monitor/app/data-model-context.md) en een [sessie-ID](../../azure-monitor/app/data-model-context.md) als twee van de standaardeigenschappen. Standaard aantal alle van de tools voor gebruiksanalyse gebruikers en sessies op basis van deze id. Als deze standaard eigenschappen zijn niet wordt ingevuld met de unieke id's voor elke gebruiker en de sessie van uw app, ziet u een onjuiste telling van gebruikers en sessies in de analytics-hulpprogramma's voor gebruik.

Als u een web-app wordt bewaakt, de eenvoudigste oplossing is het toevoegen van de [Application Insights JavaScript SDK](../../azure-monitor/app/javascript.md) aan uw app en zorg ervoor dat het script codefragment is geladen op elke pagina die u wilt bewaken. De JavaScript SDK automatisch genereert anonieme gebruiker en sessie-id's, vervolgens gevuld telemetriegebeurtenissen met deze id's als ze zijn verzonden vanuit uw app.

Als u een webservice (geen gebruikersinterface), uw bewakingsgegevens [maken van een telemetrie-initializer die de anonieme gebruiker-ID en sessie-ID-eigenschappen vult](usage-send-user-context.md) op basis van uw service begrippen van unieke gebruikers en sessies.

Als uw app verzendt [geverifieerde gebruikers-id's](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users), u kunt rekenen op basis van geverifieerde gebruikers-id's in het hulpprogramma voor gebruikers. Kies in de vervolgkeuzelijst 'Show', 'Geverifieerde gebruikers'.

De gebruiker gedrag analysehulpprogramma's ondersteund momenteel niet tellen gebruikers of -sessies op basis van eigenschappen dan anonieme gebruikers-ID, geverifieerde gebruikers-ID of sessie-ID.

## <a name="naming-events"></a>Naamgeving van gebeurtenissen
**Mijn app heeft duizenden andere paginaweergave en de namen van aangepaste gebeurtenis. Het is moeilijk onderscheid maken tussen deze en de analysehulpmiddelen van de gebruiker-gedrag vaak reageert. Hoe kan ik deze naamgevingsconventie problemen oplossen?**

Paginaweergave en de namen van aangepaste gebeurtenis worden gebruikt in de analysehulpmiddelen van de gebruiker-gedrag. Naamgeving van gebeurtenissen goed is essentieel voor het ophalen van de waarde van deze hulpprogramma's. Het doel is een balans tussen met te weinig, overmatig algemene namen ("knop hebt geklikt") en met te veel, zeer specifieke namen ("knop bewerken hebt geklikt op http:\//www.contoso.com/index ').

Als u wilt geen wijzigingen aanbrengt aan de paginaweergave en aangepaste gebeurtenisnamen van die uw app verzendt, moet u de broncode en opnieuw implementeren van uw app wijzigen. **Alle telemetrie gegevens in Application Insights voor 90 dagen worden opgeslagen en kan niet worden verwijderd**, zodat de wijzigingen die u in gebeurtenisnamen aanbrengt duurt 90 dagen volledig manifest. Voor de 90 dagen na het aanbrengen van wijzigingen in de naam, wordt de gebeurtenisnamen van zowel de oude en nieuwe weergegeven in uw telemetrie, dus aanpassen van query's en communiceren vanuit uw teams dienovereenkomstig.

Als uw app te veel pagina weergavenamen verzendt, controleert u of de namen van deze pagina weergeven in de code handmatig worden opgegeven of als ze wordt automatisch door de Application Insights JavaScript SDK verzonden bent:

* Als de weergavenamen van de pagina handmatig worden opgegeven in de code met behulp van de [ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md), wijzigt u de naam moet minder specifiek. Veelvoorkomende fouten, zoals de naam van de paginaweergave-URL plaatsen vermijden. In plaats daarvan gebruikt u de URL-parameter van de `trackPageView` API. Andere gegevens van de naam van de pagina verplaatsen naar aangepaste eigenschappen.

* Als de Application Insights JavaScript SDK automatisch paginanamen weergeven verzendt, kunt u titels van uw pagina's wijzigen of overschakelen naar verzending handmatig paginanamen weergeven. De SDK verzendt de [titel](https://developer.mozilla.org/docs/Web/HTML/Element/title) van elke pagina als de naam van de pagina, standaard. U kunt uw titels om te worden algemene, maar houd ook rekening met de SEO en andere gevolgen die deze wijziging hebt kan wijzigen. Handmatig op te geven in de weergave met de naam de `trackPageView` API vervangt de namen van de automatisch verzameld, zodat u meer algemene namen in telemetrie verzenden kunt zonder te hoeven wijzigen van pagina's.   

Als uw app te veel namen van aangepaste gebeurtenissen verzendt is, moet u de naam in de volgende code worden minder specifieke wijzigen. Nogmaals, te voorkomen dat de URL's en andere per pagina of dynamische gegevens rechtstreeks in de namen van aangepaste gebeurtenis plaatsen. In plaats daarvan deze gegevens te verplaatsen naar aangepaste eigenschappen van het aangepaste gebeurtenis met de `trackEvent` API. Bijvoorbeeld, in plaats van `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`, wordt aangeraden dat er ongeveer als `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van gebruiker gedrag analytics-hulpprogramma 's](usage-overview.md)

## <a name="get-help"></a>Help opvragen
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)

