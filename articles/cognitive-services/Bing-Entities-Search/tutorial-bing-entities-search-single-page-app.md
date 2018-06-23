---
title: Bing entiteit zoeken één pagina web-app | Microsoft Docs
description: Laat zien hoe de Bing entiteit zoeken-API gebruiken in een webtoepassing van één pagina.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/08/2017
ms.author: v-jerkin
ms.openlocfilehash: 91c60913cd806baf100e5511cbf59299bf9a84f0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345587"
---
# <a name="tutorial-single-page-web-app"></a>Zelfstudie: Single-page-web-app

De API van Bing entiteit zoeken kunt u zoeken op het Web voor informatie over *entiteiten* en *wordt geplaatst.* U kunt aanvragen soort resultaat of beide in een bepaalde query. Hieronder vindt u de definities van plaatsen en entiteiten.

|||
|-|-|
|Entiteiten|Bekende mensen, plaatsen en zaken die u kunt op naam zoeken|
|Plaatsen|Restaurant, hotels en andere lokale bedrijven die u kunt op naam zoeken *of* door type (Italiaanse restaurant)|

In deze zelfstudie maken we een single-page-webtoepassing die gebruikmaakt van de entiteit zoeken-API van Bing om zoekresultaten weer te rechts op de pagina. De toepassing bevat onderdelen die HTML, CSS en JavaScript.

De API kunt u resultaten per locatie prioriteren. In een mobiele app vraagt u het apparaat voor de eigen locatie. In een Web-app kunt u de `getPosition()` functie. Maar deze aanroep werkt alleen in de veilige contexten, en kan niet hierin een specifieke locatie. De gebruiker wil ook voor entiteiten in de buurt van een andere locatie dan hun eigen zoeken.

De app wordt daarom de Bing Maps-service voor breedtegraad en lengtegraad van een door de gebruiker ingevoerde locatie verzocht. De gebruiker Voer vervolgens de naam van een kenmerkende ('ruimte naald') of een volledige of gedeeltelijke adres ('New York City') en de Bing kaarten-API biedt de coördinaten.

<!-- Remove until we can replace with a sanitized version.
![[Single-page Bing Entity Search app]](media/entity-search-spa-demo.png)
-->

> [!NOTE]
> De JSON- en HTTP-koppen aan de onderkant van de pagina onthullen de JSON-antwoord en HTTP-aanvraag informatie wanneer geklikt. Deze gegevens zijn nuttig wanneer de service te verkennen.

De app zelfstudie ziet u hoe:

> [!div class="checklist"]
> * Een Bing entiteit zoeken-API-aanroep in JavaScript uitvoeren
> * Uitvoeren van een Bing Maps `locationQuery` API-aanroep in JavaScript
> * Zoekopties doorgeven aan de API-aanroepen
> * Zoekresultaten weergeven
> * De Bing-client-ID en API-abonnement sleutels verwerken
> * Omgaan met eventuele fouten die optreden

De zelfstudie pagina is volledig onafhankelijke; gebruikt niet alle externe frameworks, opmaakmodellen of zelfs afbeeldingsbestanden. Alleen functies voor sterk ondersteund JavaScript-taal wordt gebruikt en werkt met huidige versies van alle bekende webbrowsers.

In deze zelfstudie bespreken we alleen geselecteerde gedeelten van de broncode. De volledige broncode is beschikbaar [op een afzonderlijke pagina](tutorial-bing-entities-search-single-page-app-source.md). Kopieer en plak deze code in een teksteditor en sla het bestand als `bing.html`.

> [!NOTE]
> Deze zelfstudie is aanzienlijk vergelijkbaar met de [één pagina Bing zoeken op het Web-app zelfstudie](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), maar behandelt alleen zoekresultaten entiteit.

## <a name="app-components"></a>App-onderdelen

Net als alle Web-Apps van één pagina is de zelfstudie toepassing bestaat uit drie delen:

> [!div class="checklist"]
> * HTML - definieert de structuur en inhoud van de pagina
> * CSS - bepaalt de vormgeving van de pagina
> * JavaScript - definieert het gedrag van de pagina

