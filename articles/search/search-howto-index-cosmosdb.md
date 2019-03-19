---
title: Een Azure Cosmos DB-gegevensbron - Azure Search-index
description: Een Azure Cosmos DB-gegevensbron verkennen en opnemen van gegevens in een doorzoekbare index met volledige tekst in Azure Search. Indexeerfuncties automatiseren opname van gegevens voor bepaalde gegevensbronnen, zoals Azure Cosmos DB.
ms.date: 02/28/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: dceabc799e187f3af56588d5a9008e5cdca517c0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57864453"
---
# <a name="how-to-index-cosmos-db-using-an-azure-search-indexer"></a>Indexeren van Cosmos DB met behulp van een Azure Search-indexeerfunctie

Dit artikel leest u hoe het configureren van een Azure Cosmos DB [indexeerfunctie](search-indexer-overview.md) inhoud uit te pakken en kunt u in Azure Search kan worden doorzocht. Deze werkstroom wordt gemaakt van een Azure Search-index en geladen met de bestaande tekst die is geëxtraheerd uit Azure Cosmos DB. 

Omdat terminologie kan verwarrend zijn, is het vermelden waard dat [Azure Cosmos DB indexeren](https://docs.microsoft.com/azure/cosmos-db/index-overview) en [Azure Search indexeren](search-what-is-an-index.md) afzonderlijke bewerkingen, zijn uniek voor elke service. Voordat u begint met Azure Search moet indexeren, uw Azure Cosmos DB-database al bestaan en gegevens bevatten.

U kunt de [portal](#cosmos-indexer-portal), REST-API's of .NET SDK om Cosmos-inhoud te indexeren. De Cosmos DB-indexeerfunctie in Azure Search kan worden verkend [Azure Cosmos-items](https://docs.microsoft.com/azure/cosmos-db/databases-containers-items#azure-cosmos-items) toegankelijk is via deze protocollen:

* [SQL-API](https://docs.microsoft.com/azure/cosmos-db/sql-api-query-reference) 
* [MongoDB-API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction) (Azure Search-ondersteuning voor deze API is in openbare preview-versie)  

> [!Note]
> Uservoice heeft bestaande artikelen voor aanvullende API-ondersteuning. U kunt een stem voor de Cosmos-API's die u graag zou willen zien in Azure Search ondersteunde cast-conversie uitvoeren: [Table-API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab), [Graph API](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4), [Apache Cassandra-API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu).
>

<a name="cosmos-indexer-portal"></a>

## <a name="use-the-portal"></a>Gebruik de portal

De eenvoudigste methode voor het indexeren van items van Azure Cosmos is het gebruik van een wizard in de [Azure-portal](https://portal.azure.com/). Door steekproeven van gegevens en het lezen van de metagegevens voor de container de [ **gegevens importeren** ](search-import-data-portal.md) wizard in Azure Search kunt maken van een standaardindex, wijzen bronvelden toe aan indexvelden doel en laden van de index in een enkel de bewerking. U kunt een operationele volledige tekst search-index in minuten hebben, afhankelijk van de grootte en complexiteit van de brongegevens.

Het is raadzaam om met behulp van hetzelfde Azure-abonnement voor Azure Search en Azure Cosmos DB, bij voorkeur in dezelfde regio.

### <a name="1---prepare-source-data"></a>1 - brongegevens voorbereiden

U hebt een Cosmos-account, een Azure Cosmos-database die is toegewezen aan de SQL-API of MongoDB-API en een container van JSON-documenten. 

Zorg ervoor dat uw Cosmos DB-database gegevens bevat. De [wizard gegevens importeren](search-import-data-portal.md) metagegevens leest en voert steekproeven te nemen aan het afleiden van een indexschema, maar ook geladen gegevens uit Cosmos DB. Als de gegevens ontbreekt, wordt de wizard wordt gestopt vanwege de volgende fout ' fout detectie indexschema uit de gegevensbron: Kan niet een prototype-index bouwen omdat datasource 'emptycollection' zijn geen gegevens geretourneerd'.

### <a name="2---start-import-data-wizard"></a>2 - de wizard gegevens importeren starten

U kunt [start de wizard](search-import-data-portal.md) vanuit de opdrachtbalk op de pagina met Azure Search-service of door te klikken op **Azure Search toevoegen** in de **instellingen** sectie van uw opslagaccount de linker navigatiedeelvenster.

   ![De gegevensopdracht in de portal importeren](./media/search-import-data-portal/import-data-cmd2.png "Start de wizard gegevens importeren")

### <a name="3---set-the-data-source"></a>3 - de gegevensbron instellen

> [!NOTE] 
> U kunt op dit moment maken of bewerken **MongoDB** gegevensbronnen met behulp van Azure portal of de .NET SDK. Echter, u **kunt** uitvoeringsgeschiedenis van MongoDB indexeerfuncties in de portal controleren.

In de **gegevensbron** pagina, de bron moet **Cosmos DB**, met de volgende specificaties:

+ **Naam** is de naam van het gegevensbronobject. Nadat u hebt gemaakt, kunt u deze kunt kiezen voor andere werkbelastingen.

+ **Cosmos DB-account** moet de primaire of secundaire verbindingsreeks uit Cosmos DB, waarbij een `AccountEdpointPoint` en een `AccountKey`. Het account bepaalt of de gegevens als SQL-API of Mongo DB API is geconverteerd

+ **Database** is een bestaande database van het account. 

+ **Verzameling** is een container met documenten. Documenten moeten zich in de volgorde voor het importeren te voltooien. 

+ **Query** mag leeg zijn als u wilt dat alle documenten, anders kan ingevoerde een query die een subset van het document wordt geselecteerd. 

   ![Cosmos DB-gegevensbron definitie](media/search-howto-index-cosmosdb/cosmosdb-datasource.png "definitie voor Cosmos DB-gegevensbron")

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - slaat u de pagina 'Cognitief zoeken toevoegen' in de wizard

Cognitieve vaardigheden toevoegen is niet nodig voor het document importeren. Tenzij u specifiek behoefte aan hebt [zijn onder andere Cognitive Services API's en transformaties](cognitive-search-concept-intro.md) aan uw pijplijn indexering, moet u deze stap overslaan.

Als u wilt de stap overslaat, gaat u eerst naar de volgende pagina.

   ![De knop volgende pagina voor Cognitive Search](media/search-get-started-portal/next-button-add-cog-search.png)

Vanaf deze pagina kunt u verder naar de aanpassing van de index.

   ![Stap voor cognitieve vaardigheden overslaan](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5 - set indexkenmerken

In de **Index** pagina, ziet u een lijst met velden met een gegevenstype en een reeks selectievakjes in voor het instellen van indexkenmerken. De wizard kan een lijst met velden op basis van metagegevens en door steekproeven van de brongegevens te genereren. 

U kunt bulksgewijs: Selecteer de kenmerken door te klikken op het selectievakje aan de bovenkant van de kolom van een kenmerk. Kies **ophalen mogelijk** en **doorzoekbaar** voor elk veld dat moet worden geretourneerd naar een client-app en onderworpen aan de verwerking van de zoeken in volledige tekst. U zult zien dat gehele getallen niet volledige tekst zijn of fuzzy doorzoekbare (getallen bewoordingen worden geëvalueerd en zijn vaak nuttig zijn in de filters).

Lees de beschrijving van [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) en [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) voor meer informatie. 

Neem even de tijd om te controleren van uw selecties. Zodra u de wizard uitvoert, fysieke gegevensstructuren worden gemaakt en kunt u zich niet aan het bewerken van deze velden zonder te verwijderen en opnieuw maken van alle objecten.

   ![Cosmos DB indexeren definitie](media/search-howto-index-cosmosdb/cosmosdb-index-schema.png "indexdefinitie Cosmos DB")

### <a name="6---create-indexer"></a>6 - indexeerfunctie maken

De wizard maakt volledig is opgegeven, drie afzonderlijke objecten in uw zoekservice. Een gegevensbron en index-object worden opgeslagen als benoemde resources in uw Azure Search-service. De laatste stap maakt u een indexeerfunctie-object. Naamgeving van de indexeerfunctie kan bestaan als een zelfstandige-resource die u kunt plannen en beheren onafhankelijk van het bronobject index en gegevens, die zijn gemaakt in dezelfde volgorde van de wizard.

Als u niet bekend met indexeerfuncties bent, een *indexeerfunctie* is een resource in Azure Search, een externe gegevensbron voor doorzoekbare inhoud te verkennen. De uitvoer van de **gegevens importeren** wizard wordt een indexeerfunctie die uw Cosmos DB-gegevensbron te verkennen, doorzoekbare inhoud worden uitgepakt en in een Azure Search-index importeert.

De volgende schermafbeelding ziet u de standaardconfiguratie van de indexeerfunctie. U kunt overschakelen naar **eenmaal** als u wilt de indexeerfunctie één keer uitgevoerd. Klik op **indienen** uitvoeren van de wizard en alle objecten maken. Indexeren begint onmiddellijk.

   ![De definitie van de cosmos DB-indexeerfunctie](media/search-howto-index-cosmosdb/cosmosdb-indexer.png "de definitie van de Cosmos DB-indexeerfunctie")

U kunt gegevens importeren in de portal-pagina's bewaken. Voortgang van meldingen geven indexering status en het aantal documenten worden geüpload. 

Wanneer het indexeren is voltooid, kunt u [Search explorer](search-explorer.md) query uitvoeren op uw index.

> [!NOTE]
> Als u de verwachte gegevens niet ziet, moet u mogelijk meer kenmerken instellen op meer velden. Verwijder de index en indexeerfunctie die u zojuist hebt gemaakt en doorloopt u de wizard opnieuw wijzigen van de selecties voor indexkenmerken in stap 5. 

<a name="cosmosdb-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST-API's gebruiken

U kunt de REST-API voor indexering van gegevens door Azure Cosmos DB, een gemeenschappelijke driedelige-werkstroom te volgen voor alle indexeerfuncties in Azure Search: maken van een gegevensbron, een index maken, een indexeerfunctie maken. Ophalen van gegevens uit de opslag van Cosmos treedt op wanneer u de indexeerfunctie maken-aanvraag indienen. Nadat deze aanvraag is voltooid, hebt u een index waarin u kunt zoeken. 

Als u MongoDB evalueert, moet u de REST-API gebruiken om de gegevensbron te maken.

U kunt in uw Cosmos DB-account of u wilt dat de verzameling moeten worden alle documenten automatisch te indexeren. Standaard alle documenten automatisch worden geïndexeerd, maar u kunt automatische indexering uitschakelen. Wanneer indexeren is uitgeschakeld, documenten zijn toegankelijk via alleen hun Self link-elementen of door query's met behulp van het document-id. Azure Search is vereist voor Cosmos DB automatische indexering zijn ingeschakeld in de verzameling die door Azure Search worden geïndexeerd. 

> [!NOTE]
> Azure Cosmos DB is de volgende generatie van DocumentDB. Hoewel de productnaam wordt gewijzigd, de `documentdb` -syntaxis in Azure Search-indexeerfuncties nog steeds bestaat voor achterwaartse compatibiliteit in de Azure Search API's en de portal-pagina's. Bij het configureren van indexeerfuncties, moet u opgeven de `documentdb` syntaxis volgens de instructies in dit artikel.


### <a name="1---assemble-inputs-for-the-request"></a>1 - invoer voor de aanvraag samenvoegen

U moet de servicenaam en de beheersleutel voor Azure Search (in de bericht-header) en de naam van het opslagaccount en de sleutel voor blob-opslag opgeven voor elke aanvraag. U kunt [Postman](search-fiddler.md) HTTP-aanvragen verzenden naar Azure Search.

Kopieer de volgende vier waarden in Kladblok, zodat u ze in een aanvraag kunt plakken:

+ Naam van een Azure Search-service
+ Azure Search-administratorsleutel
+ Cosmos DB-verbindingsreeks

U kunt deze waarden vinden in de portal:

1. In de portal-pagina's voor Azure Search, kopieert u de search service-URL van de pagina overzicht.

2. Klik in het linkernavigatiedeelvenster op **sleutels** en kopieer het primaire of secundaire sleutel (ze zijn equivalent).

3. Schakel over naar de portal-pagina's voor uw Cosmos-storage-account. In het navigatiedeelvenster links onder **instellingen**, klikt u op **sleutels**. Deze pagina vindt u een URI, twee sets met verbindingsreeksen, en twee sets met sleutels. Kopieer een van de verbindingsreeksen naar Kladblok.

### <a name="2---create-a-data-source"></a>2 - een gegevensbron maken

Een **gegevensbron** Hiermee geeft u de gegevens naar de index, referenties en beleidsregels voor het identificeren van wijzigingen in de gegevens (zoals gewijzigde of verwijderde documenten in uw verzameling). De gegevensbron wordt gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Formuleer een POST-aanvraag voor het maken van een gegevensbron:

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

| Veld   | Description |
|---------|-------------|
| **De naam** | Vereist. Kies een naam voor uw data source-object. |
|**type**| Vereist. Moet `documentdb`. |
|**credentials** | Vereist. Moet u een Cosmos DB-verbindingsreeks.<br/>Voor de SQL-collecties zijn tekenreeksen voor databaseverbindingen in deze indeling: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`<br/>Voor MongoDB-verzamelingen toevoegen **API-soort MongoDb =** op de verbindingstekenreeks:<br/>`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`<br/>Vermijd poortnummers in de eindpunt-url. Als u het poortnummer opgeeft, worden Azure Search kan geen index van uw Azure Cosmos DB-database.|
| **container** | Bevat de volgende elementen: <br/>**name**: Vereist. Geef de ID van de databaseverzameling worden geïndexeerd.<br/>**query**: Optioneel. U kunt een query voor het samenvoegen van een willekeurige JSON-document in een vast schema dat Azure Search kunt indexeren.<br/>Query's worden niet ondersteund voor MongoDB-verzamelingen. |
| **dataChangeDetectionPolicy** | Aanbevolen. Zie [gewijzigd documenten te indexeren](#DataChangeDetectionPolicy) sectie.|
|**dataDeletionDetectionPolicy** | Optioneel. Zie [verwijderd documenten te indexeren](#DataDeletionDetectionPolicy) sectie.|

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


### <a name="3---create-a-target-search-index"></a>3 - een doel search-index maken 

[Maken van een doel-Azure Search-index](/rest/api/searchservice/create-index) als u er nog geen hebt. Het volgende voorbeeld wordt een index met een veld-ID en -beschrijving:

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
| Bool |Edm.Boolean, Edm.String |
| Cijfers die lijkt op gehele getallen |Edm.Int32, Edm.Int64, Edm.String |
| Getallen die eruit drijvende-punten |Edm.Double, Edm.String |
| String |Edm.String |
| Matrices met primitieve typen, bijvoorbeeld ["a", "b", "c"] |Collection(EDM.String) |
| Tekenreeksen die lijkt op datums |Edm.DateTimeOffset, Edm.String |
| GeoJSON-objecten, bijvoorbeeld {"type": 'Point', "coördinaten": [lang zijn en lat]} |Edm.GeographyPoint |
| Andere JSON-objecten |N/A |

### <a name="4---configure-and-run-the-indexer"></a>4 - configureren en de indexeerfunctie uitvoeren

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

## <a name="use-net"></a>.NET gebruiken

De .NET SDK is volledig pariteit met de REST-API. Het is raadzaam om de vorige sectie van de REST-API om te leren van concepten, werkstromen en vereisten te controleren. U kunt vervolgens verwijzen naar de volgende .NET API-referentiedocumentatie voor het implementeren van een JSON-indexeerfunctie in beheerde code.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

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

## <a name="watch-this-video"></a>Deze video bekijken

In deze enigszins oudere 7 minuten durende video Azure Cosmos DB Program Manager Andrew Liu ziet u hoe u een Azure Search-index toevoegen aan een Azure Cosmos DB-container. De portal-pagina's wordt weergegeven in de video verouderd zijn, maar de informatie die relevant is.

>[!VIDEO https://www.youtube.com/embed/OyoYu1Wzk4w]

## <a name="NextSteps"></a>Volgende stappen

Gefeliciteerd! U hebt geleerd hoe u Azure Cosmos DB integreert met Azure Search met behulp van een indexeerfunctie.

* Zie voor meer informatie over Azure Cosmos DB, de [Azure Cosmos DB-servicepagina](https://azure.microsoft.com/services/cosmos-db/).
* Zie voor meer informatie over Azure Search, de [pagina zoekservice](https://azure.microsoft.com/services/search/).
