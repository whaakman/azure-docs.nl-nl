---
title: 'Python-zelfstudie: Cognitive Services worden aangeroepen in een indexering pijplijn - Azure Search'
description: Stap door een voorbeeld van het ophalen van gegevens, natuurlijke taal en afbeelding AI verwerken in Azure Search met behulp van een Jupyter-Python-notebook. Geëxtraheerde gegevens worden geïndexeerd en eenvoudig toegankelijk voor query.
manager: cgronlun
author: LisaLeib
services: search
ms.service: search
ms.devlang: python
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: v-lilei
ms.openlocfilehash: 7f8ac2b7973ddd21f16f2914b01618b2b005cd13
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485795"
---
# <a name="python-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>Python-zelfstudie: Cognitive Services API's aanroepen in een Azure Search indexeren van pijplijn

In deze zelfstudie leert u de mechanismen achter gegevensverrijking programmeren in Azure Search met behulp van *cognitieve vaardigheden*. Vaardigheden worden ondersteund door natuurlijke taalverwerking (NLP) en de installatiekopie van analysemogelijkheden in Cognitive Services. U kunt via de samenstelling van vaardigheden en configuratie, tekst en de vorm van een afbeelding of gescande documentbestand tekst extraheren. Ook kunt u de taal, entiteiten en sleuteltermen detecteren. Het resultaat is een uitgebreide aanvullende inhoud in een Azure Search-index, die zijn gemaakt met AI enrichments in een pijplijn voor indexering. 

In deze zelfstudie gebruikt u Python om de volgende taken uitvoeren:

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

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), bieden Python 3.x en Jupyter-Notebooks.

+ [Voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) bestaat uit een verzameling kleine bestanden van verschillende typen. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

Om te communiceren met uw Azure Search-service, moet u de service-URL en een toegangssleutel. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Een geldige sleutel stelt trust, op basis van per aanvraag, tussen het verzenden van de aanvraag en de service die verantwoordelijk is voor deze toepassing.

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

