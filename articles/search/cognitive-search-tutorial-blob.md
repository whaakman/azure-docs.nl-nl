---
title: "Zelfstudie: API's voor cognitief zoeken aanroepen in Azure Search | Microsoft Docs"
description: Voorbeeld van gegevensextractie, natuurlijke taal en AI-beeldverwerking in Azure Search indexeren voor gegevensextractie en transformatie.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 023e55724124afb9c97d01540c6319a775681c84
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-learn-how-to-call-cognitive-search-apis-preview"></a>Zelfstudie: API's voor cognitief zoeken aanroepen (preview)

In deze zelfstudie leert u de mechanismen achter gegevensverrijking programmeren in Azure Search met behulp van *cognitieve vaardigheden*. Cognitieve vaardigheden zijn bewerkingen voor de verwerking van natuurlijke taal (NLP) en afbeeldingsanalyse die tekst en tekstweergaven van een afbeelding extraheren en taal, entiteiten, sleuteltermen en meer detecteren. Het eindresultaat is uitgebreide aanvullende inhoud in een Azure Search-index, gemaakt met een indexeringspijplijn voor cognitief zoeken. 

In deze zelfstudie maakt u REST API-aanroepen om de volgende taken uit te voeren:

> [!div class="checklist"]
> * Een indexeringspijplijn maken die brongegevens onderweg naar een index verrijkt
> * Vaardigheden voor ingebouwde herkenning van entiteiten, taaldetectie, tekstbewerkingen en sleuteltermextractie gebruiken in een set voorbeeldgegevens
> * Vaardigheden aan elkaar koppelen door invoeren aan uitvoeren toe te wijzen in een set vaardigheden
> * Aanvragen uitvoeren en resultaten bekijken
> * De index en indexeerfuncties opnieuw instellen voor verdere ontwikkeling

