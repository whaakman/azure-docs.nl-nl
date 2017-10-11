---
title: Telemetrie steekproeven in Azure Application Insights | Microsoft Docs
description: Hoe het volume van telemetrie onder controle te houden.
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: ceaeced414c9c302fba335b4578bcdcbfaef0410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="sampling-in-application-insights"></a>Steekproeven in Application Insights


Steekproeven is een functie in [Azure Application Insights](app-insights-overview.md). Dit is de aanbevolen manier om te beperken van telemetrie verkeer en opslag, behoud een statistisch correcte analyse van toepassingsgegevens. Het filter selecteert items die zijn gerelateerd, zodat u tussen items navigeren kunt als u de diagnostische onderzoeken.
Wanneer metrische aantallen bij u in de portal worden aangebracht, zijn ze renormalized rekening te houden met de steekproeven, om te beperken van invloed zijn op de statistieken.

Steekproeven verlaagt de kosten van verkeer en gegevens en kunt u voorkomen beperking.

## <a name="in-brief"></a>Kort gezegd:
* Steekproeven behoudt 1 in  *n*  registreert en de rest wordt genegeerd. Het kan bijvoorbeeld een samplefrequentie van 20% 1: 5 gebeurtenissen behouden. 
* Steekproeven gebeurt automatisch als uw toepassing veel telemetrie, in ASP.NET web server-apps verzendt.
* U kunt ook instellen steekproef nemen handmatig, hetzij in de portal op de pagina met prijzen; of in de ASP.NET-SDK in het .config-bestand ook het om netwerkverkeer te beperken.
* Als u aangepaste gebeurtenissen en u ervoor zorgen wilt dat een verzameling van gebeurtenissen wordt behouden of verwijderd samen, zorg dat zij dezelfde OperationId waarde hebben.
* De deler steekproeven  *n*  in elke record in de eigenschap wordt gerapporteerd `itemCount`, die in de zoekopdracht wordt weergegeven onder de beschrijvende naam van de aanvraag 'aantal' of 'gebeurtenis aantal'. Wanneer steekproeven niet uitgevoerd is, `itemCount==1`.
* Als u analysequery's schrijft, moet u [rekening houden met steekproeven](app-insights-analytics-tour.md#counting-sampled-data). In het bijzonder in plaats van gewoon telling registreert, moet u `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Typen steekproeven
Er zijn drie steekproeven van alternatieve methoden:

* **Adaptieve steekproeven** automatisch het volume van telemetrie uit de SDK in uw ASP.NET-app verzonden. De standaardinstelling van SDK v 2.0.0-beta3. Momenteel beschikbaar voor ASP.NET-serverzijde telemetrie alleen. 
* **Vast aantal steekproeven** vermindert het volume van telemetrie verzonden van zowel uw ASP.NET-server en de browsers van uw gebruikers. Stelt u het tarief. De client en server synchroniseert de steekproeven zodat die, in zoekopdracht u tussen gerelateerde paginaweergaven en aanvragen navigeren kunt.
* **Opname steekproeven** werkt in de Azure-portal. Het aantal de telemetrie van uw app, met een frequentie die u instelt binnenkomt wordt verwijderd. Niet verminderen het verkeer van telemetrie, maar u kunt houden binnen uw maandelijkse quotum. Het grote voordeel van opname steekproeven is dat u dit instellen kunt zonder dat uw app en het op uniforme wijze voor alle servers en clients werkt. 

Als Adaptief of vast aantal steekproeven in bewerking, is opname steekproeven uitgeschakeld.

## <a name="ingestion-sampling"></a>Opname steekproeven
Deze vorm van steekproeven werkt op het punt waar de telemetrie van uw webserver, browsers en apparaten bereikt voor het Application Insights-service-eindpunt. Hoewel dit niet de telemetrie verkeer dat wordt verzonden vanuit uw app beperken, het minder verwerkt en bewaard (en in rekening gebracht voor) door Application Insights.

Gebruik dit type steekproeven als uw app vaak via het maandelijkse quotum wordt en u de optie hoeft van het gebruik van een van de typen SDK op basis van steekproeven. 

