---
title: Bing zoeken op het Web één pagina Web-app | Microsoft Docs
description: Laat zien hoe de Bing Web zoeken-API gebruiken in een webtoepassing van één pagina.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: f22e38a1d6ee4042684b9822b58669bed6fe29a0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345481"
---
# <a name="tutorial-single-page-web-app"></a>Zelfstudie: Single-page-Web-app

De Bing Web Search API kunt u zoeken op het Web en resultaten van de verschillende typen die relevant zijn voor een zoekopdracht krijgen. In deze zelfstudie maken we een single-page-webtoepassing die gebruikmaakt van de Web zoeken-API van Bing om zoekresultaten weer te rechts op de pagina. De toepassing bevat onderdelen die HTML, CSS en JavaScript.

<!-- Remove until this can be replaced with a sanitized version.
![[Single-page Bing Web Search app]](media/cognitive-services-bing-web-api/web-search-spa-demo.png)
-->

> [!NOTE]
> De JSON- en HTTP-koppen aan de onderkant van de pagina onthullen de JSON-antwoord en HTTP-aanvraag informatie wanneer geklikt. Deze gegevens zijn nuttig wanneer de service te verkennen.

De app zelfstudie ziet u hoe:

> [!div class="checklist"]
> * Een zoekopdracht Bing-Web-API-aanroep in JavaScript uitvoeren
> * Zoekopties doorgeven aan de Web zoeken-API van Bing
> * Web, nieuws, afbeeldingen en video zoekresultaten weergeven
> * Pagina met zoekresultaten
> * Ingang Bing-ID en API-abonnementssleutel van de client
> * Afhandelen van fouten die optreden

De zelfstudie pagina is volledig onafhankelijke; gebruikt niet alle externe frameworks, opmaakmodellen of zelfs afbeeldingsbestanden. Alleen functies voor sterk ondersteund JavaScript-taal wordt gebruikt en werkt met huidige versies van alle bekende webbrowsers.

In deze zelfstudie bespreken we alleen geselecteerde gedeelten van de broncode. De volledige broncode is beschikbaar [op een afzonderlijke pagina](tutorial-bing-web-search-single-page-app-source.md). Kopieer en plak deze code in een teksteditor en sla het bestand als `bing.html`.

## <a name="app-components"></a>App-onderdelen

Net als alle Web-Apps van één pagina is de zelfstudie toepassing bestaat uit drie delen:

> [!div class="checklist"]
> * HTML - definieert de structuur en inhoud van de pagina
> * CSS - bepaalt de vormgeving van de pagina
> * JavaScript - definieert het gedrag van de pagina

Deze zelfstudie dekt niet in de meeste van de HTML-indeling of CSS in detail zoals ze simpel zijn.

De HTML-code bevat het zoekformulier waarin de gebruiker voert een query en zoekopties kiest. Het formulier is verbonden met de JavaScript die daadwerkelijk wordt uitgevoerd de zoekopdracht op de `<form>` van de tag `onsubmit` kenmerk:

```html
<form name="bing" onsubmit="return newBingWebSearch(this)">
```

De `onsubmit` handler retourneert `false`, waarmee het formulier worden verzonden naar een server houdt. De JavaScript-code komt daadwerkelijk het werk van de benodigde informatie verzamelen van het formulier en het uitvoeren van de zoekopdracht.

De HTML-code bevat ook de afdelingen (HTML `<div>` labels) waar de zoekresultaten worden weergegeven.

## <a name="managing-subscription-key"></a>Abonnementssleutel beheren

Om te voorkomen dat de sleutel Bing zoeken-API-abonnement in de code opnemen, gebruiken we de permanente opslag van de browser voor het opslaan van de sleutel. Als er geen sleutel is opgeslagen, wordt gevraagd om de sleutel van de gebruiker en opgeslagen voor later gebruik. Als de sleutel wordt later geweigerd door de API, ongeldig we opgeslagen zodat de gebruiker opnieuw wordt gevraagd.

Definiëren we `storeValue` en `retrieveValue` functies die gebruikmaken van een de `localStorage` object (als de browser het ondersteunt) of een cookie. Onze `getSubscriptionKey()` functie maakt gebruik van deze functies op te slaan en het ophalen van de sleutel van de gebruiker.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
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

