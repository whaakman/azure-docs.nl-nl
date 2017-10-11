---
title: Azure Search Service REST API-versie Preview 2015-02-28-| Microsoft Docs
description: Azure Search Service REST API-versie 2015-02-28-Preview bevat experimentele functies zoals natuurlijke Taalanalyse en moreLikeThis zoekopdrachten.
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Search Service REST-API: Versie 2015-02-28-Preview
Dit artikel is de documentatie bij `api-version=2015-02-28-Preview`. Deze preview breidt de huidige versie van de algemeen beschikbaar [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), door de volgende experimentele functies:

* `moreLikeThis`queryparameter in de [documenten zoeken](#SearchDocs) API. Het zoeken naar andere documenten die relevant voor een ander specifieke document zijn.

Een paar extra onderdelen van de `2015-02-28-Preview` REST-API afzonderlijk worden beschreven. Deze omvatten:

* [Score berekenen voor profielen](search-api-scoring-profiles-2015-02-28-preview.md)
* [Indexeerfuncties](search-api-indexers-2015-02-28-preview.md)

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie.

## <a name="apis-in-this-document"></a>API's in dit document
Azure-API van zoekservice ondersteunt twee URL-syntaxis voor de API-bewerkingen: eenvoudige en OData (Zie [ondersteuning voor OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) voor meer informatie). De volgende lijst ziet u de eenvoudige syntaxis.

[Index maken](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index bijwerken](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index ophalen](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Aanbieding-indexen](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Indexstatistieken opvragen](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Test Analyzer](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Een Index verwijderen](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Toevoegen, verwijderen, en gegevens in een Index bijwerken](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Documenten zoeken](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Lookup-Document](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Aantal documenten](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Suggesties](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Indexbewerkingen
U kunt maken en beheren van de indexen in Azure Search-service via een eenvoudige HTTP-aanvragen (POST, GET, PUT, DELETE) op basis van een resource voor de gegeven index. Als u wilt een index maken, moet u eerst een JSON-document met een beschrijving van het indexschema boeken. De velden van de index, de gegevenstypen en hoe ze kunnen worden gebruikt (bijvoorbeeld in zoekopdrachten in volledige tekst, filters, sorteren of facetten) worden gedefinieerd in het schema. Het definieert ook scoreprofiel profielen, suggestiefunctie en andere kenmerken voor het configureren van het gedrag van de index.

Het volgende voorbeeld bevat een afbeelding van een schema dat wordt gebruikt voor zoekopdrachten op gegevens van hotel met het beschrijvingsveld gedefinieerd in twee talen. U ziet hoe kenmerken bepalen hoe het veld wordt gebruikt. Bijvoorbeeld de `hotelId` wordt gebruikt als de documentsleutel (`"key": true`) en wordt uitgesloten van zoekopdrachten in volledige tekst (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nadat de index is gemaakt, kunt u documenten die de index te vullen gaat uploaden. Zie [toevoegen of Update documenten](#AddOrUpdateDocuments) voor deze stap.

Zie voor een video-Inleiding tot in Azure Search indexeren, de [aflevering Channel 9 Cloud hebben betrekking op Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Index maken
Een index is de primaire methode voor het ordenen en documenten zoeken in Azure Search, vergelijkbaar met hoe records in een database in een tabel worden geordend. Elke index heeft een verzameling van documenten dat alle conform het indexschema (veldnamen, gegevenstypen en eigenschappen), maar indexen ook aanvullende constructies (suggestiefunctie, score-profielen en opties voor CORS) waarmee andere zoekgedrag opgeven.

U kunt een nieuwe index maken in Azure Search-service met behulp van een HTTP POST of PUT-aanvraag. De hoofdtekst van de aanvraag is een JSON-schema dat de index en configuratie-informatie bevat.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook gebruik van opslag en geef de indexnaam op de URI. Als de index niet bestaat, wordt deze gemaakt.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Maken van een index bepaalt de structuur van de documenten opgeslagen en gebruikt in zoekopdrachten. Vullen van de index is een afzonderlijke bewerking. Voor deze stap kunt u een [indexeerfunctie](https://msdn.microsoft.com/library/azure/mt183328.aspx) (beschikbaar voor ondersteunde gegevensbronnen) of een [toevoegen, bijwerken of verwijderen van documenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) bewerking. De omgekeerde index wordt gegenereerd wanneer de documenten worden geplaatst.

**Opmerking**: het maximum aantal indexen toegestaan hangt af van de prijscategorie. De gratis service kunnen maximaal 3 indexen. Standard-service kan 50-indexen per zoekservice. Zie [limieten en beperkingen](http://msdn.microsoft.com/library/azure/dn798934.aspx) voor meer informatie.

**Aanvraag**

HTTPS is vereist voor alle aanvragen van de service. De **Create Index** aanvraag kan worden geconstrueerd met een methode POST of PUT. Wanneer u POST, moet u de naam van een index in de aanvraagtekst samen met de indexdefinitie schema opgeven. Met opslag is de naam van de index onderdeel van de URL. Als de index niet bestaat, wordt deze gemaakt. Als deze al bestaat, wordt deze bijgewerkt naar de nieuwe definitie.

De naam van de index moet in kleine letters worden, beginnen met een letter of cijfer, hebben geen slashes of punten en minder dan 128 tekens bevatten. De rest van de naam kan na het starten van de naam van de index met een letter of cijfer bevatten een letter, cijfer en streepjes, zolang de streepjes niet opeenvolgende zijn.

De `api-version` is vereist. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor een lijst met beschikbare versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `Content-Type`: Vereist. Stel dit in op`application/json`
* `api-key`: Vereist. De `api-key` wordt gebruikt voor
* de aanvraag om uw Search-service te verifiëren. Het is een tekenreekswaarde die uniek is voor uw service. De **Create Index** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

<a name="RequestData"></a>
**Syntaxis van de aanvraag hoofdtekst**

De hoofdtekst van de aanvraag bevat een schemadefinitie waarin de lijst met gegevensvelden binnen de documenten die zal worden opgenomen in deze index, gegevenstypen, kenmerken, evenals een optionele lijst met profielen die worden gebruikt voor het beoordelen van overeenkomende documenten op het moment dat de query score berekenen .

Houd er rekening mee dat een POST-aanvraag, moet u de naam van de index in de aanvraagtekst.

Er kan alleen worden één sleutelveld in de index. Er moet een tekenreeksveld. Dit veld wordt de unieke identificatie aangegeven voor elk document dat is opgeslagen in de index.

De belangrijkste onderdelen van een index omvatten het volgende:

* `name`
* `fields`die zal worden opgenomen in deze index, zoals naam, gegevenstype en eigenschappen die de toegestane acties op dat veld definiëren.
* `suggesters`gebruikt voor automatisch aanvullen of automatisch aangevulde query's.
* `scoringProfiles`gebruikt voor aangepaste zoekactie score positie. Zie [scoreprofiel profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` gebruikt om te definiëren hoe uw documenten/query's worden onderverdeeld in worden geïndexeerd/doorzoekbare-tokens. Zie [analyse in Azure Search](https://aka.ms//azsanalysis) voor meer informatie.
* `defaultScoringProfile`gebruikt voor het overschrijven van de standaard score berekenen gedrag.
* `corsOptions`cross-origin-query's op uw index toestaat.

De syntaxis voor het structureren nettolading van de aanvraag is als volgt. Een voorbeeld van een aanvraag wordt aangeboden op verder in dit onderwerp.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexkenmerken**

De volgende kenmerken kunnen worden ingesteld bij het maken van een index. Zie voor meer informatie over score berekenen en score berekenen voor profielen [toevoegen score berekenen voor profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-De naam van het veld ingesteld.

`type`-Hiermee stelt u het gegevenstype voor het veld.

`searchable`-Markeert het veld als volledige tekst kunnen zoeken. Dit betekent dat deze ietwat analysis zoals woordafbreking tijdens het indexeren. Als u een `searchable` veld een waarde zoals 'mooi day', intern dit verdeeld in de afzonderlijke tokens 'mooi' en 'dag'. Hierdoor kan de volledige tekst zoekt naar deze voorwaarden. Velden van het type `Edm.String` of `Collection(Edm.String)` zijn `searchable` standaard. Mag geen velden van andere typen `searchable`.

* **Opmerking**: `searchable` velden gebruiken extra ruimte in de index omdat Azure Search, een extra tokens versie van de waarde van het veld voor zoekopdrachten in volledige tekst wordt opgeslagen. Als u wilt opslaan ruimte in de index en hoeft u niet een veld in zoekopdrachten wilt opnemen, stelt `searchable` naar `false`.

`filterable`-Kan het veld verwezen in `$filter` query's. `filterable`verschilt van `searchable` in de verwerking van tekenreeksen. Velden van het type `Edm.String` of `Collection(Edm.String)` die zijn `filterable` niet worden bewerkt woordafbreking, zodat vergelijkingen zijn voor exact overeenkomt met alleen. Bijvoorbeeld, als u dergelijke veld instellen `f` om 'mooi day' `$filter=f eq 'sunny'` wordt geen overeenkomsten gevonden maar `$filter=f eq 'sunny day'` wordt. Alle velden zijn `filterable` standaard.

`sortable`-Het systeem worden standaard resultaten gesorteerd op score, maar veel ervaringen gebruikers wilt sorteren op velden in de documenten. Velden van het type `Collection(Edm.String)` kan niet worden `sortable`. Alle andere velden zijn `sortable` standaard.

`facetable`-Doorgaans gebruikt in een weergave van zoekresultaten met treffers per categorie (bijvoorbeeld zoeken naar digitale camera's en Zie treffers op merk, door megapixels, door de prijs, enz.). Deze optie kan niet worden gebruikt met velden van het type `Edm.GeographyPoint`. Alle andere velden zijn `facetable` standaard.

* **Opmerking**: velden van het type `Edm.String` die zijn `filterable`, `sortable`, of `facetable` kan niet groter zijn dan 32 KB lang. Dit is omdat deze velden worden behandeld als een enkel zoekterm en de maximale lengte van een term in Azure Search 32KB is. Als u meer tekst dan deze opgeslagen in een enkel tekenreeksveld wilt, moet u expliciet instellen `filterable`, `sortable`, en `facetable` naar `false` in de indexdefinitie van de.
* **Opmerking**: als een veld geen van de bovenstaande kenmerken die zijn ingesteld heeft op `true` (`searchable`, `filterable`, `sortable`, of`facetable`) het veld effectief is uitgesloten van de omgekeerde index. Deze optie is nuttig voor velden die niet worden gebruikt in query's, maar die nodig zijn in de zoekresultaten. Met uitzondering van dergelijke velden uit de index verbetert de prestaties.

`key`-Markeert het veld bevat de unieke id's voor documenten in de index. Moet precies één veld worden gekozen als de `key` veld en moet zijn van het type `Edm.String`. Sleutelvelden kunnen worden gebruikt voor het opzoeken van documenten rechtstreeks via de [Lookup API](#LookupAPI).

`retrievable`-Instellen of het veld in een zoekresultaat kan worden geretourneerd.  Dit is handig als u een veld (bijvoorbeeld marge) gebruiken als een filter, wilt sorteren of score berekenen mechanisme maar niet dat het veld wilt zichtbaar is voor de eindgebruiker. Dit kenmerk moet `true` voor `key` velden.

`analyzer`-De naam van de analyzer te gebruiken voor het veld op zoektijd en indexering tijd instellen. Zie voor de toegestane set waarden [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Deze optie kan alleen worden gebruikt met `searchable` velden en deze kunnen niet worden ingesteld met een `searchAnalyzer` of `indexAnalyzer`.  Zodra de analyzer is gekozen, kan deze niet meer wijzigen voor het veld.

`searchAnalyzer`-De naam van de analyzer gebruikt op het moment van de zoekactie voor het veld instellen. Zie voor de toegestane set waarden [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Deze optie kan alleen worden gebruikt met `searchable` velden. Deze moet worden ingesteld samen met `indexAnalyzer` en deze kan niet worden ingesteld samen met de `analyzer` optie. Deze analyzer kan worden bijgewerkt op een bestaand veld.

`indexAnalyzer`-Hiermee stelt u de naam van de analysefunctie voor indexering tijd voor het veld wordt gebruikt. Zie voor de toegestane set waarden [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Deze optie kan alleen worden gebruikt met `searchable` velden. Deze moet worden ingesteld samen met `searchAnalyzer` en deze kan niet worden ingesteld samen met de `analyzer` optie. Zodra de analyzer is gekozen, kan deze niet meer wijzigen voor het veld.

`suggesters`-De modus en de velden die de bron van de inhoud voor suggesties zijn ingesteld. Zie [Suggestiefunctie](#Suggesters) voor meer informatie.

`scoringProfiles`-Definieert aangepast scoreprofiel gedrag waarmee u het van invloed zijn op welke objecten hoger in de zoekresultaten weergegeven. Scoreprofiel profielen zijn opgebouwd veldgewichten en functies. Zie [toevoegen score berekenen voor profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie over de kenmerken in een scoreprofiel gebruikt.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Taalondersteuning**

De doorzoekbare velden ondergaan analyse die het beste omvat vaak woordafbreking, tekst normalisatie en voorwaarden worden uitgefilterd. Standaard doorzoekbare velden in Azure Search worden geanalyseerd met de [Apache Lucene standaard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) die tekst opgesplitst in elementen na de['Segmentering Unicode-tekst'](http://unicode.org/reports/tr29/) regels. De standaard analyzer converteert bovendien alle tekens naar hun formulier kleine letters. Zowel geïndexeerde documenten en zoektermen doorlopen die de analyse tijdens het indexeren en de verwerking van query's.

Azure Search biedt ondersteuning voor verschillende talen. Elke taal is vereist voor een niet-standaard tekst analyzer die accounts voor kenmerken van een bepaalde taal. Azure Search biedt twee typen analyzers:

* 35 analyzers Lucene back-up.
* 50 analyzers bedrijfseigen Microsoft natuurlijke taal verwerken van de technologie die wordt gebruikt in Office en Bing back-up.

Sommige ontwikkelaars liever de meer vertrouwde, eenvoudig en open-source Lucene-oplossing. Lucene analyzers sneller zijn, maar de analyzers Microsoft hebben geavanceerde mogelijkheden, zoals Lemmata, word decompounding (in talen zoals Duits, Deens, Nederlands, Zweeds, Noors, Ests, voltooien, Hongaars, Slowaaks) en entiteit erkenning (URL 's e-mailberichten, datums, getallen). Indien mogelijk moet u vergelijkingen van de Microsoft- en Lucene analyzers om te bepalen welke beter geschikt is uitgevoerd.

***Hoe ze zich verhouden***

De analyzer Lucene voor Engels breidt de standaard analyzer. Deze bezittelijke voornaamwoorden (afsluitende van) met woorden verwijdert, is van toepassing als gevolg conform [Porter afleiding algoritme](http://tartarus.org/~martin/PorterStemmer/), en verwijdert u Engels [stopwoorden](http://en.wikipedia.org/wiki/Stop_words).

Ter vergelijking voert Microsoft analyzer Lemmata in plaats van de gegevens als gevolg. Dit betekent dat deze kan omgaan met verbogen en onregelmatige word formulieren veel beter wat resulteert in meer relevante zoekresultaten (controle module 7 van [Azure Search MVA presentatie](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) voor meer informatie).

Indexering met Microsoft analyzers is gemiddeld twee tot drie keer langzamer dan de Lucene-equivalenten, afhankelijk van de taal. Prestaties van de zoekopdracht moet niet aanzienlijk beïnvloed voor de gemiddelde grootte query's.

***Configuratie***

U kunt voor elk veld in de indexdefinitie instellen de `analyzer` eigenschap in op de naam van een analyzer die welke taal en de leverancier aangeeft. De dezelfde analyzer worden toegepast wanneer het indexeren en het zoeken voor dat veld.
U kunt bijvoorbeeld afzonderlijke velden voor Engels, Frans en Spaans hotel beschrijvingen die bestaan naast elkaar op dezelfde positie hebben. Gebruik de ['searchFields' queryparameter](#SearchQueryParameters) om op te geven welk veld specifieke taal zijn gebonden om te zoeken op basis van uw query's. U kunt bekijken query voorbeelden die zijn de `analyzer` eigenschap in [documenten zoeken](#SearchDocs). 

***Lijst met Analyzer***

Hieronder volgt de lijst van ondersteunde talen samen met de namen van Lucene en Microsoft analyzer.

<table style="font-size:12">
    <tr>
        <th>Taal</th>
        <th>De naam van de Microsoft-analyzer</th>
        <th>Lucene analyzer naam</th>
    </tr>
    <tr>
        <td>Arabisch</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>Armeens</td>
        <td></td>
        <td>Hy.lucene</td>
      </tr>
    <tr>
        <td>Bengaals</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>Baskisch</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>Bulgaars</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>Catalaans</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>Vereenvoudigd Chinees</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>Traditioneel Chinees</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>Kroatisch</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tsjechisch</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>        
    </tr>    
    <tr>
        <td>Deens</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>Nederlands</td>
        <td>nl.Microsoft</td>
        <td>nl.lucene</td>    
    </tr>    
    <tr>
        <td>Nederlands</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>Estisch</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Fins</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>Frans</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>        
    </tr>
    <tr>
        <td>Galicisch</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>Duits</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>Grieks</td>
        <td>El.Microsoft</td>
        <td>El.lucene</td>        
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebreeuws</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>Hongaars</td>        
        <td>Hu.Microsoft</td>
        <td>Hu.lucene</td>
    </tr>
    <tr>
        <td>IJslands</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesisch (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>        
    </tr>
    <tr>
        <td>Iers</td>
        <td></td>
          <td>Ga.lucene</td>
    </tr>
    <tr>
        <td>Italiaans</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>        
    </tr>
    <tr>
        <td>Japans</td>
        <td>Ja.Microsoft</td>
        <td>Ja.lucene</td>

    </tr>
    <tr>
        <td>Kannada</td>
        <td>Ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreaans</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lets</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>Litouws</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maleis (Latijns)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Noors</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>Perzisch</td>
        <td></td>
        <td>FA.lucene</td>        
      </tr>
    <tr>
        <td>Pools</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>        
    </tr>
    <tr>
        <td>Portugees (Brazilië)</td>
        <td>pt Br.microsoft</td>
        <td>pt Br.lucene</td>        
    </tr>
    <tr>
        <td>Portugees (Portugal)</td>
        <td>pt Pt.microsoft</td>        
        <td>pt Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Roemeens</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Russisch</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>Servisch (Cyrillisch)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Servisch (Latijns)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowaaks</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Sloveens</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spaans</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Zweeds</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thais</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turks</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>        
    </tr>
    <tr>
        <td>Oekraïens</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamees</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Bovendien biedt Azure Search taal networkdirect analyzer configuraties</td>
    <tr>
        <td>Standaard ASCII Folding</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode-tekst segmentering (standaard Tokenizer)</li>
            <li>Opvouwbaar filter ASCII - converteert Unicode-tekens die geen deel uitmaken van de set eerst 127 ASCII-tekens naar hun ASCII-equivalenten. Dit is handig voor het verwijderen van diakritische tekens.</li>
        </ul>
        </td>
    </tr>
</table>

Alle analyzers met namen van aantekeningen voorzien met <i>lucene</i> worden van stroom voorzien door [van Apache Lucene taalanalyse](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Meer informatie over de ASCII vouwen filter vindt [hier](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesties**

Een `suggester` wordt gedefinieerd welke velden in een index worden gebruikt voor de ondersteuning voor automatisch aanvullen in zoekopdrachten. Doorgaans gedeeltelijke zoekreeksen worden verzonden naar de [suggesties API](#Suggestions) terwijl de gebruiker een zoekopdracht typen is en de API een set met voorgestelde zinnen retourneert. Een suggestie die u in de index definieert bepaalt welke velden worden gebruikt om de type-ahead zoektermen samen te stellen. Zie [Suggestiefunctie](#Suggesters) voor configuratie-informatie.

**Scoreprofielen**

Een `scoringProfile` definieert aangepast scoreprofiel gedrag waarmee u het van invloed zijn op welke objecten hoger in de zoekresultaten weergegeven. Scoreprofiel profielen zijn opgebouwd veldgewichten en functies. Als u wilt gebruiken, geeft u een profiel met de naam van de queryreeks.

Een standaard score berekenen profiel werkt achter de schermen om de score van een zoekopdracht voor elk item in een resultatenset te berekenen. U kunt de interne naamloze scoreprofiel gebruiken. U kunt ook instellen `defaultScoringProfile` aangeroepen een aangepast profiel gebruiken als de standaard, wanneer u een aangepast profiel niet is opgegeven in de queryreeks.

Zie [scoreprofiel profielen toevoegen aan een zoekindex (Azure Search Service REST-API)](search-api-scoring-profiles-2015-02-28-preview.md) voor meer informatie.

**CORS-opties**

Client-side Javascript kan API's standaard niet aanroepen omdat de browser voorkomen alle cross-origin-aanvragen dat wordt. Inschakelen van CORS (Cross-Origin Resource Sharing) door in te stellen de `corsOptions` kenmerk waarmee cross-origin-query's naar uw index. Houd er rekening mee dat alleen query API-ondersteuning CORS uit veiligheidsoverwegingen. De volgende opties kunnen worden ingesteld voor CORS:

* `allowedOrigins`(vereist): dit is een lijst met oorsprongen op waarvoor toegang tot uw index wordt verleend. Dit betekent dat eventuele Javascript-code opgehaald uit deze oorsprongen mag worden query uitvoeren in uw index (ervan uitgaande dat de juiste API-sleutel biedt). Elke oorsprong heeft meestal de vorm `protocol://fully-qualified-domain-name:port` Hoewel de poort vaak wordt weggelaten. Zie [in dit artikel](http://go.microsoft.com/fwlink/?LinkId=330822) voor meer informatie.
  * Als u toestaan toegang tot alle oorsprongen wilt, opnemen `*` als één item in de `allowedOrigins` matrix. Houd er rekening mee dat **dit wordt niet aanbevolen beveiligingsprocedure voor productie search-services.** Dit kan echter nuttig zijn voor ontwikkeling of foutopsporing zijn.
* `maxAgeInSeconds`(optioneel): Browsers gebruiken deze waarde voor de duur (in seconden) te voorbereidende CORS-antwoorden cache. Dit moet een niet-negatief geheel getal zijn. Hoe hoger deze waarde is, de prestaties beter, maar hoe langer het duurt CORS wijzigingen voor beleid van kracht te laten worden. Als deze niet is ingesteld, wordt een standaardduur van 5 minuten gebruikt.

<a name="CreateUpdateIndexExample"></a>
**Voorbeeld van de aanvraag hoofdtekst**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Antwoord**

Aanvraag is gelukt: '201 gemaakt'.

De antwoordtekst bevatten standaard de JSON voor de definitie van de index die is gemaakt. Als de `Prefer` aanvraagheader is ingesteld op `return=minimal`, de antwoordtekst leeg en worden de statuscode geslaagd zijn ' 204 geen inhoud ' in plaats van '201 gemaakt'. Dit geldt ook als plaatsen of POST is gebruikt om de index te maken.

**Opmerkingen**

Op dit moment is er beperkte ondersteuning voor index schema-updates. Alle schema-updates die opnieuw worden geïndexeerd moeten zoals het wijzigen van veldtypen worden momenteel niet ondersteund. Hoewel u bestaande velden kunnen niet worden gewijzigd of verwijderd, worden nieuwe velden kunnen worden toegevoegd aan een bestaande index op elk gewenst moment. Wanneer u een nieuw veld toevoegt, hebben alle documenten in de index automatisch een null-waarde voor dat veld. Er zijn geen extra opslagruimte worden gebruikt totdat nieuwe documenten die zijn toegevoegd aan de index.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Suggesties
De functie suggesties in Azure Search is een type-ahead of automatisch aanvullen query-functie, met een lijst van mogelijke zoektermen in reactie op invoer van gedeeltelijke tekenreeks ingevoerd in een zoekvak. U vast opgevallen Querysuggesties bij gebruik van zoekmachines commerciële web: '.NET' te typen Bing genereert een overzicht van voorwaarden voor '.NET 4.5 uitvoeren ', '.NET Framework 3.5", enzovoort. Wanneer u de zoekservice REST-API, suggesties implementeren in een aangepaste Azure Search-toepassing is het volgende vereist:

* Suggesties inschakelen door het toevoegen van een **suggestie** bouw in uw index, geeft de naam, zoekmodus en een lijst met velden waarvoor automatisch aangevulde wordt aangeroepen. Bijvoorbeeld, als u 'stad' opgeeft als een bronveld gedeeltelijke zoektekenreeks 'Sea' te typen, leidt tot "Seattle", 'Strand' en 'Seatac' (alle drie zijn de namen van de werkelijke stad) aangeboden als suggesties voor de query voor de gebruiker.
* Suggesties worden aangeroepen door het aanroepen van de [suggesties API](#Suggestions) in uw toepassingscode. Gedeeltelijke zoekreeksen worden meestal verzonden naar de service terwijl de gebruiker een zoekopdracht typen is en deze API een set met voorgestelde zinnen retourneert.

Dit artikel wordt uitgelegd hoe u configureert een **suggestie**. Controleer ook de [suggesties API](#Suggestions) voor meer informatie over hoe een suggestie wordt gebruikt.

**Gebruik**

`Suggesters`worden gemaakt in de index en werk aanbevolen als gebruikt voor bepaalde documenten in plaats van losse termen of woordgroepen voorstellen. De beste kandidaat velden zijn titels, namen en andere relatief korte zinnen die een item kunnen identificeren. Minder effectief zijn herhalende velden, zoals de categorieën en tags, of zeer lange velden zoals beschrijvingen of opmerkingen velden.

Als onderdeel van de indexdefinitie, kunt u een suggestie één aan toevoegen de `suggesters` verzameling. Eigenschappen die, een suggestie bepalen omvatten het volgende:

* `name`: De naam van de suggestie. U de naam van de suggestie gebruiken bij het aanroepen van de `suggest` API.
* `searchMode`: De gebruikte strategie voor het zoeken naar candidate zinnen. Alleen de modus die momenteel worden ondersteund is `analyzingInfixMatching`, uitvoert, wordt er flexibele overeenkomende woordgroepen aan het begin of in het midden van zinnen.
* `sourceFields`: Een lijst met een of meer velden die de bron van de inhoud voor suggesties. Alleen velden van het type `Edm.String` en `Collection(Edm.String)` mogelijk bronnen voor suggesties. Alleen de velden die een aangepaste taalanalyse ingesteld niet kunnen worden gebruikt.

**Suggestie voorbeeld**

Een suggestie maakt deel uit van de index. Er kan slechts één suggestie bestaan in de `suggesters` verzameling in de huidige versie, naast de verzameling van velden en `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Als u de openbare preview-versie van Azure Search gebruikt `suggesters` wordt vervangen door een oudere Boole-eigenschap (`"suggestions": false`) die alleen voorvoegsel suggesties voor korte tekenreeksen (3-25 tekens) wordt ondersteund. De vervanging ervan `suggesters`, ondersteunt infix overeenkomende waarmee wordt gezocht naar overeenkomende voorwaarden aan het begin of in het midden van veldinhoud, met betere tolerantie voor fouten in zoekreeksen. Vanaf versie van de algemeen beschikbaar, is dit nu de enige implementatie van de suggesties API. De oudere `suggestions` eigenschap die is geïntroduceerd in `api-version=2014-07-31-Preview` blijft werken in deze versie, maar is niet operationeel is in de `2015-02-28` of latere versies van Azure Search.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Index bijwerken
U kunt een bestaande index in Azure Search met behulp van een HTTP PUT-aanvraag bijwerken. Updates kunnen nieuwe velden toevoegen aan het bestaande schema, CORS-opties wijzigen en wijzigen, scoreprofiel profielen bevatten. Zie [toevoegen score berekenen voor profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie. U opgeven de naam van de index om bij te werken op de aanvraag-URI:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Belangrijk:** ondersteuning voor index schema-updates is beperkt tot bewerkingen waarvoor de search-index opnieuw opbouwen niet nodig. Alle schema-updates die opnieuw worden geïndexeerd moeten, zoals het wijzigen van veldtypen, worden momenteel niet ondersteund. Nieuwe velden kunnen op elk gewenst moment worden toegevoegd, hoewel bestaande velden kunnen niet worden gewijzigd of verwijderd. Hetzelfde geldt voor `suggesters`. Nieuwe velden kunnen worden toegevoegd aan een suggestie op het moment dat de velden worden toegevoegd, maar velden kunnen niet worden verwijderd uit `suggesters` en bestaande velden kunnen niet worden toegevoegd aan `suggesters`.

Wanneer u een nieuw veld toevoegt aan een index, hebben alle documenten in de index automatisch een null-waarde voor dat veld. Er zijn geen extra opslagruimte worden gebruikt totdat nieuwe documenten die zijn toegevoegd aan de index.

**Aanvraag**

HTTPS is vereist voor alle aanvragen van de service. De **Index bijwerken** aanvraag is opgesteld met HTTP PUT. Met opslag is de naam van de index onderdeel van de URL. Als de index niet bestaat, wordt deze gemaakt. Als de index al bestaat, wordt deze bijgewerkt naar de nieuwe definitie.

De naam van de index moet in kleine letters worden, beginnen met een letter of cijfer, hebben geen slashes of punten en minder dan 128 tekens bevatten. De rest van de naam kan na het starten van de naam van de index met een letter of cijfer bevatten een letter, cijfer en streepjes, zolang de streepjes niet opeenvolgende zijn.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `Content-Type`: Vereist. Stel dit in op`application/json`
* `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **Index bijwerken** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Syntaxis van de aanvraag hoofdtekst**

Bij het bijwerken van een bestaande index vergezeld de hoofdtekst gaan van de oorspronkelijke schemadefinitie, plus de nieuwe velden die u wilt toevoegen, evenals de gewijzigde scoreprofiel profielen, suggestiefunctie en CORS-opties, indien van toepassing. Als u de CORS-opties en profielen scoreprofiel niet wijzigt, moet u de oorspronkelijke wanneer de index is gemaakt opnemen. In het algemeen het beste patroon voor updates te gebruiken is het ophalen van de indexdefinitie met een GET, wijzigt u deze vervolgens bijwerken met PUT.

De schemasyntaxis van het gebruikt voor het maken van een index is hier gereproduceerd voor uw gemak. Zie [Create Index](#CreateIndex) voor meer informatie.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Antwoord**

Aanvraag is gelukt: ' 204 geen inhoud '.

Standaard wordt de antwoordtekst leeg zijn. Echter, als de `Prefer` aanvraagheader is ingesteld op `return=representation`, de hoofdtekst van het antwoord bevat de JSON voor de definitie van de index die is bijgewerkt. In dit geval wordt de statuscode geslaagd zijn ' 200 OK '.

**Definitie van de index met aangepaste analyzers bijwerken**

Als er een analyzer, een tokenizer, een token filter of een char-filter is gedefinieerd, kan niet worden gewijzigd. Nieuwe te kunnen alleen worden toegevoegd aan een bestaande index als de `allowIndexDowntime` vlag is ingesteld op true in de index update-aanvraag: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Houd er rekening mee dat deze bewerking uw index offline voor ten minste een paar seconden, waardoor uw indexeren en queryaanvragen mislukken wordt geplaatst. Beschikbaarheid van de prestaties en schrijven van de index kan worden gehinderd minuten nadat de index is bijgewerkt of langer voor zeer grote indexen.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Lijst met indexen
De **lijst indexen** bewerking retourneert een lijst van de indexen momenteel in uw Azure Search-service.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle aanvragen van de service. De **lijst indexen** aanvraag kan worden opgesteld met de GET-methode.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **lijst indexen** aanvraag moet bevatten een `api-key` ingesteld op een beheersleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

Hier volgt een voorbeeld-antwoordtekst:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Houd er rekening mee dat u het antwoord naar beneden eigenschappen die u geïnteresseerd bent in kunt filteren. Bijvoorbeeld: als u alleen een lijst met indexnamen wilt, gebruiken de OData `$select` query-optie:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

In dit geval wordt zou het antwoord van het bovenstaande voorbeeld als volgt uitzien:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Dit is een nuttig techniek bandbreedte opslaan als er een groot aantal indexen in uw zoekservice.

<a name="GetIndex"></a>

## <a name="get-index"></a>Index ophalen
De **Index ophalen** bewerking krijgt de definitie van de index van Azure Search.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **Index ophalen** aanvraag kan worden opgesteld met de GET-methode.

De [indexnaam] in de aanvraag-URI geeft aan welke index om te retourneren van de indexverzameling.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **Index ophalen** aanvraag moet bevatten een `api-key` ingesteld op een beheersleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

Zie het voorbeeld JSON in [maken en bijwerken van een Index](#CreateUpdateIndexExample) voor een voorbeeld van de nettolading van de reactie.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Index verwijderen
De **Index verwijderen** bewerking verwijdert u een index en de bijbehorende documentatie van uw Azure Search-service. U krijgt de naam van de index van het servicedashboard in de Azure portal of van de API. Zie [lijst indexen](#ListIndexes) voor meer informatie.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **Index verwijderen** aanvraag kan worden opgesteld met de DELETE-methode.

De [indexnaam] in de aanvraag-URI geeft aan welke index te verwijderen uit de indexverzameling.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service-URL. De **Index verwijderen** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 204 geen inhoud wordt geretourneerd voor een geslaagde reactie.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Indexstatistieken opvragen
De **statistieken voor Index ophalen** bewerking retourneert van Azure Search een aantal documenten voor de huidige index plus opslaggebruik.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Statistieken over document aantal en de opslaggrootte worden elke paar minuten, niet in realtime verzameld. De statistieken die is geretourneerd door deze API kan daarom niet overeen met wijzigingen die zijn veroorzaakt door een recente indexbewerkingen.
> 
> 

**Aanvraag**

HTTPS is vereist voor alle services aanvragen. De **statistieken voor Index ophalen** aanvraag kan worden opgesteld met de GET-methode.

De [indexnaam] in de aanvraag-URI Hiermee geeft u de service om terug te keren indexstatistieken voor de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **ophalen indexstatistieken** aanvraag moet bevatten een `api-key` ingesteld op een beheersleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

De antwoordtekst is in de volgende indeling:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Test Analyzer
De **analyseren API** ziet u hoe een analyzer tekst opgesplitst in tokens.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle services aanvragen. De **analyseren API** aanvraag kan worden opgesteld met de POST-methode.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **analyseren API** aanvraag moet bevatten een `api-key` ingesteld op een beheersleutel (in plaats van een querysleutel).

U moet ook de naam van de index en de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

of

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

De `analyzer_name`, `tokenizer_name`, `token_filter_name` en `char_filter_name` moet een geldige naam van de vooraf gedefinieerde of aangepaste analyzers, tokenizers, token filters en char filters voor de index. Voor meer informatie over het proces van lexicale analyse Zie [analyse in Azure Search](https://aka.ms/azsanalysis).

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

De antwoordtekst is in de volgende indeling:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**API-voorbeeld analyseren**

**Aanvraag**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Antwoord**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>Document bewerkingen
In Azure Search is een index wordt opgeslagen in de cloud, en ingevuld met JSON-documenten die u naar de service uploadt. Alle documenten die u uploadt, omvatten de corpus van uw zoekgegevens. Documenten bevatten velden, zijn sommige van deze in de zoektermen tokenized als ze worden geüpload. De `/docs` URL-segment in de Azure Search API vertegenwoordigt de verzameling van documenten in een index. Alle bewerkingen die worden uitgevoerd op de verzameling zoals uploaden, samenvoegen, verwijderen of documenten nemen opvragen plaatsen in de context van een enkele index, dus de URL's voor deze bewerkingen wordt altijd gestart met `/indexes/[index name]/docs` voor een naam voor de gegeven index.

Uw toepassingscode moet ofwel JSON-documenten te uploaden naar Azure Search genereren of kunt u een [indexeerfunctie](https://msdn.microsoft.com/library/dn946891.aspx) documenten laden als de gegevensbron Azure SQL Database of Azure Cosmos DB is. Indexen worden gewoonlijk ingevuld uit een enkele gegevensset die u opgeeft.

U moet plannen, moeten een document voor elk item dat u wilt zoeken. Een toepassing van de verhuur film wellicht een document per film, een toepassing via wellicht een document per SKU, een toepassing online cursusprogramma wellicht een document per loop, een bedrijf wellicht een document voor elk academic papier in hun opslagplaats, enzovoort.

Documenten bestaan uit een of meer velden. Velden kunnen bevatten tekst die wordt door Azure Search in zoektermen tokenized, evenals niet getokeniseerd of niet-tekstwaarden plaatsen die kunnen worden gebruikt in filters of scoreprofiel profielen. De namen, gegevenstypen en zoekfuncties ondersteund voor elk veld worden bepaald door het schema van de index. Een van de velden in elke indexschema dat moet worden aangemerkt als een ID en elk document moet een waarde hebben voor het veld ID die een unieke identificatie van dat document in de index. Alle andere documentvelden zijn optioneel en wordt teruggezet op een null-waarde als niet opgegeven. Houd er rekening mee dat null-waarden geen ruimte vrijmaken in de search-index hebben.

Als u documenten uploaden, moet u al hebt gemaakt de index van de service. Zie [Create Index](#CreateIndex) voor meer informatie over deze eerste stap.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Toevoegen, bijwerken of verwijderen van documenten
U kunt uploaden, samenvoegen, merge of uploaden of delete documenten vanuit een opgegeven index met behulp van HTTP POST. Batchverwerking van documenten maximaal (1000 documenten per batch) of 16 MB per batch wordt aanbevolen voor grote aantallen van updates.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvraag**

HTTPS is vereist voor alle aanvragen van de service. U kunt uploaden, samenvoegen, merge of uploaden of delete documenten vanuit een opgegeven index met behulp van HTTP POST.

De aanvraag-URI bevat [naam van de index] opgeven welke index om documenten te plaatsen. U kunt alleen documenten naar een index boeken tegelijk.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `Content-Type`: Vereist. Stel dit in op`application/json`
* `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **documenten toevoegen** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel).

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

De hoofdtekst van de aanvraag bevat een of meer documenten te indexeren. Documenten worden aangeduid met een unieke sleutel. Elk document dat is gekoppeld aan een actie: uploaden, samenvoegen, mergeOrUpload of verwijderen. Het uploaden van aanvragen moeten de gegevens van het document bevatten als een set van sleutel-waardeparen.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> Document sleutels mogen alleen letters, cijfers, streepjes ('-'), onderstrepingstekens ('_') en gelijk tekens ('='). Zie voor meer informatie [naamgevingsregels](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**Documentacties**

* `upload`: Er is een upload-actie is vergelijkbaar met een "upsert", waarbij het document wordt ingevoegd als het nieuwe en bijgewerkt/vervangen als deze bestaat. Houd er rekening mee dat alle velden in het geval van de update worden vervangen.
* `merge`: Een bestaand document merge bijgewerkt met de opgegeven velden. Als het document niet bestaat, mislukt de samenvoeging. Alle velden die u in een samenvoeging opgeeft, vervangen de bestaande velden in het document, ook velden van het type `Collection(Edm.String)`. Bijvoorbeeld, als het document bevat een veld 'labels' met de waarde `["budget"]` en u een samenvoeging met de waarde `["economy", "pool"]` voor 'tags', worden de uiteindelijke waarde van het veld 'tags' `["economy", "pool"]`. Het **niet** worden `["budget", "economy", "pool"]`.
* `mergeOrUpload`: gedraagt zich als `merge` wanneer een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat nog, gedraagt zich als `upload` met een nieuw document.
* `delete`: Het opgegeven document verwijdert verwijderen uit de index. Houd er rekening mee dat alle velden u opgeeft in een `delete` bewerking dan het sleutelveld worden genegeerd. Als u verwijderen van een afzonderlijk veld uit een document wilt, gebruikt u `merge` en stelt u het veld expliciet naar `null`.

**Antwoord**

Statuscode 200 wordt (OK) geretourneerd voor een geslaagd antwoord, wat betekent dat alle items zijn geïndexeerd. Dit wordt aangegeven door de `status` eigenschap wordt ingesteld op true voor alle artikelen, ook als de `statusCode` eigenschap wordt ingesteld op 201 (voor recent geüploade documenten) of 200 (voor samengevoegde of verwijderde documenten):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Statuscode 207 (meerdere Status) wordt geretourneerd wanneer ten minste één item is niet geïndexeerd. Items die niet zijn geïndexeerd hebben de `status` veld ingesteld op false. De `errorMessage` en `statusCode` eigenschappen wordt de reden voor de indexering fout aangeven:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

De volgende tabel beschrijft de verschillende per document statuscodes die kunnen worden geretourneerd in het antwoord. Houd er rekening mee dat sommige duiden op problemen met de aanvraag zelf, terwijl anderen tijdelijke fouten geven. De laatste die probeert u het opnieuw na een vertraging.

<table style="font-size:12">
    <tr>
        <th>Statuscode</th>
        <th>Betekenis</th>
        <th>Herstelbare</th>
        <th>Opmerkingen</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Document is gewijzigd of verwijderd.</td>
        <td>N.v.t.</td>
        <td>Verwijderbewerkingen zijn <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Dat wil zeggen, zelfs als de documentsleutel van een niet in de index bestaat, leidt probeert een delete-bewerking met die sleutel tot een 200-statuscode.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Document is gemaakt.</td>
        <td>N.v.t.</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Er is een fout opgetreden in het document dat waardoor deze niet kan worden geïndexeerd.</td>
        <td>Nee</td>
        <td>Het foutbericht in het antwoord geeft aan wat is het probleem met het document.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Het document kan niet worden samengevoegd omdat de opgegeven sleutel niet in de index bestaat.</td>
        <td>Nee</td>
        <td>Deze fout treedt niet op bij uploads omdat ze nieuwe documenten maken en dit niet voor verwijderingen gebeurt omdat ze <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Er is een versieconflict gedetecteerd bij een poging tot het indexeren van een document.</td>
        <td>Ja</td>
        <td>Dit kan gebeuren wanneer u probeert meer dan één keer gelijktijdig index van hetzelfde document.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>De index is tijdelijk niet beschikbaar omdat deze is bijgewerkt met de vlag 'allowIndexDowntime' is ingesteld op 'true'.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Uw search-service is tijdelijk niet beschikbaar is, mogelijk vanwege een zware belasting.</td>
        <td>Ja</td>
        <td>Uw code moet worden gewacht voordat opnieuw uit te voeren in dit geval of u het risico verlenging van de service niet beschikbaar zijn.</td>
    </tr>
</table> 

**Opmerking**: als uw clientcode wordt vaak een 207 antwoord tegenkomt, een mogelijke reden is dat het systeem belast wordt. U kunt dit bevestigen door het controleren van de `statusCode` eigenschap voor 503. Als dit het geval is, wordt aangeraden ***beperking indexering aanvragen***. Anders als indexeren verkeer niet subside, kan start het systeem het verwijderen van alle aanvragen met 503-fouten.

Statuscode 429 geeft aan dat u het quotum van het aantal documenten per index hebt overschreden. Een nieuwe index maken of een upgrade uit voor hogere Capaciteitslimieten.

**Voorbeeld:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Documenten zoeken
Een **Search** bewerking als een GET of POST-aanvraag wordt uitgegeven en parameters waarmee de criteria voor het selecteren van de overeenkomende documenten bevat.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer POST gebruiken in plaats van GET**

Wanneer u HTTP GET aan te roepen de **Search** API, moet u er rekening mee dat de lengte van de aanvraag-URL kan niet hoger is dan 8 KB. Dit is meestal genoeg voor de meeste toepassingen. Sommige toepassingen produceren echter zeer grote query's of OData-filterexpressies. Voor deze toepassingen namelijk met behulp van HTTP POST een betere keuze kunt u grotere filters en query's dan GET. Met POST is het aantal voorwaarden of componenten in een query de beperkende factor, niet de grootte van de onbewerkte query omdat de limiet voor de aanvraag voor POST ongeveer 16 MB is.

> [!NOTE]
> Hoewel de maximale grootte van POST-aanvraag te lang. is, zoekquery's en filterexpressies mogen niet willekeurig complexe. Zie [Lucene-querysyntaxis](https://msdn.microsoft.com/library/mt589323.aspx) en [OData-expressiesyntaxis](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over zoekopdracht query en filter complexiteit beperkingen.
> 
> 

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **Search** aanvraag kan worden opgesteld met de methoden GET of POST.

De aanvraag-URI geeft aan welke index van de query voor alle documenten die overeenkomen met de parameters. Parameters zijn opgegeven voor de queryreeks weergegeven in het geval van GET-aanvragen en in de aanvraag hoofdtekst in het geval van POST-aanvragen.

Als een best practice bij het maken van GET-aanvragen, moet u [URL coderen](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifieke queryparameters bij het aanroepen van de REST-API rechtstreeks. Voor **Search** bewerkingen, dit omvat:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

URL-codering wordt alleen aanbevolen voor de bovenstaande queryparameters. Als u per ongeluk URL coderen de volledige query-tekenreeks (alles na de?), aanvragen worden verbroken.

Zo is de URL-codering ook alleen nodig bij het aanroepen van de REST-API die rechtstreeks met de GET. Er is geen URL-codering is nodig bij het aanroepen van **zoeken** met behulp van POST, of bij het gebruik de [.NET-clientbibliotheek](https://msdn.microsoft.com/library/dn951165.aspx), die het URL-codering voor u verwerkt.

<a name="SearchQueryParameters"></a>
**Query-Parameters**

**Search** verschillende querycriteria kunnen geven en ook opgeven zoekgedrag parameters accepteert. U opgeven dat deze parameters in de URL van de querytekenreeks bij het aanroepen van **Search** via GET en als JSON-eigenschappen in de aanvraagtekst bij het aanroepen van **Search** via POST. De syntaxis voor een aantal parameters is enigszins verschillen tussen GET en POST. Deze verschillen worden vermeld als die van toepassing zijn hieronder:

`search=[string]`(optioneel) - de tekst om naar te zoeken. Alle `searchable` velden worden doorzocht door standaard tenzij `searchFields` is opgegeven. Bij het zoeken naar `searchable` velden, de zoektekst zelf tokenized, zodat meerdere voorwaarden kunnen worden gescheiden door spaties bevatten (bijvoorbeeld: `search=hello world`). Gebruik voor elke term `*` (dit kan handig zijn voor Booleaanse filterquery's). Als deze parameter wordt weggelaten heeft hetzelfde effect als instellen op `*`. Zie [eenvoudige querysyntaxis](https://msdn.microsoft.com/library/dn798920.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht.

* **Opmerking**: de resultaten kunnen soms verrassend worden tijdens het opvragen via `searchable` velden. Het tokenizer bevat de logica voor de afhandeling van aanvragen die gemeenschappelijk zijn voor de Engelse tekst zoals apostrof, komma's in cijfers, enzovoort. Bijvoorbeeld: `search=123,456` komt overeen met een enkele term 123,456 in plaats van de afzonderlijke termen 123 en 456, omdat komma's worden gebruikt als duizend scheidingstekens voor grote aantallen in het Engels. Daarom wordt u aangeraden witruimte in plaats van leestekens te scheiden van de voorwaarden in de `search` parameter.

`searchMode=any|all`(optioneel, wordt standaard ingesteld op `any`) - of een of meer van de zoektermen om op te tellen van het document als een overeenkomst moeten overeenkomen.

`searchFields=[string]`(optioneel): de lijst met door komma's gescheiden veldnamen om te zoeken naar de opgegeven tekst. Doelvelden moeten worden gemarkeerd als `searchable`.

`queryType=simple|full`(optioneel, wordt standaard ingesteld op `simple`): wanneer is ingesteld op 'eenvoudige' zoektekst wordt geïnterpreteerd met behulp van een eenvoudige querytaal waarmee symbolen zoals +, * en ' '. Query's worden geëvalueerd in de doorzoekbare velden (of velden die zijn aangegeven in `searchFields`) in elk document standaard. Als het querytype is ingesteld op `full` zoektekst met behulp van de Lucene-querytaal waarmee veld-specifieke en gewogen zoekopdrachten wordt geïnterpreteerd. Zie [eenvoudige querysyntaxis](https://msdn.microsoft.com/library/dn798920.aspx) en [Lucene-querysyntaxis](https://msdn.microsoft.com/library/mt589323.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht. 

> [!NOTE]
> Zoeken in de querytaal wordt niet ondersteund voor $filter dat vergelijkbare functionaliteit biedt Lucene liggen.
> 
> 

`moreLikeThis=[key]`(optioneel) **Belangrijk:** deze functie is alleen beschikbaar in `2015-02-28-Preview`. Deze optie kan niet worden gebruikt in een query met de parameter text search `search=[string]`. De `moreLikeThis` parameter vindt documenten die vergelijkbaar met het document dat is opgegeven door de documentsleutel zijn. Wanneer een search-aanvraag wordt gedaan met `moreLikeThis`, een lijst met zoektermen is gegenereerd op basis van de frequentie en zeldzaamheid van termen in de brondocument. Deze voorwaarden worden vervolgens gebruikt om de aanvraag. Standaard wordt de inhoud van alle `searchable` velden worden beschouwd als tenzij `searchFields` wordt gebruikt om te beperken welke velden worden doorzocht.  

`$skip=#`(optioneel): het aantal zoekresultaten om over te slaan; Kan niet meer dan 100.000. Als u wilt scannen van documenten in volgorde, maar kan niet worden gebruikt `$skip` vanwege deze beperking, kunt u overwegen `$orderby` voor een compleet besteld sleutel en `$filter` query in plaats daarvan met een bereik.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `skip` in plaats van `$skip`.
> 
> 

`$top=#`(optioneel): het aantal zoekresultaten om op te halen. Dit kan worden gebruikt in combinatie met `$skip` clientzijde oproepen van zoekresultaten.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `top` in plaats van `$top`.
> 
> 

`$count=true|false`(optioneel, wordt standaard ingesteld op `false`)-geeft aan of voor het ophalen van het totale aantal resultaten. Dit is de telling van alle documenten die overeenkomen met de `search` en `$filter` parameters worden genegeerd `$top` en `$skip`. De waarde instelt op `true` mogelijk invloed op de prestaties. Houd er rekening mee dat het aantal dat wordt geretourneerd een benadering is.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `count` in plaats van `$count`.
> 
> 

`$orderby=[string]`(optioneel): een lijst met door komma's gescheiden expressies sorteer de resultaten op. Elke expressie kan bestaan uit naam van een veld of een aanroep van de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` aangegeven oplopende, en `desc` om aan te geven aflopende. De standaardwaarde is oplopende volgorde. Ties wordt de overeenkomst scores van documenten worden opgesplitst. Als er geen `$orderby` is opgegeven, wordt de standaardsorteervolgorde is Aflopend op document overeen score. Er is een limiet van 32 componenten voor `$orderby`.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `orderby` in plaats van `$orderby`.
> 
> 

`$select=[string]`(optioneel): een lijst met door komma's gescheiden velden om op te halen. Als u niets opgeeft, moet alle velden die zijn gemarkeerd als worden opgehaald in het schema zijn opgenomen. U kunt ook expliciet alle velden aanvragen door deze parameter in te stellen op `*`.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `select` in plaats van `$select`.
> 
> 

`facet=[string]`(nul of meer) - facet door een veld. De tekenreeks kan eventueel parameters voor het aanpassen van de facetten die zijn uitgedrukt als een door komma's gescheiden `name:value` paren. Geldige parameters zijn:

* `count`(max. aantal facet voorwaarden; standaardwaarde is 10). Er is geen maximum maar hogere waarden worden een bijbehorende op de prestaties, vooral als het meervoudige veld een groot aantal unieke voorwaarden bevat.
  * Bijvoorbeeld: `facet=category,count:5` de top vijf categorieën in facet resultaten opgehaald.  
  * **Opmerking**: als de `count` parameter kleiner is dan het aantal unieke termen, de resultaten mogelijk niet nauwkeurig. Dit is vanwege de manier waarop facetten query's zijn verdeeld over shards. Verhogen `count` doorgaans verhoogt de nauwkeurigheid van de termijn tellingen, maar in een prestatievermindering optreden.
* `sort`(een van de `count` te sorteren *aflopende* op aantal `-count` te sorteren *oplopende* op aantal `value` te sorteren *oplopende* door de waarde of `-value` te sorteren *aflopende* door waarde)
  * Bijvoorbeeld: `facet=category,count:3,sort:count` opgehaald van de belangrijkste drie categorieën in facet resultaten in aflopende volgorde voor het aantal documenten met de stadsnaam van elke. Bijvoorbeeld, als de eerste drie categorieën Budget, Motel en luxe zijn, Budget gevonden in de 5 is, Motel 6 en heeft, luxe 4 is, klikt u vervolgens de buckets niet in de volgorde Motel, Budget, luxe.
  * Bijvoorbeeld: `facet=rating,sort:-value` buckets voor alle mogelijke beoordelingen in aflopende volgorde voor waarde produceert. Als de classificaties uit 1 tot en met 5, wordt de buckets besteld 5, 4, 3, 2, 1, ongeacht het aantal documenten overeenkomen met elke beoordeling.
* `values`(pipe gescheiden numerieke of `Edm.DateTimeOffset` waarden opgeven van een dynamische set facet vermelding waarden)
  * Bijvoorbeeld: `facet=baseRate,values:10|20` produceert drie buckets: één voor base snelheid 0 tot en met, maar niet 10, één voor 10 tot inclusief maar niet inclusief 20 en één voor 20 of hoger.
  * Bijvoorbeeld: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produceert twee buckets: één voor hotels renovated vóór februari 2010 en één voor hotels renovated februari 1e, 2010 of hoger.
* `interval`(geheel getal interval dat groter is dan 0 voor getallen, of `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` datumwaarden tijd)
  * Bijvoorbeeld: `facet=baseRate,interval:100` produceert op basis van base snelheid bereiken met een grootte van 100 buckets. Bijvoorbeeld, als base tarieven alle tussen $60 en $600, zullen er buckets voor 0-100, 100-200, 200 en 300, 400 500, 300 400 en 500-600.
  * Bijvoorbeeld: `facet=lastRenovationDate,interval:year` één bucket voor elk jaar wanneer hotels zijn renovated produceert.
* `timeoffset`([+-] hh: mm, [+-] UUMM, of [+-] hh) `timeoffset` is optioneel. Kan alleen worden gecombineerd met de `interval` optie, en alleen wanneer toegepast op een veld van type `Edm.DateTimeOffset`. De waarde geeft de UTC-tijd offset voor serviceaccount bij het instellen van tijd grenzen.
  * Bijvoorbeeld: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` maakt gebruik van de dag-grens die begint bij 01:00:00 UTC (middernacht in de doel-tijdzone)
* **Opmerking**: `count` en `sort` kunnen worden gecombineerd in dezelfde facet-specificatie, maar kan niet worden gecombineerd met `interval` of `values`, en `interval` en `values` kan niet worden gecombineerd.
* **Opmerking**: Interval facetten op datum / tijd worden berekend op basis van UTC-tijd als `timeoffset` is niet opgegeven. Bijvoorbeeld: voor `facet=lastRenovationDate,interval:day`, de dag grens bij 00:00:00 UTC begint. 

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `facets` in plaats van `facet`. Bovendien geeft u het als een JSON-matrix van tekenreeksen waar elke tekenreeks een afzonderlijke facet-expressie is.
> 
> 

`$filter=[string]`(optioneel): een zoekexpressie gestructureerde in de standaard OData-syntaxis.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `filter` in plaats van `$filter`.
> 
> 

`highlight=[string]`(optioneel): een reeks door komma's gescheiden veldnamen gebruikt voor treffer worden gemarkeerd. Alleen `searchable` velden kunnen worden gebruikt voor treffers markeren.

`highlightPreTag=[string]`(optioneel, wordt standaard ingesteld op `<em>`): een string-code die voegt toe om licht bereikt. Moet worden ingesteld met `highlightPostTag`.

> [!NOTE]
> Bij het aanroepen van **Search** gebruik van GET, gereserveerde tekens in de URL moet procent gecodeerd (bijvoorbeeld, in plaats van #, % 23).
> 
> 

`highlightPostTag=[string]`(optioneel, wordt standaard ingesteld op `</em>`)-een string-code die wordt toegevoegd voor licht bereikt. Moet worden ingesteld met `highlightPreTag`.

> [!NOTE]
> Bij het aanroepen van **Search** gebruik van GET, gereserveerde tekens in de URL moet procent gecodeerd (bijvoorbeeld, in plaats van #, % 23).
> 
> 

`scoringProfile=[string]`(optioneel): de naam van een scoreprofiel evalueren overeen scores voor documenten-koppeling om de resultaten te sorteren.

`scoringParameter=[string]`Geeft aan de waarden voor elke parameter die is gedefinieerd in een score-functie (nul of meer) - (bijvoorbeeld `referencePointParameter`) met de indeling `name-value1,value2,...`.

* Bijvoorbeeld, als de scoreprofiel een functie met een parameter met de naam 'mylocation definieert' de queryoptie-tekenreeks zou zijn `&scoringParameter=mylocation--122.2,44.8`. Het eerste streepje scheidt u de naam van de lijst met waarden terwijl de tweede streepje deel uit van de eerste waarde (lengtegraad in dit voorbeeld maakt).
* Scoreprofiel parameters kunt u een dergelijke waarden in de lijst met enkele aanhalingstekens escape zoals voor de code die versterking kunt bevatten komma's. Als de waarden zelf tussen enkele aanhalingstekens bevatten, kunt u dit omheen door te verdubbelen.
  * Bijvoorbeeld, als u een parameter met de naam 'mytag' versterking label hebben en u wilt verhogen in het label waarden 'Hallo, O'Brien' en 'Smith' de query verbindingsreeksoptie zou worden `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Aanhalingstekens zijn alleen vereist voor waarden die door komma's bevatten.

> [!NOTE]
> Bij het aanroepen van **Search** POST gebruikt, deze parameter is met de naam `scoringParameters` in plaats van `scoringParameter`. U ook als een JSON-matrix van tekenreeksen waar elke tekenreeks een afzonderlijke is `name-values` paar.
> 
> 

`minimumCoverage`(optioneel, de standaardwaarde is 100)-een getal tussen 0 en 100 die het percentage van de index die moet worden gedekt door een zoekopdracht om de query moet worden gerapporteerd als een succes aangeeft. Standaard de gehele index moet beschikbaar zijn of `Search` HTTP-statuscode 503 wordt geretourneerd. Als u instelt `minimumCoverage` en `Search` is geslaagd, wordt HTTP 200 retourneren en bevatten een `@search.coverage` waarde in het antwoord die het percentage van de index die is opgenomen in de query aangeeft.

> [!NOTE]
> Als deze parameter op een waarde lager dan 100 is handig voor beschikbaarheid van de zoekopdracht zelfs voor services met slechts één replica. Niet alle overeenkomende documenten zijn echter gegarandeerd aanwezig zijn in de zoekresultaten. Als zoeken intrekken belangrijker voor uw toepassing dan beschikbaarheid, wordt het is raadzaam om te laten `minimumCoverage` op de standaardwaarde van 100.
> 
> 

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter in de URL ongeacht of u aanroepen **Search** met GET of POST.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service-URL. De **Search** aanvraag kunt opgeven, een beheersleutel of een querysleutel voor `api-key`.

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Voor GET: geen.

Voor POST:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Voortzetting van antwoorden voor Deelzoekopdrachten**

Soms kan geen Azure Search alle aangevraagde resultaten retourneren in een enkel antwoord met zoeken. Dit kan gebeuren om verschillende redenen, zoals wanneer de query te veel documenten aanvragen door op te geven niet `$top` of een waarde opgeeft voor `$top` die te groot is. In dergelijke gevallen kunt u Azure Search bevat de `@odata.nextLink` aantekening in de hoofdtekst van antwoord en ook `@search.nextPageParameters` als deze een POST-aanvraag. De waarden van deze aantekeningen kunt u een andere Search-aanvraag voor het ophalen van het volgende gedeelte van het antwoord van de zoekactie te formuleren. Dit wordt genoemd, een ***voortzetting*** van de oorspronkelijke zoekopdracht aanvraag en de aantekeningen in het algemeen worden genoemd ***voortzetting tokens***. Zie [het onderstaande voorbeeld](#SearchResponse) voor meer informatie over de syntaxis van deze aantekeningen en waar ze worden weergegeven in de hoofdtekst van het antwoord. 

De redenen waarom Azure Search voortzetting tokens retourneert zijn implementatie en kan worden gewijzigd. Robuuste clients moet altijd gereed is voor het afhandelen van gevallen waarbij minder documenten dan verwacht worden geretourneerd en een vervolgtoken is opgenomen om door te gaan met het ophalen van documenten. Houd er ook rekening mee dat u dezelfde HTTP-methode als de oorspronkelijke aanvraag gebruiken moet om door te gaan. Bijvoorbeeld, als u een GET-aanvraag verzonden, voortzetting verzoeken u verzendt moeten ook gebruiken GET (en ook voor POST).

<a name="SearchResponse"></a>
**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Voorbeelden:**

U vindt aanvullende voorbeelden gegeven op de [OData-expressiesyntaxis voor Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) pagina.

1)    Zoeken in de Index gesorteerd Aflopend op datum.

    GET-/indexes/hotels/docs? zoeken = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": ' * ', 'orderby': "lastRenovationDate desc"}

2)    Zoeken in de index in een meervoudige zoekopdracht, en facetten voor categorieën, classificatie, labels, evenals artikelen met baseRate in specifieke bereiken ophalen:

    GET /indexes/hotels/docs? zoeken = test & facet = categorie & facet = classificatie & facet = labels & facet baseRate, waarden: 80 = | 150 | 220 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": 'test', 'facetten': ['categorie', 'classificatie', 'tags', ' baseRate, waarden: 80 | 150 | 220"]}

3)    De queryresultaten van de vorige meervoudige met een filter beperken nadat de gebruiker klikt op classificatie 3 en categorie 'Motel':

    GET /indexes/hotels/docs? zoeken = test & facet = labels & facet baseRate, waarden: 80 = | 150 | 220 & $filter = classificatie eq 3 en categorie-eq "Motel" & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": 'test', 'facetten': ['tags', ' baseRate, waarden: 80 | 150 | 220"], 'filter': 'classificatie eq 3 en categorie-eq 'Motel' '}

4) In een meervoudige zoekopdracht, moet u een bovenlimiet instellen op unieke voorwaarden in een query zijn geretourneerd. De standaardwaarde is 10, maar u kunt vergroten of verkleinen van deze waarde met behulp van de `count` parameter bij de `facet` kenmerk:

    GET-/indexes/hotels/docs? zoeken = test & facet = city, aantal: 5 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {'zoeken': 'test', 'facetten': ["city, aantal: 5"]}

5)    Zoeken in de Index binnen specifieke velden; Bijvoorbeeld, een specifieke taal zijn gebonden veld:

    GET-/indexes/hotels/docs? zoeken = hôtel & searchFields = description_fr & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "hôtel', 'searchFields':"description_fr"}

6) De Index zoeken in meerdere velden. U kunt bijvoorbeeld, opslaan en opvragen doorzoekbare velden in meerdere talen, allemaal binnen dezelfde index.  Als de Engelse en Franse beschrijvingen in hetzelfde document naast elkaar bestaan, kunt u een of meer in de queryresultaten retourneren:

    GET-/indexes/hotels/docs? zoeken = hotel & searchFields = beschrijving, description_fr & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "Hotels", "searchFields": "beschrijving, description_fr"}

Houd er rekening mee dat u kunt alleen een query één index op een tijdstip. Maak meerdere indexen voor elke taal geen tenzij u van plan bent een voor een query.

7)    Wisselbestand - Get de 1e pagina van de items (paginaformaat is 10):

    GET-/indexes/hotels/docs? zoeken = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "* ', 'overslaan': 0, 'top': 10}

8)    Wisselbestand - Get van de 2e pagina van de items (paginaformaat is 10):

    GET-/indexes/hotels/docs? zoeken = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {'search': ' * ', 'overslaan': 10, 'top': 10}

9)    Ophalen van een specifieke set velden:

    GET-/indexes/hotels/docs? zoeken = * & $select = hotelName, beschrijving en api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": ' * ', 'select': "hotelName Beschrijving"}

10)  Ophalen van documenten die overeenkomt met een specifieke filterexpressie:

    GET-/indexes/hotels/docs? $filter = (baseRate ge 60 en baseRate lt 300) of hotelName eq fraai blijven & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {'filter': ' (baseRate ge 60 en baseRate lt 300) of hotelName eq 'Fraai verblijf' "}

11) De index en keer terug fragmenten met treffers licht zoeken

    GET-/indexes/hotels/docs? zoeken = iets & Markeer = beschrijving & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "iets", "highlight": 'beschrijving'}

12) Zoeken in de index en documenten van dichter bij verder weg van verwijzing naar een locatie gesorteerd retourneren

    GET-/indexes/hotels/docs? zoeken = iets & $orderby=geo.distance (locatie, geography'POINT(-122.12315 47.88121)') & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "iets', 'orderby': ' geo.distance (locatie, geography'POINT(-122.12315 47.88121)')"}

