---
title: "Snelstartgids voor PowerShell: Maken en query's uitvoeren met behulp van Azure Search REST API's - Azure Search indexen laden"
description: Wordt uitgelegd hoe u een index maken, gegevens laden en uitvoeren van query's met behulp van PowerShell Invoke-RestMethod en de Azure Search REST-API.
ms.date: 06/10/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: afd73ee3461fff11019be887dbf3078963644c5b
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485488"
---
# <a name="quickstart-create-an-azure-search-index-in-powershell-using-rest-apis"></a>Quickstart: Een Azure Search-index in PowerShell met behulp van REST-API's maken
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

In dit artikel begeleidt u bij het proces van het maken, laden en query's in een Azure Search-index met behulp van PowerShell en de [Azure Search REST API's](https://docs.microsoft.com/rest/api/searchservice/). In dit artikel wordt uitgelegd hoe u PowerShell-opdrachten interactief uitvoeren. U kunt ook een klaar script uitvoeren. Als u wilt een kopie downloaden, gaat u naar de [azure-search-powershell-voorbeelden](https://github.com/Azure-Samples/azure-search-powershell-samples/tree/master/Quickstart) opslagplaats.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint en meldt u zich daarna aan voor [Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [PowerShell 5.1 of hoger](https://github.com/PowerShell/PowerShell), met [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) voor opeenvolgende en interactieve stappen.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

2. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

1. In PowerShell, maakt u een **$headers** object voor het opslaan van de inhoud van het type en de API-sleutel. Vervang de beheer-API-sleutel (uw-ADMIN-API-sleutel) met een sleutel die geldig is voor uw zoekservice. U hoeft alleen te deze header eenmaal worden ingesteld voor de duur van de sessie, maar u gaat deze toevoegen aan elke aanvraag. 

    ```powershell
    $headers = @{
    'api-key' = '<YOUR-ADMIN-API-KEY>'
    'Content-Type' = 'application/json' 
    'Accept' = 'application/json' }
    ```

2. Maak een **$url** -object van de service bepaalt geïndexeerd verzameling. De naam van de service (uw-SEARCH-SERVICE-naam) vervangen door een geldige search-service.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06"
    ```

3. Voer **Invoke-RestMethod** een GET-aanvraag verzenden naar de service en controleer of de verbinding. Voeg **ConvertTo Json** zodat u de reactie van de service-responsen kunt bekijken.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

   Als de service leeg is en geen indexen heeft, zijn resultaten vergelijkbaar met het volgende voorbeeld. Anders is, ziet u een JSON-weergave van definities van de index.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes",
        "value":  [

                ]
    }
    ```

## <a name="1---create-an-index"></a>1 - Een index maken

Tenzij u de portal, wordt een index moet bestaan op de service voordat u gegevens kunt laden. Deze stap definieert de index en gepusht naar de service. De [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index) voor deze stap wordt gebruikt.

Vereiste elementen van een index bevatten een naam en een verzameling van velden. De Veldenverzameling definieert u de structuur van een *document*. Elk veld heeft een naam, type en kenmerken die bepalen hoe deze wordt gebruikt (bijvoorbeeld, of het volledige-tekstindex is kan worden doorzocht, gefilterd of worden opgehaald in de zoekresultaten). In een index, een van de velden van het type `Edm.String` moet worden aangemerkt als de *sleutel* voor de id van het document.

Deze index met de naam "hotels-quickstart" en heeft de velddefinities u hieronder ziet. Dit is een subset van een grotere [index Hotels](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) gebruikt in andere scenario's. We stelling in deze Quick Start voor kort te houden.

1. Plak het volgende voorbeeld in PowerShell voor het maken een **$body** -object met het indexschema.

    ```powershell
    $body = @"
    {
        "name": "hotels-quickstart",  
        "fields": [
            {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
            {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
            {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
            {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
            {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
            {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Address", "type": "Edm.ComplexType", 
            "fields": [
            {"name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true},
            {"name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true}
            ]
         }
      ]
    }
    "@
    ```

2. De URI ingesteld op de verzameling indexen op uw service en de *hotels-quickstart* index.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06"
    ```

3. Voer de opdracht uit met **$url**, **$headers**, en **$body** de index te maken op de service. 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    Resultaten moeten er ongeveer als volgt (afgekapt tot de eerste twee velden voor beknoptheid):

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6EDE28CFEABDA\"",
        "name":  "hotels-quickstart",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "HotelId",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "HotelName",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  false,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  false,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> Voor verificatie, kunt u ook de lijst met indexen in de portal controleren.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - documenten laden

Als u wilt pushen documenten, gebruik een HTTP POST-aanvraag naar de URL-eindpunt van uw index. De REST-API voor deze taak is [toevoegen, bijwerken of verwijderen documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

1. Plak het volgende voorbeeld in PowerShell voor het maken een **$body** object met de documenten die u wilt uploaden. 

    Dit verzoek bevat twee vol is en een gedeeltelijke record. De gedeeltelijke record ziet u dat u onvolledig documenten kunt uploaden. De `@search.action` parameter geeft u op hoe indexering werkt. Geldige waarden zijn uploaden, samenvoegen, mergeOrUpload en verwijderen. Het gedrag mergeOrUpload ofwel maakt u een nieuw document voor hotelId = 3, of de inhoud bijgewerkt als deze al bestaat.

    ```powershell
    $body = @"
    {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
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
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
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
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": 
            {
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
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": 
            {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
            }
        }
    ]
    }
    "@
    ```

1. Het eindpunt instellen op de *hotels-quickstart* docs-verzameling en de bewerking van de index (indexes/hotels-quickstart/docs/index) bevatten.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06"
    ```

