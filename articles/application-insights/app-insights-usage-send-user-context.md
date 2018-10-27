---
title: Ervaringen met gebruikerscontext id's gebruik in te schakelen in Azure Application Insights verzenden | Microsoft Docs
description: Bijhouden hoe gebruikers verplaatsen via de service door toe te wijzen ze allemaal een unieke, permanente ID-reeks in Application Insights.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/02/2017
ms.reviewer: abgreg;mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: bfb04f596a61ff79c75cd38473c9480a29b0e6c4
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139887"
---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Gebruikerscontext-id's aan het gebruik van tekst in Azure Application Insights verzenden

## <a name="tracking-users"></a>Gebruikers bijhouden

Application Insights kunt u controleren en bijhouden van uw gebruikers via een set hulpprogramma's voor het gebruik van product: 
* [Gebruikers, sessies, gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trechters](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Retentie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Cohorten
* [Werkmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Als u wilt bijhouden wat een gebruiker na verloop van tijd doet, moet Application Insights een ID voor elke gebruiker of de sessie. Neem de volgende id's in elke aangepaste gebeurtenis of pagina weergave.
- Gebruikers, Trechters, bewaren en cohorten: opnemen van gebruikers-ID.
- : Besproken sessie-ID.

Als uw app is geïntegreerd met de [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), gebruiker-ID wordt automatisch bijgehouden.

## <a name="choosing-user-ids"></a>Gebruikers-id's te kiezen

Gebruikers-id's behouden tussen gebruikerssessies om bij te houden hoe gebruikers zich gedragen na verloop van tijd. Er zijn verschillende manieren voor het opslaan van de ID.
- Een definitie van een gebruiker die u al in uw service hebt.
- Als de service toegang tot een browser heeft, kan deze de browser een cookie met de ID in het doorgeven. De ID blijven behouden voor zolang de cookie in de browser van de gebruiker blijft.
- Indien nodig, kunt u een nieuwe ID elke sessie, maar de resultaten van gebruikers worden beperkt. Bijvoorbeeld, u niet mogelijk om te zien hoe het gedrag van gebruikers na verloop van tijd verandert.

De ID moet een Guid of een andere tekenreeks complex genoeg voor unieke identificatie voor elke gebruiker. Het kan bijvoorbeeld een lange willekeurig getal zijn.

Als de ID persoonlijke gegevens over de gebruiker bevat, is het niet de juiste waarde om te verzenden naar Application Insights als een gebruikers-ID. U kunt verzenden die een ID als een [geverifieerde gebruikers-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), maar deze niet voldoet aan de gebruiker-ID-vereisten voor gebruiksscenario's.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET-apps: instelling van de context van de gebruiker in een ITelemetryInitializer

Maak een telemetrische initializer, zoals beschreven in detail [hier](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), en stel de Context.User.Id en de Context.Session.Id.

In het volgende voorbeeld wordt de gebruikers-ID op een id die na de sessie is verlopen. Gebruik zo mogelijk een gebruikers-ID die clusterverbinding blijven tussen sessies behouden.

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
- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    * [Gebruiksoverzicht](app-insights-usage-overview.md)
    * [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
    * [Trechters](usage-funnels.md)
    * [Retentie](app-insights-usage-retention.md)
    * [Werkmappen](app-insights-usage-workbooks.md)
