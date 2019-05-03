---
title: Het model complexe gegevenstypen - Azure Search
description: Geneste of hiërarchische gegevensstructuren kunnen worden gemodelleerd in een Azure Search-index met behulp van gegevenstypen ComplexType en verzamelingen.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: seodec2018
ms.openlocfilehash: 397b3ea7fee67e25cd160f6b529a660e18c44046
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024745"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Hoe u complexe gegevenstypen modelleren in Azure Search

Externe gegevenssets die worden gebruikt voor het vullen van een Azure Search-index soms bevatten hiërarchische of geneste substructuren. Voorbeelden mogelijk meerdere locaties en telefoonnummers voor één klant, meerdere kleuren en formaten omvatten voor een enkele SKU, meerdere auteurs van een enkel rapport, enzovoort. Voorwaarden modelleren, ziet u deze structuren aangeduid als *complexe gegevenstypen*, *samengestelde gegevenstypen*, *samengestelde gegevenstypen*, of *cumulatieve gegevenstypen*. In de terminologie voor Azure Search is een complex type een veld dat onderliggende objecten (onderliggende velden) bevat die zelf kunnen eenvoudig of complex zijn. Dit is vergelijkbaar met een type gestructureerde gegevens in een programmeertaal. Complexe velden kunnen worden enkele velden, die staan voor een enkel object in het document of een verzameling, waarmee een matrix met objecten

Azure Search biedt systeemeigen ondersteuning voor complexe typen en verzamelingen. Deze typen samen, kunnen u vrijwel alle geneste JSON-structuur in een Azure Search-index model. In eerdere versies van Azure Search API's alleen afgevlakt rij sets kunnen worden geïmporteerd. In de nieuwste versie, de index kan nu beter aansluiten komen overeen met brongegevens. Met andere woorden, als de brongegevens complexe typen bevat, uw index kunt complexe typen hebben ook.

Als u wilt beginnen, wordt aangeraden de [Hotels gegevensset](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md), die u kunt laden de **gegevens importeren** wizard in de Azure-portal. De wizard detecteert complexe typen in de bron en stelt een indexschema op basis van de gedetecteerde structuren.

> [!Note]
> Ondersteuning voor complexe typen is algemeen beschikbaar in `api-version=2019-05-06`. 
>
> Als uw search-oplossing is gebouwd op eerdere oplossingen van platte gegevenssets in een verzameling, moet u uw index om op te nemen van complexe typen zoals ondersteund in de nieuwste versie van de API wijzigen. Zie voor meer informatie over het upgraden van API-versies [upgraden naar de nieuwste versie van de REST-API](search-api-migration.md) of [upgraden naar de nieuwste versie van .NET SDK](search-dotnet-sdk-migration.md).

## <a name="example-of-a-complex-structure"></a>Voorbeeld van een complexe structuur

De volgende JSON-document bestaat uit een eenvoudige en complexe velden. Complexe velden, zoals `Address` en `Rooms`, onderliggende velden hebben. `Address` heeft één set met waarden voor deze onderliggende velden, omdat het een enkel object in het document. Daarentegen `Rooms` heeft meerdere sets met waarden voor de onderliggende velden, één voor elk object in de verzameling.

```json
{
    "HotelId": "1",
    "HotelName": "Secret Point Motel",
    "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
    "Address": {
        "StreetAddress": "677 5th Ave",
        "City": "New York",
        "StateProvince": "NY"
    },
    "Rooms": [
        {
            "Description": "Budget Room, 1 Queen Bed (Cityside)",
            "Type": "Budget Room",
            "BaseRate": 96.99,
        },
        {
            "Description": "Deluxe Room, 2 Double Beds (City View)",
            "Type": "Deluxe Room",
            "BaseRate": 150.99,
        },
    ]
}
```

## <a name="creating-complex-fields"></a>Het maken van complexe velden

Als met een definitie van de index, kunt u de portal [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-index), of [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) om een schema met complexe typen te maken. 

Het volgende voorbeeld ziet een JSON-schema-index met eenvoudige velden, verzamelingen en complexe typen. U ziet dat binnen een complex type elk veld heeft een type en kan kenmerken hebben, net als op het hoogste niveau velden. Het schema komt overeen met het bovenstaande voorbeeldgegevens. `Address` is een complexe veld dat is niet een verzameling (een hotel heeft één adres). `Rooms` is een verzameling complexe veld (een hotel heeft veel ruimten).

