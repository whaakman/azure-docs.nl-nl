---
title: Hoe om te met kennis analysemodel (preview beginnen) - Azure Search
description: Meer informatie over de stappen voor het verzenden van verrijkt documenten die zijn gemaakt door AI indexeren in Azure Search die leiden tot een archief kennis in uw Azure storage-account. Van daaruit kunt u bekijken, vorm en verrijkt documenten in Azure Search en andere toepassingen gebruiken.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 06/29/2019
ms.author: heidist
ms.openlocfilehash: e50dfcdc5ac2fbe2435066546a340874e1b8f682
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551059"
---
# <a name="how-to-get-started-with-knowledge-mining-in-azure-search"></a>Aan de slag met kennis analysemodel in Azure Search

> [!Note]
> Kennis store is in preview en niet bedoeld voor gebruik in productieomgevingen. De [2019 in de REST-API-versie-05-06-Preview](search-api-preview.md) biedt deze functie. Er is geen .NET SDK-ondersteuning op dit moment.
>
[Kennis store](knowledge-store-concept-intro.md) AI verrijkt documenten die zijn gemaakt tijdens het indexeren met uw Azure storage-account voor downstream kennis gegevensanalyse in andere apps worden opgeslagen. U kunt ook opgeslagen enrichments gebruiken om te begrijpen en verfijnen van een Azure Search-pijplijn voor indexering. 

Een store kennis wordt gedefinieerd door een *vaardigheden* en die zijn gemaakt door een *indexeerfunctie*. De fysieke expressie van een winkel kennis is opgegeven via *projecties* om te bepalen wanneer de gegevensstructuren in de opslag. Op het moment dat u klaar bent met dit scenario, hebt u gemaakt al deze objecten en u weet hoe ze allemaal bij elkaar passen. 

In deze oefening begint u met voorbeeldgegevens, services en hulpprogramma's voor meer informatie over de algemene werkstroom voor het maken en gebruiken van de store van uw eerste kennis, met nadruk op de definitie van de vaardigheden.

## <a name="prerequisites"></a>Vereisten

