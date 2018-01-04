---
title: Onderzoeken fouten en uitzonderingen in de web-apps met Azure Application Insights | Microsoft Docs
description: Uitzonderingen op de ASP.NET-apps samen met aanvraagtelemetrie vastleggen.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: mbullwin
ms.openlocfilehash: d6a0b945bad36842142d16a4840c9c3d69e1564e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Diagnose van uitzonderingen in uw web-apps met Application Insights
Uitzonderingen in uw live web-app worden gerapporteerd door [Application Insights](app-insights-overview.md). U kunt mislukte aanvragen correleren met uitzonderingen en andere gebeurtenissen op de client en de server, zodat u kunt snel de oorzaken te analyseren.

## <a name="set-up-exception-reporting"></a>Uitzondering reporting instellen
* Aan de uitzonderingen die zijn gerapporteerd door uw app server hebben:
  * Installeer [Application Insights-SDK](app-insights-asp-net.md) in uw app-code of
  * IIS-webservers: Voer [Application Insights-Agent](app-insights-monitor-performance-live-website-now.md); of
  * Azure-web-apps: Voeg de [Application Insights-extensie](app-insights-azure-web-apps.md)
  * Java-web-apps: Installeer de [Java-agent](app-insights-java-agent.md)
* Installeer de [JavaScript-fragment](app-insights-javascript.md) in uw webpagina's om af te vangen browseruitzonderingen.
* In sommige App-frameworks of met een aantal instellingen moet u een aantal extra stappen nemen om meer onderscheppen:
  * [Webformulieren](#web-forms)
  * [MVC](#mvc)
  * [Web-API-1.*](#web-api-1x)
  * [Web-API 2.*](#web-api-2x)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Diagnose van uitzonderingen met Visual Studio
Open de app-oplossing in Visual Studio om u te helpen bij foutopsporing.

Voer de app op uw server of op uw ontwikkelcomputer met behulp van F5.

Het venster Application Insights zoeken openen in Visual Studio en stel deze gebeurtenissen van uw app wilt weergeven. Terwijl u fouten opspoort, kunt u dit doen door te klikken op de knop Application Insights.

![Met de rechtermuisknop op het project en kies Application Insights openen.](./media/app-insights-asp-net-exceptions/34.png)

U ziet dat u kunt het rapport filteren zodat alleen uitzonderingen.

*Er zijn geen uitzonderingen weergegeven? Zie [vastleggen uitzonderingen](#exceptions).*

Klik op een uitzonderingenrapport om de stacktracering weer te geven.
Klik op de verwijzing naar een regel in de stack-trace om de relevante code-bestand te openen.  

U ziet dat CodeLens gegevens over de uitzonderingen bevat in de code:

![CodeLens melding van uitzonderingen.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Oplossen van problemen met de Azure portal
Application Insights wordt geleverd met een curated APM-ervaring voor hulp bij het opsporen van fouten in de bewaakte toepassingen. Als u wilt starten, klikt u op de optie fouten in het menu Application Insights resource in de sectie onderzoeken. Hier ziet u een volledige weergave waarin de fout snelheid trends voor uw verzoeken, hoeveel mislukken en hoeveel gebruikers ondervinden gevolgen. Aan de rechterkant ziet u enkele van de handigste distributies die specifiek zijn voor de geselecteerde bewerking, inclusief de top-3-reactiecodes, top-3 uitzondering typen en bovenste 3 afhankelijkheidstypen mislukt mislukken. 

![Fouten sorteren weergave (operations tabblad)](./media/app-insights-asp-net-exceptions/FailuresTriageView.png)

U kunt vervolgens representatieve voorbeelden voor elk van deze subsets van bewerkingen in een enkele klik bekijken. Met name om op te sporen uitzonderingen, kunt u op de telling van een bepaalde uitzondering om te worden gepresenteerd met een blade uitzonderingen details, zoals deze:

![De blade toewijzingdetails uitzondering](./media/app-insights-asp-net-exceptions/ExceptionDetailsBlade.png)

**U kunt ook** in plaats van de uitzonderingen van een specifieke mislukte bewerking bekijkt, kunt u beginnen bij de algehele weergave van uitzonderingen, door het overschakelen naar het tabblad uitzonderingen:

![Fouten sorteren weergave (tabblad Uitzonderingen)](./media/app-insights-asp-net-exceptions/FailuresTriageView_Exceptions.png)

Hier ziet u alle uitzonderingen die worden verzameld voor uw bewaakte app.

*Er zijn geen uitzonderingen weergegeven? Zie [vastleggen uitzonderingen](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Aangepaste tracering en logboekgegevens
Als u diagnostische gegevens die specifiek zijn voor uw app, kunt u code voor het verzenden van uw eigen telemetriegegevens invoegen. Dit weergegeven in de diagnostische gegevens doorzoeken samen met de aanvraag, paginaweergave en andere gegevens automatisch verzameld.

U hebt verschillende mogelijkheden:

* [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) wordt doorgaans gebruikt voor het bewaken van de gebruikspatronen, maar de gegevens ook worden verzonden, wordt weergegeven onder aangepaste gebeurtenissen in diagnostische gegevens doorzoeken. Gebeurtenissen zijn benoemde beheerverkeersstromen kan uitvoeren en eigenschappen van een verbindingsreeks en numerieke metrische gegevens waarop u kunt [uw diagnostische zoekopdrachten filteren](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) kunt u meer gegevens zoals POST informatie verzenden.
* [TrackException()](#exceptions) verzendt stack-traces. [Meer informatie over uitzonderingen](#exceptions).
* Als u al een framework voor logboekregistratie zoals Log4Net of NLog gebruikt, kunt u [deze logboeken vastleggen](app-insights-asp-net-trace-logs.md) en ze ziet in diagnostische gegevens doorzoeken samen met de aanvraag en uitzonderingsgegevens.

Overzicht van deze gebeurtenissen openen [Search](app-insights-diagnostic-search.md)Filter openen en kies vervolgens Custom Event, Trace of uitzondering.

![In detail analyseren](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Als uw app veel telemetriegegevens genereert, beperkt de adaptieve steekproefmodule automatisch het volume dat naar de portal wordt verzonden door alleen een representatieve fractie van de gebeurtenissen te sturen. Gebeurtenissen die deel van dezelfde bewerking uitmaken worden geselecteerd of gedeselecteerd als groep, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>Hoe ziet u POST-aanvraag-gegevens
Gegevens voor de aanvraag bevatten niet de gegevens die worden verzonden naar uw app in een POST-aanroep. Deze gegevens hebben gerapporteerd:

* [De SDK installeren](app-insights-asp-net.md) in uw toepassingsproject.
* Voeg de code in uw toepassing om aan te roepen [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). De POST-gegevens in de parameter bericht verzenden. Er is een limiet voor de toegestane grootte, zodat u proberen moet te verzenden, alleen de essentiële gegevens.
* Wanneer u een mislukte aanvraag onderzoeken, vinden de bijbehorende traceringen.  

![In detail analyseren](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Vastleggen van uitzonderingen en verwante diagnostische gegevens
Aanvankelijk ziet u niet in de portal alle uitzonderingen die ontstaan in uw app problemen. Ziet u alle browseruitzonderingen (als u de [JavaScript SDK](app-insights-javascript.md) in uw webpagina's). Maar de meeste serveruitzonderingen zijn opgepikt door IIS en u moet schrijven van een deel van de code te zien.

U kunt:

* **Meld u uitzonderingen expliciet** door code te plaatsen in uitzonderingshandlers voor het rapporteren van de uitzonderingen.
* **Uitzonderingen automatisch vastleggen** door het configureren van uw ASP.NET-framework. De benodigde toevoegingen zijn verschillend voor verschillende soorten framework.

## <a name="reporting-exceptions-explicitly"></a>Uitzonderingen expliciet rapportage
De eenvoudigste manier is een aanroep naar TrackException() invoegen in een uitzonderings-handler.

Javascript

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

C#

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

VB

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

De eigenschappen en metingen parameters zijn optioneel, maar zijn handig voor [filtering en toe te voegen](app-insights-diagnostic-search.md) extra informatie. Als u een app hebt die verschillende games kunt uitvoeren, kan u bijvoorbeeld alle uitzonderingenrapporten die betrekking hebben op een bepaald spel vinden. U kunt zoveel objecten als u elke woordenlijst wilt toevoegen.

## <a name="browser-exceptions"></a>Browseruitzonderingen
De meeste browseruitzonderingen worden gerapporteerd.

Als de webpagina scriptbestanden van netwerken die inhoud leveren of andere domeinen bevat, zorg ervoor dat uw Scriptlabel heeft het kenmerk ```crossorigin="anonymous"```, en dat de server verzendt [CORS headers](http://enable-cors.org/). Hierdoor kunt u een stack-trace en de details ophalen voor niet-verwerkte JavaScript-uitzonderingen van deze resources.

## <a name="web-forms"></a>Webformulieren
Voor webformulieren, de HTTP-Module kan worden de uitzonderingen verzamelen als er geen omleidingen geconfigureerd met CustomErrors.

Maar als u actieve omleidingen, voegt u de volgende regels toe aan de functie Application_Error in Global.asax.cs. (Het bestand Global.asax toevoegen als u er nog geen hebt).

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a>MVC
Als de [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuratie is `Off`, en vervolgens uitzonderingen zijn beschikbaar voor de [HTTP-Module](https://msdn.microsoft.com/library/ms178468.aspx) te verzamelen. Echter, als het `RemoteOnly` (standaard), of `On`, wordt de uitzondering gewist en niet beschikbaar voor Application Insights voor het automatisch verzamelen. U kunt dit oplossen door het overschrijven van de [System.Web.Mvc.HandleErrorAttribute klasse](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), en het toepassen van de klasse overschreven zoals wordt weergegeven voor de andere onderstaande MVC-versies ([github-bron](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

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

#### <a name="mvc-2"></a>MVC 2
Het kenmerk HandleError vervangen door uw nieuw kenmerk in uw domeincontrollers.

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Registreren `AiHandleErrorAttribute` als globaal filter in Global.asax.cs:

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
Registreer AiHandleErrorAttribute als globaal filter in FilterConfig.cs:

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[Voorbeeld](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a>Web-API 1.x
System.Web.Http.Filters.ExceptionFilterAttribute overschrijven:

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

U kunt dit overschreven kenmerk toevoegen aan specifieke domeincontrollers of toe te voegen aan de configuratie van de globaal filter in de klasse WebApiConfig:

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

[Voorbeeld](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

Er zijn een aantal cases dat de filters uitzondering kunnen niet worden verwerkt. Bijvoorbeeld:

* Uitzonderingen uit controller constructors.
* Uitzonderingen uit bericht handlers.
* Uitzonderingen bij routering.
* Uitzonderingen tijdens de serialisatie van reacties inhoud.

## <a name="web-api-2x"></a>Web-API 2.x
Voeg een implementatie van IExceptionLogger toe:

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

Voeg dit toe aan de services in WebApiConfig:

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

[Voorbeeld](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Als alternatief, kunt u het volgende doen:

1. De enige ExceptionHandler vervangen door een aangepaste implementatie van IExceptionHandler. Dit wordt alleen aangeroepen wanneer het framework nog steeds kunnen kiezen welke response-bericht is verzenden (niet wanneer de verbinding is afgebroken voor exemplaar)
2. Uitzondering Filters (zoals beschreven in de sectie op Web-API 1.x domeincontrollers hierboven) - is niet in alle gevallen wordt aangeroepen.

## <a name="wcf"></a>WCF
Toevoegen van een klasse met kenmerk uitbreidt en IErrorHandler en IServiceBehavior implementeert.

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

Het kenmerk toevoegen aan de service-implementaties:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Voorbeeld](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Uitzondering-prestatiemeteritems
Als u hebt [de Application Insights-Agent geïnstalleerd](app-insights-monitor-performance-live-website-now.md) op uw server, kunt u een grafiek zien van de frequentie van uitzonderingen, gemeten door .NET krijgen. Dit omvat verwerkte en onverwerkte uitzonderingen voor .NET.

Een metriek Explorer-blade geopend, Voeg een nieuwe grafiek toe en selecteer **uitzondering snelheid**, vermeld onder prestatiemeteritems.

De snelheid berekend .NET framework door het aantal uitzonderingen in een interval tellen en te delen door de lengte van het interval.

Dit wijkt af van het 'Uitzonderingen' aantal die berekend door de Application Insights-portal TrackException rapporten tellen. Steekproefintervals zijn verschillend en de SDK niet TrackException rapporten voor alle verwerkte en onverwerkte uitzonderingen verzenden.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Volgende stappen
* [REST, SQL en andere aanroepen van afhankelijkheden bewaken](app-insights-asp-net-dependencies.md)
* [Laadtijden voor pagina's, browseruitzonderingen en AJAX-aanroepen bewaken](app-insights-javascript.md)
* [Prestatiemeteritems van monitor](app-insights-performance-counters.md)
