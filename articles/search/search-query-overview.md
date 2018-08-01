---
title: Query uitvoeren op fundamentals in Azure Search | Microsoft Docs
description: Basisbeginselen voor het bouwen van een zoekopdracht in Azure Search met behulp van parameters om te filteren, selecteert en resultaten sorteren.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: 4650ad89850f32ae5e83a7ac1cd5eac096b180ed
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366443"
---
# <a name="query-fundamentals-in-azure-search"></a>Grondbeginselen van de query in Azure Search

De definitie van een query in Azure Search is een volledige specificatie van een aanvraag met de volgende onderdelen: service-URL-eindpunt, doelindex, api-sleutel die wordt gebruikt om de aanvraag, api-versie en een query-tekenreeks te verifiëren. 

Query-tekenreeks samenstelling bestaat uit parameters zoals gedefinieerd in [zoeken-API voor documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents). Belangrijkste daarvan is de **search =** parameter, die niet-gedefinieerde worden kan (`search=*`), maar het meer waarschijnlijk bestaat uit de termen en zinnen operators die vergelijkbaar is met het volgende voorbeeld:

```
"search": "seattle townhouse +\"lake\""
```

Andere parameters worden gebruikt voor directe verwerking van query's of breid het antwoord. Voorbeelden van hoe de parameters worden gebruikt zijn scoping zoeken naar specifieke velden, instellen van een modus voor het afstemmen van de afwijking van precisie-aan-/ oproepdiagram en het toevoegen van een aantal zodat u kunt bijhouden van resultaten. 

```
{  
    "search": "seattle townhouse* +\"lake\"",  
    "searchFields": "description, city",  
    "searchMode": "all",
    "count": "true", 
    "queryType": "simple" 
 } 
```

Hoewel de querydefinitie van de is van cruciaal belang, is uw indexschema net zo belangrijk in hoe het toegestane bewerkingen op basis van de door veld bevat. Tijdens het ontwikkelen van index bepalen de kenmerken in de velden Toegestane bewerkingen. Bijvoorbeeld, als u wilt komen voor zoeken in volledige tekst en de opname in de zoekresultaten, een veld moet worden gemarkeerd als beide *doorzoekbare* en *ophaalbaar*.

Moment van de query wordt uitgevoerd altijd op basis van een index, geverifieerd met behulp van een api-sleutel opgegeven in de aanvraag. Lid worden van indexen kan of aangepaste of tijdelijke gegevensstructuren als het doel van een query maken.  

Queryresultaten worden gestreamd als JSON-documenten in de REST-API, maar als u .NET-API's, serialisatie is ingebouwd in. U kunt resultaten vormgeven parameters instelt op de query, specifieke velden voor het resultaat selecteren

Om samen te vatten, geeft de realiteit van de queryaanvraag bereik en bewerkingen: welke velden u wilt opnemen in het zoekvak, dat velden die moeten worden opgenomen in het resultaat ingesteld, of u wilt sorteren of filteren, enzovoort. Niets opgeeft, een query wordt uitgevoerd op alle doorzoekbare velden als een zoekbewerking volledige tekst, een in willekeurige volgorde zonder score resultatenset geretourneerd.

<a name="types-of-queries"></a>

## <a name="types-of-queries-search-and-filter"></a>Typen query's: zoeken en filteren

Azure Search biedt verschillende opties om krachtige query's te maken. De twee belangrijkste querytypen die u zult gebruiken zijn `search` en `filter`. 

+ `search` query's scannen voor een of meer voorwaarden in alle *doorzoekbare* velden in uw index en werkt zoals u zou verwachten een zoekmachine als Google of Bing om te werken. De voorbeelden in de inleiding de `search` parameter.

+ `filter` query's een Booleaanse expressie geëvalueerd over alle *Filterbaar* velden in een index. In tegenstelling tot `search`, een `filter` -query overeenkomt met de exacte inhoud van een veld, met inbegrip van de hoofdlettergevoeligheid voor tekenreeksvelden.

U kunt zoeken en filteren samen of afzonderlijk. Een zelfstandige worden gefilterd, zonder een queryreeks is nuttig wanneer de filterexpressie in staat om volledig te kwalificeren documenten van belang is. Zonder een queryreeks is geen lexicale of linguïstische analyse, geen beoordeling en er is geen classificatie. U ziet dat de zoektekenreeks is leeg.

