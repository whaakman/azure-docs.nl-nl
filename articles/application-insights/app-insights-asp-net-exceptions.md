---
title: Diagnosefouten en uitzonderingen in de web-apps met Azure Application Insights | Microsoft Docs
description: Uitzonderingen van ASP.NET-apps, samen met de aanvraagtelemetrie vastleggen.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: mbullwin
ms.openlocfilehash: 87de134f6f0484208e8b6cec52b5eefaac4621c6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251931"
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Diagnose-uitzonderingen in uw web-apps met Application Insights
Uitzonderingen in uw live web-app worden gerapporteerd door [Application Insights](app-insights-overview.md). U kunt mislukte aanvragen correleren met uitzonderingen en andere gebeurtenissen op de client en de server, zodat u kunt snel onderzoek de oorzaken.

## <a name="set-up-exception-reporting"></a>Instellen van de uitzondering rapporteren
* Hebben uitzonderingen die zijn gerapporteerd door uw server-app:
  * Installeer [Application Insights-SDK](app-insights-asp-net.md) in uw app-code, of
  * IIS-webservers: uitvoeren [Application Insights-Agent](app-insights-monitor-performance-live-website-now.md); of
  * Azure-web-apps: Voeg de [Application Insights-extensie](app-insights-azure-web-apps.md)
  * Java-web-apps: Installeer de [Java-agent](app-insights-java-agent.md)