De HTML-code `form` tag `onsubmit` aanroepen de `bingWebSearch` functie voor het retourneren van resultaten van de zoekmap. `bingWebSearch` maakt gebruik van `getSubscriptionKey` om te verifiëren van elke query. Zoals wordt weergegeven in de vorige definitie `getSubscriptionKey` vraagt de gebruiker om de sleutel als de sleutel nog niet is opgegeven. De sleutel wordt vervolgens opgeslagen voor voortgezet gebruik door de toepassing.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Zoekopties selecteren

![[Formulier naar Bing]](media/cognitive-services-bing-web-api/web-search-spa-form.png)

Het HTML-formulier bevat elementen met de volgende namen:

| | |
|-|-|
| `where` | Een vervolgkeuzelijst voor het selecteren van de markt (locatie en taal) dat wordt gebruikt voor de zoekopdracht. |
| `query` | Het tekstveld in te voeren van de zoektermen. |
| `what` | De selectievakjes om bepaalde soorten resultaten te bevorderen. Promoveren installatiekopieën, bijvoorbeeld, verhoogd de volgorde van afbeeldingen. |
| `when` | De vervolgkeuzelijst voor het beperken van eventueel om te zoeken naar de meest recente dag, week of maand. |
| `safe` | Een selectievakje waarmee wordt aangegeven of de Bing veilig zoeken functie gebruiken om te filteren 'volwassenen' resultaten. |
| `count` | Verborgen veld. Het aantal zoekresultaten worden geretourneerd bij elke aanvraag. Wijzigen om minder of meer resultaten per pagina weer te geven. |
| `offset` | Verborgen veld. De verschuiving van de eerste zoekresultaten in de aanvraag; gebruikt voor het wisselbestand. Dit wordt opnieuw ingesteld op `0` op een nieuwe aanvraag. |

> [!NOTE]
> Bing zoeken op het Web biedt veel meer queryparameters. We maken gebruik van slechts enkele van deze hier.

De JavaScript-functie `bingSearchOptions()` converteert u deze velden naar de indeling van de Bing zoeken-API.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++) 
        if (form.what[i].checked) what.push(form.what[i].value);
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    options.push("textDecorations=true");
    options.push("textFormat=HTML");
    return options.join("&");
}
```

Bijvoorbeeld, de `SafeSearch` parameter in een werkelijke API-aanroep is `strict`, `moderate`, of `off`, met `moderate` wordt de standaardwaarde. Onze formulier, gebruikt echter een selectievakje dat slechts twee statussen heeft. De JavaScript-code converteert u deze instelling op `strict` of `off` (`moderate` wordt niet gebruikt).

Als een van de **opwaarderen** selectievakjes worden gemarkeerd, we ook toevoegen een `answerCount` parameter aan de query. `answerCount` is vereist wanneer de `promote` parameter. We gewoon ingesteld op `9` (het aantal resultaattypen ondersteund door de Bing Web Search API) om ervoor te zorgen dat het maximum aantal resultaattypen wordt gehaald.

> [!NOTE]
> Promoveren van een resultaattype heeft geen *garanderen* dat de zoekresultaten dat soort resultaat bevatten. In plaats daarvan verhoogt promotie de volgorde van de soorten resultaten ten opzichte van hun normale positie. Gebruiken om te zoeken naar bepaalde soorten resultaten beperken, de `responseFilter` queryparameter, of een meer specifiek eindpunt zoals Bing installatiekopie of Bing nieuws zoekopdracht aanroepen.

We ook verzenden `textDecoration` en `textFormat` queryparameters leiden dat de zoekterm om te worden vet in de zoekresultaten. Deze waarden zijn vastgelegd in het script.

## <a name="performing-the-request"></a>Uitvoeren van de aanvraag

De query, de opties-tekenreeks en de API-sleutel opgegeven de `BingWebSearch` functie maakt gebruik van een `XMLHttpRequest` object, zodat de aanvraag naar het eindpunt naar Bing.

```javascript
// perform a search given query, options string, and API key
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

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

