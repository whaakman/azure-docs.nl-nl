---
title: Hoe kan ik... in Azure Application Insights | Microsoft Docs
description: Veelgestelde vragen in Application Insights.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: ef63e06c0621753e0a706d6efb709b943e38ee42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-do-i--in-application-insights"></a>Hoe kan ik ... in Application Insights?
## <a name="get-an-email-when-"></a>Ontvang een e-mail wanneer...
### <a name="email-if-my-site-goes-down"></a>E-mailadres als Mijn site uitvalt
Stel een [beschikbaarheid WebTest](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>E-als Mijn site is overbelast
Stel een [waarschuwing](app-insights-alerts.md) op **serverreactietijd**. Een drempelwaarde tussen 1 en 2 seconden zouden moeten werken.

![](./media/app-insights-how-do-i/030-server.png)

Uw app mogelijk tekenen van stam ook weergeven door te retourneren van de fout-codes. Een waarschuwing instellen voor **mislukte aanvragen**.

Als u een waarschuwing instellen wilt voor **serveruitzonderingen**, u moet doen [enkele aanvullende instellingen](app-insights-asp-net-exceptions.md) om te zien van gegevens.

### <a name="email-on-exceptions"></a>E-uitzonderingen op
1. [Uitzonderingenmonitor instellen](app-insights-asp-net-exceptions.md)
2. [Een waarschuwing instellen](app-insights-alerts.md) tellen metrische gegevens voor de uitzondering.

### <a name="email-on-an-event-in-my-app"></a>E-mail bekijkt op een gebeurtenis in mijn app
Stel dat u wil een e-mailbericht ontvangen wanneer een bepaalde gebeurtenis plaatsvindt. Application Insights geen biedt deze mogelijkheid rechtstreeks, maar dit kan [een waarschuwing verzonden wanneer een metriek een drempelwaarde overschrijden](app-insights-alerts.md).

Waarschuwingen kunnen worden ingesteld voor [aangepaste metrische gegevens](app-insights-api-custom-events-metrics.md#trackmetric)echter geen aangepaste gebeurtenissen. Code schrijven om een metriek verhogen als de gebeurtenis:

    telemetry.TrackMetric("Alarm", 10);

Of:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Omdat waarschuwingen twee statussen hebben, hebt u een lage waarde verzenden wanneer u rekening houden met de waarschuwing is beëindigd:

    telemetry.TrackMetric("Alarm", 0.5);

Maken van een grafiek in [metrische explorer](app-insights-metrics-explorer.md) om te zien van uw alarm:

![](./media/app-insights-how-do-i/010-alarm.png)

Stel nu een waarschuwing activeert wanneer de metriek hoger dan de waarde van een mid gedurende een korte periode is:

![](./media/app-insights-how-do-i/020-threshold.png)

Het berekenen van gemiddelden periode instellen op het minimum.

U krijgt e-mailberichten wanneer de metriek boven gaat en onder de drempelwaarde.

Enkele punten in overweging moet nemen:

* Een waarschuwing heeft twee statussen ('waarschuwing' en 'goede'). De status alleen geëvalueerd wanneer een metriek is ontvangen.
* Een e-mailbericht wordt alleen verzonden wanneer de status verandert. Dit is waarom u verzenden hoge en lage waarde metrische gegevens.
* Om de waarschuwing, wordt het gemiddelde genomen van de waarden ontvangen gedurende de afgelopen periode. Dit gebeurt telkens wanneer een metriek is ontvangen, zodat e-mailberichten vaker dan de ingestelde periode kunnen worden verzonden.
* Omdat e-mailberichten worden verzonden op 'waarschuwing' en 'goede', is het raadzaam te overwegen opnieuw gegevensclassificatie uw eindresultaat gebeurtenis als een voorwaarde voor twee statussen. Bijvoorbeeld, in plaats van een gebeurtenis 'taak voltooid' hebben een voorwaarde 'taak wordt uitgevoerd' waarop u e-mailberichten aan het begin en einde van een taak ophalen.

### <a name="set-up-alerts-automatically"></a>Waarschuwingen automatisch instellen
[PowerShell gebruiken voor het maken van nieuwe waarschuwingen](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>PowerShell gebruiken om Application Insights beheren
* [Maken van nieuwe resources](app-insights-powershell-script-create-resource.md)
* [Nieuwe waarschuwingen maken](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Afzonderlijke telemetrie van verschillende versies

* Meerdere rollen in een app: gebruiken één Application Insights-resource en filtert u op cloud_Rolename. [Meer informatie](app-insights-monitor-multi-role-apps.md)
* Scheiden van ontwikkeling, testen en release-versies: verschillende Application Insights-bronnen worden gebruikt. De instrumentatiesleutels uit web.config worden opgepikt. [Meer informatie](app-insights-separate-resources.md)
* Reporting bouwen versies: een eigenschap met een initialisatiefunctie telemetrie toevoegen. [Meer informatie](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Back-endservers en bureaublad-apps bewaken
[Gebruik de module Windows Server SDK](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Gegevens visualiseren
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Dashboard met metrische gegevens vanuit meerdere apps
* In [metriek Explorer](app-insights-metrics-explorer.md), de grafiek aanpassen en als favoriet opslaan. Vastmaken aan de Azure-dashboard.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Dashboard met gegevens uit andere bronnen en de Application Insights
* [Exporteren van telemetrie naar Power BI](app-insights-export-power-bi.md).

of

* SharePoint gebruiken als uw dashboard, het weergeven van gegevens in de SharePoint-webonderdelen. [Continue export en Stream Analytics gebruiken om te exporteren naar SQL](app-insights-code-sample-export-sql-stream-analytics.md).  PowerView gebruiken om te controleren van de database en het maken van een SharePoint-webonderdeel voor PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonieme of geverifieerde gebruikers filteren
Als uw gebruikers zich aanmelden, kunt u instellen de [geverifieerde gebruikers-id](app-insights-api-custom-events-metrics.md#authenticated-users). (Dit gebeurt niet automatisch.)

U kunt vervolgens:

* Zoeken naar specifieke gebruikers-id 's

![](./media/app-insights-how-do-i/110-search.png)

* Metrische gegevens voor anonieme of geverifieerde gebruikers filteren

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Wijzig de namen van eigenschappen of waarden
Maak een [filter](app-insights-api-filtering-sampling.md#filtering). Hiermee kunt u wijzigen of filteren van telemetrie voordat deze van uw app naar Application Insights wordt verzonden.

## <a name="list-specific-users-and-their-usage"></a>Lijst van specifieke gebruikers en hun gebruik
Als u alleen wilt [zoeken naar specifieke gebruikers](#search-specific-users), kunt u instellen de [geverifieerde gebruikers-id](app-insights-api-custom-events-metrics.md#authenticated-users).

Als u wilt dat een lijst met gebruikers met gegevens zoals welke pagina's ze bekijken of hoe vaak deze zich aanmeldt, hebt u twee opties:

* [Set geverifieerde gebruiker-id](app-insights-api-custom-events-metrics.md#authenticated-users), [exporteren naar een database](app-insights-code-sample-export-sql-stream-analytics.md) en geschikt hulpprogramma's gebruiken om de gebruikergegevens er te analyseren.
* Als u slechts een klein aantal gebruikers hebt, aangepaste gebeurtenissen of maatstaven voor gebruik van de gegevens van belang als de naam van de metrische waarde of gebeurtenis en het instellen van de gebruikers-id als een eigenschap verzenden. Vervang de standaard JavaScript trackPageView-aanroep voor het analyseren van paginaweergaven. Gebruik een initialisatiefunctie telemetrie de gebruikers-id toevoegen aan alle servertelemetrie voor het analyseren van serverzijde telemetrie. Vervolgens kunt u filteren en segment metrische gegevens en zoekopdrachten op de gebruikers-id.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Verminderen het verkeer van mijn app naar Application Insights
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), alle modules die u niet nodig hebt, die de prestaties teller collector uitschakelen.
* Gebruik [steekproeven en filteren](app-insights-api-filtering-sampling.md) op de SDK.
* In uw webpagina's, het aantal voor elke paginaweergave gemelde Ajax-aanroepen te beperken. In het codefragment script na `instrumentationKey:...` , invoegen: `,maxAjaxCallsPerView:3` (of een geschikte getal).
* Als u [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), de statistische functie van batches van metrische waarden voor het verzenden van het resultaat berekenen. Er is een overload van TrackMetric() die voor die biedt.

Meer informatie over [-prijzen en -quota](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Telemetrie uitschakelen
Naar **dynamisch stoppen en starten** het verzamelen en verzenden van telemetrie van de server:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



Naar **uitschakelen geselecteerde standaard collectors** : bijvoorbeeld, prestatiemeteritems, HTTP-aanvragen of afhankelijkheden - verwijderen of de relevante regels in uitcommentariëren [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). U kan doen, bijvoorbeeld, als u wilt de gegevens van uw eigen TrackRequest verzenden.

## <a name="view-system-performance-counters"></a>Systeemprestatiemeteritems weergeven
Zijn een set van systeem prestatiemeteritems tussen de metrische gegevens die u in metrics explorer weergeven kunt. Er is een vooraf gedefinieerde blade met de titel **Servers** waarin meerdere wordt weergegeven.

![Open uw Application Insights-resource en klik op Servers](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Als er geen gegevens van prestatiemeteritems
* **IIS-server** op uw computer of op een virtuele machine. [Statusmonitor installeren](app-insights-monitor-performance-live-website-now.md).
* **Azure-website** -prestatiemeteritems nog niet ondersteund. Er zijn verschillende metrische gegevens die u als standaardonderdeel van het Azure-website van het Configuratiescherm krijgt.
* **UNIX-server** - [Installeer collectd](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Om meer prestatiemeteritems weer te geven
* Eerst [toevoegen van een nieuwe grafiek](app-insights-metrics-explorer.md) en controleert u of het item in de basis die wij bieden.
* Als dat niet het geval is, [de teller toevoegen aan de set die is verzameld door de prestatiemeteritemmodule](app-insights-performance-counters.md).