Deze zelfstudie dekt niet in de meeste van de HTML-indeling of CSS in detail zoals ze simpel zijn.

De HTML-code bevat het zoekformulier waarin de gebruiker voert een query en zoekopties kiest. Het formulier is verbonden met de JavaScript die daadwerkelijk wordt uitgevoerd de zoekopdracht op de `<form>` van de tag `onsubmit` kenmerk:

```html
<form name="bing" onsubmit="return newBingEntitySearch(this)">
```

De `onsubmit` handler retourneert `false`, waarmee het formulier worden verzonden naar een server houdt. De JavaScript-code komt daadwerkelijk het werk van de benodigde informatie verzamelen van het formulier en het uitvoeren van de zoekopdracht.

De zoekopdracht wordt uitgevoerd in twee fasen. Als de gebruiker heeft een beperking van de locatie hebt ingevoerd, wordt eerst een Bing Maps-query om deze te converteren naar coördinaten gedaan. De callback voor deze query is vervolgens serversysteemstatus van de zoekopdracht Bing entiteit.

De HTML-code bevat ook de afdelingen (HTML `<div>` labels) waar de zoekresultaten worden weergegeven.

## <a name="managing-subscription-keys"></a>Abonnement sleutels beheren

> [!NOTE]
> Deze app vereist abonnement sleutels voor zowel de Bing zoeken-API en de Bing kaarten-API. U kunt een [proefversie Bing zoeksleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) en een [basic Bing Maps-sleutel](https://www.microsoft.com/maps/create-a-bing-maps-key).

Om te voorkomen dat de sleutels van de Bing-zoekopdracht en Bing kaarten-API-abonnement in de code opnemen, gebruiken we de permanente opslag van de browser wilt opslaan. Als de sleutel niet opgeslagen is, we vragen om deze en opgeslagen voor later gebruik. Als de sleutel wordt later geweigerd door de API, ongeldig we opgeslagen zodat de gebruiker gevraagd om bij hun volgende zoekopdracht.

Definiëren we `storeValue` en `retrieveValue` functies die gebruikmaken van een de `localStorage` object (als de browser het ondersteunt) of een cookie. Onze `getSubscriptionKey()` functie maakt gebruik van deze functies op te slaan en het ophalen van de sleutel van de gebruiker.

```javascript
// cookie names for data we store
SEARCH_API_KEY_COOKIE = "bing-search-api-key";
MAPS_API_KEY_COOKIE   = "bing-maps-api-key";
CLIENT_ID_COOKIE      = "bing-search-client-id";

// API endpoints
SEARCH_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/entities";
MAPS_ENDPOINT   = "http://dev.virtualearth.net/REST/v1/Locations";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey(cookie_name, key_length, api_name) {
    var key = retrieveValue(cookie_name);
    while (key.length !== key_length) {
        key = prompt("Enter " + api_name + " API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(cookie_name, key);
    return key;
}

function getMapsSubscriptionKey() {
    return getSubscriptionKey(MAPS_API_KEY_COOKIE, 64, "Bing Maps");
}

function getSearchSubscriptionKey() {
    return getSubscriptionKey(SEARCH_API_KEY_COOKIE, 32, "Bing Search");
}
```

De HTML-code `<body>` label bevat een `onload` kenmerk dat roept `getSearchSubscriptionKey()` en `getMapsSubscriptionKey()` wanneer de pagina is geladen. Deze aanroepen leveren aan onmiddellijk de gebruiker gevraagd om hun sleutels als ze deze nog niet hebt ingevoerd.

```html
<body onload="document.forms.bing.query.focus(); getSearchSubscriptionKey(); getMapsSubscriptionKey();">
```

## <a name="selecting-search-options"></a>Zoekopties selecteren

![[Formulier Bing entiteit zoeken]](media/entity-search-spa-form.png)

Het HTML-formulier bevat de volgende besturingselementen:

| | |
|-|-|
|`where`|Een vervolgkeuzelijst voor het selecteren van de markt (locatie en taal) dat wordt gebruikt voor de zoekopdracht.|
|`query`|Het tekstveld in te voeren van de zoektermen.|
|`safe`|Een selectievakje waarmee wordt aangegeven of veilig zoeken is ingeschakeld ('volwassenen' resultaten beperkt)|
|`what`|Een menu voor kiezen om te zoeken naar entiteiten, plaatsen of beide.|
|`mapquery`|Het veld waarin de gebruiker een volledige of gedeeltelijke adres, een kenmerkende, enzovoort om Bing entiteit return meer relevante zoekresultaten te kan invoeren.|

> [!NOTE]
> Resultaten van de ruimten zijn momenteel alleen beschikbaar in de Verenigde Staten. De `where` en `what` menu's hebben de code voor het afdwingen van deze beperking. Als u een niet-Amerikaanse markt terwijl plaatsen is geselecteerd de `what` menu `what` in iets anders verandert. Als u ervoor plaatsen kiest wanneer een niet-Amerikaanse markt is geselecteerd de `where` menu `where` wijzigingen in de Verenigde Staten.

Onze JavaScript-functie `bingSearchOptions()` deze velden converteert naar een gedeeltelijke query-tekenreeks voor de Bing zoeken-API.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.what.selectedIndex) options.push("responseFilter=" + form.what.value);
    return options.join("&");
}
```

Bijvoorbeeld: de functie veilig zoeken kan worden `strict`, `moderate`, of `off`, met `moderate` wordt de standaardwaarde. Maar ons formulier maakt gebruik van een selectievakje dat slechts twee statussen heeft. De JavaScript-code converteert u deze instelling op `strict` of `off` (we gebruik geen `moderate`).

De `mapquery` veld is niet verwerkt in `bingSearchOptions()` omdat deze niet voor Bing entiteit zoeken voor de query Bing Maps-locatie wordt gebruikt.

## <a name="obtaining-a-location"></a>Het verkrijgen van een locatie

De Bing kaarten-API biedt een [ `locationQuery` methode](//msdn.microsoft.com/library/ff701711.aspx)die we gebruiken om te zoeken naar de breedtegraad en lengtegraad van de locatie van de gebruiker invoert. Deze coördinaten worden vervolgens doorgegeven aan de Bing zoeken-API voor entiteit met aanvraag van de gebruiker. De zoekresultaten prioriteren entiteiten en plaatsen die zich dicht bij de opgegeven locatie.

We hebben geen toegang tot de Bing kaarten-API met behulp van een gewone `XMLHttpRequest` omdat de service biedt geen ondersteuning voor query's voor cross-origin-query in een Web-app. Gelukkig ondersteunt JSONP (de ' P ' is voor 'die is opgevuld'). Een JSONP-antwoord is een gewone JSON-antwoord ingepakt in een functieaanroep. De aanvraag wordt gedaan door in te voegen met behulp van een `<script>` label in het document. (Bij het laden van scripts is niet onderworpen aan het beveiligingsbeleid van de browser.)

De `bingMapsLocate()` functie wordt gemaakt en wordt de `<script>` tag voor de query. De `jsonp=bingMapsCallback` segment van de queryreeks bevat de naam van de functie moet worden aangeroepen met het antwoord.

```javascript
function bingMapsLocate(where) {

    where = where.trim();
    var url = MAPS_ENDPOINT + "?q=" + encodeURIComponent(where) + 
                "&jsonp=bingMapsCallback&maxResults=1&key=" + getMapsSubscriptionKey();

    var script = document.getElementById("bingMapsResult")
    if (script) script.parentElement.removeChild(script);

    // global variable holds reference to timer that will complete the search if the maps query fails
    timer = setTimeout(function() {
        timer = null;
        var form = document.forms.bing;
        bingEntitySearch(form.query.value, "", bingSearchOptions(form), getSearchSubscriptionKey());
    }, 5000);

    script = document.createElement("script");
    script.setAttribute("type", "text/javascript");
    script.setAttribute("id", "bingMapsResult");
    script.setAttribute("src", url);
    script.setAttribute("onerror", "BingMapsCallback(null)");
    document.body.appendChild(script);

    return false;
}
```

> [!NOTE]
> Als de Bing kaarten-API niet reageert, de `bingMapsCallBack()` functie wordt nooit aangeroepen. Normaal gesproken dit betekent dat dat `bingEntitySearch()` is niet aangeroepen, en de entiteit zoekresultaten worden niet weergegeven. Om te voorkomen dat dit scenario `bingMapsLocate()` stelt u ook een timer aan te roepen `bingEntitySearch()` na vijf seconden. Er is logica in de callback-functie om te voorkomen dat de zoekopdracht entiteit tweemaal uitvoeren.

Wanneer de query is voltooid, de `bingMapsCallback()` functie wordt aangeroepen, zoals aangevraagd.

```javascript
function bingMapsCallback(response) {

    if (timer) {    // we beat the timer; stop it from firing
        clearTimeout(timer);
        timer = null;
    } else {        // the timer beat us; don't do anything
        return; 
    }

    var location = "";
    var name = "";
    var radius = 1000;

    if (response) {
        try {
            if (response.statusCode === 401) {
                invalidateMapsKey();
            } else if (response.statusCode === 200) {
                var resource = response.resourceSets[0].resources[0];
                var coords   = resource.point.coordinates;
                name         = resource.name;

                // the radius is the largest of the distances between the location and the corners
                // of its bounding box (in case it's not in the center) with a minimum of 1 km
                try {
                    var bbox    = resource.bbox;
                    radius  = Math.max(haversineDistance(bbox[0], bbox[1], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[1]),
                                       haversineDistance(bbox[0], bbox[3], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[3]), 1000);
                } catch(e) {  }
                var location = "lat:" + coords[0] + ";long:" + coords[1] + ";re:" + Math.round(radius);
            }
        }
        catch (e) { }   // response is unexpected. this isn't fatal, so just don't provide location
    }

    var form = document.forms.bing;
    if (name) form.mapquery.value = name;
    bingEntitySearch(form.query.value, location, bingSearchOptions(form), getSearchSubscriptionKey());

}
```

Samen met de breedtegraad en lengtegraad de Bing-entiteit-zoekopdracht vereist een *radius* die aangeeft hoe nauwkeurig er informatie over de locatie. We berekenen van de RADIUS-met behulp van de *begrenzingsvak* opgegeven in het antwoord Bing-kaarten. Het begrenzingsvak is een rechthoek rond de hele locatie. Bijvoorbeeld, als de gebruiker invoert `NYC`, het resultaat bevat ongeveer centrale coördinaten van de New York City en een selectiekader dat de plaats omvat. 

We de afstand tussen de primaire coördinaten aan elk van de vier hoeken van het begrenzingsvak met de functie voor het eerst berekenen `haversineDistance()` (niet weergegeven). We gebruiken de grootste waarde in deze vier afstanden als de radius. De minimale radius is een kilometer. Deze waarde wordt ook gebruikt als een standaard als er geen begrenzingsvak is opgegeven in het antwoord.

De coördinaten en de radius te hebben verkregen, klikt u vervolgens noemen we `bingEntitySearch()` de werkelijke zoekopdracht wordt uitgevoerd.

## <a name="performing-the-search"></a>De zoekactie

De query, een locatie, een reeks opties en de API-sleutel opgegeven de `BingEntitySearch()` functie kunt u de zoekaanvraag Bing entiteit.

```javascript
// perform a search given query, location, options string, and API keys
function bingEntitySearch(query, latlong, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "error");

    var request = new XMLHttpRequest();
    var queryurl = SEARCH_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

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

    if (latlong) request.setRequestHeader("X-Search-Location", latlong);

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
            if (jsobj._type === "SearchResponse") {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    if (divHidden("pole") && divHidden("mainline") && divHidden("sidebar")) 
        showDiv("noresults", "No results.<p><small>Looking for restaurants or other local businesses? Those currently areen't supported outside the US.</small>");
    }

    // Any other HTTP status is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSearchKey();

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
> Een geslaagde HTTP-aanvraag komt *niet* noodzakelijkerwijs dat de zoekopdracht zelf is voltooid. Als er een fout optreedt tijdens de zoekbewerking wordt de Bing entiteit zoeken-API retourneert een statuscode 200 HTTP en bevat informatie over de fout in de JSON-antwoord. Als de aanvraag snelheid beperkt is, retourneert de API bovendien een leeg antwoord.

