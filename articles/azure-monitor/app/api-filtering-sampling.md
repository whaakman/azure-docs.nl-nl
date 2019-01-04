---
title: Filteren en voorverwerking in de Azure Application Insights-SDK | Microsoft Docs
description: Schrijf telemetrie Processors en Initializers telemetrie voor de SDK om te filteren of eigenschappen toevoegen aan de gegevens voordat de telemetrie wordt verzonden naar de Application Insights-portal.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: 193e024b4691b76b08bcbe15ace35ccafd45c394
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023444"
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filteren en voorverwerking van telemetrie in Application Insights-SDK


U kunt schrijven en configureren van plug-ins voor de Application Insights SDK om aan te passen hoe telemetrie wordt vastgelegd en voordat deze wordt verzonden naar de Application Insights-service wordt verwerkt.

* [Sampling](../../azure-monitor/app/sampling.md) reduceert de hoeveelheid telemetrie zonder gevolgen voor uw statistieken. Samen blijven gekoppeld zodat u tussen ervan navigeren kunt bij het oplossen van een probleem van gegevenspunten. In de portal worden vermenigvuldigd met het totale aantal om te compenseren voor de meting.
* Filteren met telemetrie Processors [voor ASP.NET](#filtering) of [Java](../../azure-monitor/app/java-filter-telemetry.md) kunt u selecteren of wijzigen van telemetrie in de SDK voordat deze wordt verzonden naar de server. U kunt bijvoorbeeld de hoeveelheid telemetrie verminderen door aanvragen uitsluiten van robots. Maar filteren is een eenvoudige benadering voor het beperken van verkeer dan steekproeven. Hiermee kunt u meer controle over wat wordt verzonden, maar u moet er rekening mee dat dit van invloed op uw statistieken - bijvoorbeeld: als u alle geslaagde aanvragen filteren.
* [Telemetrie Initializers eigenschappen toevoegen](#add-properties) aan elk telemetrie verzonden vanuit uw app, waaronder telemetrie van de standard-modules. U kunt bijvoorbeeld berekende waarden; toevoegen of versienummers waarmee de gegevens in de portal te filteren.
* [De SDK-API](../../azure-monitor/app/api-custom-events-metrics.md) wordt gebruikt om aangepaste gebeurtenissen en metrische gegevens te verzenden.

Voordat u begint:

* Installeer de Application Insights [SDK voor ASP.NET](../../azure-monitor/app/asp-net.md) of [SDK voor Java](../../azure-monitor/app/java-get-started.md) in uw app.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filteren: ITelemetryProcessor
Deze techniek hebt u een meer rechtstreekse controle over wat is opgenomen of uitgesloten van de telemetriestroom. U kunt deze gebruiken in combinatie met Samples of afzonderlijk.

Als u wilt filteren telemetrie, een processor telemetrie schrijven en registreren bij de SDK. Alle telemetrie verloopt via de processor en kunt u deze niet verwijderen uit de stream of eigenschappen toe te voegen. Dit omvat de telemetrie van de standard-modules, zoals de HTTP-aanvraag collector en de afhankelijkheid collector, evenals de telemetrie die u zelf hebt geschreven. U kunt bijvoorbeeld filteren om telemetrie over aanvragen van robots of geslaagde afhankelijkheidsaanroepen.

> [!WARNING]
> Filteren van de telemetrie van de SDK verzonden kunt met behulp van processors scheeftrekken de statistieken die u in de portal ziet, en soms moeilijk te volgen verwante items.
>
> In plaats daarvan kunt u overwegen [steekproeven](../../azure-monitor/app/sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Maken van een telemetrie-processor (C#)
1. Controleer of de Application Insights-SDK in uw project is versie 2.0.0 of hoger. Met de rechtermuisknop op uw project in Visual Studio Solution Explorer en kies NuGet-pakketten beheren. Controleer in het NuGet-Pakketbeheer, Microsoft.ApplicationInsights.Web.
2. U kunt een filter maken ITelemetryProcessor te implementeren. Dit is een andere uitbreidingspunt zoals telemetrie-module, telemetrische initializer en telemetrie-kanaal.

    U ziet dat telemetrie Processors maken van een keten van verwerking. Als u een exemplaar maken van een telemetrie-processor, geeft u een koppeling naar de volgende processor in de keten. Wanneer een telemetrie-gegevenspunt wordt doorgegeven aan de methode proces, zijn werk doet en roept vervolgens de volgende telemetrie-Processor in de keten.

    ```csharp

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
3. Voeg dit in ApplicationInsights.config:

```xml

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dit is dezelfde sectie waarin u een filter steekproeven initialiseren.)

U kunt tekenreekswaarden doorgeven van het .config-bestand door te geven van openbare benoemde eigenschappen in uw klasse.

> [!WARNING]
> Let erop dat overeenkomt met de naam van het type en de namen van alle eigenschappen in het .config-bestand met de namen van de klasse en de eigenschap in de code. Als het .config-bestand verwijst naar een niet-bestaande type of een eigenschap, mislukken de SDK op de achtergrond om alle telemetrie te verzenden.
>
>

**U kunt ook** kunt u het filter in code initialiseren. In de keten van een initialisatieklasse geschikt - bijvoorbeeld AppStart in Global.asax.cs - invoegen de processor:

```csharp

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients gemaakt na deze stap maakt gebruik van uw processors.

### <a name="example-filters"></a>Voorbeeld van de filters
#### <a name="synthetic-requests"></a>Synthetische aanvragen
Filteren bots en web tests. Hoewel Metrics Explorer de mogelijkheid biedt voor het filteren van synthetische bronnen, beperkt deze optie verkeer door deze te filteren op de SDK.

```csharp

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Mislukte verificatie
Filteren aanvragen met een '401'-respons.

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

#### <a name="filter-out-fast-remote-dependency-calls"></a>Snel externe afhankelijkheidsaanroepen filteren
Als u wilt dat alleen op te sporen aanroepen die langzaam worden uitgevoerd, snel weg te filteren.

> [!NOTE]
> Hiermee wordt de statistische gegevens u in de portal ziet een verkeerd. De afhankelijkheid-grafiek eruitziet als de afhankelijkheidsaanroepen alle fouten zijn.
>
>

```csharp

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
[Deze blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) beschrijft een project voor het vaststellen van afhankelijkheidsproblemen met door het automatisch verzenden van reguliere pings naar afhankelijkheden.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Voeg eigenschappen toe: ITelemetryInitializer
Telemetrie initializers gebruiken voor het definiëren van algemene eigenschappen die worden verzonden met alle telemetrie; en voor het geselecteerde probleem van de modules standaardtelemetrie overschrijven.

Bijvoorbeeld, verzamelt de Application Insights voor webpakket telemetrie over HTTP-aanvragen. Standaard worden gemarkeerd als mislukt van elke aanvraag met een responscode > = 400. Maar als u 400 behandelen als een groot succes wilt, kunt u een telemetrie-initializer die de eigenschap succes opgeven.

Als u een telemetrische initializer opgeeft, is er sprake wanneer een van de Track*() methoden zijn aangeroepen. Dit omvat de methoden die worden aangeroepen door de telemetrie-modules. Deze modules stelt volgens de conventies wordt niet elke eigenschap die al door een initialisatiefunctie is ingesteld.

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

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

*U kunt ook* kunt u de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs instantiëren:

```csharp
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Zie voor meer van dit voorbeeld.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="java-telemetry-initializers"></a>Java telemetrie initializers

[Java SDK-documentatie](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility._telemetry_initializer?view=azure-java-stable)

```Java
public interface TelemetryInitializer
{ /** Initializes properties of the specified object. * @param telemetry The {@link com.microsoft.applicationinsights.telemetry.Telemetry} to initialize. */

void initialize(Telemetry telemetry); }
```

Vervolgens de aangepaste initialisatiefunctie niet registreren in het applicationinsights.xml-bestand.

```xml
<Add type="mypackage.MyConfigurableContextInitializer">
<Param name="some_config_property" value="some_value" />
</Add>
```

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetrie initializers
*JavaScript*

Voeg een telemetrische initializer onmiddellijk na de van initialisatiecode die u hebt verkregen via de portal:

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

                // To check the telemetry item�s type - for example PageView:
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

Zie voor een overzicht van de niet-aangepaste eigenschappen die beschikbaar is op de telemetryItem [gegevensmodel voor Application Insights-exporteren](../../azure-monitor/app/export-data-model.md).

U kunt zoveel initializers als u wilt toevoegen.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor en ITelemetryInitializer
Wat is het verschil tussen de telemetrie processors en telemetrie initializers?

* Er zijn enkele overlappingen in wat u ermee kunt doen: beide eigenschappen toevoegen aan telemetrie kunnen worden gebruikt.
* TelemetryInitializers wordt altijd uitgevoerd voordat TelemetryProcessors.
* TelemetryProcessors kunt u volledig vervangen of verwijderen van een Telemetrisch item.
* TelemetryProcessors verwerken geen prestaties teller telemetrie.

## <a name="troubleshooting-applicationinsightsconfig"></a>Oplossen van problemen met ApplicationInsights.config
* Controleer of de volledig gekwalificeerde naam en de naam van assembly juist zijn.
* Controleer of het bestand applicationinsights.config in uw map met de uitvoer is en de meest recente wijzigingen bevat.

## <a name="reference-docs"></a>Referentiedocumenten
* [API-overzicht](../../azure-monitor/app/api-custom-events-metrics.md)
* [ASP.NET-naslaginformatie](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK-Code
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Volgende stappen
* [Logboeken en gebeurtenissen voor zoeken](../../azure-monitor/app/diagnostic-search.md)
* [Steekproeven](../../azure-monitor/app/sampling.md)
* [Problemen oplossen](../../application-insights/app-insights-troubleshoot-faq.md)