De HTTP-aanvraag is voltooid, JavaScript-aanroepen onze `load` gebeurtenis-handler de `handleBingResponse()` functie voor het afhandelen van een geslaagde HTTP GET-aanvraag naar de API. 

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
        return;
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
            if (jsobj._type === "SearchResponse" && "rankingResponse" in jsobj) {
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
> Een geslaagde HTTP-aanvraag komt *niet* noodzakelijkerwijs dat de zoekopdracht zelf is voltooid. Als er een fout optreedt tijdens de zoekbewerking wordt de Bing Web zoeken-API retourneert een statuscode 200 HTTP en bevat informatie over de fout in de JSON-antwoord. Als de aanvraag snelheid beperkt is, retourneert de API bovendien een leeg antwoord.

Veel van de code in beide van de voorgaande functies is speciaal bedoeld voor foutafhandeling. Er kunnen fouten optreden in de volgende fasen:

|Fase|Potentiële fouten|Verwerkt door|
|-|-|-|
|Gebouw JavaScript request-object|De URL is ongeldig|`try`/`catch` blokkeren|
|Maken van de aanvraag|Netwerkfouten, afgebroken verbindingen|`error` en `abort` gebeurtenis-handlers|
|De zoekactie|Ongeldige aanvraag, ongeldige JSON-frequentielimieten|webtests op `load` gebeurtenis-handler|

Fouten worden verwerkt door het aanroepen van `renderErrorMessage()` met details over de fout bekend. Als het antwoord voldoet aan de volledige gauntlet fout tests, noemen we `renderSearchResults()` om de zoekresultaten op de pagina weer te geven.

## <a name="displaying-search-results"></a>Zoekresultaten weergeven

De Bing Web Search API [, moet u voor het weergeven van resultaten in een bepaalde volgorde](useanddisplayrequirements.md). Omdat de API verschillende soorten antwoorden retourneren kan, het is niet genoeg is voor het hoogste niveau doorlopen `WebPages` verzameling in de JSON-respons en de resultaten weer te geven. (Als u slechts één type van de resultaten wilt, gebruikt de `responseFilter` queryparameter of een ander Bing zoeken-eindpunt.)

In plaats daarvan gebruiken we de `rankingResponse` in de zoekresultaten in de resultaten om weer te geven. Dit object verwijst naar items in de `WebPages` `News`, `Images`, en/of `Videos` verzamelingen, of in andere verzamelingen antwoord op het hoogste niveau in het JSON-antwoord.

`rankingResponse` kan maximaal drie verzamelingen van zoekresultaten aangewezen bevatten `pole`, `mainline`, en `sidebar`. 

`pole`, indien aanwezig, is de meest relevante zoekresultaten en duidelijk zichtbaar wilt maken moet worden weergegeven. `mainline` verwijst naar het merendeel van de zoekresultaten. Hoofdlijnen resultaten moeten worden weergegeven zodra `pole` (of eerst als `pole` is niet aanwezig). 

Ten slotte. `sidebar` verwijst naar reserve zoekresultaten. Deze resultaten zijn vaak verwante zoekopdrachten of de afbeeldingen. Indien mogelijk, moeten deze resultaten worden weergegeven in een werkelijke zijbalk. Als het scherm grenzen maken een sidebar niet praktisch (bijvoorbeeld op een mobiel apparaat), moeten ze worden weergegeven na de `mainline` resultaten.

Elk item in een `rankingResponse` verzameling verwijst naar de werkelijke zoekitems resultaat op twee manieren voor verschillende maar gelijkwaardig.

| | |
|-|-|
|`id`|De `id` ziet eruit als een URL, maar mag niet worden gebruikt voor koppelingen. De `id` van een ranking-resultaat overeenkomt met de `id` van beide een resultaatitem zoeken in een verzameling antwoord *of* een verzameling van het volledige antwoord (zoals `Images`).
|`answerType`, `resultIndex`|De `answerType` verwijst naar de verzameling op het hoogste niveau antwoord die het resultaat bevat (bijvoorbeeld `WebPages`). De `resultIndex` verwijst naar de index van het resultaat in die verzameling. Als `resultIndex` wordt weggelaten, wordt het resultaat ranking verwijst naar de volledige verzameling.

> [!NOTE]
> Zie voor meer informatie over dit deel van het antwoord van de zoekactie [positie resultaten](rank-results.md).

U kunt ongeacht welke methode het item waarnaar wordt verwezen zoeken resultaat te zoeken gemakkelijkste voor uw toepassing is. In onze zelfstudie code gebruiken we de `answerType` en `resultIndex` elk zoekresultaat vinden.

Ten slotte is het tijd om te kijken naar onze functie `renderSearchResults()`. Deze functie doorloopt over de drie `rankingResponse` verzamelingen met daarin de drie secties van de zoekresultaten. Voor elke sectie noemen we `renderResultsItems()` om de resultaten voor die sectie weer te geven.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // if spelling was corrected, update search field
    if (results.queryContext.alteredQuery) 
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` op zijn beurt doorloopt over de items in elke `rankingResponse` sectie, het resultaat van elke ranking wordt toegewezen aan een zoekactie resultaat met de `answerType` en `resultIndex` velden en de juiste rendering-functie voor het genereren van het resultaat HTML aanroept. 

Als `resultIndex` is niet opgegeven voor een item opgegeven ranking `renderResultsItems()` doorloopt over alle resultaten van dat type en de functie rendering aanroept voor elk item. 

In beide gevallen moet de HTML-code wordt ingevoegd in de juiste `<div>` element in de pagina.

```javascript
// render search results from rankingResponse object in specified order
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` rankingResult type is in the `webPages` top-level collection
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        // must have results of the given type AND a renderer for it
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // this ranking item refers to ONE result of the specified type
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // this ranking item refers to ALL results of the specified type
            } else {
                var len = results[type].value.length;
                for (var j = 0; j < len; j++) {
                    html.push(render(results[type].value[j], section, j, len));
                }
            }
        }
    }
    return html.join("\n\n");
}
```

## <a name="rendering-result-items"></a>Rendering resultaat items

In onze JavaScript-code is een object, `searchItemRenderers`, die bevat *renderers:* zoekresultaat functies die HTML gegenereerd voor elk type.

```javascript
// render functions for various types of search results
searchItemRenderers = { 
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!NOTE]
> Onze zelfstudie app heeft renderers voor webpagina's, nieuwsitems, afbeeldingen, video's en verwante zoekopdrachten. Uw eigen toepassing moet renderers voor alle soorten resultaten die kan worden weergegeven, waaronder berekeningen, spellingsuggesties entiteiten, tijdzones en definities kunnen.

Enkele van de functies van onze rendering accepteren alleen de `item` parameter: een JavaScript-object met een enkele zoekresultaat. Anderen accepteren extra parameters die kunnen worden gebruikt voor het weergeven van items in verschillende contexten anders. (Een renderer die u deze gegevens niet gebruikt hoeft niet te accepteren van deze parameters.)

De context-argumenten zijn:

| | |
|-|-|
|`section`|De sectie met resultaten (`pole`, `mainline`, of `sidebar`) waarin het item wordt weergegeven.
|`index`<br>`count`|Beschikbaar als de `rankingResponse` item geeft aan dat alle resultaten in een bepaalde verzameling moeten worden weergegeven. `undefined` anders. Deze parameters ontvangen de index van het item in de verzameling en het totale aantal items in die verzameling. U kunt deze gegevens als u een aantal van de resultaten van de verschillende HTML-code voor het eerste of laatste resultaat genereren, enzovoort.|

In onze zelfstudie app zowel de `images` en `relatedSearches` renderers gebruikmaken van de argumenten van de context voor het aanpassen van de HTML-code die zij genereren. Laten we dichter bij de `images` renderer:

```javascript
searchItemRenderers = { 
    // render image result using thumbnail
    images: function(item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];
        if (section === "sidebar") {
            if (index) html.push("<br>");
        } else {
            if (!index) html.push("<p class='images'>");
        }
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        return html.join("");
    }, // other renderers omitted
}
```

De functie van onze installatiekopie-renderer:

> [!div class="checklist"]
> * Berekent de miniatuur afbeeldingsgrootte (breedte varieert, terwijl de hoogte wordt vastgesteld op 60 pixels).
> * Hiermee voegt u de HTML-code die voorafgaat aan het resultaat van de installatiekopie, afhankelijk van de context.
> * De HTML-code builds `<a>` die is gekoppeld aan de pagina met de installatiekopie van het label.
> * De HTML-code builds `<img>` tag om de miniatuur weer te geven. 

Maakt gebruik van de renderer voor de installatiekopie van het `section` en `index` variabelen om weer te geven resultaten verschillend, afhankelijk van waar ze worden weergegeven. Een regeleinde (`<br>` tag) tussen de afbeeldingsresultaten van de in de zijbalk wordt geplaatst, zodat de zijbalk worden weergegeven voor een kolom van afbeeldingen. In andere secties, de eerste afbeelding leiden `(index === 0)` wordt voorafgegaan door een `<p>` label. De miniaturen anders dikke zich verhouden tot elkaar en wrap zo nodig in het browservenster.

De miniatuurgrootte wordt gebruikt in zowel de `<img>` code en de `h` en `w` velden in de miniatuur-URL. De [miniaturen Bing-service](resize-and-crop-thumbnails.md) voorziet in een miniatuur van precies die grootte. De `title` en `alt` kenmerken (een beschrijving van de afbeelding) zijn samengesteld uit de naam van de installatiekopie en de hostnaam in de URL.

Afbeeldingen worden weergegeven, zoals hier wordt weergegeven in de hoofdlijnen zoekresultaten.

![[Resultaten Bing afbeelding]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persisting-client-id"></a>Persisting client-ID

Antwoorden van de Bing zoeken-API's, omvat mogelijk een `X-MSEdge-ClientID` header die moet worden verzonden naar de API met opeenvolgende aanvragen. Als er meerdere Bing zoeken-API's worden gebruikt, moet dezelfde client-ID worden gebruikt met deze, indien mogelijk.

Geven de `X-MSEdge-ClientID` header kan de Bing-API's om te koppelen van een gebruiker zoekopdrachten met twee belangrijke voordelen.

Ten eerste kunt u de Bing zoekmachine voorbij context toepassen op zoekopdrachten resultaten oplevert die beter voldoen aan de gebruiker. Als een gebruiker eerder naar voorwaarden die betrekking hebben op afvaart gezocht is, bijvoorbeeld een hoger zoek naar 'knooppunten' mogelijk bij voorkeur informatie ophalen over knooppunten in afvaart gebruikt.

Ten tweede kunt Bing willekeurig selecteren voor gebruikers gebruikmaken van nieuwe functies voordat ze algemeen beschikbaar worden gemaakt. Dezelfde client-ID die aan elke aanvraag zorgt ervoor dat gebruikers die zijn geselecteerd om te zien van een functie altijd deze weergegeven. Zonder de client-ID ziet de gebruiker mogelijk een functie worden weergegeven en verdwijnt schijnbaar willekeurig in de zoekresultaten.

Browser-beveiligingsbeleid (CORS) mogelijk niet de `X-MSEdge-ClientID` header beschikbaar is voor JavaScript. Deze beperking treedt op wanneer het antwoord van de zoekactie heeft een andere oorsprong van de pagina die wordt aangevraagd. In een productieomgeving, moet u dit beleid door het hosten van een script op server die de API-aanroep in hetzelfde domein als de webpagina wordt adresseren. Aangezien het script dezelfde oorsprong als de webpagina heeft de `X-MSEdge-ClientID` header is beschikbaar voor JavaScript.

> [!NOTE]
> U moet de aanvraag-serverzijde toch uitvoeren in een productie-webtoepassing. Uw Bing zoeken-API-sleutel moet anders zijn opgenomen in de webpagina waar het voor iedereen die de bron-weergaven beschikbaar is. U wordt gefactureerd voor het gebruik van alle onder uw API-abonnement-sleutel, zelfs aanvragen door onbevoegden, dus is het belangrijk dat u niet uw sleutel beschikbaar.

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
> [Visual Search-zelfstudie voor mobiele app](computer-vision-web-search-tutorial.md)

> [!div class="nextstepaction"]
> [Bing Web Search API-verwijzingen](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
