---
title: Telemetrie van lijnen in Azure Application Insights | Microsoft Docs
description: Hoe u de hoeveelheid telemetrie onder controle te houden.
services: application-insights
documentationcenter: windows
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.reviewer: vitalyg
ms.author: cithomas
ms.openlocfilehash: cd0369f45529082ac929b1d87608204033cd78f6
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370513"
---
# <a name="sampling-in-application-insights"></a>Steekproeven in Application Insights

Steekproeven is een functie in [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Dit is de aanbevolen manier te verlagen Telemetrisch verkeer en de opslag, een statistisch correcte analyse van toepassingsgegevens te behouden. Het filter selecteert items die zijn gerelateerd, zodat u tussen items navigeren kunt wanneer u diagnostische onderzoek doen.
Wanneer metrische aantallen worden weergegeven in de portal, worden ze renormalized om uit te voeren in account steekproeven. In dat geval wordt geminimaliseerd is van invloed zijn op de statistieken.

Steekproeven vermindert de kosten van verkeer en gegevens, en vermijdt u beperking.

## <a name="in-brief"></a>Kort samengevat:

* Steekproeven behoudt 1 in *n* registreert en de rest wordt genegeerd. Het kan bijvoorbeeld een van de vijf gebeurtenissen, een samplefrequentie van 20% behouden. 
* Adaptieve steekproeven is standaard ingeschakeld in de meest recente versie van ASP.NET en ASP.NET Core Software Development Kits (SDK's).
* U kunt ook een steekproef handmatig instellen. Dit kan worden geconfigureerd in de portal op de *pagina gebruik en geschatte kosten*, in de SDK voor ASP.NET in het bestand ApplicationInsights.config, in de ASP.NET Core-SDK via code of in de Java SDK in de ApplicationInsights.xml-bestand.
* Als u zich aanmeldt voor aangepaste gebeurtenissen en nodig om ervoor te zorgen dat een reeks gebeurtenissen behouden of samen worden verwijderd, moeten de gebeurtenissen dezelfde bewerkings-id-waarde hebben.
* De deler steekproeven *n* in elke record in de eigenschap wordt gerapporteerd `itemCount`, die in het zoekvak wordt weergegeven onder de beschrijvende naam "aantal aanvragen" of "aantal". `itemCount==1`Wanneer steekproeven is niet in werking.
* Als u analysequery's schrijven, moet u [rekening wordt gehouden van het samplen bijhouden](../../azure-monitor/log-query/aggregations.md). In het bijzonder, in plaats van gewoon tellen van records, moet u `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Typen van het samplen bijhouden

Er zijn drie steekproeven van alternatieve methoden:

* **Adaptieve steekproeven** automatisch het volume van telemetrie van de SDK in uw ASP.NET/ASP.NET Core-app verzonden. Dit is de standaard-steekproeven van ASP.NET Web SDK v 2.0.0-beta3 of hoger en Microsoft.ApplicationInsights.AspNetCore SDK v 2.2.0-beta1 en hoger.  Adaptieve steekproeven is momenteel alleen beschikbaar voor ASP.NET-serverzijde telemetrie.

* **Vast aantal steekproeven** reduceert de hoeveelheid telemetrie die van uw ASP.NET- of ASP.NET Core- of Java server en de browsers van uw gebruikers verzonden. Stelt u het tarief. De client en server worden gesynchroniseerd hun steekproeven zo dat, in Search, u tussen gerelateerde paginaweergaven en aanvragen navigeren kunt.

* **Opnamesteekproeven** werkt in Azure portal. Het deel van de telemetrie die binnenkomt in uw app, tegen een tarief van lijnen die u hebt ingesteld, wordt verwijderd. Het verkeer dat telemetrie wordt verzonden vanuit uw app niet verlagen, maar houdt u binnen uw maandelijkse quotum. Het belangrijkste voordeel van opnamesteekproeven is dat u de steekproeffrequentie instellen kunt zonder uw app opnieuw te implementeren. Opnamesteekproeven werkt op uniforme wijze voor alle servers en clients.

Als er zich Adaptief of vast tarief steekproeven in bewerking, is opnamesteekproeven uitgeschakeld.


## <a name="adaptive-sampling-in-your-aspnetaspnet-core-web-applications"></a>Adaptieve steekproeven in uw ASP.NET/ASP.NET Core Web-Apps

Adaptieve steekproeven is beschikbaar voor de Application Insights SDK voor ASP.NET v 2.0.0-beta3 of hoger, Microsoft.ApplicationInsights.AspNetCore SDK v 2.2.0-beta1 en hoger, en is standaard ingeschakeld.

Adaptieve steekproeven is van invloed op het volume van de telemetrie van uw web-app-server wordt verzonden naar de Application Insights-service-eindpunt. Het volume automatisch wordt aangepast om te voorkomen dat binnen een opgegeven maximale snelheid van het verkeer en wordt beheerd via de instelling `MaxTelemetryItemsPerSecond`. Als de toepassing een kleine hoeveelheid telemetrie, produceert, zoals bij het opsporen van fouten, hetzij vanwege het beperkt gebruikte, items steekproef wordt niet ophalen, zolang het volume is kleiner dan `MaxTelemetryItemsPerSecond`. Als het volume van telemetrie toeneemt, samplefrequentie aangepast zodat deze het doelvolume bereiken.

Voor het bereiken van het doelvolume, deel van de gegenereerde telemetrie wordt verwijderd. Maar net als andere typen van het samplen bijhouden, behoudt de algoritme gerelateerde telemetrie-items. Bijvoorbeeld, wanneer u de telemetrie in Search inspecteren bent, zult u kunnen vinden van de aanvraag met betrekking tot een bepaald soort uitzondering.

Metrische gegevens telt, zoals snelheid van aanvragen en aantal uitzonderingen worden aangepast om te compenseren voor de samplingfrequentie, zodat ze Metric Explorer ongeveer juiste waarden weergegeven.

## <a name="configuring-adaptive-sampling-for-aspnet-applications"></a>Adaptieve steekproeven voor ASP.NET-toepassingen configureren

[Informatie over](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications) over het configureren van adaptieve steekproeven voor voor ASP.NET Core-toepassingen. 

In [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), kunt u meerdere parameters op in de `AdaptiveSamplingTelemetryProcessor` knooppunt. De cijfers die worden weergegeven, zijn de standaardwaarden:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Het doel-tarief dat het algoritme voor adaptieve is erop gericht voor **op elke server-host**. Als uw web-app wordt uitgevoerd op veel hosts, verlaagt u deze waarde zodat deze blijven binnen de snelheid van het doel van verkeer op de Application Insights-portal.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Het interval waarmee de huidige frequentie van telemetrie opnieuw geëvalueerd. Evaluatie wordt uitgevoerd als een zwevend gemiddelde. U kunt dit interval kort als uw telemetrie onderhevig aan plotselinge pieken is.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Wanneer wijzigingen van percentage steekproeven, hoe snel nadat we mogen lagere voorbeeldpercentage opnieuw om vast te leggen minder gegevens.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Wanneer wijzigingen van percentage sampling, hoe snel nadat we mogen voorbeeldpercentage opnieuw voor het vastleggen van meer gegevens te verhogen.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Als het percentage steekproeven varieert, is wat de minimale waarde die we dat mag worden ingesteld.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Als het percentage steekproeven varieert, is wat de toegestane maximumwaarde we bent om in te stellen.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    In de berekening van het zwevende gemiddelde, het gewicht aan de meest recente waarde toegewezen. Gebruik de waarde gelijk is aan of kleiner is dan 1. Lagere waarden wordt het algoritme van minder reactieve naar plotselinge wijzigingen aanbrengen.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    De waarde die wordt toegewezen wanneer de app zojuist is gestart. Verklein de waarde niet tijdens de foutopsporing.

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Een door puntkomma's gescheiden lijst met typen die u niet wilt dat deze worden verzameld. Ondersteunde typen zijn: Afhankelijkheid, gebeurtenis, uitzonderingen, paginaweergave, aanvraag, traceren. Alle exemplaren van de opgegeven typen worden verzonden; de typen die niet zijn opgegeven steekproeven worden genomen.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Een door puntkomma's gescheiden lijst met typen die u wilt worden verzameld. Ondersteunde typen zijn: Afhankelijkheid, gebeurtenis, uitzonderingen, paginaweergave, aanvraag, traceren. De opgegeven typen steekproeven worden genomen; alle exemplaren van de andere typen worden altijd verzonden.


**Uitschakelen** adaptieve steekproeven, het AdaptiveSamplingTelemetryProcessor-knooppunt verwijderen uit Application Insights-configuratie.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatief: adaptieve steekproeven in code configureren

In plaats van de parameter steekproeven instellen in het .config-bestand, kunt u programmatisch deze waarden instellen.

1. Verwijder alle de `AdaptiveSamplingTelemetryProcessor` knooppunt(en) van het .config-bestand.
2. Gebruik het volgende codefragment om te configureren van adaptieve steekproeven.

*C#*

```csharp

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    // If you are on ApplicationInsights SDK v 2.8.0-beta2 or higher, use the following line instead
    // var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    // Enable AdaptiveSampling so as to keep overall telemetry volume to 5 items per second.
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Meer informatie over de telemetrie processors](../../azure-monitor/app/api-filtering-sampling.md#filtering).)

U kunt de samplefrequentie voor elk type telemetrie ook afzonderlijk aanpassen of kan zelfs uitsluiten voor bepaalde typen, worden steekproeven genomen helemaal. 

*C#*

```csharp
    // The following configures adaptive sampling with 5 items per second, and also excludes DependencyTelemetry from being subject to sampling.
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");
```

## <a name="configuring-adaptive-sampling-for-aspnet-core-applications"></a>Adaptieve steekproeven voor ASP.NET Core-toepassingen configureren.

Er is geen `ApplicationInsights.Config` voor ASP.NET Core-toepassingen, dus elke configuratie wordt uitgevoerd via code.
Adaptieve steekproeven is standaard ingeschakeld voor alle ASP.NET Core-toepassingen. U kunt uitschakelen of aanpassen van de werking van steekproeven.

### <a name="turning-off-adaptive-sampling"></a>Het uitschakelen van adaptieve steekproeven

De standaard steekproeven-functie kan worden uitgeschakeld tijdens het toevoegen van Application Insights-service wordt in de methode ```ConfigureServices```, met ```ApplicationInsightsServiceOptions```:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    aiOptions.EnableAdaptiveSampling = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
    //...
}
```

De bovenstaande code wordt steekproeven functie uitgeschakeld. Volg de stappen hieronder om het toevoegen van sampling met meer opties voor het aanpassen.

### <a name="configure-sampling-settings"></a>Steekproeven instellingen configureren

Gebruik van uitbreidingsmethoden van ```TelemetryProcessorChainBuilder``` zoals hieronder wordt weergegeven om aan te passen van steekproeven gedrag.

> [!IMPORTANT]
> Als u deze methode gebruiken om te configureren van steekproeven, zorg ervoor dat u het gebruik van aiOptions.EnableAdaptiveSampling = false; instellingen met AddApplicationInsightsTelemetry().

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var configuration = app.ApplicationServices.GetService<TelemetryConfiguration>();

    var builder = configuration .TelemetryProcessorChainBuilder;
    // version 2.5.0-beta2 and above should use the following line instead of above. (https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/develop/CHANGELOG.md#version-250-beta2)
    // var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    // Using adaptive sampling
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // Alternately, the following configures adaptive sampling with 5 items per second, and also excludes DependencyTelemetry from being subject to sampling.
    // builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");

    builder.Build();

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));
    // ...
}

