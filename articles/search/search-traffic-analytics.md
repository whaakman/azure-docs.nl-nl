---
title: Search Traffic Analytics - Azure Search
description: Analyse zoekverkeer inschakelen voor Azure Search, een search-service van de cloud worden gehost op Microsoft Azure, voor het ontgrendelen van inzicht in uw gebruikers en uw gegevens.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/05/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4cc7434508e49715e95c87421db2bbed7e20de05
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310236"
---
# <a name="what-is-search-traffic-analytics"></a>Wat is analyse zoekverkeer
Analyse zoekverkeer is een patroon voor het implementeren van een feedback-lus voor uw zoekservice. Dit patroon beschrijft de benodigde gegevens en over het verzamelen van deze met Application Insights, een industrieleider noemt voor het bewaken van services op meerdere platforms.

Analyse zoekverkeer kunt u inzicht in uw search-service en verkrijgt u inzichten over uw gebruikers en hun gedrag. Doordat de gegevens over wat uw gebruikers kiest, is het mogelijk om beslissingen te nemen die uw zoekervaring verder te verbeteren en back-uitschakelen wanneer de resultaten zijn niet wat verwacht.

Azure Search biedt een telemetrie-oplossing die kan worden geïntegreerd Azure Application Insights en Power BI voor een uitgebreide bewaking en bij te houden. Omdat de interactie met Azure Search is alleen via API's, moet u de telemetrie geïmplementeerd door de ontwikkelaars met behulp van search, de instructies op deze pagina te volgen.

## <a name="identify-the-relevant-search-data"></a>Identificeren van de gegevens van de relevante zoeken

Als u nuttige zoekopdracht metrische gegevens, is het nodig zijn om aan te melden sommige signalen van de gebruikers van de zoektoepassing. Deze signalen geven aan inhoud die gebruikers geïnteresseerd bent in en die relevant zijn voor hun behoeften overwegen.

Er zijn twee signalen die Search Traffic Analytics moet:

1. Gebeurtenissen voor gebruiker gegenereerde zoeken: alleen zoekquery's die door een gebruiker interessant zijn. Zoekaanvragen gebruikt voor het vullen van facetten, aanvullende inhoud of interne gegevens, niet belangrijk zijn en ze scheeftrekken en bijstelling van de resultaten.

2. Gebruiker gegenereerde klikt u op gebeurtenissen: Door te klikken in dit document verwijzen we naar een gebruiker te selecteren van een bepaalde zoekopdracht resultaat geretourneerd van een zoekquery. Klik in het algemeen betekent dat een document geen relevante resultaat voor een bepaalde zoekquery.

Door te zoeken en klikt u op gebeurtenissen met een correlatie-id koppelen, is het mogelijk voor het analyseren van het gedrag van gebruikers in uw toepassing. Deze inzichten zoeken zijn mogelijk te verkrijgen met alleen zoeken in logboeken over webverkeer.

## <a name="how-to-implement-search-traffic-analytics"></a>Het implementeren van zoekverkeer

De signalen die worden vermeld in de vorige sectie moeten worden verzameld bij de zoektoepassing als de gebruiker ermee. Application Insights is een uitbreidbare oplossing voor prestatiecontrole, beschikbaar voor meerdere platformen, met flexibele instrumentation opties. Gebruik van Application Insights kunt u profiteren van de rapporten van Power BI zoeken die zijn gemaakt door Azure Search om eenvoudiger de analyse van gegevens.

