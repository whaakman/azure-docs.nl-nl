---
title: Azure Application Insights Veelgestelde vragen | Microsoft Docs
description: Veelgestelde vragen over Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: mbullwin
ms.openlocfilehash: 9f6cf019d681ce6e844481ca58de1ff472b3d32c
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Veelgestelde vragen

## <a name="configuration-problems"></a>Configuratieproblemen
*Ik ondervind problemen met het instellen van mijn:*

* [.NET-app](app-insights-asp-net-troubleshoot-no-data.md)
* [Bewaking van een app al actief](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure diagnostics](app-insights-azure-diagnostics.md)
* [Java-web-app](app-insights-java-troubleshoot.md)

*Ik heb ophalen geen gegevens uit mijn server*

* [Set firewall-uitzonderingen](app-insights-ip-addresses.md)
* [Een ASP.NET-server instellen](app-insights-monitor-performance-live-website-now.md)
* [Een Java-server instellen](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Kan ik Application Insights met... gebruiken?

* [Web-apps op een IIS-server - op-premises of in een virtuele machine](app-insights-asp-net.md)
* [Java-web-apps](app-insights-java-get-started.md)
* [Node.js-apps](app-insights-nodejs.md)
* [Web-apps in Azure](app-insights-azure-web-apps.md)
* [Cloudservices op Azure](app-insights-cloudservices.md)
* [Appservers die in Docker](app-insights-docker.md)
* [Single-page-web-apps](app-insights-javascript.md)
* [SharePoint](app-insights-sharepoint.md)
* [Windows-bureaublad-app](app-insights-windows-desktop.md)
* [Andere platforms](app-insights-platforms.md)

## <a name="is-it-free"></a>Is het gratis?

Ja, voor experimenteel gebruik. In de basis prijsstelling kan uw toepassing een bepaalde toegestane gegevens elke maand gratis verzenden. Het gratis tegoed is groot genoeg is voor de ontwikkeling van behandeld en publiceren van een app voor een klein aantal gebruikers. U kunt instellen dat een limiet om te voorkomen dat meer dan een opgegeven hoeveelheid gegevens wordt verwerkt.

Grotere volumes telemetrie door de Gb in rekening worden gebracht. We bieden een aantal tips over het [uw kosten beperken](app-insights-pricing.md).

De Enterprise-serviceplan leidt ertoe dat kosten met zich mee voor elke dag dat elke webserverknooppunt telemetrie verzendt. Het is geschikt als u wilt gebruiken van continue Export op grote schaal.

[Lees de prijsstelling](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Hoeveel is deze kosten

* Open de **functies en prijzen** pagina in een Application Insights-resource. Er is een overzicht van recente informatie over het gebruik. Als u wilt, kunt u een limiet van het volume gegevens instellen.
* Open de [blade facturering Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) om te zien van rekeningen over alle resources.

## <a name="q14"></a>Wat Application Insights in mijn project wijzigen?
De details zijn afhankelijk van het type project. Voor een webtoepassing:

* Deze bestanden toevoegt aan uw project:

  * ApplicationInsights.config.
  * AI.js
* Installeert deze NuGet-pakketten:

  * *Application Insights-API* -de kern API
  * *Application Insights-API voor webtoepassingen* : wordt gebruikt voor het verzenden van telemetrie van de server
  * *Application Insights-API voor JavaScript toepassingen* : wordt gebruikt voor het verzenden van telemetrie vanaf de client

    De pakketten bevatten deze assembly's:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Hiermee voegt items in:

  * Web.config
  * Packages.config
* (Nieuwe alleen - projecten als u [Application Insights toevoegen aan een bestaand project][start], hebt u dit handmatig doen.) Codefragmenten ingevoegd in de client en server code te initialiseren met de Application Insights-resource-ID. Bijvoorbeeld: code in een MVC-app is ingevoegd in de basispagina Views/Shared/_Layout.cshtml

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hoe voer ik een upgrade van oudere versies van de SDK?
Zie de [release-opmerkingen](app-insights-release-notes.md) voor de SDK die past bij uw toepassing.

## <a name="update"></a>Hoe kan ik welke Azure-resource mijn project gegevens worden verzonden wijzigen?
Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update Application Insights**. U kunt de gegevens verzenden naar een bestaande of nieuwe bron in Azure. De wizard update verandert de instrumentatiesleutel in ApplicationInsights.config, waarmee wordt bepaald wanneer de SDK-server verzendt uw gegevens. Tenzij u 'Alles bijwerken' uitschakelt, wordt de sleutel waar deze wordt weergegeven in uw webpagina's ook gewijzigd.

## <a name="what-is-status-monitor"></a>Wat is Status Monitor?

Een bureaublad-app die u in uw IIS-webserver gebruiken kunt om u te helpen bij het configureren van Application Insights in web-apps. Deze telemetrie niet verzamelen: u kunt deze stoppen wanneer u een app niet configureert. 

[Meer informatie](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Welke telemetrie in Application Insights worden verzameld?

Vanuit server-web-apps:

* HTTP-aanvragen
* [Afhankelijkheden](app-insights-asp-net-dependencies.md). Aanroepen naar: SQL-Databases. HTTP-aanroepen naar externe services; Azure DB Cosmos, tabel, blob-opslag en wachtrij. 
* [Uitzonderingen](app-insights-asp-net-exceptions.md) en traceringen.
* [Prestatiemeteritems](app-insights-performance-counters.md) : als u [Status Monitor](app-insights-monitor-performance-live-website-now.md), [Azure monitoring](app-insights-azure-web-apps.md) of de [Application Insights collectd writer](app-insights-java-collectd.md).
* [Aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md) dat u de code.
* [Traceerlogboeken](app-insights-asp-net-trace-logs.md) als u de juiste collector configureert.

Van [client webpagina's](app-insights-javascript.md):

* [Paginaweergave geteld](app-insights-web-track-usage.md)
* [AJAX-aanroepen](app-insights-asp-net-dependencies.md) aanvragen worden gedaan vanuit een script wordt uitgevoerd.
* Gegevens laden van pagina weergeven
* Telt het aantal gebruikers- en sessiegegevens
* [Geverifieerde gebruikers-id 's](app-insights-api-custom-events-metrics.md#authenticated-users)

Uit andere bronnen, als u ze configureren:

* [Azure diagnostics](app-insights-azure-diagnostics.md)
* [Docker-containers](app-insights-docker.md)
* [Tabellen in Analytics importeren](app-insights-analytics-import.md)
* [OMS (Log Analytics)](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Kan ik een filter of telemetrie wijzigen?

Ja, in de server die u kunt schrijven:

* Telemetrie-Processor om te filteren of eigenschappen voor de geselecteerde telemetrie-items toevoegen voordat ze worden verzonden vanuit uw app.
* Telemetrie-initialisatiefunctie eigenschappen toevoegen aan alle telemetrie-items.

Meer informatie voor [ASP.NET](app-insights-api-filtering-sampling.md) of [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Hoe worden stad, land en andere gegevens van de locatie geo berekend?

We opzoeken van het IP-adres (IPv4 of IPv6) van de webclient met behulp van [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Browser telemetrie: IP-adres van de afzender worden verzameld.
* Servertelemetrie: de Application Insights-module verzamelt IP-adres van de client. Het is niet verzameld als `X-Forwarded-For` is ingesteld.

U kunt de `ClientIpHeaderTelemetryInitializer` overnemen van het IP-adres van een andere koptekst. In sommige systemen, bijvoorbeeld: het is verplaatst door een proxy, load balancer of CDN `X-Originating-IP`. [Meer informatie](http://apmtips.com/blog/2016/07/05/client-ip-address/).

U kunt [Power BI gebruiken](app-insights-export-power-bi.md) om de aanvraagtelemetrie van uw op een kaart weer te geven.


## <a name="data"></a>Hoe lang gegevens bewaard in de portal? Is het veilig?
Kijk eens naar [bewaren van gegevens en Privacy][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-the-telemetry"></a>Mogelijk persoonsgegevens (PII) gegevens verzonden in de telemetrie?

Dit is mogelijk als uw code dergelijke gegevens verzendt. Het kan ook gebeuren als variabelen in de stack-traces PII bevatten. Uw ontwikkelteam neemt risicoanalyse om ervoor te zorgen dat PII goed wordt afgehandeld. [Meer informatie over bewaren van gegevens en privacy](app-insights-data-retention-privacy.md).

Het laatste octet van het webadres van de client is altijd ingesteld op 0 na opname door de portal.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>Mijn iKey is zichtbaar in de bron van mijn webpagina. 

* Dit is gebruikelijk om in de bewaking van oplossingen.
* Deze kan niet worden gebruikt om uw gegevens worden gestolen.
* Het kan worden gebruikt om te leiden tot onjuiste gegevens of trigger waarschuwingen.
* We hebben geen gehoord dat elke klant dergelijke problemen heeft gehad.

U kunt doen:

* Twee afzonderlijke iKeys (Scheid Application Insights-resources), gebruiken voor gegevens van client en server. of
* Schrijven van een proxy die wordt uitgevoerd op uw server en de web-client verzenden van gegevens via deze proxy.

## <a name="post"></a>Hoe kan ik postgegevens in diagnostische gegevens doorzoeken zien?
Er geen postgegevens automatisch wordt geregistreerd, maar u kunt een aanroep van TrackTrace: de gegevens in de parameter bericht plaatsen. Dit heeft een limiet langer dan de grenzen van tekenreekseigenschappen, maar u kunt niet op het filteren.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Moet ik Application Insights-resources voor één of meerdere gebruiken?

Gebruik één resource voor de onderdelen of rollen in een één-systeem. Afzonderlijke resources gebruiken voor ontwikkeling, testen en release-versies, en voor onafhankelijke toepassingen.

* [Zie het onderwerp hier](app-insights-separate-resources.md)
* [Voorbeeld - cloudservice met worker en webservice-functies](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>Hoe kan ik de instrumentatiesleutel dynamisch wijzigen?

* [Hier discussie](app-insights-separate-resources.md)
* [Voorbeeld - cloudservice met worker en webservice-functies](app-insights-cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Wat zijn de gebruikers- en sessiegegevens telt?

* De JavaScript SDK wordt de cookie van een gebruiker op de webclient, om terugkerende gebruikers te identificeren en een sessiecookie ingesteld om activiteiten te groeperen.
* Als er geen clientscript, kunt u [cookies worden ingesteld op de server](http://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Als een echte gebruiker maakt gebruik van uw site in andere browsers of met in-persoonlijke/incognito bladeren of andere machines vervolgens zij wordt meer dan één keer worden geteld.
* Als u wilt een aangemelde gebruiker identificeren over machines en browsers, Voeg een aanroep naar [setAuthenticatedUserContext()](app-insights-api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a>Hebben ik alles in Application Insights ingeschakeld?
| U ziet | Het ophalen van het | Gewenste waarom |
| --- | --- | --- |
| Beschikbaarheid grafieken |[Webtests](app-insights-monitor-web-app-availability.md) |Kent dat uw web-app actief is |
| Server app-prestaties: reactietijden,... |[Application Insights toevoegen aan uw project](app-insights-asp-net.md) of [AI Status Monitor installeren op de server](app-insights-monitor-performance-live-website-now.md) (of uw eigen code te schrijven [afhankelijkheden bijhouden](app-insights-api-custom-events-metrics.md#trackdependency)) |Perf problemen detecteren |
| Afhankelijkheidstelemetrie |[AI Status Monitor installeren op server](app-insights-monitor-performance-live-website-now.md) |Problemen met databases of andere externe componenten vaststellen |
| Stack-traces van uitzonderingen ophalen |[TrackException aanroepen invoegen in uw code](app-insights-asp-net-exceptions.md) (maar een aantal automatisch worden gerapporteerd) |Detecteren en onderzoeken van uitzonderingen |
| Logboektraceringen zoeken |[Voeg een adapter logboekregistratie](app-insights-asp-net-trace-logs.md) |Uitzonderingen, perf problemen onderzoeken |
| Basisbeginselen van de client gebruik: paginaweergaven, sessies,... |[JavaScript-initialisatiefunctie in webpagina 's](app-insights-javascript.md) |Gebruiksanalyse |
| Aangepaste metrische gegevens van client |[Bijhouden roept in webpagina 's](app-insights-api-custom-events-metrics.md) |Gebruikerservaring verbeteren |
| Aangepaste metrische gegevens van server |[Bijhouden aanroepen op de server](app-insights-api-custom-events-metrics.md) |Business intelligence |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Waarom zijn het aantal in zoek- en metrische gegevens grafieken ongelijke?

[Steekproef nemen](app-insights-sampling.md) vermindert het aantal telemetrie-items (aanvragen, aangepaste gebeurtenissen, enzovoort) die daadwerkelijk van uw app worden verzonden naar de portal. In de zoekopdracht ziet u het aantal items daadwerkelijk ontvangen. In de metrische diagrammen waarin een aantal gebeurtenissen worden weergegeven, ziet u het nummer van de oorspronkelijke gebeurtenissen die hebben plaatsgevonden. 

Elk item dat is verzonden uitvoert een `itemCount` eigenschap die laat hoeveel oorspronkelijke gebeurtenissen dat het item zien vertegenwoordigt. Als u wilt nagaan steekproeven bij bewerking, kunt u deze query uitvoeren in Analytics:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Automatisering

### <a name="configuring-application-insights"></a>Application Insights configureren

U kunt [PowerShell-scripts schrijven](app-insights-powershell.md) met Azure Broncontrole naar:

* Maken en bijwerken van Application Insights-resources.
* De prijsstelling ingesteld.
* De instrumentatiesleutel ophalen.
* Een metriek waarschuwing toevoegen.
* Een beschikbaarheidstest toevoegen.

U kan een rapport metriek Explorer instellen of instellen van continue export.

### <a name="querying-the-telemetry"></a>Opvragen van de telemetrie

Gebruik de [REST-API](https://dev.applicationinsights.io/) om uit te voeren [Analytics](app-insights-analytics.md) query's.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Hoe kan ik een waarschuwing instellen op een gebeurtenis?

Waarschuwingen van Azure zijn alleen voor metrische gegevens. Maak een aangepaste metrische gegevens die over een drempelwaarde waarde wanneer de gebeurtenis zich voordoet. Een waarschuwing wordt ingesteld op de metriek. Houd er rekening mee dat: u ontvangt een melding wanneer de metriek de drempelwaarde in beide richtingen overschrijdt; u kunt een melding totdat de eerste overschrijding, ongeacht of de beginwaarde hoog of laag is; won't ophalen Er is altijd een latentie van enkele minuten duren.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Zijn er kosten van de gegevens overbrengen tussen een Azure-web-app en de Application Insights?

* Als uw Azure-web-app wordt gehost in een datacenter wanneer er een eindpunt van de verzameling Application Insights, er zijn geen kosten. 
* Als er geen verzamelingseindpunt in uw datacentrum host, wordt de telemetrie van uw app, worden [Azure kosten voor uitgaande](https://azure.microsoft.com/pricing/details/bandwidth/).

Dit wordt niet afhankelijk van waar uw Application Insights-resource wordt gehost. Dit is alleen afhankelijk van de distributie van onze eindpunten.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Kan ik telemetrie verzenden naar de Application Insights-portal?

Aangeraden gebruik onze SDK's en gebruik de [SDK-API](app-insights-api-custom-events-metrics.md). Er zijn varianten van de SDK voor verschillende [platforms](app-insights-platforms.md). Deze SDK's verwerken bufferfunctie, compressie, beperking, nieuwe pogingen, enzovoort. Echter, de [opname schema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) en [eindpunt protocol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) zijn openbaar.

## <a name="can-i-monitor-an-intranet-web-server"></a>Kan ik een intranet-webserver bewaken?

Hier zijn twee methoden:

### <a name="firewall-door"></a>De deur van de firewall

Toestaan dat de webserver te verzenden van telemetrie naar onze eindpunten https://dc.services.visualstudio.com:443 en https://rt.services.visualstudio.com:443. 

### <a name="proxy"></a>Proxy

Verkeer leiden van uw server met een gateway op uw intranet, dit door in te stellen ApplicationInsights.config:

```XML
<TelemetryChannel>
    <EndpointAddress>your gateway endpoint</EndpointAddress>
</TelemetryChannel>
```

Uw gateway moet het verkeer naar v2-https://dc.services.visualstudio.com:443/bijhouden sturen

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Kan ik webtests voor beschikbaarheid op een intranetserver uitvoeren?

Onze [webtests](app-insights-monitor-web-app-availability.md) uitvoeren op de punten van de aanwezigheid die zijn verdeeld over de hele wereld. Er zijn twee oplossingen:

* Firewall-deur - aanvragen toestaan op uw server uit [de lang en instelbare lijst met test-Webagenten](app-insights-ip-addresses.md).
* Uw eigen code schrijven om te periodieke aanvragen verzenden naar uw server uit binnen uw intranet. U kunt Visual Studio-webtests voor dit doel kan uitvoeren. De tester kan de resultaten verzenden naar Application Insights TrackAvailability() API gebruiken.

## <a name="more-answers"></a>Meer antwoorden
* [Application Insights-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