* Installeer de [JavaScript-fragment](app-insights-javascript.md) in uw webpagina's om af te vangen browseruitzonderingen.
* In sommige App-frameworks, of met een aantal instellingen moet u enkele extra stappen uitvoeren om meer uitzonderingen catch:
  * [Webformulieren](#web-forms)
  * [MVC](#mvc)
  * [Web-API 1.*](#web-api-1x)
  * [Web-API 2.*](#web-api-2x)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Diagnose van uitzonderingen met Visual Studio
Open de app-oplossing in Visual Studio om u te helpen bij het opsporen van fouten.

De app uitvoeren op uw server of op een ontwikkelcomputer met behulp van F5.

Het venster Application Insights-zoekopdracht openen in Visual Studio en stelt u deze om gebeurtenissen van uw app weer te geven. Tijdens de foutopsporing, kunt u dit doen door te klikken op de knop Application Insights.

![Met de rechtermuisknop op het project en kies Application Insights, Open.](./media/app-insights-asp-net-exceptions/34.png)

U ziet dat u het rapport om weer te geven alleen uitzonderingen kunt filteren.

*Er zijn geen uitzonderingen weergegeven? Zie [vastleggen van uitzonderingen](#exceptions).*

Klik op een Uitzonderingsrapport om weer te geven van de stack-trace weergegeven.
Klik op de verwijzing naar een regel in de stack-trace, om de relevante code-bestand te openen.  

U ziet dat CodeLens gegevens over de uitzonderingen toont in de code:

![CodeLens-melding van uitzonderingen.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Diagnose van fouten met de Azure portal
Application Insights maakt met een persoonlijke APM-ervaring bij het opsporen van fouten in uw bewaakte toepassingen. Als u wilt starten, klikt u op de optie fouten in het menu van de Application Insights-resource zich in de sectie onderzoeken. U ziet een weergave Volledig scherm waarin u de fout tarief trends voor uw aanvragen, hoe vaak mislukken en hoeveel gebruikers zijn beïnvloed. Aan de rechterkant ziet u enkele van de handigste distributies die specifiek zijn voor de geselecteerde mislukte bewerking, met inbegrip van top 3-responscodes, top 3 uitzonderingstypen en top 3 afhankelijkheidstypen mislukt. 

![Storingen sorteren weergave (tabblad bewerkingen)](./media/app-insights-asp-net-exceptions/FailuresTriageView.png)

U kunt vervolgens representatieve voorbeelden voor elk van deze subsets van bewerkingen in één klik bekijken. In het bijzonder om op te sporen uitzonderingen, kunt u klikken op de telling van een bepaald soort uitzondering kan worden weergegeven met een uitzonderingen blade met details, zoals deze:

![Blade met details uitzondering](./media/app-insights-asp-net-exceptions/ExceptionDetailsBlade.png)

**U kunt ook** in plaats van uitzonderingen van een specifieke bewerking bekijkt, kunt u starten vanuit de algehele weergave van uitzonderingen bij het overstappen naar het tabblad uitzonderingen:

![Storingen sorteren weergave (tabblad Uitzonderingen)](./media/app-insights-asp-net-exceptions/FailuresTriageView_Exceptions.png)

Hier ziet u alle uitzonderingen die worden verzameld voor de bewaakte toepassing.

*Er zijn geen uitzonderingen weergegeven? Zie [vastleggen van uitzonderingen](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Aangepaste tracerings- en logboekgegevens
Als u diagnostische gegevens die specifiek zijn voor uw app, kunt u code om de telemetriegegevens van uw eigen te verzenden. Dit weergegeven in de diagnostische gegevens doorzoeken samen met de aanvraag, paginaweergave en andere gegevens automatisch verzameld.

U hebt verschillende mogelijkheden:

* [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) wordt doorgaans gebruikt voor het bewaken van gebruikspatronen, maar de gegevens ook stuurt weergegeven onder aangepaste gebeurtenissen doorzoeken van diagnostische gegevens. Gebeurtenissen zijn met de naam en eigenschappen van een verbindingsreeks en numerieke metrische gegevens op die u kunt kunt uitvoeren [filteren van uw diagnostische zoekopdrachten](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) kunt u meer gegevens zoals POST informatie verzenden.
* [TrackException()](#exceptions) verzendt stack-traces. [Meer informatie over uitzonderingen](#exceptions).
* Als u al een framework voor logboekregistratie, zoals Log4Net of NLog gebruikt, kunt u [vastleggen van deze logboeken](app-insights-asp-net-trace-logs.md) en ze ziet in diagnostische gegevens doorzoeken en bekijk tegelijkertijd aanvragen en uitzonderingen.

Als u wilt zien deze gebeurtenissen, open [zoeken](app-insights-diagnostic-search.md), Filter openen en kies vervolgens het aangepaste gebeurtenis Trace of uitzondering.

![In detail analyseren](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Als uw app veel telemetriegegevens genereert, beperkt de adaptieve steekproefmodule automatisch het volume dat naar de portal wordt verzonden door alleen een representatieve fractie van de gebeurtenissen te sturen. Gebeurtenissen die deel van dezelfde bewerking uitmaken worden geselecteerd of gedeselecteerd als een groep, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>Hoe ziet u gegevens van aanvragen van bericht
Details van de aanvraag zijn de gegevens verzonden naar de app in een POST-aanroep niet opgenomen. Om deze gegevens die zijn gerapporteerd:

* [Installeer de SDK](app-insights-asp-net.md) in uw project een toepassing.
* Code invoegen in uw toepassing om aan te roepen [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). De POST-gegevens in de parameter bericht verzenden. Er is een limiet voor de toegestane grootte, dus u proberen moet om alleen de essentiële gegevens te verzenden.
* Wanneer u een mislukte aanvragen onderzoeken, vindt u de bijbehorende traceringen.  

![In detail analyseren](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a> Vastleggen van uitzonderingen en gerelateerde diagnostische gegevens
Op het eerste gezicht weergegeven niet in de portal voor alle uitzonderingen die ontstaan in uw app. U ziet alle browseruitzonderingen (als u de [JavaScript SDK](app-insights-javascript.md) in uw webpagina's). Maar de meeste uitzonderingen voor servers zijn opgepikt door IIS en u hebt het schrijven van een deel van de code te zien.

U kunt:

* **Meld u uitzonderingen expliciet** door het invoegen van code in uitzonderingshandlers om de uitzonderingen te rapporteren.
* **Automatisch vastleggen van uitzonderingen** door het configureren van uw ASP.NET-framework. De benodigde toevoegingen verschillen voor verschillende soorten framework.

## <a name="reporting-exceptions-explicitly"></a>Uitzonderingen expliciet melden
De eenvoudigste manier is een aanroep naar TrackException() invoegen in een uitzonderingshandler.

```javascript
    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }
```

```csharp
    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }
```

```VB
    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try
```

De eigenschappen en metingen parameters zijn optioneel, maar zijn nuttig voor [filtering en toe te voegen](app-insights-diagnostic-search.md) extra informatie. Als u een app die meerdere games uitvoeren hebt kunt, kan u bijvoorbeeld alle uitzonderingenrapporten met betrekking tot een bepaald spel vinden. U kunt zoveel items als u aan elke woordenlijst wilt toevoegen.

## <a name="browser-exceptions"></a>Browseruitzonderingen
De meeste browseruitzonderingen worden gerapporteerd.

Als uw webpagina scriptbestanden vanuit CDN's of andere domeinen bevat, controleert u of uw script-code heeft het kenmerk ```crossorigin="anonymous"```, en dat de server verzendt [CORS-headers](http://enable-cors.org/). Hierdoor hebt u een stack-trace en de details voor niet-verwerkte JavaScript-uitzonderingen van deze resources.

## <a name="web-forms"></a>Webformulieren
Voor webformulieren zich de HTTP-Module voor het verzamelen van de uitzonderingen als er geen omleidingen geconfigureerd met CustomErrors.

Maar als u actieve omleidingen, moet u de volgende regels toevoegen aan de functie Application_Error in Global.asax.cs. (Het bestand Global.asax toevoegen als u er nog geen hebt).

```csharp
    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }
```

## <a name="mvc"></a>MVC
Beginnen met Application Insights Web SDK versie 2.6 (beta3 en hoger), Application Insights verzamelt onverwerkte uitzonderingen in de MVC 5 + controllers methoden automatisch. Als u een aangepaste handler voor het volgen van deze uitzonderingen (zoals beschreven in de volgende voorbeelden) eerder hebt toegevoegd, kunt u deze om te voorkomen dat dubbele bijhouden van uitzonderingen kan verwijderen.

Er zijn een aantal cases dat de uitzondering filters kunnen niet worden verwerkt. Bijvoorbeeld:

* Uitzonderingen uit controller constructors.
* Uitzonderingen van bericht handlers.
* Uitzonderingen tijdens de routering.
* Uitzonderingen tijdens de serialisatie van reacties-inhoud.
* Uitzondering opgetreden tijdens het opstarten van de toepassing.
* De uitzondering is opgetreden in de achtergrondtaken.

Alle uitzonderingen *verwerkt* door toepassing nog moet handmatig worden bijgehouden. Niet-verwerkte uitzonderingen die afkomstig zijn van domeincontrollers doorgaans leiden tot 500 'Interne serverfout'-antwoord. Als het antwoord handmatig is gemaakt als gevolg van de uitzondering verwerkt (of geen uitzondering op alle) deze wordt bijgehouden in de bijbehorende aanvraagtelemetrie met `ResultCode` 500, Application Insights-SDK is echter kan geen overeenkomstige uitzonderingen bijhouden.

### <a name="prior-versions-support"></a>Ondersteuning voor oudere versies
Als u MVC 4 (en vóór de) van Application Insights Web SDK 2.5 (en de voorafgaande) gebruikt, raadpleegt u de volgende voorbeelden voor het bijhouden van uitzonderingen.

Als de [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuratie is `Off`, en vervolgens uitzonderingen zijn beschikbaar voor de [HTTP-Module](https://msdn.microsoft.com/library/ms178468.aspx) te verzamelen. Echter, als het `RemoteOnly` (standaard), of `On`, wordt de uitzondering gewist en kan deze niet beschikbaar voor Application Insights voor het automatisch verzamelen. U kunt dit oplossen door overschrijven de [System.Web.Mvc.HandleErrorAttribute klasse](https://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), en het toepassen van de klasse overschreven, zoals wordt weergegeven voor de verschillende onderstaande MVC-versies ([github-bron](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

```csharp
    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }
```

#### <a name="mvc-2"></a>MVC 2
Het kenmerk HandleError vervangen door uw nieuw kenmerk in uw domeincontrollers.

```csharp
    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...
```

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Registreren `AiHandleErrorAttribute` als een globale filter in Global.asax.cs:

```csharp
    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...
```

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
AiHandleErrorAttribute registreren als een globale filter in FilterConfig.cs:

```csharp
    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }
```

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api"></a>Web-API
Beginnen met Application Insights Web SDK versie 2.6 (beta3 en hoger), Application Insights verzamelt onverwerkte uitzonderingen in de controllermethoden op automatisch voor WebAPI 2 +. Als u een aangepaste handler voor het volgen van deze uitzonderingen (zoals beschreven in de volgende voorbeelden) eerder hebt toegevoegd, kunt u deze om te voorkomen dat dubbele bijhouden van uitzonderingen kan verwijderen.

Er zijn een aantal cases dat de uitzondering filters kunnen niet worden verwerkt. Bijvoorbeeld:

* Uitzonderingen uit controller constructors.
* Uitzonderingen van bericht handlers.
* Uitzonderingen tijdens de routering.
* Uitzonderingen tijdens de serialisatie van reacties-inhoud.
* Uitzondering opgetreden tijdens het opstarten van de toepassing.
* De uitzondering is opgetreden in de achtergrondtaken.

Alle uitzonderingen *verwerkt* door toepassing nog moet handmatig worden bijgehouden. Niet-verwerkte uitzonderingen die afkomstig zijn van domeincontrollers doorgaans leiden tot 500 'Interne serverfout'-antwoord. Als het antwoord handmatig is gemaakt als gevolg van de uitzondering verwerkt (of geen uitzondering op alle) deze wordt bijgehouden in een overeenkomende aanvraagtelemetrie met `ResultCode` 500, Application Insights-SDK is echter kan geen overeenkomstige uitzonderingen bijhouden.

### <a name="prior-versions-support"></a>Ondersteuning voor oudere versies
Als u WebAPI-1 (en vóór de) van Application Insights Web SDK 2.5 (en de voorafgaande) gebruikt, raadpleegt u de volgende voorbeelden voor het bijhouden van uitzonderingen.

#### <a name="web-api-1x"></a>Web-API 1.x
Override System.Web.Http.Filters.ExceptionFilterAttribute:

```csharp
    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }
```

U kan dit overschreven kenmerk toevoegen aan specifieke domeincontrollers of toe te voegen aan de configuratie van de globale filter in de klasse WebApiConfig:

```csharp
    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }
```

[Voorbeeld](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

#### <a name="web-api-2x"></a>Web-API 2.x
Voeg een implementatie van IExceptionLogger toe:

```csharp
    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }
```

Voeg dit toe aan de services in WebApiConfig:

```csharp
    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
     }
```

[Voorbeeld](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Als alternatief, kunt u het volgende doen:

1. De enige ExceptionHandler vervangen door een aangepaste implementatie van IExceptionHandler. Dit wordt alleen aangeroepen wanneer het framework nog steeds kunnen kiezen welke antwoordbericht is te verzenden (niet wanneer de verbinding is afgebroken, bijvoorbeeld)
2. Uitzondering Filters (zoals beschreven in de sectie op Web-API 1.x controllers hierboven) - is niet in alle gevallen wordt aangeroepen.

## <a name="wcf"></a>WCF
Een klasse die het kenmerk uitbreidt en ierrorhandler en IServiceBehavior toevoegen.

```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Add the attribute to the service implementations:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...
```

[Voorbeeld](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Uitzondering van prestatiemeteritems
Als u hebt [de Application Insights-Agent geïnstalleerd](app-insights-monitor-performance-live-website-now.md) op uw server, krijgt u een grafiek van het aantal uitzonderingen, gemeten met .NET. Dit omvat zowel verwerkte en onverwerkte uitzonderingen van .NET.

Een Metric Explorer-blade geopend, Voeg een nieuwe grafiek toe en selecteer **uitzonderingsfrequentie**, vermeld onder prestatiemeteritems.

.NET framework berekent het tarief Tel het aantal uitzonderingen in een interval en te delen door de lengte van het interval.

Dit wijkt af van het 'Uitzonderingen' aantal berekend door de Application Insights-webportal tellen TrackException rapporten. De intervallen zijn verschillend en de SDK niet TrackException rapporten voor alle verwerkte en onverwerkte uitzonderingen verzenden.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Volgende stappen
* [REST, SQL en andere aanroepen van afhankelijkheden controleren](app-insights-asp-net-dependencies.md)
* [Monitor pagina laadtijden, browseruitzonderingen en AJAX-aanroepen](app-insights-javascript.md)
* [Monitor-prestatiemeteritems](app-insights-performance-counters.md)