13) Zoeken in de index ervan uitgaande dat er is een 'geografisch' aangeroepen scoreprofiel met twee afstandsscorefuncties, één definiëren van een parameter genaamd 'currentLocation' en één voor het definiëren van een parameter met de naam 'lastLocation'

    GET /indexes/hotels/docs? zoeken = iets & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "iets', 'scoringProfile': 'geo', 'scoringParameters': [' currentLocation--122.123,44.77233 ', ' lastLocation--121.499,44.2113 ']}

14) Documenten zoeken in de index met behulp [vereenvoudigde querysyntaxis](https://msdn.microsoft.com/library/dn798920.aspx). Deze query retourneert hotels waar doorzoekbare velden de termen 'comfort' en 'locatie', maar niet 'motel bevatten':

    GET-/indexes/hotels/docs? zoeken = comfort + locatie-motel & searchMode = all & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "comfort + locatie-motel ', 'searchMode': 'alle'}

Let op het gebruik van `searchMode=all` hierboven. De standaardwaarde van deze parameter inclusief overschrijft `searchMode=any`, zorgt dat `-motel` 'En niet' in plaats van "Of niet" betekent. Zonder `searchMode=all`u "Of niet" dat wordt uitgebreid dan beperkt zoekresultaten ophalen en dit kan erg intuïtief om bepaalde gebruikers zijn.

15) Documenten zoeken in de index met behulp [lucene-querysyntaxis](https://msdn.microsoft.com/library/mt589323.aspx). Deze query retourneert hotels waar de categorieveld bevat de term 'budget' en de doorzoekbare velden die de zin 'onlangs renovated'. Documenten met de zinsnede 'onlangs renovated' krijgen een hogere rang als gevolg van de termijn versterking waarde (3)

    GET-/indexes/hotels/docs? zoeken = categorie: budget en \"onlangs renovated\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype = full

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"zoeken": "categorie: budget en \"onlangs renovated\"^ 3", 'queryType': 'volledige', 'searchMode': 'alle'}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Lookup-Document
De **Lookup Document** bewerking een document opgehaald uit Azure Search. Dit is handig wanneer een gebruiker op een specifieke zoekresultaat klikt en u wilt zoeken om specifieke details over dat document.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **Lookup Document** verzoek als volgt kan worden samengesteld.

    GET /indexes/[index name]/docs/key?[query parameters]

