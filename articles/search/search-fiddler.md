---
title: "Quickstart: Postman en REST API's - Azure Search"
description: Meer informatie over het aanroepen van de Azure Search REST API's met Postman en voorbeeldgegevens en definities.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: bd3b9fe80a57a6a0dd824d92ae14a863ced240b2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793546"
---
# <a name="quickstart-explore-azure-search-rest-apis-using-postman"></a>Quickstart: Verken Azure Search REST API's met Postman
> [!div class="op_single_selector"]
> * [Postman](search-fiddler.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Een van de eenvoudigste manieren om te verkennen de [Azure Search REST API's](https://docs.microsoft.com/rest/api/searchservice) Postman of een andere website testen hulpprogramma wordt gebruikt voor HTTP-aanvragen te formuleren en de reacties. Met de juiste hulpmiddelen en deze instructies kunt u aanvragen verzenden en antwoorden bekijken voordat u code gaat schrijven.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint en meldt u zich daarna aan voor [Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Vereisten

De volgende services en hulpprogramma's worden gebruikt in deze Quick Start. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

+ [Postman bureaublad-app](https://www.getpostman.com/) of [Telerik Fiddler](https://www.telerik.com/fiddler) wordt gebruikt voor het verzenden van aanvragen naar Azure Search.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

In deze sectie maakt gebruik van uw web-hulpprogramma naar keuze voor het instellen van verbindingen met Azure Search. Elk hulpprogramma bewaart informatie over aanvraagheaders voor de sessie, wat betekent dat u alleen moet de api-sleutel en de Content-Type één keer invoeren.

Voor beide hulpprogramma nodig hebt u om een opdracht (GET, POST, PUT, enzovoort), leveren een URL-eindpunt en voor sommige taken, bieden JSON in de hoofdtekst van de aanvraag. De naam van de search service (uw-SEARCH-SERVICE-naam) vervangen door een geldige waarde. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06

U ziet het HTTPS-voorvoegsel, de naam van de service, de naam van een object (in dit geval de verzameling indexen) en de [api-versie](search-api-versions.md). De api-versie is een vereiste, kleine letter tekenreeks die is opgegeven als `?api-version=2019-05-06` voor de huidige versie. API-versies worden regelmatig bijgewerkt. Als u de API-versie toevoegt aan elke aanvraag, kunt u precies bepalen welke versie wordt gebruikt.  

De aanvraagheader bestaat uit twee elementen, inhoudstype, plus de api-sleutel gebruikt om Azure Search te verifiëren. Vervang de beheer-API-sleutel (uw-ADMIN-API-sleutel) met een geldige waarde. 

    api-key: <YOUR-ADMIN-API-KEY>
    Content-Type: application/json

In Postman, Formuleer een aanvraag die lijkt op de volgende schermafbeelding. Kies **ophalen** als de term, geef de URL en klik op **verzenden**. Deze opdracht maakt verbinding met Azure Search, de verzameling indexen leest en HTTP-statuscode 200 wordt op de verbinding is geslaagd. Als uw service al indexen heeft, wordt het antwoord ook index definities bevatten.

![De aanvraagheader voor Postman][6]

## <a name="1---create-an-index"></a>1 - Een index maken

In Azure Search maakt u meestal de index voordat deze worden geladen met gegevens. De [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index) wordt gebruikt voor deze taak. 

De URL is uitgebreid met de `hotel` naam van de index.

Om dit te doen in Postman:

1. Wijzig de bewerking in **plaatsen**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotel?api-version=2019-05-06`.

3. Geef de indexdefinitie (Zie hieronder) in de hoofdtekst van de aanvraag.

4. Klik op **verzenden**.

![Het hoofdgedeelte van de aanvraag in Postman][8]

### <a name="index-definition"></a>Indexdefinitie

De Veldenverzameling definieert de documentstructuur. Elk document moet deze velden, en elk veld moet een gegevenstype hebben. Tekenreeksvelden worden gebruikt voor zoekopdrachten in de volledige tekst. Voeg daarom numerieke gegevens als tekenreeksen toe als u wilt dat naar die inhoud kan worden gezocht.

Kenmerken voor het veld bepalen de toegestane bewerking. De REST API's staan standaard veel bewerkingen toe. Standaard kunnen alle tekenreeksen bijvoorbeeld worden doorzocht, opgehaald en gefilterd en zijn ze geschikt voor facetten. U moet vaak alleen de kenmerken instellen wanneer u moet een gedrag uitschakelen.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }

Wanneer u deze aanvraag indient, krijgt u een HTTP 201-respons om aan te geven dat de index is gemaakt. U kunt deze bewerking controleren in de portal, maar houd er rekening mee dat de portalpagina met vernieuwingsintervallen werkt, zodat het enkele minuten kan duren voordat deze actueel is.

> [!TIP]
> Als u een HTTP 504-respons ontvangt, controleert u of de URL HTTPS bevat. Als de HTTP-fout 400 of 404 wordt weergegeven ziet, controleert u of de aanvraagtekst op fouten die mogelijk zijn opgetreden tijden kopiëren en plakken. Een HTTP 403 duidt doorgaans op een probleem met de API-sleutel (een ongeldige sleutel of een syntaxisfout in de opgegeven API-sleutel).

## <a name="2---load-documents"></a>2 - documenten laden

De index maken en de index vullen zijn afzonderlijke stappen. In Azure Search bevat de index alle doorzoekbare gegevens die u kunt aanleveren als JSON-documenten. De [toevoegen, bijwerken of verwijderen van documenten REST API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) wordt gebruikt voor deze taak. 

De URL is uitgebreid met de `docs` verzamelingen en `index` bewerking.

Om dit te doen in Postman:

1. Wijzig de bewerking in **POST**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs/index?api-version=2019-05-06`.

3. Geef de JSON-documenten (Zie hieronder) in de hoofdtekst van de aanvraag.

4. Klik op **verzenden**.

![Aanvraagpayload in Postman][10]

### <a name="json-documents-to-load-into-the-index"></a>JSON-documenten in de index te laden

De aanvraagtekst bevat vier documenten die moeten worden toegevoegd aan de index hotels.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "description_fr": "Meilleur hôtel en ville",
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
             "description_fr": "Hôtel le moins cher en ville",
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
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Over enkele seconden verschijnt er een HTTP 200-respons in de sessielijst. Dit geeft aan dat de documenten zijn gemaakt. 

Als u een 207-respons ontvang, is minimaal één document niet geüpload. Als u een 404-respons ontvangt, bevat de header of het hoofdgedeelte van de aanvraag een syntaxisfout: controleer of u het eindpunt hebt gewijzigd zodat dit `/docs/index` bevat.

> [!Tip]
> Voor bepaalde gegevensbronnen kunt u de alternatieve methode *indexer* gebruiken die de indexering vereenvoudigt en de hoeveelheid code die is vereist vermindert. Zie [Indexeerbewerkingen](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) voor meer informatie.


## <a name="3---search-an-index"></a>3 - Een index doorzoeken

Nu dat er een index en documenten zijn geladen, kunt u query's op te geven met behulp van [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents).

De URL is uitgebreid met een queryreeks opgegeven met behulp van de search-operator.

Om dit te doen in Postman:

1. Wijzig de bewerking in **ophalen**.

2. Kopieer deze URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2019-05-06`.

3. Klik op **verzenden**.

Deze query zoekt de term 'motel' en retourneert het aantal documenten in de zoekresultaten. De aanvraag en het antwoord moeten eruitzien als in de volgende schermopname voor Postman nadat u op **Send** (Verzenden) hebt geklikt. De statuscode moet 200 zijn.

 ![Reactie op Postman-query][11]


## <a name="get-index-properties"></a>Indexeigenschappen ophalen
U kunt ook een query toepassen op systeemgegevens om het aantal documenten en het opslagverbruik op te vragen: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/stats?api-version=2019-05-06`

In Postman moet uw aanvraag er ongeveer als volgt uitzien en bevat de reactie het aantal documenten en de gebruikte ruimte in bytes.

 ![Systeemquery in Postman][12]

Let op dat de syntaxis van api-version verschilt. Voeg voor deze aanvraag `?` toe aan api-version. De `?` scheidt het URL-pad van de querytekenreeks, terwijl & scheidt ' naam = waarde-paar in de query-tekenreeks. Voor deze query is api-version het eerste en enige item in de querytekenreeks.

Zie voor meer informatie over deze API [Index statistieken REST-API ophalen](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics).


## <a name="use-fiddler"></a>Fiddler gebruiken

In deze sectie is gelijk aan de vorige secties alleen met Fiddler schermafbeeldingen en instructies

### <a name="connect-to-azure-search"></a>Verbinding maken met Azure Search

Formuleer een aanvraag die vergelijkbaar is met de volgende schermopname. Kies **ophalen** als de bewerking. Fiddler voegt `User-Agent=Fiddler` toe. U kunt de twee aanvullende aanvraagheaders op nieuwe regels eronder plakken. Voeg het inhoudstype en de API-sleutel voor uw service toe waarbij u de beheersleutel voor toegang voor uw service gebruikt.

Kopieer voor het doel in een aangepaste versie van deze URL: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06`

![De aanvraagheader van Fiddler][1]

> [!Tip]
> Uitschakelen van beveiligd webverkeer afwijkende en niet-gerelateerde HTTP-activiteit te verbergen. In de Fiddler **bestand** in het menu uitschakelen **verkeer vastleggen**. 

### <a name="1---create-an-index"></a>1 - Een index maken

Wijzig de bewerking in **plaatsen**. Kopiëren in een aangepaste versie van deze URL: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotel?api-version=2019-05-06`. Kopieer de indexdefinitie naar het hoofdgedeelte van de aanvraag hierboven. De pagina zijn vergelijkbaar zijn met de volgende schermafbeelding. Klik op **Execute** bovenaan rechts om de voltooide aanvraag te verzenden.

![Het hoofdgedeelte van de aanvraag in Fiddler][7]

### <a name="2---load-documents"></a>2 - documenten laden

Wijzig de bewerking in **POST**. Wijzig de URL zodat deze `/docs/index` bevat. Kopieer de documenten naar het hoofdgedeelte van de aanvraag, zoals in de volgende schermopname, en voer de aanvraag uit.

![Aanvraagpayload in Fiddler][9]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Tips voor het uitvoeren van onze voorbeeldquery's in Fiddler

De volgende voorbeeldquery is afkomstig uit de [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) artikel. Veel van de voorbeeldquery's in dit artikel bevatten spaties. Deze zijn niet toegestaan in Fiddler. Vervang elke spatie door een plusteken (+) voordat u de queryreeks in Fiddler plakt en uitvoert.

**Voordat de spaties zijn vervangen (in lastRenovationDate desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2019-05-06

**Nadat de spaties zijn vervangen door + (in lastRenovationDate+desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2019-05-06

### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Tips voor het weergeven van indexstatistieken in Fiddler

Klik in Fiddler op het tabblad **Inspectors**, klik op het tabblad **Headers** en selecteer vervolgens de JSON-indeling. Als het goed is wordt het aantal documenten en de opslaggrootte (in kB) weergegeven.

## <a name="next-steps"></a>Volgende stappen

REST-clients zijn zeer nuttig voor onvoorbereide verkenning, maar nu u weet hoe de REST API's werken, kunt u een stapje verdergaan met code. Raadpleeg de volgende koppelingen voor de volgende stappen:

+ [Snelstart: Een index maken met .NET SDK](search-create-index-dotnet.md)
+ [Snelstart: Maken van een index (REST) met behulp van PowerShell](search-create-index-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png