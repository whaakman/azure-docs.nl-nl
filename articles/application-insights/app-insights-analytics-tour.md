---
title: Een rondleiding via Analytics in Azure Application Insights | Microsoft Docs
description: Korte voorbeelden van alle belangrijke query's in Analytics, een krachtige zoekprogramma van Application Insights.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/06/2017
ms.author: mbullwin
ms.openlocfilehash: 271ccc126eeb9411646b68b32fd30ce32b5eef5c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Een rondleiding van Analytics in Application Insights
[Analytics](app-insights-analytics.md) is de functie krachtige zoeken van [Application Insights](app-insights-overview.md). Deze pagina's worden de Log Analytics query language beschreven.

* **[Bekijk de video inleidende](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Uitprobeert Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo)**  als uw app wordt niet van gegevens naar Application Insights nog verzenden.
* **[SQL-gebruikers cheats blad](https://aka.ms/sql-analytics)**  zet de meest voorkomende idioms.

We een stapsgewijze beschrijving van enkele eenvoudige query's om aan de slag te gaan.

## <a name="connect-to-your-application-insights-data"></a>Verbinding maken met uw Application Insights-gegevens
Analytics openen vanuit uw app [overzichtsblade](app-insights-dashboards.md) in Application Insights:

![Open portal.azure.com open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a>[Nemen](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): n rijen weergeven
Gegevenspunten die zich gebruiker operations (meestal HTTP-aanvragen ontvangen door uw web-app aanmelden) worden opgeslagen in een tabel met de naam `requests`. Elke rij is een telemetrie gegevenspunt ontvangen van de Application Insights-SDK in uw app.

Begin met het onderzoeken van een paar voorbeeld-rijen van de tabel:

![resultaten](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Plaats de cursor ergens in de instructie voordat u klikt op Start. U kunt een instructie verdelen over meer dan één regel, maar geen lege regels plaatsen in een instructie. Lege regels zijn een handige manier om te houden van meerdere afzonderlijke query's in het venster.
>
>

Kolommen kiezen, sleept u deze groeperen op kolommen, en te filteren:

![Klik op de kolom selecteren in de rechterbovenhoek van de resultaten](./media/app-insights-analytics-tour/030.png)

Vouw een item om de details:

![Kies tabel en kolommen configureren gebruiken](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klik op de koptekst van een kolom die de resultaten die beschikbaar zijn in de webbrowser opnieuw te rangschikken. Maar houd er rekening mee dat voor een grote resultatenset, het aantal rijen dat is gedownload naar de browser beperkt wordt. Sorteren op deze manier gewoon de geretourneerde resultatenset sorteert en komt niet altijd u de werkelijke hoogste of laagste items weergeven. U sorteert items betrouwbaar, gebruiken de `top` of `sort` operator.
>
>

## <a name="query-across-applications"></a>Query uitvoeren op alle toepassingen
Als u gegevens uit meerdere Application Insights-toepassingen wilt, gebruikt u de **app** trefwoord om op te geven van de toepassing samen met de naam van de tabel.  Deze query combineert de aanvragen van twee verschillende toepassingen met behulp van de **union** opdracht.


```AIQL

    union app('fabrikamstage').requests, app('fabrikamprod').requests
    
```

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a>[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) en [sorteren](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)
`take`is handig om een snel voorbeeld van een resultaat, maar er rijen uit de tabel wordt weergegeven in een willekeurige volgorde. Als u een geordende weergeven, gebruikt `top` (voor een voorbeeld) of `sort` (via de hele tabel).

De eerste n rijen, geordend op een bepaalde kolom weergeven:

```AIQL

    requests | top 10 by timestamp desc
```

* *Syntaxis:* hebben de meeste operators sleutelwoord parameters, zoals `by`.
* `desc`aflopende volgorde = `asc` = oplopend.

![](./media/app-insights-analytics-tour/260.png)

`top...`is een manier meer zodat de melding `sort ... | take...`. We kunnen schrijven:

```AIQL

    requests | sort by timestamp desc | take 10
```

Het resultaat zou zijn hetzelfde, maar deze zou iets langzamer uitgevoerd. (U kunt ook schrijven `order`, dit is een alias van `sort`.)

De kolomkoppen in de tabelweergave kunnen ook worden gebruikt om te sorteren van de resultaten op het scherm. Maar natuurlijk, als u hebt gebruikt `take` of `top` om op te halen, alleen deel uitmaken van een tabel, te klikken op de kolomkop wordt alleen opnieuw rangschikken de records die u hebt opgehaald.

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a>[Waar](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): voor het filteren van een voorwaarde

Laten we zien alleen aanvragen die een bepaalde resultaatcode geretourneerd:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

De `where` operator werkt met een Boole-expressie. Hier volgen enkele belangrijke punten hierover:

* `and`, `or`: Booleaanse operators
* `==`, `<>`, `!=` : gelijk en niet gelijk aan
* `=~`, `!~` : niet-hoofdlettergevoelige tekenreeks gelijk en niet gelijk zijn. Er zijn veel meer tekenreeks vergelijkingsoperators.

<!---Read all about [scalar expressions]().--->

### <a name="find-unsuccessful-requests"></a>Mislukte aanvragen zoeken

Een string-waarde converteren naar een geheel getal groter gebruiken-dan vergelijking:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Time

Uw query's zijn standaard beperkt de laatste 24 uur. Maar u kunt dit bereik wijzigen:

![](./media/app-insights-analytics-tour/change-time-range.png)

Het tijdsbereik overschrijven door een query noemt schrijven `timestamp` in een component where. Bijvoorbeeld:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

De functie voor het bereik is gelijk aan een component 'where' ingevoegd na elke vermelding van een van de brontabellen.

`ago(3d)`betekent 'drie dagen geleden'. Andere tijdseenheden uren bevatten (`2h`, `2.5h`), minuten (`25m`), en seconden (`10s`).

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

```

[Datums en tijden verwijzing](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a>[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): selecteren en de namen van kolommen berekenen
Gebruik [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) te pikken kolommen u wilt:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

U kunt ook wijzigen van kolommen en definiëren van nieuwe:

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

* Kolomnamen kunnen spaties bevatten of symbolen als ze zijn tussen zoals deze: `['...']` of`["..."]`
* `%`is de gebruikelijke modulo operator.
* `1d`(die wordt een cijfer, en vervolgens een had') is een letterlijke timespan wat betekent dat één dag. Hier volgen enkele meer timespan literals: `12h`, `30m`, `10s`, `0.01s`.
* `floor`(alias `bin`) Rondt een waarde naar beneden op het dichtstbijzijnde meervoud van de basiswaarde die u opgeeft. Dus `floor(aTime, 1s)` per keer naar beneden op het dichtstbijzijnde tweede afgerond.

Expressies kunnen bevatten de gebruikelijke operators (`+`, `-`,...), en er is een aantal handige functies.

## <a name="extend"></a>Breid uit
Als u alleen kolommen toevoegen aan een bestaande wilt, gebruikt u [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Met behulp van [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is minder uitgebreid dan [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) als u wilt behouden de bestaande kolommen.

### <a name="convert-to-local-time"></a>Converteren naar de lokale tijd

Tijdstempels worden altijd in UTC. Dus als u bijvoorbeeld op de westkust ons Pacific winter is, kunt u als volgt:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a>[Overzicht van](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): cumulatieve groepen rijen
`Summarize`van toepassing is een opgegeven *aggregatiefunctie* via Rijgroepen koppelen.

Bijvoorbeeld, de tijd die uw web-app nodig is om te reageren op een aanvraag wordt aangegeven in het veld `duration`. De gemiddelde reactietijd voor alle aanvragen laten we zien:

![](./media/app-insights-analytics-tour/410.png)

Of we het resultaat kan verdelen in aanvragen van verschillende namen:

![](./media/app-insights-analytics-tour/420.png)

`Summarize`verzamelt de gegevenspunten in de stroom in groepen waarvan de `by` component evenveel evalueert. Elke waarde in de `by` - elke unieke naam van de bewerking in het bovenstaande voorbeeld - expressie resulteert in een rij in de resultaattabel.

Of we kan resultaten groeperen op tijd van de dag:

![](./media/app-insights-analytics-tour/430.png)

U ziet hoe we maken gebruik van de `bin` functie (aka `floor`). Als we zojuist gebruikt `by timestamp`, elke rij invoer in een eigen weinig groep zou eindigen. Voor een continue scalaire als tijden of cijfers die we hebben continue reeks opdelen in een beheersbare aantal discrete waarden. `bin`-Dit is slechts de bekend afronden omlaag `floor` werken - is de eenvoudigste manier om u te doen.

We kunnen dezelfde techniek gebruiken om te bereiken met tekenreeksen verminderen:

![](./media/app-insights-analytics-tour/440.png)

Merk op dat u kunt `name=` instellen van de naam van een resultaatkolom in de statistische expressies of de component by.

## <a name="counting-sampled-data"></a>Voorbeeldgegevens tellen
`sum(itemCount)`is de aanbevolen aggregatie voor het tellen van gebeurtenissen. In veel gevallen itemCount == 1, zodat de functie gewoon het aantal rijen in de groep telt. Maar wanneer [steekproeven](app-insights-sampling.md) is uitgevoerd, alleen een fractie van de oorspronkelijke gebeurtenissen worden bewaard als gegevenspunten in Application Insights, zodat er zijn voor elk gegevenspunt u ziet, `itemCount` gebeurtenissen.

Bijvoorbeeld, als steekproeven 75% van de oorspronkelijke gebeurtenissen en vervolgens de itemCount negeert == 4 in de terugkerende records - dat wil zeggen, voor elke record behouden, zijn er vier oorspronkelijke records.

Adaptieve steekproeven zorgt ervoor dat itemCount moet hoger tijdens perioden wanneer uw toepassing veel wordt gebruikt.

Berekening van het itemCount daarom resulteert in een goede indicatie van het oorspronkelijke aantal gebeurtenissen.

![](./media/app-insights-analytics-tour/510.png)

Er is ook een `count()` aggregatie (en een aantal bewerking) voor gevallen waarin u echt wilt om het aantal rijen in een groep.

Er is een bereik van [aggregatiefuncties](https://docs.loganalytics.io/learn/tutorials/aggregations.html).

## <a name="charting-the-results"></a>De resultaten voor grafieken
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Standaard worden de resultaten weergegeven als een tabel:

![](./media/app-insights-analytics-tour/225.png)

We kunt beter dan de tabelweergave doen. Bekijk de resultaten in de diagramweergave met de verticale balk optie:

![Klik op grafiek, kiest u verticaal staafdiagram en toewijzen x en y assen](./media/app-insights-analytics-tour/230.png)

U ziet dat hoewel we niet sorteer de resultaten door tijd (zoals u ziet in de weergave van de tabel), datum/tijd de grafiekweergave altijd weergegeven in de juiste volgorde.


## <a name="timecharts"></a>Timecharts
Weergeven hoeveel gebeurtenissen er elk uur zijn:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Selecteer de optie van grafiek weergeven:

![timechart](./media/app-insights-analytics-tour/080.png)

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

### <a name="segment-a-chart-by-dimensions"></a>Een grafiek segmenteren door dimensies
Als u een tabel met een tekenreeks en een numerieke kolom grafiek, kan de tekenreeks voor numerieke gegevens splitsen in afzonderlijke reeks punten worden gebruikt. Als er meer dan een kolom met tekenreeksen, kunt u kiezen welke kolom moet worden gebruikt als de discriminator.

![Een grafiek met segmenteren](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Stuiteren snelheid

Een Booleaanse waarde niet converteren naar een tekenreeks als een discriminator te gebruiken:

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
Als u een tabel met meer dan een numerieke kolom naast de tijdstempel van grafiekgebied kunt u een combinatie van beide weergeven.

![Een grafiek met segmenteren](./media/app-insights-analytics-tour/110.png)

U moet selecteren **niet splitsen** voordat u kunt meerdere numerieke kolommen selecteren. U kunt niet splitsen door een kolom met tekenreeksen op hetzelfde moment als meer dan een numerieke kolom om weer te geven.

## <a name="daily-average-cycle"></a>Dagelijkse gemiddelde cyclus
Hoe informatie over het gebruik gedurende de dag van de gemiddelde variëren?

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
> U ziet dat momenteel we hebben tijdsduren converteren naar datum/tijd om in een lijndiagram weer te geven.
>
>

## <a name="compare-multiple-daily-series"></a>Meerdere dagelijkse reeksen vergelijken
Hoe informatie over het gebruik variëren via het tijdstip in andere landen?

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

![Gesplitste door client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Tekenen van een distributiepunt
Hoeveel sessies zijn er van verschillende lengtes?

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

De laatste regel is vereist voor het converteren naar datetime. Op dit moment wordt de x-as van een grafiek weergegeven als een scalaire alleen als het een datum/tijd.

De `where` component eindresultaat sessies worden uitgesloten (sessionDuration == 0) en stelt u de lengte van de x-as.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[Percentielen](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
Welke bereiken van duur hebben betrekking op verschillende percentages van sessies?

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

We hebben ook de bovengrens verwijderd in de where-component, om op te halen van de juiste cijfers inclusief alle sessies met meer dan één verzoek:

![Resultaat](./media/app-insights-analytics-tour/180.png)

Waaruit zien we dat:

* 5% van sessies hebben een duur van minder dan 3 minuten 34s;
* 50% van sessies laatste minder dan 36 minuten;
* 5% van sessies laatste meer dan 7 dagen

Als u een afzonderlijke verdeling voor elk land, we zojuist hebben om de kolom client_CountryOrRegion afzonderlijk via beide geven een overzicht operators:

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

Als u de uitzonderingen die betrekking hebben op een aanvraag die is geretourneerd foutantwoord zoekt, kunnen we de tabellen samenvoegen op `session_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Het is raadzaam om gebruik van `project` selecteren van kolommen moet voordat u de join uitvoert.
In de dezelfde kunnen we de naam van de timestamp-kolom.

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-to-a-variable"></a>[Laat](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): een resultaat toewijzen aan een variabele

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
> Plaats geen lege regels tussen de onderdelen van de query in de Analytics-client. Zorg ervoor dat u alle functies van it.
>

Gebruik `toscalar` één tabelcel converteren naar een waarde:

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


### <a name="functions"></a>Functies

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

## <a name="accessing-nested-objects"></a>Het openen van geneste objecten
Geneste objecten gemakkelijk toegankelijk. Bijvoorbeeld, in de stroom uitzonderingen ziet u gestructureerde objecten als volgt:

![Resultaat](./media/app-insights-analytics-tour/520.png)

U kunt deze afvlakken door het kiezen van de eigenschappen die u geïnteresseerd bent in:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Houd er rekening mee dat u wilt converteren van het resultaat dat het juiste type.


## <a name="custom-properties-and-measurements"></a>Aangepaste eigenschappen en metingen
Als uw toepassing koppelt [aangepaste dimensies (eigenschappen) en aangepaste metingen](app-insights-api-custom-events-metrics.md#properties) op gebeurtenissen die vervolgens worden deze weergegeven in de `customDimensions` en `customMeasurements` objecten.

Bijvoorbeeld, als uw app bevat:

```csharp

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Uitpakken van deze waarden in Analytics:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type

```

Controleren of een aangepaste dimensie van een bepaald type is:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Dashboards
U kunt uw resultaten aan een dashboard vastmaken om het samenbrengen van uw belangrijkste grafieken en tabellen.

* [Azure gedeelde dashboard](app-insights-dashboards.md#share-dashboards): klik op het pictogram pincode. Voordat u dit doet, moet u een gedeelde dashboard hebben. Open in de Azure-portal of een dashboard maken en klikt u op delen.
* [Power BI-dashboard](app-insights-export-power-bi.md): klik op exporteren, Power BI Query. Een voordeel van dit alternatief is dat u uw query samen met andere resultaten van een breed scala van gegevensbronnen kunt weergeven.

## <a name="combine-with-imported-data"></a>Gecombineerd met de geïmporteerde gegevens

Analytics-rapporten perfect op het dashboard, maar soms wilt u de gegevens aan een formulier hapklare te converteren. Stel bijvoorbeeld dat uw geverifieerde gebruikers worden geïdentificeerd in de telemetrie door een alias. U wilt weergeven van de namen van de echte in uw resultaten. Om dit te doen, moet u een CSV-bestand dat is toegewezen vanuit de aliassen aan de werkelijke namen.

U kunt een bestand importeren en gebruiken net als de standaardtabellen (aanvragen, uitzonderingen, enzovoort). U kunt deze query uitvoeren op een eigen of deel hieraan met andere tabellen. Bijvoorbeeld, als u een tabel met de naam usermap hebt en er kolommen `realName` en `userId`, kunt u deze gebruiken om te vertalen naar de `user_AuthenticatedId` veld in de aanvraagtelemetrie:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Voor het importeren van een tabel in de blade Schema onder **andere gegevensbronnen**, volg de instructies voor het toevoegen van een nieuwe gegevensbron door het uploaden van een steekproef van uw gegevens. Vervolgens kunt u deze definitie voor het uploaden van tabellen.

De functie is momenteel in preview, zodat u in eerste instantie ziet een koppeling 'Contact met ons opnemen' onder "Andere gegevensbronnen." Gebruik deze voor het aanmelden voor de preview-programma en de koppeling vervolgens worden vervangen door een knop 'Een nieuwe gegevensbron toevoegen'.


## <a name="tables"></a>Tabellen
De stroom van telemetrie van uw app ontvangt, is toegankelijk via verschillende tabellen. Het schema van de eigenschappen die beschikbaar zijn voor elke tabel is aan de linkerkant van het venster zichtbaar.

### <a name="requests-table"></a>Tabel aanvragen
Aantal HTTP-aanvragen op uw web-app en het segment met de paginanaam:

![Aantal aanvragen gesegmenteerd op naam](./media/app-insights-analytics-tour/analytics-count-requests.png)

De aanvragen die niet voldoen aan de meeste zoeken:

![Aantal aanvragen gesegmenteerd op naam](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Aangepaste gebeurtenissentabel
Als u [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) voor het verzenden van uw eigen gebeurtenissen kunt u ze lezen uit deze tabel.

We nemen een voorbeeld waarin uw app-code deze regels bevat:

```csharp

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

De frequentie van deze gebeurtenissen worden weergegeven:

![Aantal aangepaste gebeurtenissen weergeven](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Pak metingen en dimensies van de gebeurtenissen:

![Aantal aangepaste gebeurtenissen weergeven](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Tabel met aangepaste metrische gegevens
Als u [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) voor het verzenden van uw eigen metrische waarden vindt u de resultaten in de **customMetrics** stroom. Bijvoorbeeld:  

![Aangepaste metrische gegevens in Application Insights analytics](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> In [Metrics Explorer](app-insights-metrics-explorer.md), alle aangepaste metingen die zijn gekoppeld aan elk type telemetrie samen voorkomen in de blade metrische gegevens samen met metrische gegevens die zijn verzonden met behulp van `TrackMetric()`. Maar in Analytics, aangepaste metingen nog steeds gekoppeld aan wat voor soort telemetrie ze werden uitgevoerd op - gebeurtenissen of aanvragen, enzovoort -terwijl metrische gegevens die zijn verzonden door TrackMetric worden weergegeven in hun eigen stroom.
>
>

### <a name="performance-counters-table"></a>Prestaties tellers tabel
[Prestatiemeteritems](app-insights-performance-counters.md) u basissysteem metrische gegevens voor uw app, zoals CPU, geheugen, weergeven en netwerkgebruik. U kunt de SDK voor het verzenden van extra items, met inbegrip van uw eigen aangepaste tellers configureren.

De **performanceCounters** schema beschrijft de `category`, `counter` naam, en `instance` naam van elk prestatiemeteritem. Namen van exemplaren zijn alleen van toepassing op sommige prestatiemeters en geven doorgaans aan de naam van het proces waarmee het aantal is gekoppeld. In de telemetrie voor elke toepassing ziet u alleen de items voor die toepassing. Bijvoorbeeld, als u wilt zien zijn welke items beschikbaar:

![Prestatiemeters in Application Insights analytics](./media/app-insights-analytics-tour/analytics-performance-counters.png)

Een grafiek van het beschikbare geheugen voor de geselecteerde periode ophalen:

![Geheugen timechart in Application Insights analytics](./media/app-insights-analytics-tour/analytics-available-memory.png)

Zoals u andere telemetrie **performanceCounters** heeft ook een kolom `cloud_RoleInstance` die aangeeft dat de identiteit van de hostcomputer waarop uw app wordt uitgevoerd. Als u bijvoorbeeld wilt vergelijken de prestaties van uw app in de verschillende machines:

![Prestaties gesegmenteerd op rolinstantie in Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Uitzonderingentabel
[Uitzonderingen die zijn gerapporteerd door uw app](app-insights-asp-net-exceptions.md) zijn beschikbaar in deze tabel.

Als u wilt zoeken in de HTTP-aanvragen dat uw app is in behandeling wanneer de uitzondering is gegenereerd, aanmelden op operation_Id:

![Uitzonderingen met aanvragen operation_Id koppelen](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Browser tijdsinstellingen tabel
`browserTimings`toont de pagina laden gegevens verzameld in browsers van uw gebruikers.

[Instellen van uw app voor clientzijde telemetrie](app-insights-javascript.md) om te zien van deze metrische gegevens.

Het schema bevat [metrische gegevens die wijzen op de lengte van de verschillende stadia van de pagina laadproces](app-insights-javascript.md#page-load-performance). (Ze geven de tijdsduur die uw gebruikers een pagina lezen.)  

De popularities van verschillende pagina's weergeven en tijden voor elke pagina te laden:

![Laadtijden voor pagina's in Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Tabel met resultaten beschikbaarheid
`availabilityResults`toont de resultaten van uw [webtests](app-insights-monitor-web-app-availability.md). Elke uitvoering van uw tests vanaf elke testlocatie wordt afzonderlijk gerapporteerd.

![Laadtijden voor pagina's in Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Afhankelijkheden tabel
Resultaten van aanroepen dat uw app databases en REST-API's maakt en andere naar TrackDependency() aanroepen bevat. Tevens AJAX-aanroepen vanuit de browser.

AJAX-aanroepen vanuit de browser:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Het aanroepen van de afhankelijkheden van de server:

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
* [SQL-gebruikers cheats blad](https://aka.ms/sql-analytics) zet de meest voorkomende idioms.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