```
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "count": "true"
    }
```

Het filter eerst wordt toegepast op de gehele index samen worden gebruikt, en vervolgens de zoekopdracht wordt uitgevoerd op de resultaten van het filter. Filters zijn dus nuttig om de resultaten van de zoekopdracht te verbeteren, doordat het aantal documenten dat moet worden doorzocht, wordt verminderd.

De syntaxis voor filterexpressies is een subset van de [OData-filtertaal](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Voor zoekquery's kunt u de [vereenvoudigde syntaxis](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) of de [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) gebruiken. Deze worden hieronder behandeld.


## <a name="choose-a-syntax-simple-or-full"></a>Kies een syntaxis: eenvoudige of volledige

Azure Search bevindt zich boven op Apache Lucene en biedt u een keuze tussen de twee query-parsers voor het verwerken van typische en speciale query's. Standaardzoekopdrachten aanvragen worden geformuleerd met behulp van de standaard [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search). Deze syntaxis ondersteunt een aantal algemene Zoekoperators, zoals de functies en, of niet, woordgroep, achtervoegsel en prioriteit operators.

De [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), ingeschakeld wanneer u toevoegen **queryType = full** wordt aangegeven dat de algemene en expressieve querytaal die is ontwikkeld als onderdeel van de aanvraag [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Deze query-syntaxis gebruiken, kunt gespecialiseerde query's:

+ [Veldgerelateerde query 's](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields)
+ [fuzzy zoeken](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy)
+ [zoeken op nabijheid](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity)
+ [versterking](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost)
+ [zoeken op reguliere expressies](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex)
+ [zoeken met jokertekens](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard)

Booleaanse operators zijn grotendeels hetzelfde als in beide syntaxis, met extra volledige Lucene-indelingen:

+ [Booleaanse operators in eenvoudige syntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search#operators-in-simple-search)
+ [Booleaanse operators in de volledige Lucene-syntaxis](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean)

## <a name="required-and-optional-elements"></a>Vereiste en optionele elementen

Als u zoekaanvragen verzendt naar Azure Search, kunt u naast de werkelijke woorden die in het zoekvak van uw toepassing worden getypt een aantal parameters opgeven. Deze queryparameters bieden een betere controle over de [functie voor het zoeken in volledige tekst](search-lucene-query-architecture.md).

Vereiste elementen voor een queryaanvraag zijn onder andere de volgende onderdelen:

+ Service-eindpunt en de index documenten verzameling, uitgedrukt als een URL `https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`.
+ API-versie (alleen REST), uitgedrukt als **api-version =**
+ query of de admin api-sleutel, uitgedrukt als **api-sleutel =**
+ querytekenreeks uitgedrukt als **search =**, die kan worden niet nader omschreven als u wilt uitvoeren van een lege zoekopdracht. U kunt ook een filterexpressie als verzenden **$filter =**.
+ **queryType =**, eenvoudig of volledig, die kan worden genegeerd als u wilt gebruiken de standaard eenvoudige syntaxis.

Alle andere zoekparameters zijn optioneel.

## <a name="apis-and-tools-for-testing"></a>API's en hulpprogramma's voor het testen

De volgende tabel bevat de API's en methoden op basis van een hulpprogramma voor het verzenden van query's.

| Methodologie | Beschrijving |
|-------------|-------------|
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | De client die kan worden gebruikt om op te vragen van een Azure Search-index.  <br/>[Meer informatie.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Documenten zoeken (REST-API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | GET of POST-methoden op een index, met behulp van de queryparameters voor aanvullende invoer.  |
| [Fiddler of Postman andere hulpprogramma voor het testen van HTTP](search-fiddler.md) | Wordt uitgelegd hoe u een aanvraag koptekst en hoofdtekst voor het verzenden van query's naar Azure Search kunt instellen.  |
| [Search explorer in Azure portal](search-explorer.md) | Biedt een zoekbalk en opties voor de geselecteerde index en api-versie. Resultaten worden geretourneerd als JSON-documenten. <br/>[Meer informatie.](search-get-started-portal.md#query-index) | 

## <a name="manage-search-results"></a>Beheren van zoekresultaten

Parameters voor de query kunnen worden gebruikt om te structureren van het resultaat ingesteld op de volgende manieren:

+ Beperken of batchverwerking van het aantal documenten in de resultaten (50 standaard)
+ Velden om op te nemen in de resultaten te selecteren
+ Instellen van een bepaalde volgorde gesorteerd
+ Toe te voegen hit markeringen om deze te vestigen op die overeenkomt met de voorwaarden in de hoofdtekst van de lijst met zoekresultaten

### <a name="tips-for-unexpected-results"></a>Tips voor het onverwachte resultaten

De stof en niet de structuur van de resultaten zijn soms onverwacht. Wanneer query-resultaten niet wat u verwacht te zien, kunt u proberen deze wijzigingen in de query om te controleren of de resultaten verbeteren:

+ Wijziging `searchMode=any` (standaard) `searchMode=all` om af te dwingen van overeenkomsten op alle criteria in plaats van een van de criteria. Dit is vooral van toepassing wanneer Booleaanse operators inbegrepen zijn de query.

+ De techniek van de query wijzigen als tekst of lexicale analyse nodig is, maar het querytype linguïstische verwerking uitsluit. Zoekopdracht in volledige tekst, tekst of lexicale analyse automatisch gecorrigeerd voor spelfouten, singuliere meervoud word formulieren, en zelfs onregelmatige termen of woorden. Voor sommige query's zoals fuzzy of zoeken met jokertekens, tekstanalyse maakt geen deel uit van de pijplijn parseren van de query. Voor sommige scenario's, zijn reguliere expressies gebruikt als tijdelijke oplossing. 

### <a name="paging-results"></a>Resultaten pagineren
Met Azure Search kunt u gemakkelijk zoekresultaten oproepen. Met behulp van de `top`- en `skip`-parameter kunt u eenvoudig zoekaanvragen uitgeven waarmee u alle zoekresultaten in een beheersbare, geordende subset kunt weergeven om een goede zoekopdracht uit te voeren. Tijdens het ontvangen van deze kleinere subsets van resultaten kunt u ook het aantal documenten weergeven in het totale aantal zoekresultaten.

Zie het artikel [Zoekresultaten oproepen in Azure Search](search-pagination-page-layout.md) voor meer informatie over het oproepen van zoekresultaten.

### <a name="ordering-results"></a>Resultaten ordenen
De resultaten van een zoekopdracht kunnen door Azure Search worden geordend op de waarde in een bepaald veld. Standaard ordent Azure Search zoekresultaten op basis van de positie van de zoekscore van elk document, die is afgeleid van [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Als u de zoekresultaten op een andere waarde wilt ordenen, kunt u de `orderby`-zoekparameter gebruiken. U kunt opgeven dat in de waarde van de `orderby`-parameter de veldnamen en aanroepen naar de [`geo.distance()`-functie](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) voor georuimtelijke waarden worden opgenomen. Elke expressie kan worden gevolgd door `asc` om aan te geven dat de resultaten zijn aangevraagd in oplopende volgorde of door `desc` om aan te geven dat de resultaten in aflopende volgorde worden aangevraagd. De standaard oplopende volgorde.


### <a name="hit-highlighting"></a>Markeren
U kunt in Azure Search eenvoudig het exacte aantal zoekresultaten weergeven met behulp van de `highlight`-, `highlightPreTag`- en `highlightPostTag`-parameters. U kunt aangeven in welke *doorzoekbare* velden de tekst moet worden benadrukt. Ook kunt u de exacte tekenreekslabels opgeven die moeten worden toegevoegd aan het begin en einde van de overeenkomstige tekst die Azure Search retourneert.

## <a name="see-also"></a>Zie ook

+ [Hoe vol tekstzoekopdrachten werkt in Azure Search (parseren van de query architectuur](search-lucene-query-architecture.md)
+ [Search explorer](search-explorer.md)
+ [Query's uitvoeren in .NET](search-query-dotnet.md)
+ [Query's uitvoeren in REST](search-query-rest-api.md)
