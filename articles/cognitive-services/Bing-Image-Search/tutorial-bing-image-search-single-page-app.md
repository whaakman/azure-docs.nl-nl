---
title: Zoeken naar Bing-afbeelding één pagina Web-app | Microsoft Docs
description: Laat zien hoe de Bing installatiekopie zoeken-API gebruiken in een webtoepassing van één pagina.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: d0e1dc24513c8fc3a405cf1c18f531a0c58fad13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345480"
---
# <a name="tutorial-single-page-web-app"></a>Zelfstudie: Single-page-Web-app

De Bing installatiekopie Search API kunt u zoeken op het Web en afbeeldingsresultaten relevant zijn voor de zoekopdracht krijgen. In deze zelfstudie maken we een single-page-webtoepassing die gebruikmaakt van de installatiekopie Search-API van Bing om zoekresultaten weer te rechts op de pagina. De toepassing bevat onderdelen die HTML, CSS en JavaScript.

<!-- Remove until we can sanitize images
![[Single-page Bing Image Search app]](media/cognitive-services-bing-images-api/image-search-spa-demo.png)
-->

> [!NOTE]
> De JSON- en HTTP-koppen aan de onderkant van de pagina onthullen de JSON-antwoord en HTTP-aanvraag informatie wanneer geklikt. Deze gegevens zijn nuttig wanneer de service te verkennen.

De app zelfstudie ziet u hoe:

> [!div class="checklist"]
> * Een Bing installatiekopie zoeken-API-aanroep in JavaScript uitvoeren
> * Zoekopties doorgeven aan de installatiekopie Search-API van Bing
> * Zoekresultaten weergeven
> * Pagina met zoekresultaten
> * Ingang Bing-ID en API-abonnementssleutel van de client
> * Afhandelen van fouten die optreden

De zelfstudie pagina is volledig onafhankelijke; gebruikt niet alle externe frameworks, opmaakmodellen of zelfs afbeeldingsbestanden. Alleen functies voor sterk ondersteund JavaScript-taal wordt gebruikt en werkt met huidige versies van alle bekende webbrowsers.

In deze zelfstudie bespreken we alleen geselecteerde gedeelten van de broncode. De volledige broncode is beschikbaar [op een afzonderlijke pagina](tutorial-bing-image-search-single-page-app-source.md). Kopieer en plak deze code in een teksteditor en sla het bestand als `bing.html`.

> [!NOTE]
> Deze zelfstudie is aanzienlijk vergelijkbaar met de [één pagina Bing zoeken op het Web-app zelfstudie](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), maar behandelt alleen zoekresultaten installatiekopie.

## <a name="app-components"></a>App-onderdelen

Net als alle Web-Apps van één pagina is de zelfstudie toepassing bestaat uit drie delen:

> [!div class="checklist"]
> * HTML - definieert de structuur en inhoud van de pagina
> * CSS - bepaalt de vormgeving van de pagina
> * JavaScript - definieert het gedrag van de pagina

Deze zelfstudie dekt niet in de meeste van de HTML-indeling of CSS in detail zoals ze simpel zijn.

De HTML-code bevat het zoekformulier waarin de gebruiker voert een query en zoekopties kiest. Het formulier is verbonden met de JavaScript die daadwerkelijk wordt uitgevoerd de zoekopdracht op de `<form>` van de tag `onsubmit` kenmerk:

```html
<form name="bing" onsubmit="return newBingImageSearch(this)">
```

De `onsubmit` handler retourneert `false`, waarmee het formulier worden verzonden naar een server houdt. De JavaScript-code komt daadwerkelijk het werk van de benodigde informatie verzamelen van het formulier en het uitvoeren van de zoekopdracht.

De HTML-code bevat ook de afdelingen (HTML `<div>` labels) waar de zoekresultaten worden weergegeven.

## <a name="managing-subscription-key"></a>Abonnementssleutel beheren

