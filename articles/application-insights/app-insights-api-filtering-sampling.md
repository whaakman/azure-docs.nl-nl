---
title: Filteren en in de Application Insights-SDK van Azure voorverwerking | Microsoft Docs
description: Schrijf telemetrie Processors en telemetrie initalisatiefuncties voor de SDK om te filteren of eigenschappen toevoegen aan de gegevens voordat de telemetrie wordt verzonden naar de Application Insights-portal.
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: borooji;mbullwin
ms.openlocfilehash: 3f621010c1c36445ad35d81d96a2e5aefc46b10c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filteren en voorverwerking telemetrie in Application Insights-SDK


U kunt schrijven en configureren van invoegtoepassingen voor de Application Insights-SDK om aan te passen hoe telemetrie worden vastgelegd en voordat deze wordt verzonden naar de Application Insights-service verwerkt.

* [Steekproef nemen](app-insights-sampling.md) vermindert het volume van telemetrie zonder uw statistieken. Samen blijven gerelateerde gegevenspunten zodat u tussen deze twee navigeren kunt bij het oplossen van een probleem. Het totale aantal worden in de portal om te compenseren voor de steekproeven vermenigvuldigd.
* Filteren met telemetrie Processors [voor ASP.NET](#filtering) of [Java](app-insights-java-filter-telemetry.md) kunt u selecteren of telemetrie in de SDK wijzigen voordat deze wordt verzonden naar de server. U kunt bijvoorbeeld het volume van telemetrie verminderen door robots uitsluiten van aanvragen. Maar voor het filteren is een eenvoudige benadering van verkeer dan steekproeven te verminderen. Hiermee kunt u meer controle over wat wordt verzonden, maar u moet denken dat dit van invloed op uw statistieken - bijvoorbeeld, als u alle geslaagde aanvragen filteren.
* [Telemetrie initalisatiefuncties eigenschappen toevoegen](#add-properties) aan elk telemetrie van uw app, inclusief telemetrie van de standaard-modules worden verzonden. Bijvoorbeeld zou u berekende waarden; of versienummers waarmee de gegevens in de portal te filteren.
* [De SDK-API](app-insights-api-custom-events-metrics.md) wordt gebruikt om aangepaste gebeurtenissen en metrische gegevens te verzenden.

Voordat u begint:

* Installeer de Application Insights [SDK voor ASP.NET](app-insights-asp-net.md) of [SDK voor Java](app-insights-java-get-started.md) in uw app.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filteren: ITelemetryProcessor
Deze techniek hebt u meer rechtstreekse controle over wat is opgenomen of uitgesloten van de telemetriestroom. U kunt deze gebruiken in combinatie met steekproeven, of afzonderlijk.

Als u wilt filteren telemetrie, moet u een processor telemetrie schrijven en registreren met de SDK. Alle telemetrie doorloopt de processor en u kunt deze niet verwijderen uit de stroom of eigenschappen toe te voegen. Dit omvat de telemetrie van de standaard modules, zoals de HTTP-aanvraag collector en de afhankelijkheid collector, evenals de telemetrie die u zelf hebt geschreven. U kunt, bijvoorbeeld telemetrie over aanvragen van robots of geslaagde afhankelijkheidsaanroepen filter.

> [!WARNING]
> Filteren van de telemetrie van de SDK worden verzonden kan met processors leiden tot onjuiste de statistieken die u in de portal ziet en het moeilijker te volgen verwante items.
>
> In plaats daarvan kunt u overwegen [steekproeven](app-insights-sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Maken van een telemetrie-processor (C#)
1. Controleer of de Application Insights-SDK in uw project versie 2.0.0 of hoger. Met de rechtermuisknop op het project in Visual Studio Solution Explorer en kies NuGet-pakketten beheren. Schakel in NuGet-Pakketbeheer Microsoft.ApplicationInsights.Web.
2. U kunt een filter maken ITelemetryProcessor worden geïmplementeerd. Dit is een andere uitbreidbaar punt zoals telemetrie-module, telemetrie initialisatiefunctie en telemetrie-kanaal.

    U ziet dat telemetrie Processors samenstellen van een keten van verwerking. Wanneer u een telemetrie-processor, kunt u een koppeling doorgeven aan de volgende processor in de keten. Wanneer een gegevenspunt telemetrie is doorgegeven aan de methode proces, het werkt en roept vervolgens de volgende telemetrie-Processor in de keten.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify the item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. Voeg dit in ApplicationInsights.config:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dit is dezelfde sectie waarin u een filter steekproeven initialiseren.)

U kunt tekenreekswaarden doorgeven van het .config-bestand door openbare benoemde eigenschappen in uw klasse.

> [!WARNING]
> Wees voorzichtig overeenkomen met de naam van het type en de namen van alle eigenschappen in het .config-bestand met de namen van de klasse en de eigenschap in de code. Als het .config-bestand verwijst naar een niet-bestaande type of een eigenschap, mislukken de SDK achtergrond om eventuele uitzonderingstelemetrie te verzenden.
>
>

**U kunt ook** kunt u het filter in code initialiseren. Voeg een geschikte initialisatie-klasse - bijvoorbeeld AppStart in Global.asax.cs - de processor in de keten:

```csharp

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients gemaakt nadat dit punt wordt gebruik van uw processors.

De volgende code laat zien hoe een initialisatiefunctie telemetrie toevoegen in ASP.NET Core.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var initializer = new SuccessfulDependencyFilter();
    var configuration = app.ApplicationServices.GetService<TelemetryConfiguration>();
    configuration.TelemetryInitializers.Add(initializer);
}
```

### <a name="example-filters"></a>Voorbeeld-filters
#### <a name="synthetic-requests"></a>Synthetische aanvragen
Filter bots en web-tests. Hoewel Metrics Explorer u de optie geeft voor het filteren van synthetische bronnen, beperkt deze optie verkeer door ze te filteren op de SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Mislukte verificatiepogingen
Filteren van aanvragen met een '401'-respons.

```csharp

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Snel externe afhankelijkheidsaanroepen uitfilteren
Als u alleen wilt op te sporen aanroepen die traag, fast weg te filteren.

> [!NOTE]
> Hiermee wordt de statistische gegevens die u in de portal ziet een verkeerd. De grafiek dependency ziet er als alle fouten voor de afhankelijkheidsaanroepen hebben.
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Problemen als gevolg van diagnoseafhankelijkheid
[Deze blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) beschrijft een project om op te sporen afhankelijkheidsproblemen door automatisch reguliere pings afhankelijkheden wordt verzonden.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Eigenschappen toevoegen: ITelemetryInitializer
Telemetrie initalisatiefuncties gebruiken voor het definiëren van algemene eigenschappen die worden verzonden met alle telemetrie; en voor het onderdrukken van geselecteerde gedrag van de standaard telemetrie-modules.

De Application Insights voor Web-pakket verzamelt bijvoorbeeld telemetrie over HTTP-aanvragen. Standaard worden gemarkeerd als een aanvraag met een antwoordcode mislukt > = 400. Maar als u 400 behandelen als een is voltooid wilt, kunt u een initialisatiefunctie voor telemetrie die de eigenschap geslaagd opgeven.

Als u een initialisatiefunctie telemetrie opgeeft, wordt aangeroepen wanneer een van de methoden Track*() wordt aangeroepen. Dit omvat de methoden die worden aangeroepen door de standaard telemetrie-modules. Volgens de conventies worden alle eigenschappen die al is ingesteld door een initialisatiefunctie voor objecten in deze modules niet ingesteld.

**De initialisatiefunctie definiëren**

*C#*

```csharp

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**De initialisatiefunctie laden**

In ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*U kunt ook* u kunt de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs instantiëren:

```csharp
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Zie voor meer informatie in dit voorbeeld.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetrie initalisatiefuncties
*JavaScript*

Voeg een initialisatiefunctie telemetrie direct na de van de initialisatiecode die u hebt verkregen via de portal:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Zie voor een overzicht van de niet-aangepast eigenschappen beschikbaar zijn op de telemetryItem [Application Insights-exporteren gegevensmodel](app-insights-export-data-model.md).

U kunt zoveel initalisatiefuncties als u wilt toevoegen.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor en ITelemetryInitializer
Wat is het verschil tussen de telemetrie processors en telemetrie initalisatiefuncties?

* Er zijn enkele overlappingen in wat u ermee kunt doen: beide eigenschappen toevoegen aan de telemetrie kunnen worden gebruikt.
* TelemetryInitializers wordt altijd uitgevoerd voordat TelemetryProcessors.
* TelemetryProcessors kunt u volledig vervangen of een telemetrie-item verwijderen.
* TelemetryProcessors niet prestatietelemetrie prestatiemeteritem niet verwerken.

## <a name="troubleshooting-applicationinsightsconfig"></a>Het oplossen van problemen ApplicationInsights.config
* Controleer of de volledig gekwalificeerde typenaam en de naam van assembly juist zijn.
* Controleer of het bestand applicationinsights.config in uw directory uitvoer is en de meest recente wijzigingen bevat.

## <a name="reference-docs"></a>Referentiedocumenten
* [API-overzicht](app-insights-api-custom-events-metrics.md)
* [ASP.NET-verwijzing](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK-Code
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Volgende stappen
* [Zoekopdracht gebeurtenissen en Logboeken](app-insights-diagnostic-search.md)
* [Steekproeven](app-insights-sampling.md)
* [Problemen oplossen](app-insights-troubleshoot-faq.md)
