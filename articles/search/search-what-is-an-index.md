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
ms.openlocfilehash: addc1a0d7356cf1ba536c7ab47e376a48621e2d9
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342487"
---
# <a name="create-a-basic-index-in-azure-search"></a>Maak een eenvoudige index in Azure Search

In Azure Search, een *index* is een permanente opslag van *documenten* en andere constructies die worden gebruikt voor gefilterde en de volledige tekst zoekt naar een Azure Search-service. Conceptueel gezien is een document een eenheid die doorzoekbare gegevens in uw index. Een e-commercedetailhandel heeft bijvoorbeeld een document voor elk item dat wordt verkocht, een nieuwsbureau heeft een document voor elk artikel, enzovoort. Deze begrippen aan betrouwbaardere database-equivalenten toewijzen: een *index* lijkt conceptueel gezien op een *tabel* en *documenten* lijken ruwweg op *rijen* in een tabel.

Wanneer u toevoegt of uploaden van een index, maakt Azure Search fysieke structuren op basis van het schema dat u opgeeft. Bijvoorbeeld, als een veld in de index is gemarkeerd als kan worden doorzocht, een omgekeerde index is gemaakt voor dat veld. Later, wanneer u toevoegt of uploadt documenten of naar Azure Search zoekopdrachten, verzendt u aanvragen naar een specifieke index in uw zoekservice. Het laden van velden met documentwaarden heet *indexeren* of opname van gegevens.

