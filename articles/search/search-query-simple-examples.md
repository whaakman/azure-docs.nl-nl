---
title: Eenvoudige query-voorbeelden - Azure Search
description: Voorbeelden van eenvoudige query voor zoeken in volledige tekst, filter, zoeken, geo zoeken, facetzoekopdrachten en andere queryreeksen gebruikt om te vragen van een Azure Search-index.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/09/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 9697b88e23fea0cb06ab0c4a6197b5255e7076bf
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316264"
---
# <a name="simple-syntax-query-examples-for-building-queries-in-azure-search"></a>Voorbeelden van eenvoudige syntaxis voor het bouwen van query's in Azure Search

[Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) roept de queryparser standaard voor het uitvoeren van volledige-tekstquery's op basis van een Azure Search-index. De eenvoudige query analyzer is snel en veelvoorkomende scenario's in Azure Search, met inbegrip van zoeken in volledige tekst, gefilterd en meervoudige zoeken en geografische locaties zoeken verwerkt. In dit artikel wordt stapsgewijs door voorbeelden demonstreren querybewerkingen beschikbaar wanneer u de eenvoudige syntaxis.

De alternatieve query-syntaxis is [volledige Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), zoals ondersteuning van meer complexe query-structuren, fuzzy en zoeken met jokertekens, wat extra tijd voor het verwerken van kan duren. Zie voor meer informatie en voorbeelden demonstreren van de volledige syntaxis [voorbeelden van Lucene-querysyntaxis](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Aanvragen in Postman formuleren

De volgende voorbeelden gebruikmaken van een NYC-Jobs search-index die bestaat uit functies die beschikbaar zijn op basis van een gegevensset die is geleverd door de [stad New York OpenData](https://nycopendata.socrata.com/) initiatief. Deze gegevens moet niet worden beschouwd als huidige of voltooid. De index is een sandboxservice van Microsoft, wat betekent dat u hoeft niet een Azure-abonnement of een Azure Search om te proberen deze query's.

Wat u moet is Postman of een gelijkwaardig hulpprogramma voor het uitgeven van HTTP-aanvraag voor ontvangen. Zie voor meer informatie, [verkennen met REST-clients](search-fiddler.md).

### <a name="set-the-request-header"></a>De aanvraagheader instellen

1. Stel in de aanvraagheader **Content-Type** naar `application/json`.

2. Voeg een **api-sleutel**, en stel deze in op deze tekenreeks: `252044BE3886FE4A8E3BAA4F595114BB`. Dit is de querysleutel van een voor de sandbox-search-service die als host fungeert voor de index NYC-Jobs.

Nadat u de aanvraagheader opgeeft, u deze opnieuw kunt gebruiken voor alle van de query's in dit artikel wordt alleen simpelweg de **search =** tekenreeks. 

  ![De aanvraagheader voor Postman](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>De aanvraag-URL instellen

De aanvraag is een GET-opdracht die is gekoppeld aan een URL met de tekenreeks die Azure Search-eindpunt en zoeken.

  ![De aanvraagheader voor Postman](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

Samenstelling van de URL heeft de volgende elementen:

+ **`https://azs-playground.search.windows.net/`** een sandbox-zoekservice is onderhouden door het ontwikkelingsteam Azure Search. 
+ **`indexes/nycjobs/`** is de index NYC-Jobs in de verzameling indexen van de betreffende service. De servicenaam en de index zijn vereist voor de aanvraag.
+ **`docs`** de verzameling documenten is die alle doorzoekbare inhoud bevat. De query api-sleutel opgegeven in de aanvraagheader werkt alleen op leesbewerkingen die gericht is op de verzameling documenten.
+ **`api-version=2017-11-11`** Hiermee stelt u de api-versie, is een vereiste parameter bij elke aanvraag.
+ **`search=*`** de querytekenreeks, die in de eerste query null is is, de eerste 50 resultaten retourneren (standaard).

## <a name="send-your-first-query"></a>Uw eerste query verzenden

Als verificatiestap, plak de volgende aanvraag ophalen en klikt u op **verzenden**. Resultaten worden geretourneerd als uitgebreide JSON-documenten. U kunt kopiëren en plakken deze URL in het eerste voorbeeld hieronder.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=*
  ```

De queryreeks **`search=*`**, is een niet-opgegeven zoekopdracht gelijk is aan null of leeg zijn zoeken. Het is niet met name nuttig, maar is de eenvoudigste zoeken die u kunt doen.

U kunt desgewenst toevoegen **`$count=true`** naar de URL om een telling van de documenten die voldoen aan de zoekcriteria te retourneren. Op een lege zoekopdracht tekenreeks is is dat alle documenten in de index (ongeveer 2800 in het geval van NYC-Jobs).

## <a name="how-to-invoke-simple-query-parsing"></a>Het parseren van eenvoudige query aanroepen

Voor interactieve query's, u hebt geen op te geven: eenvoudige is de standaardinstelling. In de code, als u eerder aangeroepen **queryType = full** voor volledige query-syntaxis, kan u de standaard opnieuw worden ingesteld **queryType = eenvoudige**.

## <a name="example-1-field-scoped-query"></a>Voorbeeld 1: Veldgerelateerde query

Het eerste voorbeeld is niet-parser-specifieke, maar we ermee leiden om te introduceren de eerste query fundamentele concept: containment. In dit voorbeeld scopes uitvoeren van query's en de reactie op een paar specifieke velden. Weten hoe u een leesbare JSON-antwoord is belangrijk bij het hulpprogramma Postman of Search explorer is. 

Beknopt alternatief, gericht op de query alleen de *business_title* veld en geeft u alleen zakelijke titels worden geretourneerd. De syntaxis is **searchFields** om te beperken van de uitvoering van de query alleen het veld business_title en **Selecteer** om op te geven welke velden zijn opgenomen in het antwoord.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

Antwoord voor deze query moet eruitzien als op de volgende schermafbeelding.

  ![Het voorbeeldantwoord postman](media/search-query-lucene-examples/postman-sample-results.png)

U hebt mogelijk de zoekscore in het antwoord opgemerkt. Uniform scores van 1 optreden wanneer er is geen positie, ofwel omdat de zoekopdracht is niet zoeken in volledige tekst, of omdat er geen criteria is toegepast. Voor null search zonder criteria keert u rijen terug in willekeurige volgorde. Wanneer u de werkelijke criteria opgeeft, ziet u scores in zinvolle waarden ontwikkelen zoeken.

## <a name="example-2-look-up-by-id"></a>Voorbeeld 2: Zoeken op ID

In dit voorbeeld is een en ander ongewone, maar bij het evalueren van zoekgedrag, kunt u de volledige inhoud van een bepaald document om te begrijpen waarom deze is opgenomen of uitgesloten van de resultaten controleren. Als één document in zijn geheel toe, gebruikt u een [opzoekfunctie](https://docs.microsoft.com/rest/api/searchservice/lookup-document) om door te geven in de document-ID.

Alle documenten hebben een unieke id. Als u wilt uitproberen de syntaxis voor een lookup-query, eerst retourneert een lijst met document-id's zodat u er een vinden kunt om te gebruiken. Voor NYC-Jobs, de id's worden opgeslagen in de `id` veld.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=id&$select=id&search=*
 ```

Het volgende voorbeeld wordt een lookup-query retourneert een bepaald document op basis van `id` "9E1E3AF9-0660-4E00-AF51-9B654925A2D5", bovenaan in het vorige antwoord weergegeven. De volgende query retourneert het gehele document, niet alleen de geselecteerde velden. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2017-11-11&$count=true&search=*
 ```

## <a name="example-3-filter-queries"></a>Voorbeeld 3: Filter-query 's

[Filtersyntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples) is een OData-expressie die u met gebruiken kunt **zoeken** of door zelf. Een zelfstandige worden gefilterd, zonder een search-parameter is nuttig wanneer de filterexpressie in staat om volledig te kwalificeren documenten van belang is. Zonder een queryreeks geen lexicale of linguïstische analyse, er is geen scoren van (alle scores zijn 1), en er is geen classificatie. U ziet dat de zoektekenreeks is leeg.

```http
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "select=job_id, business_title, agency, salary_range_from",
      "count": "true"
    }
```

Het filter eerst wordt toegepast op de gehele index samen worden gebruikt, en vervolgens de zoekopdracht wordt uitgevoerd op de resultaten van het filter. Filters zijn dus nuttig om de resultaten van de zoekopdracht te verbeteren, doordat het aantal documenten dat moet worden doorzocht, wordt verminderd.

  ![Filter-query-antwoord](media/search-query-simple-examples/filtered-query.png)

Als u dit proberen in Postman met behulp van GET wilt, kunt u in deze reeks plakken:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,business_title,agency,salary_range_from&search=&$filter=salary_frequency eq 'Annual' and salary_range_from gt 90000
 ```

Een andere krachtige manier om te filteren en zoeken combineren, is via **`search.ismatch*()`** in een filterexpressie, waarbij u gebruik van een zoekopdracht in het filter maken kan. Dit filterexpressie wordt een jokerteken op *plan* selecteren business_title met inbegrip van de termijn plan, planner, planning, enzovoort.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,business_title,agency&search=&$filter=search.ismatch('plan*', 'business_title', 'full', 'any')
 ```

Zie voor meer informatie over de functie [search.ismatch in "Filter voorbeelden"](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples).

## <a name="example-4-range-filters"></a>Voorbeeld 4: Bereikfilters

Filteren op datumbereik is alleen beschikbaar via **`$filter`** expressies voor elk gegevenstype. De volgende voorbeelden zoeken over een tekenreeks en numerieke velden. 

Gegevenstypen zijn belangrijk in bereikfilters en werken het beste als numerieke gegevens in de numerieke velden en tekenreeksgegevens in tekenreeksvelden. Numerieke gegevens in de tekenreeksvelden is niet geschikt voor bereiken omdat numerieke tekenreeksen niet vergelijkbaar in Azure Search zijn. 

De volgende voorbeelden zijn in de POST-indeling voor de leesbaarheid (numeriek bereik, gevolgd door tekstbereik):

```http
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency",
      "count": "true"
    }
```
  ![Bereikfilter voor numerieke bereiken](media/search-query-simple-examples/rangefilternumeric.png)


```http
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title",
      "count": "true"
    }
```

  ![Bereikfilter voor tekstbereiken](media/search-query-simple-examples/rangefiltertext.png)

U kunt deze ook proberen om in Postman met behulp van GET:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=&$filter=num_of_positions ge 5 and num_of_positions lt 10&$select=job_id, business_title, num_of_positions, agency&$orderby=agency&$count=true
```

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=&$filter=business_title ge 'A*' and business_title lt 'C*'&$select=job_id, business_title, agency&$orderby=business_title&$count=true
 ```

> [!NOTE]
> Onderverdeling via bereiken met waarden is een algemene vereiste voor search-toepassing. Zie voor meer informatie en voorbeelden voor het bouwen van filters voor facet navigatiestructuur ["Filter op basis van een bereik' in *facetnavigatie implementeren*](search-faceted-navigation.md#filter-based-on-a-range).

## <a name="example-5-geo-search"></a>Voorbeeld 5: Op geografische locaties zoeken

De voorbeeldindex bevat een veld geo_location met de breedtegraad en lengtegraad coördinaten. In dit voorbeeld wordt de [geo.distance functie](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples) die filters op documenten in de omtrek van een beginpunt uit op een willekeurige afstand (in kilometer) die u opgeeft. De laatste waarde in de query (4) naar het gebied van de query vergroten of verkleinen, kunt u aanpassen.

Het volgende voorbeeld is in de POST-indeling voor de leesbaarheid:

```http
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "job_id, business_title, work_location",
      "count": "true"
    }
