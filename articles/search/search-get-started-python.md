---
title: "Quickstart: Python en REST-API's - Azure Search"
description: Maken, laden en query uitvoeren in een index met behulp van Python, Jupyter-Notebooks en de Azure Search REST-API.
ms.date: 05/15/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 1ab6bb069f60f4d2dbb4cfaecda54c3c2ef20adc
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806429"
---
# <a name="quickstart-create-an-azure-search-index-using-jupyter-python-notebooks"></a>Quickstart: Maken van een Azure Search-index met behulp van Python voor Jupyter notebooks
> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

Bouw een Jupyter-notebook die wordt gemaakt, wordt geladen en query's van een Azure Search [index](search-what-is-an-index.md) met behulp van Python en de [Azure Search Service REST API's](https://docs.microsoft.com/rest/api/searchservice/). In dit artikel wordt uitgelegd hoe u uw eigen notebook stap voor stap bouwen. U kunt eventueel een voltooide notebook uitvoeren. Als u wilt een kopie downloaden, gaat u naar [opslagplaats voor Azure-Search-python-voorbeelden](https://github.com/Azure-Samples/azure-search-python-samples).

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint en meldt u zich daarna aan voor [Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), bieden Python 3.x en Jupyter-Notebooks.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

Een Jupyter-notebook openen en controleer of de verbinding vanaf uw lokale werkstation door aan te vragen van een lijst van indexen voor uw service. Op Windows met Anaconda3, kunt u Anaconda Navigator een notebook te starten.

1. Maak een nieuwe Python3-notebook.

1. In de eerste cel laden van de bibliotheken die wordt gebruikt voor het werken met JSON en HTTP-aanvragen te formuleren.

   ```python
   import json
   import requests
   from pprint import pprint
   ```

1. Voer in de tweede cel in de aanvraag-elementen die constanten bij elke aanvraag worden. Vervang de naam van de zoekopdracht-service (uw-SEARCH-SERVICE-naam) en beheer-API-sleutel (uw-ADMIN-API-sleutel) met geldige waarden. 

   ```python
    endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
    api_version = '?api-version=2019-05-06'
    headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>' }
   ```

1. In de derde cel formuleren van de aanvraag. Deze GET-aanvraag is gericht op de verzameling indexen van uw search-service en selecteert u de eigenschap name.

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. Elke stap uitvoeren. Als indexen aanwezig is, betekent dit dat het antwoord een lijst van indexen bevat. In de onderstaande schermafbeelding wordt de service omvat een azureblob-index en een realestate-us-sample-index.

   ![Python-script in Jupyter-notebook met HTTP-aanvragen voor Azure Search](media/search-get-started-python/connect-azure-search.png "Python-script in Jupyter-notebook met HTTP-aanvragen voor Azure Search")

   Een verzameling leeg index retourneert deze reactie: `{'@odata.context': 'https://mydemo.search.windows.net/$metadata#indexes(name)', 'value': []}`

> [!Tip]
> Op een gratis service bent u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen. In deze quickstart maakt u één van elk. Zorg ervoor dat er voldoende ruimte is het maken van nieuwe objecten voordat u doorgaat een.

## <a name="1---create-an-index"></a>1 - Een index maken

Tenzij u de portal, wordt een index moet bestaan op de service voordat u gegevens kunt laden. Deze stap maakt gebruik van de [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index) een indexschema naar de service te pushen

De Veldenverzameling definieert u de structuur van een *document*. Vereiste elementen van een index bevatten een naam en een verzameling van velden. Elk veld heeft een naam, type en kenmerken die bepalen hoe deze wordt gebruikt (bijvoorbeeld, of het volledige-tekstindex is kan worden doorzocht, gefilterd of worden opgehaald in de zoekresultaten). In een index, een van de velden van het type `Edm.String` moet worden aangemerkt als de *sleutel* voor de id van het document.

Deze index met de naam "hotels-py" en heeft de velddefinities u hieronder ziet. Dit is een subset van een grotere [index Hotels](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) gebruikt in andere scenario's. We stelling in deze Quick Start voor kort te houden.


1. Plak het volgende voorbeeld in een cel voor het schema in de volgende cel. 

    ```python
    index_schema = {
       "name": "hotels-py",  
       "fields": [
         {"name": "HotelId", "type": "Edm.String", "key": "true", "filterable": "true"},
         {"name": "HotelName", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "true", "facetable": "false"},
         {"name": "Description", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "en.lucene"},
         {"name": "Description_fr", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "fr.lucene"},
         {"name": "Category", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Tags", "type": "Collection(Edm.String)", "searchable": "true", "filterable": "true", "sortable": "false", "facetable": "true"},
         {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Rating", "type": "Edm.Double", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Address", "type": "Edm.ComplexType", 
         "fields": [
         {"name": "StreetAddress", "type": "Edm.String", "filterable": "false", "sortable": "false", "facetable": "false", "searchable": "true"},
         {"name": "City", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "StateProvince", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "PostalCode", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Country", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"}
        ]
       }
      ]
    }
    ```

2. In een andere cel formuleren van de aanvraag. Deze opslag aanvraag is gericht op de verzameling indexen van uw search-service en maakt u een index op basis van het indexschema dat u hebt opgegeven in de vorige stap.

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. Elke stap uitvoeren.

   Het antwoord bevat de JSON-weergave van het schema. De volgende schermafbeelding verwijdert onderdelen van het indexschema zodat u meer van het antwoord kunt zien.

    ![Aanvraag voor het maken van een index](media/search-get-started-python/create-index.png "aanvraag voor het maken van een index")

> [!Tip]
> Voor verificatie, u ook de lijst met indexen in de portal controleren of opnieuw uitvoeren van de service-verbindingsaanvraag om te zien de *hotels py* index die worden vermeld in de verzameling van indexen.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - documenten laden

Als u wilt pushen documenten, gebruik een HTTP POST-aanvraag naar de URL-eindpunt van uw index. De REST-API is [toevoegen, bijwerken of verwijderen documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). Documenten zijn afkomstig uit [HotelsData](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON) op GitHub.

1. In een nieuwe cel bieden drie documenten die aan het indexschema voldoen. Een actie uploaden voor elk document opgeven.

    ```python
    documents = {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
            "StreetAddress": "677 5th Ave",
            "City": "New York",
            "StateProvince": "NY",
            "PostalCode": "10022",
            "Country": "USA"
            }
        },
        {
        "@search.action": "upload",
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
            "StreetAddress": "140 University Town Center Dr",
            "City": "Sarasota",
            "StateProvince": "FL",
            "PostalCode": "34243",
            "Country": "USA"
            }
        },
        {
        "@search.action": "upload",
        "HotelId": "3",
        "HotelName": "Triple Landscape Hotel",
        "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
        }
      }
     ]
    }
    ```

2. In een andere cel formuleren van de aanvraag. Deze POST-aanvraag is gericht op de docs-verzameling van de index hotels py en duwt de documenten die zijn opgegeven in de vorige stap.

   ```python
   url = endpoint + "indexes/hotels-py/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. Elke stap als u wilt de documenten pushen naar een index in uw search-service worden uitgevoerd. Resultaten moeten eruitzien zoals in het volgende voorbeeld. 

   ```
   {'@odata.context': "https://mydemo.search.windows.net/indexes('hotels-py')/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
    'value': [{'errorMessage': None,
            'key': '1',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '2',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '3',
            'status': True,
            'statusCode': 201}]}
     ```


## <a name="3---search-an-index"></a>3 - Een index doorzoeken

In deze stap ziet u hoe u query's een index met behulp van de [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).


1. Geef een query-expressie in een nieuwe cel. Het volgende voorbeeld wordt gezocht in de termen "hotels" en 'Wi-Fi'. Retourneert ook een *aantal* van documenten die overeenkomen met, en *selecteert* welke velden u wilt opnemen in de lijst met zoekresultaten.

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

2. Formuleer een aanvraag. Deze GET-aanvraag is gericht op de docs-verzameling van de index hotels py, en koppelt u de query die u hebt opgegeven in de vorige stap.

   ```python
   url = endpoint + "indexes/hotels-py/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

   Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

   ```
   {'@odata.context': "https://mydemo.search.windows.net/indexes('hotels-py')/$metadata#docs(*)",
    '@odata.count': 3,
    'value': [{'@search.score': 1.0,
               'HotelId': '1',
               'HotelName': 'Secret Point Motel'},
              {'@search.score': 1.0,
               'HotelId': '2',
               'HotelName': 'Twin Dome Motel'},
              {'@search.score': 1.0,
               'HotelId': '3',
               'HotelName': 'Triple Landscape Hotel'}]}
   ```

3. Probeer enkele andere voorbeelden van query om een idee voor de syntaxis. U kunt een filter toepassen, worden de twee bovenste resultaten, sorteren op een bepaald veld of 

   + `searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=hotel&$top=2&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince'`

## <a name="clean-up"></a>Opruimen 

Als u deze niet meer nodig hebt, moet u de index verwijderen. Er is een gratis service beperkt tot drie indexen. Het is raadzaam om te verwijderen van alle indexen die u niet actief gebruikt om ruimte voor andere zelfstudies te maken.

   ```python
  url = endpoint + "indexes/hotels-py" + api_version
  response  = requests.delete(url, headers=headers)
   ```

Index verwijderen kunt u controleren door te retourneren van een lijst met bestaande indexen. Als hotels py verdwenen is, weet u uw aanvraag is voltooid.

```python
url = endpoint + "indexes" + api_version + "&$select=name"

response  = requests.get(url, headers=headers)
index_list = response.json()
pprint(index_list)
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over scenario's en query-syntaxis.

> [!div class="nextstepaction"]
> [Een eenvoudige query maken](search-query-overview.md)
