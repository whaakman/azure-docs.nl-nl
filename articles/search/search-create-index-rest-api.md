---
title: "Quickstart: PowerShell en REST-API's - Azure Search"
description: Maken, laden en query uitvoeren in een index met behulp van PowerShell Invoke-RestMethod en de Azure Search REST-API.
ms.date: 05/16/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: a82ee51a168a018a4df537c05d987974e775b6cc
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795939"
---
# <a name="quickstart-create-an-azure-search-index-using-powershell"></a>Quickstart: Maken van een Azure Search-index met behulp van PowerShell
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

In dit artikel begeleidt u bij het proces van het maken, laden en opvragen van een Azure Search [index](search-what-is-an-index.md) met behulp van PowerShell en de [Azure Search Service REST API's](https://docs.microsoft.com/rest/api/searchservice/). De definitie van de index en doorzoekbare inhoud worden gegeven in de aanvraagtekst als opgemaakte JSON-inhoud.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint en meldt u zich daarna aan voor [Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

+ [PowerShell 5.1 of hoger](https://github.com/PowerShell/PowerShell), met [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) voor opeenvolgende en interactieve stappen.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

2. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

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

Deze index met de naam 'hotels-powershell' en heeft de velddefinities u hieronder ziet. Dit is een subset van een grotere [index Hotels](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) gebruikt in andere scenario's. We stelling in deze Quick Start voor kort te houden.

1. Plak het volgende voorbeeld in PowerShell voor het maken een **$body** -object met het indexschema.

    ```powershell
    $body = @"
    {
        "name": "hotels-powershell",  
        "fields": [
            {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
            {"name": "baseRate", "type": "Edm.Double"},
            {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
            {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
            {"name": "hotelName", "type": "Edm.String", "facetable": false},
            {"name": "category", "type": "Edm.String"},
            {"name": "tags", "type": "Collection(Edm.String)"},
            {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
            {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
            {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
            {"name": "rating", "type": "Edm.Int32"},
            {"name": "location", "type": "Edm.GeographyPoint"}
        ]
    }
    "@
    ```

2. De URI ingesteld op de verzameling indexen op uw service en de *hotels powershell* index.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell?api-version=2019-05-06"
    ```

3. Voer de opdracht uit met **$url**, **$headers**, en **$body** de index te maken op de service. 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    Resultaten moeten er ongeveer als volgt (afgekapt tot de eerste twee velden voor beknoptheid):

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6A99E2DED96B0\"",
        "name":  "hotels-powershell",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "hotelId",
                        "type":  "Edm.String",
                        "searchable":  false,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  false,
                        "facetable":  false,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "baseRate",
                        "type":  "Edm.Double",
                        "searchable":  false,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> Voor verificatie, u ook controleren op de lijst met indexen in de portal of opnieuw uitvoeren van de opdracht die wordt gebruikt om te controleren of verbinding om te zien de *hotels powershell* index die worden vermeld in de verzameling van indexen.

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
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
                "hotelName": "Fancy Stay",
                "category": "Luxury",
                "tags": ["pool", "view", "wifi", "concierge"],
                "parkingIncluded": false,
                "smokingAllowed": false,
                "lastRenovationDate": "2010-06-27T00:00:00Z",
                "rating": 5,
                "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
            },
            {
                "@search.action": "upload",
                "hotelId": "2",
                "baseRate": 79.99,
                "description": "Cheapest hotel in town",
                "hotelName": "Roach Motel",
                "category": "Budget",
                "tags": ["motel", "budget"],
                "parkingIncluded": true,
                "smokingAllowed": true,
                "lastRenovationDate": "1982-04-28T00:00:00Z",
                "rating": 1,
                "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
            },
            {
                "@search.action": "mergeOrUpload",
                "hotelId": "3",
                "baseRate": 129.99,
                "description": "Close to town hall and the river"
            }
        ]
    }
    "@
    ```

1. Het eindpunt instellen op de *hotels powershell* docs-verzameling en de bewerking van de index (indexes/hotels-powershell/docs/index) bevatten.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell/docs/index?api-version=2019-05-06"
    ```

1. Voer de opdracht uit met **$url**, **$headers**, en **$body** documenten laden in de index hotels-powershell.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    Resultaten moeten eruitzien zoals in het volgende voorbeeld. U ziet een statuscode 201. Zie voor een beschrijving van alle statuscodes [HTTP-statuscodes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes/hotels-powershell/$metadata#Collection(Microsoft.Azure.Search.V2017_11_11.IndexResult)",
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
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - Een index doorzoeken

In deze stap ziet u hoe u query's een index met behulp van de [zoeken-API voor documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).

1. Het eindpunt instellen op de *hotels powershell* docs-verzameling en voegt u toe een **zoeken** parameter om op te nemen van queryreeksen. Deze tekenreeks vormt een lege zoekopdracht en geen positie een lijst met alle documenten die worden geretourneerd.

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*'
    ```

1. Voer de opdracht voor het verzenden van de **$url** naar de service.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes/hotels-powershell/$metadata#docs(*)",
        "value":  [
                    {
                        "@search.score":  1.0,
                        "hotelId":  "1",
                        "baseRate":  199.0,
                        "description":  "Best hotel in town",
                        "description_fr":  null,
                        "hotelName":  "Fancy Stay",
                        "category":  "Luxury",
                        "tags":  "pool view wifi concierge",
                        "parkingIncluded":  false,
                        "smokingAllowed":  false,
                        "lastRenovationDate":  "2010-06-27T00:00:00Z",
                        "rating":  5,
                        "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                    },
                    {
                        "@search.score":  1.0,
                        "hotelId":  "2",
                        "baseRate":  79.99,
                        "description":  "Cheapest hotel in town",
                        "description_fr":  null,
                        "hotelName":  "Roach Motel",
                        "category":  "Budget",
                        "tags":  "motel budget",
                        "parkingIncluded":  true,
                        "smokingAllowed":  true,
                        "lastRenovationDate":  "1982-04-28T00:00:00Z",
                        "rating":  1,
                        "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                    },
                    {
                        "@search.score":  1.0,
                        "hotelId":  "3",
                        "baseRate":  129.99,
                        "description":  "Close to town hall and the river",
                        "description_fr":  null,
                        "hotelName":  null,
                        "category":  null,
                        "tags":  "",
                        "parkingIncluded":  null,
                        "smokingAllowed":  null,
                        "lastRenovationDate":  null,
                        "rating":  null,
                        "location":  null
                    }
                ]
    }
    ```

Probeer enkele andere voorbeelden van query om een idee voor de syntaxis. U kunt een zoekactie tekenreeks, verbatim $filter query's uitvoeren, beperken van de resultatenset, scope om te zoeken naar specifieke velden en meer.

```powershell
# Query example 1
# Search the entire index for the term 'budget'
# Return only the `hotelName` field, "Roach hotel"
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell/docs?api-version=2019-05-06&search=budget&$select=hotelName'

# Query example 2 
# Apply a filter to the index to find hotels cheaper than $150 per night
# Returns the `hotelId` and `description`. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*&$filter=baseRate lt 150&$select=hotelId,description'

# Query example 3
# Search the entire index, order by a specific field (`lastRenovationDate`) in descending order
# Take the top two results, and show only `hotelName` and `lastRenovationDate`
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate'
```
## <a name="clean-up"></a>Opruimen 

Als u deze niet meer nodig hebt, moet u de index verwijderen. Er is een gratis service beperkt tot drie indexen. U wilt mogelijk geen indexen die u niet actief gebruikt zodat u kunt stap voor stap door andere zelfstudies verwijderen.

```powershell
# Set the URI to the hotel index
$url = 'https://mydemo.search.windows.net/indexes/hotels-powershell?api-version=2019-05-06'

# Delete the index
Invoke-RestMethod -Uri $url -Headers $headers -Method Delete
```

## <a name="next-steps"></a>Volgende stappen

Probeer Franse beschrijvingen toe te voegen aan de index. Het volgende voorbeeld bevat Franse tekenreeksen en ziet u aanvullende zoekacties. Gebruik mergeOrUpload maken of toevoegen aan bestaande velden. De volgende tekenreeksen moeten UTF-8-codering.

```json
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "1",
            "description_fr": "Meilleur hôtel en ville"
        },
        {
            "@search.action": "merge",
            "hotelId": "2",
            "description_fr": "Hôtel le moins cher en ville"
        }
    ]
}
```