```
Zoekresultaten zijn voor een beter leesbare resultaten bijgesneden om op te nemen van een taak-id, de functie en de werklocatie. De eerste coördinaten zijn verkregen vanuit een willekeurige document in de index (in dit geval voor een werklocatie op Staten eiland.

U kunt dit ook proberen om in Postman met behulp van GET:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=&$select=job_id, business_title, work_location&$filter=geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4
```

## <a name="example-6-search-precision"></a>Voorbeeld 6: De precisie zoeken

Term query's zijn enkelvoudige termen, mogelijk veel van deze, die onafhankelijk van elkaar worden geëvalueerd. Woordgroep query's tussen aanhalingstekens en geëvalueerd als een verbatim-tekenreeks. De nauwkeurigheid van de overeenkomst wordt bepaald door de operators en searchMode.

Voorbeeld 1: **`&search=fire`** 150 resultaten geretourneerd waarbij alle komt overeen met de brand word ergens in het document bevatten.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire
```

Voorbeeld 2: **`&search=fire department`** 2002 resultaten oplevert. Resultaten geretourneerd voor documenten die worden gestart of afdeling.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire department
```

Voorbeeld 3: **`&search="fire department"`** 82 resultaten oplevert. De tekenreeks insluitende aanhalingstekens is een verbatim zoeken op beide termen, en overeenkomsten worden gevonden op tokens voorwaarden in de index die bestaat uit de gecombineerde voorwaarden. Dit verklaart waarom een zoekopdracht, zoals **`search=+fire +department`** komt niet overeen. Beide termen zijn vereist, maar onafhankelijk van elkaar worden gescand op. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search="fire department"
```

## <a name="example-7-booleans-with-searchmode"></a>Voorbeeld 7: Booleaanse waarden met searchMode

Eenvoudige syntaxis ondersteunt Booleaanse operators in de vorm van tekens (`+, -, |`). De parameter searchMode informeert balans tussen de precisie en zoals eerder vermeld, met `searchMode=any` voorkeur intrekken (die overeenkomen met in elk criterium in aanmerking komt een document voor de resultatenset), en `searchMode=all` voorkeur precisie (alle criteria moeten worden vergeleken). De standaardwaarde is `searchMode=any`, die kunnen worden als u een query worden samengesteld met meerdere operators verwarrend en bredere in plaats van smaller resultaten ophalen. Dit geldt met name met niet, waarbij het resultaat van alle documenten "niet met" bevatten een bepaalde periode.

Met behulp van de standaard searchMode die (willekeurig), 2800 documenten worden geretourneerd: met de meerdelige termijn 'afdeling worden gestart', plus alle documenten waarvoor geen de term 'Metrotech Center'.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```

  ![modus elk zoeken](media/search-query-simple-examples/searchmodeany.png)

