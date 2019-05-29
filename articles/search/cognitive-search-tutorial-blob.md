---
title: "Zelfstudie: Cognitive Services REST-API's aanroepen in een indexering pijplijn - Azure Search"
description: Stap door een voorbeeld van het ophalen van gegevens, natuurlijke taal en afbeelding AI verwerken in Azure Search indexeren voor ophalen van gegevens en -transformaties via JSON-blobs met Postman en de REST-API.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: fb45d2e36939a53d6242cf7cd5a0b9f1990780c3
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299040"
---
# <a name="rest-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>REST-zelfstudie: Cognitive Services API's aanroepen in een Azure Search indexeren van pijplijn

In deze zelfstudie leert u de mechanismen achter gegevensverrijking programmeren in Azure Search met behulp van *cognitieve vaardigheden*. Vaardigheden worden ondersteund door natuurlijke taalverwerking (NLP) en de installatiekopie van analysemogelijkheden in Cognitive Services. U kunt via de samenstelling van vaardigheden en configuratie, tekst en de vorm van een afbeelding of gescande documentbestand tekst extraheren. Ook kunt u de taal, entiteiten en sleuteltermen detecteren. Het eindresultaat is uitgebreide aanvullende inhoud in een Azure Search-index, die zijn gemaakt met AI enrichments in een pijplijn voor indexering. 

In deze zelfstudie maakt u REST API-aanroepen om de volgende taken uit te voeren:

> [!div class="checklist"]
> * Een indexeringspijplijn maken die voorbeeldgegevens onderweg naar een index verrijkt
> * Pas ingebouwde vaardigheden toe: herkenning van entiteiten, taaldetectie, tekstbewerkingen en extractie van sleuteltermen
> * Vaardigheden aan elkaar koppelen door invoeren aan uitvoeren toe te wijzen in een set vaardigheden
> * Aanvragen uitvoeren en resultaten bekijken
> * De index en indexeerfuncties opnieuw instellen voor verdere ontwikkeling

