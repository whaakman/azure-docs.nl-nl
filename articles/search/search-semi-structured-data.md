---
title: Zoeken semi-gestructureerde gegevens in de Azure-cloud-opslag
description: Zoeken naar semi-gestructureerde blob-gegevens met behulp van Azure Search.
author: roygara
manager: timlt
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: a80ae99c2ada00885019ee93e4ef36821340d3a5
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="part-2-search-semi-structured-data-in-cloud-storage"></a>Deel 2: Zoeken semi-gestructureerde gegevens in de cloudopslag

In een tweedelige zelfstudie reeks leert u hoe moet worden gezocht semi-gestructureerde en ongestructureerde gegevens met behulp van Azure search. [Deel 1](../storage/blobs/storage-unstructured-search.md) doorlopen u zoeken via ongestructureerde gegevens, maar ook belangrijke vereisten voor deze zelfstudie, zoals het maken van het opslagaccount is opgenomen. 

In deel 2 verplaatst focus naar semi-gestructureerde gegevens, zoals JSON, opgeslagen in Azure blobs. Semi-gestructureerde gegevens bevat labels of markeringen die inhoud in de gegevens te scheiden. Hiermee splitst u het verschil tussen ongestructureerde gegevens die wholistically moet worden geïndexeerd en formeel gestructureerde gegevens die in overeenstemming is met een gegevensmodel, zoals een relationele database-schema, dat op basis van veld per kan worden benaderd.

In deel 2, leest u hoe:

> [!div class="checklist"]
> * Een Azure Search-gegevensbron voor een Azure blob-container
> * U maakt en vult u een Azure Search-index en een indexeerfunctie met de container verkennen en uitpakken van inhoud
> * Zoeken in de index die u zojuist hebt gemaakt

> [!NOTE]
> Deze zelfstudie wordt gebruikgemaakt van JSON matrix ondersteuning, momenteel een preview-functie in Azure Search is. Het is niet beschikbaar in de portal. Om deze reden we maken gebruik van de preview REST-API waarmee deze functie en een REST-clienthulpprogramma de API aan te roepen.

## <a name="prerequisites"></a>Vereisten

* Voltooiing van de [vorige zelfstudie](../storage/blobs/storage-unstructured-search.md) bieden de storage-account en zoek service gemaakt in de vorige zelfstudie.

* Installatie van een REST-client en een goed begrip van het maken van een HTTP-aanvraag. Voor de doeleinden van deze zelfstudie gebruiken we [Postman](https://www.getpostman.com/). U kunt een andere REST-client gebruiken als u al vertrouwd met een bepaald bent.

## <a name="set-up-postman"></a>Postman instellen

Start Postman en instellen van een HTTP-aanvraag. Als u niet bekend met dit hulpprogramma bent, raadpleegt u [verkennen Azure Search REST API's met Fiddler of Postman](search-fiddler.md) voor meer informatie.

De aanvraagmethode voor elke aanroep in deze zelfstudie is "POST". De header-sleutels zijn 'Content-type' en "api-sleutel." De waarden van de header-sleutels zijn ' application/json' en de administratorsleutel' ' (de beheersleutel is een tijdelijke aanduiding voor de primaire sleutel van uw zoekopdracht) respectievelijk. De hoofdtekst is waar u de werkelijke inhoud van de aanroep van plaatsen. Afhankelijk van de client die u gebruikt, mogelijk zijn er enkele variaties op de manier waarop u uw query samenstelt, maar zijn de basisprincipes.

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/postmanoverview.png)

Uw zoekopdracht api-sleutel is vereist voor de REST-aanroepen behandeld in deze zelfstudie. U vindt uw api-sleutel onder **sleutels** binnen uw search-service. Deze api-sleutel moet zich in de koptekst van elke API-aanroep (vervangen ' administratorsleutel' in de vorige schermafbeelding ermee) u kunt in deze zelfstudie wordt verwezen. De sleutel behouden omdat u deze nodig voor elke aanroep hebt.

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Download de voorbeeldgegevens

Een verzameling voorbeeldgegevens is bedoeld voor u. **Download [klinische op proefversies json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)**  en pak deze naar de eigen map.

Voorbeeld van de JSON-bestanden die oorspronkelijk tekst opgenomen in het voorbeeld zijn bestanden die zijn verkregen via [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). We hebben ze converteren naar JSON voor uw gemak.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="upload-the-sample-data"></a>De voorbeeldgegevens te uploaden

In de Azure portal, gaat u terug naar het opslagaccount dat is gemaakt in de [vorige zelfstudie](../storage/blobs/storage-unstructured-search.md). Open vervolgens de **gegevens** container en op **uploaden**.

Klik op **Geavanceerd**'klinische-proefversies-json' opgeven en vervolgens uploaden alle van de JSON-bestanden die u hebt gedownload.

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/clinicalupload.png)

