---
title: Een SQL-API van Azure Cosmos DB-gegevensbron indexeren voor Azure Search | Microsoft Docs
description: In dit artikel leest u hoe een Azure Search-indexeerfunctie maken met een gegevensbron Azure Cosmos-DB (SQL-API).
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 01/08/2018
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: e449f13adcd1a3651e1cac852b23f21d0227038a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Cosmos DB verbinding te maken met Azure Search met behulp van indexeerfuncties

[Azure Cosmos DB](../cosmos-db/introduction.md) database is globaal gedistribueerd en modellen van Microsoft. Met de [SQL-API](../cosmos-db/sql-api-introduction.md), Azure Cosmos DB biedt uitgebreide en vertrouwd SQL-query-mogelijkheden met consistente lage latenties ten opzichte van de JSON-gegevens zonder schema. Azure Search integreert naadloos met de SQL-API. U kunt pull-JSON-documenten rechtstreeks in een Azure Search-index met een [Azure Search-indexeerfunctie](search-indexer-overview.md), ontworpen specifiek voor Azure Cosmos DB SQL-API. 

In dit artikel leest u hoe:

> [!div class="checklist"]
> * Azure Search voor het gebruik van een API van Azure Cosmos DB SQL-database als een gegevensbron configureren. Geef desgewenst een query voor het selecteren van een subset.
> * Een search-index maken met de gegevenstypen compatibel is met JSON.
> * Configureer een indexeerfunctie voor op aanvraag en terugkerende indexering.
> * De index op basis van wijzigingen in de onderliggende gegevens stapsgewijs vernieuwen.

> [!NOTE]
> Azure Cosmos DB SQL-API is de volgende generatie van DocumentDB. Hoewel de productnaam is gewijzigd, de `documentdb` syntaxis in Azure Search indexeerfuncties nog bestaat voor achterwaartse compatibiliteit in de Azure Search API's en de portal-pagina's. Bij het configureren van indexeerfuncties moet opgeven de `documentdb` syntaxis volgens de instructies in dit artikel.

<a name="supportedAPIs"></a>

## <a name="supported-api-types"></a>Ondersteunde API-typen

Hoewel Azure Cosmos DB een groot aantal gegevensmodellen en API's ondersteunt, breidt de ondersteuning van de indexeerfunctie alleen de SQL-API. 

Ondersteuning voor aanvullende API's ontbreekt. Converteer op de website van de gebruiker stem zodat ons prioriteren welke u wilt ondersteunen eerst:

