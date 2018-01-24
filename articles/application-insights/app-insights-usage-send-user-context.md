---
title: Context-id's aan het inschakelen van informatie over het gebruik van de gebruiker optreedt in Azure Application Insights verzenden | Microsoft Docs
description: Bijhouden hoe gebruikers via uw service verplaatsen door ze elk een unieke, permanente ID-reeks in Application Insights toe te wijzen.
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: mbullwin
ms.openlocfilehash: fe4481cf851fc021b3073a6d9d6254f546218785
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Context-id's aan het inschakelen van Azure Application Insights ervaringen voor informatie over het gebruik van de gebruiker te verzenden

## <a name="tracking-users"></a>Gebruikers bijhouden

Application Insights kunt u om te controleren en bijhouden van uw gebruikers via een set hulpprogramma's informatie over het gebruik van product: 
* [Gebruikers, sessies, gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trechters](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Retentie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Cohorten
* [Werkmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Te houden wat een gebruiker gedurende een bepaalde periode doet, moet een Application Insights een ID voor elke gebruiker of de sessie. De volgende id's in elke aangepaste gebeurtenis- of -weergave wilt opnemen.
- Gebruikers, schoorstenen, bewaren en cohorten: bevatten de gebruikers-ID.
- Sessies: Omvatten sessie-ID.

Als uw app is geïntegreerd met de [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), gebruiker-ID automatisch wordt bijgehouden.

## <a name="choosing-user-ids"></a>Gebruikers-id's kiezen

Gebruikers-id's moeten blijven bewaard tussen gebruikerssessies bijhouden hoe gebruikers zich gedragen gedurende een bepaalde periode. Er zijn verschillende manieren voor het persistent maken van de-ID.
- Een definitie van een gebruiker die u al in uw service hebt.
- Als de service toegang tot een browser heeft, kan deze de browser een cookie met een ID in het doorgegeven. De ID bewaard voor, zolang de cookie in de browser van de gebruiker blijft.
- Indien nodig, wordt kunt u een nieuwe ID elke sessie, maar de resultaten over gebruikers beperkt. Bijvoorbeeld, u niet mogelijk om te zien hoe gedrag van een gebruiker verandert gedurende een bepaalde periode.

De ID moet een Guid of een andere tekenreeks complex genoeg elke gebruiker uniek wordt geïdentificeerd. Bijvoorbeeld, wordt een lange willekeurig getal.

Als de ID persoonlijke gegevens over de gebruiker bevat, is het niet de juiste waarde te verzenden naar Application Insights als een gebruikers-ID. U kunt verzenden dergelijke ID als een [geverifieerde gebruikers-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), maar deze voldoet niet aan de vereisten van de gebruiker-ID voor gebruiksscenario's.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET-apps: instelling van de context van de gebruiker in een ITelemetryInitializer

Maak een initialisatiefunctie telemetrie zoals beschreven in detail [hier](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), en stel de Context.User.Id en de Context.Session.Id.

In het volgende voorbeeld wordt de gebruikers-ID op een id die na de sessie is verlopen. Gebruik zo mogelijk een gebruikers-ID dat behouden over de sessies blijft.

```csharp

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    * [Overzicht gebruik](app-insights-usage-overview.md)
    * [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
    * [Trechters](usage-funnels.md)
    * [Retentie](app-insights-usage-retention.md)
    * [Werkmappen](app-insights-usage-workbooks.md)
