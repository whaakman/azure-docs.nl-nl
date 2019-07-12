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
ms.date: 06/13/2019
ms.custom: seodec2018
ms.openlocfilehash: e7e6ddefd13d669c949389bc4fad85fb6cff4d3a
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621363"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Hoe u complexe gegevenstypen modelleren in Azure Search

Externe gegevenssets die worden gebruikt om een Azure Search-index te vullen kunnen worden geleverd in vele vormen. Ze bevatten soms hiërarchische of geneste substructuren. Voorbeelden mogelijk meerdere adressen bevatten voor één klant, meerdere kleuren en formaten voor een enkele SKU, meerdere auteurs van een enkel rapport, enzovoort. Voorwaarden modelleren, ziet u deze structuren aangeduid als *complexe*, *samengestelde*, *samengestelde*, of *cumulatieve* gegevenstypen. De term Azure Search voor dit concept gebruikt is **complexe type**. In Azure Search, complexe typen worden gemodelleerd met behulp van **complexe velden**. Een complexe veld is een veld dat onderliggende objecten (onderliggende velden) die van elk gegevenstype zijn kunnen, met inbegrip van andere complexe typen bevat. Dit werkt op dezelfde manier als de typen gestructureerde gegevens in een programmeertaal.

Complexe velden vertegenwoordigen een enkel object in het document of een matrix met objecten, afhankelijk van het gegevenstype. Velden van het type `Edm.ComplexType` vertegenwoordigen één objecten, terwijl de velden van het type `Collection(Edm.ComplexType)` matrices van objecten te vertegenwoordigen.

Azure Search biedt systeemeigen ondersteuning voor complexe typen en verzamelingen. Deze typen kunnen u als model voor bijna elk JSON-structuur in een Azure Search-index. In eerdere versies van Azure Search API's alleen afgevlakt rij sets kunnen worden geïmporteerd. In de nieuwste versie, de index kan nu beter aansluiten komen overeen met brongegevens. Met andere woorden, als de brongegevens complexe typen bevat, uw index kunt complexe typen hebben ook.

Als u wilt beginnen, wordt aangeraden de [Hotels gegevensset](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md), die u kunt laden de **gegevens importeren** wizard in de Azure-portal. De wizard detecteert complexe typen in de bron en stelt een indexschema op basis van de gedetecteerde structuren.

