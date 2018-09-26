---
title: Slimme detectie in Azure Application Insights | Microsoft Docs
description: Application Insights voert een automatische grondige analyse van uw app-Telemetrie en waarschuwt u mogelijke problemen.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: fe1fe5d270dd8eb871301a8ec81375f35b2568da
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096579"
---
# <a name="smart-detection-in-application-insights"></a>Slimme detectie in Application Insights
 Slimme detectie waarschuwt automatisch u over potentiële prestatieproblemen in uw webtoepassing. Het voert proactieve analyse van de telemetrie die uw app naar verzendt [Application Insights](app-insights-overview.md). Als er een plotselinge toename van de foutpercentages of abnormale patronen in de prestaties van de client of server is, krijgt u een waarschuwing. Deze functie heeft geen configuratie nodig. Het werkt als uw toepassing zoveel telemetrie verzendt.

U kunt waarschuwingen voor slimme detectie openen van de e-mailberichten ontvangt u, en van de blade voor slimme detectie.

## <a name="review-your-smart-detections"></a>Controleer uw Slimme detectie
U kunt detectie op twee manieren detecteren:

* **U ontvangt een e-mailbericht** van Application Insights. Hier volgt een voorbeeld:
  
    ![E-mailmelding](./media/app-insights-proactive-diagnostics/03.png)
  
    Klik op de grote knop om meer details in de portal.
* **De tegel voor slimme detectie** op van uw app-overzicht blade geeft het aantal recente waarschuwingen. Klik op de tegel voor een overzicht van recente waarschuwingen.

![Recente detecties weergeven](./media/app-insights-proactive-diagnostics/04.png)

Selecteer een waarschuwing om de details te bekijken.

## <a name="what-problems-are-detected"></a>Welke problemen worden gedetecteerd?
Er zijn drie soorten detectie:

* [Slimme detectie: afwijkende fouten](app-insights-proactive-failure-diagnostics.md). We gebruiken voor machine learning om in te stellen de verwachte frequentie van mislukte aanvragen voor uw app, correleren met belasting en andere factoren. Als het aantal fouten buiten de verwachte envelop gaat, sturen we een waarschuwing.
* [Slimme detectie: afwijkende prestaties](app-insights-proactive-performance-diagnostics.md). U vooraf meldingen krijgt als de reactietijd van een duur van de bewerking of een afhankelijkheid is steeds trager wordt vergeleken met de historische basislijn of als we een afwijkende patroon in de reactietijd of laadtijd van browserpagina identificeren.   
* [Slimme detectie - problemen met de Azure Cloud Service](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). U kunt waarschuwingen krijgen als uw app wordt gehost in Azure Cloud Services en een rolinstantie heeft problemen met opstarten, regelmatige recycling of crashes runtime.

(De help-koppelingen in elke melding gaat u naar de relevante artikelen.)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app controleren:

* [Metric explorer](app-insights-metrics-explorer.md)
* [Search explorer](app-insights-diagnostic-search.md)
* [Analytics - krachtige querytaal](app-insights-analytics-tour.md)

Slimme detectie is volledig automatisch. Maar misschien u graag enkele meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](app-insights-alerts.md)
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md) 