```

**Als u met behulp van de bovenstaande methode configureren steekproeven, zorg ervoor dat u ```aiOptions.EnableAdaptiveSampling = false;``` instellingen met AddApplicationInsightsTelemetry().**

## <a name="fixed-rate-sampling-for-aspnet-aspnet-core-and-java-websites"></a>Vast tarief sampling voor ASP.NET, ASP.NET Core en Java-websites

Vast tarief met steekproeven vermindert het verkeer dat wordt verzonden vanuit uw webserver en webbrowsers. In tegenstelling tot adaptieve steekproeven vermindert telemetrie tegen een vast tarief door u bepaald. Het ook synchroniseert de client en server steekproeven zodat gerelateerde items worden bewaard: als u de weergave van een pagina in het zoekvak bekijkt, kunt u bijvoorbeeld de gerelateerde aanvraag vinden.

Net als andere technieken steekproeven houdt dit ook verwante items. Voor elke HTTP-aanvraag zijn gebeurtenis, de aanvraag en de gerelateerde gebeurtenissen verwijderd of samen worden verzonden.

Tarieven zoals telt het aantal aanvragen en uitzonderingen worden vermenigvuldigd met een factor om te compenseren voor de samplingfrequentie in Metrics Explorer zodat ze ongeveer juist zijn.

### <a name="configuring-fixed-rate-sampling-in-aspnet"></a>Vast aantal steekproeven configureren in ASP.NET

1. **Schakel adaptieve steekproeven**: In [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), verwijderen of de opmerkingen bij de `AdaptiveSamplingTelemetryProcessor` knooppunt.

    ```xml

    <TelemetryProcessors>

    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->
    ```

2. **Schakel de steekproefmodule vast tarief.** Dit codefragment toevoegen [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```
   ### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternatief: vast tarief van lijnen in uw servercode inschakelen
    
    In plaats van de parameter steekproeven instellen in het .config-bestand, kunt u programmatisch deze waarden instellen. 