Veel van de code in beide van de voorgaande functies is speciaal bedoeld voor foutafhandeling. Er kunnen fouten optreden in de volgende fasen:

|Fase|Potentiële fouten|Verwerkt door|
|-|-|-|
|Gebouw JavaScript request-object|De URL is ongeldig|`try`/`catch` blokkeren|
|Maken van de aanvraag|Netwerkfouten, afgebroken verbindingen|`error` en `abort` gebeurtenis-handlers|
|De zoekactie|Ongeldige aanvraag, ongeldige JSON-frequentielimieten|webtests op `load` gebeurtenis-handler|

Fouten worden verwerkt door het aanroepen van `renderErrorMessage()` met details over de fout bekend. Als het antwoord voldoet aan de volledige gauntlet fout tests, noemen we `renderSearchResults()` om de zoekresultaten op de pagina weer te geven.

## <a name="displaying-search-results"></a>Zoekresultaten weergeven

De Bing-API van zoekservice entiteit [, moet u voor het weergeven van resultaten in een bepaalde volgorde](use-display-requirements.md). Omdat de API twee soorten antwoorden retourneren kan, het is niet genoeg is voor het hoogste niveau doorlopen `Entities` of `Places` verzameling in de JSON-respons en de resultaten weer te geven. (Als u slechts één type resultaat wilt, gebruikt de `responseFilter` queryparameter.)

