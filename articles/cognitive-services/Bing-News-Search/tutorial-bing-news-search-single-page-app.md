---
title: Bing één pagina Nieuws zoeken app | Microsoft Docs
description: Legt uit hoe u de API van Bing nieuws zoeken gebruiken in een webtoepassing van één pagina.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 10/30/2017
ms.author: v-gedod
ms.openlocfilehash: fb8cd24dfdfb03500cc86ee1b1f0126ec044a873
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345582"
---
# <a name="tutorial-single-page-news-search-app"></a>Zelfstudie: Single-page-nieuws zoeken app
De Bing nieuws Search API kunt u zoeken op het Web en resultaten van het type nieuws relevant zijn voor een zoekopdracht krijgen. In deze zelfstudie maken we een single-page-webtoepassing die gebruikmaakt van de API van Bing nieuws Search zoekresultaten weergegeven op de pagina. De toepassing bevat onderdelen die HTML, CSS en JavaScript.

<!-- Remove until we can replace it with sanitized copy
![Single-page Bing News Search app](media/news-search-singlepage.png)
-->

> [!NOTE]
> De JSON- en HTTP-koppen onder aan de pagina aanklikken ziet u de JSON-antwoord en gegevens voor het HTTP-aanvragen. Deze informatie kunnen nuttig zijn wanneer de service te verkennen.

De app zelfstudie ziet u hoe:
> [!div class="checklist"]
> * Een Bing nieuws zoeken-API-aanroep in JavaScript uitvoeren
> * Zoekopties doorgeven aan de API van Bing nieuws zoeken
> * Zoekresultaten nieuws uit vier categorieën weergeven: een willekeurig type, business, status of politiek van tijdschema van 24 uur, de afgelopen week, maand of alle beschikbare tijd
> * Pagina met zoekresultaten
> * Ingang Bing-ID en API-abonnementssleutel van de client
> * Afhandelen van fouten die optreden

De zelfstudie pagina is volledig onafhankelijke; gebruikt niet alle externe frameworks, opmaakmodellen of afbeeldingsbestanden. Alleen functies voor sterk ondersteund JavaScript-taal wordt gebruikt en werkt met huidige versies van alle bekende webbrowsers.

In deze zelfstudie besproken geselecteerde gedeelten van de broncode. De volledige [broncode](tutorial-bing-news-search-single-page-app-source.md) is beschikbaar. De als voorbeeld wilt uitvoeren, kopieer en plak de broncode in een teksteditor en sla deze als `bing.html`.

## <a name="app-components"></a>App-onderdelen
Net als alle Web-Apps van één pagina is deze zelfstudie toepassing bestaat uit drie delen:

> [!div class="checklist"]
> * HTML - definieert de structuur en inhoud van de pagina
> * CSS - bepaalt de vormgeving van de pagina
> * JavaScript - definieert het gedrag van de pagina

De meeste HTML en CSS is conventionele, zodat de zelfstudie worden niet behandeld. De HTML-code bevat het zoekformulier waarin de gebruiker voert een query en zoekopties kiest. Het formulier is verbonden met JavaScript die daadwerkelijk wordt uitgevoerd de zoekactie met de `onsubmit` kenmerk van de `<form>` tag:

```html
<form name="bing" onsubmit="return newBingNewsSearch(this)">
```
De `onsubmit` handler retourneert `false`, waarmee het formulier worden verzonden naar een server houdt. De JavaScript-code komt het werk van de benodigde informatie verzamelen van het formulier en het uitvoeren van de zoekopdracht.

De HTML-code bevat ook de afdelingen (HTML `<div>` labels) waar de zoekresultaten worden weergegeven.

## <a name="managing-subscription-key"></a>Abonnementssleutel beheren

Om te voorkomen dat de sleutel Bing zoeken-API-abonnement in de code opnemen, gebruiken we de permanente opslag van de browser voor het opslaan van de sleutel. Voordat u de sleutel wordt opgeslagen, vragen wij voor de sleutel van de gebruiker. Als de sleutel wordt later geweigerd door de API, ongeldig we opgeslagen zodat de gebruiker opnieuw wordt gevraagd.

Definiëren we `storeValue` en `retrieveValue` functies die gebruikmaken van een de `localStorage` object (niet alle browsers ondersteunen deze) of een cookie. De `getSubscriptionKey()` functie maakt gebruik van deze functies op te slaan en het ophalen van de sleutel van de gebruiker.