In de [portal](https://portal.azure.com) pagina voor uw Azure Search-service, de blade Search Traffic Analytics bevat een referentieoverzicht voor het volgen van dit patroon telemetrie. U kunt ook selecteren of maken van een Application Insights-resource en de benodigde gegevens, allemaal op één locatie bekijken.

![Search Traffic Analytics instructies][1]

### <a name="1-select-an-application-insights-resource"></a>1. Selecteer een Application Insights-resource

U moet selecteren van een Application Insights-resource te gebruiken of als u er nog geen hebt. U kunt een resource die wordt al gebruikt om de vereiste aangepaste gebeurtenissen gebruiken.

Bij het maken van een nieuwe Application Insights-resource, zijn alle soorten toepassingen geldig voor dit scenario. Optie die het beste past bij het platform dat u gebruikt.

In dat geval moet u de instrumentatiesleutel in voor het maken van de telemetrieclient voor uw toepassing. U kunt deze ophalen van het dashboard van de Application Insights-portal, of kunt u deze ophalen van de pagina Search Traffic Analytics selecteren van het exemplaar dat u wilt gebruiken.

### <a name="2-instrument-your-application"></a>2. Uw toepassing instrumenteren

Deze fase is waar de instrumenteren van uw eigen zoektoepassing met de Application Insights-resource gemaakt in de bovenstaande stap. Er zijn vier stappen om dit proces:

**IK. Maak een telemetrieclient** dit is het object die gebeurtenissen worden verzonden naar de Application Insights-Resource.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Voor andere talen en platformen, Zie de volledige [lijst](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Aanvragen van een ID zoeken voor correlatie** om te correleren zoekaanvragen met klikken, is het nodig zijn om een correlatie-id die is gekoppeld aan deze twee afzonderlijke gebeurtenissen. Azure Search biedt een Id zoeken wanneer u deze aanvraag met een header:

*C#*

    // This sample uses the Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Meld u gebeurtenissen voor zoeken**

Elke keer dat een zoekaanvraag is uitgegeven door een gebruiker, moet u zich die als een gebeurtenis zoeken met het volgende schema op een aangepaste gebeurtenis van Application Insights:

**ServiceName**: (tekenreeks) zoekservicenaam **SearchId**: de unieke id (guid) van de zoekquery (komt in het antwoord zoeken) **index**: (tekenreeks) search-service-index moet opgevraagd **QueryTerms**: (tekenreeks) zoektermen ingevoerd door de gebruiker **ResultCount**: het aantal documenten die zijn geretourneerd (int) (komt in het antwoord zoeken)  **ScoringProfile**: naam van het scoringprofiel gebruikt, indien van toepassing (tekenreeks)

> [!NOTE]
> Aantal aanvragen op query's van gebruikers die zijn gegenereerd door toe te voegen $count = waar uw zoekopdracht. Zie voor meer informatie [hier](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Vergeet niet om aan te melden alleen zoekquery's die worden gegenereerd door gebruikers.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Meld u klikt u op gebeurtenissen**

Elke keer dat een gebruiker een document dat een signaal dat moet worden geregistreerd voor analysedoeleinden zoeken op. Aangepaste gebeurtenissen van Application Insights gebruiken om deze gebeurtenissen met het volgende schema:

**ServiceName**: (tekenreeks) zoekservicenaam **SearchId**: de unieke id (guid) van de query verwante zoekopdrachten **DocId**: id van het document (tekenreeks) **positie**: (int) positie van het document in de zoekopdracht resultaten pagina

> [!NOTE]
> Positie verwijst naar de volgorde van de kardinaalcurve in uw toepassing. U bent om in te stellen van dit getal, gratis, zolang het is altijd hetzelfde, om toe te staan voor de vergelijking.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Analyseren met Power BI Desktop

Nadat u hebt uw app geïmplementeerd en geverifieerd met dat uw toepassing correct is verbonden met Application Insights, kunt u een vooraf gedefinieerde sjabloon gemaakt door Azure Search voor Power BI desktop.
Deze sjabloon bevat grafieken en tabellen die u helpen beter onderbouwde beslissingen om uw prestaties en relevantie te verbeteren.

Als u wilt de Power BI desktop-sjabloon maken, moet u drie soorten informatie over Application Insights. Deze gegevens kunnen worden gevonden op de pagina Search Traffic Analytics, wanneer u selecteert de resource moet worden gebruikt

![Application Insights-gegevens in de blade Search Traffic Analytics][2]

Metrische gegevens opgenomen in de Power BI desktop-sjabloon:

*   Klikt u op via tarief (CTR): verhouding van gebruikers die op een bepaald document aan het aantal totale zoekopdrachten.
*   Zoekacties zonder klikken: voorwaarden voor belangrijkste query's die geen klikken registreren
*   Meest geklikt documenten: meest geklikt documenten door de ID in de afgelopen 24 uur, 7 dagen en 30 dagen.
*   Populaire term-document paren: voorwaarden die in het document op hebt geklikt resulteren, geordend door te klikken.
*   Tijd te klikken: gerangschikte klikken door de tijd sinds de zoekquery

![Power BI-sjabloon voor het lezen van Application Insights][3]


## <a name="next-steps"></a>Volgende stappen
Instrumenteer uw zoektoepassing krachtige en inzichtelijke gegevens over uw search-service op te halen.

U vindt meer informatie over Application Insights [hier](https://go.microsoft.com/fwlink/?linkid=842905). Ga naar Application Insights [pagina met prijzen](https://azure.microsoft.com/pricing/details/application-insights/) voor meer informatie over de verschillende Servicelagen.

Meer informatie over het maken van geweldige rapporten. Zie [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) voor meer informatie

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
