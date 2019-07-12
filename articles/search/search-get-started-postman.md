---
title: "Quickstart: Maken en query's uitvoeren met behulp van Postman en REST-API's - Azure Search indexen laden"
description: Meer informatie over het aanroepen van de Azure Search REST API's met Postman en voorbeeldgegevens en definities.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 07/11/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 015dd3631322978d6416041a3eea8390a72b0c17
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840183"
---
# <a name="quickstart-create-an-azure-search-index-in-postman-using-rest-apis"></a>Quickstart: Een Azure Search-index in Postman met behulp van REST-API's maken
> [!div class="op_single_selector"]
> * [Postman](search-get-started-postman.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Een van de eenvoudigste manieren om te verkennen de [Azure Search REST API's](https://docs.microsoft.com/rest/api/searchservice) Postman of een andere website testen hulpprogramma wordt gebruikt voor HTTP-aanvragen te formuleren en de reacties. Met de juiste hulpmiddelen en deze instructies kunt u aanvragen verzenden en antwoorden bekijken voordat u code gaat schrijven.

In dit artikel wordt uitgelegd hoe u aanvragen interactief te formuleren. U kunt ook [downloaden en importeren van een Postman-verzameling](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Quickstart) gebruik van vooraf gedefinieerde aanvragen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Postman bureaublad-app](https://www.getpostman.com/) wordt gebruikt voor het verzenden van aanvragen naar Azure Search.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

In deze sectie maakt gebruik van uw web-hulpprogramma naar keuze voor het instellen van verbindingen met Azure Search. Elk hulpprogramma bewaart informatie over aanvraagheaders voor de sessie, wat betekent dat u alleen moet de api-sleutel en de Content-Type één keer invoeren.

Voor beide hulpprogramma nodig hebt u om een opdracht (GET, POST, PUT, enzovoort), leveren een URL-eindpunt en voor sommige taken, bieden JSON in de hoofdtekst van de aanvraag. De naam van de search service (uw-SEARCH-SERVICE-naam) vervangen door een geldige waarde. Voeg `$select=name` om terug te keren alleen de naam van elke index. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06&$select=name

U ziet het HTTPS-voorvoegsel, de naam van de service, de naam van een object (in dit geval de verzameling indexen) en de [api-versie](search-api-versions.md). De api-versie is een vereiste, kleine letter tekenreeks die is opgegeven als `?api-version=2019-05-06` voor de huidige versie. API-versies worden regelmatig bijgewerkt. Als u de API-versie toevoegt aan elke aanvraag, kunt u precies bepalen welke versie wordt gebruikt.  

De aanvraagheader bestaat uit twee elementen, inhoudstype, plus de api-sleutel gebruikt om Azure Search te verifiëren. De beheer-API-sleutel (YOUR-AZURE-SEARCH-ADMIN-API-KEY) vervangen door een geldige waarde. 

    api-key: <YOUR-AZURE-SEARCH-ADMIN-API-KEY>
    Content-Type: application/json

In Postman, Formuleer een aanvraag die lijkt op de volgende schermafbeelding. Kies **ophalen** als de term, geef de URL en klik op **verzenden**. Deze opdracht maakt verbinding met Azure Search, de verzameling indexen leest en HTTP-statuscode 200 wordt op de verbinding is geslaagd. Als uw service al indexen heeft, wordt het antwoord ook index definities bevatten.

![Postman-aanvraag-URL en koptekst](media/search-get-started-postman/postman-url.png "Postman-aanvraag-URL en koptekst")

## <a name="1---create-an-index"></a>1 - Een index maken

In Azure Search maakt u meestal de index voordat deze worden geladen met gegevens. De [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index) wordt gebruikt voor deze taak. 

De URL is uitgebreid met de `hotels` naam van de index.

Om dit te doen in Postman:

1. Wijzig de bewerking in **plaatsen**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels?api-version=2019-05-06`.

3. Geef de indexdefinitie (gereed is voor kopiëren-code hieronder vindt u) in de hoofdtekst van de aanvraag.

4. Klik op **verzenden**.

![Index JSON-document in de aanvraagtekst](media/search-get-started-postman/postman-request.png "Index JSON-document in de aanvraagtekst")

### <a name="index-definition"></a>Indexdefinitie

De Veldenverzameling definieert de documentstructuur. Elk document moet deze velden, en elk veld moet een gegevenstype hebben. Tekenreeksvelden worden gebruikt voor zoekopdrachten in de volledige tekst. Voeg daarom numerieke gegevens als tekenreeksen toe als u wilt dat naar die inhoud kan worden gezocht.

Kenmerken voor het veld bepalen de toegestane bewerking. De REST API's staan standaard veel bewerkingen toe. Standaard kunnen alle tekenreeksen bijvoorbeeld worden doorzocht, opgehaald en gefilterd en zijn ze geschikt voor facetten. U moet vaak alleen de kenmerken instellen wanneer u moet een gedrag uitschakelen.

```json
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
```

Wanneer u deze aanvraag indient, krijgt u een HTTP 201-respons om aan te geven dat de index is gemaakt. U kunt deze bewerking controleren in de portal, maar houd er rekening mee dat de portalpagina met vernieuwingsintervallen werkt, zodat het enkele minuten kan duren voordat deze actueel is.

> [!TIP]
> Als u een HTTP 504-respons ontvangt, controleert u of de URL HTTPS bevat. Als de HTTP-fout 400 of 404 wordt weergegeven ziet, controleert u of de aanvraagtekst op fouten die mogelijk zijn opgetreden tijden kopiëren en plakken. Een HTTP 403 duidt doorgaans op een probleem met de API-sleutel (een ongeldige sleutel of een syntaxisfout in de opgegeven API-sleutel).

## <a name="2---load-documents"></a>2 - documenten laden

De index maken en de index vullen zijn afzonderlijke stappen. In Azure Search bevat de index alle doorzoekbare gegevens die u kunt aanleveren als JSON-documenten. De [toevoegen, bijwerken of verwijderen van documenten REST API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) wordt gebruikt voor deze taak. 

De URL is uitgebreid met de `docs` verzamelingen en `index` bewerking.

Om dit te doen in Postman:

1. Wijzig de bewerking in **POST**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06`.

3. Geef de JSON-documenten (gereed is voor kopiëren-code is lager dan) in de hoofdtekst van de aanvraag.

4. Klik op **verzenden**.

![JSON-documenten in de aanvraagtekst](media/search-get-started-postman/postman-docs.png "JSON-documenten in de hoofdtekst van de aanvraag")

### <a name="json-documents-to-load-into-the-index"></a>JSON-documenten in de index te laden

De aanvraagtekst bevat vier documenten die moeten worden toegevoegd aan de index hotels.

```json
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
```

In een paar seconden ziet u een HTTP 201-respons in de sessielijst. Dit geeft aan dat de documenten zijn gemaakt. 

Als u een 207-respons ontvang, is minimaal één document niet geüpload. Als u een 404-respons ontvangt, bevat de header of het hoofdgedeelte van de aanvraag een syntaxisfout: controleer of u het eindpunt hebt gewijzigd zodat dit `/docs/index` bevat.

> [!Tip]
> Voor bepaalde gegevensbronnen kunt u de alternatieve methode *indexer* gebruiken die de indexering vereenvoudigt en de hoeveelheid code die is vereist vermindert. Zie [Indexeerbewerkingen](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) voor meer informatie.


## <a name="3---search-an-index"></a>3 - Een index doorzoeken

Nu dat er een index en documenten zijn geladen, kunt u query's op te geven met behulp van [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).

De URL is uitgebreid met een query-expressie opgegeven met behulp van de search-operator.

Om dit te doen in Postman:

1. Wijzig de bewerking in **ophalen**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$count=true&api-version=2019-05-06`.

3. Klik op **verzenden**.

Deze query is een lege en retourneert het aantal documenten in de lijst met zoekresultaten. De aanvraag en het antwoord moeten eruitzien als in de volgende schermopname voor Postman nadat u op **Send** (Verzenden) hebt geklikt. De statuscode moet 200 zijn.

 ![AAN de met de zoektekenreeks op de URL](media/search-get-started-postman/postman-query.png "ophalen met de zoektekenreeks op de URL")

Probeer enkele andere voorbeelden van query om een idee voor de syntaxis. U kunt een zoekactie tekenreeks, verbatim $filter query's uitvoeren, beperken van de resultatenset, scope om te zoeken naar specifieke velden en meer.

Wisselen van de URL voor de huidige door die hieronder te klikken op **verzenden** telkens wanneer de resultaten wilt weergeven.

```
# Query example 1 - Search on restaurant and wifi
# Return only the HotelName, Description, and Tags fields
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=restaurant wifi&$count=true&$select=HotelName,Description,Tags&api-version=2019-05-06

# Query example 2 - Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$filter=Rating gt 4&$select=HotelName,Rating&api-version=2019-05-06

# Query example 3 - Take the top two results, and show only HotelName and Category in the results
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=boutique&$top=2&$select=HotelName,Category&api-version=2019-05-06

# Query example 4 - Sort by a specific field (Address/City) in ascending order
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating&api-version=2019-05-06
```

## <a name="get-index-properties"></a>Indexeigenschappen ophalen
U kunt ook [statistieken ophalen](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) query's uitvoeren voor het aantal documenten en indexeren, grootte: 

```
https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/stats?api-version=2019-05-06`
```

Toe te voegen `/stats` naar uw URL retourneert informatie over de index. In Postman moet uw aanvraag er ongeveer als volgt uitzien en bevat de reactie het aantal documenten en de gebruikte ruimte in bytes.

 ![Index informatie](media/search-get-started-postman/postman-system-query.png "indexinformatie ophalen")

Let op dat de syntaxis van api-version verschilt. Voeg voor deze aanvraag `?` toe aan api-version. De `?` scheidt het URL-pad van de querytekenreeks, terwijl & scheidt ' naam = waarde-paar in de query-tekenreeks. Voor deze query is api-version het eerste en enige item in de querytekenreeks.

## <a name="clean-up"></a>Opruimen

Wanneer u in uw eigen abonnement werkt, is het een goed idee aan het einde van een project om te bepalen of u moet nog steeds de resources die dat u hebt gemaakt. Resources naar links wordt uitgevoerd kan kosten u geld. U kunt afzonderlijke resources verwijderen of verwijder de resourcegroep als u wilt verwijderen van de volledige set van resources.

U kunt zoeken en beheren van resources in de portal, met behulp van de **alle resources** of **resourcegroepen** koppeling in het deelvenster navigatie aan de linkerkant.

Als u van een gratis service gebruikmaakt, houd er rekening mee dat u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen bent. U kunt afzonderlijke items in de portal om te blijven onder de limiet verwijderen. 

## <a name="next-steps"></a>Volgende stappen

REST-clients zijn zeer nuttig voor onvoorbereide verkenning, maar nu u weet hoe de REST API's werken, kunt u een stapje verdergaan met code. Zie de volgende koppeling voor de volgende stap:

+ [Snelstart: Een index maken met .NET SDK](search-get-started-dotnet.md)
