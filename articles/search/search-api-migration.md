---
title: Upgraden naar de nieuwste versie in de Azure Search Service REST API - Azure Search
description: Verschillen in API-versies en informatie over welke acties zijn vereist voor het migreren van bestaande code naar de nieuwste versie van Azure Search Service REST API.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: 286d8bbc01b5916e842c196aed5a49ef1c76bc3c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025202"
---
# <a name="upgrade-to-the-latest-azure-search-service-rest-api-version"></a>Upgraden naar de nieuwste versie van Azure Search Service REST API
Als u een eerdere versie van de [Azure Search Service REST API](https://docs.microsoft.com/rest/api/searchservice/), in dit artikel ziet u een upgrade van uw toepassing met de nieuwste algemeen beschikbaar API-versie, 2019-05-06.

Versie 2019-05-06 van de REST-API bevat enkele wijzigingen van eerdere versies. Dit zijn meestal achterwaarts compatibel, zodat u uw code wijzigt, moet alleen minimale inspanning, afhankelijk van welke versie u hebt gebruikt vóór nodig. [Stappen voor het upgraden](#UpgradeSteps) geeft een overzicht van de wijzigingen in de code vereist voor het gebruik van nieuwe functies.

> [!NOTE]
> Een exemplaar van Azure Search-service ondersteunt een bereik van REST-API-versies, met inbegrip van eerdere overschrijven. U kunt echter ook doorgaan met deze API-versies, maar u kunt het beste uw code migreren naar de nieuwste versie zodat u toegang hebben tot de nieuwe mogelijkheden.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2019-05-06"></a>Wat is er nieuw in versie 2019-05-06
Versie 2019-05-06 is de nieuwste algemeen beschikbare versie van de Azure Search Service REST API. Functies die zijn overgebracht naar de status in het algemeen beschikbaar in deze API-versie zijn onder andere:

* [Automatisch aanvullen](index-add-suggesters.md) is een typeahead-functie die de invoer van een gedeeltelijk opgegeven term is voltooid.

* [Complexe typen](search-howto-complex-data-types.md) biedt systeemeigen ondersteuning voor gestructureerde objectgegevens in een Azure Search-index.

* [Parseren van de modi JsonLines](search-howto-index-json-blobs.md), onderdeel van Azure Blob indexeren, maakt u een search-document per JSON-entiteit die is gescheiden door een nieuwe regel.

* [Cognitieve zoekopdrachten](cognitive-search-concept-intro.md) indexeren die gebruikmaakt van de AI-verrijking-engines van Cognitive Services biedt.

Verschillende preview-versies van functie samenvallen met deze update algemeen beschikbaar. De lijst met nieuwe preview-functies, Zie [Search REST api-versie 2019-05-06-Preview](search-api-preview.md).

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

Bestaande code met de volgende functionaliteit wordt verbreken op api-version = 2019-05-06.

### <a name="indexer-for-azure-cosmos-db---datasource-is-now-type-cosmosdb"></a>Indexeerfunctie voor Azure Cosmos DB - gegevensbron is nu "type": "cosmosdb"

Als u een [Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md ), moet u `"type": "documentdb"` naar `"type": "cosmosdb"`.

### <a name="indexer-execution-result-errors-no-longer-have-status"></a>Resultaat van indexeerfunctie uitvoeringsfouten hoeft niet langer status

De structuur van de fout voor de uitvoering van de indexeerfunctie is eerder een `status` element. Dit element is verwijderd omdat deze is niet nuttige informatie verstrekken.

### <a name="indexer-data-source-api-no-longer-returns-connection-strings"></a>Gegevensbron indexer API retourneert niet meer verbindingsreeksen

Vanuit de API retourneert versies 2019-05-06- en 2019-05-06-Preview en hoger, de API van de gegevensbron niet meer verbindingsreeksen in het antwoord van een REST-bewerking. In eerdere versies van de API, voor gegevensbronnen die zijn gemaakt met behulp van boeken, Azure Search geretourneerd **201** gevolgd door de OData-antwoord dat de verbindingsreeks in tekst zonder opmaak die zijn opgenomen.

### <a name="named-entity-recognition-cognitive-skill-is-now-discontinued"></a>Met de naam van entiteit herkenning van cognitieve vaardigheden is nu niet meer beschikbaar

Als u aanroepen [naam entiteit erkenning](cognitive-search-skill-named-entity-recognition.md) vaardigheid in uw code de aanroep mislukt. Vervanging van de functionaliteit is [entiteit erkenning](cognitive-search-skill-entity-recognition.md). U moet mogelijk verwijzing moet worden vervangen de kwalificatie geen andere wijzigingen. De API-handtekening is hetzelfde voor beide versies. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Als u een upgrade van een eerdere versie van de algemene beschikbaarheid uitvoert, 2017-11-11- of 2016-09-01, u waarschijnlijk geen te wijzigen in uw code, anders dan het versienummer wijzigen. De enige situaties waarin u mogelijk nodig hebt om code te wijzigen zijn wanneer:

* Uw code mislukt wanneer het niet-herkende eigenschappen worden geretourneerd in een API-reactie. Standaard moet uw toepassing worden genegeerd eigenschappen die niet wordt herkend.

* Uw code zich blijft voordoen API-aanvragen en wil ze opnieuw te verzenden naar de nieuwe API-versie. Dit kan bijvoorbeeld gebeuren als uw toepassing zich blijft voortzetting van tokens die zijn geretourneerd door de Search-API voordoen (Zoek voor meer informatie, `@search.nextPageParameters` in de [Search API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Als een van deze situaties op u van toepassing, moet u mogelijk uw code dienovereenkomstig wijzigen. Anders geen wijzigingen nodig moeten zijn, tenzij u wilt beginnen met behulp van de [nieuwe functies](#WhatsNew) van 2019-05-06-versie.

Als u een van een preview-API-versie upgrade, de bovenstaande ook van toepassing, maar u wel rekening mee dat sommige preview-functies zijn niet beschikbaar in versie 2019-05-06:

* [Query's "Meer als dit"](search-more-like-this.md) nog steeds een alleen-preview-functie.

Als uw code maakt gebruik van deze functies, wordt het niet mogelijk om te upgraden naar API-versie 2019-05-06 zonder te verwijderen van uw gebruik van deze.

> [!IMPORTANT]
> Voorbeeld-API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving.
> 

### <a name="upgrading-complex-types"></a>Upgraden van complexe typen

Als u uw code complexe typen met de oudere versies preview-API 2017-11-11-Preview of 2016-09-01-Preview gebruikt, zijn er enkele nieuwe en gewijzigde beperkingen in versie 2019-05-06 hiervan u moet rekening mee moet houden:

+ De limieten voor de diepte van de onderliggende velden en het aantal complexe-verzamelingen per index verlaagd. Als u de indexen die groter zijn dan deze limieten met behulp van de preview-api-versies hebt gemaakt, wordt elke poging om te werken of deze opnieuw maken met behulp van API-versie 2019-05-06 mislukken. Als dit voor u geldt, moet u opnieuw wilt vormgeven, uw schema om te passen binnen de grenzen van de nieuwe en vervolgens de index opnieuw opbouwen.

+ Er is een nieuwe limiet in voor het aantal elementen van complexe-verzamelingen per document 2019-05-06-api-versie. Als u indexen met de documenten die groter zijn dan deze limieten met behulp van de preview-api-versies gemaakt, mislukken alle pogingen om te indexeren die gegevens met behulp van 2019-05-06-api-versie. Als dit voor u geldt, moet u het aantal verzameling complexe elementen per document beperken voordat het indexeren van uw gegevens.

Zie voor meer informatie, [limieten voor Azure Search-Service](search-limits-quotas-capacity.md).

### <a name="how-to-upgrade-an-old-complex-type-structure"></a>Upgrade uitvoeren van de typestructuur van een oude complex

Als uw code wordt complexe typen worden gebruikt met een van de oudere versies van de preview-API, gebruikt u mogelijk een index definition-indeling die er als uitzien volgt:

```json
{
  "name": "hotels",  
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.microsoft" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.microsoft" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true, "analyzer": "tagsAnalyzer" },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true },
    { "name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType" },
    { "name": "Address/StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
    { "name": "Address/City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)" }, 
    { "name": "Rooms/Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene" },
    { "name": "Rooms/Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "Rooms/Type", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
    { "name": "Rooms/BedOptions", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "Rooms/SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Rooms/Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "facetable": true, "analyzer": "tagsAnalyzer" }
  ]
}  
```

Een nieuwere boomstructuur indeling voor het definiëren van de indexvelden is geïntroduceerd in API-versie 2017-11-11-Preview. In de nieuwe indeling heeft elk complexe veld een verzameling van velden waar de onderliggende velden worden gedefinieerd. In versie 2019-05-06-API, deze nieuwe indeling uitsluitend wordt gebruikt en probeert te maken of bijwerken van een index met behulp van de oude notatie mislukken. Als u gemaakt met behulp van de oude notatie indexen hebt, moet u ze updaten naar de nieuwe indeling, voordat ze kunnen worden beheerd met behulp van API-versie 2019-05-06 met API-versie 2017-11-11-Preview.

U kunt "platte" indexen bijwerken naar de nieuwe indeling met de volgende stappen met behulp van API-versie 2017-11-11-Preview:

1. Voer een GET-aanvraag voor het ophalen van uw index. Als deze al in de nieuwe indeling, bent u klaar.

2. Vertalen van de index van de indeling 'vast' naar de nieuwe indeling. Hebt u code schrijven voor dit omdat er geen voorbeeldcode beschikbaar is op het moment van schrijven.

3. Voer een PUT-aanvraag naar de index bijwerken naar de nieuwe indeling. Zorg ervoor dat er geen andere gegevens van de index, zoals de doorzoekbaarheid/filterability van velden, niet wijzigen omdat dit is niet toegestaan door de Update Index-API.

> [!NOTE]
> Het is niet mogelijk voor het beheren van indexen die zijn gemaakt met de oude "platte" indeling van de Azure-portal. Werk uw indexen van de weergave 'vast' aan de ' boomstructuur "zo snel mogelijk.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie van Azure Search Service REST API-verwijzing. Als u problemen ondervindt, ons om hulp vragen op [StackOverflow](https://stackoverflow.com/) of [contact op met ondersteuning](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Search service REST API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/)