De verrijkingspijplijn haalt gegevens uit Azure-gegevensbronnen. Brongegevens moeten afkomstig zijn van een ondersteund type gegevensbron van een [Azure Search-indexeerfunctie](search-indexer-overview.md). Azure Table Storage wordt niet ondersteund voor cognitief zoeken. In dit voorbeeld gebruiken we blobopslag om meerdere inhoudstypen te laten zien.

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) voorbeeldgegevens bevatten. U kunt het niveau van de openbare toegang instellen op een van de geldige waarden.

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk voor het uploaden van de voorbeeldbestanden die u hebt gedownload in de vorige stap.

   ![Bronbestanden in Azure-blobopslag](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Nadat de voorbeeldbestanden zijn geladen, haalt u de containernaam en een verbindingsreeks voor de Blob-opslag op. U kunt dat doen door in de Azure-portal naar uw opslagaccount te navigeren. Klik op **toegangssleutels**, en kopieer vervolgens de **Connection String** veld.

De verbindingsreeks heeft de volgende notatie: `DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT-NAME>;AccountKey=<YOUR-STORAGE-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

De verbindingsreeks bij de hand te houden. U moet deze in een toekomstige stap.

Er zijn andere manieren om de verbindingsreeks op te geven, zoals een Shared Access Signature bieden. Zie [Indexing Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials) (Azure Blob Storage indexeren) voor meer informatie over referenties voor gegevensbronnen.

## <a name="create-a-jupyter-notebook"></a>Een Jupyter-notebook maken

> [!Note]
> In dit artikel laat u over het bouwen van een gegevensbron, index, indexeerfunctie en vaardigheden met behulp van een reeks van Python-scripts. Het volledige notebook-voorbeeld downloaden, gaat u naar de [opslagplaats voor Azure-Search-python-voorbeelden](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment-Jupyter-Notebook).

Gebruik Anaconda Navigator Jupyter-Notebook Start en een nieuwe Python 3-notebook maken.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

Voer in het notitieblok, dit script voor het laden van de bibliotheken die wordt gebruikt voor het werken met JSON en HTTP-aanvragen te formuleren.

```python
import json
import requests
from pprint import pprint
```

Vervolgens definieert u de namen voor de gegevensbron, index, indexeerfunctie en vaardigheden. Voer dit script voor het instellen van de namen voor deze zelfstudie.

```python
#Define the names for the data source, skillset, index and indexer
datasource_name="cogsrch-py-datasource"
skillset_name="cogsrch-py-skillset"
index_name="cogsrch-py-index"
indexer_name="cogsrch-py-indexer"
```

> [!Tip]
> Op een gratis service bent u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen. In deze zelfstudie wordt één exemplaar van elk onderdeel gemaakt. Zorg ervoor dat er voldoende ruimte is het maken van nieuwe objecten voordat u doorgaat een.

In het volgende script, vervang de tijdelijke aanduidingen voor uw search-service (uw-SEARCH-SERVICE-naam) en beheer-API-sleutel (uw-ADMIN-API-sleutel), en voer het voor het instellen van de search service-eindpunt.

```python
#Setup the endpoint
endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
headers = {'Content-Type': 'application/json',
        'api-key': '<YOUR-ADMIN-API-KEY>' }
params = {
    'api-version': '2019-05-06'
}
```

## <a name="create-a-data-source"></a>Een gegevensbron maken

Nu dat uw services en bronbestanden zijn voorbereid, kunt u de onderdelen van uw indexeringspijplijn samenstellen. Beginnen met het object voor een gegevensbron die Azure Search hoe u externe gegevens ophalen.

In het volgende script, kunt u de tijdelijke aanduiding voor uw-BLOB-RESOURCE-CONNECTION-STRING vervangen door de verbindingsreeks voor de blob die u in de vorige stap hebt gemaakt. Voer het script voor het maken van een gegevensbron met de naam `cogsrch-py-datasource`.

```python
#Create a data source
datasourceConnectionString = "<YOUR-BLOB-RESOURCE-CONNECTION-STRING>"
datasource_payload = {
    "name": datasource_name,
    "description": "Demo files to demonstrate cognitive search capabilities.",
    "type": "azureblob",
    "credentials": {
    "connectionString": datasourceConnectionString
   },
    "container": {
     "name": "basic-demo-data-pr"
   }
}
r = requests.put( endpoint + "/datasources/" + datasource_name, data=json.dumps(datasource_payload), headers=headers, params=params )
print (r.status_code)
```

De aanvraag moet de statuscode 201 bevestiging retourneren.

In de Azure-portal op de dashboardpagina van search-service controleert u of de gegevensbron voor het kopiëren van een cogsrch verschijnt de **gegevensbronnen** lijst. Klik op **vernieuwen** op de pagina bijwerken.

![Tegel Gegevensbronnen in de portal](./media/cognitive-search-tutorial-blob-python/py-data-source-tile.png "Tegel Gegevensbronnen in de portal")

## <a name="create-a-skillset"></a>Een set vaardigheden maken

In deze stap definieert u een reeks stappen verrijking toe te passen op uw gegevens. We noemen elke verrijkingsstap een *vaardigheid* en de reeks verrijkingsstappen een *set vaardigheden*. Deze zelfstudie wordt gebruikgemaakt van [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) voor de vaardigheden:

+ [Taaldetectie](cognitive-search-skill-language-detection.md) om de taal van de inhoud vast te stellen.

+ [Tekst splitsen](cognitive-search-skill-textsplit.md) om grote inhoud in kleinere stukken op te delen voordat u de vaardigheid voor sleuteltermextractie aanroept. Sleuteltermextractie accepteert invoeren van 50.000 tekens of minder. Enkele voorbeeldbestanden moeten worden opgesplitst om aan deze limiet te voldoen.

+ [Herkenning van entiteit](cognitive-search-skill-entity-recognition.md) voor het uitpakken van de namen van organisaties van de inhoud in de blob-container.

+ [Sleuteltermextractie](cognitive-search-skill-keyphrases.md) voor het ophalen van de belangrijkste sleuteltermen. 

### <a name="python-script"></a>Python-script
Voer het volgende script voor het maken van een set vaardigheden met de naam `cogsrch-py-skillset`.

```python
#Create a skillset
skillset_payload = {
  "name": skillset_name,
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

r = requests.put(endpoint + "/skillsets/" + skillset_name, data=json.dumps(skillset_payload), headers=headers, params=params)
print(r.status_code)
```

De aanvraag moet de statuscode 201 bevestiging retourneren.

De kwalificatie van de extractie sleuteluitdrukkingen wordt toegepast voor elke pagina. Door de context in te stellen `"document/pages/*"`, u deze enricher uitvoeren voor elk lid van de matrix document/pagina's (voor elke pagina in het document).

Elke vaardigheid wordt uitgevoerd voor de inhoud van het document. Tijdens het verwerken opent Azure Search elk document om inhoud van verschillende bestandsindelingen te lezen. Tekst gevonden in het bronbestand wordt geplaatst in een `content` veld, één voor elk document. Stel daarom de invoer als `"/document/content"`.

Een grafische weergave van de set vaardigheden wordt hieronder weergegeven.

![Een set vaardigheden begrijpen](media/cognitive-search-tutorial-blob/skillset.png "Een set vaardigheden begrijpen")

Uitvoer kunnen worden toegewezen aan een index, die wordt gebruikt als invoer voor een downstream vaardigheid, of beide, zoals het geval is bij de taalcode die. Een taalcode is in de index handig om te filteren. Taalcode wordt als invoer door vaardigheden voor tekstanalyse gebruikt om de taalregels voor woordafbreking in te stellen.

Zie [How to define a skillset](cognitive-search-defining-skillset.md) (Een set vaardigheden definiëren) voor meer informatie over de grondbeginselen van vaardigheden.

## <a name="create-an-index"></a>Een index maken

In deze sectie definieert u het indexschema dat door de velden die u wilt opnemen in de doorzoekbare index op te geven en het instellen van de search-kenmerken voor elk veld. Velden hebben een type en kunnen kenmerken opnemen die bepalen hoe het veld wordt gebruikt (doorzoekbaar, sorteerbaar enzovoort). Veldnamen in een index hoeven niet identiek te zijn aan de veldnamen in de bron. In een latere stap voegt u veldverwijzingen in een indexeerfunctie toe om bron-doelvelden te verbinden. Definieer voor deze stap de index met behulp van veldnaamconventies die relevant zijn voor uw zoektoepassing.

In deze oefening worden de volgende velden en veldtypen gebruikt:

| field-names: | id         | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |

Voer dit script voor het maken van de index met de naam `cogsrch-py-index`.

```python
#Create an index
index_payload = {
    "name": index_name,
    "fields": [
      {
        "name": "id",
        "type": "Edm.String",
        "key": "true",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false",
        "sortable": "true"
      },
      {
        "name": "content",
        "type": "Edm.String",
        "sortable": "false",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "languageCode",
        "type": "Edm.String",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "keyPhrases",
        "type": "Collection(Edm.String)",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "organizations",
        "type": "Collection(Edm.String)",
        "searchable": "true",
        "sortable": "false",
        "filterable": "false",
        "facetable": "false"
      }
   ]
}

r = requests.put(endpoint + "/indexes/" + index_name, data=json.dumps(index_payload), headers=headers, params=params)
print(r.status_code)
```

De aanvraag moet de statuscode 201 bevestiging retourneren.

Zie [Create Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index maken (Azure Search REST API)) voor meer informatie over het definiëren van een index.

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Een indexeerfunctie maken, velden toewijzen en transformaties uitvoeren

Tot nu toe hebt u een gegevensbron, een set vaardigheden en een index gemaakt. Deze drie onderdelen gaan deel uitmaken van een [indexeerfunctie](search-indexer-overview.md) die de onderdelen combineert in één meervoudige bewerking. Als u wilt deze objecten met elkaar verbinden in een indexeerfunctie, moet u veldtoewijzingen definiëren.

+ De fieldMappings worden verwerkt voordat de vaardigheden, toewijzing van de bronvelden uit de gegevensbron naar de doelvelden in een index. Als de veldnamen en typen identiek aan beide uiteinden zijn, zijn er is geen toewijzing is vereist.

+ De outputFieldMappings worden verwerkt na de vaardigheden, verwijst naar een sourceFieldNames die niet tot documenten kraken of verrijking gemaakt. De targetFieldName is een veld in een index.

Naast het installeren van de invoer voor uitvoer, kunt u ook veldtoewijzingen gebruiken om af te vlakken gegevensstructuren. Zie voor meer informatie, [verrijkt velden toewijzen aan een doorzoekbare index](cognitive-search-output-field-mapping.md).

Voer dit script voor het maken van een indexeerfunctie met de naam `cogsrch-py-indexer`.

```python
# Create an indexer
indexer_payload = {
    "name": indexer_name,
    "dataSourceName": datasource_name,
    "targetIndexName": index_name,
    "skillsetName": skillset_name,
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

r = requests.put(endpoint + "/indexers/" + indexer_name, data=json.dumps(indexer_payload), headers=headers, params=params)
print(r.status_code)
```

De aanvraag moet de statuscode 201 snel retourneren, maar de verwerking van enkele minuten kan duren. Hoewel de set gegevens klein is, wordt analytische vaardigheden, zoals het analyseren van afbeeldingen, rekenintensieve en tijd in beslag nemen.

Gebruik de [Controleer de status van de indexeerfunctie](#check-indexer-status) script in de volgende sectie om te bepalen wanneer de indexeerfunctie is voltooid.

> [!TIP]
> Het maken van een indexeerfunctie roept de pijplijn aan. Als er een probleem opgetreden bij het openen van de gegevens, toewijzing van invoer en uitvoer, of met de volgorde van bewerkingen, wordt deze in deze fase weergegeven. Opnieuw uit te voeren de pijplijn met de code of scripts wijzigingen, moet u mogelijk eerst objecten verwijderen. Zie [Reset and re-run](#reset) (Opnieuw instellen en uitvoeren) voor meer informatie.

#### <a name="explore-the-request-body"></a>De aanvraagbody verkennen

Het script stelt `"maxFailedItems"` op -1 in, wat de indexeerengine de opdracht geeft om fouten tijdens het importeren van gegevens te negeren. Dit is nuttig omdat de demo-gegevensbron maar een paar documenten bevat. Voor een grotere gegevensbron zou u de waarde op groter dan 0 instellen.

Let ook op de instructie `"dataToExtract":"contentAndMetadata"` in de configuratieparameters. Deze instructie vertelt de indexeerfunctie om op te halen van de inhoud van verschillende bestandsindelingen en de metagegevens die betrekking hebben op elk bestand.

Wanneer inhoud wordt uitgepakt, kunt u instellen dat `imageAction` tekst ophaalt uit afbeeldingen die in de gegevensbron worden gevonden. De configuratie van `"imageAction":"generateNormalizedImages"`, OCR Skill en Text Merge Skill geeft de indexeerfunctie de opdracht om tekst uit de afbeeldingen te extraheren (bijvoorbeeld het woord 'stop' van een Stop-verkeersbord) en deze in te sluiten als onderdeel van het inhoudsveld. Dit gedrag is van toepassing op de afbeeldingen die zijn ingesloten in de documenten (denk aan van een afbeelding in een PDF-bestand) en de afbeeldingen vinden in de gegevensbron, bijvoorbeeld een JPG-bestand.

<a name="check-indexer-status"></a>

## <a name="check-indexer-status"></a>De status van de indexeerfunctie controleren

Nadat de indexeerfunctie is gedefinieerd, wordt deze automatisch uitgevoerd wanneer u de aanvraag verzendt. Afhankelijk van welke cognitieve vaardigheden u hebt gedefinieerd, kan het indexeren langer duren dan verwacht. Als u wilt weten of de verwerking van de indexeerfunctie voltooid is, moet u het volgende script uitvoeren.

```python
#Get indexer status
r = requests.get(endpoint + "/indexers/" + indexer_name + "/status", headers=headers,params=params)
pprint(json.dumps(r.json(), indent=1))
```

Monitor de waarden "status" en "endTime" het "lastResult' in het antwoord. Het script om te controleren of de status uitvoeren periodiek. Wanneer de indexeerfunctie is voltooid, wordt de status worden ingesteld op "geslaagd", "endTime" worden opgegeven en de reactie omvat eventuele fouten en waarschuwingen die zijn opgetreden tijdens verrijking.

![Indexeerfunctie wordt gemaakt](./media/cognitive-search-tutorial-blob-python/py-indexer-is-created.png "indexeerfunctie wordt gemaakt")

Waarschuwingen komen veel voor bij sommige combinaties van bronbestand en vaardigheid en wijzen niet altijd op een probleem. In deze zelfstudie worden de waarschuwingen goedaardige. Een van de JPEG-bestanden die geen tekst wordt bijvoorbeeld de waarschuwing weergegeven in deze schermafbeelding.

![Voorbeeld van de indexeerfunctie waarschuwing](./media/cognitive-search-tutorial-blob-python/py-indexer-warning-example.png "voorbeeld indexeerfunctie waarschuwing")

## <a name="query-your-index"></a>Een query uitvoeren in uw index

Voer nadat het indexeren is voltooid query's uit waarmee de inhoud van afzonderlijke velden wordt geretourneerd. Standaard retourneert Azure Search de 50 hoogste resultaten. De voorbeeldgegevens zijn beperkt, dus de standaardinstelling werkt prima. Als u echter werkt met grotere gegevenssets, moet u mogelijk parameters opnemen in de queryreeks om meer resultaten te retourneren. Zie [How to page results in Azure Search](search-pagination-page-layout.md) (Resultaten genereren in Azure Search) voor instructies.

Voer als verificatiestap query's uit op de index voor alle velden.

```python
#Query the index for all fields
r = requests.get(endpoint + "/indexes/" + index_name, headers=headers,params=params)
pprint(json.dumps(r.json(), indent=1))
```

De resultaten moeten eruitzien zoals in het volgende voorbeeld. De schermafbeelding ziet u alleen een deel van het antwoord.

![Queryindex voor alle velden](./media/cognitive-search-tutorial-blob-python/py-query-index-for-fields.png "query uitvoeren in de index voor alle velden")

De uitvoer is het schema van de index met de naam, het type en de kenmerken van elk veld.

Verzend een tweede query voor `"*"` om alle inhoud te retourneren van één veld, zoals `organizations`.

```python
#Query the index to return the contents of organizations
r = requests.get(endpoint + "/indexes/" + index_name + "/docs?&search=*&$select=organizations", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

De resultaten moeten eruitzien zoals in het volgende voorbeeld. De schermafbeelding ziet u alleen een deel van het antwoord.

![Queryindex voor de inhoud van organisaties](./media/cognitive-search-tutorial-blob-python/py-query-index-for-organizations.png "query uitvoeren in de index om te retourneren van de inhoud van organisaties")

Herhaal dit voor aanvullende velden: inhoud, languageCode Sleutelzinnen en organisaties in deze oefening. U kunt meerdere velden retourneren via `$select` met behulp van een door komma's gescheiden lijst.

U kunt GET of POST gebruiken, afhankelijk van de complexiteit en lengte van de queryreeks. Zie [Query using the REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Query's uitvoeren met de REST API) voor meer informatie.
Deze <a name="reset"></a>

## <a name="reset-and-rerun"></a>Opnieuw instellen en uitvoeren

In de eerste experimentele fasen van pijplijnontwikkeling, is de meest praktische aanpak voor ontwerpiteraties om de objecten uit Azure Search te verwijderen en uw code ze te laten herbouwen. Resourcenamen zijn uniek. Na het verwijderen van een object kunt u het opnieuw maken met dezelfde naam.

Uw documenten opnieuw indexeren met de nieuwe definities:

1. Verwijder de index om persistente gegevens te verwijderen. Verwijder de indexeerfunctie om deze in de service opnieuw te maken.
2. De definities van vaardigheden en index wijzigen.
3. Maak de index en indexeerfunctie opnieuw in de service om de pijplijn uit te voeren.

U kunt de portal verwijderen indexen, Indexeerfuncties en kennis en vaardigheden gebruiken. Wanneer u de indexeerfunctie verwijdert, kunt u eventueel, selectief verwijderen de index, vaardigheden en de gegevensbron op hetzelfde moment.

![Verwijderen van objecten zoeken](./media/cognitive-search-tutorial-blob-python/py-delete-indexer-delete-all.png "verwijderen zoeken objecten in de portal")

U kunt ook verwijderen met behulp van een script. Het volgende script wordt verwijderd van de vaardigheden die we hebben gemaakt. U kunt eenvoudig de aanvraag voor het verwijderen van de index, indexeerfunctie en gegevensbron wijzigen.

```python
#delete the skillset
r = requests.delete(endpoint + "/skillsets/" + skillset_name, headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Naarmate uw code meer vormt krijgt, is het raadzaam om uw herbouwstrategie te verfijnen. Zie [How to rebuild an index](search-howto-reindex.md) (Een index herbouwen) voor meer informatie.

## <a name="takeaways"></a>Opgedane kennis

In deze zelfstudie zijn de basisstappen voor het bouwen van een pijplijn voor verrijkte indexering gedemonstreerd via het maken van onderdelen: een gegevensbron, een set vaardigheden, een index en een indexeerfunctie.

[Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) zijn geïntroduceerd, samen met de definities van vaardigheden en een manier om keten vaardigheden samen met de invoer en uitvoer. U hebt ook geleerd dat `outputFieldMappings` vereist is in de definitie van de indexeerfunctie voor de routering van verrijkte waarden uit de pijplijn naar een doorzoekbare index op een Azure Search-service.

Ten slotte hebt u geleerd hoe u de testresultaten en opnieuw instellen van het systeem voor verdere iteraties. U hebt geleerd dat het uitvoeren van query's op de index de uitvoer retourneert die is gemaakt door de pijplijn voor verrijkte indexering. Deze release bevat een mechanisme voor het weergeven van interne constructies (verrijkte documenten die zijn gemaakt door het systeem). U hebt ook geleerd hoe u om de status van de indexeerfunctie te controleren en welke objecten moet worden verwijderd voordat u een pijplijn.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is om de resourcegroep met de Azure Search-service en Azure Blob service te verwijderen. Ervan uitgaande dat u beide services in dezelfde groep plaatsen, verwijder de resourcegroep permanent verwijdert u alle informatie in, met inbegrip van de services en opgeslagen inhoud die u voor deze zelfstudie hebt gemaakt. De naam van de resourcegroep staat in de portal op de pagina Overzicht van elke service.

## <a name="next-steps"></a>Volgende stappen

De pijplijn uitbreiden of aanpassen met aangepaste vaardigheden. Door een aangepaste vaardigheid te maken en deze toe te voegen aan een set vaardigheden, kunt u zelfgeschreven tekst- of afbeeldingsanalyse integreren.

> [!div class="nextstepaction"]
> [Voorbeeld: een aangepaste vaardigheid maken](cognitive-search-create-custom-skill-example.md)