Wijzigen van searchMode naar `all` dwingt een cumulatieve effect van criteria en retourneert een kleiner resultatenset - 21 documenten - bestaande van documenten die de hele woordgroep "fire-afdeling, min de deze taken op het adres Metrotech Center bevatten.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```
  ![alle zoekmodus](media/search-query-simple-examples/searchmodeall.png)

## <a name="example-8-structuring-results"></a>Voorbeeld 8: Structureren van resultaten

Aantal parameters bepalen welke velden worden in het zoekvak resulteert, het aantal geretourneerde documenten in elke batch, en de sorteervolgorde. In dit voorbeeld resurfaces enkele van de eerdere voorbeelden, beperken van resultaten op bepaalde velden met behulp van de **$select** instructie en verbatim zoekcriteria 82 overeenkomsten retourneren 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Toegevoegd op het vorige voorbeeld, kunt u sorteren op de titel. Deze sorteren werkt omdat civil_service_title *sorteerbaar* in de index.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Resultaten pagineren is geïmplementeerd met behulp van de **$top** parameter, de top 5-documenten in dit geval retourneren:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Overslaan als u de volgende 5, de eerste batch:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Volgende stappen
Probeer de query's in uw code op te geven. De volgende koppelingen wordt uitgelegd hoe het instellen van zoekquery's voor .NET en de REST-API met behulp van de standaard eenvoudige syntaxis.

* [Query uitvoeren op uw Azure Search-Index met de .NET SDK](search-query-dotnet.md)
* [Query uitvoeren op uw Azure Search-Index met behulp van de REST-API](search-query-rest-api.md)

Naslaginformatie over aanvullende syntaxis voor query-architectuur en voorbeelden vindt u in de volgende koppelingen:

+ [Voorbeelden van Lucene-querysyntaxis voor het bouwen van geavanceerde query 's](search-query-lucene-examples.md)
+ [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)
+ [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Volledige Lucene-query](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Syntaxis voor filteren en sorteren op](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)