Uitvoer is een volledige doorzoekbare index in Azure Search. U kunt de index uitbreiden met andere standaardfuncties, zoals [synoniemen](search-synonyms.md), [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [analyses](search-analyzers.md) en [filters](search-filters.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Is cognitief zoeken nieuw voor u? Lees dan [Wat is cognitief zoeken?](cognitive-search-concept-intro.md) voor een introductie of ga naar de [snelstart in de portal](cognitive-search-quickstart-blob.md) voor een praktische inleiding tot belangrijke concepten.

Als u REST-aanroepen naar Azure Search wilt maken, kunt u PowerShell of een online testhulpprogramma zoals Telerik Fiddler of Postman gebruiken om HTTP-aanvragen te formuleren. Zie [REST API's voor Azure Search verkennen met Fiddler of Postman](search-fiddler.md) als u deze hulpprogramma's nog niet kent.

Gebruik [Azure Portal](https://portal.azure.com/) voor het maken van services die worden gebruikt in een end-to-end-werkstroom. 

### <a name="set-up-azure-search"></a>Azure Search instellen

Registreer u eerst voor de Azure Search-service. 

1. Ga naar [Azure Portal](https://portal.azure.com) en meld u aan met uw Azure-account.

1. Klik op **Een resource maken**, zoek naar Azure Search en klik op **Maken**. Zie [Een Azure Search-service maken in de portal](search-create-service-portal.md) als u voor de eerste keer een zoekservice instelt.

  ![Dashboard van portal](./media/cognitive-search-tutorial-blob/create-service-full-portal.png "Een Azure Search-service maken in de portal")

1. Maak een resourcegroep voor alle resources die u in deze zelfstudie gaat maken. Dit vergemakkelijkt het opschonen van de resources nadat u de zelfstudie hebt voltooid.

1. Kies bij locatie voor **Zuid-centraal VS** of **West-Europa**. Op dit moment is de preview alleen in deze regio's beschikbaar.

1. Voor de prijscategorie kunt u een **Gratis** service maken om de zelfstudies en snelstarts te voltooien. Voor nadere analyse met behulp van uw eigen gegevens, maakt u een [betaalde service](https://azure.microsoft.com/pricing/details/search/) zoals **Basic** of **Standard**. 

  Een gratis service is beperkt tot 3 indexen, maximaal 16 MB aan blobgrootte en 2 minuten indexeren. Dit is voldoende om de volledige functionaliteit van cognitief zoeken te verkennen. Zie [Servicelimieten](search-limits-quotas-capacity.md) om de limieten voor verschillende prijscategorieën te bekijken.

  > [!NOTE]
  > Cognitief zoeken is een openbare preview. De uitvoering van vaardigheden is momenteel in alle prijscategorieën beschikbaar, waaronder de gratis categorie. De prijzen voor deze functionaliteit worden op een later moment bekend gemaakt.

1. Maak de service vast aan het dashboard voor snelle toegang tot service-informatie.

  ![Pagina Servicedefinitie in de portal](./media/cognitive-search-tutorial-blob/create-search-service.png "Pagina Servicedefinitie in de portal")

1. Nadat de service is gemaakt, verzamelt u de volgende informatie: de **URL** van de pagina Overzicht en **api-key** (primair of secundair) van de pagina Sleutels.

  ![Eindpunt- en sleutelinformatie in de portal](./media/cognitive-search-tutorial-blob/create-search-collect-info.png "Eindpunt- en sleutelinformatie in de portal")

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Azure Blob service instellen en voorbeeldgegevens laden

De verrijkingspijplijn haalt gegevens uit Azure-gegevensbronnen. Brongegevens moeten afkomstig zijn van een ondersteund type gegevensbron van een [Azure Search-indexeerfunctie](search-indexer-overview.md). In dit voorbeeld gebruiken we blobopslag om meerdere inhoudstypen te laten zien.

1. [Download de voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). Voorbeeldgegevens bestaan uit een kleine set van verschillende typen bestanden. 

1. Meld u aan bij Azure Blob-opslag, maak een opslagaccount, meld u aan bij Storage Explorer en maak een container met de naam `basicdemo`. Zie [Azure Storage Explorer Quickstart](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) (Snelstart voor Azure Storage Explorer) voor instructies over alle stappen.

1. Klik in de container `basicdemo` die u hebt gemaakt met behulp van Azure Storage Explorer op **Uploaden** om de voorbeeldbestanden te uploaden.

1. Nadat de voorbeeldbestanden zijn geladen, haalt u de containernaam en een verbindingsreeks voor de Blob-opslag op. U kunt dat doen door in Azure Portal naar uw opslagaccount te navigeren. Ga naar **Toegangssleutels** en kopieer het veld **Verbindingsreeks**.

  De verbindingsreeks moet een URL zijn die er ongeveer als volgt uitziet:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=y1NIlE9wFVBIabcd562GzZl+JO9TEGdqOerqfbT78C8zrn28Te8DsWlxvKKnjh67P/HM5k80zt4shOt9vqlbg==;EndpointSuffix=core.windows.net
      ```

Er zijn andere manieren om de verbindingsreeks op te geven, zoals een Shared Access Signature bieden. Zie [Indexing Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials) (Azure Blob Storage indexeren) voor meer informatie over referenties voor gegevensbronnen.

## <a name="create-a-data-source"></a>Een gegevensbron maken

Nu dat uw services en bronbestanden zijn voorbereid, kunt u de onderdelen van uw indexeringspijplijn samenstellen. Begin met een [gegevensbronobject](https://docs.microsoft.com/rest/api/searchservice/create-data-source) dat Azure Search vertelt hoe het externe gegevens moet ophalen.

Gebruik voor deze zelfstudie de REST API en een hulpprogramma dat HTTP-aanvragen kan formuleren en verzenden, zoals PowerShell, Postman of Fiddler. Geef in de aanvraagheader de naam van de service op die u hebt gebruikt bij het maken van de Azure Search-service en de API-sleutel die is gegenereerd voor uw zoekservice. Geef in de aanvraagbody de naam van de blobcontainer en de verbindingsreeks op.

### <a name="sample-request"></a>Voorbeeldaanvraag
```http
POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
Content-Type: application/json  
api-key: [admin key]  
```
#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
```json
{   
    "name" : "demodata",  
    "description" : "Demo files to demonstrate cognitive search capabilities.",  
    "type" : "azureblob",
    "credentials" :
    { "connectionString" :
      "DefaultEndpointsProtocol=https;AccountName=<your account name>;AccountKey=<your account key>;"
    },  
    "container" : { "name" : "<your blob container name>" }
}  
```
Verzend de aanvraag. Het online hulpprogramma zou een 201-statuscode moeten retourneren om de succesvolle uitvoering te bevestigen. 

Aangezien dit uw eerste aanvraag is, controleert u Azure Portal om te bevestigen dat de gegevensbron in Azure Search is gemaakt. Controleer op de dashboardpagina van de zoekservice of de tegel Gegevensbronnen een nieuw item heeft. U moet mogelijk een paar minuten wachten tot de portalpagina is vernieuwd. 

  ![Tegel Gegevensbronnen in de portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Tegel Gegevensbronnen in de portal")

Als u een 403- of 404-fout krijgt, controleert u de constructie van de aanvraag: `api-version=2017-11-11-Preview` moet in het eindpunt staan, `api-key` in de header na `Content-Type`, en de waarde ervan moet geldig voor een zoekservice zijn. U kunt de header hergebruiken voor de overige stappen in deze zelfstudie.

> [!TIP]
> Voordat u veel werk uitvoert, is het goed om te controleren of de zoekservice wordt uitgevoerd in een van de ondersteunde locaties die de preview-functie bieden: Zuid-centraal VS of West-Europa.

## <a name="create-a-skillset"></a>Een set vaardigheden maken

In deze stap definieert u een reeks verrijkingsstappen die u op uw gegevens wilt toepassen. We noemen elke verrijkingsstap een *vaardigheid* en de reeks verrijkingsstappen een *set vaardigheden*. In deze zelfstudie wordt gebruikgemaakt van [vooraf gedefinieerde cognitieve vaardigheden](cognitive-search-predefined-skills.md) voor de set vaardigheden:

+ [Taaldetectie](cognitive-search-skill-language-detection.md) om de taal van de inhoud vast te stellen.

+ [Tekst splitsen](cognitive-search-skill-textsplit.md) om grote inhoud in kleinere stukken op te delen voordat u de vaardigheid voor sleuteltermextractie aanroept. Sleuteltermextractie accepteert invoeren van 50.000 tekens of minder. Enkele voorbeeldbestanden moeten worden opgesplitst om aan deze limiet te voldoen.

+ [Herkenning van benoemde entiteiten](cognitive-search-skill-named-entity-recognition.md) voor het extraheren van de namen van organisaties uit inhoud van de blobcontainer.

+ [Sleuteltermextractie](cognitive-search-skill-keyphrases.md) voor het ophalen van de belangrijkste sleuteltermen. 

### <a name="sample-request"></a>Voorbeeldaanvraag
Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Deze aanvraag maakt een set vaardigheden. Gebruik de naam van de set vaardigheden ```demoskillset``` voor de rest van deze zelfstudie.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
```json
{
  "description": 
  "Extract entities, detect language and extract key-phrases",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations", "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "languageCode",
          "targetName": "languageCode"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "textSplitMode" : "pages", 
      "maximumPageLength": 4000,
      "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      },
      { 
        "name": "languageCode",
        "source": "/document/languageCode"
      }
    ],
    "outputs": [
      {
            "name": "textItems",
            "targetName": "pages"
      }
    ]
  },
  {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "context": "/document/pages/*",
      "inputs": [
        {
          "name": "text", "source": "/document/pages/*"
        },
        {
          "name":"languageCode", "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "keyPhrases",
          "targetName": "keyPhrases"
        }
      ]
    }
  ]
}
```

Verzend de aanvraag. Het online hulpprogramma zou een 201-statuscode moeten retourneren om de succesvolle uitvoering te bevestigen. 

#### <a name="about-the-request"></a>Over de aanvraag

U ziet hoe de vaardigheid voor sleuteltermextractie op elke pagina wordt toegepast. Door de context op ```"document/pages/*"``` in te stellen, voert u deze verrijker uit voor elk onderdeel van de matrix document/pagina's (voor elke pagina in het document).

Elke vaardigheid wordt uitgevoerd voor de inhoud van het document. Tijdens het verwerken opent Azure Search elk document om inhoud van verschillende bestandsindelingen te lezen. Gevonden tekst uit het bronbestand wordt in een gegenereerd ```content```-veld geplaatst, één voor elk document. Stel de invoer daarom in als ```"/document/content"```.

Een grafische weergave van de set vaardigheden wordt hieronder weergegeven. 

![Een set vaardigheden begrijpen](media/cognitive-search-tutorial-blob/skillset.png "Een set vaardigheden begrijpen")

Uitvoeren kunnen aan een index worden toegewezen, als invoer worden gebruikt voor een downstream-vaardigheid, of beide zoals bij taalcode. Een taalcode is in de index handig om te filteren. Taalcode wordt als invoer door vaardigheden voor tekstanalyse gebruikt om de taalregels voor woordafbreking in te stellen.

Zie [How to define a skillset](cognitive-search-defining-skillset.md) (Een set vaardigheden definiëren) voor meer informatie over de grondbeginselen van vaardigheden.

## <a name="create-an-index"></a>Een index maken

In dit gedeelte kunt u het indexschema definiëren door op te geven welke velden in de doorzoekbare index moeten worden opgenomen en de zoekkenmerken voor elk veld op te geven. Velden hebben een type en kunnen kenmerken opnemen die bepalen hoe het veld wordt gebruikt (doorzoekbaar, sorteerbaar enzovoort). Veldnamen in een index hoeven niet identiek te zijn aan de veldnamen in de bron. In een latere stap voegt u veldverwijzingen in een indexeerfunctie toe om bron-doelvelden te verbinden. Definieer voor deze stap de index met behulp van veldnaamconventies die relevant zijn voor uw zoektoepassing.

In deze oefening worden de volgende velden en veldtypen gebruikt:

| field-names: | id       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Voorbeeldaanvraag
Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Deze aanvraag maakt een index. Gebruik de naam van de index ```demoindex``` voor de rest van deze zelfstudie.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody

```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
Verzend de aanvraag. Het online hulpprogramma zou een 201-statuscode moeten retourneren om de succesvolle uitvoering te bevestigen. 

Zie [Create Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index maken (Azure Search REST API)) voor meer informatie over het definiëren van een index.


## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Een indexeerfunctie maken, velden toewijzen en transformaties uitvoeren

Tot nu toe hebt u een gegevensbron, een set vaardigheden en een index gemaakt. Deze drie onderdelen gaan deel uitmaken van een [indexeerfunctie](search-indexer-overview.md) die de onderdelen combineert in één meervoudige bewerking. Als u deze in een indexeerfunctie wilt combineren, moet u veldtoewijzingen definiëren. Veldtoewijzingen maken deel uit van de definitie van de indexeerfunctie en voeren de transformaties uit wanneer u de aanvraag verzendt.

Bij niet-verrijkt indexeren biedt de definitie van de indexeerfunctie een optioneel gedeelte *fieldMappings* als de veldnamen of gegevenstypen niet exact overeenkomen, of als u een functie wilt gebruiken.

Voor werkbelastingen voor cognitief zoeken met een verrijkingspijplijn, heeft een indexeerfunctie *outputFieldMappings* nodig. Deze toewijzingen worden gebruikt wanneer een intern proces (de verrijkingspijplijn) de bron van de veldwaarden is. Gedrag dat uniek is voor *outputFieldMappings* omvat de mogelijkheid voor het gebruiken van complexe typen die zijn gemaakt als onderdeel van verrijking (via de vaardigheid shaper). Bovendien kunnen er veel elementen per document zijn (bijvoorbeeld meerdere organisaties in een document). De constructie *outputFieldMappings* kan ervoor zorgen dat het systeem verzamelingen van elementen 'plat maakt' in één record.

### <a name="sample-request"></a>Voorbeeldaanvraag

Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Geef ook de naam van de indexeerfunctie op. U kunt er voor de rest van deze zelfstudie naar verwijzen als ```demoindexer```.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
        {
          "sourceFieldName" : "metadata_storage_path",
          "targetFieldName" : "id",
          "mappingFunction" : 
            { "name" : "base64Encode" }
        },
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
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyPhrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "languageCode"
        }      
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "maxFailedItemsPerBatch":-1,
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
        }
  }
}
```

Verzend de aanvraag. Het online hulpprogramma zou een 201-statuscode moeten retourneren om de succesvolle verwerking te bevestigen. 

Deze stap kan enkele minuten in beslag nemen. De gegevensset is klein, maar analytische vaardigheden zijn berekeningsintensief. Sommige vaardigheden, zoals afbeeldingsanalyse, zijn langlopend.

> [!TIP]
> Het maken van een indexeerfunctie roept de pijplijn aan. Als er problemen zijn met het bereiken van de gegevens, het toewijzen van invoeren en uitvoeren of de volgorde van bewerkingen, worden ze in deze fase weergegeven. Mogelijk moet u eerst objecten verwijderen om de pijplijn opnieuw uit te voeren met code- of scriptwijzigingen. Zie [Reset and re-run](#reset) (Opnieuw instellen en uitvoeren) voor meer informatie.

### <a name="explore-the-request-body"></a>De aanvraagbody verkennen

Het script stelt ```"maxFailedItems"``` op -1 in, wat de indexeerengine de opdracht geeft om fouten tijdens het importeren van gegevens te negeren. Dit is nuttig omdat de demo-gegevensbron maar een paar documenten bevat. Voor een grotere gegevensbron zou u de waarde op groter dan 0 instellen.

Let ook op de instructie ```"dataToExtract":"contentAndMetadata"``` in de configuratieparameters. Deze instructie geeft de indexeerfunctie de opdracht om automatisch de inhoud van verschillende bestandsindelingen te extraheren, evenals metagegevens voor elk bestand. 

Wanneer inhoud wordt uitgepakt, kunt u instellen dat ```ImageAction``` tekst ophaalt uit afbeeldingen die in de gegevensbron worden gevonden. De ```"ImageAction":"generateNormalizedImages"``` geeft de indexeerfunctie de opdracht om tekst uit de afbeeldingen te extraheren (bijvoorbeeld het woord 'stop' van een Stop-verkeersbord) en deze in te sluiten als onderdeel van het inhoudsveld. Dit gedrag is van toepassing op zowel de afbeeldingen die zijn ingesloten in de documenten (zoals een afbeelding in een pdf-bestand) als afbeeldingen die zijn gevonden in de gegevensbron, zoals een JPG-bestand.

In deze preview is ```"generateNormalizedImages"``` de enige geldige waarde voor ```"ImageAction"```.

## <a name="check-indexer-status"></a>De status van de indexeerfunctie controleren

Nadat de indexeerfunctie is gedefinieerd, wordt deze automatisch uitgevoerd wanneer u de aanvraag verzendt. Afhankelijk van welke cognitieve vaardigheden u hebt gedefinieerd, kan het indexeren langer duren dan verwacht. Als u wilt weten of de indexeerfunctie nog wordt uitgevoerd, verzendt u de volgende aanvraag om de status van de indexeerfunctie te controleren.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Het antwoord geeft aan of de indexeerfunctie wordt uitgevoerd. Gebruik nadat het indexeren is voltooid een andere HTTP GET voor het STATUS-eindpunt (zoals hierboven) om rapporten te bekijken van eventuele fouten en waarschuwingen die tijdens de verrijking zijn opgetreden.  

Waarschuwingen komen veel voor bij sommige combinaties van bronbestand en vaardigheid en wijzen niet altijd op een probleem. In deze zelfstudie zijn de waarschuwingen onschadelijk (bijvoorbeeld geen tekstinvoeren van de JPEG-bestanden). U kunt het statusantwoord bekijken voor uitgebreide informatie over waarschuwingen die tijdens het indexeren zijn verzonden.
 
## <a name="verify-content"></a>Inhoud verifiëren

Voer nadat het indexeren is voltooid query's uit waarmee de inhoud van afzonderlijke velden wordt geretourneerd. Standaard retourneert Azure Search de 50 hoogste resultaten. De voorbeeldgegevens zijn beperkt, dus de standaardinstelling werkt prima. Als u echter werkt met grotere gegevenssets, moet u mogelijk parameters opnemen in de queryreeks om meer resultaten te retourneren. Zie [How to page results in Azure Search](search-pagination-page-layout.md) (Resultaten genereren in Azure Search) voor instructies.

Voer als verificatiestap query's uit op de index voor alle velden.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

De uitvoer is het schema van de index met de naam, het type en de kenmerken van elk veld.

Verzend een tweede query voor `"*"` om alle inhoud te retourneren van één veld, zoals `organizations`.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Herhaal dit voor andere velden: inhoud, taal, sleuteltermen en organisaties in deze oefening. U kunt meerdere velden retourneren via `$select` met behulp van een door komma's gescheiden lijst.

U kunt GET of POST gebruiken, afhankelijk van de complexiteit en lengte van de queryreeks. Zie [Query using the REST API](https://docs.microsoft.com/azure/search/search-query-rest-api) (Query's uitvoeren met de REST API) voor meer informatie.

<a name="access-enriched-document"></a>

## <a name="accessing-the-enriched-document"></a>Het verrijkte document openen

Met cognitief zoeken kunt u de structuur van het verrijkte document bekijken. Verrijkte documenten zijn tijdelijke structuren die tijdens het verrijken worden gemaakt en weer worden verwijderd wanneer het proces is voltooid.

Als u een momentopname van het verrijkte document wilt vastleggen tijdens het indexeren, voegt u een veld met de naam ```enriched``` toe aan uw index. De indexeerfunctie dumpt automatisch een tekenreeksrepresentatie van de verrijkingen voor het document in het veld.

Het veld ```enriched``` bevat dan een tekenreeks die een logische representatie vormt van het verrijkte document in het geheugen in JSON.  De veldwaarde is echter een geldig JSON-document. Aanhalingstekens worden geïdentificeerd met een escape-teken, waardoor u `\"` met `"` moet vervangen als u het document als geformatteerde JSON wilt weergeven.  

Het veld ```enriched``` is bedoeld voor foutopsporing, alleen om u inzicht te geven in de logische vorm van de inhoud op basis waarvan expressies worden geëvalueerd. Het kan een handig hulpmiddel zijn om uw set vaardigheden te begrijpen en er fouten in op te sporen.

Herhaal de vorige oefening en voeg een veld `enriched` toe om de inhoud van een verrijkt document vast te leggen:

### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
<a name="reset"></a>

## <a name="reset-and-rerun"></a>Opnieuw instellen en uitvoeren

In de eerste experimentele fasen van pijplijnontwikkeling, is de meest praktische aanpak voor ontwerpiteraties om de objecten uit Azure Search te verwijderen en uw code ze te laten herbouwen. Resourcenamen zijn uniek. Na het verwijderen van een object kunt u het opnieuw maken met dezelfde naam.

Uw documenten opnieuw indexeren met de nieuwe definities:

1. Verwijder de index om persistente gegevens te verwijderen. Verwijder de indexeerfunctie om deze in de service opnieuw te maken.
2. Wijzig de definitie van een set vaardigheden en index.
3. Maak de index en indexeerfunctie opnieuw in de service om de pijplijn uit te voeren. 

U kunt de portal gebruiken om indexen en indexeerfuncties te verwijderen. Sets vaardigheden kunnen alleen worden verwijderd via een HTTP-opdracht.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

De statuscode 204 wordt na verwijdering geretourneerd.

Naarmate uw code meer vormt krijgt, is het raadzaam om uw herbouwstrategie te verfijnen. Zie [How to rebuild an index](search-howto-reindex.md) (Een index herbouwen) voor meer informatie.

## <a name="takeaways"></a>Opgedane kennis

In deze zelfstudie zijn de basisstappen voor het bouwen van een pijplijn voor verrijkte indexering gedemonstreerd via het maken van onderdelen: een gegevensbron, een set vaardigheden, een index en een indexeerfunctie.

[Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) zijn geïntroduceerd, samen met de definitie van de set vaardigheden en het mechanisme voor het koppelen van vaardigheden via in- en uitvoeren. U hebt ook geleerd dat `outputFieldMappings` vereist is in de definitie van de indexeerfunctie voor de routering van verrijkte waarden uit de pijplijn naar een doorzoekbare index op een Azure Search-service.

Tot slot hebt u geleerd hoe u resultaten kunt testen en het systeem opnieuw kunt instellen voor verdere iteraties. U hebt geleerd dat het uitvoeren van query's op de index de uitvoer retourneert die is gemaakt door de pijplijn voor verrijkte indexering. Deze release bevat een mechanisme voor het weergeven van interne constructies (verrijkte documenten die zijn gemaakt door het systeem). U hebt ook geleerd hoe u de status van de indexeerfunctie kunt controleren en welke objecten u moet verwijderen voordat u een pijplijn opnieuw uitvoert.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is om de resourcegroep met de Azure Search-service en Azure Blob service te verwijderen. Ervan uitgaande dat u beide services in dezelfde groep hebt geplaatst, verwijdert u de resourcegroep om alle inhoud ervan permanent te verwijderen, waaronder de services en alle opgeslagen inhoud die u voor deze zelfstudie hebt gemaakt. De naam van de resourcegroep staat in de portal op de pagina Overzicht van elke service.

## <a name="next-steps"></a>Volgende stappen

De pijplijn uitbreiden of aanpassen met aangepaste vaardigheden. Door een aangepaste vaardigheid te maken en deze toe te voegen aan een set vaardigheden, kunt u zelfgeschreven tekst- of afbeeldingsanalyse integreren. 

> [!div class="nextstepaction"]
> [Voorbeeld: een aangepaste vaardigheid maken](cognitive-search-create-custom-skill-example.md)