Om te voorkomen dat de sleutel Bing zoeken-API-abonnement in de code opnemen, gebruiken we de permanente opslag van de browser voor het opslaan van de sleutel. Als er geen sleutel is opgeslagen, wordt gevraagd om de sleutel van de gebruiker en opgeslagen voor later gebruik. Als de sleutel wordt later geweigerd door de API, ongeldig we opgeslagen zodat de gebruiker is het opnieuw.

Definiëren we `storeValue` en `retrieveValue` functies die gebruikmaken van een de `localStorage` object (als de browser het ondersteunt) of een cookie. Onze `getSubscriptionKey()` functie maakt gebruik van deze functies op te slaan en het ophalen van de sleutel van de gebruiker.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";

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

De HTML-code `<form>` tag `onsubmit` aanroepen de `bingWebSearch` functie voor het retourneren van resultaten van de zoekmap. `bingWebSearch` maakt gebruik van `getSubscriptionKey` om te verifiëren van elke query. Zoals wordt weergegeven in de vorige definitie `getSubscriptionKey` vraagt de gebruiker om de sleutel als de sleutel nog niet is opgegeven. De sleutel wordt vervolgens opgeslagen voor voortgezet gebruik door de toepassing.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Zoekopties selecteren

![[Formulier zoeken naar Bing-afbeelding]](media/cognitive-services-bing-images-api/image-search-spa-form.png)

Het HTML-formulier bevat de volgende besturingselementen:

| | |
|-|-|
|`where`|Een vervolgkeuzelijst voor het selecteren van de markt (locatie en taal) dat wordt gebruikt voor de zoekopdracht.|
|`query`|Het tekstveld in te voeren van de zoektermen.|
|`aspect`|Keuzerondjes voor het kiezen van de verhoudingen van installatiekopieën van het gevonden: ongeveer vierkante breed of hoog.|
|`color`|Kleur zwart is, of een overwegend kleur selecteert.
|`when`|De vervolgkeuzelijst voor het beperken van eventueel om te zoeken naar de meest recente dag, week of maand.|
|`safe`|Een selectievakje waarmee wordt aangegeven of de Bing veilig zoeken functie gebruiken om te filteren 'volwassenen' resultaten.|
|`count`|Verborgen veld. Het aantal zoekresultaten worden geretourneerd bij elke aanvraag. Wijzigen om minder of meer resultaten per pagina weer te geven.|
|`offset`|Verborgen veld. De verschuiving van de eerste zoekresultaten in de aanvraag; gebruikt voor het wisselbestand. Dit wordt opnieuw ingesteld op `0` op een nieuwe aanvraag.|
|`nextoffset`|Verborgen veld. Bij ontvangst van een zoekresultaat, dit veld is ingesteld op de waarde van de `nextOffset` in het antwoord. In dit veld voorkomt overlappende resultaten op pagina's met opeenvolgende.|
|`stack`|Verborgen veld. Een lijst JSON-codering van de verschuivingen van de voorgaande pagina's met zoekresultaten voor het navigeren terug naar vorige pagina's.|

> [!NOTE]
> Zoeken naar Bing-afbeelding biedt veel meer queryparameters. We maken gebruik van slechts enkele van deze hier.

Onze JavaScript-functie `bingSearchOptions()` deze velden converteert naar een gedeeltelijke queryreeks in de indeling van de Bing zoeken-API.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var aspect = "all";
    for (var i = 0; i < form.aspect.length; i++) {
        if (form.aspect[i].checked) {
            aspect = form.aspect[i].value;
            break;
        }
    }
    options.push("aspect=" + aspect);
    if (form.color.value) options.push("color=" + form.color.value);
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Bijvoorbeeld: de functie veilig zoeken kan worden `strict`, `moderate`, of `off`, met `moderate` wordt de standaardwaarde. Maar ons formulier maakt gebruik van een selectievakje dat slechts twee statussen heeft. De JavaScript-code converteert u deze instelling op `strict` of `off` (we gebruik geen `moderate`).

## <a name="performing-the-request"></a>Uitvoeren van de aanvraag

