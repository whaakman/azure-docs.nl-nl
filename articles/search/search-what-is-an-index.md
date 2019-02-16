---
title: Maak een definitie van de index en concepten - Azure Search
description: Inleiding tot index termen en concepten in Azure Search, met inbegrip van onderdelen en de fysieke structuur.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 9cd43172fc57443cc89f238e1d4ffaae45301936
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330559"
---
# <a name="create-a-basic-index-in-azure-search"></a>Maak een eenvoudige index in Azure Search

In Azure Search, een *index* is een permanente opslag van *documenten* en andere constructies die worden gebruikt voor gefilterde en de volledige tekst zoekt naar een Azure Search-service. Conceptueel gezien is een document een eenheid die doorzoekbare gegevens in uw index. Een e-commercedetailhandel heeft bijvoorbeeld een document voor elk item dat wordt verkocht, een nieuwsbureau heeft een document voor elk artikel, enzovoort. Deze begrippen aan betrouwbaardere database-equivalenten toewijzen: een *index* lijkt conceptueel gezien op een *tabel* en *documenten* lijken ruwweg op *rijen* in een tabel.

Wanneer u toevoegt of uploaden van een index, maakt Azure Search fysieke structuren op basis van het schema dat u opgeeft. Bijvoorbeeld, als een veld in de index is gemarkeerd als kan worden doorzocht, een omgekeerde index is gemaakt voor dat veld. Later, wanneer u toevoegt of uploadt documenten of naar Azure Search zoekopdrachten, verzendt u aanvragen naar een specifieke index in uw zoekservice. Het laden van velden met documentwaarden heet *indexeren* of opname van gegevens.