De samplingfrequentie in de quota en prijzen van blade ingesteld:

![Vanuit de blade overzicht van toepassing, klik op instellingen, Quota, voorbeelden, en vervolgens een samplefrequentie selecteren en klik op bijwerken.](./media/app-insights-sampling/04.png)

Net als andere soorten steekproeven behoudt de algoritme gerelateerde telemetrie-items. Bijvoorbeeld, wanneer u de telemetrie in de zoekopdracht bekijken wilt, zult u kunnen vinden van de aanvraag die betrekking hebben op een bepaalde uitzondering. Metriek telt zoals aanvraagsnelheid en snelheid van de uitzondering correct blijven behouden.

Gegevenspunten die zijn verwijderd door steekproeven zijn niet beschikbaar in de Application Insights-functies, zoals [continue Export](app-insights-export-telemetry.md).

Opname steekproeven functioneren niet terwijl er op basis van SDK adaptieve of vast aantal steekproeven wordt uitgevoerd. Als u de samplingfrequentie op de SDK is minder dan 100%, wordt de samplefrequentie voor opname die u instelt genegeerd.

> [!WARNING]
> De waarde die wordt weergegeven op de tegel geeft de waarde die u voor de opname steekproeven instellen. Het vertegenwoordigen niet de werkelijke samplefrequentie als SDK steekproeven uitgevoerd wordt.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Adaptieve steekproeven op uw webserver
Adaptieve steekproeven is beschikbaar voor de Application Insights-SDK voor ASP.NET v 2.0.0-beta3 of hoger, en is standaard ingeschakeld. 

Adaptieve steekproeven is van invloed op het volume van de telemetrie van uw web-app-server naar de Application Insights-service verzonden. Het volume wordt automatisch aangepast behouden in de opgegeven maximale overdrachtssnelheid van verkeer.

Deze werkt niet op lage volumes telemetrie, dus een app in foutopsporing of een website met geringe gebruiksduur, worden niet beïnvloed.

Zodat het doelvolume enkele van de telemetrie die is gegenereerd wordt genegeerd. Maar net als andere soorten steekproeven, behoudt de algoritme gerelateerde telemetrie-items. Bijvoorbeeld, wanneer u de telemetrie in de zoekopdracht bekijken wilt, zult u kunnen vinden van de aanvraag die betrekking hebben op een bepaalde uitzondering. 

Metriek telt zoals aanvraagsnelheid en snelheid van de uitzondering worden aangepast om te compenseren voor de samplefrequentie zodat ze ongeveer juiste waarden in de metriek Explorer tonen.

**Bijwerken van uw project NuGet** pakketten naar de meest recente *voorlopige versie* versie van Application Insights: met de rechtermuisknop op het project in Solution Explorer, kiest u NuGet-pakketten beheren controleren **opnemen voorlopige** en zoek naar Microsoft.ApplicationInsights.Web. 

In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), kunt u verschillende parameters in de `AdaptiveSamplingTelemetryProcessor` knooppunt. De cijfers die wordt weergegeven, zijn de standaardwaarden:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    De doel-snelheid waarmee de adaptieve algoritme is erop gericht voor **op elke server-host**. Als uw web-app wordt uitgevoerd op veel hosts, verlaagt u deze waarde om te voorkomen binnen de snelheid van uw doel van het verkeer op de Application Insights-portal blijven.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Het interval waarmee de huidige frequentie van telemetrie opnieuw geëvalueerd is. Evaluatie wordt uitgevoerd als een zwevend gemiddelde. U kunt dit interval verkorten als uw telemetrie onderhevig aan plotselinge bursts is.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Wanneer steekproef nemen percentage waarde verandert, hoe snel nadat we mogen voorbeeldpercentage opnieuw om vast te leggen minder gegevens te verlagen.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Wanneer steekproef nemen percentage waarde verandert, hoe snel nadat we mogen steekproeven percentage opnieuw voor het vastleggen van meer gegevens te verhogen.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Wat is de minimumwaarde mag we ingesteld als percentage steekproef varieert.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Als percentage steekproef varieert, is wat de toegestane maximumwaarde we klaar om in te stellen.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    In de berekening van zwevend gemiddelde, het gewicht aan de meest recente waarde toegewezen. Gebruik een waarde die gelijk is aan of kleiner is dan 1. Lagere waarden wordt de algoritme van minder reactieve naar plotselinge wijzigingen aanbrengen.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    De waarde die wordt toegewezen wanneer de app NET is gestart. Niet verlaagt dit terwijl u fouten opspoort. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Puntkomma's gescheiden lijst van typen die u niet wilt op te nemen. Typen zijn herkend: afhankelijkheid, gebeurtenis, uitzondering, PageView, aanvraag, Trace. Alle exemplaren van de opgegeven typen worden verzonden; de typen die niet zijn opgegeven, worden vastgelegd.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Puntkomma's gescheiden lijst met typen die u wenst op te nemen. Typen zijn herkend: afhankelijkheid, gebeurtenis, uitzondering, PageView, aanvraag, Trace. De opgegeven typen steekproeven worden genomen; alle exemplaren van de andere typen altijd verzonden.


