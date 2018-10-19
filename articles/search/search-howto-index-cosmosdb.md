---
title: Indexeren van een Azure Cosmos DB-gegevensbron voor Azure Search | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een indexeerfunctie Azure Search maken met een Azure Cosmos DB-gegevensbron.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
robot: noindex
ms.openlocfilehash: c74563ba98835403f12a4df048d7ff358014b826
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406667"
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Cosmos DB verbinden met Azure Search met behulp van indexeerfuncties

In dit artikel leert hoe u:

> [!div class="checklist"]
> * Configureer [Azure Search-indexeerfunctie](search-indexer-overview.md) die gebruikmaakt van een Azure Cosmos DB-verzameling als een gegevensbron.
> * Een search-index maken met de gegevenstypen compatibel is met JSON.
> * Een indexeerfunctie voor on-demand en terugkerende indexeren configureren.
> * De index op basis van wijzigingen in de onderliggende gegevens incrementeel vernieuwen

> [!NOTE]
> Azure Cosmos DB is de volgende generatie van DocumentDB. Hoewel de productnaam wordt gewijzigd, de `documentdb` -syntaxis in Azure Search-indexeerfuncties nog steeds bestaat voor achterwaartse compatibiliteit in de Azure Search API's en de portal-pagina's. Bij het configureren van indexeerfuncties, moet u opgeven de `documentdb` syntaxis volgens de instructies in dit artikel.

In de volgende video Azure Cosmos DB Program Manager Andrew Liu ziet u hoe u een Azure Search-index toevoegen aan een Azure Cosmos DB-container.