In plaats daarvan gebruiken we de `rankingResponse` verzameling in de zoekresultaten in de resultaten om weer te geven. Dit object verwijst naar items in de `Entitiess` en/of `Places` verzamelingen.

`rankingResponse` kan maximaal drie verzamelingen van zoekresultaten aangewezen bevatten `pole`, `mainline`, en `sidebar`. 

`pole`, indien aanwezig, is de meest relevante zoekresultaten en duidelijk zichtbaar wilt maken moet worden weergegeven. `mainline` verwijst naar het merendeel van de zoekresultaten. Hoofdlijnen resultaten moeten worden weergegeven zodra `pole` (of eerst als `pole` is niet aanwezig). 

Ten slotte. `sidebar` verwijst naar reserve zoekresultaten. Ze kunnen worden weergegeven in een werkelijke zijbalk of gewoon na de hoofdlijnen resultaten. We hebben ervoor gekozen de laatste voor onze zelfstudie app.

Elk item in een `rankingResponse` verzameling verwijst naar de werkelijke zoekitems resultaat op twee manieren voor verschillende maar gelijkwaardig.

| | |
|-|-|
|`id`|De `id` ziet eruit als een URL, maar mag niet worden gebruikt voor koppelingen. De `id` van een ranking-resultaat overeenkomt met de `id` van beide een resultaatitem zoeken in een verzameling antwoord *of* een verzameling van het volledige antwoord (zoals `Entities`).
|`answerType`<br>`resultIndex`|De `answerType` verwijst naar de verzameling op het hoogste niveau antwoord die het resultaat bevat (bijvoorbeeld `Entities`). De `resultIndex` verwijst naar de index van het resultaat in die verzameling. Als `resultIndex` wordt weggelaten, wordt het resultaat ranking verwijst naar de volledige verzameling.

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

    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

