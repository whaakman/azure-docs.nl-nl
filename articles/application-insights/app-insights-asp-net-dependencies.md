---
title: Afhankelijkheid bijhouden in Azure Application Insights | Microsoft Docs
description: Analyseer het gebruik, de beschikbaarheid en de prestaties van uw on-premises webtoepassing of Microsoft Azure-webtoepassing met Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: 060f1c9d2c74ed45e8077ec99503a1d7b885d325
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="set-up-application-insights-dependency-tracking"></a>Application Insights instellen: afhankelijkheid bijhouden
Een *afhankelijkheid* is een externe component die wordt aangeroepen door uw app. Dit is doorgaans een service die is aangeroepen met behulp van HTTP, of een database of een bestandssysteem. [Application Insights](app-insights-overview.md) maatregelen hoe lang de toepassing moet wachten voor afhankelijkheden en hoe vaak een afhankelijkheidsaanroep is mislukt. U kunt specifieke aanroepen te onderzoeken en koppelen aan aanvragen en uitzonderingen.

![voorbeeldgrafieken](./media/app-insights-asp-net-dependencies/10-intro.png)

De afhankelijkheidsmonitor voor out-of-the-box rapporten momenteel aanroepen naar deze typen afhankelijkheden:

* Server
  * SQL-databases
  * ASP.NET-webtoepassingen en WCF-services die gebruikmaken van bindingen op basis van HTTP
  * Lokale of externe HTTP-aanroepen
  * Azure DB Cosmos, tabel, blob-opslag en wachtrij
* Webpagina's
  * AJAX-aanroepen

Bewaking werkt met [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) rond geselecteerde methoden. Prestatieoverhead is minimaal.