**Uitschakelen** adaptieve steekproef nemen, verwijdert u het knooppunt AdaptiveSamplingTelemetryProcessor uit applicationinsights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatief: adaptieve steekproeven in code configureren
In plaats van steekproeven in het .config-bestand aanpassen, kunt u code. Hiermee kunt u opgeven van een callbackfunctie die wordt aangeroepen wanneer de samplefrequentie opnieuw geëvalueerd wordt. U kunt dit bijvoorbeeld gebruiken om erachter te komen welke samplefrequentie wordt gebruikt.

Verwijder de `AdaptiveSamplingTelemetryProcessor` knooppunt uit het .config-bestand.

*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Meer informatie over de telemetrie processors](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Steekproef voor webpagina's met JavaScript
U kunt de webpagina's voor vast tarief steekproeven vanaf een willekeurige server configureren. 

Wanneer u [de webpagina's configureren voor Application Insights](app-insights-javascript.md), wijzigt u het codefragment die u via de Application Insights-portal. (In ASP.NET-apps het fragment doorgaans gaat in _Layout.cshtml.)  Voeg een regel zoals `samplingPercentage: 10,` voordat de instrumentatiesleutel:

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

Kies een percentage dat bijna 100/N waarbij N staat voor een geheel getal voor het voorbeeldpercentage.  Momenteel wordt biedt geen ondersteuning voor andere waarden.

Als u tevens vast aantal steekproeven op de server inschakelt, worden de clients en de server worden gesynchroniseerd zodat die, in zoekopdracht u tussen gerelateerde paginaweergaven en aanvragen navigeren kunt.

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Vast aantal steekproeven voor ASP.NET-websites
Vast tarief steekproeven vermindert het verkeer van uw webserver en webbrowsers worden verzonden. In tegenstelling tot adaptieve steekproeven wordt telemetrie tegen een vast tarief dat u hebt besloten gereduceerd. Het ook synchroniseert de client en server steekproeven zodat verwante items worden bewaard - bijvoorbeeld, zodat als u de weergave van een pagina in de zoekopdracht bekijkt, kunt u de gerelateerde aanvraag vinden.

Het algoritme steekproeven behoudt verwante items. Voor elke aanvraag HTTP-worden gebeurtenissen, deze en gerelateerde gebeurtenissen genegeerd of verzonden. 

In Metrics Explorer tarieven zoals aanvragen en uitzonderingen aantallen vermenigvuldigd met een factor om te compenseren voor de samplefrequentie zodat ze ongeveer juist zijn.

1. **Bijwerken van uw project NuGet-pakketten** naar de meest recente *voorlopige versie* versie van Application Insights. Met de rechtermuisknop op het project in Solution Explorer, kiest u NuGet-pakketten beheren controleren **Include prerelease** en zoek naar Microsoft.ApplicationInsights.Web. 
2. **Uitschakelen van adaptieve steekproeven**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), verwijderen of uitcommentariëren de `AdaptiveSamplingTelemetryProcessor` knooppunt.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. **De module vast tarief steekproeven inschakelen.** Voeg dit codefragment aan [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> Kies een percentage dat bijna 100/N waarbij N staat voor een geheel getal voor het voorbeeldpercentage.  Momenteel wordt biedt geen ondersteuning voor andere waarden.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternatief: vast tarief steekproeven in uw servercode inschakelen
In plaats van de parameter steekproeven instellen in het .config-bestand, kunt u code. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Meer informatie over de telemetrie processors](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-to-use-sampling"></a>Het gebruik van steekproeven?
Adaptieve steekproeven wordt automatisch ingeschakeld als u de ASP.NET-SDK-versie 2.0.0-beta3 of hoger. Ongeacht welke SDK versie die u gebruikt, kunt u opname steekproeven (op onze server).

U hoeft niet steekproeven voor de meeste grootte voor kleine en middelgrote toepassingen. De nuttigste diagnostische gegevens en de meest nauwkeurige statistieken worden verkregen door het verzamelen van gegevens op alle activiteiten van gebruikers. 

De belangrijkste voordelen van steekproeven zijn:

* Application Insights-service verwijdert ('vertragingen') gegevenspunten wanneer uw app verzendt een zeer hoge frequentie van telemetrie in korte tijd interval. 
* Om te behouden in de [quotum](app-insights-pricing.md) van gegevenspunten voor uw prijscategorie. 
* Om netwerkverkeer te beperken van de verzameling van telemetrie. 

### <a name="which-type-of-sampling-should-i-use"></a>Welk type steekproeven moet ik gebruiken?
**Gebruik de opname steekproef nemen als:**

* U doorlopen uw maandelijkse quotum telemetrie vaak.
* U gebruikt een versie van de SDK die biedt geen ondersteuning voor sampling - bijvoorbeeld, de Java SDK of ASP.NET-versies ouder is dan 2.
* U krijgt een groot aantal telemetrie van uw gebruikers webbrowsers.

**Gebruik vast tarief steekproef nemen als:**

* U gebruikt de Application Insights-SDK voor ASP.NET-web-services versie 2.0.0 of hoger, en
* U wilt dat gesynchroniseerde meting tussen client en server, zodat, wanneer u gebeurtenissen in onderzoeken [Search](app-insights-diagnostic-search.md), kunt u navigeren tussen gerelateerde gebeurtenissen op de client en server, zoals paginaweergaven en http-aanvragen.
* Bent u er zeker van zijn van het juiste voorbeeldpercentage voor uw app. Moet hoog genoeg zijn voor het verkrijgen van nauwkeurige meetgegevens, maar lager dan de frequentie die overschrijdt het quotum voor uw prijscategorie en de bandbreedteregeling limieten. 

**Gebruik adaptieve steekproeven:**

Anders wordt aangeraden adaptieve steekproeven. Dit is standaard ingeschakeld in de ASP.NET-server SDK-versie 2.0.0-beta3 of hoger. Het verminderen niet het verkeer totdat een bepaalde minimale frequentie, zodat dit geen invloed op een site intensief wordt gebruikt.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Hoe weet ik of steekproeven uitgevoerd wordt?
Voor het detecteren van de werkelijke samplefrequentie ongeacht waar deze is toegepast, gebruikt u een [Analytics query](app-insights-analytics.md) zoals deze:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

In elk bewaard record, `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt, gelijk zijn aan 1 + het aantal eerdere verwijderde records. 

## <a name="how-does-sampling-work"></a>Hoe werkt steekproeven?
Vaste snelheid en adaptieve steekproeven zijn een functie van de SDK in ASP.NET-versies van 2.0.0 en hoger. Opname steekproeven is een functie van de Application Insights-service en kan zich in de bewerking als de SDK niet steekproeven uitvoert. 

Het algoritme steekproeven besluit welke telemetrie-items verwijderen en welke u wilt behouden (ongeacht of deze in de SDK of in de Application Insights-service is). De beslissing steekproeven is gebaseerd op verschillende regels die richt u op alle verwante gegevenspunten intact onderhouden van een diagnostische ervaring in Application Insights die actie worden uitgevoerd en betrouwbare zelfs met een beperkte set gegevens behouden. Bijvoorbeeld, als voor een mislukte aanvraag verzendt uw app aanvullende telemetrie-items (zoals uitzondering en traceringen van deze aanvraag wordt vastgelegd), steekproeven niet gesplitst deze aanvraag en andere telemetrie. Het behouden of ze elkaar zakt. Wanneer u de aanvraagdetails van de in Application Insights bekijkt, kunt u als gevolg hiervan altijd de aanvraag samen met de bijbehorende telemetrie-items zien. 

Voor toepassingen die definiëren 'gebruiker' (dat wil zeggen, meest voorkomende webtoepassingen), de beschikking steekproeven is gebaseerd op de hash van de gebruikers-id, wat betekent dat alle telemetrie voor een bepaalde gebruiker wordt behouden of verwijderd. De beslissing steekproeven is voor de typen toepassingen die geen gebruikers (zoals web-services) opgeven op basis van de bewerkings-id van de aanvraag. Ten slotte voor de telemetrie-items waarvoor geen gebruiker noch bewerking-id ingesteld (voor voorbeeld telemetrie-items is gerapporteerd door asynchrone threads met geen http-context) bevat steekproeven gewoon een percentage van de telemetrie-items van elk type. 

Bij het weergeven van telemetrie naar u terug, past de Application Insights-service de metrische gegevens die door de dezelfde voorbeeldpercentage die is gebruikt op het moment van verzameling, om te compenseren voor de ontbrekende gegevenspunten. Daarom bij het onderzoeken van de telemetrie in Application Insights, zien de gebruikers statistisch correcte benaderingen die heel dicht bij de reële getallen zijn.

De nauwkeurigheid van de aanpassing is grotendeels afhankelijk van het geconfigureerde voorbeeldpercentage. De nauwkeurigheid verhoogt ook, voor toepassingen die een groot aantal in het algemeen soortgelijke aanvragen van een groot aantal gebruikers verwerken. Aan de andere kant voor toepassingen die niet met een aanzienlijke belasting werken is steekproeven niet nodig als u deze toepassingen kunnen doorgaans alle hun telemetrie verzenden bijwerkt binnen het quotum zonder verlies van gegevens van de beperking. 

Houd er rekening mee dat Application Insights biedt geen telemetrie-typen, metrische gegevens en sessies, sinds het voorbeeld voor deze typen, vermindering van de precisie mag maximaal ongewenste. 

### <a name="adaptive-sampling"></a>Adaptieve steekproeven
Adaptieve steekproeven wordt toegevoegd een component die wordt bewaakt de huidige frequentie van de overdracht van de SDK en past u het voorbeeldpercentage om te blijven binnen de maximale snelheid doel. De aanpassing is berekend met regelmatige tussenpozen en is gebaseerd op een zwevend gemiddelde van de uitgaande verzending snelheid.

## <a name="sampling-and-the-javascript-sdk"></a>Steekproeven en de JavaScript-SDK
De client (JavaScript) SDK deelneemt aan vast tarief steekproeven in combinatie met de SDK-serverzijde. De providers's verzendt alleen clientzijde telemetrie uit dezelfde gebruikers waarvoor het include-bestand de beslissing genomen om te 'voorbeeld in'. Deze logica is ontworpen om de integriteit van de gebruikerssessie onderhouden tussen de client - en server-zijde. Als gevolg hiervan op een bepaalde telemetrie-item in de Application Insights kunt u alle overige telemetrie-items voor deze gebruiker of de sessie vinden. 

*Client- en serverzijde telemetrie weergeven niet gecoördineerde voorbeelden zoals hierboven worden beschreven.*

* Controleer of u vast aantal steekproeven op server en client ingeschakeld.
* Zorg ervoor dat de SDK-versie 2.0 of hoger.
* Controleer of u de dezelfde voorbeeldpercentage ingesteld in de client en de server.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
*Waarom wordt niet wordt een eenvoudige 'verzamelen X procent van elk type telemetrie'?*

* Terwijl deze benadering steekproeven met een zeer hoge precisie in metrische benaderingen opgeven wilt, zou het correleren van diagnostische gegevens per gebruiker, sessie en verzoek, wat van essentieel belang voor diagnostische gegevens kunnen worden verbroken. Steekproef werkt daarom beter met "verzamelen alle telemetrie-items voor X procent van de app-gebruikers" of 'verzamelen alle telemetrie voor X percentage aanvragen dat app' logica. Voor de telemetrie-items niet zijn gekoppeld aan de verzoeken (zoals achtergrond asynchrone verwerking), is het de terugvallen te "verzamelen X percentage van alle items voor elk type telemetrie." 

*Kan het voorbeeldpercentage na verloop van tijd wijzigen?*

* Ja, verandert adaptieve steekproeven geleidelijk het voorbeeldpercentage op basis van de momenteel waargenomen volume van de telemetrie.

*Als ik vast tarief steekproeven gebruikt, hoe weet ik welke steekproeven percentage werkt het meest geschikt voor mijn app?*

* Een manier worden gestart met adaptieve steekproeven, uitzoeken wat waardering geven vereffent op (Zie de bovenstaande vraag) en ga vervolgens naar vast tarief voert sampling uit met behulp van dit percentage. 
  
    U hebt anders te achterhalen. Het gebruik van uw huidige telemetrie in AI analyseren, observeren beperking die zich voordoet en schatting maken van het volume van de verzamelde telemetrie. Deze drie invoer, samen met de geselecteerde prijscategorie voorgesteld hoeveel kunt u het volume van de verzamelde telemetrie verminderen. Echter, een toename van het nummer van uw gebruikers- of enige andere verschuiving van het volume van telemetrie mogelijk uw schatting ongeldig.

*Wat gebeurt er als ik voorbeeldpercentage te laag configureren?*

* Uitzonderlijk lage voorbeeldpercentage (over-aggressive steekproeven) vermindert de nauwkeurigheid van de benaderingen, wanneer Application Insights probeert om te compenseren de visualisatie van de gegevens voor het volume gegevens verminderen. Ook diagnostische ervaring mogelijk negatieve gevolgen hebben, zoals enkele van de aanvragen zelden beschadigde of traag mogen voorbeelden worden gemaakt uit.

*Wat gebeurt er als ik voorbeeldpercentage te hoog configureren?*

* Configureren (niet agressief genoeg) te hoog voorbeeldpercentage resulteert in een onvoldoende vermindering van het volume van de verzamelde telemetrie. Telemetrie gegevensverlies gerelateerd aan de beperking kunnen nog steeds optreden en de kosten van het gebruik van Application Insights is mogelijk hoger is dan u geplande vanwege overschrijding kosten.

*Op welke platforms kan ik steekproeven gebruiken?*

* Opname steekproeven kan automatisch voor alle telemetrie boven een bepaald volume optreden als de SDK niet bezig is met steekproeven. Dit zou bijvoorbeeld werken als uw app gebruikmaakt van een Java-server, of als u een oudere versie van de ASP.NET-SDK.
* Als u ASP.NET-SDK-versies 2.0.0 en hoger (gehost in Azure of een eigen server), krijgt u adaptieve steekproef nemen standaard, maar u kunt overschakelen naar vast tarief zoals hierboven is beschreven. Met vast tarief steekproeven, de browser SDK worden automatisch gesynchroniseerd voor een steekproef van gerelateerde gebeurtenissen. 

*Er zijn bepaalde zeldzame gebeurtenissen die ik altijd wilt zien. Hoe krijg ik ze voorbij de module steekproeven?*

* Initialiseren van een afzonderlijk exemplaar van TelemetryClient met een nieuwe TelemetryConfiguration (niet de standaard actief). Gebruikt u dat uw zeldzame gebeurtenissen verzendt.

## <a name="next-steps"></a>Volgende stappen
* [Filteren](app-insights-api-filtering-sampling.md) krijgt u meer strikte controle over wat uw SDK verzendt.