## <a name="rendering-result-items"></a>Rendering resultaat items

In onze JavaScript-code is een object, `searchItemRenderers`, die bevat *renderers:* zoekresultaat functies die HTML gegenereerd voor elk type.

```javascript
searchItemRenderers = { 
    entities: function(item) { ... },
    places: function(item) { ... }
}
```

Een functie renderer mogen de volgende parameters:

| | |
|-|-|
|`item`|De JavaScript-object met de eigenschappen van het item, zoals de URL en de beschrijving.|
|`index`|De index van het resultaatitem binnen de verzameling.|
|`count`|Het aantal items in de verzameling van de zoekopdracht resultaat item.|

De `index` en `count` parameters kunnen worden gebruikt om te aantal resultaten voor het genereren van speciale HTML voor het begin of einde van een verzameling regeleinden invoegen na een bepaald aantal items, enzovoort. Als een renderer niet nodig heeft voor deze functionaliteit, hoeft deze niet te accepteren van deze twee parameters. In feite gebruik we geen ze in de renderers voor onze zelfstudie app.

Laten we dichter bij de `entities` renderer:

```javascript
    entities: function(item) {
        var html = [];
        html.push("<p class='entity'>");
        if (item.image) {
            var img = item.image;
            if (img.hostPageUrl) html.push("<a href='" + img.hostPageUrl + "'>");
            html.push("<img src='" + img.thumbnailUrl +  "' title='" + img.name + "' height=" + img.height + " width= " + img.width + ">");
            if (img.hostPageUrl) html.push("</a>");
            if (img.provider) {
                var provider = img.provider[0];
                html.push("<small>Image from ");
                if (provider.url) html.push("<a href='" + provider.url + "'>");
                html.push(provider.name ? provider.name : getHost(provider.url));
                if (provider.url) html.push("</a>");
                html.push("</small>");
            }
        }
        html.push("<p>");
        if (item.entityPresentationInfo) {
            var pi = item.entityPresentationInfo;
            if (pi.entityTypeHints || pi.entityTypeDisplayHint) {
                html.push("<i>");
                if (pi.entityTypeDisplayHint) html.push(pi.entityTypeDisplayHint);
                else if (pi.entityTypeHints) html.push(pi.entityTypeHints.join("/"));
                html.push("</i> - ");
            }
        }
        html.push(item.description);
        if (item.webSearchUrl) html.push("&nbsp;<a href='" + item.webSearchUrl + "'>More</a>")
        if (item.contractualRules) {
            html.push("<p><small>");
            var rules = [];
            for (var i = 0; i < item.contractualRules.length; i++) {
                var rule = item.contractualRules[i];
                var link = [];
                if (rule.license) rule = rule.license;
                if (rule.url) link.push("<a href='" + rule.url + "'>");
                link.push(rule.name || rule.text || rule.targetPropertyName + " source");
                if (rule.url) link.push("</a>");
                rules.push(link.join(""));
            }
            html.push("License: " + rules.join(" - "));
            html.push("</small>");
        }
        return html.join("");
    }, // places renderer omitted
```

