---
title: Azure Application Insights Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: mbullwin
ms.openlocfilehash: 815388db673673a3802f8e5e515b7e16cb180a29
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323351"
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Veelgestelde vragen

## <a name="configuration-problems"></a>Configuratieproblemen
*Ik ondervind problemen met het instellen van mijn:*

* [.NET app](app-insights-asp-net-troubleshoot-no-data.md)
* [Bewaken van een app al actief](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure diagnostics](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Java-web-app](app-insights-java-troubleshoot.md)

*Kan ik ophalen geen gegevens uit mijn server*

* [Set firewall-uitzonderingen](app-insights-ip-addresses.md)
* [Een ASP.NET-server instellen](app-insights-monitor-performance-live-website-now.md)
* [Een Java-server instellen](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Kan ik met Application Insights... gebruiken?

* [Web-apps op een IIS-server, on-premises of in een virtuele machine](app-insights-asp-net.md)
* [Java-web-apps](app-insights-java-get-started.md)
* [Node.js-apps](app-insights-nodejs.md)
* [Web-apps op Azure](app-insights-azure-web-apps.md)
* [Cloudservices op Azure](app-insights-cloudservices.md)
* [Appservers die worden uitgevoerd in Docker](app-insights-docker.md)
* [Één pagina web-apps](app-insights-javascript.md)
* [SharePoint](app-insights-sharepoint.md)
* [Windows desktop-app](app-insights-windows-desktop.md)
* [Andere platforms](app-insights-platforms.md)

## <a name="is-it-free"></a>Is dit gratis?

Ja, voor experimentele gebruik. In de basic prijsstelling, kan uw toepassing een bepaalde toegestane hoeveelheid gegevens per maand gratis verzenden. Het gratis tegoed is groot genoeg is voor cover ontwikkelings- en publiceren van een app voor een klein aantal gebruikers. U kunt instellen dat een limiet om te voorkomen dat meer dan een opgegeven hoeveelheid gegevens die worden verwerkt.

Grotere hoeveelheden telemetriegegevens worden gefactureerd op de Gb. We bieden enkele tips over het [beperken van de kosten voor uw](app-insights-pricing.md).

Het Enterprise-plan leidt tot een kosten in rekening gebracht voor elke dag dat elk knooppunt van de server web telemetrie verzendt. Het is geschikt als u wilt gebruiken van continue Export op grote schaal.

[Lees de prijsstelling](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Wat is het stuk?

* Open de **pagina gebruik en geschatte kosten** pagina in een Application Insights-resource. Er is een diagram van recente gebruik. Als u wilt, kunt u een maximum gegevensvolume instellen.
* Open de [blade Azure Billing](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) om te zien van uw facturen voor alle resources.

## <a name="q14"></a>Wat Application Insights in mijn project wijzigen?
De details zijn afhankelijk van het type van het project. Voor een webtoepassing:

* Deze bestanden toevoegt aan uw project:

  * ApplicationInsights.config.
  * AI.js
* Deze NuGet-pakketten installeert:

  * *Application Insights-API* -de core API
  * *Application Insights-API voor webtoepassingen* : wordt gebruikt voor het verzenden van telemetrie van de server
  * *Application Insights-API voor JavaScript-toepassingen* : wordt gebruikt voor het verzenden van telemetrie van de client

    De pakketten zijn onder andere de volgende assembly's:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Hiermee voegt u items in:

  * Web.config
  * packages.config
* (Nieuwe alleen - projecten als u [Application Insights toevoegen aan een bestaand project][start], u moet dit handmatig doen.) Hiermee voegt u codefragmenten in de client en server code ze initialiseren met de Application Insights-resource-ID. Bijvoorbeeld, wordt code in een MVC-app ingevoegd in de basispagina Views/Shared/_Layout.cshtml

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hoe voer ik een upgrade van oudere versies van de SDK?
Zie de [opmerkingen bij de release](app-insights-release-notes.md) voor de SDK die geschikt is voor uw type van de toepassing.

## <a name="update"></a>Hoe kan ik welke Mijn project gegevens naar verzendt Azure-resource wijzigen?
Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update Application Insights**. U kunt de gegevens verzenden naar een bestaande of nieuwe resource in Azure. De wizard update verandert de instrumentatiesleutel in ApplicationInsights.config, waarmee wordt bepaald waar uw gegevens in de SDK-server wordt verzonden. Tenzij u 'Alles bijwerken' uitschakelt, wordt de sleutel waar deze wordt weergegeven in uw webpagina's ook gewijzigd.

## <a name="what-is-status-monitor"></a>Wat is Status Monitor?

Een bureaublad-app die u in uw IIS-webserver gebruiken kunt voor het configureren van Application Insights in web-apps. Status monitor verzamelt niet telemetrie: u kunt voorkomen dat deze wanneer u een app niet configureert. 

[Meer informatie](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Welke telemetrie wordt verzameld door Application Insights?

Van server-web-apps:

* HTTP-aanvragen
* [Afhankelijkheden](app-insights-asp-net-dependencies.md). Aanroepen naar: SQL-Databases. HTTP-aanroepen naar externe services; Azure Cosmos DB, tabel, blob-opslag en wachtrij. 
* [Uitzonderingen](app-insights-asp-net-exceptions.md) en stack-traces.
* [Prestatiemeteritems](app-insights-performance-counters.md) : als u [Status Monitor](app-insights-monitor-performance-live-website-now.md), [Azure monitoring](app-insights-azure-web-apps.md) of de [Application Insights verzamelde schrijver](app-insights-java-collectd.md).
* [Aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md) dat u de code.
* [Logboeken met traceringen](app-insights-asp-net-trace-logs.md) als u de juiste collector configureert.

Van [client webpagina's](app-insights-javascript.md):

* [Aantal paginaweergaven](app-insights-usage-overview.md)
* [AJAX-aanroepen](app-insights-asp-net-dependencies.md) aanvragen van een script uit te voeren.
* Gegevens laden van pagina weergeven
* Aantal gebruikers en sessies
* [Geverifieerde gebruikers-id 's](app-insights-api-custom-events-metrics.md#authenticated-users)

Uit andere bronnen, als u ze configureren:

* [Azure diagnostics](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Docker-containers](app-insights-docker.md)
* [Tabellen importeren in Analytics](app-insights-analytics-import.md)
* [Log Analytics](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Kan ik wegfilteren of telemetrie wijzigen?

Ja, in de server die u kunt schrijven:

* Telemetrie-Processor om te filteren of eigenschappen aan geselecteerde telemetrie-items toevoegen, voordat ze worden verzonden vanuit uw app.
* Telemetrie-Initializer eigenschappen toevoegen aan alle telemetrie-items.

Meer informatie voor [ASP.NET](app-insights-api-filtering-sampling.md) of [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Hoe worden stad, land en andere gegevens geo-locatie berekend?

We controleren of het IP-adres (IPv4 of IPv6) van de webclient met behulp van [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Telemetrie van de browser: We verzamelen van de afzender IP-adres.
* Servertelemetrie: De module Application Insights verzamelt IP-adres van de client. Het is niet verzameld als `X-Forwarded-For` is ingesteld.

U kunt configureren dat de `ClientIpHeaderTelemetryInitializer` overnemen van het IP-adres van een andere koptekst. In sommige systemen, bijvoorbeeld, wordt deze verplaatst door een proxy, load balancer of een CDN `X-Originating-IP`. [Meer informatie](https://apmtips.com/blog/2016/07/05/client-ip-address/).

U kunt [Power BI gebruiken](app-insights-export-power-bi.md) om de aanvraagtelemetrie van uw op een kaart weer te geven.


## <a name="data"></a>Hoelang gegevens bewaard in de portal? Is het veilig?
Kijk eens [bewaren van gegevens en Privacy][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-the-telemetry"></a>Mogelijk persoonlijk identificeerbare informatie (PII) worden verzonden in de telemetrie?

Dit is mogelijk als de code dergelijke gegevens verzendt. Het kan ook gebeuren als variabelen in de stack-traces PII bevatten. Uw ontwikkelteam moet risicobeoordelingen om ervoor te zorgen dat PII goed wordt afgehandeld uitvoeren. [Meer informatie over het bewaren van gegevens en privacy](app-insights-data-retention-privacy.md).

**Alle** octetten van het webadres van de client altijd zijn ingesteld op 0 nadat de geo-locatie-kenmerken worden opgezocht.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>Mijn iKey is zichtbaar in de bron van mijn webpagina. 

* Dit is gebruikelijk om in oplossingen voor de controle.
* Het kan niet worden gebruikt voor het stelen van uw gegevens.
* Het kan worden gebruikt om uw gegevens of trigger waarschuwingen scheeftrekken.
* We hebben geen gehoord dat elke klant met een dergelijke problemen heeft gehad.

U kunt de onderstaande:

* Gebruik twee verschillende iKeys (Scheid Application Insights-resources), voor client en server-gegevens. of
* Schrijven van een proxy die wordt uitgevoerd op uw server en de web client verzenden van gegevens via deze proxy.

## <a name="post"></a>Hoe kan ik postgegevens doorzoeken van diagnostische gegevens zien?
We geen bericht gegevens automatisch vastleggen, maar u kunt een aanroep van TrackTrace: de gegevens in de parameter bericht plaatsen. Dit heeft een limiet langer dan de grenzen van de eigenschappen van een verbindingsreeks, maar u kunt niet op het filteren.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Moet ik één of meerdere Application Insights-resources gebruiken?

Gebruik één resource voor alle onderdelen of rollen in een enkele business-systeem. Gebruik afzonderlijke resources voor ontwikkelings-, test- en release-versies en voor onafhankelijke toepassingen.

* [Zie het onderwerp hier](app-insights-separate-resources.md)
* [Voorbeeld - service in de cloud met behulp van worker en webservice](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>Hoe kan ik de instrumentatiesleutel dynamisch wijzigen?

* [Hier discussie](app-insights-separate-resources.md)
* [Voorbeeld - service in de cloud met behulp van worker en webservice](app-insights-cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Wat zijn de gebruikers- en telt?

* De JavaScript-SDK wordt de cookie van een gebruiker op de webclient, om terugkerende gebruikers te identificeren en een sessiecookie ingesteld om activiteiten te groeperen.
* Als er geen client-side-script, kunt u [cookies ingesteld op de server](https://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Als een echte gebruiker maakt gebruik van uw site in verschillende browsers of met behulp van in-privé-/ incognito te bladeren, of verschillende computers, en ze zullen meer dan één keer worden geteld.
* Voor het identificeren van een gebruiker is aangemeld op computers en browsers, Voeg een aanroep naar [setAuthenticatedUserContext()](app-insights-api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a> Hebt ik alles in Application Insights ingeschakeld?
| U ziet | Hoe u deze ophalen | Waarom u deze wilt |
| --- | --- | --- |
| Beschikbaarheid van grafieken |[Webtests](app-insights-monitor-web-app-availability.md) |Kent dat uw web-app is actief |
| Server app-prestaties: reactietijden,... |[Application Insights toevoegen aan uw project](app-insights-asp-net.md) of [AI Status Monitor installeren op de server](app-insights-monitor-performance-live-website-now.md) (of uw eigen code te schrijven [afhankelijkheden bijhouden](app-insights-api-custom-events-metrics.md#trackdependency)) |Perf problemen detecteren |
| Afhankelijkheidstelemetrie |[AI Status Monitor installeren op server](app-insights-monitor-performance-live-website-now.md) |Problemen met databases of andere externe componenten |
| Ontvang stack-traces van uitzonderingen |[TrackException aanroepen invoegen in uw code](app-insights-asp-net-exceptions.md) (maar enkele automatisch worden gerapporteerd) |Detecteer en diagnosticeer uitzonderingen |
| Logboektraceringen zoeken |[Een registratie-adapter toevoegen](app-insights-asp-net-trace-logs.md) |Uitzonderingen, prestaties, problemen vaststellen |
| Grondbeginselen van de client gebruik: paginaweergaven, sessies,... |[De initialisatiefunctie JavaScript in webpagina 's](app-insights-javascript.md) |Gebruiksanalyse |
| Aangepaste metrische gegevens van client |[Tracerings-aanroepen in webpagina 's](app-insights-api-custom-events-metrics.md) |Gebruikerservaring te verbeteren |
| Aangepaste metrische gegevens van server |[Aanroepen voor het bijhouden van server](app-insights-api-custom-events-metrics.md) |Business intelligence |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Waarom zijn de aantallen in zoek- en metrische gegevens over grafieken ongelijk?

[Sampling](app-insights-sampling.md) vermindert het aantal telemetrie-items (aanvragen, aangepaste gebeurtenissen, enzovoort) die daadwerkelijk vanuit uw app worden verzonden naar de portal. In Search ziet u het aantal items daadwerkelijk ontvangen. In grafieken met metrische gegevens die een aantal gebeurtenissen worden weergegeven, ziet u het nummer van de oorspronkelijke gebeurtenissen die hebben plaatsgevonden. 

Elk item dat is verzonden uitvoert een `itemCount` eigenschap die laat hoeveel oorspronkelijke gebeurtenissen dat item zien vertegenwoordigt. Als u wilt nagaan steekproeven in bewerking, kunt u deze query uitvoeren in Analytics:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Automation

### <a name="configuring-application-insights"></a>Application Insights configureren

U kunt [PowerShell-scripts schrijven](app-insights-powershell.md) met behulp van Azure Resource Monitor:

* Maken en bijwerken van Application Insights-resources.
* Stel de prijsstelling.
* De instrumentatiesleutel ophalen.
* Een metrische waarschuwing toevoegen.
* Een beschikbaarheidstest toevoegen.

U kan instellen van een rapport Metric Explorer of instellen van continue export.

### <a name="querying-the-telemetry"></a>Uitvoeren van query's de telemetrie

Gebruik de [REST-API](https://dev.applicationinsights.io/) om uit te voeren [Analytics](app-insights-analytics.md) query's.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Hoe kan ik een melding instellen op een gebeurtenis?

Azure-waarschuwingen zijn alleen voor metrische gegevens. Maak een aangepaste meetwaarde die een waarde drempel overschrijden wanneer de gebeurtenis zich voordoet. Vervolgens stelt u een waarschuwing op de metrische gegevens. Houd er rekening mee dat: u ontvangt een melding wanneer de metriek de drempelwaarde in beide richtingen; u ontvangen een melding totdat de eerste overschrijding, ongeacht of de aanvankelijke waarde hoog of laag; is geen Er is altijd een latentie van een paar minuten.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Zijn er kosten voor gegevensoverdracht tussen een Azure-web-app en de Application Insights?

* Als uw Azure-web-app wordt gehost in een datacenter waar er een verzamelingseindpunt voor Application Insights is, er zijn geen kosten verbonden. 
* Als er geen verzamelingseindpunt in uw datacenter host is, wordt de telemetrie van uw app wordt in rekening gebracht [Azure kosten uitgaande](https://azure.microsoft.com/pricing/details/bandwidth/).

Dit wordt niet afhankelijk van waar uw Application Insights-resource wordt gehost. Dit is alleen afhankelijk van de distributie van onze eindpunten.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Kan ik telemetrie verzenden naar de Application Insights-portal?

Aangeraden u onze SDK's gebruiken en gebruik de [SDK-API](app-insights-api-custom-events-metrics.md). Er zijn varianten van de SDK voor verschillende [platforms](app-insights-platforms.md). Deze SDK's verwerken buffer, compressie, beperking, nieuwe pogingen, enzovoort. Echter, de [opname schema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) en [eindpunt protocol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) openbaar zijn.

## <a name="can-i-monitor-an-intranet-web-server"></a>Kan ik een intranet-webserver controleren?

Hier volgen twee methoden:

### <a name="firewall-door"></a>De deur van de firewall

Toestaan dat uw webserver voor het verzenden van telemetrie naar onze eindpunten https://dc.services.visualstudio.com:443 en https://rt.services.visualstudio.com:443. 

### <a name="proxy"></a>Proxy

Routeren van verkeer van uw server aan een gateway op uw intranet door overwritting deze instellingen in het voorbeeld ApplicationInsights.config. Als deze eigenschappen 'Eindpunt' niet aanwezig in uw configuratie zijn, wordt deze klassen gebruikt de standaardwaarden die in het voorbeeld hieronder wordt weergegeven.

#### <a name="example-applicationinsightsconfig"></a>Voorbeeld ApplicationInsights.config:
```xml
<ApplicationInsights>
    ...
    <TelemetryChannel>
         <EndpointAddress>https://dc.services.visualstudio.com/v2/track</EndpointAddress>
    </TelemetryChannel>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

_Houd er rekening mee ApplicationIdProvider is beschikbaar in v2.6.0_

Uw gateway moet het verkeer routeren https://dc.services.visualstudio.com:443

Vervang in de bovenstaande waarden met: `http://<your.gateway.address>/<relative path>`
 
Voorbeeld: 
```
http://<your.gateway.endpoint>/v2/track 
http://<your.gateway.endpoint>/api/profiles/{0}/apiId
```




## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Kan ik webtests voor beschikbaarheid uitvoeren op een intranetserver?

Onze [webtests](app-insights-monitor-web-app-availability.md) worden uitgevoerd op de punten van de aanwezigheid die zijn verdeeld over de hele wereld. Er zijn twee mogelijke oplossingen:

* De deur van Firewall - aanvragen toestaan op uw server uit [de lange en instelbare lijst web testagents](app-insights-ip-addresses.md).
* Schrijf uw eigen code voor het verzenden van periodieke aanvragen naar uw server uit in uw intranet. U kunt Visual Studio-webtests die voor dit doel kan uitvoeren. Het testprogramma kan de resultaten verzenden naar Application Insights met behulp van de API TrackAvailability().

## <a name="more-answers"></a>Meer antwoorden
* [Application Insights-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