U kunt ook kunt u de traditionele OData-syntaxis voor het opzoeken van een sleutel:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

De aanvraag-URI bevat [index name] en [sleutel], die aangeeft welk document op te halen uit welke index. U kunt slechts één document tegelijk ophalen. Gebruik **Search** meerdere documenten in één aanvraag ophalen.

**Query-Parameters**

`$select=[string]`(optioneel): een lijst met door komma's gescheiden velden om op te halen. Als dat niet-opgegeven of ingesteld op `*`, alle velden die zijn gemarkeerd als worden opgehaald in het schema zijn opgenomen in de projectie.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service-URL. De **Lookup Document** aanvraag kunt opgeven, een beheersleutel of een querysleutel voor `api-key`.

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Voorbeeld**

Het document dat de sleutel '2' heeft lookup

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Het document dat uit de sleutel met OData-syntaxis 3 lookup:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>Aantal documenten
De **aantal documenten** bewerking wordt een telling van het aantal documenten in een zoekindex opgehaald. De `$count` syntaxis maakt deel uit van het OData-protocol.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **aantal documenten** aanvraag kan worden opgesteld met de GET-methode.

De [indexnaam] in de aanvraag-URI Hiermee geeft u de service te retourneren van een telling van alle items in de verzameling documenten van de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders.