Kennis store is in het centrum van meerdere services, met Azure Blob storage en Azure-tabelopslag fysieke opslag en Azure Search en Cognitive Services voor het maken van databaseobject en updates te geven. Vertrouwd zijn met de [basisarchitectuur](knowledge-store-concept-intro.md) is een vereiste voor dit scenario.

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Postman bureaublad-app ophalen](https://www.getpostman.com/), die wordt gebruikt voor het HTTP-aanvragen verzenden naar Azure Search.

+ [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van voorbeeldgegevens en de kennis opslaan. Uw store kennis aanwezig in Azure storage.

+ [Een Cognitive Services-resource maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) op de betalen per gebruik S0-laag voor broad-spectrum toegang tot het volledige bereik van de vaardigheden die in AI enrichments gebruikt. Cognitive Services en uw Azure Search-service moet zich in dezelfde regio bevinden.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken. 

Voorbeeld van JSON-documenten en een Postman collection-bestand zijn ook vereist. Instructies voor het zoeken en aanvullende bestanden laden is opgegeven de [voorbeeldgegevens voorbereiden](#prepare-sample-data) sectie.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

    ![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. U vindt de servicenaam en API-sleutel in elke HTTP-aanvraag in de volgende secties.

<a name="prepare-sample-data"></a>

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

Een knowledge-store bevat de uitvoer van een pijplijn verrijking. Invoer bestaan uit 'onbruikbare' gegevens die uiteindelijk "toegankelijk wordt" terwijl deze wordt uitgevoerd via de pijplijn. Voorbeelden van onbruikbare gegevens mogelijk zijn afbeeldingsbestanden hebt die moeten worden geanalyseerd voor tekst- of image-kenmerken of compacte tekstbestanden die kunnen worden geanalyseerd voor entiteiten, sleuteltermen of sentiment. 

In deze oefening gebruikt compacte tekstbestanden (informatie over het recht van de aanvraag) die afkomstig is van de [Caselaw toegang Project](https://case.law/bulk/download/) downloadpagina openbare grote hoeveelheden gegevens. We een voorbeeld van een 10-document geüpload naar GitHub voor deze oefening. 

In deze taak maakt u een Azure Blob-container voor deze documenten te gebruiken als invoer voor de pijplijn. 

1. Downloaden en uitpakken van de [voorbeeldgegevens voor Azure Search](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) opslagplaats om op te halen de [Caselaw gegevensset](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw). 

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) voorbeeldgegevens bevatten: 

   1. Naam van de container `caselaw-test`. 
   
   1. Het niveau van de openbare toegang ingesteld op een van de geldige waarden.

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk.

   ![Uploaden op de opdrachtbalk](media/search-semi-structured-data/upload-command-bar.png "uploaden op de opdrachtbalk")

1. Navigeer naar de map met de **caselaw sample.json** -voorbeeldbestand. Selecteer het bestand en klik vervolgens op **uploaden**.

1. Wanneer u zich in Azure storage, krijgen de naam van de verbinding tekenreeks en de container.  U moet beide van deze tekenreeksen in [gegevensbron maken](#create-data-source):

   1. Klik in de overzichtspagina op **toegangssleutels** en kopieer een *verbindingsreeks*. Het begint al met `DefaultEndpointsProtocol=https;` en eindigt met `EndpointSuffix=core.windows.net`. Uw accountnaam en -sleutel zijn tussen. 

   1. De containernaam moet `caselaw-test` of zelfgekozen naam geven die u hebt toegewezen.



## <a name="set-up-postman"></a>Postman instellen

Postman is de client-app die u gebruikt voor het verzenden van aanvragen en JSON-documenten in Azure Search. Aantal van de aanvragen kunnen worden geformuleerd met behulp van alleen de informatie in dit artikel. Twee van de grootste aanvragen (het maken van een index, het maken van een set vaardigheden) bevatten echter uitgebreide JSON die is te groot om in te sluiten in een artikel. 

Als u wilt alle van de JSON-documenten en aanvragen volledig beschikbaar maken, hebben we een Postman collection-bestand gemaakt. Is uw eerste taak bij het instellen van de client downloaden en vervolgens dit bestand te importeren.

1. Downloaden en pak deze uit de [voorbeelden van Azure Search Postman](https://github.com/Azure-Samples/azure-search-postman-samples) opslagplaats.

1. Start Postman en de Caselaw Postman-verzameling importeren:

   1. Klik op **importeren** >  **-bestanden importeren** > **bestanden kiezen**. 

   1. Navigeer naar de \azure-search-postman-samples-master\azure-search-postman-samples-master\Caselaw map.

   1. Select **Caselaw.postman_collection_v2.json**. Ziet u vier **POST** aanvragen in de verzameling.

   ![Postman-verzameling voor Caselaw demo](media/knowledge-store-howto/postman-collection.png "Postman-verzameling voor Caselaw-demo")
   

## <a name="create-an-index"></a>Een index maken
    
De eerste aanvraag maakt gebruik van de [API ' Create Index '](https://docs.microsoft.com/rest/api/searchservice/create-data-source), een Azure Search-index te maken die alle doorzoekbare gegevens worden opgeslagen. Een index geeft alle velden, parameters en kenmerken.

U hoeft niet per se een index voor knowledge gegevensanalyse, maar een indexeerfunctie niet uitgevoerd, tenzij een index wordt geleverd. 

1. In de URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexes?api-version=2019-05-06-Preview`, Vervang `YOUR-AZURE-SEARCH-SERVICE-NAME` met de naam van uw search-service. 

1. Vervang in de sectie Header `<YOUR AZURE SEARCH ADMIN API-KEY>` met een beheer-API-sleutel voor Azure Search.

1. In de sectie instantie is het JSON-document een indexschema. Samengevouwen voor zichtbaarheid, bestaat de buitenste shell van een index uit de volgende elementen. De Veldenverzameling komt overeen met de velden in de gegevensset caselaw.

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

1. Vouw de `fields` verzameling. Het bevat het grootste deel van de indexdefinitie bestaat uit eenvoudige velden [complexe velden](search-howto-complex-data-types.md) met geneste substructuren en verzamelingen.

   Neem even de tijd om te controleren van de velddefinitie voor de `casebody` complexe veld op regels 302-384. U ziet dat een complexe veld andere complexe velden bevatten kan als hiërarchische weergaven nodig is. Hiërarchische structuren kunnen worden gemodelleerd in een index, zoals wordt weergegeven, ook als een projectie, hier, en in een set vaardigheden, waardoor het maken van een geneste gegevensstructuur in het archief met kennis.

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

1. Klik op **verzenden** voor het uitvoeren van de aanvraag.  U krijgt een **Status: 201-gemaakt** bericht als antwoord.

<a name="create-data-source"></a>

## <a name="create-a-data-source"></a>Een gegevensbron maken

Maakt gebruik van de tweede aanvraag de [Data Source-API maken](https://docs.microsoft.com/rest/api/searchservice/create-data-source) verbinding maken met Azure Blob-opslag. 

1. In de URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/datasources?api-version=2019-05-06-Preview`, Vervang `YOUR-AZURE-SEARCH-SERVICE-NAME` met de naam van uw search-service. 

1. Vervang in de sectie Header `<YOUR AZURE SEARCH ADMIN API-KEY>` met een beheer-API-sleutel voor Azure Search.

1. In de sectie instantie bevat het JSON-document connection string en blob-containernaam van uw opslagaccount. De verbindingsreeks in Azure portal in uw storage-account vindt **toegangssleutels**. 

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<YOUR-BLOB-CONTAINER-NAME>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

1. Klik op **verzenden** voor het uitvoeren van de aanvraag.  U krijgt een **Status: 201-gemaakt** bericht als antwoord.



<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Maak een winkel vaardigheden en kennis

Maakt gebruik van de derde aanvraag de [vaardigheden API maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset), het maken van een Azure Search-object die Hiermee geeft u op welke cognitieve vaardigheden om aan te roepen, hoe u vaardigheden samen en het belangrijkste is dat voor dit scenario - hoe u een archief kennis op te geven.

1. In de URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/skillsets?api-version=2019-05-06-Preview`, Vervang `YOUR-AZURE-SEARCH-SERVICE-NAME` met de naam van uw search-service. 

1. Vervang in de sectie Header `<YOUR AZURE SEARCH ADMIN API-KEY>` met een beheer-API-sleutel voor Azure Search.

1. In de sectie instantie is het JSON-document een definitie vaardigheden. Samengevouwen voor zichtbaarheid, bestaat de buitenste shell van een set vaardigheden uit de volgende elementen. De `skills` verzameling definieert de enrichments in het geheugen, maar de `knowledgeStore` definitie Hiermee geeft u op hoe de uitvoer wordt opgeslagen. De `cognitiveServices` definitie is uw connectie met de AI-verrijking-engines.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

1. Vouw `cognitiveServices` en `knowledgeStore` zodat u gegevens kunt opgeven. In het voorbeeld zijn deze tekenreeksen gevonden na de definitie van de vaardigheden aan het einde van de aanvraagtekst. 

   Voor `cognitiveServices`, inrichten van een resource op de S0-laag, die zich in dezelfde regio als de Azure Search. U krijgt de cognitiveServices naam en sleutel van de dezelfde pagina in Azure portal. 
   
   Voor `knowledgeStore`, kunt u de dezelfde verbindingsreeks die wordt gebruikt voor de caselaw Blob-container.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "YOUR-SAME-REGION-S0-COGNITIVE-SERVICES-RESOURCE",
        "key": "YOUR-COGNITIVE-SERVICES-KEY"
    },
    "knowledgeStore": {
        "storageConnectionString": "YOUR-STORAGE-ACCOUNT-CONNECTION-STRING",
    ```

1. Vouw de verzameling vaardigheden in het bijzonder de vaardigheden Shaper op regels 85 en 179, respectievelijk. De kwalificatie Shaper is belangrijk omdat deze de gegevensstructuren die u wilt gebruiken voor knowledge analysestructuur ophaalprotocol. Tijdens de uitvoering van vaardigheden, deze structuren zijn alleen in het geheugen, maar als u naar de volgende stap gaat, ziet u hoe deze uitvoer naar een winkel kennis voor verder onderzoek kan worden opgeslagen.

   Het volgende codefragment is afkomstig van regel 217. 

    ```json
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
   . . .
   ```

1. Vouw `projections` -element in `knowledgeStore`, beginnend op regel 262. Projecties opgeven de samenstelling van de store kennis. Projecties worden in paren met tabellen-objecten, maar momenteel maar één keer opgegeven. Zoals u in de eerste projectie ziet `tables` is opgegeven maar `objects` niet. In de tweede is het tegenovergestelde.

   Tabellen wordt gemaakt in de tabelopslag voor elke tabel die u maakt in Azure-opslag, en elk object Hiermee haalt u een container in Blob-opslag.

   BLOB-objecten bevatten doorgaans de volledige expressie van een verrijking. Tabellen bevatten doorgaans gedeeltelijke enrichments, in de combinaties die u ervoor zorgen dat specifieke doeleinden. In dit voorbeeld ziet u een tabel aanvragen en een tabel met adviezen, maar niet weergegeven, zijn andere tabellen, zoals entiteiten, advocaten, volgens de taak en partijen.

    ```json
    "projections": [
        {
            "tables": [
                {
                    "tableName": "Cases",
                    "generatedKeyName": "CaseId",
                    "source": "/document/Case"
                },
                {
                    "tableName": "Opinions",
                    "generatedKeyName": "OpinionId",
                    "source": "/document/Case/OpinionsSnippets/*"
                }
            ],
            "objects": []
        },
        {
            "tables": [],
            "objects": [
                {
                    "storageContainer": "enrichedcases",
                    
                    "source": "/document/CaseFull"
                }
            ]
        }
    ]
    ```

1. Klik op **verzenden** voor het uitvoeren van de aanvraag. Het antwoord moet zijn **201** en er ongeveer als volgt het volgende voorbeeld wordt het eerste deel van het antwoord weergegeven.

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

De vierde aanvraag maakt gebruik van de [Indexer-API maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer), het maken van een Azure Search-indexeerfunctie. Een indexeerfunctie is de engine voor het uitvoeren van de pijplijn voor indexering. Alle van de definities die u tot nu toe hebt gemaakt zijn in beweging bij deze stap geplaatst.

1. In de URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexers?api-version=2019-05-06-Preview`, Vervang `YOUR-AZURE-SEARCH-SERVICE-NAME` met de naam van uw search-service. 

1. Vervang in de sectie Header `<YOUR AZURE SEARCH ADMIN API-KEY>` met een beheer-API-sleutel voor Azure Search.

1. Hiermee geeft u de naam van de indexeerfunctie in de sectie instantie op de JSON-document. Een gegevensbron en index zijn vereist door de indexeerfunctie. Een set vaardigheden is optioneel voor een indexeerfunctie, maar vereist voor verrijking AI.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
    ```

1. Vouw outputFieldMappings. In tegenstelling tot fieldMappings, die worden gebruikt voor de aangepaste toewijzing tussen de velden in een gegevensbron en velden in een index, worden de outputFieldMappings gebruikt voor het toewijzen van verrijkt velden, gemaakt en ingevuld door de pijplijn, uitvoer velden in een index of de projectie.

    ```json
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
    ```

1. Klik op **verzenden** voor het uitvoeren van de aanvraag. Het antwoord moet zijn **201** en hoofdtekst van het antwoord ziet er bijna identiek aan de aanvraaglading die u hebt opgegeven (bijgesneden voor beknoptheid).

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
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
