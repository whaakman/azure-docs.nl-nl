---
title: Zelfstudie voor het zoeken naar JSON in Azure-blob-opslag - Azure Search
description: In deze zelfstudie leert u hoe u semi-gestructureerde Azure-blob-gegevens kunt zoeken met Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 1c8ce14dd3961eff33a54a14c2bd0b27650d8a50
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201344"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Zelfstudie: Semi-gestructureerde gegevens zoeken in Azure-cloudopslag

Azure Search kunt indexeren van JSON-documenten en matrices in Azure blob storage met behulp van een [indexeerfunctie](search-indexer-overview.md) die weet hoe het lezen van semi-gestructureerde gegevens. Semi-gestructureerde gegevens bevatten labels of markeringen die inhoud in de gegevens scheiden. Het verschil tussen niet-gestructureerde gegevens, die volledig moeten worden geïndexeerd en formeel gestructureerde gegevens die in overeenstemming is met een gegevensmodel, zoals een relationele database-schema, die kan worden geïndexeerd op basis van per veld gesplitst.

In deze zelfstudie gebruikt u de [Azure Search REST API's](https://docs.microsoft.com/rest/api/searchservice/) en een REST-client de volgende taken uitvoeren:

> [!div class="checklist"]
> * Een Azure Search-gegevensbron configureren voor een Azure-blobcontainer
> * Een Azure Search-index bevatten doorzoekbare inhoud maken
> * Configureren en uitvoeren van een indexeerfunctie om te lezen van de container en doorzoekbare inhoud ophalen uit Azure blob-opslag
> * De index doorzoeken die u zojuist hebt gemaakt

> [!NOTE]
> In deze zelfstudie wordt gebruikgemaakt van JSON-matrixondersteuning. Dit is momenteel een preview-functie in Azure Search. Deze is niet beschikbaar in de portal. Daarom wordt hier gebruikgemaakt van de preview-REST API, die deze functie biedt, en een REST-clienthulpprogramma om de API aan te roepen.

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

[Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voorbeeldgegevens bevatten.

[Postman gebruiken](https://www.getpostman.com/) of een andere REST-client om uw aanvragen te verzenden. Instructies voor het instellen van een HTTP-aanvraag in Postman zijn opgegeven in de volgende sectie.

## <a name="set-up-postman"></a>Postman instellen

Start Postman en stel een HTTP-aanvraag in. Als u niet bekend met dit hulpprogramma bent, raadpleegt u [verkennen Azure Search REST API's met Postman](search-fiddler.md).

De aanvraagmethode voor elke aanroep in deze zelfstudie is 'POST'. De headersleutels zijn 'Content-type' en 'api-key'. De waarden van de headersleutels zijn respectievelijk 'application/json' en 'admin key' (admin key is een tijdelijke aanduiding voor de primaire sleutel van uw zoekopdracht). In de hoofdtekst plaatst u de werkelijke inhoud van uw aanroep. Afhankelijk van de client die u gebruikt, kunnen er enkele variaties zijn op de manier waarop u uw query samenstelt, maar dit zijn de basisprincipes.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/postmanoverview.png)

Voor de REST-aanroepen die in deze zelfstudie worden behandeld, is de API-sleutel van de zoekopdracht vereist. U vindt de API-sleutel onder **Sleutels** in uw zoekservice. Deze API-sleutel moet voorkomen in de header van elke API-aanroep (vervang 'admin key' in de vorige schermafbeelding door deze API-sleutel) die u in deze zelfstudie gevraagd wordt te maken. Noteer de sleutel. U hebt deze nodig voor elke aanroep.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/keys.png)

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

1. **Download [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** en pak het uit in een eigen map. Data is afkomstig uit [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results), geconverteerd naar JSON voor deze zelfstudie.

2. Aanmelden bij de [Azure-portal](https://portal.azure.com), gaat u naar uw Azure storage-account, open de **gegevens** container en klik op **uploaden**.

3. Klik op **Geavanceerd**, voer 'clinical-trials-json' in en upload alle JSON-bestanden die u hebt gedownload.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/clinicalupload.png)

Nadat de upload is voltooid, worden de bestanden weergegeven in hun eigen submap in de gegevenscontainer.

## <a name="connect-your-search-service-to-your-container"></a>Uw zoekservice verbinden met de container

We gebruiken Postman om drie API-aanroepen te doen naar uw zoekservice; voor het maken van een gegevensbron, een index en een indexeerfunctie. De gegevensbron bevat een verwijzing naar uw opslagaccount en uw JSON-gegevens. Uw zoekservice maakt de verbinding tijdens het laden van de gegevens.

De query-tekenreeks moet bevatten een preview-API (zoals **api-version = 2017-11-11-Preview**) en elke aanroep moet resulteren in een **201-gemaakt**. Met de algemeen beschikbare api-versie is het nog niet mogelijk om json te verwerken als een jsonArray. Dat is momenteel alleen mogelijk met de preview-api-versie.

Voer de volgende drie API-aanroepen uit vanuit de REST-client.

## <a name="create-a-data-source"></a>Een gegevensbron maken

Een gegevensbron is een Azure Search-object waarmee wordt aangegeven welke gegevens moeten worden geïndexeerd.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Vervang `[service name]` door de naam van uw zoekservice. Voor deze aanroep hebt u de naam en sleutel van uw opslagaccount nodig. De opslagaccountsleutel vindt u in de **Toegangssleutels** van uw opslagaccount in Azure Portal. De locatie wordt in de volgende afbeelding weergegeven:

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/storagekeys.png)

Zorg dat u de `[storage account name]` en `[storage account key]` in de hoofdtekst van de aanroep vervangt voordat u de aanroep uitvoert.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Een index maken
    
De tweede API-aanroep wordt een Azure Search-index gemaakt. Een index geeft alle parameters en hun kenmerken op.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Vervang `[service name]` door de naam van uw zoekservice.

Vervang eerst de URL. Kopieer daarna de volgende code, plak deze in de hoofdtekst en voer de query uit.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

## <a name="create-and-run-an-indexer"></a>Maken en een indexeerfunctie uitvoeren

Een indexeerfunctie verbindt de gegevensbron, worden gegevens geïmporteerd in de doelzoekindex en biedt optioneel een schema voor het automatiseren van het vernieuwen van gegevens.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Vervang `[service name]` door de naam van uw zoekservice.

Vervang eerst de URL. Kopieer en plak de volgende code in uw instantie en de aanvraag verzenden. De aanvraag wordt onmiddellijk verwerkt. Wanneer het antwoord terug afkomstig is, hebt u een index met volledige tekst kan worden doorzocht.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Uw JSON-bestanden doorzoeken

U kunt nu query's op basis van de index uitgeven. Gebruik voor deze taak [ **Search explorer** ](search-explorer.md) in de portal.

  ![Niet-gestructureerde zoekopdracht](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Door gebruiker gedefinieerde metagegevens doorzoeken

Ook nu kunnen gegevens worden opgevraagd op verschillende manieren: zoeken in volledige tekst, systeemeigenschappen of door de gebruiker gedefinieerde metagegevens. Systeemeigenschappen en door de gebruiker gedefinieerde metagegevens kunnen alleen worden doorzocht met de parameter `$select` als ze tijdens het maken van de doelindex zijn gemarkeerd als **Ophalen mogelijk**. Parameters in de index mogen niet meer worden gewijzigd nadat ze zijn gemaakt. Er kunnen echter wel extra parameters worden toegevoegd.

Een voorbeeld van een eenvoudige query is `$select=Gender,metadata_storage_size`. Deze retourneert alleen deze twee parameters.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/lastquery.png)

Een voorbeeld van een complexere query is `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Deze retourneert alleen resultaten als de parameter MinimumAge groter is dan of gelijk is aan 30 en als MaximumAge kleiner is dan 75.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/metadatashort.png)

Ga gerust uw gang als u wilt experimenteren en zelf nog een aantal query's wilt proberen. U kunt logische operators (AND, OR, NOT) en vergelijkingsoperators (eq, ne, gt, lt, ge en le) gebruiken. Tekenreeksvergelijkingen zijn hoofdlettergevoelig.

De parameter `$filter` werkt alleen met metagegevens die bij het maken van de index zijn gemarkeerd als filterbaar.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is de resourcegroep met de Azure Search-service te verwijderen. U kunt de resourcegroep nu verwijderen om alles daarin permanent te verwijderen. De naam van de resourcegroep staat in de portal op de pagina Overzicht van de Azure Search-service.

## <a name="next-steps"></a>Volgende stappen

U kunt AI-algoritmen aan een indexeringspijplijn koppelen. Als volgende stap kunt u verdergaan met de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Documenten in Azure Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md)