Uitvoer is een volledige doorzoekbare index in Azure Search. U kunt de index uitbreiden met andere standaardfuncties, zoals [synoniemen](search-synonyms.md), [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [analyses](search-analyzers.md) en [filters](search-filters.md).

In deze zelfstudie wordt uitgevoerd op de gratis service, maar het aantal gratis transacties is beperkt tot 20 documenten per dag. Als u wilt meer dan eenmaal in deze zelfstudie wordt uitgevoerd in dezelfde dag, gebruikt u een kleiner bestand instellen, zodat u in meer uitvoeringen past.

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogramma's en gegevens worden gebruikt in deze zelfstudie. 

+ [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van de voorbeeldgegevens. Zorg ervoor dat het opslagaccount zich in dezelfde regio als de Azure Search.

+ [Postman bureaublad-app](https://www.getpostman.com/) wordt gebruikt voor het maken van de REST-aanroepen naar Azure Search.

+ [Voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) bestaat uit een verzameling kleine bestanden van verschillende typen. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

De verrijkingspijplijn haalt gegevens uit Azure-gegevensbronnen. Brongegevens moeten afkomstig zijn van een ondersteund type gegevensbron van een [Azure Search-indexeerfunctie](search-indexer-overview.md). Azure Table Storage wordt niet ondersteund voor cognitief zoeken. In dit voorbeeld gebruiken we blobopslag om meerdere inhoudstypen te laten zien.

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) voorbeeldgegevens bevatten. U kunt het niveau van de openbare toegang instellen op een van de geldige waarden.

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk voor het uploaden van de voorbeeldbestanden die u hebt gedownload in de vorige stap.

   ![Bronbestanden in Azure-blobopslag](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Nadat de voorbeeldbestanden zijn geladen, haalt u de containernaam en een verbindingsreeks voor de Blob-opslag op. U kunt dat doen door in de Azure-portal naar uw opslagaccount te navigeren. Ga naar **Toegangssleutels** en kopieer het veld **Verbindingsreeks**.

   De verbindingsreeks moet een URL zijn die er ongeveer als volgt uitziet:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Er zijn andere manieren om de verbindingsreeks op te geven, zoals een Shared Access Signature bieden. Zie [Indexing Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials) (Azure Blob Storage indexeren) voor meer informatie over referenties voor gegevensbronnen.

## <a name="set-up-postman"></a>Postman instellen

Start Postman en stel een HTTP-aanvraag in. Als u niet bekend met dit hulpprogramma bent, raadpleegt u [verkennen Azure Search REST API's met Postman](search-fiddler.md).

De aanvraagmethoden die worden gebruikt in deze zelfstudie zijn **POST**, **plaatsen**, en **ophalen**. De headersleutels zijn 'Content-type' ingesteld op ' application/json' en 'api-sleutel' ingesteld op geen Administrator-code van uw Azure Search-service. In de hoofdtekst plaatst u de werkelijke inhoud van uw aanroep. 

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/postmanoverview.png)

We gebruiken Postman om vier API-aanroepen naar uw search-service om te kunnen maken van een gegevensbron, een set vaardigheden, een index en een indexeerfunctie maken. De gegevensbron bevat een verwijzing naar uw opslagaccount en uw JSON-gegevens. Uw zoekservice maakt de verbinding tijdens het laden van de gegevens.


## <a name="create-a-data-source"></a>Een gegevensbron maken

Nu dat uw services en bronbestanden zijn voorbereid, kunt u de onderdelen van uw indexeringspijplijn samenstellen. Begin met een [gegevensbronobject](https://docs.microsoft.com/rest/api/searchservice/create-data-source) dat Azure Search vertelt hoe het externe gegevens moet ophalen.

Geef in de aanvraagheader de naam van de service op die u hebt gebruikt bij het maken van de Azure Search-service en de API-sleutel die is gegenereerd voor uw zoekservice. Geef in de aanvraagbody de naam van de blobcontainer en de verbindingsreeks op.

### <a name="sample-request"></a>Voorbeeldaanvraag
```http
POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
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

Aangezien dit uw eerste aanvraag is, controleert u Azure Portal om te bevestigen dat de gegevensbron in Azure Search is gemaakt. Controleer of dat de lijst met gegevensbronnen heeft een nieuw item op de dashboardpagina van search-service. U moet mogelijk een paar minuten wachten tot de portalpagina is vernieuwd. 

  ![Tegel Gegevensbronnen in de portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Tegel Gegevensbronnen in de portal")

Als u een 403- of 404-fout krijgt, controleert u de constructie van de aanvraag: `api-version=2019-05-06` moet in het eindpunt staan, `api-key` in de header na `Content-Type`, en de waarde ervan moet geldig voor een zoekservice zijn. U kunt de header hergebruiken voor de overige stappen in deze zelfstudie.

## <a name="create-a-skillset"></a>Een set vaardigheden maken

In deze stap definieert u een reeks verrijkingsstappen die u op uw gegevens wilt toepassen. We noemen elke verrijkingsstap een *vaardigheid* en de reeks verrijkingsstappen een *set vaardigheden*. Deze zelfstudie wordt gebruikgemaakt van [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) voor de vaardigheden:

+ [Taaldetectie](cognitive-search-skill-language-detection.md) om de taal van de inhoud vast te stellen.

+ [Tekst splitsen](cognitive-search-skill-textsplit.md) om grote inhoud in kleinere stukken op te delen voordat u de vaardigheid voor sleuteltermextractie aanroept. Sleuteltermextractie accepteert invoeren van 50.000 tekens of minder. Enkele voorbeeldbestanden moeten worden opgesplitst om aan deze limiet te voldoen.

+ [Herkenning van entiteit](cognitive-search-skill-entity-recognition.md) voor het uitpakken van de namen van organisaties van de inhoud in de blob-container.

+ [Sleuteltermextractie](cognitive-search-skill-keyphrases.md) voor het ophalen van de belangrijkste sleuteltermen. 

### <a name="sample-request"></a>Voorbeeldaanvraag
Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Deze aanvraag maakt een set vaardigheden. Gebruik de naam van de set vaardigheden ```demoskillset``` voor de rest van deze zelfstudie.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
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
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

#### <a name="explore-the-request-body"></a>De aanvraagbody verkennen

U ziet hoe de vaardigheid voor sleuteltermextractie op elke pagina wordt toegepast. Door de context op ```"document/pages/*"``` in te stellen, voert u deze verrijker uit voor elk onderdeel van de matrix document/pagina's (voor elke pagina in het document).

Elke vaardigheid wordt uitgevoerd voor de inhoud van het document. Tijdens het verwerken opent Azure Search elk document om inhoud van verschillende bestandsindelingen te lezen. Gevonden tekst uit het bronbestand wordt in een gegenereerd ```content```-veld geplaatst, één voor elk document. Stel de invoer daarom in als ```"/document/content"```.

Een grafische weergave van de set vaardigheden wordt hieronder weergegeven. 

![Een set vaardigheden begrijpen](media/cognitive-search-tutorial-blob/skillset.png "Een set vaardigheden begrijpen")

Uitvoeren kunnen aan een index worden toegewezen, als invoer worden gebruikt voor een downstream-vaardigheid, of beide zoals bij taalcode. Een taalcode is in de index handig om te filteren. Taalcode wordt als invoer door vaardigheden voor tekstanalyse gebruikt om de taalregels voor woordafbreking in te stellen.

Zie [How to define a skillset](cognitive-search-defining-skillset.md) (Een set vaardigheden definiëren) voor meer informatie over de grondbeginselen van vaardigheden.

## <a name="create-an-index"></a>Een index maken

In dit gedeelte kunt u het indexschema definiëren door op te geven welke velden in de doorzoekbare index moeten worden opgenomen en de zoekkenmerken voor elk veld op te geven. Velden hebben een type en kunnen kenmerken opnemen die bepalen hoe het veld wordt gebruikt (doorzoekbaar, sorteerbaar enzovoort). Veldnamen in een index hoeven niet identiek te zijn aan de veldnamen in de bron. In een latere stap voegt u veldverwijzingen in een indexeerfunctie toe om bron-doelvelden te verbinden. Definieer voor deze stap de index met behulp van veldnaamconventies die relevant zijn voor uw zoektoepassing.

In deze oefening worden de volgende velden en veldtypen gebruikt:

| field-names: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Voorbeeldaanvraag
Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Deze aanvraag maakt een index. Gebruik de naam van de index ```demoindex``` voor de rest van deze zelfstudie.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
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

Tot nu toe hebt u een gegevensbron, een set vaardigheden en een index gemaakt. Deze drie onderdelen gaan deel uitmaken van een [indexeerfunctie](search-indexer-overview.md) die de onderdelen combineert in één meervoudige bewerking. Als u deze in een indexeerfunctie wilt combineren, moet u veldtoewijzingen definiëren. 

+ De fieldMappings worden verwerkt voordat de vaardigheden, toewijzing van de bronvelden uit de gegevensbron naar de doelvelden in een index. Als de veldnamen en typen identiek aan beide uiteinden zijn, zijn er is geen toewijzing is vereist.

+ De outputFieldMappings worden verwerkt na de vaardigheden, verwijst naar een sourceFieldNames die niet tot documenten kraken of verrijking gemaakt. De targetFieldName is een veld in een index.

Naast het installeren van de invoer voor uitvoer, kunt u ook veldtoewijzingen gebruiken om af te vlakken gegevensstructuren. Zie voor meer informatie, [verrijkt velden toewijzen aan een doorzoekbare index](cognitive-search-output-field-mapping.md).

### <a name="sample-request"></a>Voorbeeldaanvraag

Voordat u de REST-aanroep uitvoert, moet u de servicenaam en de beheersleutel in de aanvraag hieronder vervangen als het hulpprogramma de aanvraagheader tussen aanroepen niet behoudt. 

Geef ook de naam van de indexeerfunctie op. U kunt er voor de rest van deze zelfstudie naar verwijzen als ```demoindexer```.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2019-05-06
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

#### <a name="explore-the-request-body"></a>De aanvraagbody verkennen

Het script stelt ```"maxFailedItems"``` op -1 in, wat de indexeerengine de opdracht geeft om fouten tijdens het importeren van gegevens te negeren. Dit is nuttig omdat de demo-gegevensbron maar een paar documenten bevat. Voor een grotere gegevensbron zou u de waarde op groter dan 0 instellen.

Let ook op de instructie ```"dataToExtract":"contentAndMetadata"``` in de configuratieparameters. Deze instructie geeft de indexeerfunctie de opdracht om automatisch de inhoud van verschillende bestandsindelingen te extraheren, evenals metagegevens voor elk bestand. 

Wanneer inhoud wordt uitgepakt, kunt u instellen dat ```imageAction``` tekst ophaalt uit afbeeldingen die in de gegevensbron worden gevonden. De configuratie van ```"imageAction":"generateNormalizedImages"```, OCR Skill en Text Merge Skill geeft de indexeerfunctie de opdracht om tekst uit de afbeeldingen te extraheren (bijvoorbeeld het woord 'stop' van een Stop-verkeersbord) en deze in te sluiten als onderdeel van het inhoudsveld. Dit gedrag is van toepassing op zowel de afbeeldingen die zijn ingesloten in de documenten (zoals een afbeelding in een pdf-bestand) als afbeeldingen die zijn gevonden in de gegevensbron, zoals een JPG-bestand.

## <a name="check-indexer-status"></a>De status van de indexeerfunctie controleren

Nadat de indexeerfunctie is gedefinieerd, wordt deze automatisch uitgevoerd wanneer u de aanvraag verzendt. Afhankelijk van welke cognitieve vaardigheden u hebt gedefinieerd, kan het indexeren langer duren dan verwacht. Als u wilt weten of de indexeerfunctie nog wordt uitgevoerd, verzendt u de volgende aanvraag om de status van de indexeerfunctie te controleren.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Het antwoord geeft aan of de indexeerfunctie wordt uitgevoerd. Gebruik nadat het indexeren is voltooid een andere HTTP GET voor het STATUS-eindpunt (zoals hierboven) om rapporten te bekijken van eventuele fouten en waarschuwingen die tijdens de verrijking zijn opgetreden.  

Waarschuwingen komen veel voor bij sommige combinaties van bronbestand en vaardigheid en wijzen niet altijd op een probleem. In deze zelfstudie zijn de waarschuwingen onschadelijk (bijvoorbeeld geen tekstinvoeren van de JPEG-bestanden). U kunt het statusantwoord bekijken voor uitgebreide informatie over waarschuwingen die tijdens het indexeren zijn verzonden.
 
## <a name="query-your-index"></a>Een query uitvoeren in uw index

Voer nadat het indexeren is voltooid query's uit waarmee de inhoud van afzonderlijke velden wordt geretourneerd. Standaard retourneert Azure Search de 50 hoogste resultaten. De voorbeeldgegevens zijn beperkt, dus de standaardinstelling werkt prima. Als u echter werkt met grotere gegevenssets, moet u mogelijk parameters opnemen in de queryreeks om meer resultaten te retourneren. Zie [How to page results in Azure Search](search-pagination-page-layout.md) (Resultaten genereren in Azure Search) voor instructies.

Voer als verificatiestap query's uit op de index voor alle velden.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

De uitvoer is het schema van de index met de naam, het type en de kenmerken van elk veld.

Verzend een tweede query voor `"*"` om alle inhoud te retourneren van één veld, zoals `organizations`.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Herhaal dit voor aanvullende velden: inhoud, languageCode Sleutelzinnen en organisaties in deze oefening. U kunt meerdere velden retourneren via `$select` met behulp van een door komma's gescheiden lijst.

U kunt GET of POST gebruiken, afhankelijk van de complexiteit en lengte van de queryreeks. Zie [Query using the REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Query's uitvoeren met de REST API) voor meer informatie.



<a name="reset"></a>

## <a name="reset-and-rerun"></a>Opnieuw instellen en uitvoeren

In de eerste experimentele fasen van pijplijnontwikkeling, is de meest praktische aanpak voor ontwerpiteraties om de objecten uit Azure Search te verwijderen en uw code ze te laten herbouwen. Resourcenamen zijn uniek. Na het verwijderen van een object kunt u het opnieuw maken met dezelfde naam.

Uw documenten opnieuw indexeren met de nieuwe definities:

1. Verwijder de index om persistente gegevens te verwijderen. Verwijder de indexeerfunctie om deze in de service opnieuw te maken.
2. Wijzig de definitie van een set vaardigheden en index.
3. Maak de index en indexeerfunctie opnieuw in de service om de pijplijn uit te voeren. 

U kunt de portal gebruiken om indexen, indexeerfuncties en vaardighedensets te verwijderen.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
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