U kunt een index maken in de portal [REST-API](search-create-index-rest-api.md), of [.NET SDK](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Aanbevolen workflow

Omdat fysieke structuren zijn gemaakt tijdens het indexeren, moet u [verwijderen en opnieuw maken van indexen](search-howto-reindex.md) wanneer u er belangrijke wijzigingen aanbrengen in de definitie van een bestaande veld. Dit betekent dat tijdens de ontwikkeling, u van plan frequente opnieuw op te bouwen bent moet. U kunt overwegen werken met een subset van uw gegevens om te maken van opnieuw gebouwde go sneller. 

Code in plaats van portal indexeren wordt ook aanbevolen. Als u zich op de portal voor de definitie van de index baseert, heeft u om de definitie van de index op elke opnieuw in te vullen. Als alternatief kunt met behulp van een hulpprogramma zoals [Postman en de REST-API](search-fiddler.md) zijn handig voor het testen van proof of concept wanneer ontwikkelingsprojecten zich nog steeds in de eerste fasen. U kunt incrementele wijzigingen aanbrengen aan de indexdefinitie van een in de hoofdtekst van de aanvraag, de aanvraag wordt verzonden naar uw service opnieuw maken van een index met behulp van een bijgewerkte schema.

## <a name="components-of-an-index"></a>Onderdelen van een index

Stroomschema, wordt een Azure Search-index bestaat uit de volgende elementen. 

De [ *Veldenverzameling* ](#fields-collection) is doorgaans het grootste deel van een index, waarbij elk veld heet, getypt en toegeschreven met toegestane gedragingen die bepalen hoe deze wordt gebruikt. Andere elementen zijn [suggesties](#suggesters), [scoreprofielen](#scoring-profiles), [analyzers](#analyzers) met onderdelen op ondersteuning voor aanpassing, en [CORS](#cors) Opties.

```json
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
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }  
  ],  
  "suggesters": [  
    {  
      "name": "name of suggester",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": ["field1", "field2", ...]  
    }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "name of scoring profile",  
      "text": (optional, only applies to searchable fields) {  
        "weights": {  
          "searchable_field_name": relative_weight_value (positive #'s),  
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
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)  
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)  
          },  
          "tag": {  
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)  
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
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>De kenmerken van de verzameling en veld van de velden

Bij het definiëren van het schema moet u de naam, het type en de kenmerken van elk veld in de index opgeven. Het veldtype classificeert de gegevens die in dat veld worden opgeslagen. Kenmerken worden ingesteld op afzonderlijke velden om op te geven hoe het veld wordt gebruikt. De volgende tabellen bevatten de typen en kenmerken die u kunt opgeven.

### <a name="data-types"></a>Gegevenstypen
| Type | Description |
| --- | --- |
| *Edm.String* |Tekst die van tokens kan worden voorzien om te zoeken in de volledige tekst (woordafbreking, afleiding, enzovoort). |
| *Collection(Edm.String)* |Een lijst met tekenreeksen die van tokens kan worden voorzien om te zoeken in de volledige tekst. Er is geen theoretische bovengrens voor het aantal items in een verzameling, maar de bovengrens van 16 MB voor de nettolading geldt voor alle verzamelingen. |
| *Edm.Boolean* |Bevat de waarden waar/niet waar. |
| *Edm.Int32* |32-bits waarden van een heel getal. |
| *Edm.Int64* |64-bits waarden van een heel getal. |
| *Edm.Double* |Numerieke gegevens met dubbele precisie. |
| *Edm.DateTimeOffset* |Datum- en tijdwaarden die worden weergegeven in de OData-V4-indeling (bijvoorbeeld `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Een punt voor een geografische locatie op de wereld. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-gegevenstypen vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="index-attributes"></a>Indexkenmerken
| Kenmerk | Description |
| --- | --- |
| *Sleutel* |Een tekenreeks met de unieke id van elk document. Deze reeks wordt gebruikt om op te zoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld gebruiken in een [meervoudige navigatie](search-faceted-navigation.md)structuur om op de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om meerdere documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-indexkenmerken vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="storage-implications"></a>Gevolgen van opslag

De kenmerken die u selecteert hebben een invloed op opslag. De volgende schermafbeelding is een afbeelding van index opslag patronen die voortvloeien uit verschillende combinaties van kenmerken. De index is gebaseerd op de [ingebouwde onroerend goed voorbeeld](search-get-started-portal.md) gegevensbron, die u kunt de index en query's uitvoeren in de portal.

Filteren en sorteren van operations-query op exacte overeenkomsten, zodat documenten intact worden opgeslagen. Doorzoekbare velden inschakelen van zoeken in volledige tekst en zoeken bij benadering. Omgekeerde indexen worden gemaakt voor doorzoekbare velden en gevuld met tokens voorwaarden. Een veld te markeren heeft als ophaalbaar geen merkbare invloed op de indexgrootte van de.

![Index-grootte op basis van kenmerk selectie](./media/search-what-is-an-index/realestate-index-size.png "Index-grootte op basis van kenmerk selectie")

Verschillende combinaties van deze zijn kunstmatige, handig voor het verlichten van een punt, maar niet zou leiden tot een levensvatbare index. In de praktijk zou u nooit elk één veld toevoegen aan een suggestie of geen index maken die kan worden doorzocht, maar niet worden opgehaald.

Storage-architectuur wordt beschouwd als een implementatiedetail in Azure Search en kan zonder kennisgeving worden gewijzigd. Er is geen garantie dat de huidige gedrag in de toekomst blijven behouden.

## <a name="suggesters"></a>Suggesties
Een suggestie is een gedeelte van het schema waarmee wordt gedefinieerd welke velden in een index worden gebruikt voor de ondersteuning van automatisch aanvullen of automatisch aangevulde query's in zoekopdrachten. Gedeeltelijke zoekreeksen worden meestal verzonden naar de suggesties (Azure Search Service REST API) terwijl de gebruiker een zoekopdracht typen is en de API een set met voorgestelde zinnen retourneert. 

Een suggestie die u in de index definieert bepaalt welke velden worden gebruikt voor het bouwen van de type-ahead zoektermen. Zie voor meer informatie, [toevoegen suggesties](index-add-suggesters.md) voor informatie over de configuratie.

## <a name="scoring-profiles"></a>Scoreprofielen

Een scoring-profiel is een gedeelte van het schema waarmee aangepaste scoring gedragingen waarmee u invloed hebben op welke items worden weergegeven op een hoger in de lijst met zoekresultaten. Scoreprofielen bestaan uit veldgewichten en functies. Voor het gebruik ervan, geeft u een profiel met de naam van de query-tekenreeks.

Een standaard scoringprofiel is van invloed op de achtergrond voor het berekenen van een zoekscore voor elk item in een resultatenset. U kunt de interne naamloze scoringprofiel gebruiken. U kunt ook instellen defaultScoringProfile gebruik van een aangepast profiel als de standaard, aangeroepen wanneer er een aangepast profiel niet is opgegeven in de query-tekenreeks.

Zie voor meer informatie, [scoreprofielen toevoegen](index-add-scoring-profiles.md).

## <a name="analyzers"></a>Analyses

Het element analyzers Hiermee stelt u de naam van de taal-analysefunctie moet worden gebruikt voor het veld. Zie voor de toegestane set waarden, [taalanalyse in Azure Search](index-add-language-analyzers.md). Deze optie kan alleen worden gebruikt met doorzoekbare velden en deze kan niet worden ingesteld, samen met een **searchAnalyzer** of **indexAnalyzer**. Nadat de analyzer is gekozen, wordt deze niet wijzigen voor het veld.

## <a name="cors"></a>CORS

Client-side '-JavaScript kan niet alle API's aanroepen standaard omdat de browser voorkomen alle cross-origin-aanvragen dat wordt. Inschakelen als u cross-origin-query's naar uw index, CORS (Cross-Origin Resource Sharing) door in te stellen de **corsOptions** kenmerk. Alleen query's uit veiligheidsoverwegingen ondersteuning voor CORS. 

De volgende opties kunnen worden ingesteld voor CORS:

+ **allowedOrigins** (vereist): Dit is een lijst met oorsprongen dat toegang tot uw index wordt verleend. Dit betekent dat alle JavaScript-code opgehaald uit deze oorsprongen mag worden query uitvoeren in uw index (ervan uitgaande dat het de juiste api-sleutel bevat). Elke oorsprong heeft meestal de vorm `protocol://<fully-qualified-domain-name>:<port>` Hoewel `<port>` vaak wordt weggelaten. Zie [Cross-origin resource sharing (Wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) voor meer informatie.

  Als u wilt toestaan toegang tot alle oorsprongen, `*` als één item in de **allowedOrigins** matrix. *Dit wordt niet aanbevolen voor productie-zoekservices* maar vaak is het handig is voor de ontwikkeling en foutopsporing.

+ **maxAgeInSeconds** (optioneel): Browsers gebruiken deze waarde om te bepalen van de duur (in seconden) aan voorbereidende CORS-antwoorden cache. Dit moet een niet-negatief geheel getal zijn. Hoe groter deze waarde is, de prestaties beter, maar hoe langer het duurt voor CORS-beleidswijzigingen worden doorgevoerd. Als deze niet is ingesteld, wordt een standaardduur van 5 minuten worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

U kunt met een goed begrip van de samenstelling van de index blijven in de portal om uw eerste index te maken.

> [!div class="nextstepaction"]
> [Toevoegen van een index (portal)](search-create-index-portal.md)