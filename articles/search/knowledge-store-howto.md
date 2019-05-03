---
title: Aan de slag met kennis Store (preview) - Azure Search
description: Meer informatie over de stappen voor het verzenden van verrijkt documenten die zijn gemaakt door AI indexeren in Azure Search die leiden tot een archief kennis in uw Azure storage-account. Van daaruit kunt u bekijken, vorm en verrijkt documenten in Azure Search en andere toepassingen gebruiken.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 2a904cfb049af413887798c8aab449561bc2b73f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026968"
---
# <a name="how-to-get-started-with-knowledge-store"></a>Aan de slag met kennis Store

[Kennis Store](knowledge-store-concept-intro.md) is een nieuwe previewfunctie in Azure Search die wordt opgeslagen als AI enrichments gemaakt in een pijplijn indexering voor knowledge-gegevensanalyse in andere apps. U kunt ook opgeslagen enrichments gebruiken om te begrijpen en verfijnen van een Azure Search-pijplijn voor indexering.

Een store kennis wordt gedefinieerd door een set vaardigheden. Voor reguliere's voor Azure Search-zoekopdracht in volledige tekst, is het leveren van het doel van een set vaardigheden AI enrichments zodat inhoud meer doorzoekbaar. Voor knowledge store scenario's, is de rol van een set vaardigheden maken en vullen met meerdere gegevensstructuren voor knowledge gegevensanalyse.

In deze oefening begint u met voorbeeldgegevens, services en hulpprogramma's voor meer informatie over de algemene werkstroom voor het maken en gebruiken van de store van uw eerste kennis, met nadruk op de definitie van de vaardigheden.

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogramma's en gegevens worden gebruikt in deze Quick Start. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken. 

