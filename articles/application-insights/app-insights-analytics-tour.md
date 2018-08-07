---
title: Een rondleiding langs Analytics in Azure Application Insights | Microsoft Docs
description: Korte voorbeelden van alle belangrijkste query's in Analytics, de krachtige zoekfunctie van Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: mbullwin
ms.openlocfilehash: 470779f80e998c3908cf28328cfb415d98c5e06c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579252"
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Een rondleiding door analytische gegevens in Application Insights
[Analytics](app-insights-analytics.md) is van de krachtige zoekfunctie van [Application Insights](app-insights-overview.md). Deze pagina's beschrijven de querytaal van Log Analytics.

* **[Bekijk de inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Probeer de Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo)**  als uw app wordt niet van gegevens naar Application Insights nog verzenden.
* **[Overzichtskaart van SQL-gebruikers](https://aka.ms/sql-analytics)**  vertaalt de meest voorkomende idioms.

We gaan een beknopt overzicht van enkele eenvoudige query's aan de slag te gaan.

## <a name="connect-to-your-application-insights-data"></a>Verbinding maken met uw Application Insights-gegevens
Analytics openen vanuit uw app [overzichtsblade](app-insights-dashboards.md) in Application Insights:

![Portal.azure.com Open, open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsiodocslanguage-referencetabular-operators-show-me-n-rows"></a>[Nemen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators): n rijen weergeven
Gegevenspunten die gebruikersbewerkingen (meestal HTTP-aanvragen ontvangen door uw web-app) in een logboek worden opgeslagen in een tabel met de naam `requests`. Elke rij is een gegevenspunt voor telemetrie ontvangen van de Application Insights-SDK in uw app.

Laten we beginnen door een paar voorbeeldrijen van de tabel:

![resultaten](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Plaats de cursor ergens in de instructie voordat u klikt op Start. U kunt een instructie splitsen op meer dan één regel, maar plaats geen lege regels in een instructie. Lege regels zijn een handige manier om te houden van meerdere afzonderlijke query's in het venster.
>
>

Kolommen kiezen, versleept u ze groeperen op kolommen, en te filteren:

![Klik op de kolom selecteren in de rechterbovenhoek van de resultaten](./media/app-insights-analytics-tour/030.png)

Vouw een item om te zien van de details:

![Kies tabel en gebruiken van kolommen configureren](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klik op de kop van een kolom die de resultaten die beschikbaar zijn in de webbrowser opnieuw te rangschikken. Maar houd er rekening mee dat voor een grote resultatenset, het aantal rijen dat is gedownload naar de browser beperkt wordt. Sorteren op deze manier hoeft de geretourneerde resultatenset sorteert en niet altijd u de werkelijke hoogste of laagste items weergeven. Als u wilt sorteren op betrouwbare wijze items, gebruikt u de `top` of `sort` operator.
>
>

## <a name="query-across-applications"></a>Query's uitvoeren voor toepassingen
Als u combineren van gegevens uit meerdere Application Insights-toepassingen wilt, gebruikt u de **app** trefwoord om op te geven van de toepassing, samen met de naam van de tabel.  Deze query worden gecombineerd voor het aanvragen van twee verschillende toepassingen met behulp van de **union** opdracht.


```AIQL

    union app('fabrikamstage').requests, app('fabrikamprod').requests
    
```

## <a name="tophttpsdocsloganalyticsiodocslanguage-referencetabular-operatorstop-operator-and-sorthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssort-operator"></a>[Top](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) en [sorteren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
`take` is handig om een snel voorbeeld van een resultaat, maar hier ziet u rijen uit de tabel in willekeurige volgorde. Als u een geordende weergeven, gebruikt `top` (voor een voorbeeld) of `sort` (over de hele tabel).

De eerste n rijen zijn geordend op een bepaalde kolom weergeven:

```AIQL

    requests | top 10 by timestamp desc
```

* *Syntaxis:* meeste operators hebben sleutelwoord parameters zoals `by`.
* `desc` aflopende volgorde = `asc` = oplopend.

![](./media/app-insights-analytics-tour/260.png)

`top...` is een meer goed presterende manier uitspraak `sort ... | take...`. We kunnen hebt geschreven:

```AIQL

    requests | sort by timestamp desc | take 10
```

Het resultaat zou zijn hetzelfde, maar het zou een en ander langzamer uitgevoerd. (U kunt ook schrijven `order`, dit is een alias van `sort`.)

De kolomkoppen in de tabelweergave kunnen ook worden gebruikt om de resultaten op het scherm te sorteren. Maar natuurlijk, als u bekend bent met `take` of `top` om op te halen, alleen deel uitmaken van een tabel, te klikken op de kolomkop wordt alleen opnieuw rangschikken de records die u hebt opgehaald.

## <a name="wherehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorswhere-operator-filtering-on-a-condition"></a>[Waar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator): filteren op een voorwaarde

We gaan nu kijken alleen aanvragen die een bepaalde resultaatcode geretourneerd:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

De `where` operator wordt een Booleaanse expressie. Hier volgen enkele belangrijke punten over deze:

* `and`, `or`: Booleaanse operators
* `==`, `<>`, `!=` : gelijk zijn en niet gelijk aan
* `=~`, `!~` : niet-hoofdlettergevoelige tekenreeks gelijk zijn aan en niet gelijk zijn. Er zijn veel meer tekenreeks vergelijkingsoperators.

<!---Read all about [scalar expressions]().--->

### <a name="find-unsuccessful-requests"></a>Mislukte aanvragen zoeken

Een string-waarde converteren naar een geheel getal en gebruik groter-dan vergelijking:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Time

Standaard zijn uw query's beperkt tot de laatste 24 uur. Maar u kunt dit bereik wijzigen:

![](./media/app-insights-analytics-tour/change-time-range.png)

Het tijdsbereik overschrijven door te schrijven van een query die wordt vermeld `timestamp` in een component where. Bijvoorbeeld:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

De functie voor het bereik is gelijk aan een 'where'-component die na elke vermelding van een van de brontabellen ingevoegd.

`ago(3d)` betekent 'drie dagen geleden'. Andere tijdseenheden omvatten uur (`2h`, `2.5h`), minuten (`25m`), en seconden (`10s`).

Andere voorbeelden:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

    // Between specific day/time range
    requests
    | where timestamp > datetime(2018-05-17T17:06:19.892Z) and timestamp <= datetime(2018-05-18T17:06:19.892Z)
    | where duration > 0

```

[Datums en tijden verwijzing](https://docs.loganalytics.io/docs/Language-Reference/Data-types/datetime).


## <a name="projecthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorsproject-operator-select-rename-and-compute-columns"></a>[Project](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator): selecteren en de namen van kolommen berekenen
Gebruik [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) om eruit te pikken alleen de kolommen die u wilt:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

U kunt ook namen van kolommen wijzigen en nieuwe definiëren:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![Resultaat](./media/app-insights-analytics-tour/270.png)

* De namen van kolommen mag spaties bevatten of symbolen als ze zijn tussen zoals deze: `['...']` of `["..."]`
* `%` is de gebruikelijke modulo operator.
* `1d` (die een cijfer, is een had') is een letterlijke timespan wat betekent dat één dag. Hier volgen enkele meer timespan letterlijke waarden: `12h`, `30m`, `10s`, `0.01s`.
* `floor` (alias `bin`) rondt af naar een waarde naar het dichtstbijzijnde meervoud van de basiswaarde die u opgeeft. Dus `floor(aTime, 1s)` rondt af naar een tijdstip omlaag naar de dichtstbijzijnde seconde.

Expressies kunnen opnemen de gebruikelijke operators (`+`, `-`,...), en er is een bereik van handige functies.

## <a name="extend"></a>Breid uit
Als u alleen kolommen toevoegen aan een bestaande wilt, gebruikt u [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Met behulp van [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator) is minder uitgebreid dan [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) als u wilt behouden de bestaande kolommen.

### <a name="convert-to-local-time"></a>Converteren naar de lokale tijd

Tijdstempels zijn altijd in UTC. Dus als u op de oostkust ons Pacific en winterspelen is, de lokale tijd-8 uren van UTC is, kan als volgt:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```

## <a name="summarizehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssummarize-operator-aggregate-groups-of-rows"></a>[Samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator): Rijgroepen samenvoegen
`Summarize` van toepassing is een opgegeven *statistische functie* via Rijgroepen.

Bijvoorbeeld, de tijd uw web-app nodig is om te reageren op een aanvraag wordt gerapporteerd in het veld `duration`. We gaan nu kijken de gemiddelde reactietijd voor alle aanvragen:

![](./media/app-insights-analytics-tour/410.png)

Of we het resultaat kan verdelen in aanvragen van verschillende namen:

![](./media/app-insights-analytics-tour/420.png)

`Summarize` verzamelt de gegevenspunten in de stroom in groepen waarvan de `by` component even evalueert. Elke waarde in de `by` - elke unieke bewerkings-naam in het bovenstaande voorbeeld - expressie resulteert in een rij in de resultaattabel staan.

Of er kan resultaten groeperen op tijd van de dag:

![](./media/app-insights-analytics-tour/430.png)

U ziet hoe gebruiken we de `bin` functie (ook wel `floor`). Als we net hebben gebruikt `by timestamp`, elke rij met invoer in een eigen groep weinig zou terechtkomen. Voor een continue scalaire als tijden of getallen, hebben we de continue reeks in een beheersbare aantal discrete waarden opsplitsen. `bin` -Dit is slechts de vertrouwde afronding omlaag `floor` functie - is de eenvoudigste manier om dat te doen.

We kunnen dezelfde techniek gebruiken voor het bereiken van tekenreeksen verminderen:

![](./media/app-insights-analytics-tour/440.png)

U ziet dat u kunt `name=` om in te stellen van de naam van een kolom met resultaten, in de statistische expressies of de component by.

## <a name="counting-sampled-data"></a>Voorbeeldgegevens tellen
`sum(itemCount)` is de aanbevolen aggregatie voor het tellen van gebeurtenissen. In veel gevallen itemCount == 1, zodat de functie gewoon het aantal rijen in de groep telt. Maar wanneer [steekproeven](app-insights-sampling.md) is in werking slechts een fractie van de oorspronkelijke gebeurtenissen worden bewaard als gegevenspunten in Application Insights, zodat voor elk gegevenspunt u ziet, zijn er `itemCount` gebeurtenissen.

Bijvoorbeeld, als steekproeven 75% van de oorspronkelijke gebeurtenissen en klik vervolgens op itemCount negeert == 4 in de bewaarde records - dat wil zeggen, voor elke record behouden, zijn er vier oorspronkelijke records.

Adaptieve steekproeven zorgt ervoor dat itemCount moet hoger zijn tijdens perioden wanneer uw toepassing veel wordt gebruikt.

ItemCount tellen daarom resulteert in een goede schatting van het oorspronkelijke aantal gebeurtenissen.

![](./media/app-insights-analytics-tour/510.png)

Er is ook een `count()` aggregatie (en een aantal bewerking), voor gevallen waarin u echt wilt voor het tellen van het aantal rijen in een groep.

Er is een bereik van [aggregatiefuncties](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions).

## <a name="charting-the-results"></a>De resultaten voor grafieken
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Standaard worden de resultaten weergegeven als een tabel:

![](./media/app-insights-analytics-tour/225.png)

We kunnen doen beter dan de tabelweergave. Bekijk de resultaten in de diagramweergave met de verticale balk optie:

![Klik op grafiek, vervolgens kies verticale staafdiagram en toewijzen x en y assen](./media/app-insights-analytics-tour/230.png)

U ziet dat hoewel we niet de resultaten sorteren op tijd (zoals u ziet in de tabel weer te geven), datum/tijd in de weergave diagram altijd worden weergegeven in de juiste volgorde.


## <a name="timecharts"></a>Timecharts
Weergeven hoeveel gebeurtenissen er elk uur zijn:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Selecteer de optie lijndiagram weergeven:

![tijdgrafiek](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Meerdere reeksen
Meerdere expressies in de `summarize` component maakt meerdere kolommen.

Meerdere expressies in de `by` component maakt meerdere rijen, één voor elke combinatie van waarden.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabel met aanvragen per uur en locatie](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Een grafiek Segmenteer op dimensies
Als u een tabel met een kolom met tekenreeksen en een numerieke kolom grafiek, kan de tekenreeks voor de numerieke gegevens splitsen in afzonderlijke reeks punten worden gebruikt. Als er meer dan één kolom met tekenreeksen, kunt u kiezen welke kolom moet worden gebruikt als de onderscheiding.

![Een grafiek met gebruiksanalyses segment](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Percentage geblokkeerd

Een Booleaanse waarde converteren naar een tekenreeks die moet worden gebruikt als een onderscheiding:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Meerdere metrische gegevens weergeven
Als u een tabel met meer dan een numerieke kolom, naast de tijdstempel van grafiek kunt u een combinatie hiervan kunt weergeven.

![Een grafiek met gebruiksanalyses segment](./media/app-insights-analytics-tour/110.png)

U moet selecteren **niet splitsen** voordat u kunt meerdere numerieke kolommen selecteren. U kunt niet splitsen door een kolom met tekenreeksen op hetzelfde moment als het weergeven van meer dan één van de numerieke kolommen.

## <a name="daily-average-cycle"></a>Dagelijkse gemiddelde cyclus
Hoe verschilt gebruik gedurende de gemiddelde dag?

Aantal aanvragen op dat moment modulo één dag voor binned in uren:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Lijndiagram van uren in een gemiddelde dag](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> U ziet dat we momenteel hebben tijdsduren converteren naar datum/tijd om op een lijndiagram weer te geven.
>
>

## <a name="compare-multiple-daily-series"></a>Meerdere dagelijkse reeksen vergelijken
Hoe gebruik verschilt na verloop van de tijd van de dag in andere landen?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Splitsen door client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Tekenen van een distributiepunt
Hoeveel sessies zijn er met verschillende lengtes?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

De laatste regel is moet worden geconverteerd naar datum/tijd vereist. Op dit moment wordt de x-as van een grafiek weergegeven als een scalaire alleen als het een datum/tijd.

De `where` component niet van toepassing op eindresultaat sessies (sessionDuration == 0) en stelt u de lengte van de x-as.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsiodocslanguage-referenceaggregation-functionspercentiles"></a>[Percentielen](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/percentiles())
Welke bereiken van de duur betrekking hebben op andere percentages van sessies?

Gebruik de bovenstaande query, maar vervang de laatste regel:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

We hebben ook de bovengrens verwijderd in de where-component, om op te halen van de juiste cijfers, met inbegrip van alle sessies met meer dan één verzoek:

![Resultaat](./media/app-insights-analytics-tour/180.png)

Waaruit zien we dat:

* 5% van de sessies hebben een duur van minder dan 3 minuten 34s;
* 50% van de sessies laatste minder dan 36 minuten;
* 5% van de sessies laatst meer dan 7 dagen

Als u een afzonderlijke specificatie voor elk land, we net hebben om Samenvatting de kolom client_CountryOrRegion afzonderlijk via beide operators:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Koppelen
We hebben toegang tot verschillende tabellen, inclusief aanvragen en uitzonderingen.

Als u de uitzonderingen die betrekking hebben op een aanvraag die is geretourneerd foutantwoord zoekt, kunnen we de tabellen samenvoegen op `operation_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Het is raadzaam om te gebruiken `project` selecteert alleen de kolommen die we nodig hebt voordat u de join.
In de dezelfde componenten wijzigen we de timestamp-kolom.

## <a name="lethttpsdocsloganalyticsiodocslanguage-referencequery-statementslet-statement-assign-a-result-to-a-variable"></a>[Laat](https://docs.loganalytics.io/docs/Language-Reference/Query-statements/Let-statement): een resultaat toewijzen aan een variabele

Gebruik `let` voor het scheiden van de onderdelen van de vorige expressie. De resultaten zijn niet gewijzigd:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> Plaats geen lege regels tussen de onderdelen van de query in de Analytics-client. Zorg ervoor dat u alles.
>

Gebruik `toscalar` één cel converteren naar een waarde:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Functions

Gebruik *laten* voor het definiëren van een functie:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>Toegang tot geneste objecten
Geneste objecten kunnen eenvoudig worden geopend. Bijvoorbeeld, in de stroom uitzonderingen ziet u gestructureerde objecten als volgt:

![Resultaat](./media/app-insights-analytics-tour/520.png)

U kunt deze afvlakken door het kiezen van de eigenschappen die u geïnteresseerd bent in:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Houd er rekening mee dat u wilt converteren van het resultaat dat het juiste type.


## <a name="custom-properties-and-measurements"></a>Aangepaste eigenschappen en metingen
Als uw toepassing [aangepaste dimensies (eigenschappen) en aangepaste metingen](app-insights-api-custom-events-metrics.md#properties) op gebeurtenissen die vervolgens worden deze weergegeven in de `customDimensions` en `customMeasurements` objecten.

Bijvoorbeeld, als uw app bevat:

```csharp

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2.d2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Deze waarden in Analytics extraheren:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type
```

Om te controleren of een aangepaste dimensie van een bepaald type is:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

### <a name="special-characters"></a>Speciale tekens

Voor id's met speciale tekens of trefwoorden voor de taal in hun namen, moet u toegang tot deze via `['` en `']` of met behulp van `["` en `"]`.

```AIQL

    customEvents
    | extend p2d2 = customDimensions.['p2.d2'], ...
```

[Naamgevingsregels voor ID-verwijzing](https://docs.loganalytics.io/docs/Learn/References/Naming-principles)

## <a name="dashboards"></a>Dashboards
Als u wilt samenbrengen van uw belangrijkste grafieken en tabellen kunt u de resultaten aan een dashboard vastmaken.

* [Azure-gedeelde dashboard](app-insights-dashboards.md#share-dashboards): klik op het pictogram vastmaken. Voordat u dit doet, moet u een gedeeld dashboard hebben. In de Azure-portal openen of maken van een dashboard en klik op delen.
* [Power BI-dashboard](app-insights-export-power-bi.md): klikt u op exporteren, Power BI Query. Een voordeel van dit alternatief is dat u uw query samen met andere resultaten uit een breed scala van bronnen kunt weergeven.

## <a name="combine-with-imported-data"></a>Combineer met geïmporteerde gegevens

Analyserapporten perfect op het dashboard, maar soms wilt u de gegevens aan een formulier hapklare te vertalen. Stel bijvoorbeeld dat uw geverifieerde gebruikers worden geïdentificeerd in de telemetrie door een alias. U wilt om hun echte namen in de resultaten weer te geven. Om dit te doen, moet u een CSV-bestand dat is toegewezen vanuit de aliassen aan de werkelijke namen.

U kunt een bestand importeren en gebruiken deze net als bij een van de standaard tabellen (aanvragen, uitzonderingen, enzovoort). U kunt deze query uit op een eigen of voeg deze toe met andere tabellen. Bijvoorbeeld, als u een tabel met de naam usermap hebt en er kolommen `realName` en `userId`, kunt u deze gebruiken om te vertalen de `user_AuthenticatedId` veld in de aanvraagtelemetrie:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Voor het importeren van een tabel in de blade Schema onder **andere gegevensbronnen**, volg de instructies voor het toevoegen van een nieuwe gegevensbron door een voorbeeld van uw gegevens worden geüpload. Vervolgens kunt u deze definitie voor het uploaden van tabellen.

De functie is momenteel in preview, wordt in eerste instantie ziet u een koppeling 'Contact met ons opnemen' onder "Andere gegevensbronnen." Gebruik deze optie voor het aanmelden bij de preview-programma en de koppeling wordt vervolgens vervangen door een knop 'Nieuwe gegevensbron toevoegen'.


## <a name="tables"></a>Tabellen
De stroom van telemetrie ontvangen van uw app is toegankelijk via verschillende tabellen. Het schema van de eigenschappen die beschikbaar zijn voor elke tabel is zichtbaar aan de linkerkant van het venster.

### <a name="requests-table"></a>Tabel ' Requests '
Aantal HTTP-aanvragen naar uw web-app en het segment met de paginanaam:

![Aantal aanvragen gesegmenteerd op naam](./media/app-insights-analytics-tour/analytics-count-requests.png)

De aanvragen die niet voldoen aan de meeste zoeken:

![Aantal aanvragen gesegmenteerd op naam](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Aangepaste gebeurtenissen-tabel
Als u [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) voor het verzenden van uw eigen gebeurtenissen, kunt u ze lezen uit deze tabel.

We nemen een voorbeeld waarin code van de app deze regels bevat:

```csharp

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

De frequentie van deze gebeurtenissen worden weergegeven:

![Het aantal aangepaste gebeurtenissen weergeven](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Pak metingen en dimensies van de gebeurtenissen:

![Het aantal aangepaste gebeurtenissen weergeven](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Aangepaste metrische gegevenstabel
Als u [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) voor het verzenden van uw eigen metrische waarden, vindt u de resultaten in de **customMetrics** stream. Bijvoorbeeld:  

![Aangepaste metrische gegevens in Application Insights analytics](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> In [Metrics Explorer](app-insights-metrics-explorer.md), alle aangepaste metingen die zijn gekoppeld aan elk type telemetrie samen voorkomen in de blade met metrische gegevens samen met metrische gegevens verzonden met behulp van `TrackMetric()`. Maar in Analytics, aangepaste metingen nog steeds zijn gekoppeld aan wat voor soort telemetrie ze werden uitgevoerd op - gebeurtenissen of aanvragen, enzovoort -terwijl metrische gegevens die zijn verzonden door TrackMetric worden weergegeven in hun eigen stream.
>
>

### <a name="performance-counters-table"></a>Prestaties tellers tabel
[Prestatiemeteritems](app-insights-performance-counters.md) laten u basissysteem metrische gegevens voor uw app, zoals CPU, geheugen, en netwerkgebruik. U kunt de SDK voor het verzenden van extra items, inclusief uw eigen aangepaste prestatiemeteritems configureren.

De **performanceCounters** schema wordt aangegeven dat de `category`, `counter` naam, en `instance` naam van elk prestatiemeteritem. Namen van prestatiemeteritems exemplaar zijn alleen van toepassing op bepaalde prestatiemeteritems en geven doorgaans aan de naam van het proces waarmee het aantal is gekoppeld. In de telemetrie voor elke toepassing ziet u alleen de items voor de toepassing. Bijvoorbeeld, als u wilt zien zijn welke items beschikbaar:

![Prestatiemeteritems in Application Insights analytics](./media/app-insights-analytics-tour/analytics-performance-counters.png)

Een grafiek van het beschikbare geheugen in de geselecteerde periode ophalen:

![Geheugen tijdgrafiek in Application Insights analytics](./media/app-insights-analytics-tour/analytics-available-memory.png)

Andere telemetrie, zoals **performanceCounters** heeft ook een kolom `cloud_RoleInstance` die aangeeft dat de identiteit van de hostcomputer waarop uw app wordt uitgevoerd. Als u bijvoorbeeld kunnen de prestaties van uw app op verschillende computers:

![Prestaties gesegmenteerd op rolinstantie in Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Uitzonderingentabel
[Uitzonderingen die zijn gerapporteerd door uw app](app-insights-asp-net-exceptions.md) zijn beschikbaar in deze tabel.

Als u zoekt de HTTP-aanvraag dat uw app is in behandeling wanneer de uitzondering is opgetreden, deelnemen aan op operation_Id:

![Neem deel aan uitzonderingen met aanvragen op operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Browser tijdsinstellingen tabel
`browserTimings` pagina laden gegevens die zijn verzameld in browsers van uw gebruikers bevat.

[Stelt u de app voor client-side telemetrie](app-insights-javascript.md) als u wilt deze metrische gegevens bekijken.

Het schema bevat [metrische gegevens die wijzen op de lengte van de verschillende stadia van de pagina tijdens het laden](app-insights-javascript.md#page-load-performance). (Ze niet duiden op de tijdsduur dat uw gebruikers een pagina lezen.)  

De popularities van verschillende pagina's weergeven en tijden voor elke pagina te laden:

![Laadtijden voor pagina's in Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Tabel met resultaten beschikbaarheid
`availabilityResults` toont de resultaten van uw [webtests](app-insights-monitor-web-app-availability.md). Elke uitvoering van uw tests vanaf elke testlocatie wordt afzonderlijk gerapporteerd.

![Laadtijden voor pagina's in Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Afhankelijkheden tabel
Resultaten van aanroepen dat uw app voor databases en REST-API's maakt en andere naar TrackDependency() aanroepen bevat. Daarnaast AJAX-aanroepen vanuit de browser.

AJAX-aanroepen vanuit de browser:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Afhankelijkheidsaanroepen van de server:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Serverzijde afhankelijkheid resultaten altijd weergeven `success==False` als de Application Insights-Agent niet is geïnstalleerd. Echter, de andere gegevens juist zijn.

### <a name="traces-table"></a>Traceringen tabel
Bevat de telemetrie die is verzonden door uw app met TrackTrace(), of [andere frameworks voor logboekregistratie](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Video 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Geavanceerde query's:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Volgende stappen
* [Naslaggids voor Analytics](app-insights-analytics-reference.md)
* [Overzichtskaart van SQL-gebruikers](https://aka.ms/sql-analytics) vertaalt de meest voorkomende idioms.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