> [!Note]
> Ondersteuning voor complexe typen is algemeen beschikbaar in `api-version=2019-05-06`. 
>
> Als uw search-oplossing is gebouwd op eerdere oplossingen van platte gegevenssets in een verzameling, moet u uw index om op te nemen van complexe typen zoals ondersteund in de nieuwste versie van de API wijzigen. Zie voor meer informatie over het upgraden van API-versies [upgraden naar de nieuwste versie van de REST-API](search-api-migration.md) of [upgraden naar de nieuwste versie van .NET SDK](search-dotnet-sdk-migration-version-9.md).

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
      "BaseRate": 96.99
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99
    },
  ]
}
```

## <a name="creating-complex-fields"></a>Het maken van complexe velden

Als met een definitie van de index, kunt u de portal [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-index), of [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) om een schema met complexe typen te maken. 

Het volgende voorbeeld ziet een JSON-schema-index met eenvoudige velden, verzamelingen en complexe typen. U ziet dat binnen een complex type elk veld heeft een type en kan kenmerken hebben, net als op het hoogste niveau velden. Het schema komt overeen met het bovenstaande voorbeeldgegevens. `Address` is een complexe veld die niet van een verzameling (een hotel heeft één adres). `Rooms` is een verzameling complexe veld (een hotel heeft veel ruimten).

<!---
For indexes used in a [push-model data import](search-what-is-data-import.md) strategy, where you are pushing a JSON data set to an Azure Search index, you can only have the basic syntax shown here: single complex types like `Address`, or a `Collection(Edm.ComplexType)` like `Rooms`. You cannot have complex types nested inside other complex types in an index used for push-model data ingestion.

Indexers are a different story. When defining an indexer, in particular one used to build a knowledge store, your index can have nested complex types. An indexer is able to hold a chain of complex data structures in-memory, and when it includes a skillset, it can support highly complex data forms. For more information and an example, see [How to get started with knowledge store](knowledge-store-howto.md).
-->

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>Complexe velden bijwerken

Alle van de [opnieuw indexeren regels](search-howto-reindex.md) die gelden voor velden in het algemeen nog steeds van toepassing op complexe velden. Anders formuleren van enkele van de belangrijkste regels hier toevoegen van een veld een index opnieuw opbouwen niet nodig, maar de meeste wijzigingen doen.

### <a name="structural-updates-to-the-definition"></a>Structurele updates voor de definitie

U kunt nieuwe onderliggende velden toevoegen aan een complexe veld op elk gewenst moment zonder de noodzaak voor het opnieuw opbouwen van een index. Bijvoorbeeld, "Postcode" toe te voegen aan `Address` of 'Faciliteiten' naar `Rooms` is toegestaan, net zoals het toevoegen van een veld op het hoogste niveau naar een index. Bestaande documenten hebben een null-waarde voor de nieuwe velden totdat u deze velden expliciet vullen met het bijwerken van uw gegevens.

U ziet dat binnen een complex type elk veld heeft een type en kan kenmerken hebben, net als op het hoogste niveau velden

### <a name="data-updates"></a>Gegevensupdates

Bijwerken van bestaande documenten in een index met de `upload` actie werkt op dezelfde manier voor complexe en eenvoudige velden--alle velden zijn vervangen. Echter, `merge` (of `mergeOrUpload` wanneer toegepast op een bestaand document) werkt niet hetzelfde voor alle velden. Met name `merge` biedt geen ondersteuning voor samenvoegen elementen in een verzameling. Deze beperking bestaat voor verzamelingen van primitieve typen en complexe verzamelingen. Voor het bijwerken van een verzameling, nodig hebt u om op te halen van de waarde van de volledige verzameling, wijzigingen aanbrengen, en vervolgens de nieuwe verzameling kunt opnemen in de Index API-aanvraag.

## <a name="searching-complex-fields"></a>Zoeken naar complexe velden

Uitdrukkingen zoeken met vrije vorm werkt zoals verwacht met complexe typen. Als overeenkomt met elke doorzoekbaar veld of de onderliggende veld overal in een document, is het document zelf een overeenkomst.

Query's krijgen meer genuanceerde wanneer u meerdere voorwaarden en operators, en enkele termen veldnamen die zijn opgegeven hebben, als er mogelijk is met de [Lucene syntaxis](query-lucene-syntax.md). Bijvoorbeeld: deze query probeert twee termen, 'Rotterdam' en 'Of', op basis van twee onderliggende velden van het veld adres:

    search=Address/City:Portland AND Address/State:OR

Query's, zoals deze zijn *niet-gerelateerde* voor zoeken in volledige tekst, in tegenstelling tot filters. In de filters, query's van de onderliggende velden van een complexe verzameling worden gecorreleerd met het gebruik van variabelen in bereik [ `any` of `all` ](search-query-odata-collection-operators.md). De bovenstaande Lucene-query retourneert documenten met zowel 'Portland, Groningen ' gemarkeerd als "Portland, Oregon", samen met andere plaatsen in Oregon. Dit gebeurt omdat elke-component is van toepassing op alle waarden van het veld in het hele document, zodat er geen concept van een document"huidige subregio'. Zie voor meer informatie over dit [Understanding OData verzameling filters in Azure Search](search-query-understand-collection-filters.md).

## <a name="selecting-complex-fields"></a>Complexe velden selecteren

De `$select` parameter wordt gebruikt om te kiezen welke velden worden weergegeven in de lijst met zoekresultaten. Voor het gebruik van deze parameter om bepaalde onderliggende velden van een complexe veld te selecteren, zijn onder andere de veld van de bovenliggende en onderliggende veld gescheiden door een schuine streep (`/`).

    $select=HotelName, Address/City, Rooms/BaseRate

Velden moet worden gemarkeerd als ophalen mogelijk in de index, als u wilt dat ze in de zoekresultaten. Alleen de velden die zijn gemarkeerd als ophalen mogelijk kunnen worden gebruikt in een `$select` instructie.

## <a name="filter-facet-and-sort-complex-fields"></a>Filter, facet en complexe velden sorteren

Dezelfde [syntaxis voor OData-pad](query-odata-filter-orderby-syntax.md) gebruikt om te filteren en fielded zoekopdrachten kunnen ook worden gebruikt voor facetten, sorteren en velden te selecteren in een zoekaanvraag. Voor complexe typen toepassing regels die bepalen welke onderliggende velden worden gemarkeerd als sorteerbaar of geschikt voor facetten. Zie voor meer informatie over deze regels de [maken Index API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/create-index#request).

### <a name="faceting-sub-fields"></a>De onderliggende velden op meerdere niveaus

Een veld kan worden gemarkeerd als geschikt voor facetten, tenzij deze van het type is `Edm.GeographyPoint` of `Collection(Edm.GeographyPoint)`.

Het aantal documenten die worden geretourneerd in de resultaten facet worden berekend voor de bovenliggende document (een hotel), niet de onderliggende documenten in een complexe verzameling (ruimten). Stel bijvoorbeeld dat een hotel is 20 ruimten van het type 'suite'. Deze facetparameter gegeven `facet=Rooms/Type`, het aantal facet worden één voor het hotel, niet 20 voor de ruimten.

### <a name="sorting-complex-fields"></a>Complexe velden sorteren

Bewerkingen sorteren van toepassing op (Hotels) en geen onderliggende documenten (ruimten). Wanneer u een verzameling complexe type, zoals ruimten hebt, is het Houd er rekening mee dat u kan niet worden gesorteerd op ruimten helemaal. U kunt niet in feite sorteren op een verzameling.

Sorteren operations werken wanneer u velden hebben één waarde per document of het veld een eenvoudig veld of een veld in een complex type is. Bijvoorbeeld, `Address/City` mag worden gesorteerd, omdat er slechts één adres per hotel, dus `$orderby=Address/City` hotels per plaats te sorteren.

### <a name="filtering-on-complex-fields"></a>Filteren op complexe velden

U kunt verwijzen naar de onderliggende velden van een complexe veld in een filterexpressie. Gebruikt u dezelfde [syntaxis voor OData-pad](query-odata-filter-orderby-syntax.md) die wordt gebruikt voor facetten, sorteren en velden te selecteren. Het volgende filter retourneert bijvoorbeeld alle hotels in Canada:

    $filter=Address/Country eq 'Canada'

Als u wilt filteren op een verzameling complexe-veld, kunt u een **lambda-expressie** met de [ `any` en `all` operators](search-query-odata-collection-operators.md). In dat geval de **bereik variabele** van de lambda-expressie is een object met onderliggende velden. U kunt verwijzen naar de onderliggende velden met de standaard OData-syntaxis. Bijvoorbeeld, retourneert het volgende filter alle niet-soorten ruimten en alle hotels met ten minste één deluxe ruimte:

    $filter=Rooms/any(room: room/Type eq 'Deluxe Room') and Rooms/all(room: not room/SmokingAllowed)

Als met eenvoudige velden op het hoogste niveau, eenvoudige onderliggende velden van complexe velden kunnen alleen worden opgenomen in de filters voor als ze beschikken over de **Filterbaar** kenmerk ingesteld op `true` in het definitie van de index. Zie voor meer informatie de [maken Index API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/create-index#request).

## <a name="next-steps"></a>Volgende stappen

Probeer de [Hotels gegevensset](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) in de **gegevens importeren** wizard. U moet de Cosmos DB-verbindingsgegevens opgegeven in het Leesmij-bestand voor toegang tot de gegevens.

Met die informatie beschikt is de eerste stap in de wizard het maken van een nieuwe Azure Cosmos DB-gegevensbron. Verder ziet op in de wizard, wanneer u op de doel-index-pagina, u een index met complexe typen. Maken en deze index laden en uitvoeren van query's voor meer informatie over de nieuwe structuur.

> [!div class="nextstepaction"]
> [Snelstartgids: portal-wizard voor het importeren, indexeren en query 's](search-get-started-portal.md)