+ [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van de voorbeeldgegevens. Uw store kennis aanwezig in Azure storage.

+ [Een Cognitive Services-resource maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) op de betalen per gebruik S0-laag voor broad-spectrum toegang tot het volledige bereik van de vaardigheden die in AI enrichments gebruikt.

+ [Postman bureaublad-app](https://www.getpostman.com/) voor het verzenden van aanvragen naar Azure Search.

+ [Postman-verzameling](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/caselaw) met voorbereide aanvragen voor het maken van een gegevensbron, index, vaardigheden en indexeerfunctie. Er zijn verschillende objectdefinities te lang om op te nemen in dit artikel. U moet deze verzameling om te zien van de definities van de index en vaardigheden in hun geheel ophalen.

+ [Voorbeeldgegevens Caselaw](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) die afkomstig zijn van de [Caselaw toegang Project](https://case.law/bulk/download/) downloadpagina openbare grote hoeveelheden gegevens. De oefening gebruikt met name de eerste 10 documenten van het downloaden van de eerste (Arkansas). We een voorbeeld van een 10-document geüpload naar GitHub voor deze oefening.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

    ![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service.

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) voorbeeldgegevens bevatten. U kunt het niveau van de openbare toegang instellen op een van de geldige waarden.

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk.

   ![Uploaden op de opdrachtbalk](media/search-semi-structured-data/upload-command-bar.png "uploaden op de opdrachtbalk")

1. Navigeer naar de map met de **caselaw sample.json** -voorbeeldbestand. Selecteer het bestand en klik vervolgens op **uploaden**.


## <a name="set-up-postman"></a>Postman instellen

Start Postman en stel een HTTP-aanvraag in. Als u niet bekend met dit hulpprogramma bent, raadpleegt u [verkennen Azure Search REST API's met Postman](search-fiddler.md).

+ Aanvraagmethode voor elke aanroep in dit scenario is **POST**.
+ Aanvraagheaders (2) omvatten het volgende: "Content-type' ingesteld op 'application/json', 'api-key' ingesteld op 'admin key' (admin key is een tijdelijke aanduiding voor de primaire sleutel van uw zoekopdracht) respectievelijk. 
+ De aanvraagtekst is waar u de werkelijke inhoud van de aanroep van plaatsen. 

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/postmanoverview.png)

We gebruiken Postman voor vier API-aanroepen naar uw zoekservice, het maken van een gegevensbron, een index, een set vaardigheden en een indexeerfunctie. De gegevensbron bevat een verwijzing naar uw opslagaccount en JSON-gegevens. Uw zoekservice maakt de verbinding bij het importeren van de gegevens.

[Maken van een set vaardigheden](#create-skillset) wordt de focus van dit scenario: Hiermee geeft u de stappen verrijking en hoe gegevens worden opgeslagen in een winkel kennis.

URL-eindpunt moet opgeven, een api-versie en elke aanroep moeten retourneren een **201-gemaakt**. De preview-api-versie voor het maken van een set vaardigheden met ondersteuning voor het opslaan van kennis is `2019-05-06-Preview`.

De volgende API-aanroepen van de REST-client uitvoeren.

## <a name="create-a-data-source"></a>Een gegevensbron maken

De [Data Source-API maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source) maakt een Azure Search-object waarmee wordt aangegeven welke gegevens moeten worden geïndexeerd.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview` 

1. Vervang `[service name]` door de naam van uw zoekservice. 

2. Voor deze aanroep moet de aanvraagtekst connection string en blob-containernaam van uw opslagaccount bevatten. De verbinding kan worden gevonden in de Azure-portal in uw storage-account **toegangssleutels**. 

   Zorg ervoor dat u de tekenreeks als de blob-container verbindingsnaam in de hoofdtekst van de aanvraag Vervang voordat u de aanroep uitvoert.

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your storage key>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<your blob container name>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

3. Verzend de aanvraag. Het antwoord moet zijn **201** en hoofdtekst van het antwoord ziet er bijna identiek aan de nettolading van de aanvraag die u hebt opgegeven.

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your storage key>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<your blob container name>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

## <a name="create-an-index"></a>Een index maken
    
De tweede aanroep [API ' Create Index '](https://docs.microsoft.com/rest/api/searchservice/create-data-source), een Azure Search-index te maken die alle doorzoekbare gegevens worden opgeslagen. Een index geeft alle velden, parameters en kenmerken.

U hoeft niet per se een index voor knowledge gegevensanalyse, maar een indexeerfunctie niet uitgevoerd, tenzij een index wordt geleverd. 

De URL voor deze aanroep is `https://[service name].search.windows.net/indexes?api-version=2019-05-06-Preview`

1. Vervang `[service name]` door de naam van uw zoekservice.

2. Kopieer de indexdefinitie van de aanvraag Create Index in de Postman-verzameling in de aanvraagtekst. De indexdefinitie van de is enkele honderden regels die te lang om hier af te drukken. 

   De buitenste shell van een index bestaat uit de volgende elementen. 

   ```json
   {
      "name": "caselaw",
      "defaultScoringProfile": null,
      "fields": [],
      "scoringProfiles": [],
      "corsOptions": null,
      "suggesters": [],
      "analyzers": [],
      "tokenizers": [],
      "tokenFilters": [],
      "charFilters": [],
      "encryptionKey": null
   }
   ```

3. De `fields` verzameling bevat het grootste deel van de indexdefinitie. Het eenvoudige velden bevat [complexe velden](search-howto-complex-data-types.md) met geneste substructuren en verzamelingen.

   Controleer de velddefinitie voor `casebody` op regels 302-384. U ziet dat een complexe veld andere complexe velden bevatten kan als hiërarchische weergaven nodig is.

   ```json
   {
    "name": "casebody",
    "type": "Edm.ComplexType",
    "fields": [
        {
            "name": "status",
            "type": "Edm.String",
            "searchable": true,
            "filterable": true,
            "retrievable": true,
            "sortable": true,
            "facetable": true,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "data",
            "type": "Edm.ComplexType",
            "fields": [
                {
                    "name": "head_matter",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "retrievable": true,
                    "sortable": false,
                    "facetable": false,
                    "key": false,
                    "indexAnalyzer": null,
                    "searchAnalyzer": null,
                    "analyzer": null,
                    "synonymMaps": []
                },
                {
                    "name": "opinions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "author",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": false,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "type",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        }
                    ]
                },
    . . .
   ```

4. Verzend de aanvraag. 

   Het antwoord moet zijn **201** en het volgende voorbeeld wordt de eerste verschillende velden weergegeven als volgt uitzien:

    ```json
    {
        "name": "caselaw",
        "defaultScoringProfile": null,
        "fields": [
            {
                "name": "id",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "retrievable": true,
                "sortable": true,
                "facetable": true,
                "key": true,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
            {
                "name": "name",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "retrievable": true,
                "sortable": true,
                "facetable": true,
                "key": false,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
      . . .
    ```

<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Maak een winkel vaardigheden en kennis

De [vaardigheden API maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset) maakt een Azure Search-object waarmee wordt aangegeven welke cognitieve vaardigheden om aan te roepen, hoe u vaardigheden samen en het belangrijkste is dat voor dit scenario - hoe u een archief kennis op te geven.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/skillsets?api-version=2019-05-06-Preview`

1. Vervang `[service name]` door de naam van uw zoekservice.

2. Kopieer de definitie van de vaardigheden van de aanvraag vaardigheden maken in de Postman-verzameling in de aanvraagtekst. De definitie van de vaardigheden is enkele honderden regels die te lang om af te drukken, maar het is de focus van dit scenario.

   De buitenste shell van een set vaardigheden bestaat uit de volgende elementen. De `skills` verzameling definieert de enrichments in het geheugen, maar de `knowledgeStore` definitie Hiermee geeft u op hoe de uitvoer wordt opgeslagen. De `cognitiveServices` definitie is uw connectie met de AI-verrijking-engines.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

3. Stel eerst `cognitiveServices` en `knowledgeStore` sleutel en de verbindingsreeks. In het voorbeeld zijn deze tekenreeksen gevonden na de definitie van de vaardigheden aan het einde van de aanvraagtekst.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "<your cognitive services resource name>",
        "key": "<your cognitive services key>"
    },
    "knowledgeStore": {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your storage account key>;EndpointSuffix=core.windows.net",
    ```

3. Controleer de vaardigheden verzameling, met name de vaardigheden Shaper op regels 85 en 170, respectievelijk. De kwalificatie Shaper is belangrijk omdat deze de gegevensstructuren die u wilt gebruiken voor knowledge analysestructuur ophaalprotocol. Tijdens de uitvoering van vaardigheden, deze structuren zijn alleen in het geheugen, maar als u naar de volgende stap gaat, ziet u hoe deze uitvoer naar een winkel kennis voor verder onderzoek kan worden opgeslagen.

   Het volgende codefragment is afkomstig van regel 207. 

    ```json
    {
    "name": "Opinions",
    "source": null,
    "sourceContext": "/document/casebody/data/opinions/*",
    "inputs": [
        {
            "name": "Text",
            "source": "/document/casebody/data/opinions/*/text"
        },
        {
            "name": "Author",
            "source": "/document/casebody/data/opinions/*/author"
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/casebody/data/opinions/*/text/pages/*/entities/*",
            "inputs": [
                {
                    "name": "Entity",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/value"
                },
                {
                    "name": "EntityType",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/category"
                }
             ]
          }
     ]
   }
   . . .
   ```

3. Controleer de `projections` -element in `knowledgeStore`, beginnend op regel 253. Projecties opgeven de samenstelling van de store kennis. Projecties worden in paren met tabellen-objecten, maar momenteel maar één keer opgegeven. Zoals u in de eerste projectie ziet `tables` is opgegeven maar `objects` niet. In de tweede is het tegenovergestelde.

   Tabellen wordt gemaakt in de tabelopslag voor elke tabel die u maakt in Azure-opslag, en elk object Hiermee haalt u een container in Blob-opslag.

   Objecten bevatten doorgaans de volledige expressie van een verrijking. Tabellen bevatten doorgaans gedeeltelijke enrichments, in de combinaties die u ervoor zorgen dat specifieke doeleinden. In dit voorbeeld ziet u een tabel gevallen, maar niet weergegeven andere tabellen, zoals entiteiten, volgens de taak en adviezen zijn.

    ```json
    "projections": [
    {
        "tables": [
            {
              "tableName": "Opinions",
              "generatedKeyName": "OpinionId",
              "source": "/document/Case/OpinionsSnippets/*"
            },
          . . . 
        ],
        "objects": []
    },
    {
        "tables": [],
        "objects": [
            {
                "storageContainer": "enrichedcases",
                "key": "/document/CaseFull/Id",
                "source": "/document/CaseFull"
            }
          ]
        }
      ]
    }
    ```

5. Verzend de aanvraag. Het antwoord moet zijn **201** en er ongeveer als volgt het volgende voorbeeld wordt het eerste deel van het antwoord weergegeven.

    ```json
    {
    "name": "caselaw-ss",
    "description": null,
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "name": "SplitSkill#1",
            "description": null,
            "context": "/document/casebody/data/opinions/*/text",
            "defaultLanguageCode": "en",
            "textSplitMode": "pages",
            "maximumPageLength": 5000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/casebody/data/opinions/*/text
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        . . .
    ```

## <a name="create-and-run-an-indexer"></a>Maken en een indexeerfunctie uitvoeren

De [Indexer-API maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer) gemaakt en een indexeerfunctie onmiddellijk uitgevoerd. Alle van de definities die u tot nu toe hebt gemaakt zijn in beweging bij deze stap geplaatst. De indexeerfunctie wordt onmiddellijk uitgevoerd omdat deze niet in de service bestaat. Nadat deze bestaat, wordt een POST-aanroep naar een bestaande indexeerfunctie een updatebewerking.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview`

1. Vervang `[service name]` door de naam van uw zoekservice. 

2. De aanvraagtekst voor deze aanroep Hiermee geeft u de naam van de indexeerfunctie. Een gegevensbron en index zijn vereist door de indexeerfunctie. Een set vaardigheden is optioneel voor een indexeerfunctie, maar vereist voor verrijking AI.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
        "outputFieldMappings": [
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
                "targetFieldName": "people",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/organizations/*",
                "targetFieldName": "orginizations",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/locations/*",
                "targetFieldName": "locations",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/Case/OpinionsSnippets/*/Entities/*",
                "targetFieldName": "entities",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/keyPhrases/*",
                "targetFieldName": "keyPhrases",
                "mappingFunction": null
            }
        ]
    }
    ```

3. Verzend de aanvraag. Het antwoord moet zijn **201** en hoofdtekst van het antwoord ziet er bijna identiek aan de aanvraaglading die u hebt opgegeven (bijgesneden voor beknoptheid).

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
        "outputFieldMappings": [
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
                "targetFieldName": "people",
                "mappingFunction": null
            }
        ]
    }
    ```

## <a name="explore-knowledge-store"></a>Kennis store verkennen

U kunt beginnen met het verkennen, zodra het eerste document wordt geïmporteerd. Gebruik voor deze taak [ **Opslagverkenner** ](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer) in de portal.

Het is belangrijk om te profiteren van dat een winkel kennis volledig losgekoppeld van Azure Search is. De Azure Search-index en de kennis store beide weergave van gegevens en de inhoud echter deel manieren van daaruit bevatten. De index voor zoeken in volledige tekst, gefilterde zoeken en alle scenario's die worden ondersteund in Azure Search gebruiken. Of vooruit met slechts uw kennis store, die zijn bezig met koppelen van andere hulpprogramma's voor het analyseren van inhoud.

## <a name="takeaways"></a>Opgedane kennis

U hebt nu uw eerste kennis store gemaakt in Azure storage en gebruikt Storage Explorer om de enrichments weer te geven. Dit is de fundamentele ervaring voor het werken met opgeslagen enrichments. 

## <a name="next-steps"></a>Volgende stappen

De kwalificatie Shaper voert het zware werk voor het maken van formulieren met gedetailleerde gegevens die kunnen worden gecombineerd in nieuwe vormen. Bekijk de referentiepagina voor deze kwalificatie voor meer informatie over hoe deze wordt gebruikt als de volgende stap.

> [!div class="nextstepaction"]
> [Naslaginformatie over shaper-kwalificatie](cognitive-search-skill-shaper.md)


<!---
## Keep This

How to convert unformatted JSON into an indented JSON document structure that allows you to quickly identify nested structures. Useful for creating an index that includes complex types.

1. Use Visual Studio Code.
2. Open data.jsonl
--->