U kunt ook uw eigen SDK-aanroepen voor het bewaken van andere afhankelijkheden, zowel op de client en server-code schrijven met behulp van de [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Bewaking van afhankelijkheid ingesteld
Gedeeltelijke afhankelijkheidsgegevens worden verzameld, automatisch door de [Application Insights-SDK](app-insights-asp-net.md). Als u alle gegevens, installeert u de desbetreffende agent voor de hostserver.

| Platform | Installeren |
| --- | --- |
| IIS Server |Beide [Status Monitor installeren op uw server](app-insights-monitor-performance-live-website-now.md) of [Upgrade van uw toepassing naar .NET framework 4.6 of hoger](http://go.microsoft.com/fwlink/?LinkId=528259) en installeer de [Application Insights-SDK](app-insights-asp-net.md) in uw app. |
| Azure Web App |In het Configuratiescherm voor web-app [de Application Insights-blade geopend in het Configuratiescherm van de web-app](app-insights-azure-web-apps.md) en kies installeren als u wordt gevraagd. |
| Azure Cloud Service |[Gebruik opstarttaak](app-insights-cloudservices.md) of [installeren .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Waar vind ik afhankelijkheidsgegevens
* [De toepassingstoewijzing](#application-map) visualiseren van afhankelijkheden tussen uw app en de aangrenzende onderdelen.
* [Prestaties, browser en fout blades](#performance-and-blades) server afhankelijkheidsgegevens worden weergegeven.
* [Blade browsers](#ajax-calls) AJAX-aanroepen vanuit de browsers van uw gebruikers bevat.
* [Klik door vanuit traag of mislukte aanvragen](#diagnose-slow-requests) aanroepen om te controleren van de afhankelijkheid.
* [Analytics](#analytics) kan worden gebruikt om gegevens van de afhankelijkheid opvragen.

## <a name="application-map"></a>Toepassingskaart
De toepassingstoewijzing fungeert als een visuele hulpmiddel voor het detecteren van afhankelijkheden tussen de onderdelen van uw toepassing. Deze wordt automatisch gegenereerd vanuit de telemetrie van uw app. Dit voorbeeld toont AJAX-aanroepen vanuit de browser-scripts en het aanroepen van de REST van de server-app voor twee externe services.

![Toepassingskaart](./media/app-insights-asp-net-dependencies/08.png)

* **Navigeer in de vakken** relevante afhankelijkheid en andere grafieken.
* **De kaart vastmaken** naar de [dashboard](app-insights-dashboards.md), wordt waar deze volledig functioneel.

[Meer informatie](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Prestatie- en blades
De blade performance toont de duur van afhankelijkheidsaanroepen die door de server-app. Er is een overzichtstabel en een tabel gesegmenteerd op aanroep.

![Afhankelijkheid van de prestatiegrafieken-blade](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Klik in de samenvatting grafieken of de tabelitems om te zoeken onbewerkte instanties van deze aanroepen.

![Afhankelijkheid aanroep exemplaren](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Foutaantallen** worden weergegeven op de **fouten** blade. Een mislukte is een retourcode die zich niet in het bereik 200-399, of een onbekend.

> [!NOTE]
> **100% fouten?** -Dit wordt waarschijnlijk geeft aan dat u alleen afhankelijkheidsgegevens van gedeeltelijke krijgt. U moet [instellen van de bewaking van afhankelijkheid geschikt is voor uw platform](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-aanroepen
De blade Browsers toont de duur en mislukt de frequentie van AJAX-aanroepen vanuit [JavaScript in uw webpagina's](app-insights-javascript.md). Deze worden weergegeven als afhankelijkheden.

## <a name="diagnosis"></a>Diagnose van trage aanvragen
Elke gebeurtenis van de aanvraag is gekoppeld aan het aanroepen van afhankelijkheden, uitzonderingen en andere gebeurtenissen die worden bijgehouden, terwijl de aanvraag is verwerkt door uw app. Dus als bepaalde aanvragen onjuist uitvoert, kunt u vinden of deze is als gevolg van trage reacties van een afhankelijkheid.

We gaan een voorbeeld van die doorlopen.

### <a name="tracing-from-requests-to-dependencies"></a>Tracering van aanvragen naar afhankelijkheden
Open de blade Performance en bekijk het raster van aanvragen:

![Lijst met aanvragen met gemiddelden en tellingen](./media/app-insights-asp-net-dependencies/02-reqs.png)

De bovenste een duurt zeer lang. Laten we zien als we vindt waarbij de tijd is besteed.

Klik op die rij om afzonderlijke aanvraag gebeurtenissen te bekijken:

![Lijst van instanties van de aanvraag](./media/app-insights-asp-net-dependencies/03-instances.png)

Klik op een willekeurig exemplaar langlopende om verdere inspecteren en schuif omlaag naar de externe afhankelijkheidsaanroepen die betrekking hebben op deze aanvraag:

![Vinden van aanroepen naar externe afhankelijkheden, het identificeren van ongebruikelijke duur](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Het lijkt erop dat de meeste van de tijd van onderhoud van deze aanvraag is die is doorgebracht in een aanroep van een lokale service.

Selecteer die rij voor meer informatie:

![Deze externe afhankelijkheid voor het identificeren van de stackpad doorklikken](./media/app-insights-asp-net-dependencies/05-detail.png)

Lijkt erop dat dit is waar het probleem is. We het probleem hebt nauwkeurige, dus nu we zojuist hebt om erachter te komen waarom die aanroep te lang duurt.

### <a name="request-timeline"></a>Aanvraag tijdlijn
Er is geen afhankelijkheidsaanroep die is een bijzonder lange in andere gevallen. Maar door het overschakelen naar de tijdlijnweergave, kunnen we zien waar de vertraging is opgetreden bij het verwerken van onze interne:

![Vinden van aanroepen naar externe afhankelijkheden, het identificeren van ongebruikelijke duur](./media/app-insights-asp-net-dependencies/04-1.png)

Er lijkt een gap big nadat de eerste afhankelijkheid aangeroepen, zodat we onze code om te zien waarom moet kijken.

### <a name="profile-your-live-site"></a>Profiel van uw live site

Er is geen idee waar de tijd gaat? De [Application Insights profiler](app-insights-profiler.md) traceringen HTTP-aanroepen naar uw live site en ziet u welke functies in uw code het langst heeft geduurd.

## <a name="failed-requests"></a>Mislukte aanvragen
Mislukte aanvragen kunnen ook zijn gekoppeld aan de mislukte aanroepen naar afhankelijkheden. We kunnen opnieuw in en klik in voor het opsporen van het probleem.

![Klik op de grafiek mislukte aanvragen](./media/app-insights-asp-net-dependencies/06-fail.png)

Klik verder naar een exemplaar van een mislukte aanvraag en bekijk de bijbehorende gebeurtenissen.

![Klik op een aanvraag met het type, het exemplaar voor ophalen naar een andere weergave van hetzelfde exemplaar, klikt u erop om details van uitzondering.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analytische gegevens
U kunt volgen afhankelijkheden in de [querytaal van logboekanalyse](https://docs.loganalytics.io/). Hier volgen enkele voorbeelden.

* Alle mislukte afhankelijkheidsaanroepen vinden:

```

    dependencies | where success != "True" | take 10
```

* AJAX-aanroepen vinden:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Afhankelijkheidsaanroepen verband met aanvragen zoeken:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Zoeken naar AJAX-aanroepen die zijn gekoppeld aan paginaweergaven:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Bijhouden van aangepaste afhankelijkheid
De standaard afhankelijkheid bijhouden module detecteert automatisch de externe afhankelijkheden, zoals databases en REST-API's. Maar u kunt een aantal extra onderdelen op dezelfde manier worden behandeld.

Kunt u code schrijven waarmee afhankelijkheidsinformatie, worden verzonden met behulp van dezelfde [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) dat wordt gebruikt door de standard-modules.

Als u uw code met een assembly die u zelf niet schrijven maken, kan u bijvoorbeeld alle aanroepen, om erachter te komen welke bijdrage hiervan in uw reactietijden tijd. Als u deze gegevens weergegeven in de afhankelijkheidsgrafiek in Application Insights, verzenden met behulp van `TrackDependency`.

```csharp

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

Als u de standaard afhankelijkheid bijhouden module uitschakelen wilt, verwijdert u de verwijzing naar DependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Problemen oplossen
*Afhankelijkheid geslaagd vlag altijd toont waar of ONWAAR.*

*SQL-query is niet volledig worden weergegeven.*

* Voer een upgrade naar de nieuwste versie van de SDK. Als uw versie van .NET minder dan 4.6 is:
  * IIS-host: installeren [Application Insights-Agent](app-insights-monitor-performance-live-website-now.md) op de host-servers.
  * Azure-web-app: Open Application Insights tabblad in het Configuratiescherm van de web-app en installeer Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
* [Uitzonderingen](app-insights-asp-net-exceptions.md)
* [Gebruiker- en paginagegevens toevoegen](app-insights-javascript.md)
* [Beschikbaarheid](app-insights-monitor-web-app-availability.md)
