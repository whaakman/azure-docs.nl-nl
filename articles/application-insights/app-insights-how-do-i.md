---
title: Hoe kan ik... in Azure Application Insights | Microsoft Docs
description: Veelgestelde vragen over het in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: mbullwin
ms.openlocfilehash: 8cee346a45cd20e7dd677fd7f2efed5500175598
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096391"
---
# <a name="how-do-i--in-application-insights"></a>Hoe kan ik ... in Application Insights?
## <a name="get-an-email-when-"></a>Ontvang een e-mail wanneer...
### <a name="email-if-my-site-goes-down"></a>E-mailbericht als Mijn site uitvalt
Stel een [beschikbaarheid van WebTest](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>E-als Mijn site is overbelast
Stel een [waarschuwing](app-insights-alerts.md) op **serverreactietijd**. Een drempelwaarde tussen 1 en 2 seconden moet werken.

![](./media/app-insights-how-do-i/030-server.png)

Uw app kan tekenen van stam ook weergeven door te retourneren van foutcodes. Een melding instellen op **mislukte aanvragen**.

Als u een melding instellen wilt op **serveruitzonderingen**, u moet doen [enkele aanvullende instellingen](app-insights-asp-net-exceptions.md) om te kunnen gegevens bekijken.

### <a name="email-on-exceptions"></a>E-uitzonderingen op
1. [Uitzonderingencontrole instellen](app-insights-asp-net-exceptions.md)
2. [Een melding instellen](app-insights-alerts.md) aantal metrische gegevens van de uitzondering

### <a name="email-on-an-event-in-my-app"></a>E-mail verzenden bij een gebeurtenis in mijn app
Stel dat u wilt een e-mail ontvangen wanneer een specifieke gebeurtenis zich voordoet. Application Insights biedt geen deze faciliteit rechtstreeks, maar kan het [verzenden van een waarschuwing wanneer een metriek een drempelwaarde](app-insights-alerts.md).

Meldingen kunnen worden ingesteld op [aangepaste metrische gegevens](app-insights-api-custom-events-metrics.md#trackmetric), maar geen aangepaste gebeurtenissen. De code schrijven om een metrische waarde vergroten wanneer de gebeurtenis zich voordoet:

    telemetry.TrackMetric("Alarm", 10);

Of:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Aangezien de waarschuwingen hebben twee statussen, hebt u een lage waarde verzenden wanneer u rekening houden met de waarschuwing is beëindigd:

    telemetry.TrackMetric("Alarm", 0.5);

Maak een grafiek in [metric explorer](app-insights-metrics-explorer.md) om te zien van uw waarschuwingen:

![](./media/app-insights-how-do-i/010-alarm.png)

Stel nu een waarschuwing geactiveerd wanneer er een mid waarde de metrische gegevens de honderd gedurende een korte periode:

![](./media/app-insights-how-do-i/020-threshold.png)

De gemiddelde periode tot het minimum instellen.

U ontvangt e-mailberichten wanneer de metriek honderd en lager dan de drempelwaarde.

Enkele punten om in overweging te nemen:

* Een waarschuwing heeft twee statussen ('waarschuwing', 'gezond'). De status wordt geëvalueerd wanneer een metrische waarde wordt ontvangen.
* Een e-mailbericht wordt verzonden, alleen wanneer de status verandert. Dit is waarom u hebt voor het verzenden van beide hoge en lage waarde metrische gegevens.
* Om te evalueren de waarschuwing, wordt het gemiddelde genomen van de ontvangen waarden in de voorafgaande periode. Dit gebeurt telkens wanneer een metrische waarde wordt ontvangen, zodat e-mailberichten vaker dan de ingestelde periode kunnen worden verzonden.
* Omdat e-mailberichten worden verzonden, zowel op 'waarschuwing', 'gezond', is het raadzaam om te overwegen overweegt u opnieuw het eindresultaat gebeurtenis als een voorwaarde voor twee statussen. Bijvoorbeeld, in plaats van een gebeurtenis 'taak voltooid' hebben een voorwaarde 'taak wordt uitgevoerd' waarop u e-mailberichten aan het begin en einde van een taak ophalen.

### <a name="set-up-alerts-automatically"></a>Waarschuwingen automatisch instellen
[PowerShell gebruiken voor het maken van nieuwe waarschuwingen](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>PowerShell gebruiken voor het beheren van Application Insights
* [Nieuwe resources maken](app-insights-powershell-script-create-resource.md)
* [Nieuwe waarschuwingen maken](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Afzonderlijke telemetrie van verschillende versies

* Meerdere rollen in een app: één Application Insights-resource gebruiken en filtert u op cloud_Rolename. [Meer informatie](app-insights-monitor-multi-role-apps.md)
* Het scheiden van ontwikkelings-, test- en release-versies: verschillende Application Insights-resources gebruiken. De instrumentatiesleutels uit web.config pikken. [Meer informatie](app-insights-separate-resources.md)
* Rapportage buildversies: voegt u de eigenschap met een telemetrische initializer. [Meer informatie](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Back-endservers en bureaublad-apps bewaken
[Gebruik de Windows Server SDK-module](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Gegevens visualiseren
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Dashboard met metrische gegevens van meerdere apps
* In [Metric Explorer](app-insights-metrics-explorer.md), uw grafiek aanpassen en sla deze op als een favoriet. Deze vastmaken aan het Azure-dashboard.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Dashboard met gegevens uit andere bronnen en Application Insights
* [Telemetrie exporteren naar Power BI](app-insights-export-power-bi.md).

of

* SharePoint gebruiken als uw dashboard, het weergeven van gegevens in de SharePoint-webonderdelen. [Continue export en Stream Analytics gebruiken om te exporteren naar SQL](app-insights-code-sample-export-sql-stream-analytics.md).  PowerView gebruiken om te controleren van de database en een SharePoint-webonderdeel voor PowerView maken.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonieme of geverifieerde gebruikers filteren
Als uw gebruikers zich aanmelden, kunt u instellen de [geverifieerde gebruikers-id](app-insights-api-custom-events-metrics.md#authenticated-users). (Het gebeurt niet automatisch.)

U kunt vervolgens het volgende doen:

* Zoeken naar specifieke gebruikers-id 's

![](./media/app-insights-how-do-i/110-search.png)

* Metrische gegevens voor de anonieme of geverifieerde gebruikers filteren

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Wijzig waarden of namen van eigenschappen
Maak een [filter](app-insights-api-filtering-sampling.md#filtering). Hiermee kunt u wijzigen of telemetrie filteren voordat deze wordt verzonden vanuit uw app naar Application Insights.

## <a name="list-specific-users-and-their-usage"></a>Lijst met specifieke gebruikers en hun gebruik
Als u alleen wilt [zoeken naar specifieke gebruikers](#search-specific-users), kunt u instellen de [geverifieerde gebruikers-id](app-insights-api-custom-events-metrics.md#authenticated-users).

Als u een lijst van gebruikers met gegevens, zoals welke pagina's wilt ze bekijken of hoe vaak deze zich aanmeldt, hebt u twee opties:

* [Geverifieerde gebruikers-id van verzameling](app-insights-api-custom-events-metrics.md#authenticated-users), [exporteren naar een database](app-insights-code-sample-export-sql-stream-analytics.md) en geschikt hulpprogramma's gebruiken voor het analyseren van uw gebruikersgegevens bevat.
* Als u slechts een klein aantal gebruikers hebt, aangepaste gebeurtenissen of metrische gegevens, met behulp van de gegevens van belang als de naam van de metrische waarde of een gebeurtenis en het instellen van de gebruikers-id als een eigenschap verzenden. Voor het analyseren van paginaweergaven, vervangt u de standaard JavaScript trackPageView-aanroep. Gebruiken voor het analyseren van telemetrie aan de serverzijde van de server, een telemetrische initializer toe te voegen de gebruikers-id aan alle servertelemetrie. Vervolgens kunt u metrische gegevens filteren en te segmenteren en zoekopdrachten op de gebruikers-id.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Verkeer beperken op mijn app naar Application Insights
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), alle modules die u niet nodig hebt, zoals de prestaties teller collector uitschakelen.
* Gebruik [Sampling en filteren](app-insights-api-filtering-sampling.md) bij de SDK.
* Beperk het aantal Ajax-aanroepen die zijn gerapporteerd voor de weergave van elke pagina in uw webpagina's. In het codefragment script na `instrumentationKey:...` , invoegen: `,maxAjaxCallsPerView:3` (of een geschikt aantal).
* Als u [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), de statistische functie van batches van metrische waarden berekent voordat het resultaat wordt verzonden. Er is een overbelasting van TrackMetric() dat voor die.

Meer informatie over [prijzen en quota voor](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Telemetrie uitschakelen
Naar **dynamisch stoppen en starten** het verzamelen en verzenden van telemetrie van de server:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



Naar **uitschakelen geselecteerde standard collectors** : bijvoorbeeld, prestatiemeteritems, HTTP-aanvragen of afhankelijkheden - verwijderen of opmerkingen bij de desbetreffende regels in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). U kunt dit doet, bijvoorbeeld, als u wilt de gegevens van uw eigen TrackRequest verzenden.

## <a name="view-system-performance-counters"></a>Systeemprestatiemeteritems weergeven
Tussen de metrische gegevens die u in metrics explorer weergeven kunt zijn een set van systeem prestatiemeteritems. Er is een vooraf gedefinieerde blade met de titel **Servers** die meerdere wordt weergegeven.

![Open uw Application Insights-resource en klik op de Servers](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Als er geen gegevens van prestatiemeteritems
* **IIS-server** op uw computer of op een virtuele machine. [Installeer statusmonitor](app-insights-monitor-performance-live-website-now.md).
* **Azure-website** -prestatiemeteritems nog niet ondersteund. Er zijn verschillende metrische gegevens u als een standaard deel uitmaken van het Azure-website van het Configuratiescherm krijgt.
* **UNIX-server** - [Installeer collectd](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Om meer prestatiemeteritems weer te geven
* Eerste, [toevoegen van een nieuwe grafiek](app-insights-metrics-explorer.md) en kijk of het item in de basic die we bieden.
* Als dit niet het geval is, [het item toevoegen aan de set die worden verzameld door de prestatiemeteritemmodule](app-insights-performance-counters.md).
