---
title: Afhankelijkheid bijhouden van het Azure Application Insights | Microsoft Docs
description: Afhankelijkheidsaanroepen bewaken van uw on-premises of de Microsoft Azure-webtoepassing met Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbullwin
ms.openlocfilehash: 479b810c5a66917bde5754d32991fb489ea26c9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66299277"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Afhankelijkheid bijhouden van het Azure Application Insights 

Een *afhankelijkheid* is een externe component die wordt aangeroepen door uw app. Het is doorgaans een service die wordt aangeroepen met behulp van HTTP, of een database of een bestandssysteem. [Application Insights](../../azure-monitor/app/app-insights-overview.md) meet de duur van afhankelijkheidsaanroepen, of zijn mislukt of niet, samen met aanvullende informatie zoals de naam van de afhankelijkheid enzovoort. U kunt specifieke afhankelijkheidsaanroepen onderzoeken en correlaties aanvragen en uitzonderingen.

## <a name="automatically-tracked-dependencies"></a>Automatisch bijgehouden afhankelijkheden

Application Insights-SDK's voor .NET en .NET Core wordt geleverd met `DependencyTrackingTelemetryModule` dit is een telemetrie-Module die automatisch worden verzameld van afhankelijkheden. Deze verzameling afhankelijkheid is automatisch ingeschakeld voor [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) en [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) toepassingen, wanneer geconfigureerd aan de hand van de gekoppelde officiële documenten. `DependencyTrackingTelemetryModule` is geleverd als [dit](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) -pakket NuGet, en is automatisch geplaatst wanneer u een van de NuGet-pakketten `Microsoft.ApplicationInsights.Web` of `Microsoft.ApplicationInsights.AspNetCore`.

 `DependencyTrackingTelemetryModule` momenteel worden de volgende afhankelijkheden automatisch bijgehouden:

|Afhankelijkheden |Details|
|---------------|-------|
|HTTP/Https | Lokale of externe http/https-aanroepen |
|WCF-aanroepen| Alleen automatisch bijgehouden als Http-gebaseerde bindingen worden gebruikt.|
|SQL | Aanroepen met `SqlClient`. Zie [dit](##advanced-sql-tracking-to-get-full-sql-query) voor het vastleggen van de SQL-query.  |
|[Azure storage (Blob, tabel, wachtrij)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Aanroepen met Azure Storage-Client. |
|[EventHub Client SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versie 1.1.0 of hoger. |
|[ServiceBus Client SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versie 3.0.0 en hoger. |
|Azure Cosmos DB | Alleen automatisch bijgehouden als HTTP/HTTPS wordt gebruikt. De TCP-modus wordt niet door Application Insights worden vastgelegd. |


## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Automatische afhankelijkheid bijhouden Console Apps instellen

Als u wilt bijhouden automatisch afhankelijkheden van.NET/.NET Core-console-apps, installeer het Nuget-pakket `Microsoft.ApplicationInsights.DependencyCollector`, en initialiseren `DependencyTrackingTelemetryModule` als volgt:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="how-automatic-dependency-monitoring-works"></a>Werking van automatische afhankelijkheid controle werkt?

Afhankelijkheden worden automatisch verzameld met behulp van een van de volgende technieken:

* Met behulp van byte code instrumentation rond Selecteer methoden. (InstrumentationEngine vanuit StatusMonitor of Azure-Web-Appuitbreiding)
* Gebeurtenisbron retouraanroepen
* DiagnosticSource callbacks (in de meest recente.NET/.NET Core SDK's)

## <a name="manually-tracking-dependencies"></a>Handmatig de afhankelijkheden bijhouden

Hier volgen enkele voorbeelden van afhankelijkheden, die niet worden automatisch verzameld, en daarom moeten handmatig bijhouden.

* Azure Cosmos DB is automatisch bijgehouden alleen als [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) wordt gebruikt. De TCP-modus wordt niet door Application Insights worden vastgelegd.
* Redis

Voor deze afhankelijkheden niet automatisch worden verzameld door de SDK, kunt u volgen ze handmatig met behulp van de [TrackDependency API](api-custom-events-metrics.md#trackdependency) die wordt gebruikt door de standaard automatisch verzameling modules.

Als u uw code met een assembly die u niet zelf schrijft maken, kunt u bijvoorbeeld alle aanroepen naar deze, om erachter te komen wat de bijdrage van voor uw reactietijden wordt tijd. Als u wilt dat deze gegevens worden weergegeven in de afhankelijkheidsgrafiek in Application Insights, verzenden met behulp van `TrackDependency`.

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

U kunt ook `TelemetryClient` biedt uitbreidingsmethoden `StartOperation` en `StopOperation` die kan worden gebruikt voor het handmatig bijhouden van afhankelijkheden, zoals [hier](custom-operations-tracking.md#outgoing-dependencies-tracking)

Als u uitschakelen in de module voor het bijhouden van standard afhankelijkheid wilt, verwijder de verwijzing naar DependencyTrackingTelemetryModule in [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) voor ASP.NET-toepassingen. Voor ASP.NET Core-toepassingen, instructies [hier](asp-net-core.md#configuring-or-removing-default-telemetrymodules).

## <a name="tracking-ajax-calls-from-web-pages"></a>AJAX-aanroepen van webpagina's bijhouden

Voor webpagina's, Application Insights JavaScript SDK verzamelt automatisch AJAX-aanroepen als afhankelijkheden zoals wordt beschreven [hier](javascript.md#ajax-performance). Dit document is gericht op de afhankelijkheden van server-onderdelen.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Geavanceerde SQL bijhouden om op te halen van volledige SQL-Query

Voor SQL-aanroepen, de naam van de server en database worden altijd verzameld en opgeslagen als de naam van de verzamelde `DependencyTelemetry`. Er is een extra veld met de naam 'gegevens', die de volledige tekst van de SQL-query kan bevatten.

Er is geen aanvullende stap vereist voor het ophalen van de volledige SQL-Query voor ASP.NET Core-toepassingen.

Voor ASP.NET-toepassingen, volledige SQL-query worden verzameld met behulp van de byte-code instrumentation, waarvoor instrumentatie-engine. Extra platform-specifieke stappen, zijn zoals hieronder wordt beschreven, vereist.

| Platform | Stappen die nodig zijn om op te halen van volledige SQL-Query |
| --- | --- |
| Azure Web App |In het Configuratiescherm van web-app [opent u de Application Insights-blade](../../azure-monitor/app/azure-web-apps.md) en SQL-opdrachten onder .NET in te schakelen |
| IIS-Server (virtuele machines van Azure, on-premises, enzovoort.) | [Installeer Status Monitor op uw server waarop de toepassing wordt uitgevoerd](../../azure-monitor/app/monitor-performance-live-website-now.md) en start IIS opnieuw.
| Azure-Cloudservice |[Gebruik opstarttaak](../../azure-monitor/app/cloudservices.md) naar [Status Monitor installeren](monitor-performance-live-website-now.md#download) |
| IIS Express | Niet ondersteund

In de bovenstaande gevallen de juiste manier te valideren dat instrumentatie-engine is correct geïnstalleerd is door te valideren dat de SDK-versie van de verzamelde `DependencyTelemetry` 'rddp' is. 'rdddsd' of 'rddf' geeft aan afhankelijkheden worden verzameld via DiagnosticSource of EventSource callbacks en kan daarom volledige SQL-query wordt niet worden vastgelegd.

## <a name="where-to-find-dependency-data"></a>Waar vind ik afhankelijkheidsgegevens

* [Overzicht van de toepassing](app-map.md) worden gevisualiseerd met afhankelijkheden tussen uw app en door in aangrenzende onderdelen.
* [Diagnostische gegevens voor transacties](transaction-diagnostics.md) toont unified, gecorreleerde server-gegevens.
* [Blade browsers](javascript.md#ajax-performance) AJAX-aanroepen van de browsers van uw gebruikers bevat.
* Klik op traag of mislukte aanvragen om te controleren op hun afhankelijkheidsaanroepen.
* [Analytics](#analytics) kan worden gebruikt om gegevens van de afhankelijkheid opvragen.

## <a name="diagnosis"></a> Diagnose van trage aanvragen

Elke aanvraaggebeurtenis is gekoppeld aan het aanroepen van afhankelijkheden, uitzonderingen en andere gebeurtenissen die worden bijgehouden, terwijl uw app de aanvraag wordt verwerkt. Dus als een ongeldige bepaalde aanvragen doen, kunt u vinden of dit is vanwege de trage reacties van een afhankelijkheid.

We nemen een voorbeeld van die.

### <a name="tracing-from-requests-to-dependencies"></a>Tracering van aanvragen naar afhankelijkheden

Open de blade Performance en kijken naar het raster van aanvragen:

![Lijst met aanvragen met gemiddelden en tellingen](./media/asp-net-dependencies/02-reqs.png)

De bovenste een duurt lang. Laten we zien als we vindt waar de tijd is besteed.

Klik op die rij om afzonderlijke aanvraaggebeurtenissen te bekijken:

![Lijst met instanties van de aanvraag](./media/asp-net-dependencies/03-instances.png)

Klik op een willekeurig exemplaar langlopende verder inspecteren en bladert u omlaag naar de externe afhankelijkheidsaanroepen die betrekking hebben op deze aanvraag:

![Aanroepen naar externe afhankelijkheden zoeken, identificeren ongebruikelijke duur](./media/asp-net-dependencies/04-dependencies.png)

Deze ziet eruit als de meeste van de tijd van onderhoud van die deze aanvraag is besteed in een aanroep naar een lokale service.

Selecteer de rij voor meer informatie:

![Klik op die externe afhankelijkheid voor het identificeren van het overmatig](./media/asp-net-dependencies/05-detail.png)

Het lijkt deze afhankelijkheid is waar het probleem zich bevindt. We hebben het probleem kunt, dus nu we gewoon wilt weten waarom die aanroep zo lang duurt.

### <a name="request-timeline"></a>Tijdlijn van de aanvraag

Er is geen afhankelijkheidsaanroep die erg lang in andere gevallen. Maar door over te schakelen naar de tijdlijnweergave, kunnen we zien waar de vertraging is opgetreden in onze interne verwerking:

![Aanroepen naar externe afhankelijkheden zoeken, identificeren ongebruikelijke duur](./media/asp-net-dependencies/04-1.png)

Er lijkt te zijn van een wijd na het aanroepen van de eerste afhankelijkheid, zodat we onze code om te zien waarom moeten kijken.

### <a name="profile-your-live-site"></a>Profileren van uw live site

Er is geen idee waar de tijd komt? De [Application Insights profiler](../../azure-monitor/app/profiler.md) traceringen HTTP-aanroepen naar de live site en ziet u de functies in uw code die de meeste tijd heeft.

## <a name="failed-requests"></a>Mislukte aanvragen

Mislukte aanvragen kunnen ook zijn gekoppeld aan de mislukte aanroepen van afhankelijkheden. Nogmaals, kunnen we doorklikken voor het opsporen van het probleem.

![Klik op de grafiek van mislukte aanvragen](./media/asp-net-dependencies/06-fail.png)

Klik door naar een exemplaar van een mislukte aanvragen en de bijbehorende gebeurtenissen bekijken.

![Klik op een aanvraag met het type en het exemplaar naar een andere weergave van hetzelfde exemplaar, klikt u erop om details van uitzondering.](./media/asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analyse

U kunt volgen afhankelijkheden in de [Kusto-querytaal](/azure/kusto/query/). Hier volgen enkele voorbeelden.

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

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*Hoe kan automatische Afhankelijkheidsrapport collector kiest, wordt er niet aanroepen van afhankelijkheden?*

* Mislukte afhankelijkheidsaanroepen moet 'geslaagd' veld ingesteld op False. `DependencyTrackingTelemetryModule` Er wordt niet gerapporteerd `ExceptionTelemetry`. Het volledige gegevensmodel voor afhankelijkheid is beschreven [hier](data-model-dependency-telemetry.md).

## <a name="open-source-sdk"></a>Open-source-SDK
Net zoals elke Application Insights-SDK is afhankelijkheid verzameling module ook open-source. Lezen en bijdragen aan de code of problemen melden [de officiële GitHub-opslagplaats](https://github.com/Microsoft/ApplicationInsights-dotnet-server).


## <a name="next-steps"></a>Volgende stappen

* [Uitzonderingen](../../azure-monitor/app/asp-net-exceptions.md)
* [Gebruiker- en paginagegevens toevoegen](../../azure-monitor/app/javascript.md)
* [Beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)