* `Accept`: Met deze waarde moet worden ingesteld op `text/plain`.
* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service-URL. De **aantal documenten** aanvraag kunt opgeven, een beheersleutel of een querysleutel voor `api-key`.

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Geen.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

Hoofdtekst van de reactie bevat de count-waarde als een geheel getal als tekst zonder opmaak geformatteerd.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Suggesties
De **suggesties** bewerking suggesties op basis van deelzoekopdrachten invoer worden opgehaald. Dit wordt doorgaans gebruikt in de zoekvakken opgeven type-ahead suggesties gebruikers zoektermen invoert.

Suggestie aanvragen gericht op de voorstellen doeldocumenten, zodat de voorgestelde tekst kan worden herhaald als meerdere candidate documenten invoer dezelfde zoekopdracht overeenkomen. U kunt `$select` om op te halen van andere documentvelden (inclusief de documentsleutel) zodat u kunt zien welke document is de bron voor elke suggestie.

Een **suggesties** bewerking als een GET of POST-aanvraag is uitgegeven.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer POST gebruiken in plaats van GET**

Wanneer u HTTP GET aan te roepen de **suggesties** API, moet u er rekening mee dat de lengte van de aanvraag-URL kan niet hoger is dan 8 KB. Dit is meestal genoeg voor de meeste toepassingen. Sommige toepassingen produceren echter zeer grote query's specifiek OData-filterexpressies. Voor deze toepassingen namelijk met behulp van HTTP POST een betere keuze kunt u grotere filters dan GET. Met POST is het aantal componenten in een filter de beperkende factor, niet de grootte van de onbewerkte filtertekenreeks omdat de limiet voor de aanvraag voor POST ongeveer 16 MB is.

