---
title: Metrische gegevens livestream met aangepaste metrische gegevens en diagnostische gegevens in Azure Application Insights | Microsoft Docs
description: Bewaken van uw web-app in realtime met aangepaste metrische gegevens en onderzoeken van problemen met een live feed fouten, traceringen en gebeurtenissen.
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mbullwin
ms.openlocfilehash: 866fc729b3167863c2d423d0e6ac0d7640e3425e
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Livestream metrische gegevens: De Monitor & spoor met een latentie van 1 seconde 

Test de kern kloppen in de race van uw live, in productie-webtoepassing met behulp van livestream metrische gegevens van [Application Insights](app-insights-overview.md). Selecteer en filteren van de prestatiemeteritems voor metrieken als bekijkt in real-time, zonder eventuele storingen van uw service. Inspecteer de stack-traces van voorbeeld kan aanvragen en uitzonderingen. Samen met [Profiler](app-insights-profiler.md), [momentopname foutopsporingsprogramma](app-insights-snapshot-debugger.md), en [prestatietests](app-insights-monitor-web-app-availability.md#performance-tests), metrische gegevens livestream biedt een krachtige en niet-Invasief diagnostisch hulpprogramma van uw live web site.

U kunt met livestream metrische gegevens:

* Valideren van een oplossing, terwijl deze wordt uitgebracht, prestaties en mislukte aantallen volgen.
* Bekijk het effect van belasting te testen en problemen diagnosticeren live. 
* Zich richten op bepaalde test sessies of bekende problemen uitfilteren door te selecteren en filteren van de metrische gegevens die u wilt bekijken.
* Uitzondering traceringen meteen worden opgehaald.
* Experimenteer met filters om te zoeken naar de meest relevante KPI's.
* Alle vensters prestaties meteritem live bewaken.
* Een server die problemen en filter die alle de KPI/live feed met alleen die server gemakkelijk herkennen.

[![Live video van de gegevensstroom van de metrische](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Metrische gegevens livestream is momenteel beschikbaar op de lokale met ASP.NET-apps of in de Cloud. 

## <a name="get-started"></a>Aan de slag

1. Als u nog niet hebt [Application Insights geïnstalleerd](app-insights-asp-net.md) in uw ASP.NET-web-app of [app voor Windows server](app-insights-windows-services.md), doet u dat nu. 
2. **Update naar de nieuwste versie** van het pakket voor Application Insights. In Visual Studio met de rechtermuisknop op uw project en kies **beheren Nuget-pakketten**. Open de **Updates** tabblad selectievakje **Include prerelease**, en selecteer alle Microsoft.ApplicationInsights.*-pakketten.

    Implementeer uw app opnieuw.

3. In de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw app en open vervolgens Live Stream.

4. [Beveiligen van het besturingskanaal](#secure-the-control-channel) als u mogelijk gevoelige gegevens zoals klantnamen in filters.


![Klik op de blade overzicht Live Stream](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Zijn er geen gegevens? Controleer de firewall van uw server

Controleer de [uitgaande poorten voor de metrische gegevens livestream](app-insights-ip-addresses.md#outgoing-ports) zijn geopend in de firewall van uw servers. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Hoe verschilt livestream metrische gegevens van Metrics Explorer en Analytics?

| |Live Stream | Metrics Explorer en analyses |
|---|---|---|
|Latentie|Gegevens die worden weergegeven binnen één seconde|Geaggregeerd in minuten|
|Er geen bewaarperiode|Gegevens zich blijft voordoen terwijl deze op de grafiek en vervolgens wordt verwijderd|[Gegevens bewaard gedurende 90 dagen](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|Op aanvraag|Gegevens gestreamd tijdens het openen van Live metrische gegevens|Gegevens worden verzonden wanneer de SDK is geïnstalleerd en ingeschakeld|
|Gratis|Er zijn geen kosten voor Live Stream gegevens|Behoudens [prijzen](app-insights-pricing.md)
|Steekproeven|Alle geselecteerde metrische gegevens en prestatiemeteritems worden verzonden. Fouten en stack-traces steekproeven worden genomen. TelemetryProcessors worden niet toegepast.|Mogelijk zijn gebeurtenissen [door actieve](app-insights-api-filtering-sampling.md)|
|Besturingskanaal|Filter besturingselement signalen worden verzonden naar de SDK. We raden u [secure dit kanaal](#secure-channel).|Communicatie kan niet worden teruggedraaid naar de portal|


## <a name="select-and-filter-your-metrics"></a>Selecteer en metrische gegevens over uw filteren

(Beschikbaar op klassieke ASP.NET-apps met de nieuwste SDK).

U kunt aangepaste KPI live bewaken door willekeurige filters zijn toegepast op alle telemetrie voor Application Insights vanuit de portal. Klik op het filterbesturingselement waarin wordt weergegeven wanneer u muis een van de grafieken. Het volgende diagram wordt een aangepaste aanvraag aantal KPI met filters op de URL en de duur van de kenmerken tekengebied. Filters met de stroom Preview sectie waarin een live feed telemetrie die overeenkomt met de criteria die u hebt opgegeven op elk punt in tijd valideren. 

![Aangepaste aanvraag KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

U kunt een andere waarde dan het aantal bewaken. De opties is afhankelijk van het type stream, wat erop kan telemetrie voor Application Insights: aanvragen, afhankelijkheden, uitzonderingen, traceringen, gebeurtenissen of metrische gegevens. Het kan ook uw eigen [aangepaste meting](app-insights-api-custom-events-metrics.md#properties):

![Opties voor](./media/app-insights-live-stream/live-stream-valueoptions.png)

Naast telemetrie voor Application Insights, kunt u ook een Windows-prestatiemeteritem bewaken door te selecteren die uit de stroom-opties, en de naam van het prestatiemeteritem.

Live metrische gegevens worden samengevoegd op twee punten: lokaal op elke server en klik vervolgens op alle servers. U kunt de standaardwaarde op door een andere opties selecteren in de desbetreffende vervolgkeuzelijsten wijzigen.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Voorbeeld telemetrie: Aangepaste Live diagnostische gebeurtenissen
Standaard ziet de feed live gebeurtenissen u voorbeelden van mislukte aanvragen en afhankelijkheidsaanroepen, uitzonderingen, gebeurtenissen en traceringen. Klik op het filterpictogram om te zien van de toegepaste criteria op elk punt in tijd. 

![Standaard live feed](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Als met metrische gegevens, kunt u een willekeurige criteria aan een van de Application Insights telemetrie-typen. In dit voorbeeld selecteren we, traceringen en gebeurtenissen voor mislukte aanvragen voor specifieke. Er worden ook alle uitzonderingen en fouten van afhankelijkheid selecteren.

![Aangepaste live feed](./media/app-insights-live-stream/live-stream-events.png)

Opmerking: Op dit moment voor uitzondering criteria op basis van berichten, gebruiken de buitenste uitzonderingsbericht. In het voorgaande voorbeeld, voor het filteren van de onschadelijk uitzondering met het InnerException-bericht (volgt de "<--' scheidingsteken)" de client de verbinding verbroken." Gebruik een bericht bevat niet-'Fout bij het lezen van de aanvraaginhoud' criteria.

Zie de details van een item in de live feed door erop te klikken. U kunt de feed onderbreken door te klikken op **onderbreken** gewoon omlaag schuiven, of op een item te klikken. Live feed wordt hervat nadat u Schuif terug naar het begin of door te klikken op de teller van items die zijn verzameld tijdens het programma is onderbroken.

![Steekproef live fouten](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Filteren op server-exemplaar

Als u bewaken van een bepaalde server rolinstantie wilt, kunt u filteren op de server.

![Steekproef live fouten](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK-vereisten
Aangepaste metrische gegevens livestream is beschikbaar met versie 2.4.0-beta2 of nieuwere van [Application Insights-SDK voor web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Vergeet niet 'Bevatten Prerelease' optie van NuGet-Pakketbeheer selecteren.

## <a name="secure-the-control-channel"></a>Beveiligen van het besturingskanaal
De aangepaste filters criteria die u opgeeft worden verzonden naar het onderdeel Live metrische gegevens in de Application Insights-SDK. De filters kunnen mogelijk vertrouwelijke gegevens zoals customerIDs bevatten. U kunt het kanaal beveiligde maken met een geheime sleutel van de API naast de instrumentatiesleutel.
### <a name="create-an-api-key"></a>Maak een API-sleutel

![Api-sleutel maken](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>API-sleutel toevoegen aan configuratie
In het bestand applicationinsights.config voegt u de AuthenticationApiKey toe aan de QuickPulseTelemetryModule:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
Of in code wordt ingesteld op de QuickPulseTelemetryModule:

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

Als u kent en de gekoppelde servers vertrouwt, kunt u de aangepaste filters zonder het geverifieerde kanaal proberen. Deze optie is beschikbaar voor zes maanden. Met deze overschrijving is vereist eenmaal elke nieuwe sessie of als een nieuwe server online is.

![Live metrische gegevens Auth-opties](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Het is raadzaam dat u de geverifieerde kanaal instellen voordat u een potentieel gevoelige informatie, zoals CustomerID in de filtercriteria invoert.
>

## <a name="generating-a-performance-test-load"></a>Een test prestatiebelasting genereren

Als u wilt bekijken van het effect van een verhoging van het laden, gebruikt u de blade Test van de prestaties. Het simuleert aanvragen van een aantal gelijktijdige gebruikers. Beide 'handmatig 'tests uit te voeren (ping-tests) van één URL of uit te voeren een [WebTest met meerdere stappen prestatietest](app-insights-monitor-web-app-availability.md#multi-step-web-tests) die u uploadt (op dezelfde manier als een beschikbaarheidstest).

> [!TIP]
> Nadat u de prestatietest maakt, opent u de test en de blade Live Stream in afzonderlijke vensters. U ziet wanneer de wachtrij prestatietest wordt gestart, en Bekijk live stream op hetzelfde moment.
>


## <a name="troubleshooting"></a>Problemen oplossen

Zijn er geen gegevens? Als uw toepassing bevindt zich in een beveiligd netwerk: metrische gegevens Stream Live maakt gebruik van een ander IP-adressen dan andere telemetrie voor Application Insights. Zorg ervoor dat [die IP-adressen](app-insights-ip-addresses.md) zijn geopend in uw firewall.



## <a name="next-steps"></a>Volgende stappen
* [Gebruik met Application Insights controleren](app-insights-web-track-usage.md)
* [Met behulp van diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md)
* [Profiler](app-insights-profiler.md)
* [Momentopname-foutopsporingsprogramma](app-insights-snapshot-debugger.md)
