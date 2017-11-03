---
title: Zoek Azure Search Traffic Analytics | Microsoft Docs
description: Schakel Search traffic analytics voor Azure Search, een zoekservice in de cloud gehoste in Microsoft Azure voor het ontgrendelen van inzicht in uw gebruikers en uw gegevens.
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
ms.openlocfilehash: 303ca5c820f573dc0b58f1910f258403c3baad2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-search-traffic-analytics"></a>Wat is search traffic analytics
Search traffic analytics is een patroon voor het implementeren van een Feedbacklus voor uw zoekservice. Dit patroon beschrijft de benodigde gegevens en om te verzamelen met behulp van Application Insights, een industrie-opvulteken voor het bewaken van services op meerdere platforms.

Search traffic analytics kunt u meer inzicht verkrijgen in uw zoekservice en inzicht in uw gebruikers en hun gedrag te ontgrendelen. Doordat de gegevens over wat uw gebruikers kiest, is het mogelijk om beslissingen te nemen die de zoekfunctie verder te verbeteren, en voor wanneer de resultaten worden verwacht niet uitstellen.

Azure Search biedt een oplossing voor telemetrie die wordt geïntegreerd met Azure Application Insights en Power BI om diepgaande bewaken en bijhouden. Omdat de interactie met Azure Search is alleen via API's, moet u de telemetrie geïmplementeerd door de ontwikkelaars zoekactie, volgens de instructies in deze pagina.

## <a name="identify-the-relevant-search-data"></a>Identificeren van de relevante zoekgegevens

Als u nuttige zoekopdracht metrische gegevens, is het nodig om sommige signalen aanmelden van de gebruikers van de zoektoepassing. Deze signalen geven inhoud die gebruikers zijn geïnteresseerd zijn in- en die relevant zijn voor hun behoeften overwegen.

Er zijn twee signalen die Search Traffic Analytics moet:

1. Gebruiker gegenereerde zoekopdracht gebeurtenissen: alleen zoekquery's die zijn geïnitieerd door een gebruiker interessante zijn. Search-aanvragen gevuld facetten, aanvullende inhoud en ook geen interne informatie zijn niet belangrijk en ze leiden tot onjuiste en bijstelling van de resultaten.

2. Gebruiker gegenereerde klikt u op gebeurtenissen: door te klikken in dit document is een gebruiker een bepaald zoekresultaat geretourneerd vanuit de zoekquery's selecteren. Een Klik in het algemeen betekent dat een document geen relevante resultaat voor een specifieke zoekopdracht.

Door te zoeken en klikt u op gebeurtenissen met een correlatie-id koppelen, is het mogelijk te analyseren van het gedrag van gebruikers in uw toepassing. Deze inzichten zoeken zijn mogelijk te verkrijgen met alleen verkeer zoeklogboeken.

## <a name="how-to-implement-search-traffic-analytics"></a>Het implementeren van search traffic analytics

De signalen vermeld in de voorgaande sectie moeten worden verzameld van de zoektoepassing als de gebruiker wordt gebruikt. Application Insights is een uitbreidbaar bewakingsoplossing, beschikbaar voor meerdere platforms, flexibele instrumentation opties. Informatie over het gebruik van Application Insights kunt u profiteren van de Power BI zoeken rapporten gemaakt met Azure Search om de analyse van gegevens te vereenvoudigen.