> [!NOTE]
> Hoewel de maximale grootte van POST-aanvraag erg groot is, kunnen niet filterexpressies willekeurig complex zijn. Zie [OData-expressiesyntaxis](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over filter complexiteit beperkingen.
> 
> 

**Aanvraag**

HTTPS is vereist voor serviceaanvragen. De **suggesties** aanvraag kan worden opgesteld met de methoden GET of POST.

De aanvraag-URI bevat de naam van de index van de query. Parameters, zoals de gedeeltelijk invoer zoekterm zijn opgegeven voor de queryreeks weergegeven in het geval van GET-aanvragen en in de aanvraag hoofdtekst in het geval van POST-aanvragen.

Als een best practice bij het maken van GET-aanvragen, moet u [URL coderen](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifieke queryparameters bij het aanroepen van de REST-API rechtstreeks. Voor **suggesties** bewerkingen, dit omvat:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

URL-codering wordt alleen aanbevolen voor de bovenstaande queryparameters. Als u per ongeluk URL coderen de volledige query-tekenreeks (alles na de?), aanvragen worden verbroken.

Zo is de URL-codering ook alleen nodig bij het aanroepen van de REST-API die rechtstreeks met de GET. Er is geen URL-codering is nodig bij het aanroepen van **suggesties** POST, met of wanneer u de [.NET-clientbibliotheek](https://msdn.microsoft.com/library/dn951165.aspx), die URL-codering voor u verwerkt.

**Query-Parameters**

**Suggesties** verschillende querycriteria kunnen geven en ook opgeven zoekgedrag parameters accepteert. U opgeven dat deze parameters in de URL van de querytekenreeks bij het aanroepen van **suggesties** via GET en als JSON-eigenschappen in de aanvraagtekst bij het aanroepen van **suggesties** via POST. De syntaxis voor een aantal parameters is enigszins verschillen tussen GET en POST. Deze verschillen worden vermeld als die van toepassing zijn hieronder:

`search=[string]`-de tekst moet worden gebruikt voor het voorstellen van query's. Moet minimaal 1 teken en niet meer dan 100 tekens.

`highlightPreTag=[string]`(optioneel): een tekenreeks tag die voegt toe om te zoeken naar treffers. Moet worden ingesteld met `highlightPostTag`.

> [!NOTE]
> Bij het aanroepen van **suggesties** gebruik van GET, gereserveerde tekens in de URL moet procent gecodeerd (bijvoorbeeld, in plaats van #, % 23).
> 
> 

`highlightPostTag=[string]`(optioneel): een tekenreeks tag die worden toegevoegd om te zoeken naar treffers. Moet worden ingesteld met `highlightPreTag`.

> [!NOTE]
> Bij het aanroepen van **suggesties** gebruik van GET, gereserveerde tekens in de URL moet procent gecodeerd (bijvoorbeeld, in plaats van #, % 23).
> 
> 

`suggesterName=[string]`-de naam van de suggestie zoals opgegeven in de `suggesters` verzameling die deel uitmaakt van de indexdefinitie. Een `suggester` bepaalt welke velden voor voorgestelde querytermen worden gescand. Zie [Suggestiefunctie](#Suggesters) voor meer informatie.

`fuzzy=[boolean]`(optioneel, standaard = false)-als ingesteld op waar deze API vindt u suggesties zelfs als er een teken vervangen of ontbreekt in de zoektekst. Het wordt geleverd op de prestaties, zoals fuzzy suggestie zoekopdrachten trager en meer bronnen gebruiken terwijl dit een betere ervaring in sommige scenario's biedt.

`searchFields=[string]`(optioneel): de lijst met door komma's gescheiden veldnamen om te zoeken naar de opgegeven zoektekst. Doelvelden moeten zijn ingeschakeld voor suggesties.

`$top=#`(optioneel, standaard = 5)-het aantal suggesties om op te halen. Moet een getal tussen 1 en 100 liggen.

> [!NOTE]
> Bij het aanroepen van **suggesties** POST gebruikt, deze parameter is met de naam `top` in plaats van `$top`.
> 
> 

`$filter=[string]`(optioneel): een expressie die de documenten filters in aanmerking voor suggesties.

> [!NOTE]
> Bij het aanroepen van **suggesties** POST gebruikt, deze parameter is met de naam `filter` in plaats van `$filter`.
> 
> 

`$orderby=[string]`(optioneel): een lijst met door komma's gescheiden expressies sorteer de resultaten op. Elke expressie kan bestaan uit naam van een veld of een aanroep van de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` aangegeven oplopende, en `desc` om aan te geven aflopende. De standaardwaarde is oplopende volgorde. Er is een limiet van 32 componenten voor `$orderby`.

> [!NOTE]
> Bij het aanroepen van **suggesties** POST gebruikt, deze parameter is met de naam `orderby` in plaats van `$orderby`.
> 
> 

`$select=[string]`(optioneel): een lijst met door komma's gescheiden velden om op te halen. Als u niets opgeeft, worden alleen de sleutel en suggestie tekst wordt geretourneerd. U kunt alle velden expliciet aanvragen door deze parameter in te stellen op `*`.

> [!NOTE]
> Bij het aanroepen van **suggesties** POST gebruikt, deze parameter is met de naam `select` in plaats van `$select`.
> 
> 

`minimumCoverage`(optioneel, de standaardwaarde 80)-een getal tussen 0 en 100 die het percentage van de index die moet worden gedekt door een query suggesties om de query moet worden gerapporteerd als een succes aangeeft. Standaard ten minste 80% van de index moet beschikbaar zijn of `Suggest` HTTP-statuscode 503 wordt geretourneerd. Als u instelt `minimumCoverage` en `Suggest` is geslaagd, wordt HTTP 200 retourneren en bevatten een `@search.coverage` waarde in het antwoord die het percentage van de index die is opgenomen in de query aangeeft.

> [!NOTE]
> Als deze parameter op een waarde lager dan 100 is handig voor beschikbaarheid van de zoekopdracht zelfs voor services met slechts één replica. Niet alle overeenkomende suggesties zijn echter gegarandeerd aanwezig zijn in de resultaten. Als intrekken belangrijker voor uw toepassing dan beschikbaarheid, wordt het is raadzaam niet te verlagen `minimumCoverage` lager dan de standaardwaarde 80.
> 
> 

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter in de URL ongeacht of u aanroepen **suggesties** met GET of POST.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders

* `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service-URL. De **suggesties** aanvraag kunt opgeven, een beheersleutel of een querysleutel als de `api-key`.

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure Portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

**Aanvraagtekst**

Voor GET: geen.

Voor POST:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Als de optie projectie wordt gebruikt voor het ophalen van velden die zijn opgenomen in elk element van de matrix:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Voorbeeld**

5 suggesties waar de deelzoekopdrachten-invoer is 'lux' ophalen

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