<!---
For indexes used in a [push-model data import](search-what-is-data-import.md) strategy, where you are pushing a JSON data set to an Azure Search index, you can only have the basic syntax shown here: single complex types like `Address`, or a `Collection(Edm.ComplexType)` like `Rooms`. You cannot have complex types nested inside other complex types in an index used for push-model data ingestion.

Indexers are a different story. When defining an indexer, in particular one used to build a knowledge store, your index can have nested complex types. An indexer is able to hold a chain of complex data structures in-memory, and when it includes a skillset, it can support highly complex data forms. For more information and an example, see [How to get started with Knowledge Store](knowledge-store-howto.md).
-->

```json
{
    "name": "hotels",
    "fields": [
        {   "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true    },
        {   "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        {   "name": "Address", "type": "Edm.ComplexType",
            "fields": [{
                    "name": "StreetAddress",
                    "type": "Edm.String",
                    "filterable": false,
                    "sortable": false,
                    "facetable": false,
                    "searchable": true  },
                {
                    "name": "City",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
                {
                    "name": "StateProvince",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                }
            ]
        },
        {
            "name": "Rooms",
            "type": "Collection(Edm.ComplexType)",
            "fields": [{
                    "name": "Description",
                    "type": "Edm.String",
                    "searchable": true,
                    "analyzer": "en.lucene"
                },
                {
                    "name": "Type",
                    "type": "Edm.String",
                    "searchable": true
                },
                {
                    "name": "BaseRate",
                    "type": "Edm.Double",
                    "filterable": true,
                    "facetable": true
                },
            ]
        }
    ]
}
```
## <a name="updating-complex-fields"></a>Complexe velden bijwerken

Alle van de [opnieuw indexeren regels](search-howto-reindex.md) die gelden voor velden in het algemeen nog steeds van toepassing op complexe velden. Anders formuleren van enkele van de belangrijkste regels hier een indexen niet is vereist wanneer u een veld toe te voegen, maar de meeste wijzigingen doen.

### <a name="structural-updates-to-the-definition"></a>Structurele updates voor de definitie

U kunt nieuwe onderliggende velden toevoegen aan een complexe veld op elk gewenst moment zonder de noodzaak voor het opnieuw opbouwen van een index. Bijvoorbeeld, "Postcode" toe te voegen aan `Address` of 'Faciliteiten' naar `Rooms` is toegestaan, net zoals het toevoegen van een veld op het hoogste niveau naar een index. Bestaande documenten hebben een null-waarde voor de nieuwe velden totdat u deze velden expliciet vullen met het bijwerken van uw gegevens.

U ziet dat binnen een complex type elk veld heeft een type en kan kenmerken hebben, net als op het hoogste niveau velden

### <a name="data-updates"></a>Gegevensupdates

Bijwerken van bestaande documenten in een index met de actie uploaden werkt op dezelfde manier voor complexe en eenvoudige velden--alle velden zijn vervangen. Echter, samenvoegen (of mergeOrUpload wanneer toegepast op een bestaand document) werkt niet hetzelfde zijn in alle velden. Specifiek, hoeft samenvoegen niet de mogelijkheid om samen te voegen elementen in een verzameling. Dit geldt voor verzamelingen van primitieve typen, evenals complexe verzamelingen. Voor het bijwerken van een verzameling, nodig hebt u om op te halen van de waarde van de volledige verzameling, wijzigingen aanbrengen, en vervolgens de nieuwe verzameling kunt opnemen in de Index API-aanvraag.


## <a name="searching-complex-fields"></a>Zoeken naar complexe velden

Uitdrukkingen zoeken met vrije vorm werkt zoals verwacht met complexe typen. Als overeenkomt met elke doorzoekbaar veld of de onderliggende veld overal in een document, is het document zelf een overeenkomst. 

Query's krijgen meer genuanceerde wanneer u meerdere voorwaarden en operators, en enkele termen veldnamen die zijn opgegeven hebben, als er mogelijk is met de [Lucene syntaxis](query-lucene-syntax.md). Bijvoorbeeld: deze query probeert twee termen, 'Rotterdam' en 'Of', op basis van twee onderliggende velden van het veld adres:

```json
search=Address/City:Portland AND Address/State:OR
```

Query's, zoals deze zijn niet-gerelateerde voor zoeken in volledige tekst (in tegenstelling tot filters, waar query's van de onderliggende velden van een complexe verzameling kunnen worden gecorreleerd met behulp van een of alle, zoals een gecorreleerde subquery in SQL). Dit betekent dat de bovenstaande Lucene-query documenten retourneert "Portland, Groningen ' gemarkeerd met evenals 'Portland, Oregon', en andere plaatsen in Oregon. Dit is omdat elke-component wordt geëvalueerd op basis van alle waarden van het opgegeven veld in het hele document, zodat er geen concept van een document"huidige subregio'. 

 

## <a name="selecting-complex-fields"></a>Complexe velden selecteren

De `$select` parameter wordt gebruikt om te kiezen welke velden worden weergegeven in de lijst met zoekresultaten. Voor het gebruik van deze parameter om bepaalde onderliggende velden van een complexe veld te selecteren, zijn onder andere de veld van de bovenliggende en onderliggende veld gescheiden door een schuine streep (`/`).

```json
$select=HotelName, Address/City, Rooms/BaseRate
```

Velden moet worden gemarkeerd als ophalen mogelijk in de index, als u wilt dat ze in de zoekresultaten. Alleen de velden die zijn gemarkeerd als ophalen mogelijk kunnen worden gebruikt in een `$select` instructie. 


## <a name="filter-facet-and-sort-complex-fields"></a>Filter, facet en complexe velden sorteren

Dezelfde [syntaxis voor OData-pad](query-odata-filter-orderby-syntax.md) gebruikt om te filteren en fielded zoekopdrachten kunnen ook worden gebruikt voor facetten, sorteren en velden te selecteren in een zoekaanvraag. Voor complexe typen toepassing regels die bepalen welke onderliggende velden worden gemarkeerd als sorteerbaar of geschikt voor facetten. 

### <a name="faceting-sub-fields"></a>De onderliggende velden op meerdere niveaus 

Een veld kan worden gemarkeerd als geschikt voor facetten, tenzij deze van het type is `Edm.GeographyPoint` of `Collection(Edm.GeographyPoint)`. 

Wanneer het aantal documenten worden geretourneerd voor de meervoudige navigatie-structuur, zijn de aantallen ten opzichte van het bovenliggende document (een hotel), niet in geneste documenten binnen een complexe verzameling (ruimten). Stel bijvoorbeeld dat een hotel is 20 ruimten van het type 'suite'. Deze facetparameter gegeven `facet=Rooms/Type`, het aantal facet worden één voor de bovenliggende document (hotels) en niet tussenliggende onderliggende documenten (ruimten). 

### <a name="sorting-complex-fields"></a>Complexe velden sorteren

Bewerkingen sorteren van toepassing op (Hotels) en geen onderliggende documenten (ruimten). Wanneer u een verzameling complexe type, zoals ruimten hebt, is het Houd er rekening mee dat u kan niet worden gesorteerd op ruimten helemaal. U kunt niet in feite sorteren op een verzameling. 

Bewerkingen sorteren werkt wanneer velden één waarde zijn, of als een eenvoudige veldverwijzing of als een veld in een complex type. Bijvoorbeeld, `$orderby=Address/ZipCode` complex type is sorteerbaar omdat er slechts één postcode per hotel. 

Anders formuleren van de regels om te sorteren, binnen een indexveld moet worden gemarkeerd als filteren mogelijk en sorteerbaar moet worden gebruikt een `$orderby` instructie. 

## <a name="next-steps"></a>Volgende stappen

 Probeer de [Hotels gegevensset](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) in de **gegevens importeren** wizard. U moet de Cosmos DB-verbindingsgegevens opgegeven in het Leesmij-bestand voor toegang tot de gegevens. 
 
 Met die informatie beschikt is de eerste stap in de wizard het maken van een nieuwe Azure Cosmos DB-gegevensbron. Verder ziet op in de wizard, wanneer u op de doel-index-pagina, u een index met complexe typen. Maken en deze index laden en uitvoeren van query's voor meer informatie over de nieuwe structuur.

> [!div class="nextstepaction"]
> [Snelstartgids: portal-wizard voor het importeren, indexeren en query 's](search-get-started-portal.md)