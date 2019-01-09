---
title: Live Metrics Stream met aangepaste metrische gegevens en diagnostische gegevens in Azure Application Insights | Microsoft Docs
description: Uw web-app in realtime met aangepaste metrische gegevens bewaken en problemen met een live-uitzending van fouten, traceringen en gebeurtenissen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/04/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 403906a60d16a478dffd313b45aa1ce24e42196a
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119210"
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Live Metrics Stream: Bewaking en Diagnose met een latentie van 1 seconde

De kern van de heartbeat van uw webtoepassing live en in productie--test met behulp van Live Metrics Stream van [Application Insights](../../azure-monitor/app/app-insights-overview.md). Selecteren en filteren van tellers van metrische gegevens en prestaties om te kijken in realtime, zonder eventuele storingen van uw service. Inspecteer de stack-traces uit het kan de niet-voorbeeldaanvragen en uitzonderingen. Samen met [Profiler](../../azure-monitor/app/profiler.md), [Snapshot debugger](../../azure-monitor/app/snapshot-debugger.md), en [prestatietests](../../azure-monitor/app/monitor-web-app-availability.md#performance-tests), Live Metrics Stream biedt een krachtige en niet-Invasief diagnostisch hulpprogramma voor uw live web site.

Met Live Metrics Stream, kunt u het volgende doen:

* Valideren van een oplossing terwijl dit wordt uitgebracht, door het aantal prestaties en de fout te bekijken.
* Bekijk het effect van de belasting te testen en problemen diagnosticeren live. 
* Richt u op bepaalde test sessies of filteren van bekende problemen, door te selecteren en filteren van de metrische gegevens die u wilt bekijken.
* Get-uitzondering traceringen wanneer deze zich voordoen.
* Experimenteer met filters te vinden van de meest relevante KPI's.
* Alle Windows prestaties teller live bewaken.
* Eenvoudig Zoek een server die u ondervindt problemen en filter die alle de KPI/live-feed naar alleen die server.

[![Live Metrics Stream-video](./media/live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

## <a name="get-started"></a>Aan de slag

1. Als u nog niet hebt [Application Insights geïnstalleerd](../../azure-monitor/app/asp-net.md) in uw ASP.NET-web-app of [app voor Windows server](../../azure-monitor/app/windows-services.md), doet u dat nu. 
2. **Update naar de nieuwste versie** van het pakket Application Insights. In Visual Studio met de rechtermuisknop op uw project en kies **Nuget-pakketten beheren**. Open de **Updates** tabblad controle **Include prerelease**, en selecteer alle Microsoft.ApplicationInsights.*-pakketten.

    Implementeer uw app opnieuw.

3. In de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw app en open Live Stream.

4. [Beveiligen van het besturingskanaal](#secure-the-control-channel) als u gevoelige gegevens, zoals namen van klanten in de filters gebruiken.


![Klik in de blade overzicht op Live Stream](./media/live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Zijn er geen gegevens? Controleer de firewall van uw server

Controleer de [uitgaande poorten voor Live Metrics Stream](../../azure-monitor/app/ip-addresses.md#outgoing-ports) zijn geopend in de firewall van uw servers. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Hoe verschilt Live Metrics Stream van Metrics Explorer en Analytics?

| |Live Stream | Metrics Explorer en Analytics |
|---|---|---|
|Latentie|Gegevens die worden weergegeven binnen één seconde|Samengevoegd in minuten|
|Er is geen bewaarperiode|Gegevens zich blijft voordoen terwijl deze in de grafiek en vervolgens wordt verwijderd|[Gegevens gedurende 90 dagen bewaard](../../azure-monitor/app/data-retention-privacy.md#how-long-is-the-data-kept)|
|Op aanvraag|Gegevens worden gestreamd terwijl u Live metrische gegevens openen|Gegevens worden verzonden wanneer de SDK is geïnstalleerd en ingeschakeld|
|Gratis|Er zijn geen kosten voor Live Stream-gegevens|Onderhevig aan [prijzen](../../azure-monitor/app/pricing.md)
|Steekproeven|Alle geselecteerde metrische gegevens en prestatiemeteritems worden verzonden. Fouten en stack-traces steekproeven worden genomen. TelemetryProcessors worden niet toegepast.|Mogelijk zijn gebeurtenissen [steekproef](../../azure-monitor/app/api-filtering-sampling.md)|
|Besturingskanaal|Filter beheer locatiesignalen worden verzonden naar de SDK. We raden u [beveiligd dit kanaal](#secure-channel).|Communicatie kan niet worden teruggedraaid naar de portal|


## <a name="select-and-filter-your-metrics"></a>Selecteren en filteren van uw metrische gegevens

(Beschikbaar op klassieke ASP.NET-apps met de nieuwste SDK).

U kunt aangepaste KPI live bewaken met willekeurige filters zijn toegepast op geen Application Insights-telemetriegegevens vanuit de portal. Klik op het filterbesturingselement dat laat zien wanneer u muis een van de grafieken. Het volgende diagram wordt een aangepaste aanvraag aantal KPI met filters op basis-URL en de duur van de kenmerken plotten. Valideer uw filters met de Preview van Stream-sectie die wordt een dynamische feed van telemetrie die voldoet aan de criteria die u hebt opgegeven op elk gewenst moment in-time. 

![Aangepaste KPI-aanvraag](./media/live-stream/live-stream-filteredMetric.png)

U kunt een waarde van aantal verschillende bewaken. De opties, is afhankelijk van het type van de stroom, wat erop kan geen Application Insights-telemetriegegevens: aanvragen, afhankelijkheden, uitzonderingen, traceringen, gebeurtenissen en metrische gegevens. Kan het zijn uw eigen [aangepaste meting](../../azure-monitor/app/api-custom-events-metrics.md#properties):

![Waardeopties voor](./media/live-stream/live-stream-valueoptions.png)

Naast de telemetrie van Application Insights, kunt u ook een Windows-prestatiemeteritem doordat te selecteren uit de stream-opties, en de naam van het prestatiemeteritem bewaken.

Live metrische gegevens op twee punten worden geaggregeerd: lokaal op elke server, en klik vervolgens op alle servers. U kunt de standaard beperkt door een andere opties selecteren in de respectieve vervolgkeuzelijsten wijzigen.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Voorbeeld van telemetrie: Aangepaste Live diagnostische gebeurtenissen
Voorbeelden van mislukte aanvragen en aanroepen van afhankelijkheden, uitzonderingen, gebeurtenissen en traceringen worden standaard de live-feed van de gebeurtenissen weergegeven. Klik op het filterpictogram om te zien van de toegepaste criteria op elk gewenst moment in-time. 

![Standaard live feed](./media/live-stream/live-stream-eventsdefault.png)

Als met metrische gegevens, kunt u elke willekeurige criteria aan een van de Application Insights-telemetrietypen opgeven. In dit voorbeeld selecteren we specifieke aanvraag fouten, traceringen en gebeurtenissen. We zijn ook alle uitzonderingen en afhankelijkheidsfouten selecteren.

![Aangepaste live feed](./media/live-stream/live-stream-events.png)

Opmerking: Gebruik op dit moment de buitenste uitzonderingsbericht voor uitzondering bericht op basis van criteria. In het voorgaande voorbeeld, voor het filteren van de goedaardige uitzondering met binnenste uitzonderingsbericht (volgt de ' <--"scheidingsteken)"klient byl odpojen." Gebruik een bericht bevat niet-'Fout bij het lezen van de inhoud van aanvraag' criteria.

Bekijk de details van een item in de live-feed door erop te klikken. U kunt de feed onderbreken door te klikken op **onderbreken** of gewoon bladeren, of op een item te klikken. Live feed wordt hervat nadat u schuiven terug naar de bovenkant of door te klikken op de teller van de items die zijn verzameld tijdens het programma is onderbroken.

![Sample live fouten](./media/live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Filteren op server-exemplaar

Als u wilt voor het bewaken van een bepaalde server rolinstantie, kunt u filteren op de server.

![Sample live fouten](./media/live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK-vereisten
Aangepaste Live Metrics Stream is beschikbaar in versie 2.4.0-beta2 of nieuwere van [Application Insights SDK voor web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Houd er rekening mee de optie 'Voorlopige versie opnemen' software selecteren uit de NuGet-Pakketbeheer.

## <a name="secure-the-control-channel"></a>Het besturingskanaal beveiligen
De aangepaste filters criteria die u opgeeft worden verzonden naar het onderdeel Live Metrics in Application Insights-SDK. De filters kunnen mogelijk vertrouwelijke gegevens zoals customerIDs bevatten. U kunt het kanaal beveiligde maken met een geheime sleutel van de API naast de instrumentatiesleutel.
### <a name="create-an-api-key"></a>Een API-sleutel maken

![Api-sleutel maken](./media/live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>API-sleutel toevoegen aan configuratie

### <a name="classic-aspnet"></a>Klassieke ASP.NET

In het bestand applicationinsights.config, voegt u de AuthenticationApiKey toe aan de QuickPulseTelemetryModule:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
Of in de code, stel deze in op de QuickPulseTelemetryModule:

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;

             TelemetryConfiguration configuration = new TelemetryConfiguration();
            configuration.InstrumentationKey = "YOUR-IKEY-HERE";

            QuickPulseTelemetryProcessor processor = null;

            configuration.TelemetryProcessorChainBuilder
                .Use((next) =>
                {
                    processor = new QuickPulseTelemetryProcessor(next);
                    return processor;
                })
                        .Build();

            var QuickPulse = new QuickPulseTelemetryModule()
            {

                AuthenticationApiKey = "YOUR-API-KEY"
            };
            QuickPulse.Initialize(configuration);
            QuickPulse.RegisterTelemetryProcessor(processor);
            foreach (var telemetryProcessor in configuration.TelemetryProcessors)
                {
                if (telemetryProcessor is ITelemetryModule telemetryModule)
                    {
                    telemetryModule.Initialize(configuration);
                    }
                }

```

### <a name="aspnet-core-requires-application-insights-aspnet-core-sdk-230-beta-or-greater"></a>ASP.NET Core (vereist Application Insights ASP.NET Core-SDK 2.3.0-beta of hoger)

Het bestand startup.cs als volgt wijzigen:

Eerst toevoegen

``` C#
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
```

Klik in de methode ConfigureServices toevoegen:

``` C#
services.ConfigureTelemetryModule<QuickPulseTelemetryModule> ((module, o) => module.AuthenticationApiKey = "YOUR-API-KEY-HERE");
```


Als u kent en de gekoppelde servers vertrouwt, kunt u de aangepaste filters zonder het geverifieerde kanaal proberen. Deze optie is beschikbaar voor zes maanden. Met deze onderdrukking is vereist wanneer elke nieuwe sessie, of als een nieuwe server online is.

![Live metrische gegevens Auth-opties](./media/live-stream/live-stream-auth.png)

>[!NOTE]
>Het is raadzaam dat u de geverifieerde channel instellen voordat u een potentieel gevoelige informatie, zoals CustomerID in de filtercriteria invoert.
>

## <a name="generating-a-performance-test-load"></a>Een test prestatiebelasting genereren

Als u wilt om te bekijken van het effect van een verhoging van het laden, gebruikt u de blade van de prestaties testen. Het aanvragen van een aantal gebruikers dat gelijktijdig wordt gesimuleerd. Een van beide 'handmatig 'tests uit te voeren (ping-tests) van één URL of het kan worden uitgevoerd een [prestatietest WebTest met meerdere stappen](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) die u uploadt (op dezelfde manier als een beschikbaarheidstest).

> [!TIP]
> Nadat u de prestatietest maakt, opent u de test en de blade Live Stream in afzonderlijke vensters. U ziet wanneer de test in de wachtrij performance wordt gestart, en Bekijk live stream op hetzelfde moment.
>


## <a name="troubleshooting"></a>Problemen oplossen

Zijn er geen gegevens? Als uw toepassing zich in een beveiligd netwerk: Live Metrics Stream maakt gebruik van een ander IP-adressen dan andere telemetrie Application Insights. Zorg ervoor dat [deze IP-adressen](../../azure-monitor/app/ip-addresses.md) in uw firewall zijn geopend.



## <a name="next-steps"></a>Volgende stappen
* [Bewaking met Application Insights wordt gebruikt](../../azure-monitor/app/usage-overview.md)
* [Met behulp van diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md)
* [Profiler](../../azure-monitor/app/profiler.md)
* [Snapshot debugger](../../azure-monitor/app/snapshot-debugger.md)
