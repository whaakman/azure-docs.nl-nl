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
ms.openlocfilehash: d47b14342caf0312e052584d20f1a9c86ca29cad
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Maak indexeerfunctie (Azure Search Service REST-api-version = 2017-11-11-Preview)

Deze API-verwijzing is een preview-versie van de documentatie die betrekking hebben op cognitieve zoeken verbeteringen op het indexeren.

Net als bij de [algemeen beschikbaar](https://docs.microsoft.com/rest/api/searchservice/create-indexer) versie, kunt u een nieuwe indexeerfunctie binnen een Azure Search-service met behulp van een HTTP POST-aanvraag. 

```http
POST https://[service name].search.windows.net/indexers?api-version=[api-version]  
    Content-Type: application/json  
    api-key: [admin key]  
```  
De **api-sleutel** moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [beveiliging in Azure Search](search-security-overview.md) voor meer informatie over sleutels. [Maak een Azure Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

U kunt ook gebruik van opslag en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat nog, wordt deze gemaakt.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
De **api-versie** is vereist. De huidige versie is `2016-09-01`. Zie [API-versies in Azure Search](search-api-versions.md) voor meer informatie.

Voor gegevens-platform-specifieke richtlijnen over het maken van indexeerfuncties, beginnen met [indexeerfuncties overzicht](search-indexer-overview.md), waaronder de volledige lijst met [verwante artikelen](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  Het maximum aantal toegestane indexeerfuncties varieert per prijscategorie. De gratis service kunnen maximaal 3 indexeerfuncties. Standard-service kan 50 indexeerfuncties. Zie [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.    

## <a name="request"></a>Aanvraag  
 De hoofdtekst van de aanvraag bevat een definitie van een indexeerfunctie, waarmee de gegevensbron en de doelindex voor indexeren, evenals optionele indexering planning en parameters.  

 De syntaxis voor het structureren nettolading van de aanvraag is als volgt. Verderop in dit onderwerp krijgt u een voorbeeld van een aanvraag.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "schedule" : { Optional. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```

### <a name="indexer-schedule"></a>Indexeerfunctie planning  
Een indexeerfunctie kunt optioneel een planning opgeven. Als een planning aanwezig is, de indexeerfunctie periodiek wordt uitgevoerd volgens de planning. De planner is ingebouwd; u kunt een externe scheduler niet gebruiken. **Planning** heeft de volgende kenmerken: 

-   **interval**: vereist. Een duration-waarde die een interval of de periode voor de indexeerfunctie aangeeft wordt uitgevoerd. De minimaal toegestane interval is 5 minuten. het langste is één dag. Moet zijn geformatteerd als de waarde van een XSD-'dayTimeDuration' (een beperkte subset van een [duur van de ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `"P[nD][T[nH][nM]]".` voorbeelden: `PT15M` voor elke 15 minuten `PT2H` voor elke 2 uur.  

-   **startTime**: optioneel. Een UTC datetime wanneer de indexeerfunctie moet zijn gestart.  

### <a name="indexer-parameters"></a>Indexeerfunctie parameters  
 Een indexeerfunctie kunt eventueel verschillende parameters die invloed hebben op het gedrag opgeven. De onderstaande parameters zijn optioneel.  

-   **maxFailedItems**: het aantal items dat niet kan worden geïndexeerd voordat de uitvoerbewerking van de indexeerfunctie wordt beschouwd als een fout. De standaardwaarde is 0. Informatie over mislukte items wordt geretourneerd door de [indexeerfunctie Status ophalen &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) bewerking.  

-   **maxFailedItemsPerBatch**: het aantal items dat niet kan worden geïndexeerd in elke batch voordat de uitvoerbewerking van de indexeerfunctie wordt beschouwd als een fout. De standaardwaarde is 0.  

-   **batchSize:** geeft het aantal items die zijn gelezen uit de gegevensbron en geïndexeerd als één batch om de prestaties verbeteren. De standaardwaarde is afhankelijk van het gegevensbrontype: is 1000 voor Azure SQL en Azure Cosmos DB en 10 voor Azure Blob Storage.

### <a name="field-mapping-parameters"></a>Parameters voor het toewijzen van veld

Indexeerfunctie definities bevatten koppelingen veld voor het toewijzen van een bronveld aan een doelveld in een Azure Search-index. Er zijn twee soorten koppelingen, afhankelijk van of de inhoudsoverdracht een directe of verrijkt pad volgt:

+ **fieldMappings** zijn optioneel, toegepast wanneer de bron-doel veldnamen komen niet overeen, of wanneer u wilt opgeven van een functie.
+ **outputFieldMappings** zijn vereist als u bouwt [een pijplijn verrijking](cognitive-search-concept-intro.md). In een pijplijn verrijking is het uitvoerveld een gedefinieerd tijdens het proces verrijking constructie. De uitvoerveld is bijvoorbeeld mogelijk een samengestelde structuur gebouwd tijdens verrijking van twee afzonderlijke velden in het brondocument. 

#### <a name="fieldmappings"></a>fieldMappings

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

#### <a name="outputfieldmappings"></a>outputFieldMappings

In cognitieve zoeken scenario's waarin een vaardigheden is gebonden aan een indexeerfunctie, moet u toevoegen `outputFieldMappings` eventuele uitvoer van een stap verrijking waarmee de inhoud naar een doorzoekbaar veld in de index te koppelen.

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

#### <a name="field-mapping-functions"></a>Veld toewijzing functies

Veldtoewijzingen kunnen ook worden gebruikt voor het transformeren van bron veldwaarden met *veld toewijzing functies*. Bijvoorbeeld, mag een willekeurige tekenreekswaarde base64-gecodeerd zodat deze kan worden gebruikt voor het vullen van het sleutelveld van een document.

Zie voor meer informatie over wanneer en hoe veld toewijzing functies kunt gebruiken, [veld toewijzing functies](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

### <a name="request-body-examples"></a>Aanvraag hoofdtekst voorbeelden  
 Het volgende voorbeeld wordt een indexeerfunctie waarmee gegevens worden gekopieerd uit de tabel waarnaar wordt verwezen door de `ordersds` gegevensbron naar de `orders` index volgens een schema dat begint op 1 januari 2015 UTC en wordt elk uur wordt uitgevoerd. Elke aanroep van indexeerfunctie zijn geslaagd als niet meer dan 5 items niet worden geïndexeerd in elke batch en niet meer dan 10 items niet worden geïndexeerd in totaal.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

## <a name="response"></a>Antwoord  
 201 gemaakt voor de aanvraag is gelukt.  

## <a name="see-also"></a>Zie ook

+ [Zoekoverzicht van cognitieve](cognitive-search-concept-intro.md)
+ [Snelstartgids: Probeer cognitieve zoeken](cognitive-search-quickstart-blob.md)
+ [Het toewijzen van velden](cognitive-search-output-field-mapping.md)