Basis van de query, de opties-tekenreeks en de API-sleutel, de `BingImageSearch` functie maakt gebruik van een `XMLHttpRequest` object, zodat de aanvraag naar het eindpunt zoeken naar Bing-afbeelding.

```javascript
// perform a search given query, options string, and API key
function bingImageSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

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
            if (jsobj._type === "Images") {
                if (jsobj.nextOffset) document.forms.bing.nextoffset.value = jsobj.nextOffset;
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
> Een geslaagde HTTP-aanvraag komt *niet* noodzakelijkerwijs dat de zoekopdracht zelf is voltooid. Als er een fout optreedt tijdens de zoekbewerking wordt de Bing installatiekopie zoeken-API retourneert een statuscode 200 HTTP en bevat informatie over de fout in de JSON-antwoord. Als de aanvraag snelheid beperkt is, retourneert de API bovendien een leeg antwoord.

Veel van de code in beide van de voorgaande functies is speciaal bedoeld voor foutafhandeling. Er kunnen fouten optreden in de volgende fasen:

|Fase|Potentiële fouten|Verwerkt door|
|-|-|-|
|Gebouw JavaScript request-object|De URL is ongeldig|`try`/`catch` blokkeren|
|Maken van de aanvraag|Netwerkfouten, afgebroken verbindingen|`error` en `abort` gebeurtenis-handlers|
|De zoekactie|Ongeldige aanvraag, ongeldige JSON-frequentielimieten|webtests op `load` gebeurtenis-handler|

Fouten worden verwerkt door het aanroepen van `renderErrorMessage()` met details over de fout bekend. Als het antwoord voldoet aan de volledige gauntlet fout tests, noemen we `renderSearchResults()` om de zoekresultaten op de pagina weer te geven.

## <a name="displaying-search-results"></a>Zoekresultaten weergeven

De belangrijkste functie voor het weergeven van de zoekresultaten is `renderSearchResults()`. Deze functie neemt de JSON die is geretourneerd door de service voor zoeken naar Bing-afbeelding en de afbeeldingen en de verwante zoekopdrachten renders, indien van toepassing.

```javascript
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    showDiv("results", renderImageResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```

De belangrijkste installatiekopie zoekresultaten worden geretourneerd als het hoogste niveau `value` object in het JSON-antwoord. Ze geven we onze functie `renderImageResults()`, waardoor ze doorlopen en roept een afzonderlijke functie voor elk item weergeven in HTML. De HTML-code wordt geretourneerd naar `renderSearchResults()`, waar deze wordt ingevoegd in de `results` deling op de pagina.

```javascript
function renderImageResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.images(items[i], i, len));
    }
    return html.join("\n\n");
}
```

De Bing installatiekopie zoeken-API retourneert maximaal vier verschillende soorten gerelateerde resultaten in een eigen object op het hoogste niveau. Dit zijn:

|||
|-|-|
|`pivotSuggestions`|Query's die een woord pivot in oorspronkelijke zoekopdracht door een andere naam vervangen. Bijvoorbeeld, als u zoekt naar 'rode bloemen', een woord pivot mogelijk "red" en een suggestie pivot mogelijk "gele bloemen."|
|`queryExpansions`|Query's die de oorspronkelijke zoekopdracht te verfijnen door meer voorwaarden toe te voegen. Bijvoorbeeld, als u zoekt naar 'Microsoft Surface ', een query-uitbreiding mogelijk "Microsoft Surface Pro."|
|`relatedSearches`|Query's die ook door andere gebruikers die de oorspronkelijke zoekopdracht ingevoerd zijn ingevoerd. Bijvoorbeeld, als u zoekt naar 'Mount Rainier', een gerelateerde zoekopdracht mogelijk 'onderwerp Mt. Saint Helens."|
|`similarTerms`|Query's met dezelfde betekenis voor de oorspronkelijke zoekopdracht. Bijvoorbeeld, als u zoekt naar 'katjes', misschien een vergelijkbare term "leuk."|

Zoals eerder is gezien in `renderSearchResults()`, we alleen weergeven, de `relatedItems` suggesties en plaats de resulterende koppelingen in de zijbalk van de pagina.

## <a name="rendering-result-items"></a>Rendering resultaat items

In onze JavaScript-code is een object, `searchItemRenderers`, die bevat *renderers:* zoekresultaat functies die HTML gegenereerd voor elk type.

```javascript
searchItemRenderers = { 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```

Een functie renderer mogen de volgende parameters:

| | |
|-|-|
|`item`|De JavaScript-object met de eigenschappen van het item, zoals de URL en de beschrijving.|
|`index`|De index van het resultaatitem binnen de verzameling.|
|`count`|Het aantal items in de verzameling van de zoekopdracht resultaat item.|

De `index` en `count` parameters kunnen worden gebruikt om te aantal resultaten voor het genereren van speciale HTML voor het begin of einde van een verzameling regeleinden invoegen na een bepaald aantal items, enzovoort. Als een renderer niet nodig heeft voor deze functionaliteit, hoeft deze niet te accepteren van deze twee parameters.

Laten we dichter bij de `images` renderer:

```javascript
    images: function (item, index, count) {
        var height = 120;
        var width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
        var html = [];
        if (index === 0) html.push("<p class='images'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<p class='images' style='max-width: " + width + "px'>");
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + "'>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Image</a> - ");
        html.push("<a href='" + item.hostPageUrl + "'>Page</a></nobr><br>");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }, // relatedSearches renderer omitted
