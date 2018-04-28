---
title: Semi-gestructureerde gegevens zoeken in Azure-cloudopslag
description: Semi-gestructureerde blobgegevens zoeken met behulp van Azure Search.
author: roygara
manager: cgronlun
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.openlocfilehash: f05e9dd12a838199b23deddb4f6c4fb4c2fced08
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="part-2-search-semi-structured-data-in-cloud-storage"></a>Deel 2: Semi-gestructureerde gegevens zoeken in cloudopslag

In een tweedelige zelfstudie leert u hoe u semi-gestructureerde en niet-gestructureerde gegevens kunt doorzoeken met behulp van Azure Search. In [deel 1](../storage/blobs/storage-unstructured-search.md) hebt u gezien hoe u niet-gestructureerde gegevens doorzoekt en zijn ook belangrijke vereisten voor deze zelfstudie behandeld, zoals het maken van het opslagaccount. 

Deel 2 gaat voornamelijk over semi-gestructureerde gegevens, zoals JSON, die zijn opgeslagen in Azure-blobs. Semi-gestructureerde gegevens bevatten labels of markeringen die inhoud in de gegevens scheiden. Hierin ligt ook het verschil tussen niet-gestructureerde gegevens en formeel gestructureerde gegevens. Niet-gestructureerde gegevens moeten als geheel worden geïndexeerd en gestructureerde gegevens zijn in overeenstemming met een gegevensmodel, zoals een relationele-databaseschema, en kunnen op veldbasis worden benaderd.

In deel 2 vindt u informatie over:

> [!div class="checklist"]
> * Een Azure Search-gegevensbron configureren voor een Azure-blobcontainer
> * Een Azure Search-index maken en vullen, en een indexeerfunctie maken om de container te verkennen en doorzoekbare inhoud uit te pakken
> * De index doorzoeken die u zojuist hebt gemaakt

> [!NOTE]
> In deze zelfstudie wordt gebruikgemaakt van JSON-matrixondersteuning. Dit is momenteel een preview-functie in Azure Search. Deze is niet beschikbaar in de portal. Daarom wordt hier gebruikgemaakt van de preview-REST API, die deze functie biedt, en een REST-clienthulpprogramma om de API aan te roepen.

## <a name="prerequisites"></a>Vereisten

* De [vorige zelfstudie](../storage/blobs/storage-unstructured-search.md) moet zijn voltooid. Deze biedt het opslagaccount en daar is de zoekservice gemaakt.