*C#*

```csharp

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    // If you are on ApplicationInsights SDK v 2.8.0-beta2 or higher, use the following line instead
    // var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```
([Meer informatie over de telemetrie processors](../../azure-monitor/app/api-filtering-sampling.md#filtering).)

### <a name="configuring-fixed-rate-sampling-in-aspnet-core"></a>Configureren van steekproeven vast tarief in ASP.NET Core

1. **Schakel adaptieve steekproeven**:  Wijzigingen kunnen worden aangebracht in de methode ```ConfigureServices```, met ```ApplicationInsightsServiceOptions```:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
    // ...

        var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        aiOptions.EnableAdaptiveSampling = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    //...
    }
    ```

2. **Schakel de steekproefmodule vast tarief.** Wijzigingen kunnen worden aangebracht in de methode ```Configure``` zoals wordt weergegeven in onderstaande fragment:

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        var configuration = app.ApplicationServices.GetService<TelemetryConfiguration>();

        var builder = configuration.TelemetryProcessorChainBuilder;
        // version 2.5.0-beta2 and above should use the following line instead of above. (https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/develop/CHANGELOG.md#version-250-beta2)
        // var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

        // Using fixed rate sampling   
        double fixedSamplingPercentage = 10;
        builder.UseSampling(fixedSamplingPercentage);

        builder.Build();

        // ...
    }

    ```

### <a name="configuring-fixed-rate-sampling-in-java"></a>Configureren van steekproeven vast tarief in JAVA ###

1. Downloaden en configureren uw webtoepassing met de meest recente [application insights java SDK](../../azure-monitor/app/java-get-started.md)

2. **Inschakelen van de module vast tarief steekproeven** door het volgende codefragment toe te voegen aan ApplicationInsights.xml-bestand.

    ```XML
        <TelemetryProcessors>
            <BuiltInProcessors>
                <Processor type = "FixedRateSamplingTelemetryProcessor">
                    <!-- Set a percentage close to 100/N where N is an integer. -->
                    <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                    <Add name = "SamplingPercentage" value = "50" />
                </Processor>
            </BuiltInProcessors>
        <TelemetryProcessors/>
    ```

3. U kunt opnemen of uitsluiten van specifieke typen telemetrie van Sampling met behulp van de volgende codes in de tag Processor "FixedRateSamplingTelemetryProcessor"
    ```XML
        <ExcludedTypes>
            <ExcludedType>Request</ExcludedType>
        </ExcludedTypes>

        <IncludedTypes>
            <IncludedType>Exception</IncludedType>
        </IncludedTypes>
    ```

De telemetrietypen die kunnen worden opgenomen of uitgesloten van steekproeven zijn: Afhankelijkheid, gebeurtenis, uitzonderingen, paginaweergave, aanvragen en tracering.

> [!NOTE]
> Voor het voorbeeldpercentage, kiest u een percentage dat bijna 100/N waarbij N staat voor een geheel getal zijn.  Op dit moment steekproeven biedt geen ondersteuning voor andere waarden.
> 
> 

<a name="other-web-pages"></a>



## <a name="ingestion-sampling"></a>Opnamesteekproeven

Deze vorm van steekproeven is van invloed op het punt waar de Application Insights-service-eindpunt wordt bereikt door de telemetrie van uw webserver, browsers en apparaten. Hoewel dit niet het verkeer dat telemetrie wordt verzonden vanuit uw app beperken, het u aanzienlijk minder verwerkt en bewaard (en in rekening gebracht voor) door Application Insights.

Gebruik dit type van het samplen bijhouden als uw app vaak over het maandelijkse quotum bereikt gaat en u de optie hoeft van het gebruik van een van de typen SDK op basis van het samplen bijhouden. 

Stel de samplingfrequentie in de pagina gebruik en geschatte kosten:

![Op de blade overzicht van toepassing, klik op instellingen, Quota, voorbeelden, selecteer vervolgens een samplefrequentie, en klikt u op bijwerken.](./media/sampling/04.png)

Net als andere typen van het samplen bijhouden behoudt de algoritme gerelateerde telemetrie-items. Bijvoorbeeld, wanneer u de telemetrie in Search inspecteren bent, zult u kunnen vinden van de aanvraag met betrekking tot een bepaald soort uitzondering. Metrische gegevens zoals snelheid van aanvragen voor het aantal en aantal uitzonderingen correct worden bewaard.

Gegevenspunten die worden genegeerd door middel van steekproeven zijn niet beschikbaar in een functie van Application Insights zoals [continue Export](../../azure-monitor/app/export-telemetry.md).

Opnamesteekproeven functioneren niet terwijl er op basis van een SDK adaptieve of vast tarief steekproeven worden uitgevoerd. Adaptieve steekproeven is standaard ingeschakeld wanneer ASP.NET/ASP.NET Core-SDK is ingeschakeld in Visual Studio of ingeschakeld in extensies voor Azure-Web-App of met behulp van Status Monitor en opnamesteekproeven is uitgeschakeld. Als u de samplingfrequentie bij de SDK is minder dan 100% (zoals) items zijn, worden steekproeven genomen) en vervolgens de opname-samplingfrequentie die u hebt ingesteld, wordt genegeerd.