U kunt een index maken in de portal [REST-API](search-create-index-rest-api.md), of [.NET SDK](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Aanbevolen workflow

Die binnenkomen in de juiste index-ontwerp wordt normaal gesproken via meerdere pogingen bereikt. Met behulp van een combinatie van API's en hulpprogramma's, kunt u uw ontwerp snel voltooien.

1. Bepalen of u kunt een [indexeerfunctie](search-indexer-overview.md#supported-data-sources). Als uw externe gegevens een van de ondersteunde gegevensbronnen is, kunt u prototype en laden van een index met behulp van de [ **gegevens importeren** ](search-import-data-portal.md) wizard.

2. Als u niet kunt gebruiken **gegevens importeren**, kunt u nog steeds [een eerste index maken via de portal](search-create-index-portal.md), velden, gegevenstypen, toe te voegen en het toewijzen van kenmerken met behulp van besturingselementen op de **Index toevoegen** de pagina. De portal ziet u welke kenmerken beschikbaar zijn voor verschillende gegevenstypen. Dit is handig als u geen ervaring met index-ontwerp.

   ![Index-pagina toevoegen met de kenmerken van het gegevenstype](media/search-create-index-portal/field-attributes.png "kenmerken van het gegevenstype van de indexpagina toevoegen")
  
   Wanneer u klikt op **maken**, alle van de fysieke structuren ondersteuning van uw index worden gemaakt in uw search-service.

3. Download de index schema met behulp [Index REST-API ophalen](https://docs.microsoft.com/rest/api/searchservice/get-index) en een web testen hulpprogramma zoals [Postman](search-fiddler.md). U hebt nu een JSON-weergave van de index die u hebt gemaakt in de portal. 

   U wordt op dit moment overschakelen naar een benadering op basis van code. De portal is niet geschikt voor herhaling omdat u niet bewerken, een index die al is gemaakt. Maar u kunt Postman en REST gebruiken voor de resterende taken.

4. [Laden van uw index met gegevens](search-what-is-data-import.md). Azure Search accepteert JSON-documenten. Voor het laden van uw gegevens via een programma, kunt u Postman gebruiken met JSON-documenten in de nettolading van de aanvraag. Als uw gegevens niet eenvoudig in JSON uitgedrukt is, is deze stap is het meest intensieve arbeid.

5. Query uitvoeren in uw index, Bekijk resultaten en meer op het indexschema herhalen totdat u begint met de verwachte resultaten ziet. U kunt [ **Search explorer** ](search-explorer.md) of Postman query uitvoeren op uw index.

6. Doorgaan met behulp van code om te herhalen uw ontwerp.  

Omdat fysieke structuren zijn gemaakt in de service [verwijderen en opnieuw maken van indexen](search-howto-reindex.md) is noodzakelijk wanneer u er belangrijke wijzigingen in bestaande velddefinitie ab aanbrengen. Dit betekent dat tijdens de ontwikkeling, u van plan frequente opnieuw op te bouwen bent moet. U kunt overwegen werken met een subset van uw gegevens om te maken van opnieuw gebouwde go sneller. 

Code, in plaats van een portal-benadering wordt aanbevolen voor terugkerende ontwerp. Als u zich op de portal voor de definitie van de index baseert, heeft u om de definitie van de index op elke opnieuw in te vullen. Als alternatief, hulpprogramma's zoals [Postman en de REST-API](search-fiddler.md) zijn handig voor het testen van proof of concept wanneer ontwikkelingsprojecten zich nog steeds in de eerste fasen. U kunt incrementele wijzigingen aanbrengen in de indexdefinitie van een in de hoofdtekst van de aanvraag, en vervolgens de aanvraag te verzenden naar uw service opnieuw maken van een index met behulp van een schema bijgewerkt.

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
| *Edm.String* |De tekst die kan worden voorzien om zoeken in volledige tekst (woordafbreking, afleiding, enzovoort). |
| *Collection(Edm.String)* |Een lijst met tekenreeksen die van tokens kan worden voorzien om te zoeken in de volledige tekst. Er is geen theoretische bovengrens voor het aantal items in een verzameling, maar de bovengrens van 16 MB voor de nettolading geldt voor alle verzamelingen. |
| *Edm.Boolean* |Bevat de waarden waar/niet waar. |
| *Edm.Int32* |32-bits waarden van een heel getal. |
| *Edm.Int64* |64-bits waarden van een heel getal. |
| *Edm.Double* |Numerieke gegevens met dubbele precisie. |
| *Edm.DateTimeOffset* |Datum-en tijdwaarden weergegeven in de OData-V4-indeling (bijvoorbeeld `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
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

De kenmerken die u selecteert hebben een invloed op opslag. De volgende schermafbeelding ziet u index opslag patronen die voortvloeien uit verschillende combinaties van kenmerken.

De index is gebaseerd op de [ingebouwde onroerend goed voorbeeld](search-get-started-portal.md) gegevensbron, die u kunt de index en query's uitvoeren in de portal. Hoewel de index schema's zijn niet wordt weergegeven, kunt u de kenmerken op basis van de naam van de index kunt afleiden. Bijvoorbeeld, *onroerend goed doorzoekbaar* index heeft de **doorzoekbare** geselecteerd kenmerk en niets anders, *onroerend goed worden opgehaald* index heeft de  **ophalen mogelijk** geselecteerd kenmerk en niets anders, enzovoort.

![Index-grootte op basis van kenmerk selectie](./media/search-what-is-an-index/realestate-index-size.png "Index-grootte op basis van kenmerk selectie")

Hoewel deze index-varianten kunstmatige zijn, kunt we deze raadplegen voor brede vergelijkingen van de invloed van kenmerken op opslag. Instelling komt **ophaalbaar** index vergroten? Nee. Kiest, wordt er velden aan toe te voegen een **suggestie** index vergroten? Ja.

Indexen die ondersteuning bieden voor filteren en sorteren zijn proportioneel groter is dan de indexen die ondersteuning bieden voor zoeken in alleen volledige tekst. De reden is dat query filteren en sorteren op exacte overeenkomsten, zodat documenten intact worden opgeslagen. Doorzoekbare velden ondersteuning van volledige tekst en fuzzy zoeken daarentegen gebruik omgekeerde indexen, die worden ingevuld met tokens termen die minder ruimte dan de hele documenten gebruiken.

> [!Note]
> Storage-architectuur wordt beschouwd als een implementatiedetail in Azure Search en kan zonder kennisgeving worden gewijzigd. Er is geen garantie dat de huidige gedrag in de toekomst blijven behouden.

## <a name="suggesters"></a>Suggesties
Een suggestie is een gedeelte van het schema waarmee wordt gedefinieerd welke velden in een index worden gebruikt voor de ondersteuning van automatisch aanvullen of automatisch aangevulde query's in zoekopdrachten. Normaal gesproken gedeeltelijke zoekreeksen worden verzonden naar de [suggesties (REST-API)](https://docs.microsoft.com/rest/api/searchservice/suggestions) terwijl de gebruiker een zoekopdracht typen is en de API een set met voorgestelde zinnen retourneert. 

Velden worden toegevoegd aan een suggestie worden gebruikt voor het bouwen, automatisch aangevulde zoektermen. Alle van de zoektermen zijn gemaakt tijdens het indexeren en apart opgeslagen. Zie voor meer informatie over het maken van een structuur suggestie [toevoegen suggesties](index-add-suggesters.md).

## <a name="scoring-profiles"></a>Scoreprofielen

Een [scoringprofiel](index-add-scoring-profiles.md) is een gedeelte van het schema waarmee wordt gedefinieerd aangepast scoren gedrag waarmee kunt u bepalen welke items worden weergegeven op een hoger in de lijst met zoekresultaten. Scoreprofielen bestaan uit veldgewichten en functies. Voor het gebruik ervan, geeft u een profiel met de naam van de query-tekenreeks.

Een standaard scoringprofiel is van invloed op de achtergrond voor het berekenen van een zoekscore voor elk item in een resultatenset. U kunt de interne naamloze scoringprofiel gebruiken. U kunt ook instellen **defaultScoringProfile** gebruik van een aangepast profiel als de standaard, aangeroepen wanneer er een aangepast profiel niet is opgegeven in de query-tekenreeks.

## <a name="analyzers"></a>Analyses

Het element analyzers Hiermee stelt u de naam van de taal-analysefunctie moet worden gebruikt voor het veld. Zie voor meer informatie over het bereik van de analyzers die beschikbaar zijn voor u, [analyzers toe te voegen aan een Azure Search-index](search-analyzers.md). Analyse kunnen alleen worden gebruikt met doorzoekbare velden. Nadat de analyzer is toegewezen aan een veld, kan niet worden gewijzigd, tenzij u de index opnieuw opbouwen.

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