``` javascript
// Cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

// Bing Search API endpoint
BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/news";

// ... omitted definitions of storeValue() and retrieveValue()
// Browsers differ in their support for persistent storage by 
// local HTML files. See the source code for browser-specific
// options.

// Get stored API subscription key, or 
// prompt if it's not found.
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```
De HTML-code `<form>` tag `onsubmit` aanroepen de `bingWebSearch` functie voor het retourneren van resultaten van de zoekmap. `bingWebSearch` maakt gebruik van `getSubscriptionKey()` om te verifiëren van elke query. Zoals wordt weergegeven in de vorige definitie `getSubscriptionKey` vraagt de gebruiker om de sleutel als de sleutel nog niet is opgegeven. De sleutel wordt vervolgens opgeslagen voor voortgezet gebruik door de toepassing.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```
## <a name="selecting-search-options"></a>Zoekopties selecteren
De volgende afbeelding ziet u de querytekstvak en de opties die een zoekopdracht voor nieuws over school middelen definiëren.

![Bing nieuws zoekopties](media/news-search-categories.png)

Het HTML-formulier bevat elementen met de volgende namen:

|Element|Beschrijving|
|-|-|
| `where` | Een vervolgkeuzelijst voor het selecteren van de markt (locatie en taal) dat wordt gebruikt voor de zoekopdracht. |
| `query` | Het tekstveld zoektermen invoeren. |
| `category` | De selectievakjes om bepaalde soorten resultaten te bevorderen. De volgorde van de gezondheid van nieuws Health promoveren, bijvoorbeeld, worden verhoogd. |
| `when` | De vervolgkeuzelijst voor het beperken van eventueel om te zoeken naar de meest recente dag, week of maand. |
| `safe` | Een selectievakje waarmee wordt aangegeven of de functie Bing veilig zoeken gebruiken voor het filteren van resultaten 'volwassenen'. |
| `count` | Verborgen veld. Het aantal zoekresultaten worden geretourneerd bij elke aanvraag. Wijzigen om minder of meer resultaten per pagina weer te geven. |
| `offset`|  Verborgen veld. De verschuiving van de eerste zoekresultaten in de aanvraag; gebruikt voor het wisselbestand. Dit wordt opnieuw ingesteld op `0` op een nieuwe aanvraag. |

> [!NOTE]
> Bing zoeken op het Web biedt andere queryparameters. We maken gebruik van slechts enkele van deze.

``` javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);

    for (var i = 0; i < form.category.length; i++) {
        if (form.category[i].checked) {
            category = form.category[i].value;
            break;
        }
    }
    if (category.valueOf() != "all".valueOf()) { 
        options.push("category=" + category); 
        }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Bijvoorbeeld, de `SafeSearch` parameter in een werkelijke API-aanroep is `strict`, `moderate`, of `off`, met `moderate` wordt de standaardwaarde. Onze formulier, gebruikt echter een selectievakje dat slechts twee statussen heeft. De JavaScript-code converteert u deze instelling op `strict` of `off` (`moderate` wordt niet gebruikt).

## <a name="performing-the-request"></a>Uitvoeren van de aanvraag
De query, de opties-tekenreeks en de API-sleutel opgegeven de `BingNewsSearch` functie maakt gebruik van een `XMLHttpRequest` object, zodat de aanvraag naar het eindpunt Bing nieuws zoeken.

```javascript
// perform a search given query, options string, and API key
function bingNewsSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
     if (category.valueOf() != "all".valueOf()) {
        var queryurl = BING_ENDPOINT + "/search?" + "?q=" + encodeURIComponent(query) + "&" + options;
    }
    else
    {
        if (query){
        var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;
        }
        else {
            var queryurl = BING_ENDPOINT + "?" + options;
        }
    }

    // open the request
    try {
        request.open("GET", queryurl);
    } 
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // add request headers
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");
    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);

    // event handler for successful response
    request.addEventListener("load", handleBingResponse);

    // event handler for erorrs
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // event handler for aborted request
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // send the request
    request.send();
    return false;
}
```
De HTTP-aanvraag is voltooid, JavaScript-aanroepen de `load` gebeurtenis-handler de `handleBingResponse()` functie voor het afhandelen van een geslaagde HTTP GET-aanvraag naar de API. 

