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
ms.date: 01/03/2019
ms.reviewer: abgreg;mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 29e76338b8d19ce70dedea971d26a49544e9f152
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018140"
---
# <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Gebruikerscontext-id's aan het gebruik van tekst in Azure Application Insights verzenden

## <a name="tracking-users"></a>Gebruikers bijhouden

Application Insights kunt u controleren en bijhouden van uw gebruikers via een set hulpprogramma's voor het gebruik van product:

- [Gebruikers, sessies, gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
- [Trechters](https://docs.microsoft.com/azure/application-insights/usage-funnels)
- [Retentie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention) cohorten
- [Werkmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Als u wilt bijhouden wat een gebruiker na verloop van tijd doet, moet Application Insights een ID voor elke gebruiker of de sessie. Neem de volgende id's in elke aangepaste gebeurtenis of pagina weergave.

- Gebruikers, Trechters, bewaren en cohorten: Opnemen van gebruikers-ID.
- Sessies: Sessie-id

> [!NOTE]
> Dit is een geavanceerde artikel geeft een overzicht van de handmatige stappen voor het bijhouden van gebruikersactiviteit met Application Insights. Met veel webtoepassingen **stappen mogelijk niet meer nodig**, als de standaard-server-side SDK's in combinatie met de [JavaScript SDK op de Browser-Client-side](app-insights-website-monitoring.md), vaak voldoende zijn voor het automatisch bijhouden Gebruikersactiviteit. Als u dit nog niet hebt geconfigureerd [bewaking aan clientzijde](app-insights-website-monitoring.md) naast de SDK-serverzijde dat eerst doen en te testen om te zien als de gebruiker gedrag analytics-hulpprogramma's worden uitgevoerd zoals verwacht.

## <a name="choosing-user-ids"></a>Gebruikers-id's te kiezen

Gebruikers-id's behouden tussen gebruikerssessies om bij te houden hoe gebruikers zich gedragen na verloop van tijd. Er zijn verschillende manieren voor het opslaan van de ID.

- Een definitie van een gebruiker die u al in uw service hebt.
- Als de service toegang tot een browser heeft, kan deze de browser een cookie met de ID in het doorgeven. De ID blijven behouden voor zolang de cookie in de browser van de gebruiker blijft.
- Indien nodig, kunt u een nieuwe ID elke sessie, maar de resultaten van gebruikers worden beperkt. Bijvoorbeeld, u niet mogelijk om te zien hoe het gedrag van gebruikers na verloop van tijd verandert.

De ID moet een Guid of een andere tekenreeks complex genoeg voor unieke identificatie voor elke gebruiker. Het kan bijvoorbeeld een lange willekeurig getal zijn.

Als de ID persoonlijke gegevens over de gebruiker bevat, is het niet de juiste waarde om te verzenden naar Application Insights als een gebruikers-ID. U kunt verzenden die een ID als een [geverifieerde gebruikers-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), maar deze niet voldoet aan de gebruiker-ID-vereisten voor gebruiksscenario's.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET-apps: Instellen van de context van de gebruiker in een ITelemetryInitializer

Maak een telemetrische initializer, zoals beschreven in detail [hier](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer). De sessie-ID via de aanvraagtelemetrie doorgeven en stel de Context.User.Id en de Context.Session.Id.

In het volgende voorbeeld wordt de gebruikers-ID op een id die na de sessie is verlopen. Gebruik zo mogelijk een gebruikers-ID die clusterverbinding blijven tussen sessies behouden.

### <a name="telemetry-initializer"></a>Telemetrie-initializer

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
        var ctx = HttpContext.Current;

        // If telemetry initializer is called as part of request execution and not from some async thread
        if (ctx != null)
        {
            var requestTelemetry = ctx.GetRequestTelemetry();
 
            // Set the user and session ids from requestTelemetry.Context.User.Id, which is populated in Application_PostAcquireRequestState in Global.asax.cs.
            if (requestTelemetry != null && !string.IsNullOrEmpty(requestTelemetry.Context.User.Id) &&
                (string.IsNullOrEmpty(telemetry.Context.User.Id) || string.IsNullOrEmpty(telemetry.Context.Session.Id)))
            {
                // Set the user id on the Application Insights telemetry item.
                telemetry.Context.User.Id = requestTelemetry.Context.User.Id;
 
                // Set the session id on the Application Insights telemetry item.
                telemetry.Context.Session.Id = requestTelemetry.Context.User.Id;
            }
        }
    }
  }
}
```

### <a name="globalasaxcs"></a>Global.asax.cs

```csharp
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace MvcWebRole.Telemetry
{
    public class MvcApplication : HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
 
        protected void Application_PostAcquireRequestState()
        {
            var requestTelemetry = Context.GetRequestTelemetry();
 
            if (HttpContext.Current.Session != null && requestTelemetry != null && string.IsNullOrEmpty(requestTelemetry.Context.User.Id))
            {
                requestTelemetry.Context.User.Id = Session.SessionID;
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Gebruiksoverzicht](app-insights-usage-overview.md)
    - [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Werkmappen](app-insights-usage-workbooks.md)
