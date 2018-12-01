---
title: Afhankelijkheid bijhouden van het Azure Application Insights | Microsoft Docs
description: Analyseer het gebruik, de beschikbaarheid en de prestaties van uw on-premises webtoepassing of Microsoft Azure-webtoepassing met Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: mbullwin
ms.openlocfilehash: 767497a6ced0eb54559b9bdd10761b659e32a33f
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52681021"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Instellen van Application Insights: afhankelijkheid bijhouden
Een *afhankelijkheid* is een externe component die wordt aangeroepen door uw app. Het is doorgaans een service die wordt aangeroepen met behulp van HTTP, of een database of een bestandssysteem. [Application Insights](app-insights-overview.md) meet hoe lang de toepassing moet wachten voor afhankelijkheden en hoe vaak een afhankelijkheidsaanroep is mislukt. U kunt specifieke aanroepen te onderzoeken en koppelen aan aanvragen en uitzonderingen.

![voorbeeldgrafieken](./media/app-insights-asp-net-dependencies/10-intro.png)

Met de afhankelijkheidsmonitor met out-of-the-box rapporten momenteel aanroepen naar deze typen afhankelijkheden:

* Server
  * SQL-databases
  * ASP.NET-web- en WCF-services die gebruikmaken van op basis van HTTP-bindingen
  * Lokale of externe HTTP-aanroepen
  * Azure Cosmos DB, tabel, blob-opslag en wachtrij
* Webpagina's
  * AJAX-aanroepen

Controle werkt met behulp van [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) rond geselecteerde methoden. Prestatieoverhead is minimaal.