```javascript
// handle Bing search request results
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
    }

    // show raw JSON and HTTP request
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "News") {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    }

    // Any other HTTP response is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSubscriptionKey();

        // some error responses don't have a top-level errors object, so gin one up
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // display HTTP status code
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // add all fields from all error responses
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // also display Bing Trace ID if it isn't blocked by CORS
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // and display the error message
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Een geslaagde HTTP-aanvraag komt *niet* noodzakelijkerwijs dat de zoekopdracht zelf is voltooid. Als er een fout optreedt tijdens de zoekbewerking wordt de Bing nieuws zoeken-API retourneert een statuscode 200 HTTP en bevat informatie over de fout in de JSON-antwoord. Als de aanvraag snelheid beperkt is, retourneert de API bovendien een leeg antwoord.

Veel van de code in beide van de voorgaande functies is speciaal bedoeld voor foutafhandeling. Er kunnen fouten optreden in de volgende fasen:

|Fase|Potentiële fouten|Verwerkt door|
|-|-|-|
|De JavaScript-request-object maken|De URL is ongeldig|`try`/`catch` blokkeren|
|Maken van de aanvraag|Netwerkfouten, afgebroken verbindingen|`error` en `abort` gebeurtenis-handlers|
|De zoekactie|Ongeldige aanvraag, ongeldige JSON-frequentielimieten|webtests op `load` gebeurtenis-handler|

Fouten worden verwerkt door het aanroepen van `renderErrorMessage()` met details over de fout bekend. Als het antwoord voldoet aan de volledige gauntlet fout tests, noemen we `renderSearchResults()` om de zoekresultaten op de pagina weer te geven.

## <a name="displaying-search-results"></a>Zoekresultaten weergeven
De belangrijkste functie voor het weergeven van de zoekresultaten is `renderSearchResults()`. Deze functie heeft de JSON die is geretourneerd door de zoekservice Bing nieuws en worden de nieuwsresultaten en de verwante zoekopdrachten, indien van toepassing.

```javascript
// render the search results given the parsed JSON response
    function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);

    showDiv("results", renderResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```
De belangrijkste zoekresultaten worden geretourneerd als het hoogste niveau `value` object in het JSON-antwoord. Ze geven we onze functie `renderResults()`, waardoor ze doorlopen en roept een afzonderlijke functie voor elk item weergeven in HTML. De HTML-code wordt geretourneerd naar `renderSearchResults()`, waar deze wordt ingevoegd in de `results` deling op de pagina.

```javascript
function renderResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.news(items[i], i, len));
    }
    return html.join("\n\n");
}
```
De Bing nieuws zoeken-API retourneert maximaal vier verschillende soorten gerelateerde resultaten in een eigen object op het hoogste niveau. Dit zijn:

|De relatie|Beschrijving|
|-|-|
|`pivotSuggestions`|Query's die een woord pivot in oorspronkelijke zoekopdracht door een andere naam vervangen. Bijvoorbeeld, als u zoekt naar 'rode bloemen', een woord pivot mogelijk "red" en een suggestie pivot mogelijk "gele bloemen."|
|`queryExpansions`|Query's die de oorspronkelijke zoekopdracht te verfijnen door meer voorwaarden toe te voegen. Bijvoorbeeld, als u zoekt naar 'Microsoft Surface ', een query-uitbreiding mogelijk "Microsoft Surface Pro."|
|`relatedSearches`|Query's die ook door andere gebruikers die de oorspronkelijke zoekopdracht ingevoerd zijn ingevoerd. Bijvoorbeeld, als u zoekt naar 'Mount Rainier', een gerelateerde zoekopdracht mogelijk 'onderwerp Mt. Saint Helens."|
|`similarTerms`|Query's met dezelfde betekenis voor de oorspronkelijke zoekopdracht. Bijvoorbeeld, als u zoekt naar 'scholen', een vergelijkbare term mogelijk "education."|

Zoals eerder is gezien in `renderSearchResults()`, we alleen weergeven, de `relatedItems` suggesties en plaats de resulterende koppelingen in de zijbalk van de pagina.

## <a name="rendering-result-items"></a>Rendering resultaat items

Het object in de JavaScript-code `searchItemRenderers`, bevat *renderers:* zoekresultaat functies die HTML gegenereerd voor elk type.

```javascript
searchItemRenderers = {
    news: function(item) { ... },
    webPages: function (item) { ... }, 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```
Een renderer-functie kan de volgende parameters accepteert:

|Parameter|Beschrijving|
|-|-|
|`item`| De JavaScript-object met de eigenschappen van het item, zoals de URL en de beschrijving.|
|`index`| De index van het resultaatitem binnen de verzameling.|
|`count`| Het aantal items in de verzameling van de zoekopdracht resultaat item.|

De `index` en `count` parameters kunnen worden gebruikt om te aantal resultaten voor het genereren van speciale HTML voor het begin of einde van een verzameling regeleinden invoegen na een bepaald aantal items, enzovoort. Als een renderer niet nodig heeft voor deze functionaliteit, hoeft deze niet te accepteren van deze twee parameters.

De `news` renderer wordt weergegeven in het volgende javascript-fragment:
```javascript
    // render news story
    news: function (item) {
        var html = [];
        html.push("<p class='news'>");
        if (item.image) {
            width = 60;
            height = Math.round(width * item.image.thumbnail.height / item.image.thumbnail.width);
            html.push("<img src='" + item.image.thumbnail.contentUrl +
                "&h=" + height + "&w=" + width + "' width=" + width + " height=" + height + ">");
        }
        html.push("<a href='" + item.url + "'>" + item.name + "</a>");
        if (item.category) html.push(" - " + item.category);
        if (item.contractualRules) {    // MUST display source attributions
            html.push(" (");
            var rules = [];
            for (var i = 0; i < item.contractualRules.length; i++)
                rules.push(item.contractualRules[i].text);
                html.push(rules.join(", "));
                html.push(")");
            }
        html.push(" (" + getHost(item.url) + ")");
        html.push("<br>" + item.description);
        return html.join("");
    },
```
De functie van nieuws-renderer:
> [!div class="checklist"]
> * Maakt een alineatag en wijst deze naar de `news` klasse en het verstuurd naar de html-matrix.
> * Berekent de miniatuur afbeeldingsgrootte (breedte wordt vastgesteld op 60 pixels hoogte berekend naar verhouding).
> * De HTML-code builds `<img>` tag om de miniatuur weer te geven. 
> * De HTML-code builds `<a>` labels die een koppeling naar de installatiekopie en de pagina waarvan het deel uitmaakt.
> * De beschrijving die geeft informatie weer over de installatiekopie en de site is gebaseerd.

De miniatuurgrootte wordt gebruikt in zowel de `<img>` code en de `h` en `w` velden in de miniatuur-URL. De [miniaturen Bing-service](resize-and-crop-thumbnails.md) voorziet in een miniatuur van precies die grootte.

## <a name="persisting-client-id"></a>Persisting client-ID
Antwoorden van de Bing zoeken-API's, omvat mogelijk een `X-MSEdge-ClientID` header die moet worden verzonden naar de API met opeenvolgende aanvragen. Als er meerdere Bing zoeken-API's worden gebruikt, moet dezelfde client-ID worden gebruikt met deze, indien mogelijk.

Geven de `X-MSEdge-ClientID` header kan de Bing-API's om te koppelen van een gebruiker zoekopdrachten met twee belangrijke voordelen.

Ten eerste kunt u de Bing zoekmachine voorbij context toepassen op zoekopdrachten resultaten oplevert die beter voldoen aan de gebruiker. Als een gebruiker eerder naar voorwaarden die betrekking hebben op afvaart gezocht is, bijvoorbeeld een hoger zoek naar 'knooppunten' mogelijk bij voorkeur informatie ophalen over knooppunten in afvaart gebruikt.

Ten tweede kunt Bing willekeurig selecteren voor gebruikers gebruikmaken van nieuwe functies voordat ze algemeen beschikbaar worden gemaakt. Dezelfde client-ID die aan elke aanvraag zorgt ervoor dat gebruikers die de functie altijd zien deze weergegeven. Zonder de client-ID ziet de gebruiker mogelijk een functie worden weergegeven en verdwijnt schijnbaar willekeurig in de zoekresultaten.

Browser-beveiligingsbeleid (CORS) mogelijk niet de `X-MSEdge-ClientID` header beschikbaar is voor JavaScript. Deze beperking treedt op wanneer het antwoord van de zoekactie heeft een andere oorsprong van de pagina die wordt aangevraagd. In een productieomgeving, moet u dit beleid door het hosten van een script op server die de API-aanroep in hetzelfde domein als de webpagina wordt adresseren. Aangezien het script dezelfde oorsprong als de webpagina heeft de `X-MSEdge-ClientID` header is beschikbaar voor JavaScript.

> [!NOTE]
> U moet de aanvraag-serverzijde uitvoeren in een productie-webtoepassing. Uw Bing zoeken-API-sleutel moet anders zijn opgenomen in de webpagina waar het voor iedereen die de bron-weergaven beschikbaar is. U wordt gefactureerd voor het gebruik van alle onder uw API-abonnement-sleutel, zelfs aanvragen door onbevoegden, dus is het belangrijk dat u niet uw sleutel beschikbaar.

Voor ontwikkelingsdoeleinden, kunt u de zoekopdracht Bing-Web-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers` header die whitelists antwoordheaders en deze beschikbaar voor JavaScript.

Het is gemakkelijk voor het installeren van een proxy CORS zodat onze zelfstudie app toegang tot de client een koptekst-ID. Eerste, als u dit nog niet hebt [Installeer Node.js](https://nodejs.org/en/download/). Vervolgens voert u de volgende opdracht in een opdrachtvenster:

    npm install -g cors-proxy-server

Wijzig vervolgens het eindpunt naar Bing in het HTML-bestand in:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Ten slotte de CORS-proxy starten met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken u sluit het venster stopt de proxy. In de uitbreidbare HTTP-Headers gedeelte onder de zoekresultaten, nu ziet u de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Bing nieuws Search API-verwijzingen](//docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference)