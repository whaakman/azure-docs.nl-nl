---
title: Typen query's en de samenstelling - Azure Search
description: Basisbeginselen voor het bouwen van een zoekopdracht in Azure Search met behulp van parameters om te filteren, selecteert en resultaten sorteren.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/25/2019
ms.custom: seodec2018
ms.openlocfilehash: 6afc9325cb2aababb286f653323531c9ebb04b4a
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438798"
---
# <a name="how-to-compose-a-query-in-azure-search"></a>Opstellen van een query in Azure Search

In Azure Search is een query een volledige specificatie van een traject bewerking. Parameters voor de aanvraag bieden overeenkomen met criteria voor het vinden van documenten in een index, uitvoering instructies voor de engine en richtlijnen voor het vormgeven van het antwoord. 

Een queryaanvraag is een uitgebreide constructie, op te geven welke velden zijn binnen de regeling vallen, hoe u kunt zoeken, welke velden u wilt terugkeren, of u wilt sorteren of filteren, enzovoort. Niets opgeeft, een query wordt uitgevoerd op alle doorzoekbare velden als een zoekbewerking volledige tekst, een in willekeurige volgorde zonder score resultatenset geretourneerd.

## <a name="apis-and-tools-for-testing"></a>API's en hulpprogramma's voor het testen

De volgende tabel bevat de API's en methoden op basis van een hulpprogramma voor het verzenden van query's.

