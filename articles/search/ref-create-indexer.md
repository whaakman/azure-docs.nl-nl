---
title: Maak indexeerfunctie (Azure Search Service REST-api-version = 2017-11-11-Preview)
description: In de preview-API indexeerfuncties uitgebreid met parameters voor outputFieldMapping verrijking uitvoer toewijzen aan een veld in een Azure Search-index.
services: search
manager: pablocas
author: luiscabrer
ms.author: luisca
ms.date: 05/01/2018
ms.prod: azure
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.openlocfilehash: 595502ecf0a78491c73800e8077de65707c7a486
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Maak indexeerfunctie (Azure Search Service REST-api-version = 2017-11-11-Preview)

Deze API-verwijzing is een preview-versie van de documentatie, toe te voegen [cognitieve zoeken](cognitive-search-concept-intro.md) elementen op de API van de indexeerfunctie maken. Net als bij de [algemeen beschikbaar](https://docs.microsoft.com/rest/api/searchservice/create-indexer) versie, kunt u een nieuwe indexeerfunctie binnen een Azure Search-service met behulp van een HTTP POST-aanvraag. 

```http
POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json  
    api-key: [admin key]  
```  
De **api-sleutel** moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [beveiliging in Azure Search](search-security-overview.md) voor meer informatie over sleutels. [Maak een Azure Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

U kunt ook gebruik van opslag en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat nog, wordt deze gemaakt.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
De **api-versie** is vereist. De huidige beschikbare versie voor in het algemeen is `api-version=2017-11-11`, maar moet u de preview-versie voor cognitieve zoeken: `api-version=2017-11-11-Preview`.  Zie [API-versies in Azure Search](search-api-versions.md) voor meer informatie.

Voor gegevens-platform-specifieke richtlijnen over het maken van indexeerfuncties, beginnen met [indexeerfuncties overzicht](search-indexer-overview.md), waaronder de volledige lijst met [verwante artikelen](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  Het maximum aantal toegestane indexeerfuncties varieert per prijscategorie. De gratis service kunnen maximaal 3 indexeerfuncties. Standard-service kan 50 indexeerfuncties. Standaard hoogwaardige services ondersteunen geen indexeerfuncties helemaal. Zie [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.    

## <a name="request"></a>Aanvraag  

Een [gegevensbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source), [index](https://docs.microsoft.com/rest/api/searchservice/create-index), en [vaardigheden](ref-create-skillset.md) deel uitmaken van een [indexeerfunctie](search-indexer-overview.md) definitie, maar elk is een onafhankelijke onderdeel dat kan worden gebruikt in verschillende combinaties. U kunt bijvoorbeeld dezelfde gegevensbron met meerdere indexeerfuncties of dezelfde index met meerdere indexeerfuncties of meerdere indexeerfuncties schrijven naar een enkele index.

 De hoofdtekst van de aanvraag bevat een definitie van een indexeerfunctie, met de volgende onderdelen.

+ [dataSourceName](#dataSourceName)
+ [targetIndexName](#targetIndexName)
+ [skillsetName](#skillset)
+ [schedule](#indexer-schedule)
+ [parameters](#indexer-parameters)
+ [fieldMappings](#field-mappings)
+ [outputFieldMappings](#output-fieldmappings)

## <a name="request-syntax"></a>De syntaxis van de aanvraag

Syntaxis voor het structureren nettolading van de aanvraag is als volgt. Verderop in dit artikel krijgt u een voorbeeld van een aanvraag.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "skillsetName" : "Required for cognitive search enrichment",
    "schedule" : { Optional, but immediately runs once if unspecified. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```
<a name="dataSourceName"></a>

### <a name="datasourcename"></a>'dataSourceName'

Een [gegevensbrondefinitie](https://docs.microsoft.com/rest/api/searchservice/create-data-source) vaak bevat eigenschappen die een indexeerfunctie gebruiken kunt om te misbruiken bron platform kenmerken. Als zodanig bepaalt de gegevensbron die u aan de indexeerfunctie doorgeeft de beschikbaarheid van bepaalde eigenschappen en -parameters, zoals inhoudstype filteren in Azure blobs of time-out voor query's voor Azure SQL Database. 

<a name="targetIndexName"></a>

### <a name="targetindexname"></a>'targetIndexName'

Een [indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index) definieert de verzameling van velden met doorzoekbaar Filterbaar, worden opgehaald en andere afschrijvingen die bepalen hoe het veld wordt gebruikt. Tijdens het indexeren, de indexeerfunctie verkent van de gegevensbron, eventueel documenten zorgt en haalt informatie op de resultaten naar JSON serialiseert en indexen van de nettolading van de op basis van het schema is gedefinieerd voor de index.

<a name="skillset"></a>

### <a name="skillsetname"></a>'skillsetName'

[Cognitieve zoeken (preview)](cognitive-search-concept-intro.md) verwijst naar natuurlijke taal en de installatiekopie van het verwerken van de mogelijkheden in Azure Search toegepast tijdens gegevensopname entiteiten, key zinnen, taal, gegevens ophalen uit de afbeeldingen, enzovoort. Transformaties toegepast op inhoud zijn via *vaardigheden*, waarmee u combineren in één [ *vaardigheden*](ref-create-skillset.md), één per indexeerfunctie. Net als bij gegevensbronnen en indexen is een vaardigheden een onafhankelijke onderdeel waarmee u een indexeerfunctie worden gekoppeld. U kunt het doel van een vaardigheden met andere indexeerfuncties, maar elke indexeerfunctie slechts één vaardigheden tegelijk gebruiken.
 
<a name="indexer-schedule"></a>

### <a name="schedule"></a>'schema'  
Een indexeerfunctie kunt optioneel een planning opgeven. Zonder een schema, moet de indexeerfunctie wordt uitgevoerd onmiddellijk wanneer u de aanvraag verzendt: verbinding maken met, verkennen en de gegevensbron indexeren. Voor sommige scenario's met inbegrip van langlopende indexeertaken, schema's worden gebruikt om [uitbreiden van het venster verwerking](search-howto-reindex.md#scale-out-indexing) dan de maximaal 24 uur. Als een planning aanwezig is, de indexeerfunctie periodiek wordt uitgevoerd volgens de planning. De planner is ingebouwd in; u kunt een externe scheduler niet gebruiken. Een **planning** heeft de volgende kenmerken: 

-   **interval**: vereist. Een duration-waarde die een interval of de periode voor de indexeerfunctie aangeeft wordt uitgevoerd. De minimaal toegestane interval is vijf minuten. het langste is één dag. Moet zijn geformatteerd als de waarde van een XSD-'dayTimeDuration' (een beperkte subset van een [duur van de ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `"P[nD][T[nH][nM]]".` voorbeelden: `PT15M` voor elke 15 minuten `PT2H` voor elke 2 uur.  

-   **startTime**: optioneel. Een UTC datetime wanneer de indexeerfunctie moet zijn gestart.  

<a name="indexer-parameters"></a>

### <a name="parameters"></a>'parameters'

Een indexeerfunctie kan eventueel duren voordat de configuratieparameters die runtime gedrag wijzigen. Parameters voor de configuratie zijn door komma's gescheiden van de indexeerfunctie-aanvraag. 

```json
    {
      "name" : "my-blob-indexer-for-cognitive-search",
      ... other indexer properties
      "parameters" : { "maxFailedItems" : "15", "batchSize" : "100", "configuration" : { "parsingMode" : "json", "indexedFileNameExtensions" : ".json, .jpg, .png", "imageAction" : "generateNormalizedImages", "dataToExtract" : "contentAndMetadata" } }
    }
```

#### <a name="general-parameters-for-all-indexers"></a>Algemene parameters voor alle indexeerfuncties

| Parameter | Type en de toegestane waarden   | Gebruik  |
|-----------|------------|--------------------------|--------|
| `"batchSize"` | Geheel getal<br/>Standaard is specifiek voor bron (1000 voor Azure SQL Database en Azure Cosmos DB, 10 voor Azure Blob Storage) | Geeft het aantal items die zijn gelezen uit de gegevensbron en geïndexeerd als één batch om de prestaties verbeteren. |
| `"maxFailedItems"` | Geheel getal<br/>De standaardwaarde is 0 | Aantal fouten op voordat de uitvoering van een indexeerfunctie wordt beschouwd als een fout tolereren. Ingesteld op -1 als u niet wilt dat eventuele fouten om de indexering proces te stoppen. U kunt informatie ophalen over mislukte items met [indexeerfunctie Status ophalen](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).  |
| `"maxFailedItemsPerBatch"` | Geheel getal<br/>De standaardwaarde is 0 | Het aantal fouten in tolereren in elke batch voordat de uitvoering van een indexeerfunctie wordt beschouwd als een fout. Ingesteld op -1 als u niet wilt dat eventuele fouten om de indexering proces te stoppen. |

#### <a name="blob-configuration-parameters"></a>BLOB-configuratieparameters

Verschillende parameters zijn uitsluitend van toepassing op een bepaalde indexeerfunctie zoals [Azure blob-indexering](search-howto-indexing-azure-blob-storage.md).

| Parameter | Type en de toegestane waarden   | Gebruik  |
|-----------|---------------------------|--------|
| `"parsingMode"` | Reeks<br/>`"text"`<br/>`"delimitedText"`<br/>`"json"`<br/>`"jsonArray"`  | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), is ingesteld op `text` indexering prestaties op tekst zonder opmaak bestanden in blob storage te verbeteren. <br/>Voor [CSV blobs](search-howto-index-csv-blobs.md), is ingesteld op `delimitedText` wanneer blobs gewone CSV-bestanden zijn. <br/>Voor [JSON-blobs](search-howto-index-json-blobs.md), is ingesteld op `json` aan extract gestructureerd inhoud of aan `jsonArray` (preview) om op te halen van afzonderlijke elementen van een matrix als afzonderlijke documenten in Azure Search. |
| `"excludedFileNameExtensions"` | Reeks<br/>door komma's gescheiden lijst | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), negeren van de bestandstypen in de lijst. U kan bijvoorbeeld uitsluiten 'PNG, PNG, MP4' om over te slaan die bestanden tijdens het indexeren. | 
| `"indexedFileNameExtensions"` | Reeks<br/>door komma's gescheiden lijst | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), blobs wordt geselecteerd als de extensie in de lijst. Bijvoorbeeld, u kan zich richten op een specifieke toepassing indexeren bestanden ".docx, PPTX, .msg" specifiek deze bestandstypen opnemen. | 
| `"failOnUnsupportedContentType"` | true <br/>False (standaard) | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), is ingesteld op `false` als u doorgaan met het indexeren wilt als een niet-ondersteund type inhoud is gevonden en u niet alle inhoudstypen (bestandsextensies) van tevoren weet. |
| `"failOnUnprocessableDocument"` | true <br/>False (standaard)| Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), is ingesteld op `false` als u doorgaan indexeren wilt als een document niet te indexeren. |
| `"indexStorageMetadataOnlyForOversizedDocuments"` | true <br/>False (standaard)| Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), deze eigenschap instellen op `true` index nog steeds van metagegevens van de opslag voor blob-inhoud die is te groot om te verwerken.  Grote BLOB's worden behandeld als fouten standaard. Zie voor de limieten voor blobgrootte [Servicelimieten](search-limits-quotas-capacity.md). |
| `"delimitedTextHeaders"` | Reeks<br/>door komma's gescheiden lijst| Voor [CSV-blobs (preview)](search-howto-index-csv-blobs.md), bevat een door komma's gescheiden lijst met kolomkoppen nuttig voor het toewijzen van velden aan doelvelden in een index. |
| `"delimitedTextDelimiter"` | Reeks<br/>Gebruiker is gedefinieerd | Voor [CSV-blobs (preview)](search-howto-index-csv-blobs.md), geeft het einde van regel scheidingsteken voor CSV-bestanden, waarbij elke regel wordt een nieuw document wordt gestart (bijvoorbeeld: ' "|"`).  |
| `"firstLineContainsHeaders"` | True (standaard) <br/>false | Voor [CSV-blobs (preview)](search-howto-index-csv-blobs.md), geeft aan dat de eerste regel (niet-lege) van elke blob kopteksten bevat.|
| `"documentRoot"`  | Reeks<br/>Gebruiker is gedefinieerd | Voor [JSON-matrices (preview)](search-howto-index-json-blobs.md#nested-json-arrays), uitgaande van een document gestructureerde of semi-gestructureerde, kunt u een pad naar de matrix met deze eigenschap. |
| `"dataToExtract"` | Reeks<br/>`"storageMetadata"`<br/>`"allMetadata"`<br/>`"contentAndMetadata"` (standaard) | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md):<br/>Ingesteld op `"storageMetadata"` naar index alleen de [standaard blob-eigenschappen en metagegevens gebruiker opgegeven](../storage/blobs/storage-properties-metadata.md). <br/>Ingesteld op `"allMetadata"` ophalen van metagegevens geleverd door het subsysteem voor Azure blob storage en de [inhoudstype specifieke metagegevens](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (bijvoorbeeld de metagegevens uniek is voor alleen PNG-bestanden) zijn geïndexeerd. <br/>Ingesteld op `"contentAndMetadata"` extraheren van metagegevens en de tekstinhoud van elke blob. <br/><br/>Voor [installatiekopie-analyse in cognitieve zoeken (preview)](cognitive-search-concept-image-scenarios.md)als `"imageAction"` is ingesteld op `"generateNormalizedImages"`, wordt de `"dataToExtract"` instelling geeft de indexeerfunctie welke gegevens ophalen uit de inhoud. Van toepassing op ingesloten afbeelding inhoud in een. PDF- of andere toepassing of afbeeldingsbestanden zoals jpg en PNG in Azure blobs.  |
| `"imageAction"` |  Reeks<br/>`"none"`<br/>`"generateNormalizedImages"` | Voor [Azure blobs](search-howto-indexing-azure-blob-storage.md), is ingesteld op`"none"` ingesloten afbeeldingen of afbeeldingsbestanden in de gegevensset worden genegeerd. Dit is de standaardinstelling. <br/><br/>Voor [installatiekopie-analyse in cognitieve zoeken](cognitive-search-concept-image-scenarios.md), is ingesteld op`"generateNormalizedImages"` Haal de tekst van afbeeldingen (bijvoorbeeld het woord 'stop' van een verkeer Stop-teken) en insluiten als onderdeel van het veld inhoud. Tijdens de analyse van de installatiekopie van de indexeerfunctie wordt een matrix van genormaliseerde installatiekopieën gemaakt als onderdeel van het document kraken en sluit de gegenereerde gegevens in het veld inhoud. Deze actie is vereist dat `"dataToExtract"` is ingesteld op `"contentAndMetadata"`. Een installatiekopie van een genormaliseerde verwijst naar verdere verwerking, wat resulteert in een uniform installatiekopie uitvoer, grootte en gedraaid consistent rendering promoten als u installatiekopieën in de zoekresultaten visual opnemen (bijvoorbeeld dezelfde grootte foto's in een grafiek beheren zoals in de [JFK demo](https://github.com/Microsoft/AzureSearch_JFK_Files)). Wanneer u wordt deze informatie voor elke installatiekopie gegenereerd |


#### <a name="other-configuration-parameters"></a>Andere configuratieparameters

De volgende parameters zijn specifiek voor Azure SQL Database.

| Parameter | Type en de toegestane waarden   | Gebruik  |
|-----------|---------------------------|--------|
| `"queryTimeout"` | Reeks<br/>': mm: ss'<br/>' 00: 05:00 '| Voor [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), stel deze parameter Verhoog de time-out dan de standaardwaarde van 5 minuten.|

<a name="field-mappings"></a>

### <a name="fieldmappings"></a>'fieldMappings'

Indexeerfunctie definities bevatten koppelingen veld voor het toewijzen van een bronveld aan een doelveld in een Azure Search-index. Er zijn twee soorten koppelingen, afhankelijk van of de inhoudsoverdracht een directe of verrijkt pad volgt:

+ **fieldMappings** zijn optioneel, toegepast wanneer de bron-doel veldnamen komen niet overeen, of wanneer u wilt opgeven van een functie.
+ **outputFieldMappings** zijn vereist als u bouwt [een pijplijn verrijking](cognitive-search-concept-intro.md). In een pijplijn verrijking is het uitvoerveld een gedefinieerd tijdens het proces verrijking constructie. De uitvoerveld is bijvoorbeeld mogelijk een samengestelde structuur gebouwd tijdens verrijking van twee afzonderlijke velden in het brondocument. 

In het volgende voorbeeld kunt u overwegen een brontabel met een veld `_id`. Azure Search toegestaan niet in de naam van een veld met een onderstrepingsteken beginnen, zodat de naam van het veld moet worden gewijzigd. U kunt dit doen met behulp van de `fieldMappings` eigenschap van de indexeerfunctie als volgt:

```json
"fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
```

U kunt meerdere veldtoewijzingen opgeven:

```json
"fieldMappings" : [
    { "sourceFieldName" : "_id", "targetFieldName" : "id" },
    { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" }
]
```

Bron- en doelserver veldnamen zijn hoofdlettergevoelig.

Zie voor meer informatie over scenario's waarbij veld-verwijzingen nuttig zijn [zoeken indexeerfunctie veldtoewijzingen](https://docs.microsoft.com/azure/search/search-indexer-field-mappings).

<a name="output-fieldmappings"></a>

### <a name="outputfieldmappings"></a>'outputFieldMappings'

In [cognitieve zoeken](cognitive-search-concept-intro.md) scenario's waarin een vaardigheden is gebonden aan een indexeerfunctie, moet u toevoegen `outputFieldMappings` eventuele uitvoer van een stap verrijking waarmee de inhoud naar een doorzoekbaar veld in de index te koppelen.

```json
  "outputFieldMappings" : [
        {
          "sourceFieldName" : "/document/organizations", 
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyphrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "language",
            "mappingFunction": null
        }      
   ],
```

<a name="FieldMappingFunctions"></a>

### <a name="field-mapping-functions"></a>Veld toewijzing functies

Veldtoewijzingen kunnen ook worden gebruikt voor het transformeren van bron veldwaarden met *veld toewijzing functies*. Bijvoorbeeld, mag een willekeurige tekenreekswaarde base64-gecodeerd zodat deze kan worden gebruikt voor het vullen van het sleutelveld van een document.

Zie voor meer informatie over wanneer en hoe veld toewijzing functies kunt gebruiken, [veld toewijzing functies](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

## <a name="request-examples"></a>Voorbeelden van aanvraag  
 Het eerste voorbeeld maakt u een indexeerfunctie waarmee gegevens worden gekopieerd uit de tabel waarnaar wordt verwezen door de `ordersds` gegevensbron naar de `orders` index volgens een schema dat begint op 1 januari 2015 UTC en wordt elk uur wordt uitgevoerd. Elke aanroep van indexeerfunctie zijn geslaagd als niet meer dan 5 items niet worden geïndexeerd in elke batch en niet meer dan 10 items niet worden geïndexeerd in totaal.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2018-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

Het tweede voorbeeld ziet u een cognitieve zoekbewerking, aangegeven door de verwijzing naar een vaardigheden en [outputFieldMappings](#output-fieldmappings). [Vaardigheden](ref-create-skillset.md) op hoog niveau bronnen, afzonderlijk gedefinieerd. In dit voorbeeld is een afkorting van de definitie van indexeerfunctie in de [cognitieve search-zelfstudie](cognitive-search-tutorial-blob.md).

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
        "sourceFieldName" : "content",
        "targetFieldName" : "content"
    }
   ],
  "outputFieldMappings" : 
  [
    {
        "sourceFieldName" : "/document/organizations", 
        "targetFieldName" : "organizations"
    },
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "configuration": 
    {
    "dataToExtract": "contentAndMetadata",
    "imageAction": "generateNormalizedImages"
    }
  }
}
```

## <a name="response"></a>Antwoord  
 201 gemaakt voor de aanvraag is gelukt.  

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Zoekoverzicht van cognitieve](cognitive-search-concept-intro.md)
+ [Snelstartgids: Probeer cognitieve zoeken](cognitive-search-quickstart-blob.md)
+ [Het toewijzen van velden](cognitive-search-output-field-mapping.md)