* Installatie van een REST-client en goed begrip van hoe u een HTTP-aanvraag moet maken. Voor deze zelfstudie wordt gebruikgemaakt van [Postman](https://www.getpostman.com/). U kunt ook een andere REST-client gebruiken als u daar meer ervaring mee hebt.

## <a name="set-up-postman"></a>Postman instellen

Start Postman en stel een HTTP-aanvraag in. Als u niet bekend bent met dit hulpprogramma, raadpleegt u [REST Azure Search-API's verkennen in Fiddler of Postman](search-fiddler.md) voor meer informatie.

De aanvraagmethode voor elke aanroep in deze zelfstudie is 'POST'. De headersleutels zijn 'Content-type' en 'api-key'. De waarden van de headersleutels zijn respectievelijk 'application/json' en 'admin key' (admin key is een tijdelijke aanduiding voor de primaire sleutel van uw zoekopdracht). In de hoofdtekst plaatst u de werkelijke inhoud van uw aanroep. Afhankelijk van de client die u gebruikt, kunnen er enkele variaties zijn op de manier waarop u uw query samenstelt, maar dit zijn de basisprincipes.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/postmanoverview.png)

Voor de REST-aanroepen die in deze zelfstudie worden behandeld, is de API-sleutel van de zoekopdracht vereist. U vindt de API-sleutel onder **Sleutels** in uw zoekservice. Deze API-sleutel moet voorkomen in de header van elke API-aanroep (vervang 'admin key' in de vorige schermafbeelding door deze API-sleutel) die u in deze zelfstudie gevraagd wordt te maken. Noteer de sleutel. U hebt deze nodig voor elke aanroep.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>De voorbeeldgegevens downloaden

Er is een voorbeeldgegevensset voor u voorbereid. **Download [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** en pak het uit in een eigen map.

In de zip vindt u voorbeeld-JSON-bestanden. Dit waren oorspronkelijk tekstbestanden, verkregen via [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Deze zijn voor uw gemak geconverteerd naar JSON.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="upload-the-sample-data"></a>De voorbeeldgegevens uploaden

Ga in Azure Portal terug naar het opslagaccount dat is gemaakt in de [vorige zelfstudie](../storage/blobs/storage-unstructured-search.md). Open de **gegevens**container en klik op **Uploaden**.

Klik op **Geavanceerd**, voer 'clinical-trials-json' in en upload alle JSON-bestanden die u hebt gedownload.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/clinicalupload.png)

Nadat de upload is voltooid, worden de bestanden weergegeven in hun eigen submap in de gegevenscontainer.

## <a name="connect-your-search-service-to-your-container"></a>Uw zoekservice verbinden met de container

We gebruiken Postman om drie API-aanroepen te doen naar uw zoekservice; voor het maken van een gegevensbron, een index en een indexeerfunctie. De gegevensbron bevat een verwijzing naar uw opslagaccount en uw JSON-gegevens. Uw zoekservice maakt de verbinding tijdens het laden van de gegevens.

De querytekenreeks moet **api-version=2016-09-01-Preview** bevatten en elke aanroep moet resulteren in een **201 - Gemaakt**. Met de algemeen beschikbare api-versie is het nog niet mogelijk om json te verwerken als een jsonArray. Dat is momenteel alleen mogelijk met de preview-api-versie.

Voer de volgende drie API-aanroepen uit vanuit de REST-client.

### <a name="create-a-datasource"></a>Een gegevensbron maken

Een gegevensbron geeft aan welke gegevens moeten worden geïndexeerd.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Vervang `[service name]` door de naam van uw zoekservice.

Voor deze aanroep hebt u de naam en sleutel van uw opslagaccount nodig. De opslagaccountsleutel vindt u in de **Toegangssleutels** van uw opslagaccount in Azure Portal. De locatie wordt in de volgende afbeelding weergegeven:

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

### <a name="create-an-index"></a>Een index maken
    
Met de tweede API-aanroep wordt een index gemaakt. Een index geeft alle parameters en hun kenmerken op.

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

### <a name="create-an-indexer"></a>Een indexeerfunctie maken

Een indexeerfunctie verbindt de gegevensbron met de doelzoekindex en biedt optioneel een schema om het vernieuwen van de gegevens te automatiseren.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Vervang `[service name]` door de naam van uw zoekservice.

Vervang eerst de URL. Kopieer daarna de volgende code, plak deze in de hoofdtekst en voer de query uit.

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

Nu de zoekservice is verbonden met uw gegevenscontainer, kunt u uw bestanden gaan doorzoeken.

Open Azure Portal en ga terug naar uw zoekservice. Net zoals u dat hebt gedaan in de vorige zelfstudie.

  ![Niet-gestructureerde zoekopdracht](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Door gebruiker gedefinieerde metagegevens doorzoeken

Ook nu kunnen gegevens worden opgevraagd op verschillende manieren: zoeken in volledige tekst, systeemeigenschappen of door de gebruiker gedefinieerde metagegevens. Systeemeigenschappen en door de gebruiker gedefinieerde metagegevens kunnen alleen worden doorzocht met de parameter `$select` als ze tijdens het maken van de doelindex zijn gemarkeerd als **Ophalen mogelijk**. Parameters in de index mogen niet meer worden gewijzigd nadat ze zijn gemaakt. Er kunnen echter wel extra parameters worden toegevoegd.

Een voorbeeld van een eenvoudige query is `$select=Gender,metadata_storage_size`. Deze retourneert alleen deze twee parameters.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/lastquery.png)

Een voorbeeld van een complexere query is `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Deze retourneert alleen resultaten als de parameter MinimumAge groter is dan of gelijk is aan 30 en als MaximumAge kleiner is dan 75.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/metadatashort.png)

Ga gerust uw gang als u wilt experimenteren en zelf nog een aantal query's wilt proberen. U kunt logische operators (AND, OR, NOT) en vergelijkingsoperators (eq, ne, gt, lt, ge en le) gebruiken. Tekenreeksvergelijkingen zijn hoofdlettergevoelig.

De parameter `$filter` werkt alleen met metagegevens die bij het maken van de index zijn gemarkeerd als filterbaar.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het doorzoeken van semi-gestructureerde gegevens met behulp van Azure Search. U hebt onder andere het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Search Service maken met de REST API
> * De Azure Search Service gebruiken om uw container te doorzoeken

Volg deze koppeling voor meer informatie over zoeken.

> [!div class="nextstepaction"]
> [Documenten in Azure Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md)