---
title: "Node. js Quick Start: Indexen maken, laden en query's uitvoeren met behulp van Azure Search REST-Api's-Azure Search"
description: Voor beeld van node. js voor Azure Search, waarin wordt uitgelegd hoe u gegevens maakt, laadt en een query uitvoert vanuit Java script.
author: lobrien
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: laobri
ms.openlocfilehash: f1420bd4ebf4ef586a8f306d4a2037fc3247c9c0
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882625"
---
# <a name="quickstart-create-an-azure-search-index-in-nodejs"></a>Quickstart: Een Azure Search-index maken in node. js
> [!div class="op_single_selector"]
> * [JavaScript](search-get-started-nodejs.md)
> * [C#](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)

Maak een node. js-toepassing die een Azure Search index maakt, laadt en opvraagt. In dit artikel wordt beschreven hoe u de toepassing stap voor stap maakt. U kunt ook [de bron code en gegevens downloaden](https://github.com/Azure-Samples/azure-search-javascript-samples/quickstart/) en de toepassing uitvoeren vanaf de opdracht regel.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogram ma's en gegevens worden gebruikt in deze Quick Start.

+ [Node.js](https://nodejs.org).
+ [NPM](https://www.npmjs.com) moet worden geïnstalleerd door node. js.
+ In dit artikel vindt u een voor beeld van een structuur van de index en de bijbehorende documenten, of van de [opslag plaats](https://github.com/Azure-Samples/azure-search-javascript-samples/quickstart/).
+ [Een Azure Search-service maken](search-create-service-portal.md) of [een bestaande service vinden](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) onder uw huidige abonnement. U kunt een gratis service voor deze Quick Start gebruiken.

Aanbevelingen

* [Visual Studio Code](https://code.visualstudio.com).
* [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) -en [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) -uitbrei dingen voor VSCode.

<a name="get-service-info"></a>
## <a name="get-keys-and-urls"></a>Sleutels en Url's ophalen

Aanroepen naar de service vereisen een URL-eind punt en een toegangs sleutel voor elke aanvraag. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij de Azure Portal](https://portal.azure.com/)en haal de naam van uw zoek service op in de pagina **overzicht** van de zoek service. U kunt uw service naam bevestigen door de URL van het eind punt te controleren. Als uw eind punt- `https://mydemo.search.windows.net`URL zou zijn, zou uw `mydemo`service naam zouden zijn.

2. Haal in **instellingen** > **sleutels**een beheerders sleutel op voor volledige rechten op de service. Er zijn twee uitwissel bare beheer sleutels die voor bedrijfs continuïteit worden verschaft, voor het geval dat u een voor beeld moet doen. U kunt de primaire of secundaire sleutel gebruiken op aanvragen voor het toevoegen, wijzigen en verwijderen van objecten.

    Haal ook de query sleutel op. Het is een best practice voor het uitgeven van query aanvragen met alleen-lezen toegang.

![De service naam en de beheer-en query sleutels ophalen](media/search-get-started-nodejs/service-name-and-keys.png)

Alle aanvragen vereisen een API-sleutel in de header van elke aanvraag die naar uw service wordt verzonden. Een geldige sleutel brengt een vertrouwens relatie tot stand, op basis van aanvraag, tussen de toepassing die de aanvraag verzendt en de service die deze verwerkt.

## <a name="set-up-your-environment"></a>Uw omgeving instellen

Begin met het openen van een Power shell-console of een andere omgeving waarin u node. js hebt geïnstalleerd.

1. Maak een ontwikkelingsprogram Directory met de naam `quickstart` :

    ```powershell
    mkdir quickstart
    cd quickstart
    ```

2. Initialiseer een leeg project met NPM door uit te `npm init`voeren. Accepteer de standaard waarden, met uitzonde ring van de licentie, die u moet instellen op ' MIT '. 

1. Voeg pakketten toe die afhankelijk zijn van de code en hulp in ontwikkeling:

    ```powershell
    npm install nconf node-fetch
    npm install --save-dev eslint eslint-config-prettier eslint-config-airbnb-base eslint-plugin-import prettier
    ```

4. Controleer of u de projecten en de bijbehorende afhankelijkheden hebt geconfigureerd door te controleren of het bestand **package. json** er ongeveer als volgt uitziet:

    ```json
    {
      "name": "quickstart",
      "version": "1.0.0",
      "description": "Azure Search Quickstart",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [
        "Azure",
        "Azure_Search"
      ],
      "author": "Your Name",
      "license": "MIT",
      "dependencies": {
        "nconf": "^0.10.0",
        "node-fetch": "^2.6.0"
      },
      "devDependencies": {
        "eslint": "^6.1.0",
        "eslint-config-airbnb-base": "^13.2.0",
        "eslint-config-prettier": "^6.0.0",
        "eslint-plugin-import": "^2.18.2",
        "prettier": "^1.18.2"
      }
    }
    ```

<a name="configure"></a>

## <a name="1---define-and-create-index"></a>1-index definiëren en maken 

Maak een bestand **azure_search_config. json** om uw zoek service gegevens op te slaan:

    ```json
    {
        "serviceName" : "[SERVICE_NAME]",
        "adminKey" : "[ADMIN_KEY]",
        "queryKey" : "[QUERY_KEY]",
        "indexName" : "hotels-quickstart"
    }
    ```

Vervang de `[SERVICE_NAME]` waarde door de naam van uw zoek service. Vervang `[ADMIN_KEY]` en`[QUERY_KEY]` door de sleutel waarden die u eerder hebt vastgelegd. 

In Azure Search zijn documenten gegevens structuren die beide invoer zijn voor het indexeren en uitvoeren van query's. Invoer van documenten kan rijen zijn in een Data Base, blobs in Blob-opslag of, zoals in dit voor beeld, JSON-documenten op schijf. U kunt [Hotels. json](https://github.com/Azure-Samples/azure-search-javascript-samples/quickstart/blob/master/hotels.json) downloaden of uw eigen **Hotels. json** -bestand maken met de volgende inhoud:

    ```json
    {
        "value": [
            {
                "HotelId": "1",
                "HotelName": "Secret Point Motel",
                "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
                "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
                "Category": "Boutique",
                "Tags": ["pool", "air conditioning", "concierge"],
                "ParkingIncluded": false,
                "LastRenovationDate": "1970-01-18T00:00:00Z",
                "Rating": 3.6,
                "Address": {
                    "StreetAddress": "677 5th Ave",
                    "City": "New York",
                    "StateProvince": "NY",
                    "PostalCode": "10022"
                }
            },
            {
                "HotelId": "2",
                "HotelName": "Twin Dome Motel",
                "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                "Category": "Boutique",
                "Tags": ["pool", "free wifi", "concierge"],
                "ParkingIncluded": "false",
                "LastRenovationDate": "1979-02-18T00:00:00Z",
                "Rating": 3.6,
                "Address": {
                    "StreetAddress": "140 University Town Center Dr",
                    "City": "Sarasota",
                    "StateProvince": "FL",
                    "PostalCode": "34243"
                }
            },
            {
                "HotelId": "3",
                "HotelName": "Triple Landscape Hotel",
                "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
                "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                "Category": "Resort and Spa",
                "Tags": ["air conditioning", "bar", "continental breakfast"],
                "ParkingIncluded": "true",
                "LastRenovationDate": "2015-09-20T00:00:00Z",
                "Rating": 4.8,
                "Address": {
                    "StreetAddress": "3393 Peachtree Rd",
                    "City": "Atlanta",
                    "StateProvince": "GA",
                    "PostalCode": "30326"
                }
            },
            {
                "HotelId": "4",
                "HotelName": "Sublime Cliff Hotel",
                "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
                "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
                "Category": "Boutique",
                "Tags": ["concierge", "view", "24-hour front desk service"],
                "ParkingIncluded": true,
                "LastRenovationDate": "1960-02-06T00:00:00Z",
                "Rating": 4.6,
                "Address": {
                    "StreetAddress": "7400 San Pedro Ave",
                    "City": "San Antonio",
                    "StateProvince": "TX",
                    "PostalCode": "78216"
                }
            }
        ]
    }
    
    ```

Maak een bestand **hotels_quickstart_index. json**.  Dit bestand definieert hoe Azure Search werkt met de documenten die u hebt gemaakt in **Hotels. json**. Elk veld wordt geïdentificeerd door een `name` en heeft een opgegeven. `type` Elk veld bevat ook een reeks index kenmerken die aangeven of Azure Search op het veld kunnen zoeken, filteren, sorteren en facetten. De meeste velden zijn eenvoudige gegevens typen, maar sommige `AddressType` vormen zijn complexe typen waarmee u uitgebreide gegevens structuren in uw index kunt maken.  Meer informatie over [ondersteunde gegevens typen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) en [index kenmerken](https://docs.microsoft.com/azure/search/search-what-is-an-index#index-attributes)vindt u hier. 

Voeg het volgende toe aan **hotels_quickstart_index. json** of [down load het bestand](https://github.com/Azure-Samples/azure-search-javascript-samples/quickstart/blob/master/hotels_quickstart_index.json). 

    ```json
    {
        "name": "hotels-quickstart",
        "fields": [
            {
                "name": "HotelId",
                "type": "Edm.String",
                "key": true,
                "filterable": true
            },
            {
                "name": "HotelName",
                "type": "Edm.String",
                "searchable": true,
                "filterable": false,
                "sortable": true,
                "facetable": false
            },
            {
                "name": "Description",
                "type": "Edm.String",
                "searchable": true,
                "filterable": false,
                "sortable": false,
                "facetable": false,
                "analyzer": "en.lucene"
            },
            {
                "name": "Description_fr",
                "type": "Edm.String",
                "searchable": true,
                "filterable": false,
                "sortable": false,
                "facetable": false,
                "analyzer": "fr.lucene"
            },
            {
                "name": "Category",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "sortable": true,
                "facetable": true
            },
            {
                "name": "Tags",
                "type": "Collection(Edm.String)",
                "searchable": true,
                "filterable": true,
                "sortable": false,
                "facetable": true
            },
            {
                "name": "ParkingIncluded",
                "type": "Edm.Boolean",
                "filterable": true,
                "sortable": true,
                "facetable": true
            },
            {
                "name": "LastRenovationDate",
                "type": "Edm.DateTimeOffset",
                "filterable": true,
                "sortable": true,
                "facetable": true
            },
            {
                "name": "Rating",
                "type": "Edm.Double",
                "filterable": true,
                "sortable": true,
                "facetable": true
            },
            {
                "name": "Address",
                "type": "Edm.ComplexType",
                "fields": [
                    {
                        "name": "StreetAddress",
                        "type": "Edm.String",
                        "filterable": false,
                        "sortable": false,
                        "facetable": false,
                        "searchable": true
                    },
                    {
                        "name": "City",
                        "type": "Edm.String",
                        "searchable": true,
                        "filterable": true,
                        "sortable": true,
                        "facetable": true
                    },
                    {
                        "name": "StateProvince",
                        "type": "Edm.String",
                        "searchable": true,
                        "filterable": true,
                        "sortable": true,
                        "facetable": true
                    },
                    {
                        "name": "PostalCode",
                        "type": "Edm.String",
                        "searchable": true,
                        "filterable": true,
                        "sortable": true,
                        "facetable": true
                    },
                    {
                        "name": "Country",
                        "type": "Edm.String",
                        "searchable": true,
                        "filterable": true,
                        "sortable": true,
                        "facetable": true
                    }
                ]
            }
        ],
        "suggesters": [
            {
                "name": "sg",
                "searchMode": "analyzingInfixMatching",
                "sourceFields": [
                    "HotelName"
                ]
            }
        ]
    }
    ```
    
## <a name="2---a-class-for-azure-search"></a>2: een klasse voor Azure Search 

Het is raadzaam om de specifieke gegevens van een bepaald scenario te scheiden van code die breed van toepassing is. De `AzureSearchClient` klasse die in het bestand **AzureSearchClient. js** is gedefinieerd, kent het maken van aanvraag-url's, het indienen van een aanvraag met behulp van de ophaal-API en het reageren op de status code van het antwoord.

Aan de slag met **AzureSearchClient. js** door het **ophalen van het knoop punt** en het maken van een eenvoudige klasse. Isoleer de wijzigbaar delen van de `AzureSearchClient` klasse door aan de constructor de verschillende configuratie waarden door te geven:

    ```javascript
    const fetch = require('node-fetch');
    
    class AzureSearchClient {
      constructor(searchServiceName, adminKey, queryKey, indexName) {
          this.searchServiceName = searchServiceName;
          this.adminKey = adminKey;
          // The query key is used for read-only requests and so can be distributed with less risk of abuse.
          this.queryKey = queryKey;
          this.indexName = indexName;
          this.apiVersion = '2019-05-06';
      }
    
      // All methods go inside class body here!
    }
    
    module.exports = AzureSearchClient;
    ```

De eerste verantwoordelijkheid van de klasse is om te weten hoe u Url's moet maken waarnaar de verschillende aanvragen worden verzonden. Bouw deze Url's met exemplaar methoden die gebruikmaken van de configuratie gegevens die worden door gegeven aan de klasse-constructor. U ziet dat de URL die ze maken specifiek is voor een API-versie en een argument moet hebben dat die versie opgeeft ( `2019-05-06`in deze toepassing). 

Voeg de volgende methoden toe binnen de hoofd tekst van de klasse:

    ```javascript
      getIndexUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}?api-version=${this.apiVersion}`; }
      
      getPostDataUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs/index?api-version=${this.apiVersion}`;  }
    
      getSearchUrl(searchTerm) { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs?api-version=${this.apiVersion}&search=${searchTerm}&searchMode=all`; }
    ```

De volgende verantwoordelijkheid maakt een asynchrone aanvraag met de API voor ophalen. De asynchrone statische methode `request` maakt gebruik van een URL, een teken reeks waarmee de HTTP-methode (' Get ', ' put ', ' post ', ' delete ') wordt opgegeven, de sleutel die moet worden gebruikt in de aanvraag en een optioneel JSON-object. De `headers` variabele wijst de `queryKey` (of de Administrator-of de alleen-lezen-query sleutel) toe aan de HTTP-aanvraag header API-Key. De aanvraag opties bevatten altijd de `method` te gebruiken en de. `headers` Als `bodyJson` dat `null`niet het geval is, wordt de hoofd tekst van de HTTP-aanvraag `bodyJson`ingesteld op de teken reeks representatie van. Het `request` retourneert de belofte van de ophaal-API voor het uitvoeren van de HTTP-aanvraag.

    ```javascript
      static async request(url, method, apiKey, bodyJson = null) {
        // Uncomment the following for request details:
        /*
        console.log(`\n${method} ${url}`);
        console.log(`\n${apiKey}`);
        if (bodyJson !== null) {
            console.log(`\ncontent: ${JSON.stringify(bodyJson, null, 4)}`);
        }
        */
      
        const headers = {
            'content-type' : 'application/json',
            'api-key' : apiKey
        };
        const init = bodyJson === null ?
            { 
                method, 
                headers
            }
            : 
            {
                method, 
                headers,
                body : JSON.stringify(bodyJson)
            };
        return fetch(url, init);
      }
    ```

Voor demo doeleinden gaan we een uitzonde ring genereren als de HTTP-aanvraag niet is voltooid. In een echte toepassing zou u waarschijnlijk enkele logboek registratie en probleem opsporen van de HTTP-status code in `response` de aanvraag van de zoek service. 
    
    ```javascript
      static throwOnHttpError(response) {
        const statusCode = response.status;
        if (statusCode >= 300){
            console.log(`Request failed: ${JSON.stringify(response, null, 4)}`);
            throw new Error(`Failure in request. HTTP Status was ${statusCode}`);
        }
      }
    ```

Voeg ten slotte de methoden toe die met de Azure Search index werken. Deze methoden hebben allemaal dezelfde structuur:

* Haal het eind punt op waaraan de aanvraag wordt gedaan.
* Genereer de aanvraag met het juiste eind punt, HTTP-woord, API-sleutel en de hoofd tekst. `queryAsync()`maakt gebruik van de query sleutel, anders wordt de beheerders sleutel gebruikt.
* `await`het antwoord op de aanvraag.  
* Handel over de status code van het antwoord.
* Een belofte retour neren van een of meer toepasselijke waarden ( `this`een Booleaanse waarde, of de query resultaten). 

    ```javascript
      async indexExistsAsync() { 
          console.log("\n Checking if index exists...");
          const endpoint = this.getIndexUrl();
          const response = await AzureSearchClient.request(endpoint, "GET", this.queryKey);
          // Success has a few likely status codes: 200 or 204 (No Content), but accept all in 200 range...
          const exists = response.status >= 200 && response.status < 300;
          return exists;
      }
      
      async deleteIndexAsync() {
          console.log("\n Deleting existing index...");
          const endpoint = this.getIndexUrl();
          const response = await AzureSearchClient.request(endpoint, "DELETE", this.adminKey);
          AzureSearchClient.throwOnHttpError(response);
          return this;
      }
      
      async createIndexAsync(definition) {
          console.log("\n Creating index...");
          const endpoint = this.getIndexUrl();
          const response = await AzureSearchClient.request(endpoint, "PUT", this.adminKey, definition);
          AzureSearchClient.throwOnHttpError(response);
          return this;
      }
      
      async postDataAsync(hotelsData) {
          console.log("\n Adding hotel data...");
          const endpoint = this.getPostDataUrl();
          const response = await AzureSearchClient.request(endpoint,"POST", this.adminKey, hotelsData);
          AzureSearchClient.throwOnHttpError(response);
          return this;
      }
      
      async queryAsync(searchTerm) {
          console.log("\n Querying...")
          const endpoint = this.getSearchUrl(searchTerm);
          const response = await AzureSearchClient.request(endpoint, "GET", this.queryKey);
          AzureSearchClient.throwOnHttpError(response);
          return response;
      }
    ```

Controleer of de methoden zich in de klasse bevinden en of u de klasse exporteert. Het ultraperifere bereik van **AzureSearchClient. js** moet zijn:

    ```javascript
    const fetch = require('node-fetch');
    
    class AzureSearchClient {
        // ... code here ...
    }
    
    module.exports = AzureSearchClient;
    ```

## <a name="3---create-a-program"></a>3: een programma maken

Een object gerichte klasse is een goede keuze voor de mogelijk herbruikbare **AzureSearchClient. js** -module, maar is niet nodig voor het hoofd programma, die we in een bestand met de naam **index. js**plaatsen. 

Maak **index. js** en begin met het volgende:

* Het **nconf** -pakket, waarmee u de configuratie met JSON, omgevings variabelen of opdracht regel argumenten kunt opgeven.
* De gegevens uit het bestand **Hotels. json** .
* De gegevens uit het bestand **hotels_quickstart_index. json** .
* De module `AzureSearchClient`.

    ```javascript
    const nconf = require('nconf');
    
    const hotelData = require('./hotels.json');
    const indexDefinition = require('./hotels_quickstart_index.json');
    const AzureSearchClient = require('./AzureSearchClient.js');
    ```

Voeg nu enkele eenvoudige query's toe: 

    ```javascript
    const queries = [
      "*&$count=true",
      "historic&$filter=Rating gt 4&"
    ];
    ```

De eerste query retourneert alle gegevens (`*`) en een telling van het aantal geretourneerde records. De tweede retourneert alleen de documenten die het woord "historisch" bevatten in een van de velden die zijn gedefinieerd als ' Doorzoek bare ' in **hotels_quickstart_index. json** en `Rating` waarvan het veld een waarde bevat die groter is dan 4. Meer informatie over [het opstellen van een query in azure Search](https://docs.microsoft.com/azure/search/search-query-overview). 

Met het [ **nconf** -pakket](https://github.com/indexzero/nconf) kunt u configuratie gegevens opgeven in verschillende indelingen, zoals omgevings variabelen of de opdracht regel. We gaan **nconf** op een eenvoudige manier gebruiken om het bestand **azure_search_config. json** te lezen en de inhoud van het bestand als een woorden boek te retour neren. Metde `get(key)` functie nconf kunt u snel controleren of de stap [' configuratie van Azure Search-service-informatie '](#configure) is overgeslagen. Ten slotte retour neren we de configuratie:

    ```javascript
    function getAzureConfiguration() {
      const config = nconf.file({ file: 'azure_search_config.json' });
      if (config.get('serviceName') === '[SEARCH_SERVICE_NAME' ) {
        throw new Error("You have not set the values in your azure_search_config.json file. Change them to match your search service's values.");
      }
      return config;
    }
    ```

De `sleep` functie maakt een `Promise` die wordt omgezet na een opgegeven tijds duur. Met deze functie kan de app worden onderbroken tijdens het wachten tot asynchrone index bewerkingen zijn voltooid en beschikbaar worden. Het toevoegen van een dergelijke vertraging is doorgaans alleen nodig in demo's, testen en voorbeeld toepassingen.

    ```javascript
    function sleep(ms)
    {
      return(
          new Promise(function(resolve, reject)
          {
              setTimeout(function() { resolve(); }, ms);
          })
      );
    }
    ```

De `doQueries()` functie neemt een `AzureSearchClient` object en past de `AzureSearchClient.queryAsync` -methode toe op elk van de waarden `queries` in de matrix. De `Promise.all()` functie wordt gebruikt om een enkele `Promise` waarde te retour neren die alleen wordt omgezet wanneer alle query's zijn opgelost. De aanroep voor `JSON.stringify(body, null, 4)` het opmaken van het query resultaat zodat deze beter leesbaar is.

    ```javascript
    async function doQueriesAsync(client) {
      return Promise.all(
          queries.map( async query => {
              const result = await client.queryAsync(query);
              const body = await result.json();
              const str = JSON.stringify( body, null, 4);
              console.log(`Query: ${query} \n ${str}`);
          })
      );
    }
    ```

Geef ten slotte de asynchrone `run` functie op en roep deze aan. Deze functie roept de andere functies op in volg orde, in afwachting `Promise`van het oplossen van s.

* De configuratie ophalen met de `getAzureConfiguration()` eerder geschreven
* Een nieuw `AzureSearchClient` exemplaar maken en waarden door geven vanuit uw configuratie
* Controleer of de index bestaat en verwijder deze
* Een index maken met behulp van de `indexDefinition` geladen van **hotels_quickstart_index. json**
* Voeg de documenten over de hotels toe die u hebt geladen vanuit **Hotels. json**
* Query's uitvoeren op de Azure search index `doQueriesAsync()` met de methode die u hebt geschreven

    ```javascript
    const run = async () => {
      try {
        const cfg = getAzureConfiguration();
        const client = new AzureSearchClient(cfg.get("serviceName"), cfg.get("adminKey"), cfg.get("queryKey"), cfg.get["serviceName"]);
        
        const exists = await client.indexExistsAsync();
        await exists ? client.deleteIndexAsync() : Promise.resolve();
        // Deleting index can take a few seconds
        await sleep(2000);
        const indexDefinition = require('./hotels_quickstart_index.json');
        await client.createIndexAsync(indexDefinition);
        // Index availability can take a few seconds
        await sleep(2000);
        await client.postDataAsync(hotelData);
        // Data availability can take a few seconds
        await sleep(5000);
        await doQueries(client);
      } catch (x) {
        console.log(x);
      }
    }
    
    run();
    ```

Vergeet niet dat de laatste aanroep `run()`naar! Het is het begin punt voor uw programma wanneer u in `node index.js` de volgende stap uitvoert.

### <a name="prepare-and-run-the-sample"></a>Het voor beeld voorbereiden en uitvoeren

Gebruik een Terminal venster voor de volgende opdrachten.

1. Ga naar de map die het bestand **package. json** bevat en de rest van de code.
1. Installeer de pakketten voor het voor beeld `npm install`met.  Met deze opdracht worden de pakketten gedownload waarvan de code afhankelijk is.
1. Voer uw programma uit `node index.js`met.

U ziet een reeks berichten die de acties beschrijven die door het programma worden uitgevoerd, met de resultaten van sommige query's. Als u meer details van de aanvragen wilt zien, kunt u de opmerkings regels [20-26](https://github.com/Azure-Samples/azure-search-javascript-samples/quickstart/blob/master/AzureSearchClient.js#LL20-LL26) van **AzureSearchClient. js**onzichtbaar maakt. 

### <a name="about-the-sample"></a>Over het voor beeld

In het voor beeld wordt gebruikgemaakt van een klein aantal Hotel gegevens, voldoende om de basis beginselen van het maken en doorzoeken van een Azure Search index te demonstreren.

De **AzureSearchClient** -klasse kapselt de configuratie, url's en Basic HTTP-aanvragen voor de zoek service in. Het bestand **index. js** laadt de configuratie gegevens voor de Azure Search-service, de Hotel gegevens die worden geüpload voor indexering en, in de `run` functie, orders en voert de verschillende bewerkingen uit.

Het algehele gedrag van de `run` functie is het verwijderen van de Azure search index als deze bestaat, de index te maken, gegevens toe te voegen en enkele query's uit te voeren.  

## <a name="clean-up"></a>Opruimen 

Wanneer u in uw eigen abonnement werkt, is het een goed idee aan het einde van een project om te bepalen of u nog steeds de resources nodig hebt die u hebt gemaakt. Resources die actief zijn, kunnen kosten in rekening worden. U kunt resources afzonderlijk verwijderen of de resource groep verwijderen om de volledige set resources te verwijderen.

U kunt resources vinden en beheren in de portal met behulp van de koppeling **alle resources** of **resource groepen** in het navigatie deel venster aan de linkerkant.
Als u een gratis service gebruikt, moet u er rekening mee houden dat u bent beperkt tot drie indexen, Indexeer functies en gegevens bronnen. U kunt afzonderlijke items in de Portal verwijderen om de limiet te blijven. 

## <a name="next-steps"></a>Volgende stappen

In deze Snelstartgids voor node. js hebt u een reeks taken uitgevoerd voor het maken van een index, het laden van documenten en het uitvoeren van query's. We hebben bepaalde stappen uitgevoerd, zoals het lezen van de configuratie en het definiëren van de query's, op de eenvoudigste manier. In een echte toepassing wilt u deze problemen in afzonderlijke modules plaatsen die flexibiliteit en inkapseling zouden bieden. 
 
Als u al enige ervaring met Azure Search hebt, kunt u dit voorbeeld gebruiken als springplank om een suggestiefunctie (type-ahead of automatisch aangevulde query's), filters en facetnavigatie uit te proberen. Als u niet bekend bent met Azure Search, raden we u aan andere zelf studies te proberen om een goed beeld te ontwikkelen van wat u kunt maken. Bezoek de [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) voor meer resources. 

> [!div class="nextstepaction"]
> [Azure Search aanroepen vanaf een webpagina met behulp van Java script](https://github.com/liamca/azure-search-javascript-samples)