```

De functie van onze installatiekopie-renderer:

> [!div class="checklist"]
> * Berekent de miniatuur afbeeldingsgrootte (breedte varieert, met minimaal 120 pixels, terwijl de hoogte wordt vastgesteld op 90 pixels).
> * De HTML-code builds `<img>` tag om de miniatuur weer te geven. 
> * De HTML-code builds `<a>` labels die een koppeling naar de installatiekopie en de pagina waarvan het deel uitmaakt.
> * De beschrijving die geeft informatie weer over de installatiekopie en de site is gebaseerd.

We testen de `index` variabele als u wilt invoegen een `<p>` tag voordat het eerste resultaat van de installatiekopie. De miniaturen anders dikke zich verhouden tot elkaar en wrap zo nodig in het browservenster.

De miniatuurgrootte wordt gebruikt in zowel de `<img>` code en de `h` en `w` velden in de miniatuur-URL. De [miniaturen Bing-service](resize-and-crop-thumbnails.md) voorziet in een miniatuur van precies die grootte.

## <a name="persisting-client-id"></a>Persisting client-ID

Antwoorden van de Bing zoeken-API's, omvat mogelijk een `X-MSEdge-ClientID` header die moet worden verzonden naar de API met opeenvolgende aanvragen. Als er meerdere Bing zoeken-API's worden gebruikt, moet dezelfde client-ID worden gebruikt met deze, indien mogelijk.

Geven de `X-MSEdge-ClientID` header kan de Bing-API's om te koppelen van een gebruiker zoekopdrachten met twee belangrijke voordelen.

Ten eerste kunt u de Bing zoekmachine voorbij context toepassen op zoekopdrachten resultaten oplevert die beter voldoen aan de gebruiker. Als een gebruiker eerder naar voorwaarden die betrekking hebben op afvaart gezocht is, bijvoorbeeld een hoger zoek naar 'knooppunten' mogelijk bij voorkeur informatie ophalen over knooppunten in afvaart gebruikt.

Ten tweede kunt Bing willekeurig selecteren voor gebruikers gebruikmaken van nieuwe functies voordat ze algemeen beschikbaar worden gemaakt. Dezelfde client-ID die aan elke aanvraag zorgt ervoor dat de gebruikers die zijn geselecteerd om te zien van een functie altijd het bekijken. Zonder de client-ID ziet de gebruiker mogelijk een functie worden weergegeven en verdwijnt schijnbaar willekeurig in de zoekresultaten.

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
> [Bing installatiekopie Search API-verwijzingen](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