De functie van de entiteit-renderer:

> [!div class="checklist"]
> * De HTML-code builds `<img>` tag om weer te geven van de miniatuur van de afbeelding, indien van toepassing. 
> * De HTML-code builds `<a>` code die is gekoppeld aan de pagina met de installatiekopie.
> * De beschrijving die geeft informatie weer over de installatiekopie en de site is gebaseerd.
> * Bevat de entiteit classificatie met behulp van de hints weergeven indien van toepassing.
> * Bevat een koppeling naar een zoekopdracht Bing voor meer informatie over de entiteit.
> * Licentie- of toekenning informatie vereist voor de gegevensbronnen wordt weergegeven.

## <a name="persisting-client-id"></a>Persisting client-ID

Antwoorden van de Bing zoeken-API's, omvat mogelijk een `X-MSEdge-ClientID` header die moet worden verzonden naar de API met opeenvolgende aanvragen. Als er meerdere Bing zoeken-API's worden gebruikt, moet dezelfde client-ID worden gebruikt met deze, indien mogelijk.

Geven de `X-MSEdge-ClientID` header kan de Bing-API's om te koppelen van een gebruiker zoekopdrachten met twee belangrijke voordelen.

Ten eerste kunt u de Bing zoekmachine voorbij context toepassen op zoekopdrachten resultaten oplevert die beter voldoen aan de gebruiker. Als een gebruiker eerder naar voorwaarden die betrekking hebben op afvaart gezocht is, bijvoorbeeld een hoger zoek naar 'dock' mogelijk bij voorkeur informatie ophalen over locaties voor het koppelen van een zeilboot.

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
> [Bing entiteit Search API-verwijzingen](//docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)

> [!div class="nextstepaction"]
> [Bing kaarten-API-documentatie](//msdn.microsoft.com/library/dd877180.aspx)