In de [portal](https://portal.azure.com) pagina voor uw Azure Search-service, de blade Search Traffic Analytics bevat een referentieoverzicht voor het volgen van dit patroon telemetrie. U kunt selecteren of een Application Insights-resource maken en gegevens die nodig zijn, op één plek te bekijken.

![Search Traffic Analytics instructies][1]

### <a name="1-select-an-application-insights-resource"></a>1. Selecteer een Application Insights-resource

U moet een Application Insights-resource te gebruiken of een maken als u nog niet hebt selecteren. U kunt een resource die al wordt gebruikt om de vereiste aangepaste gebeurtenissen kunt gebruiken.

Wanneer u een nieuwe Application Insights-resource maakt, zijn alle toepassingstypen geldig voor dit scenario. Optie die het beste past bij het platform dat u gebruikt.

U moet de instrumentatiesleutel voor het maken van de telemetrie-client voor uw toepassing. U kunt dit downloaden vanuit het dashboard van Application Insights-portal of u kunt dit downloaden van de pagina Search Traffic Analytics selecteren van het exemplaar dat u wilt gebruiken.

### <a name="2-instrument-your-application"></a>2. Softwareontwikkelaars van uw toepassing

In deze fase is waar de softwareontwikkelaars van uw eigen zoektoepassing met de Application Insights-resource uw gemaakt in de bovenstaande stap. Er zijn vier stappen dit proces:

**IK. Maken van een client telemetrie** dit is het object dat gebeurtenissen naar de Application Insights-Resource verzendt.

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

Zie voor andere talen en platforms de volledige [lijst](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Aanvraag een Search-ID voor correlatie** voor de bijbehorende search-aanvragen met klikken, hoeft te hebben een correlatie-id die is gekoppeld van deze twee afzonderlijke gebeurtenissen. Azure Search biedt een Search-Id wanneer u deze met een header aanvragen:

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

**III. Logboekgebeurtenissen zoeken**

Elke keer dat een zoekaanvraag is uitgegeven door een gebruiker, u moet zich aanmelden die als een gebeurtenis zoeken met het volgende schema van een aangepaste Application Insights-gebeurtenis:

**ServiceName**: (tekenreeks) service zoeknaam **SearchId**: de unieke id (guid) van de zoekopdracht (komt in het antwoord zoeken) **NaamCommunity**: (tekenreeks)-zoekindex service moeten worden opgevraagd **QueryTerms**: zoektermen (tekenreeks) die zijn ingevoerd door de gebruiker **ResultCount**: (int) aantal documenten die zijn geretourneerd (komt in het antwoord zoeken) **ScoringProfile**: (tekenreeks) naam van het scoreprofiel gebruikt, indien van toepassing

> [!NOTE]
> Aantal aanvragen op de gebruiker gegenereerde query's door toe te voegen $count = True in uw query. Zie voor meer informatie [hier](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Houd er rekening mee aan te melden alleen zoekquery's die worden gegenereerd door gebruikers.
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

**IV. Meld u Klik gebeurtenissen**

Elke keer dat een gebruiker een document, dat een signaal dat moet worden geregistreerd voor analysedoeleinden zoekopdracht op. Gebruik aangepaste gebeurtenissen Application Insights voor deze gebeurtenissen met het volgende schema:

**ServiceName**: (tekenreeks) service zoeknaam **SearchId**: de unieke id (guid) van de gerelateerde zoekopdracht **DocId**: document-id (tekenreeks) **positie**: resultatenpagina (int) positie van het document in de zoekopdracht

> [!NOTE]
> Positie verwijst naar de volgorde van de kardinaalcurve in uw toepassing. U bent gratis dit nummer instellen zolang het is altijd hetzelfde, om toe te staan voor vergelijking.
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

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Analyseren met Power BI Desktop

Nadat u hebt uw app geïnstrumenteerd en uw toepassing correct is verbonden met Application Insights gecontroleerd, kunt u een vooraf gedefinieerde sjabloon is gemaakt met Azure Search voor Power BI desktop.
Deze sjabloon bevat grafieken en tabellen die u helpen betere beslissingen voor het verbeteren van de prestaties en relevantie maken.

Voor het concretiseren van de Power BI desktop-sjabloon, moet u drie soorten informatie over Application Insights. Deze gegevens kunnen worden gevonden op de pagina Search Traffic Analytics wanneer u selecteert de resource moet worden gebruikt

![Application Insights gegevens in de blade Search Traffic Analytics][2]

Metrische gegevens in de Power BI desktop-sjabloon opgenomen:

*   Klikt u op via snelheid (CTR): ratio van gebruikers die op een bepaald document aan het aantal totale zoekopdrachten.
*   Zoeken zonder te klikken: voorwaarden voor top-query's die geen klikken registreren
*   Meest geklikt documenten: meest geklikt documenten door-ID in de afgelopen 24 uur, 7 dagen en 30 dagen.
*   Paren van populaire term-document: voorwaarden die in hetzelfde document hebt geklikt resulteren, geordend door te klikken.
*   Tijd te klikken: gerangschikte klikken door tijd sinds de zoekopdracht

![Power BI-sjabloon voor het lezen van Application Insights][3]


## <a name="next-steps"></a>Volgende stappen
Softwareontwikkelaars uw zoektoepassing krachtige en begrijpelijke manier mee gegevens over uw search-service ophalen.

U vindt meer informatie over Application Insights [hier](https://go.microsoft.com/fwlink/?linkid=842905). Ga naar Application Insights [pagina met prijzen](https://azure.microsoft.com/pricing/details/application-insights/) voor meer informatie over de verschillende Servicelagen.

Meer informatie over het maken van fantastische rapporten. Zie [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) voor meer informatie

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
