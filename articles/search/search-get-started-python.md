---
title: "Python-Quickstart: Maken en query's uitvoeren met behulp van Azure Search REST API's - Azure Search indexen laden"
description: Wordt uitgelegd hoe u een index maken, gegevens laden en query's uitvoeren met Python, Jupyter-Notebooks en de Azure Search REST-API.
ms.date: 07/11/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 123afa2452c3e492b85292514e64f84d3baec390
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840291"
---
# <a name="quickstart-create-an-azure-search-index-in-python-using-jupyter-notebooks"></a>Quickstart: Een Azure Search-index maken in Python met behulp van Jupyter notebooks
> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-get-started-postman.md)
> * [Portal](search-create-index-portal.md)
> 

Bouw een Jupyter-notebook die wordt gemaakt, wordt geladen en query's van een Azure Search-index met behulp van Python en de [Azure Search REST API's](https://docs.microsoft.com/rest/api/searchservice/). In dit artikel wordt uitgelegd hoe u een stap voor stap-notebook maken. U kunt ook [downloaden en uitvoeren van een voltooide Jupyter Python-notebook](https://github.com/Azure-Samples/azure-search-python-samples).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), bieden Python 3.x en Jupyter-Notebooks.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. Voor deze Quick Start kunt u de gratis laag. 

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

In deze taak een Jupyter-notebook start en controleer of dat u verbinding met Azure Search maken kunt. U kunt dit doen door het aanvragen van een lijst van de indexen van uw service. Op Windows met Anaconda3, kunt u Anaconda Navigator een notebook te starten.

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

1. In de derde cel formuleren van de aanvraag. Deze GET-aanvraag is gericht op de verzameling indexen van uw search-service en selecteert u de eigenschap name van de bestaande indexen.

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. Elke stap uitvoeren. Als indexen aanwezig is, betekent dit dat het antwoord een lijst met indexnamen bevat. In de onderstaande schermafbeelding heeft de service al een azureblob-index en een realestate-us-sample-index.

   ![Python-script in Jupyter-notebook met HTTP-aanvragen voor Azure Search](media/search-get-started-python/connect-azure-search.png "Python-script in Jupyter-notebook met HTTP-aanvragen voor Azure Search")

   Een verzameling leeg index wordt daarentegen dit antwoord geretourneerd: `{'@odata.context': 'https://mydemo.search.windows.net/$metadata#indexes(name)', 'value': []}`

## <a name="1---create-an-index"></a>1 - Een index maken

Tenzij u de portal, wordt een index moet bestaan op de service voordat u gegevens kunt laden. Deze stap maakt gebruik van de [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index) een indexschema naar de service te pushen.

Vereiste elementen van een index bevatten een naam, een verzameling van velden en een sleutel. De Veldenverzameling definieert u de structuur van een *document*. Elk veld heeft een naam, type en kenmerken die bepalen hoe het veld wordt gebruikt (bijvoorbeeld, of het volledige-tekstindex is kan worden doorzocht, gefilterd of worden opgehaald in de zoekresultaten). In een index, een van de velden van het type `Edm.String` moet worden aangemerkt als de *sleutel* voor de id van het document.

Deze index met de naam "hotels-quickstart" en heeft de velddefinities u hieronder ziet. Dit is een subset van een grotere [index Hotels](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) gebruikt in andere scenario's. We stelling in deze Quick Start voor kort te houden.

1. Plak het volgende voorbeeld in een cel voor het schema in de volgende cel. 

    ```python
    index_schema = {
       "name": "hotels-quickstart",  
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

2. In een andere cel formuleren van de aanvraag. Deze opslag aanvraag is gericht op de verzameling indexen van uw search-service en maakt u een index op basis van het indexschema dat u hebt opgegeven in de vorige cel.

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. Elke stap uitvoeren.

   Het antwoord bevat de JSON-weergave van het schema. De volgende schermafbeelding is slechts een deel van het antwoord weergegeven.

    ![Aanvraag voor het maken van een index](media/search-get-started-python/create-index.png "aanvraag voor het maken van een index")

> [!Tip]
> Er is een andere manier om te controleren of index maken om te controleren of lijst van de indexen in de portal.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - documenten laden

Als u wilt pushen documenten, gebruik een HTTP POST-aanvraag naar de URL-eindpunt van uw index. De REST-API is [toevoegen, bijwerken of verwijderen documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). Documenten zijn afkomstig uit [HotelsData](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON) op GitHub.

1. In een nieuwe cel, bieden u vier documenten die aan het indexschema voldoen. Een actie uploaden voor elk document opgeven.

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
        },
        {
        "@search.action": "upload",
        "HotelId": "4",
        "HotelName": "Sublime Cliff Hotel",
        "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
        "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
            }
        }
    ]
    }
    ```   

2. In een andere cel formuleren van de aanvraag. Deze POST-aanvraag is gericht op de docs-verzameling van de index hotels-quickstart en duwt de documenten die zijn opgegeven in de vorige stap.

   ```python
   url = endpoint + "indexes/hotels-quickstart/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. Elke stap als u wilt de documenten pushen naar een index in uw search-service worden uitgevoerd. Resultaten moeten eruitzien zoals in het volgende voorbeeld. 

    ![Documenten verzenden naar een index](media/search-get-started-python/load-index.png "documenten verzenden naar een index")

## <a name="3---search-an-index"></a>3 - Een index doorzoeken

In deze stap ziet u hoe u query's een index met behulp van de [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).

1. In een cel, Geef een queryexpressie die een lege zoekopdracht wordt uitgevoerd (search = *), retourneert een lijst met geen positie (zoeken naar score = 1,0) van willekeurige documenten. Standaard retourneert Azure Search de 50 overeenkomsten op een tijdstip. Deze query retourneert als gestructureerde, de structuur van een hele document en waarden. Toevoegen van $count = true voor een telling van alle documenten in de resultaten.

   ```python
   searchstring = '&search=*&$count=true'
   ```

1. Geef in het volgende voorbeeld om te zoeken op de termen "hotels" en "Wi-Fi" in een nieuwe cel. Voeg $select om op te geven welke velden u wilt opnemen in de lijst met zoekresultaten.

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

1. Formuleer een aanvraag in een andere cel. Deze GET-aanvraag is gericht op de docs-verzameling van de index hotels-quickstart, en koppelt u de query die u hebt opgegeven in de vorige stap.

   ```python
   url = endpoint + "indexes/hotels-quickstart/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

1. Elke stap uitvoeren. Resultaten zijn vergelijkbaar zijn met de volgende uitvoer. 

    ![Een index doorzoeken](media/search-get-started-python/search-index.png "een index doorzoeken")

1. Probeer enkele andere voorbeelden van query om een idee voor de syntaxis. U kunt vervangen de `searchstring` met de volgende voorbeelden en voer de zoekaanvraag. 

   Een filter toepassen: 

   ```python
   searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description,Rating'
   ```

   De twee bovenste resultaten nemen:

   ```python
   searchstring = '&search=boutique&$top=2&$select=HotelId,HotelName,Description,Category'
   ```

    Sorteren op een bepaald veld:

   ```python
   searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince, Tags'
   ```

## <a name="clean-up"></a>Opruimen

Wanneer u in uw eigen abonnement werkt, is het een goed idee aan het einde van een project om te bepalen of u moet nog steeds de resources die dat u hebt gemaakt. Resources naar links wordt uitgevoerd kan kosten u geld. U kunt afzonderlijke resources verwijderen of verwijder de resourcegroep als u wilt verwijderen van de volledige set van resources.

U kunt zoeken en beheren van resources in de portal, met behulp van de **alle resources** of **resourcegroepen** koppeling in het deelvenster navigatie aan de linkerkant.

Als u van een gratis service gebruikmaakt, houd er rekening mee dat u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen bent. U kunt afzonderlijke items in de portal om te blijven onder de limiet verwijderen. 

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start gebruikt als een vereenvoudiging, een verkorte versie van de index Hotels. U kunt de volledige versie voor het uitproberen van interessante query's maken. Uitvoeren om de volledige versie en alle 50 documenten, het **gegevens importeren** wizard selecteren *hotels-sample* van de ingebouwde voorbeeld-gegevensbronnen.

> [!div class="nextstepaction"]
> [Snelstart: Een index maken in Azure portal](search-get-started-portal.md)