| Methodologie | Description |
|-------------|-------------|
| [Search explorer (portal)](search-explorer.md) | Biedt een zoekbalk en opties voor de geselecteerde index en api-versie. Resultaten worden geretourneerd als JSON-documenten. <br/>[Meer informatie.](search-get-started-portal.md#query-index) | 
| [Postman of Fiddler](search-fiddler.md) | Web testhulpprogramma's vormen een uitstekende keuze voor het formuleren van REST-aanroepen. De REST-API biedt ondersteuning voor elke mogelijke bewerking in Azure Search. In dit artikel leert u hoe u het instellen van een HTTP-aanvraagheader en de hoofdtekst voor het verzenden van aanvragen naar Azure Search.  |
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | De client die kan worden gebruikt om op te vragen van een Azure Search-index.  <br/>[Meer informatie.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Documenten zoeken (REST-API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | GET of POST-methoden op een index, met behulp van de queryparameters voor aanvullende invoer.  |

## <a name="a-first-look-at-query-requests"></a>Een eerste blik op query-aanvragen

Voorbeelden zijn handig voor de introductie van nieuwe concepten. Als een representatieve query gemaakt in de [REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents), in dit voorbeeld van doelen de [onroerend goed demo index](search-get-started-portal.md) en algemene parameters bevat.

```
{
    "queryType": "simple" 
    "search": "seattle townhouse* +\"lake\"",
    "searchFields": "description, city",
    "count": "true",
    "select": "listingId, street, status, daysOnMarket, description",
    "top": "10",
    "orderby": "daysOnMarket"
}
```

+ **`queryType`** Hiermee stelt u de parser, die in Azure Search kan de [standaard de eenvoudige queryparser](search-query-simple-examples.md) (geoptimaliseerd voor het zoeken in volledige tekst), of de [volledige Lucene-queryparser](search-query-lucene-examples.md) gebruikt voor geavanceerde query-constructs, zoals reguliere expressies , zoeken op nabijheid, fuzzy en zoeken met jokertekens, een paar te noemen.

+ **`search`** biedt de overeenkomende criteria, meestal tekst, maar vaak vergezeld van Booleaanse operators. Termen van één zelfstandige *term* query's. Meerdelige query's tussen aanhalingstekens is geplaatst zijn *sleuteluitdrukkingen* query's. Zoeken is niet gedefinieerd, zoals in **`search=*`**, maar waarschijnlijk bestaat uit de termen en zinnen operators die vergelijkbaar is met wat wordt weergegeven in het voorbeeld.

+ **`searchFields`** is optioneel, gebruikt voor het beperken van queryuitvoering op bepaalde velden.

Antwoorden worden ook gevormd door de parameters die u in de query opnemen. In het voorbeeld wordt de resulterende set bestaat uit velden die worden vermeld in de **`select`** instructie. Alleen de bovenste 10 treffers in deze query zijn geretourneerd, maar **`count`** vertelt u hoeveel documenten overeenkomen met algemene. In deze query worden rijen gesorteerd op daysOnMarket.

In Azure Search wordt de queryuitvoering altijd op basis van een index, geverifieerd met behulp van een api-sleutel opgegeven in de aanvraag. In de REST, zijn beide opgegeven in de aanvraagheaders.

### <a name="how-to-run-this-query"></a>Hoe u deze query uit te voeren

Voor het uitvoeren van deze query gebruiken [explorer en de index van de demo onroerend goed zoeken](search-get-started-portal.md). 

U kunt deze querytekenreeks in de zoekbalk van de explorer plakken: `search=seattle townhouse +lake&searchFields=description, city&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket`

## <a name="how-query-operations-are-enabled-by-the-index"></a>Hoe querybewerkingen door de index zijn ingeschakeld

Ontwerp voor de index en query ontwerp nauw zijn gekoppeld in Azure Search. Een essentiële feit van tevoren weten is dat de *indexschema*, met de kenmerken voor elk veld, bepaalt het soort query die u kunt bouwen. 

Indexkenmerken op een veld de toegestane bewerkingen - instellen of een veld is *doorzoekbare* in de index *ophaalbaar* in resultaten *sorteerbaar*,  *Filterbaar*, enzovoort. In de querytekenreeks voorbeeld `"$orderby": "daysOnMarket"` werkt alleen omdat het veld daysOnMarket is gemarkeerd als *sorteerbaar* in het indexschema. 

![Index-definitie voor het voorbeeld onroerend goed](./media/search-query-overview/realestate-sample-index-definition.png "Index-definitie voor het voorbeeld onroerend goed")

De bovenstaande schermafbeelding is een gedeeltelijke lijst van de indexkenmerken in voor het voorbeeld onroerend goed. U kunt het schema voor de gehele index weergeven in de portal. Zie voor meer informatie over indexkenmerken [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index).

> [!Note]
> Sommige functies van de query is ingeschakeld index hele in plaats van op basis van per veld. Deze mogelijkheden zijn onder andere: [synoniem toegewezen](search-synonyms.md), [analysevoorzieningen](index-add-custom-analyzers.md), [suggestie constructies (voor automatisch aanvullen en Automatische suggestie)](index-add-suggesters.md), [scoring-logica voor resultaten trefwoordenrangschikking](index-add-scoring-profiles.md).

## <a name="elements-of-a-query-request"></a>Elementen van een queryaanvraag

Query's worden altijd gericht op één index. Lid worden van indexen kan of aangepaste of tijdelijke gegevensstructuren als het doel van een query maken. 

Vereiste elementen voor een queryaanvraag zijn onder andere de volgende onderdelen:

+ Service-eindpunt en index documenten verzameling, uitgedrukt als een URL naar vaste- en -onderdelen: **`https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`**
+ **`api-version`** (Alleen REST) is nodig omdat meer dan één versie van de API te allen tijde beschikbaar is. 
+ **`api-key`**, een query of de admin api-sleutel, verifieert de aanvraag voor uw service.
+ **`queryType`**, eenvoudig of volledig, die kan worden genegeerd als u van de ingebouwde standaard eenvoudige syntaxis gebruikmaakt.
+ **`search`** of **`filter`** biedt de overeenkomen met criteria die niet nader omschreven worden kunnen als u wilt uitvoeren van een lege zoekopdracht. Beide typen query's worden besproken in termen van de eenvoudige parser, maar ook geavanceerde query's moeten de search-parameter voor het doorgeven van complexe query-expressies.

Alle andere zoekparameters zijn optioneel. Zie voor de volledige lijst van kenmerken, [Index maken (REST)](https://docs.microsoft.com/rest/api/searchservice/create-index). Zie voor een nog eens kijken hoe parameters worden gebruikt tijdens de verwerking, [hoe zoeken in volledige tekst in Azure Search werkt](search-lucene-query-architecture.md).

## <a name="choose-a-parser-simple--full"></a>Kies een parser: eenvoudige | volledige

Azure Search bevindt zich boven op Apache Lucene en biedt u een keuze tussen de twee query-parsers voor het verwerken van typische en speciale query's. Aanvragen via de eenvoudige parser worden geformuleerd met behulp van de [vereenvoudigde querysyntaxis](query-simple-syntax.md), geselecteerd als de standaardwaarde voor de snelheid en efficiëntie in vrije indeling tekstquery's. Deze syntaxis ondersteunt een aantal algemene Zoekoperators, zoals de functies en, of niet, woordgroep, achtervoegsel en prioriteit operators.

De [volledige Lucene-querysyntaxis](query-Lucene-syntax.md#bkmk_syntax), ingeschakeld wanneer u toevoegen `queryType=full` wordt aangegeven dat de breed aanvaarde en expressieve querytaal die is ontwikkeld als onderdeel van de aanvraag [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Volledige syntaxis van de uitbreiding voor de eenvoudige syntaxis. Elke query die u voor de syntaxis van de eenvoudige schrijven wordt uitgevoerd onder de volledige Lucene-parser. 

De volgende voorbeelden ziet u het punt: dezelfde query, maar met verschillende queryType instellingen, verschillende resultaten opleveren. In de eerste query, de `^3` wordt beschouwd als onderdeel van de zoekterm.

```
queryType=simple&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

Dezelfde query met behulp van de volledige Lucene-parser interpreteert de boost veld in op 'ranch', die de positie zoeken van de resultaten met die specifieke term verhoogt.

```
queryType=full&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

<a name="types-of-queries"></a>

## <a name="types-of-queries"></a>Typen query's

Azure Search ondersteunt een breed scala aan typen query's. 

| Querytype | Gebruik | Meer informatie en voorbeelden |
|------------|--------|-------------------------------|
| Vrije indeling tekst zoeken | Search-parameter en een van beide parser| Zoeken in volledige tekst zoekt naar een of meer voorwaarden in alle *doorzoekbare* velden in uw index en werkt zoals u zou verwachten een zoekmachine als Google of Bing om te werken. Het voorbeeld in de inleiding is zoeken in volledige tekst.<br/><br/>Zoeken in volledige tekst ondergaat met behulp van de standaard Lucene analyzer (standaard) voor analyse van tekst naar kleine alle voorwaarden, stopwoorden verwijderen, zoals "het". U kunt de standaardinstelling met negeren [niet-Engelse analyzers](index-add-language-analyzers.md#language-analyzer-list) of [speciale taal-neutraal analyzers](index-add-custom-analyzers.md#AnalyzerTable) die analyse van tekst wijzigen. Een voorbeeld is [sleutelwoord](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) die de volledige inhoud van een veld wordt beschouwd als een enkele token. Dit is handig voor gegevens zoals postcodes, -id's en sommige productnamen. | 
| Gefilterde zoeken | [OData-filterexpressie](query-odata-filter-orderby-syntax.md) en een van beide parser | Filterquery's een Booleaanse expressie geëvalueerd over alle *Filterbaar* velden in een index. In tegenstelling tot search, een filterquery komt overeen met de exacte inhoud van een veld, met inbegrip van de hoofdlettergevoeligheid voor tekenreeksvelden. Een ander verschil is dat de filterquery's worden uitgedrukt in OData-syntaxis. <br/>[Voorbeeld van een expressie filteren](search-query-simple-examples.md#example-3-filter-queries) |
| Op geografische locaties zoeken | [Type Edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) op het veld, filterexpressie en de parser | Coördinaten die zijn opgeslagen in een veld met een Edm.GeographyPoint worden gebruikt voor de 'in mijn buurt zoeken' of op basis van een kaart zoekbesturingselementen. <br/>[Voorbeeld van de geografische locaties zoeken](search-query-simple-examples.md#example-5-geo-search)|
| Bereik zoeken | filterexpressie en eenvoudige parser | Bereik-query's zijn gebouwd met behulp van de filterparameter in Azure Search. <br/>[Voorbeeld van de Range-filter](search-query-simple-examples.md#example-4-range-filters) | 
| [Intra veld filteren](query-lucene-syntax.md#bkmk_fields) | Zoekparameter en volledige-parser | Bouw een samengestelde query-expressie die gericht is op één veld. <br/>[Voorbeeld van de verkeersstroom binnen veld filteren](search-query-lucene-examples.md#example-2-intra-field-filtering) |
| [fuzzy zoeken](query-lucene-syntax.md#bkmk_fuzzy) | Zoekparameter en volledige-parser | Komt overeen met op voorwaarden met een dergelijke constructie of spelling. <br/>[Een voorbeeld van fuzzy zoekopdracht](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [zoeken op nabijheid](query-lucene-syntax.md#bkmk_proximity) | Zoekparameter en volledige-parser | Hiermee zoekt u naar de voorwaarden die zich dicht bij elkaar in een document. <br/>[Voorbeeld van de zoekopdracht nabijheid](search-query-lucene-examples.md#example-4-proximity-search) |
| [versterking](query-lucene-syntax.md#bkmk_termboost) | Zoekparameter en volledige-parser | Als deze bevat de boosted term, ten opzichte van andere gebruikers die niet worden gerangschikt op een hoger document. <br/>[Term Gradient boosting-voorbeeld](search-query-lucene-examples.md#example-5-term-boosting) |
| [zoeken op reguliere expressies](query-lucene-syntax.md#bkmk_regex) | Zoekparameter en volledige-parser | Overeenkomsten op basis van de inhoud van een reguliere expressie. <br/>[Voorbeeld van een reguliere expressie](search-query-lucene-examples.md#example-6-regex) |
|  [jokertekens of voorvoegsel zoeken](query-lucene-syntax.md#bkmk_wildcard) | Zoekparameter en volledige-parser | Komt overeen met op basis van een voorvoegsel en een tilde (`~`) of één teken (`?`). <br/>[Een voorbeeld van jokertekens zoekopdracht](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="manage-search-results"></a>Beheren van zoekresultaten 

Queryresultaten worden gestreamd als JSON-documenten in de REST-API, maar als u .NET-API's, serialisatie is ingebouwd in. U kunt resultaten vormgeven parameters instelt op de query, specifieke velden voor het antwoord te selecteren.

Parameters voor de query kunnen worden gebruikt om te structureren van het resultaat ingesteld op de volgende manieren:

+ Beperken of batchverwerking van het aantal documenten in de resultaten (50 standaard)
+ Velden om op te nemen in de resultaten te selecteren
+ Instellen van een bepaalde volgorde gesorteerd
+ Toe te voegen hit markeringen om deze te vestigen op die overeenkomt met de voorwaarden in de hoofdtekst van de lijst met zoekresultaten

### <a name="tips-for-unexpected-results"></a>Tips voor het onverwachte resultaten

De stof en niet de structuur van de resultaten zijn soms onverwacht. Wanneer query-resultaten niet wat u verwacht te zien, kunt u proberen deze wijzigingen in de query om te controleren of de resultaten verbeteren:

+ Wijziging **`searchMode=any`** (standaard) **`searchMode=all`** om af te dwingen van overeenkomsten op alle criteria in plaats van een van de criteria. Dit is vooral van toepassing wanneer Booleaanse operators inbegrepen zijn de query.

+ De techniek van de query wijzigen als tekst of lexicale analyse nodig is, maar het querytype linguïstische verwerking uitsluit. Zoeken in volledige tekst, tekst of autocorrects voor spelfouten, singuliere meervoud word formulieren, en zelfs onregelmatige termen of zelfstandige naamwoorden lexicale analyse. Voor sommige query's zoals fuzzy of zoeken met jokertekens, tekstanalyse maakt geen deel uit van de pijplijn parseren van de query. Voor sommige scenario's, zijn reguliere expressies gebruikt als tijdelijke oplossing. 

### <a name="paging-results"></a>Resultaten pagineren
Met Azure Search kunt u gemakkelijk zoekresultaten oproepen. Met behulp van de **`top`** en **`skip`** parameters, u kunt eenvoudig zoekaanvragen uitgeven die kunt u voor het ontvangen van het totale aantal zoekresultaten in beheersbare, geordende van subsets die eenvoudig inschakelen goede zoekopdracht uit te voeren. Tijdens het ontvangen van deze kleinere subsets van resultaten kunt u ook het aantal documenten weergeven in het totale aantal zoekresultaten.

Zie het artikel [Zoekresultaten oproepen in Azure Search](search-pagination-page-layout.md) voor meer informatie over het oproepen van zoekresultaten.

### <a name="ordering-results"></a>Resultaten ordenen
De resultaten van een zoekopdracht kunnen door Azure Search worden geordend op de waarde in een bepaald veld. Standaard ordent Azure Search zoekresultaten op basis van de positie van de zoekscore van elk document, die is afgeleid van [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Als u wilt dat Azure Search om terug te keren uw resultaten gesorteerd op een andere waarde dan de zoekscore, kunt u de **`orderby`** zoekparameter. U kunt opgeven dat de waarde van de **`orderby`** parameter om op te nemen van de veldnamen en aanroepen naar de [  **`geo.distance()` functie** ](query-odata-filter-orderby-syntax.md) voor georuimtelijke waarden. Elke expressie kan worden gevolgd door `asc` om aan te geven dat de resultaten zijn aangevraagd in oplopende volgorde, en **`desc`** om aan te geven dat de resultaten in aflopende volgorde worden aangevraagd. De standaard oplopende volgorde.


### <a name="hit-highlighting"></a>Markeren
In Azure Search, waarbij de nadruk ligt het exacte aantal zoekresultaten die overeenkomen met de zoekopdracht is eenvoudig met behulp van de **`highlight`**, **`highlightPreTag`**, en **`highlightPostTag`** parameters. U kunt aangeven in welke *doorzoekbare* velden de tekst moet worden benadrukt. Ook kunt u de exacte tekenreekslabels opgeven die moeten worden toegevoegd aan het begin en einde van de overeenkomstige tekst die Azure Search retourneert.

## <a name="see-also"></a>Zie ook

+ [Hoe vol tekstzoekopdrachten werkt in Azure Search (parseren van de architectuur van de query)](search-lucene-query-architecture.md)
+ [Search explorer](search-explorer.md)
+ [Query's uitvoeren in .NET](search-query-dotnet.md)
+ [Query's uitvoeren in REST](search-create-index-rest-api.md)