> [!WARNING]
> De waarde die wordt weergegeven op de tegel geeft de waarde die u hebt ingesteld voor opnamesteekproeven. Het vertegenwoordigen niet de werkelijke samplingfrequentie als SDK steekproeven uitgevoerd worden.
>
>
## <a name="sampling-for-web-pages-with-javascript"></a>Sampling voor webpagina's met JavaScript
U kunt de webpagina's voor vast tarief samplen bijhouden op elke server configureren. 

Wanneer u [configureren van de webpagina's voor Application Insights](../../azure-monitor/app/javascript.md), wijzigen van de JavaScript-fragment die u via de Application Insights-portal. (In ASP.NET-apps in het codefragment doorgaans gaat in _Layout.cshtml.)  Voeg een regel, zoals `samplingPercentage: 10,` voordat de instrumentatiesleutel:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Voor het voorbeeldpercentage, kiest u een percentage dat bijna 100/N waarbij N staat voor een geheel getal zijn.  Op dit moment steekproeven biedt geen ondersteuning voor andere waarden.

Als u ook steekproeven vast tarief op de server inschakelt, worden de clients en de server worden gesynchroniseerd zo dat, in Search, u tussen gerelateerde paginaweergaven en aanvragen navigeren kunt.

## <a name="when-to-use-sampling"></a>Wanneer u steekproeven?

Adaptieve steekproeven wordt automatisch ingeschakeld in de meest recente .NET en .NET Core SDK's. Ongeacht welke versie van de SDK die u gebruikt, kunt u opnamesteekproeven om toe te staan van Application Insights voor een steekproef van de verzamelde gegevens.

Er is geen steekproeven is standaard ingeschakeld in Java-SDK. Op dit moment ondersteunt deze alleen vast tarief steekproeven. Adaptieve steekproeven wordt niet ondersteund in Java-SDK.

In het algemeen voor de meeste grootte voor kleine en middelgrote toepassingen hoeft u geen steekproeven. De handigste diagnostische gegevens en de meest nauwkeurige statistieken worden verkregen door het verzamelen van gegevens op alle activiteiten van uw gebruikers. 

De belangrijkste voordelen van het samplen bijhouden zijn:

* Application Insights service val vertragingen (' in') van gegevenspunten wanneer uw app stuurt een zeer hoge frequentie van telemetrie in korte tijd interval. 
* Te bewaren binnen de [quotum](../../azure-monitor/app/pricing.md) van gegevenspunten voor uw prijscategorie. 
* Om netwerkverkeer te beperken van het verzamelen van telemetrie. 

### <a name="which-type-of-sampling-should-i-use"></a>Welk type van het samplen bijhouden moet ik gebruiken?

**Gebruik de opname steekproeven als:**

* U gaat vaak via uw maandelijkse quotum van telemetrie.
* U gebruikt een versie van de SDK die biedt geen ondersteuning voor sampling - bijvoorbeeld ASP.NET-versies ouder dan 2.
* U krijgt te veel telemetrie van uw gebruikers webbrowsers.

**Gebruik vast tarief steekproeven als:**

* U gebruikt de Application Insights SDK voor ASP.NET web services-versie 2.0.0 of hoger of Java-SDK v2.0.1 of hoger, en
* U wilt dat gesynchroniseerde steekproeven tussen client en server, zodat, wanneer u gebeurtenissen in onderzoekt [zoeken](../../azure-monitor/app/diagnostic-search.md), kunt u navigeren tussen gerelateerde gebeurtenissen op de client en server, zoals paginaweergaven en http-aanvragen.
* U er zeker van bent van de juiste voorbeeldpercentage voor uw app. Deze moet hoog genoeg zijn om nauwkeurige metrische gegevens ophalen, maar hieronder de snelheid die uw prijzen quota en limieten voor de beperking overschreden. 

**Gebruik adaptieve steekproeven:**

Als de voorwaarden voor het gebruik van de andere vormen van het samplen bijhouden niet van toepassing, raden wij adaptieve steekproeven. Deze instelling is standaard ingeschakeld in de server-ASP.NET/ASP.NET Core SDK. Het wordt verkeer niet beperken tot een bepaalde minimumtarief is bereikt, daarom laag gebruik sites worden niet beïnvloed.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Hoe weet ik of steekproeven uitgevoerd worden?

Voor het detecteren van de werkelijke samplingfrequentie, ongeacht waar deze is toegepast, gebruikt u een [analysequery](../../azure-monitor/app/analytics.md) zoals deze:

```
union requests,dependencies,pageViews,browserTimings,exceptions,traces
| where timestamp > ago(1d)
| summarize RetainedPercentage = 100/avg(itemCount) by bin(timestamp, 1h), itemType
```

Als RetainedPercentage voor elk type minder dan 100, is wordt dat item worden steekproeven genomen.

**Application Insights biedt geen voorbeeld van een sessie, metrische gegevens en prestatiemeteritems telemetrietypen in een steekproef nemen technieken die hierboven worden beschreven. Deze typen zijn altijd uitgesloten van steekproeven zoals vermindering van de precisie mag maximaal ongewenste voor deze telemetrietypen**

## <a name="how-does-sampling-work"></a>Hoe werkt de steekproef nemen?

Vast aantal steekproeven functie van de SDK in ASP.NET-versies van 2.0.0 en Java SDK versie 2.0.1 en en hoger. Adaptieve steekproeven is een functie van de SDK in ASP.NET-versies van 2.0.0 en hoger. Opnamesteekproeven is een functie van de Application Insights-service, en kan zich in de bewerking als de SDK niet bezig is met steekproeven.

Het samplingalgoritme besluit welke telemetrie-items om te verwijderen, en welke u wilt bewaren (of het nu in de SDK of in de Application Insights-service). De beslissing van lijnen is gebaseerd op verschillende regels die zijn bedoeld om u te overal met elkaar verbonden gegevens intact blijven, onderhouden van een uiterst nuttig in Application Insights die praktische en betrouwbare zelfs met een beperkte set gegevens behouden. Bijvoorbeeld, als uw app verzendt voor een mislukte aanvraag extra telemetrie-items (zoals uitzonderingen en traceringen van deze aanvraag is vastgelegd), wordt steekproeven niet splitsen deze aanvraag en andere telemetrie. Deze houdt of ze alles bij elkaar komt. Wanneer u de details van de aanvraag in Application Insights bekijkt, kunt u als gevolg hiervan altijd de aanvraag, samen met de bijbehorende telemetrie-items zien. 

De beslissing van lijnen is gebaseerd op de bewerkings-ID van de aanvraag, wat betekent dat alle telemetrie-items die behoren tot een bepaalde bewerking wordt behouden of verwijderd. Voor de telemetrie-items die u geen bewerking hebt legt ID instellen (voor voorbeeld telemetrie-items is gerapporteerd door asynchrone threads met geen http-context) steekproeven gewoon een percentage van de telemetrie-items van elk type. Vóór u 2.5.0-beta2 van .NET SDK, en 2.2.0-beta3 van ASP.NET Core-SDK, de beslissing van lijnen is gebaseerd op de hash van de gebruikers-ID voor toepassingen die definiëren 'gebruiker' (dat wil zeggen, meestal web-apps). De beslissing van lijnen is voor de typen toepassingen die gebruikers (zoals webservices) hebt gedefinieerd op basis van de bewerkings-ID van de aanvraag.

Wanneer telemetrie weer aan u gepresenteerd, past de Application Insights-service de metrische gegevens die door dezelfde kan het steekproefpercentage die op het moment van de verzameling, is gebruikt om te compenseren voor de ontbrekende gegevenspunten. Wanneer de telemetrie in Application Insights bekijkt, worden de gebruikers daarom statistisch correcte benaderingen die dicht bij de reële getallen bent zien.

De nauwkeurigheid van de aanpassing is grotendeels afhankelijk van de geconfigureerde voorbeeldpercentage. De nauwkeurigheid verhoogt ook, voor toepassingen die werken met een groot aantal in het algemeen vergelijkbaar aanvragen van een groot aantal gebruikers. Aan de andere kant voor toepassingen die niet met een aanzienlijke belasting werken, is steekproeven niet nodig als deze toepassingen gewoonlijk alle hun telemetrie verzenden kunnen bijwerkt binnen het quotum, zonder verlies van gegevens van beperking. 

> [!WARNING]
> Application Insights biedt metrische gegevens en sessies telemetrietypen niet voorbeeld. Vermindering van de precisie mag maximaal ongewenste voor deze telemetrietypen.
> 

### <a name="adaptive-sampling"></a>Adaptieve steekproeven

Adaptieve steekproeven wordt toegevoegd een onderdeel dat u controleert de huidige frequentie van de overdracht van de SDK en past u het steekproefpercentage om te proberen om binnen de maximale snelheid doel te blijven. De aanpassing wordt een nieuwe met regelmatige tussenpozen en is gebaseerd op een zwevend gemiddelde van de uitgaande verzending van fouten.

## <a name="sampling-and-the-javascript-sdk"></a>Steekproef nemen en de JavaScript-SDK

De client-side (JavaScript) SDK maakt deel uit van steekproeven in combinatie met de SDK-serverzijde vast tarief. De functionele pagina's wordt alleen aan de clientzijde telemetrie verzenden van dezelfde gebruikers waarvoor het include-bestand de beslissing genomen om te 'voorbeeld in'. Deze logica is ontworpen om de integriteit van de gebruikerssessie onderhouden tussen de client - en server-ingesteld. Als gevolg van een bepaalde telemetrie-item in Application Insights kunt u alle overige telemetrie-items voor deze gebruiker of de sessie vinden. 

*Mijn client- en serverzijde telemetrie weergeven niet gecoördineerde voorbeelden zoals u hierboven beschreven.*

* Controleer of u vast tarief steekproeven zowel op de server en client ingeschakeld.
* Zorg ervoor dat de SDK-versie 2.0 of hoger.
* Controleer of u dezelfde kan het steekproefpercentage ingesteld in de client en de server.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

*Wat is het standaardgedrag van steekproeven in ASP.NET en ASP.NET Core-SDK?*

* Als u een van de meest recente versies van de bovenstaande SDK gebruikt, is standaard met vijf telemetrie-items dat per seconde adaptieve steekproeven ingeschakeld.
  Er zijn 2 AdaptiveSamplingTelemetryProcessors standaard toegevoegd en een gebeurtenistype in steekproeven bevat en het andere type gebeurtenis uitgesloten voor steekproeven. Deze configuratie betekent dat de SDK probeert te beperken tot vijf telemetrie-items van de typen gebeurtenissen telemetrie-items en vijf telemetrie-items van alle andere soorten gecombineerd, waardoor ervoor wordt gezorgd dat gebeurtenissen afzonderlijk van andere telemetrietypen van steekproeven worden genomen. Gebeurtenissen worden gewoonlijk gebruikt voor zakelijke Telemetrie en waarschijnlijk niet moeten worden beïnvloed door diagnostische telemetrie volumes.
  
  Hieronder ziet u de standaard ApplicationInsights.Config-bestand gegenereerd. Zoals wordt beschreven, zijn er twee afzonderlijke AdaptiveSamplingTelemetryProcessor-knooppunten een hebt toegevoegd, met uitzondering van de typen gebeurtenissen, en een andere opneemt. In ASP.NET Core, is exact dezelfde standaardgedrag ingeschakeld in de code. Gebruik de voorbeelden in de vorige sectie van het document naar deze standaardwerking wijzigen.

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <ExcludedTypes>Event</ExcludedTypes>
        </Add>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <IncludedTypes>Event</IncludedTypes>
        </Add>
    </TelemetryProcessors>
    ```

*Kan telemetrie meer dan één keer worden verzameld?*

* Nee. SamplingTelemetryProcessors negeren items uit de overwegingen met betrekking tot steekproeven als het item is al een steekproef genomen. Hetzelfde geldt voor opnamesteekproeven als, niet die steekproeven van toepassing op de items die al zijn verzameld in de SDK zelf. "

*Waarom is niet een eenvoudige 'verzamelen X procent van elk telemetrietype "steekproeven?*

* Terwijl deze aanpak van steekproeven met een hoge mate van nauwkeurigheid in metrische benaderingen opgeven wilt, zou deze mogelijkheid diagnostische gegevens per gebruiker, sessie en verzoek, wat van essentieel belang voor diagnostische gegevens te conrreleren verbroken. Steekproef werkt daarom beter met 'verzamelen van alle telemetrie-items voor X procent van de app-gebruikers' of "alle telemetrie voor X procent van de app-aanvragen verzamelen" logica. Voor de telemetrie-items die niet zijn gekoppeld aan de aanvragen (zoals asynchrone verwerking op de achtergrond), de terugval is "verzamelen X procent van alle items voor elk telemetrietype." 

*Kan het steekproefpercentage na verloop van tijd wijzigen?*

* Ja, verandert adaptieve steekproeven geleidelijk het voorbeeldpercentage, op basis van de momenteel waargenomen volume van de telemetrie.

*Als ik gebruik vast tarief steekproeven, hoe weet ik welke steekproeven percentage werkt het beste voor mijn app?*

* Één manier is te beginnen met adaptieve steekproeven, Ontdek wat vereffent beoordelen op (Zie de bovenstaande vraag) en ga vervolgens naar een vast tarief steekproef op basis van deze snelheid. 
  
    U hoeft niet anders te bedenken. Analyseren van uw huidige gebruik van de telemetrie in Application Insights, bekijk beperking die zich voordoet en maak een schatting van het volume van de verzamelde telemetrie. Deze drie invoer, samen met de geselecteerde prijscategorie voorstellen hoeveel u wilt mogelijk beperken van het volume van de verzamelde telemetrie. Echter, een toename van het nummer van uw gebruikers- of enige andere verschuiving van de hoeveelheid telemetrie mogelijk uw schatting ongeldig.

*Wat gebeurt er als ik voorbeeldpercentage te laag configureren?*

* Uitzonderlijk lage voorbeeldpercentage (strategieën steekproef nemen) vermindert de nauwkeurigheid van de benaderingen, als Application Insights wordt geprobeerd om te compenseren van de visualisatie van de gegevens voor de volume-vermindering van gegevens. Ook diagnostische ervaring kan dit een negatieve invloed, zoals sommige weinig mislukte of trage aanvragen mogen voorbeelden worden gemaakt uit.

*Wat gebeurt er als ik voorbeeldpercentage te hoog configureren?*

* Configureren (niet agressief genoeg) te hoog voorbeeldpercentage resulteert in een onvoldoende vermindering van het volume van de verzamelde telemetrie. Verlies van de telemetrie-gegevens met betrekking tot de beperking kan nog steeds optreden, en de kosten van het gebruik van Application Insights kan hoger zijn dan u vanwege de kosten voor overschrijding worden gepland zijn.

*Op welke platforms kan ik steekproeven gebruiken?*

* Opnamesteekproeven automatisch kan worden uitgevoerd voor alle telemetrie boven een bepaalde volume als de SDK niet bezig is met steekproeven. Deze configuratie zou bijvoorbeeld werken als u een oudere versie van de SDK voor ASP.NET of een eerdere versie van Java SDK(1.0.10 or before).
* Als u ASP.NET-SDK-versies 2.0.0 en hoger of ASP.NET CORE SDK versie 2.2.0 en hoger (gehost in Azure of op uw eigen server), krijgt u adaptieve steekproeven standaard, maar u kunt overschakelen naar vaste-tarief, zoals hierboven beschreven. Met vast tarief Samples synchroniseert de browser SDK automatisch dat er een steekproef van gerelateerde gebeurtenissen. 
* Als u Java SDK versie 2.0.1 of hoger, kunt u ApplicationInsights.xml om in te schakelen op een vast tarief steekproeven kunt configureren. Steekproeven is standaard uitgeschakeld. Met vast tarief Samples synchroniseert de browser SDK automatisch dat er een steekproef van gerelateerde gebeurtenissen.

*Er zijn bepaalde zeldzaam gebeurtenissen die ik altijd wil weergeven. Hoe vind ik ze voorbij de steekproefmodule?*

* De beste manier om dit te bereiken is het schrijven van een aangepaste [TelemetryProcessor](../../azure-monitor/app/api-filtering-sampling.md#filtering), waarin de `SamplingPercentage` 100 op het item telemetrie u wilt behouden, zoals hieronder wordt weergegeven. Dit zorgt ervoor dat alle steekproeven technieken dit object geen maatregelen steekproef worden genegeerd.

```csharp
    if(somecondition)
    {
        ((ISupportSampling)item).SamplingPercentage = 100;
    }
```

## <a name="next-steps"></a>Volgende stappen

* [Filteren](../../azure-monitor/app/api-filtering-sampling.md) krijgt u meer strikte controle over wat uw SDK verzendt.