U kunt ook uw eigen SDK-aanroepen voor het bewaken van andere afhankelijkheden, zowel in de client en server-code schrijven met behulp van de [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Instellen van de afhankelijkheidsbewaking
Gedeeltelijke afhankelijkheidsgegevens worden verzameld, automatisch door de [Application Insights-SDK](app-insights-asp-net.md). Als u de volledige gegevens, de juiste agent voor de host-server te installeren.

| Platform | Installeren |
| --- | --- |
| IIS-Server |Een van beide [Status Monitor installeren op uw server](app-insights-monitor-performance-live-website-now.md) of [Toepassingsupgrade naar .NET framework 4.6 of hoger](https://go.microsoft.com/fwlink/?LinkId=528259) en installeer de [Application Insights-SDK](app-insights-asp-net.md) in uw app. |
| Azure Web App |In het Configuratiescherm van web-app [opent u de Application Insights-blade in het Configuratiescherm van uw web-app](app-insights-azure-web-apps.md) en kies installeren als u hierom wordt gevraagd. |
| Azure-Cloudservice |[Gebruik opstarttaak](app-insights-cloudservices.md) of [installeren .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Waar vind ik afhankelijkheidsgegevens
* [Overzicht van de toepassing](#application-map) worden gevisualiseerd met afhankelijkheden tussen uw app en door in aangrenzende onderdelen.
* [Prestaties, browser en de fout blades](#performance-and-blades) server afhankelijkheidsgegevens weergeven.
* [Blade browsers](#ajax-calls) AJAX-aanroepen van de browsers van uw gebruikers bevat.
* [Doorklikken traag of mislukte aanvragen](#diagnose-slow-requests) aanroept om te controleren van de afhankelijkheid.
* [Analytics](#analytics) kan worden gebruikt om gegevens van de afhankelijkheid opvragen.

## <a name="application-map"></a>Toepassingskaart
Overzicht van de toepassing fungeert als een visuele hulpmiddelen voor het detecteren van afhankelijkheden tussen de onderdelen van uw toepassing. Er wordt automatisch gegenereerd van de telemetrie van uw app. Dit voorbeeld toont AJAX-aanroepen vanuit de browser-scripts en REST-aanroepen van de serverapp met twee externe services.

![Toepassingskaart](./media/app-insights-asp-net-dependencies/08.png)

* **Navigeer in de vakken** relevante afhankelijkheid en andere grafieken.
* **De kaart vastmaken** naar de [dashboard](app-insights-dashboards.md), waar deze is volledig functioneel.

[Meer informatie](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Prestaties en fout-blades
De blade prestaties wordt de duur van afhankelijkheidsaanroepen van de serverapp. Er is een grafiek met samenvattingen en een tabel gesegmenteerd op aanroep.

![Afhankelijkheid van de prestatiegrafieken-blade](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Klik in de samenvatting grafieken of de tabelitems om te zoeken naar onbewerkte instanties van deze aanroepen.

![Afhankelijkheid aanroep exemplaren](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Aantal fouten** worden weergegeven op de **fouten** blade. Een fout is een retourcode die zich niet in het bereik 200-399, of onbekend.

> [!NOTE]
> **100% fouten?** -Dit geeft waarschijnlijk aan dat u alleen gedeeltelijke afhankelijkheidsgegevens ontvangt. U moet [instellen van de controle van toepassingsafhankelijkheden geschikt is voor uw platform](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-aanroepen
De blade Browsers toont de duur en mislukt de frequentie van AJAX-aanroepen van [JavaScript in uw webpagina's](app-insights-javascript.md). Ze worden weergegeven als afhankelijkheden.

## <a name="diagnosis"></a> Diagnose van trage aanvragen
Elke aanvraaggebeurtenis is gekoppeld aan het aanroepen van afhankelijkheden, uitzonderingen en andere gebeurtenissen die worden bijgehouden, terwijl uw app de aanvraag wordt verwerkt. Dus als een ongeldige bepaalde aanvragen uitvoert, u vinden kunt of dit is vanwege de trage reacties van een afhankelijkheid.

We nemen een voorbeeld van die.

### <a name="tracing-from-requests-to-dependencies"></a>Tracering van aanvragen naar afhankelijkheden
Open de blade Performance en kijken naar het raster van aanvragen:

![Lijst met aanvragen met gemiddelden en tellingen](./media/app-insights-asp-net-dependencies/02-reqs.png)

De bovenste een duurt zeer lang. Laten we zien als we vindt waar de tijd is besteed.

Klik op die rij om afzonderlijke aanvraaggebeurtenissen te bekijken:

![Lijst met instanties van de aanvraag](./media/app-insights-asp-net-dependencies/03-instances.png)

Klik op een willekeurig exemplaar langlopende verder inspecteren en bladert u omlaag naar de externe afhankelijkheidsaanroepen die betrekking hebben op deze aanvraag:

![Aanroepen naar externe afhankelijkheden zoeken, identificeren ongebruikelijke duur](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Deze ziet eruit als de meeste van de tijd van onderhoud van die deze aanvraag is besteed in een aanroep naar een lokale service.

Selecteer de rij voor meer informatie:

![Klik op die externe afhankelijkheid voor het identificeren van het overmatig](./media/app-insights-asp-net-dependencies/05-detail.png)

Het lijkt dit is waar het probleem zich bevindt. We hebben het probleem kunt, dus nu we gewoon wilt weten waarom die aanroep zo lang duurt.

### <a name="request-timeline"></a>Tijdlijn van de aanvraag
Er is geen afhankelijkheidsaanroep die erg lang in andere gevallen. Maar door over te schakelen naar de tijdlijnweergave, kunnen we zien waar de vertraging is opgetreden in onze interne verwerking:

![Aanroepen naar externe afhankelijkheden zoeken, identificeren ongebruikelijke duur](./media/app-insights-asp-net-dependencies/04-1.png)

Er lijkt te zijn van een wijd na het aanroepen van de eerste afhankelijkheid, zodat we onze code om te zien waarom moeten kijken.

### <a name="profile-your-live-site"></a>Profileren van uw live site

Er is geen idee waar de tijd komt? De [Application Insights profiler](app-insights-profiler.md) traceringen HTTP-aanroepen naar de live site en ziet u welke functies in uw code heeft de meeste tijd.

## <a name="failed-requests"></a>Mislukte aanvragen
Mislukte aanvragen kunnen ook zijn gekoppeld aan de mislukte aanroepen van afhankelijkheden. Nogmaals, kunnen we doorklikken voor het opsporen van het probleem.

![Klik op de grafiek van mislukte aanvragen](./media/app-insights-asp-net-dependencies/06-fail.png)

Klik door naar een exemplaar van een mislukte aanvragen en de bijbehorende gebeurtenissen bekijken.

![Klik op een aanvraag met het type en het exemplaar naar een andere weergave van hetzelfde exemplaar, klikt u erop om details van uitzondering.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analyse
U kunt volgen afhankelijkheden in de [querytaal van Log Analytics](https://aka.ms/LogAnalyticsLanguage). Hier volgen enkele voorbeelden.

* Alle mislukte afhankelijkheidsaanroepen zoeken:

```

    dependencies | where success != "True" | take 10
```

* AJAX-aanroepen zoeken:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Afhankelijkheidsaanroepen die zijn gekoppeld aan aanvragen zoeken:

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



## <a name="custom-dependency-tracking"></a>Aangepaste bijhouden van afhankelijkheid
De standaard afhankelijkheid bijhouden module detecteert automatisch externe afhankelijkheden, zoals databases en REST-API's. Maar wilt u mogelijk enkele aanvullende onderdelen moeten op dezelfde manier worden behandeld.

U kunt code schrijven die afhankelijkheidsinformatie, verzendt met behulp van dezelfde [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) die wordt gebruikt door de standard-modules.

Als u uw code met een assembly die u niet zelf schrijft maken, kunt u bijvoorbeeld alle aanroepen naar deze, om erachter te komen wat de bijdrage van voor uw reactietijden wordt tijd. Als u wilt dat deze gegevens worden weergegeven in de afhankelijkheidsgrafiek in Application Insights, verzenden met behulp van `TrackDependency`.

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
                // The call above has been made obsolete in the latest SDK. The updated call follows this format:
                // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
            }
```

Als u uitschakelen in de module voor het bijhouden van standard afhankelijkheid wilt, verwijder de verwijzing naar DependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Problemen oplossen
*Afhankelijkheid succes vlag altijd wordt weergegeven waar of ONWAAR.*

*SQL-query is niet volledig worden weergegeven.*

Raadpleeg de onderstaande tabel en zorgen dat u hebt ervoor gekozen de juiste configuratie van afhankelijkheidsbewaking van voor uw toepassing wilt inschakelen.

| Platform | Installeren |
| --- | --- |
| IIS-Server |Een van beide [Status Monitor installeren op uw server](app-insights-monitor-performance-live-website-now.md). Of [Toepassingsupgrade naar .NET framework 4.6 of hoger](https://go.microsoft.com/fwlink/?LinkId=528259) en installeer de [Application Insights-SDK](app-insights-asp-net.md) in uw app. |
| Azure Web App |In het Configuratiescherm van web-app [opent u de Application Insights-blade in het Configuratiescherm van uw web-app](app-insights-azure-web-apps.md) en kies installeren als u hierom wordt gevraagd. |
| Azure-Cloudservice |[Gebruik opstarttaak](app-insights-cloudservices.md) of [installeren .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
* [Uitzonderingen](app-insights-asp-net-exceptions.md)
* [Gebruiker- en paginagegevens toevoegen](app-insights-javascript.md)
* [Beschikbaarheid](app-insights-monitor-web-app-availability.md)