>[!VIDEO https://www.youtube.com/embed/OyoYu1Wzk4w]

<a name="supportedAPIs"></a>
## <a name="supported-api-types"></a>Ondersteunde API-typen

Hoewel Azure Cosmos DB een groot aantal gegevensmodellen en API's ondersteunt, breidt de ondersteuning voor productie van Azure Search-indexeerfunctie alleen de SQL-API. Ondersteuning voor MongoDB-API is momenteel in openbare preview.  

Ondersteuning voor aanvullende API's is binnenkort. Om te helpen we prioriteit geven aan welke u moet eerst worden ondersteund, neem uw stem op de website van User Voice cast-conversie:

* [Tabel-API-ondersteuning voor gegevensbronnen](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Ondersteuning voor gegevensbronnen Graph API](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [Ondersteuning voor gegevensbronnen Apache Cassandra-API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Vereisten

Naast een Cosmos DB-account, moet u beschikken over een [Azure Search-service](search-create-service-portal.md). 

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Concepten van de indexeerfunctie Azure Search

Een **gegevensbron** Hiermee geeft u de gegevens naar de index, referenties en beleidsregels voor het identificeren van wijzigingen in de gegevens (zoals gewijzigde of verwijderde documenten in uw verzameling). De gegevensbron wordt gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexeerfunctie** wordt beschreven hoe de gegevens worden overgebracht uit uw gegevensbron naar een doel search-index. Een indexeerfunctie kan worden gebruikt voor:

* Uitvoeren van een momentopname van de gegevens naar een index te vullen.
* Een index met wijzigingen in de gegevensbron volgens een schema worden gesynchroniseerd.
* Op aanvraag updates naar een index indien nodig worden aangeroepen.

Als u een Azure Cosmos DB-indexeerfunctie instelt, moet u een index, gegevensbron en ten slotte de indexeerfunctie maken. U kunt deze objecten met behulp van de [portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), of [REST-API](/rest/api/searchservice/). 

In dit artikel laat zien hoe de REST-API gebruikt. Als u ervoor voor de portal kiezen, de [wizard gegevens importeren](search-import-data-portal.md) begeleidt u bij het maken van al deze resources, met inbegrip van de index.

> [!TIP]
> U kunt de wizard **Gegevens importeren** laden vanuit het dashboard Azure Cosmos DB om indexering voor die gegevensbron te vereenvoudigen. Ga in het linkernavigatiedeelvenster naar **Verzamelingen** > **Azure Search toevoegen** om aan de slag te gaan.

> [!NOTE] 
> Nu u kunt maken of bewerken **MongoDB** gegevensbronnen met behulp van Azure Portal of de .NET SDK. Echter, u **kunt** uitvoeringsgeschiedenis van MongoDB indexeerfuncties in de portal controleren.  

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Stap 1: een gegevensbron maken
Voer een bericht voor het maken van een gegevensbron:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCollection", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

De hoofdtekst van de aanvraag bevat de definitie van de gegevensbron, waaronder de volgende velden moet:

* **naam**: Kies een naam voor uw database.
* **type**: moet `documentdb`.
* **referenties**:
  
  * **connectionString**: vereist. De verbindingsgegevens van de met uw Azure Cosmos DB-database opgeven in de volgende indeling: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>` voor MongoDB-verzamelingen toevoegen **API-soort MongoDb =** op de verbindingstekenreeks: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`
  Vermijd poortnummers in de eindpunt-url. Als u het poortnummer opgeeft, worden Azure Search kan geen index van uw Azure Cosmos DB-database.
* **container**:
  
  * **naam**: vereist. Geef de id van de databaseverzameling worden geïndexeerd.
  * **query**: optioneel. U kunt een query voor het samenvoegen van een willekeurige JSON-document in een vast schema dat Azure Search kunt indexeren. Query's worden niet ondersteund voor MongoDB-verzamelingen. 
* **dataChangeDetectionPolicy**: aanbevolen. Zie [gewijzigd documenten te indexeren](#DataChangeDetectionPolicy) sectie.
* **dataDeletionDetectionPolicy**: optioneel. Zie [verwijderd documenten te indexeren](#DataDeletionDetectionPolicy) sectie.

### <a name="using-queries-to-shape-indexed-data"></a>Met behulp van query's op vorm geïndexeerde gegevens
U kunt een SQL-query voor het samenvoegen van geneste eigenschappen of matrices, project JSON-eigenschappen opgeven, en filter de gegevens moeten worden geïndexeerd. 

> [!WARNING]
> Aangepaste query's worden niet ondersteund voor **MongoDB** verzamelingen: `container.query` parameter moet worden ingesteld op null of wordt weggelaten. Als u nodig hebt om een aangepaste query te gebruiken, laat het ons weten op [User Voice](https://feedback.azure.com/forums/263029-azure-search).

Voorbeelddocument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Filterquery:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Plat met het maken van de query:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projectie-query:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Matrix afvlakken query:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Stap 2: een index maken
Een doel-Azure Search-index maken als u er nog geen hebt. U kunt maken met een index met behulp van de [gebruikersinterface van Azure portal](search-create-index-portal.md), wordt de [Index REST-API maken](/rest/api/searchservice/create-index) of [Index-klasse](/dotnet/api/microsoft.azure.search.models.index).

Het volgende voorbeeld wordt een index met een veld-id en -beschrijving:

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Zorg ervoor dat het schema van de doelindex compatibel met het schema van de bron-JSON-documenten of de uitvoer van uw aangepaste query-projectie is.

> [!NOTE]
> Voor gepartitioneerde verzamelingen, wordt de documentsleutel standaard van Azure Cosmos DB `_rid` eigenschap, die Azure Search automatisch wordt de naam van `rid` omdat veldnamen mag niet met een teken undescore beginnen. Ook Azure Cosmos DB `_rid` waarden bevatten tekens die ongeldig in Azure Search-sleutels zijn. Om deze reden de `_rid` waarden zijn Base64-gecodeerd.
> 
> Voor de MongoDB-collecties, Azure Search automatisch wijzigt de naam van de `_id` eigenschap `doc_id`.  

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Toewijzing tussen JSON-gegevenstypen en Azure Search-gegevenstypen
| JSON-gegevenstype | Veldtypen compatibel target-index |
| --- | --- |
| BOOL |Edm.Boolean, Edm.String |
| Cijfers die lijkt op gehele getallen |Edm.Int32, Edm.Int64, Edm.String |
| Getallen die eruit drijvende-punten |Edm.Double, Edm.String |
| Reeks |Edm.String |
| Matrices met primitieve typen, bijvoorbeeld ["a", "b", "c"] |Collection(EDM.String) |
| Tekenreeksen die lijkt op datums |Edm.DateTimeOffset, Edm.String |
| GeoJSON-objecten, bijvoorbeeld {"type": "Punt", "coördinaten": [lang zijn en lat]} |Edm.GeographyPoint |
| Andere JSON-objecten |N/A |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>Stap 3: Een indexeerfunctie maken

Zodra de index en gegevensbron zijn gemaakt, bent u klaar om te maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt uitgevoerd elke twee uur (schema-interval is ingesteld op 'PT2H'). Als u wilt een indexeerfunctie om de 30 minuten uitvoeren, moet u het interval aan 'PT30M' instellen. Het kortste ondersteunde interval is 5 minuten. Het schema is optioneel: als u dit weglaat, een indexeerfunctie wordt uitgevoerd slechts eenmaal wanneer deze gemaakt. U kunt echter een indexeerfunctie op aanvraag uitvoeren op elk gewenst moment.   

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Indexeerfunctie op aanvraag
Naast het periodiek uitgevoerd volgens een schema, kan ook een indexeerfunctie worden aangeroepen op aanvraag:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2017-11-11
    api-key: [Search service admin key]

> [!NOTE]
> Wanneer uitvoeren API is geretourneerd, het aanroepen van de indexeerfunctie is gepland, maar de werkelijke verwerking verloopt asynchroon. 

U kunt de status van de indexeerfunctie in de portal of met behulp van de ophalen indexeerfunctie Status API, die we vervolgens beschrijven bewaken. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Status van de indexeerfunctie ophalen
U kunt de status en uitvoering van de geschiedenis van een indexeerfunctie ophalen:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2017-11-11
    api-key: [Search service admin key]

Het antwoord bevat de algemene status van de indexeerfunctie, het laatste (of in uitvoering) indexer-aanroepen en de geschiedenis van recente indexeerfunctie aanroepen.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Uitvoeringsgeschiedenis bevat maximaal de 50 meest recente voltooide uitvoeringen die in omgekeerde volgorde worden gesorteerd (zodat de meest recente uitvoering in het antwoord eerste komt).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Gewijzigde documenten te indexeren
Het doel van een detectiebeleid voor wijzigingen van gegevens is efficiënt gewijzigde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `High Water Mark` beleid met behulp van de `_ts` eigenschap (timestamp) geleverd door Azure Cosmos DB, dat is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Met dit beleid wordt sterk aanbevolen om ervoor te zorgen goede indexeerfunctie prestaties. 

Als u een aangepaste query gebruikt, zorg ervoor dat de `_ts` eigenschap door de query wordt geprojecteerd.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Incrementele voortgang en aangepaste query 's
Incrementele voortgang tijdens het indexeren zorgt ervoor dat als de uitvoering van de indexeerfunctie wordt onderbroken door tijdelijke fouten en zonder tijdslimiet voor uitvoering, de indexeerfunctie verder kan gaan waar deze volgende keer dat deze wordt uitgevoerd afgebroken, in plaats van de hele verzameling helemaal opnieuw. Dit is vooral belangrijk bij het indexeren van grote verzamelingen. 

Zorg ervoor dat uw query de resultaten op basis van orders om in te schakelen incrementele voortgang bij het gebruik van een aangepaste query, de `_ts` kolom. Hierdoor kunnen periodieke controle wijst die Azure Search gebruikt voor incrementele voortgang in geval van problemen.   

In sommige gevallen, zelfs als de query bevat een `ORDER BY [collection alias]._ts` -component, Azure Search kan niet afleiden dat de query wordt gesorteerd op de `_ts`. U kunt Azure Search zien dat de resultaten worden gerangschikt met behulp van de `assumeOrderByHighWaterMarkColumn` configuratie-eigenschap. Als u deze hint, maken of bijwerken van de indexeerfunctie als volgt: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Documenten te indexeren verwijderd
Wanneer rijen zijn verwijderd uit de verzameling, wilt u normaal gesproken de rijen verwijderen uit de search-index. Het doel van een beleid verwijderen detecteren is efficiënt verwijderde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `Soft Delete` beleid (verwijdering is gemarkeerd met een vlag), is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Als u een aangepaste query gebruikt, zorgt u ervoor dat de eigenschap waarnaar wordt verwezen door `softDeleteColumnName` door de query wordt geprojecteerd.

Het volgende voorbeeld wordt een gegevensbron met een beleid voor voorlopig verwijderen:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Volgende stappen
Gefeliciteerd! U hebt geleerd hoe u Azure Cosmos DB integreert met Azure Search met behulp van een indexeerfunctie.

* Zie voor meer informatie over Azure Cosmos DB, de [Azure Cosmos DB-servicepagina](https://azure.microsoft.com/services/cosmos-db/).
* Zie voor meer informatie over Azure Search, de [pagina zoekservice](https://azure.microsoft.com/services/search/).