* [Tabel API data source-ondersteuning](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Ondersteuning voor Graph API gegevensbron](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [MongoDB API data source-ondersteuning](https://feedback.azure.com/forums/263029-azure-search/suggestions/18861421-documentdb-indexer-should-be-able-to-index-mongodb)
* [Apache Cassandra API data source-ondersteuning](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Vereisten

Als u een indexeerfunctie Azure Cosmos DB instelt, moet er een [Azure Search-service](search-create-service-portal.md), en maak een index, datasource en ten slotte de indexeerfunctie. Kunt u deze objecten met behulp van de [portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), of [REST-API](/rest/api/searchservice/) voor alle niet-.NET-talen. 

Als u ervoor voor de portal kiezen de [wizard gegevens importeren](search-import-data-portal.md) begeleidt u bij het maken van al deze bronnen, met inbegrip van de index.

> [!TIP]
> U kunt de wizard **Gegevens importeren** laden vanuit het dashboard Azure Cosmos DB om indexering voor die gegevensbron te vereenvoudigen. Ga in het linkernavigatiedeelvenster naar **Verzamelingen** > **Azure Search toevoegen** om aan de slag te gaan.

<a name="Concepts"></a>

## <a name="azure-search-indexer-concepts"></a>Azure Search-indexeerfunctie-concepten
Azure Search ondersteunt het maken en beheren van gegevens gegevensbronnen (inclusief Azure Cosmos DB SQL-API) en indexeerfuncties die op basis van die gegevensbronnen werken.

Een **gegevensbron** Hiermee geeft u de gegevens naar de index, referenties en beleidsregels voor het identificeren van wijzigingen in de gegevens (zoals gewijzigde of verwijderde documenten binnen uw verzameling). De gegevensbron is gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexeerfunctie** wordt beschreven hoe de gegevens uit uw gegevensbron loopt in een doel search-index. Een indexeerfunctie kan worden gebruikt voor:

* Uitvoeren van een momentopname van de gegevens naar een index te vullen.
* Een index met wijzigingen in de gegevensbron volgens een schema worden gesynchroniseerd. De planning maakt deel uit van de indexeerfunctie-definitie.
* Updates op aanvraag naar een index zo nodig worden aangeroepen.

<a name="CreateDataSource"></a>

## <a name="step-1-create-a-data-source"></a>Stap 1: een gegevensbron maken
U kunt een POST een gegevensbron maken:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

De hoofdtekst van de aanvraag bevat de definitie van de gegevensbron, waaronder de volgende velden moet:

* **naam**: Kies een naam voor uw database.
* **type**: moet `documentdb`.
* **referenties**:
  
  * **connectionString**: vereist. Geef de verbindingsgegevens van de met uw Azure DB die Cosmos-database in de volgende indeling:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **container**:
  
  * **naam**: vereist. Geef de id van de verzameling van de database worden geïndexeerd.
  * **query**: optioneel. U kunt een query om een willekeurige JSON-document in een platte schema die Azure Search kunt indexeren plat opgeven.
* **dataChangeDetectionPolicy**: aanbevolen. Zie [gewijzigd documenten te indexeren](#DataChangeDetectionPolicy) sectie.
* **dataDeletionDetectionPolicy**: optioneel. Zie [verwijderd documenten te indexeren](#DataDeletionDetectionPolicy) sectie.

### <a name="using-queries-to-shape-indexed-data"></a>Met behulp van query's naar vorm geïndexeerde gegevens
U kunt een SQL-query voor plat geneste eigenschappen of matrices, project JSON-eigenschappen opgeven en filter de gegevens moeten worden geïndexeerd. 

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

Plat query:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projectie-query:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Matrix afvlakken query:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Stap 2: een index maken
Een doel-Azure Search-index maken als u nog niet hebt. Kunt u een index met behulp van de [gebruikersinterface van Azure portal](search-create-index-portal.md), wordt de [Index REST-API maken](/rest/api/searchservice/create-index) of [Index klasse](/dotnet/api/microsoft.azure.search.models.index).

Het volgende voorbeeld maakt een index met een veld-id en -beschrijving:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
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

Zorg ervoor dat het schema van de doelindex compatibel met het schema van de JSON-brondocumenten of de uitvoer van uw aangepaste query-projectie is.

> [!NOTE]
> Voor gepartitioneerde verzamelingen is de standaardsleutel document Azure Cosmos DB `_rid` eigenschap, die wordt gewijzigd in `rid` in Azure Search. Ook Azure Cosmos DB van `_rid` waarden bevatten tekens die ongeldig in Azure Search-sleutels zijn. Daarom moet de `_rid` waarden Base64-gecodeerd zijn.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Toewijzing tussen JSON-gegevenstypen en Azure Search-gegevenstypen
| JSON-gegevenstype | Compatibel doel index veldtypen |
| --- | --- |
| BOOL |Edm.Boolean, Edm.String |
| Cijfers die als gehele getallen eruitzien |Edm.Int32, Edm.Int64, Edm.String |
| Cijfers die zijn opgemaakt als drijvende-punten |Edm.Double, Edm.String |
| Tekenreeks |Edm.String |
| Matrices met primitieve typen, bijvoorbeeld ["a", "b", "c"] |Collection(EDM.String) |
| Tekenreeksen die lijken op datums |Edm.DateTimeOffset, Edm.String |
| GeoJSON-objecten, bijvoorbeeld {'type': 'Point', 'coordinates': [lang, lat]} |Edm.GeographyPoint |
| Andere JSON-objecten |N/A |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>Stap 3: Maak een indexeerfunctie

Als de index en gegevensbron hebt gemaakt, bent u klaar voor het maken van de indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexeerfunctie wordt uitgevoerd voor elke twee uur (schema-interval is ingesteld op 'PT2H'). Als u wilt uitvoeren met een indexeerfunctie elke 30 minuten, stelt u het interval voor 'PT30M'. De kortste interval is 5 minuten. Het schema is optioneel - als u dit weglaat, een indexeerfunctie slechts één keer is uitgevoerd wanneer deze gemaakt. U kunt echter een indexeerfunctie op aanvraag uitvoeren op elk gewenst moment.   

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Indexeerfunctie op aanvraag uitvoeren
Naast het periodiek wordt uitgevoerd op een planning, kan ook een indexeerfunctie worden aangeroepen op aanvraag:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Wanneer uitvoeren API is geactiveerd, wordt het oproepen van de indexeerfunctie is gepland, maar de daadwerkelijke verwerking asynchroon gebeurt. 

U kunt de status van de indexeerfunctie in de portal of met de ophalen indexeerfunctie Status API, die vervolgens worden beschreven bewaken. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Ophalen van de status van de indexeerfunctie
U kunt de geschiedenis van de status en de uitvoering van een indexeerfunctie ophalen:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Het antwoord bevat de algemene status van de indexeerfunctie, de aanroep van indexeerfunctie laatste (of wordt uitgevoerd) en de geschiedenis van recente indexeerfunctie aanroepen.

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

Uitvoergeschiedenis bevat tot de 50 meest recente voltooide uitvoering en in omgekeerde volgorde worden gesorteerd (zodat de meest recente uitvoering bovenaan in het antwoord).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Gewijzigde documenten te indexeren
Het doel van een beleid gegevens wijzigen detectie is efficiënt gewijzigde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `High Water Mark` beleid voor gebruik van de `_ts` (tijdstempel van) de eigenschap verstrekt door Azure Cosmos DB, die is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Met dit beleid wordt nadrukkelijk aanbevolen indexeerfunctie goede prestaties te garanderen. 

Als u een aangepaste query gebruikt, zorg ervoor dat de `_ts` eigenschap wordt toegewezen door de query.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Voortgang van de incrementele en aangepaste query 's
Incrementele voortgang tijdens het indexeren zorgt ervoor dat als de uitvoering van de indexeerfunctie wordt onderbroken door tijdelijke fouten of uitvoeringstijd limiet, de indexeerfunctie kan worden opgepikt waar deze was gebleven volgende keer dat deze wordt uitgevoerd, in plaats van de volledige verzameling vanaf het begin opnieuw te indexeren. Dit is vooral belangrijk wanneer u grote verzamelingen. 

Schakel incrementele voortgang bij gebruik van een aangepaste query door ervoor te zorgen dat uw query orders de resultaten worden op de `_ts` kolom. Hierdoor kunnen periodieke controle wijst die Azure Search gebruikt om incrementele voortgang in geval van problemen.   

In sommige gevallen, zelfs als de query bevat een `ORDER BY [collection alias]._ts` component, Azure Search kan niet afleiden dat de query is besteld op het `_ts`. U kunt Azure Search zien dat de resultaten zijn gerangschikt met behulp van de `assumeOrderByHighWaterMarkColumn` configuratie-eigenschap. Geef deze hint, maken of bijwerken van de indexeerfunctie als volgt: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Documenten te indexeren verwijderd
Wanneer rijen uit de verzameling worden verwijderd, wilt u normaal gesproken verwijderen die rijen uit de search-index. Het doel van een gegevens-verwijderingsbeleid detectie is efficiënt verwijderde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `Soft Delete` beleid (verwijdering is gemarkeerd met een van de markering van sommige sorteren), die is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Als u een aangepaste query gebruikt, controleert u of de eigenschap waarnaar wordt verwezen door `softDeleteColumnName` wordt toegewezen door de query.

Het volgende voorbeeld maakt een gegevensbron met een beleid voor voorlopig verwijderen:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
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
Gefeliciteerd! U hebt geleerd hoe Azure Cosmos DB integreren met Azure Search met behulp van een indexeerfunctie verkennen en het uploaden van documenten vanuit een SQL-gegevensmodel.

* Zie voor meer informatie over Azure Cosmos DB, de [pagina van de service Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).
* Zie voor meer informatie over Azure Search, de [service zoekpagina](https://azure.microsoft.com/services/search/).