1. Voer de opdracht uit met **$url**, **$headers**, en **$body** documenten in de index hotels-quickstart laden.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    Resultaten moeten eruitzien zoals in het volgende voorbeeld. U ziet een [statuscode 201](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
        "value":  [
                    {
                        "key":  "1",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "2",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "3",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "4",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - Een index doorzoeken

In deze stap ziet u hoe u query's een index met behulp van de [zoeken-API voor documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).

Zorg ervoor dat u enkele aanhalingstekens zoekopdracht $urls in. Querytekenreeksen bevatten **$** tekens, en u kunt weglaten als u deze als de gehele tekenreeks tussen enkele aanhalingstekens is geplaatst met...

1. Het eindpunt instellen op de *hotels-quickstart* docs-verzameling en voegt u toe een **zoeken** parameter om door te geven in een queryreeks. 
  
   Deze tekenreeks een lege zoekopdracht wordt uitgevoerd (search = *), retourneert een lijst met geen positie (zoeken naar score = 1,0) van willekeurige documenten. Standaard retourneert Azure Search de 50 overeenkomsten op een tijdstip. Deze query retourneert als gestructureerde, de structuur van een hele document en waarden. Voeg **$count = true** een telling van alle documenten in de resultaten ophalen.

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$count=true'
    ```

1. Voer de opdracht voor het verzenden van de **$url** naar de service.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

    ```
    {
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#docs(*)",
    "@odata.count":  4,
    "value":  [
                  {
                      "@search.score":  0.1547872,
                      "HotelId":  "2",
                      "HotelName":  "Twin Dome Motel",
                      "Description":  "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                      "Category":  "Boutique",
                      "Tags":  "pool free wifi concierge",
                      "ParkingIncluded":  false,
                      "LastRenovationDate":  "1979-02-18T00:00:00Z",
                      "Rating":  3.6,
                      "Address":  "@{StreetAddress=140 University Town Center Dr; City=Sarasota; StateProvince=FL; PostalCode=34243; Country=USA}"
                  },
                  {
                      "@search.score":  0.009068266,
                      "HotelId":  "3",
                      "HotelName":  "Triple Landscape Hotel",
                      "Description":  "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel\u0027s restaurant services.",
                      "Category":  "Resort and Spa",
                      "Tags":  "air conditioning bar continental breakfast",
                      "ParkingIncluded":  true,
                      "LastRenovationDate":  "2015-09-20T00:00:00Z",
                      "Rating":  4.8,
                      "Address":  "@{StreetAddress=3393 Peachtree Rd; City=Atlanta; StateProvince=GA; PostalCode=30326; Country=USA}"
                  },
                . . . 
    ```

Probeer enkele andere voorbeelden van query om een idee voor de syntaxis. U kunt een zoekactie tekenreeks, verbatim $filter query's uitvoeren, beperken van de resultatenset, scope om te zoeken naar specifieke velden en meer.

```powershell
# Query example 1
# Search the entire index for the terms 'restaurant' and 'wifi'
# Return only the HotelName, Description, and Tags fields
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=restaurant wifi&$count=true&$select=HotelName,Description,Tags'

# Query example 2 
# Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$filter=Rating gt 4&$select=HotelName,Rating'

# Query example 3
# Take the top two results, and show only HotelName and Category in the results
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=boutique&$top=2&$select=HotelName,Category'

# Query example 4
# Sort by a specific field (Address/City) in ascending order

$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating'
```
## <a name="clean-up"></a>Opruimen 

Als u deze niet meer nodig hebt, moet u de index verwijderen. Er is een gratis service beperkt tot drie indexen. U wilt mogelijk geen indexen die u niet actief gebruikt zodat u kunt stap voor stap door andere zelfstudies verwijderen.

```powershell
# Set the URI to the hotel index
$url = 'https://mydemo.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06'

# Delete the index
Invoke-RestMethod -Uri $url -Headers $headers -Method Delete
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start, kunt u PowerShell gebruikt om de algemene werkstroom voor het maken en toegang krijgen tot inhoud in Azure Search te doorlopen. Met de concepten in gedachten, kunt het beste overstappen op aan meer geavanceerde scenario's, zoals het indexeren van Azure-gegevensbronnen.

> [!div class="nextstepaction"]
> [REST-zelfstudie: Indexeren en doorzoeken van semi-gestructureerde gegevens (JSON-blobs) in Azure Search](search-semi-structured-data.md)