Nadat het uploaden is voltooid, worden de bestanden worden weergegeven in hun eigen submap in de gegevenscontainer.

## <a name="connect-your-search-service-to-your-container"></a>Verbinding maken met uw search-service naar de container

We gebruiken Postman drie API-aanroepen naar uw search-service om te kunnen maken van een gegevensbron en een index een indexeerfunctie. De gegevensbron bevat een verwijzing naar uw storage-account en uw JSON-gegevens. Uw zoekservice maakt de verbinding bij het laden van de gegevens.

De query-tekenreeks moet bevatten **api-version = 2016-09-01-Preview** en elke aanroep moet resulteren in een **201 gemaakt**. De algemeen beschikbaar api-versie heeft nog geen de mogelijkheid om te verwerken json op als een jsonArray, momenteel alleen de preview-api-versie wordt.

De volgende drie API-aanroepen van de REST-client uitvoeren.

### <a name="create-a-datasource"></a>Een gegevensbron maken

Een gegevensbron geeft aan welke gegevens u wilt een index.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Vervang `[service name]` met de naam van uw zoekservice.

Voor deze aanroep moet u de naam van uw opslagaccount en de sleutel van uw opslagaccount. De opslagaccountsleutel vindt u in de Azure-portal in uw opslagaccount **toegangstoetsen**. De locatie wordt weergegeven in de volgende afbeelding:

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/storagekeys.png)

Zorg ervoor dat u de `[storage account name]` en `[storage account key]` in de hoofdtekst van de aanroep van voordat de aanroep wordt uitgevoerd.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Het antwoord moet eruitzien als:

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
    
De tweede API-aanroep maakt een index. Een index bevat alle parameters en hun kenmerken.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Vervang `[service name]` met de naam van uw zoekservice.

Eerst Vervang de URL. Kopieer en plak de volgende code in de hoofdtekst en voer de query.

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

Het antwoord moet eruitzien als:

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

### <a name="create-an-indexer"></a>Maak een indexeerfunctie

Een indexeerfunctie verbindt de gegevensbron met de doel-zoekindex en eventueel biedt een planning voor het automatiseren van de gegevens te vernieuwen.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Vervang `[service name]` met de naam van uw zoekservice.

Eerst Vervang de URL. Kopieer en plak de volgende code in de hoofdtekst en voer de query.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Het antwoord moet eruitzien als:

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

## <a name="search-your-json-files"></a>Zoek uw JSON-bestanden

Nu dat uw search-service is verbonden met uw gegevenscontainer, kunt u beginnen uw bestanden te zoeken.

Opent u de Azure-portal en Ga terug naar uw search-service. Net als in de vorige zelfstudie.

  ![Niet-gestructureerde zoeken](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Zoeken van de gebruiker gedefinieerde metagegevens

Als voorheen kunt de gegevens kunnen worden opgevraagd op verschillende manieren: zoeken in volledige tekst, de Systeemeigenschappen of de gebruiker gedefinieerde metagegevens. Zowel de Systeemeigenschappen als de gebruiker gedefinieerde metagegevens kunnen alleen worden doorzocht met de `$select` parameter als ze zijn gemarkeerd als **ophalen mogelijk** tijdens het maken van de doelindex. Parameters in de index kunnen niet worden gewijzigd als ze zijn gemaakt. Extra parameters kunnen echter worden toegevoegd.

Een voorbeeld van een eenvoudige query is `$select=Gender,metadata_storage_size`, die beperkt de terug naar deze twee parameters.

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/lastquery.png)

Een voorbeeld van complexe query's is `$filter=MinimumAge ge 30 and MaximumAge lt 75`, die alleen resultaten waarbij de parameters MinimumAge is groter dan of gelijk is aan 30 en MaximumAge minder dan 75 retourneert.

  ![Semi-gestructureerde zoeken](media/search-semi-structured-data/metadatashort.png)

Als u wilt experimenteren en probeer het een paar meer query's zelf, u kunt doen. Weten waarmee u logische operators kunt (en, of niet) en vergelijkingsoperators (eq, ne, gt, lt, ge, RP). Tekenreeksvergelijkingen zijn hoofdlettergevoelig.

De `$filter` parameter werkt alleen met metagegevens die zijn gemarkeerd Filterbaar bij het maken van uw index.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over semi-gestructureerde gegevens met behulp van Azure search, zoals het zoeken:

> [!div class="checklist"]
> * Een Azure Search-Service met de REST-API maken
> * De Azure Search-Service gebruiken om te zoeken van de container

Volg deze koppeling voor meer informatie over zoeken.

> [!div class="nextstepaction"]
> [Indexeren van documenten in Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md)