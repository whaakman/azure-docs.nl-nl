---
title: 'Zelfstudie: Afbeelding uploaden voor Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Hierin wordt het proces geanalyseerd waarmee een afbeelding naar Bing wordt geüpload om er inzichten over te verkrijgen en vervolgens het antwoord wordt geparseerd en weergegeven.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: scottwhi
ms.openlocfilehash: 0963c61027358c2c8e971533052631de28994b57
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471440"
---
# <a name="tutorial-upload-images-to-the-bing-visual-search-api"></a>Zelfstudie: Afbeeldingen uploaden naar de Bing visuele zoekopdrachten-API

Met de Bing Visual Search-API kunt u op internet zoeken naar afbeeldingen die overeenkomen met de afbeeldingen die u uploadt. Gebruik deze zelfstudie om een webtoepassing te maken waarmee een afbeelding naar de API kan worden verzonden en waarmee de inzichten die worden geretourneerd op de webpagina worden weergegeven. Deze toepassing voldoet niet aan alle [vereisten voor gebruik en weergave van Bing](../bing-web-search/use-display-requirements.md) voor het gebruik van de API.

U vindt de volledige broncode voor dit voorbeeld met extra foutafhandeling en aantekeningen op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchUploadImage.html).

In de zelfstudie-app leert u het volgende:

> [!div class="checklist"]
> * Een afbeelding uploaden naar de Bing Visual Search-API
> * Zoekresultaten voor afbeeldingen weergeven in een webtoepassing
> * De verschillende inzichten verkennen die worden geleverd door de API

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-structure-the-webpage"></a>De webpagina maken en structureren

Maak een HTML-pagina die een installatiekopie naar de Bing visuele zoekopdrachten-API verzendt, inzichten ontvangt en geeft deze weer. Maak een bestand met de naam "uploaddemo.html" in uw favoriete editor of IDE. De volgende HTML-basisstructuur toevoegen aan het bestand:

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Visual Search Upload Demo</title>
    </head>

    <body>
    </body>
</html>
```

De pagina onderverdelen in een aanvraag, waarin de gebruiker biedt alle gegevens die vereist zijn voor de aanvraag, en een antwoord sectie waarin de inzichten worden weergegeven. Voeg de volgende `<div>`-tags toe aan de `<body>`. De `<hr>` tag visueel worden gescheiden van de aanvraag-sectie van de antwoord-sectie:

```html
<div id="requestSection"></div>
<hr />
<div id="responseSection"></div>
```

Voeg een `<script>` tag aan de `<head>` tag JavaScript voor de toepassing bevat:

```html
<script>
<\script>
```

## <a name="get-the-upload-file"></a>Het uploadbestand ophalen

De toepassing gebruikt de `<input>`-tag met het kenmerk 'type' ingesteld op `file` om de gebruiker de te uploaden afbeelding te laten selecteren. De gebruikersinterface moet duidelijk maken dat de toepassing Bing gebruikt om de zoekresultaten te krijgen.

Voeg de volgende `<div>` naar de `requestSection` `<div>`. De bestandsinvoer accepteert één bestand, van elk afbeeldingstype (bijvoorbeeld .jpg, .gif, .png). De `onchange`-gebeurtenis geeft de handler aan die wordt aangeroepen wanneer een gebruiker een bestand selecteert.

De `<output>` invoercode wordt gebruikt om een miniatuur van de geselecteerde installatiekopie weer te geven:

```html
<div>
    <p>Select image to get insights from Bing:
        <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
    </p>

    <output id="thumbnail"></output>
</div>
```

## <a name="create-a-file-handler"></a>Een bestandshandler maken

Maak een handlerfunctie die kan worden gelezen in de afbeelding die u wilt uploaden. Tijdens het doorlopen van de bestanden in het `FileList`-object moet de handler ervoor zorgen dat het geselecteerde bestand een afbeeldingsbestand is van 1 MB of minder. Als de afbeelding groter is, moet u de grootte beperken voordat u deze uploadt. Ten slotte wordt de handler een miniatuur van de afbeelding:

```javascript
function handleFileSelect(selector) {

    var files = document.getElementById(selector).files; // A FileList object

    for (var i = 0, f; f = files[i]; i++) {

        // Ensure the file is an image file.
        if (!f.type.match('image.*')) {
            alert("Selected file must be an image file.");
            document.getElementById("uploadImage").value = null;
            continue;
        }

        // Image must be <= 1 MB and should be about 1500px.
        if (f.size > 1000000) {
            alert("Image must be less than 1 MB.");
            document.getElementById("uploadImage").value = null;
            continue;
        }

        var reader = new FileReader();

        // Capture the file information.
        reader.onload = (function(theFile) {
            return function(e) {
                var fileOutput = document.getElementById('thumbnail');

                if (fileOutput.childElementCount > 0) {
                    fileOutput.removeChild(fileOutput.lastChild);  // Remove the current pic, if it exists
                }

                // Render thumbnail.
                var span = document.createElement('span');
                span.innerHTML = ['<img class="thumb" src="', e.target.result,
                                    '" title="', escape(theFile.name), '"/>'].join('');
                fileOutput.insertBefore(span, null);
            };
        })(f);

        // Read in the image file as a data URL.
        reader.readAsDataURL(f);
    }
}
```

## <a name="add-and-store-a-subscription-key"></a>Een abonnementssleutel toevoegen en opslaan

De toepassing vereist een abonnementssleutel voor de Bing visuele zoekopdrachten-API-aanroepen. In deze zelfstudie geeft u deze op in de gebruikersinterface. Voeg de volgende `<input>` tag (met het typekenmerk is ingesteld op tekst) aan de `<body>` onder van het bestand `<output>` tag:

```html
    <div>
        <p>Subscription key: 
            <input type="text" id="key" name="subscription" size=40 maxlength="32" />
        </p>
    </div>
```

Aan de hand van de afbeelding en de abonnementssleutel kunt u Bing Visual Search aanroepen om inzicht te krijgen in de afbeelding. In deze zelfstudie de aanroep maakt gebruik van de standaard-markt (`en-us`) en de waarde van veilig zoeken (`moderate`).

Deze waarden kunnen in deze toepassing worden gewijzigd. Voeg de volgende `<div>` hieronder de abonnementssleutel `<div>`. De toepassing gebruikt een `<select>`-tag om een ​​vervolgkeuzelijst te bieden voor de waarden voor de markt en Veilig Zoeken. In beide lijsten wordt de standaardwaarde weergegeven.

```html
<div>
    <p><a href="#" onclick="expandCollapse(options.id)">Query options</a></p>

    <div id="options" style="display:none">
        <p style="margin-left: 20px">Market: 
            <select id="mkt">
                <option value="es-AR">Argentina (Spanish)</option>
                <option value="en-AU">Australia (English)</option>
                <option value="de-AT">Austria (German)</option>
                <option value="nl-BE">Belgium (Dutch)</option>
                <option value="fr-BE">Belgium (French)</option>
                <option value="pt-BR">Brazil (Portuguese)</option>
                <option value="en-CA">Canada (English)</option>
                <option value="fr-CA">Canada (French)</option>
                <option value="es-CL">Chile (Spanish)</option>
                <option value="da-DK">Denmark (Danish)</option>
                <option value="fi-FI">Finland (Finnish)</option>
                <option value="fr-FR">France (French)</option>
                <option value="de-DE">Germany (German)</option>
                <option value="zh-HK">Hong Kong (Traditional Chinese)</option>
                <option value="en-IN">India (English)</option>
                <option value="en-ID">Indonesia (English)</option>
                <option value="it-IT">Italy (Italian)</option>
                <option value="ja-JP">Japan (Japanese)</option>
                <option value="ko-KR">Korea (Korean)</option>
                <option value="en-MY">Malaysia (English)</option>
                <option value="es-MX">Mexico (Spanish)</option>
                <option value="nl-NL">Netherlands (Dutch)</option>
                <option value="en-NZ">New Zealand (English)</option>
                <option value="no-NO">Norway (Norwegian)</option>
                <option value="zh-CN">People's Republic of China (Chinese)</option>
                <option value="pl-PL">Poland (Polish)</option>
                <option value="pt-PT">Portugal (Portuguese)</option>
                <option value="en-PH">Philippines (English)</option>
                <option value="ru-RU">Russia (Russian)</option>
                <option value="ar-SA">Saudi Arabia (Arabic)</option>
                <option value="en-ZA">South Africa (English)</option>
                <option value="es-ES">Spain (Spanish)</option>
                <option value="sv-SE">Sweden (Swedish)</option>
                <option value="fr-CH">Switzerland (French)</option>
                <option value="de-CH">Switzerland (German)</option>
                <option value="zh-TW">Taiwan (Traditional Chinese)</option>
                <option value="tr-TR">Turkey (Turkish)</option>
                <option value="en-GB">United Kingdom (English)</option>
                <option value="en-US" selected>United States (English)</option>
                <option value="es-US">United States (Spanish)</option>
            </select>
        </p>
        <p style="margin-left: 20px">Safe search: 
            <select id="safesearch">
                <option value="moderate" selected>Moderate</option>
                <option value="strict">Strict</option>
                <option value="off">off</option>
            </select>
        </p>
    </div>
</div>
```

## <a name="add-search-options-to-the-webpage"></a>Zoekopties doorgeven aan de webpagina

De toepassing wordt ingeschakeld, wordt de lijsten in een samenvouwbare `<div>` die wordt beheerd door de koppeling van de Query-opties. Wanneer u de koppeling van de opties voor query's op de `<div>` wordt uitgebreid zodat u kunt bekijken en wijzigen van de opties voor query's. Als u de koppeling van de opties voor Query nogmaals, op de `<div>` samengevouwen en verborgen is. Het volgende fragment toont de Query-opties-koppeling `onclick` handler. De besturingselementen van de handler of de `<div>` is uitgevouwen of samengevouwen. Voeg deze handler toe aan de `<script>`-sectie. De handler wordt gebruikt door alle samenvouwbare `<div>` secties in de demo.

```javascript
// Contains the toggle state of divs.
var divToggleMap = {};  // divToggleMap['foo'] = 0;  // 1 = show, 0 = hide


// Toggles between showing and hiding the specified div.
function expandCollapse(divToToggle) {
    var div = document.getElementById(divToToggle);

    if (divToggleMap[divToToggle] == 1) {   // if div is expanded
        div.style.display = "none";
        divToggleMap[divToToggle] = 0;
    }
    else {                                  // if div is collapsed
        div.style.display = "inline-block";
        divToggleMap[divToToggle] = 1;
    }
}
```

## <a name="call-the-onclick-handler"></a>Roep de `onclick` handler

Voeg de volgende `"Get insights"` knop onder de opties `<div>` in de hoofdtekst. Met deze knop kunt u de aanroep initiëren. Wanneer de knop wordt geklikt, de cursor is gewijzigd in de cursor van de wacht zetten en de `onclick` handler wordt genoemd.

```html
<p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>
```

Toevoegen van de knop `onclick` -handler `handleQuery()` naar de `<script>` tag.

## <a name="handle-the-query"></a>De query verwerken

De handler `handleQuery()` zorgt ervoor dat de abonnementssleutel aanwezig is en is 32 tekens lang zijn en dat er een installatiekopie is geselecteerd. Verder worden eventuele inzichten uit een eerdere query gewist. Daarna wordt de `sendRequest()`-functie aangeroepen om de aanroep uit te voeren.

```javascript
function handleQuery() {
    var subscriptionKey = document.getElementById('key').value;

    // Make sure user provided a subscription key and image.
    // For this demo, the user provides the key but typically you'd
    // get it from secured storage.
    if (subscriptionKey.length !== 32) {
        alert("Subscription key length is not valid. Enter a valid key.");
        document.getElementById('key').focus();
        return;
    }

    var imagePath = document.getElementById('uploadImage').value;

    if (imagePath.length === 0)
    {
        alert("Please select an image to upload.");
        document.getElementById('uploadImage').focus();
        return;
    }

    var responseDiv = document.getElementById('responseSection');

    // Clear out the response from the last query.
    while (responseDiv.childElementCount > 0) {
        responseDiv.removeChild(responseDiv.lastChild);
    }

    // Send the request to Bing to get insights about the image.
    var f = document.getElementById('uploadImage').files[0];
    sendRequest(f, subscriptionKey);
}
```

## <a name="send-the-search-request"></a>De zoekaanvraag verzenden

De `sendRequest()` functie maakt de eindpunt-URL, stelt de `Ocp-Apim-Subscription-Key` koptekst naar de abonnementssleutel voegt het binaire bestand van de afbeelding te uploaden, Hiermee geeft u de antwoordhandler en Hiermee wordt de aanroep:

```javascript
function sendRequest(file, key) {
    var market = document.getElementById('mkt').value;
    var safeSearch = document.getElementById('safesearch').value;
    var baseUri = `https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=${market}&safesearch=${safeSearch}`;

    var form = new FormData();
    form.append("image", file);

    var request = new XMLHttpRequest();

    request.open("POST", baseUri);
    request.setRequestHeader('Ocp-Apim-Subscription-Key', key);
    request.addEventListener('load', handleResponse);
    request.send(form);
}
```

## <a name="get-and-handle-the-api-response"></a>De API-reactie ophalen en behandelen

De `handleResponse()`-functie behandelt het antwoord van de aanroep van Bing Visual Search. Als de aanroep slaagt, parseert deze functie de JSON-reactie in de afzonderlijke tags, die de inzichten bevatten. Vervolgens wordt de lijst met zoekresultaten toegevoegd aan de pagina. De toepassing maakt dan een samenvouwbare `<div>` voor elk label voor het beheren van hoeveel gegevens worden weergegeven. Voeg de handler toe aan de `<script>`-sectie.

```javascript
function handleResponse() {
    if(this.status !== 200){
        alert("Error calling Bing Visual Search. See console log for details.");
        console.log(this.responseText);
        return;
    }

    var tags = parseResponse(JSON.parse(this.responseText));
    var h4 = document.createElement('h4');
    h4.textContent = 'Bing internet search results';
    document.getElementById('responseSection').appendChild(h4);
    buildTagSections(tags);

    document.body.style.cursor = 'default'; // reset the wait cursor set by query insights button
}
```

### <a name="parse-the-response"></a>Het antwoord parseren

De `parseResponse`-functie converteert het JSON-antwoord naar een woordenboekobject door `json.tags` te doorlopen.

```javascript
function parseResponse(json) {
    var dict = {};

    for (var i =0; i < json.tags.length; i++) {
        var tag = json.tags[i];

        if (tag.displayName === '') {
            dict['Default'] = JSON.stringify(tag);
        }
        else {
            dict[tag.displayName] = JSON.stringify(tag);
        }
    }

    return(dict);
}
```

### <a name="build-a-tag-section"></a>Een tagsectie bouwen

De `buildTagSections()` functie doorloopt de geparseerde JSON-tags en oproepen de `buildDiv()` functie voor het bouwen een `<div>` voor elk label. Elke tag wordt weergegeven als een koppeling. Wanneer de gebruiker op de koppeling klikt, wordt de tag uitgevouwen en worden de inzichten weergegeven die aan de tag zijn gekoppeld. Op de koppeling opnieuw zorgt ervoor dat de sectie om samen te vouwen.

```javascript
function buildTagSections(tags) {
    for (var tag in tags) {
        if (tags.hasOwnProperty(tag)) {
            var tagSection = buildDiv(tags, tag);
            document.getElementById('responseSection').appendChild(tagSection);
        }
    }  
}

function buildDiv(tags, tag) {
    var tagSection = document.createElement('div');
    tagSection.setAttribute('class', 'subSection');

    var link = document.createElement('a');
    link.setAttribute('href', '#');
    link.setAttribute('style', 'float: left;')
    link.text = tag;
    tagSection.appendChild(link);

    var contentDiv = document.createElement('div');
    contentDiv.setAttribute('id', tag);
    contentDiv.setAttribute('style', 'clear: left;')
    contentDiv.setAttribute('class', 'section');
    tagSection.appendChild(contentDiv);

    link.setAttribute('onclick', `expandCollapse("${tag}")`);
    divToggleMap[tag] = 0;  // 1 = show, 0 = hide

    addDivContent(contentDiv, tag, tags[tag]);

    return tagSection;
}
```

## <a name="display-the-search-results-in-the-webpage"></a>De zoekresultaten in de webpagina weergeven

De `buildDiv()` functie roept de `addDivContent` functie voor het bouwen van de inhoud van elke tag de samenvouwbare `<div>`.

De inhoud van een tag omvat de JSON uit het antwoord voor de tag. In eerste instantie alleen de eerste 100 tekens van de JSON worden weergegeven, maar u kunt klikken op de JSON-tekenreeks om weer te geven van alle de JSON. Als u hier nogmaals op klikt, wordt de JSON-tekenreeks weer samengevouwen tot 100 tekens.

Voeg vervolgens de in de tag gevonden actietypen toe. Roep de desbetreffende functies om toe te voegen van de inzichten voor elk actietype:

```javascript
function addDivContent(div, tag, json) {

    // Adds the first 100 characters of the json that contains
    // the tag's data. The user can click the text to show the
    // full json. They can click it again to collapse the json.
    var para = document.createElement('p');
    para.textContent = String(json).substr(0, 100) + '...';
    para.setAttribute('title', 'click to expand');
    para.setAttribute('style', 'cursor: pointer;')
    para.setAttribute('data-json', json);
    para.addEventListener('click', function(e) {
        var json = e.target.getAttribute('data-json');

        if (e.target.textContent.length <= 103) {  // 100 + '...'
            e.target.textContent = json;
            para.setAttribute('title', 'click to collapse');
        }
        else {
            para.textContent = String(json).substr(0, 100) + '...';
            para.setAttribute('title', 'click to expand');
        }
    });
    div.appendChild(para); 

    var parsedJson = JSON.parse(json);

    // Loop through all the actions in the tag and display them.
    for (var j = 0; j < parsedJson.actions.length; j++) {
        var action = parsedJson.actions[j];

        var subSectionDiv = document.createElement('div');
        subSectionDiv.setAttribute('class', 'subSection');
        div.appendChild(subSectionDiv);

        var h4 = document.createElement('h4');
        h4.innerHTML = action.actionType;
        subSectionDiv.appendChild(h4);

        if (action.actionType === 'ImageResults') {
            addImageWithWebSearchUrl(subSectionDiv, parsedJson.image, action);
        }
        else if (action.actionType === 'DocumentLevelSuggestions') {
            addRelatedSearches(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'RelatedSearches') {
            addRelatedSearches(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'PagesIncluding') {
            addPagesIncluding(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'VisualSearch') {
            addRelatedImages(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'Recipes') {
            addRecipes(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'ShoppingSources') {
            addShopping(subSectionDiv, action.data.offers);
        }
        else if (action.actionType === 'ProductVisualSearch') {
            addProducts(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'TextResults') {
            addTextResult(subSectionDiv, action);
        }
        else if (action.actionType === 'Entity') {
            addEntity(subSectionDiv, action);
        }
    }
}
```

## <a name="display-insights-for-different-actions"></a>Inzichten voor verschillende acties weergeven

De volgende functies geven inzichten voor verschillende acties weer. De functies bieden een afbeelding of koppeling waarop kan worden geklikt om naar een webpagina te gaan met meer informatie over de afbeelding. Deze pagina wordt gehost door Bing.com of de oorspronkelijke website van de afbeelding. Niet alle gegevens van de inzichten worden in deze toepassing weergegeven. Alle velden die beschikbaar zijn voor een beter inzicht, Zie de [installatiekopieën - visuele zoekopdrachten](https://aka.ms/bingvisualsearchreferencedoc) verwijzing.

> [!NOTE]
> Op de pagina moet een minimale hoeveelheid inzichtgegevens worden weergegeven. Zie de [Bing zoeken-API gebruiken en weergavevereisten](../bing-web-search/use-display-requirements.md) voor meer informatie.

### <a name="relatedimages-insights"></a>RelatedImages-inzichten

De `addRelatedImages()` functie maakt een titel voor elk van de websites die als host fungeert de gerelateerde afbeeldingen door de lijst met doorlopen `RelatedImages` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript
    function addRelatedImages(div, images) {
        var length = (images.length > 10) ? 10 : images.length;

        // Set the title to the website that hosts the image. The title displays
        // when the user hovers over the image.

        // Make the image clickable. If the user clicks the image, they're taken
        // to the image in Bing.com.

        for (var j = 0; j < length; j++) {
            var img = document.createElement('img');
            img.setAttribute('src', images[j].thumbnailUrl + '&w=120&h=120');
            img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
            img.setAttribute('title', images[j].hostPageDisplayUrl);
            img.setAttribute('data-webSearchUrl', images[j].webSearchUrl)

            img.addEventListener('click', function(e) {
                var url = e.target.getAttribute('data-webSearchUrl');
                window.open(url, 'foo');
            })

            div.appendChild(img);
        }
    }
```

### <a name="pagesincluding-insights"></a>PagesIncluding-inzichten

De `addPagesIncluding()` functie wordt een koppeling gemaakt voor elk van de websites die als host fungeert de geüploade afbeelding door te doorlopen van de lijst met `PagesIncluding` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript

    // Display links to the first 5 webpages that include the image.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addPagesIncluding(div, pages) {
        var length = (pages.length > 5) ? 5 : pages.length;

        for (var j = 0; j < length; j++) {
            var page = document.createElement('a');
            page.text = pages[j].name;
            page.setAttribute('href', pages[j].hostPageUrl);
            page.setAttribute('style', 'margin: 20px 20px 0 0');
            page.setAttribute('target', '_blank')
            div.appendChild(page);

            div.appendChild(document.createElement('br'));
        }
    }
```

### <a name="relatedsearches-insights"></a>RelatedSearches-inzichten

De `addRelatedSearches()` functie maakt een koppeling voor de website die als host fungeert de installatiekopie door het doorlopen van de lijst met `RelatedSearches` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript

    // Display the first 10 related searches. Include a link with the image
    // that when clicked, takes the user to Bing.com and displays the 
    // related search results.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addRelatedSearches(div, relatedSearches) {
        var length = (relatedSearches.length > 10) ? 10 : relatedSearches.length;

        for (var j = 0; j < length; j++) {
            var childDiv = document.createElement('div');
            childDiv.setAttribute('class', 'stackLink');
            div.appendChild(childDiv);

            var img = document.createElement('img');
            img.setAttribute('src', relatedSearches[j].thumbnail.url + '&w=120&h=120');
            img.setAttribute('style', 'margin: 20px 20px 0 0;');
            childDiv.appendChild(img);

            var relatedSearch = document.createElement('a');
            relatedSearch.text = relatedSearches[j].displayText;
            relatedSearch.setAttribute('href', relatedSearches[j].webSearchUrl);
            relatedSearch.setAttribute('target', '_blank');
            childDiv.appendChild(relatedSearch);

        }
    }
```

### <a name="recipes-insights"></a>Recepten-inzichten

De `addRecipes()` functie wordt een koppeling gemaakt voor elk van die wordt geretourneerd door de lijst met doorlopen recepten `Recipes` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript
    // Display links to the first 10 recipes. Include the recipe's rating,
    // if available.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addRecipes(div, recipes) {
        var length = (recipes.length > 10) ? 10 : recipes.length;

        for (var j = 0; j < length; j++) {
            var para = document.createElement('p');

            var recipe = document.createElement('a');
            recipe.text = recipes[j].name;
            recipe.setAttribute('href', recipes[j].url);
            recipe.setAttribute('style', 'margin: 20px 20px 0 0');
            recipe.setAttribute('target', '_blank')
            para.appendChild(recipe);

            if (recipes[j].hasOwnProperty('aggregateRating')) {
                var span = document.createElement('span');
                span.textContent = 'rating: ' + recipes[j].aggregateRating.text;
                para.appendChild(span);
            }

            div.appendChild(para);
        }
    }
```

### <a name="shopping-insights"></a>Winkel-inzichten

De `addShopping()` functie maakt u een koppeling voor een winkelwagen resultaten geretourneerd door het doorlopen van de lijst met `RelatedImages` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript
    // Display links for the first 10 shopping offers.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addShopping(div, offers) {
        var length = (offers.length > 10) ? 10 : offers.length;

        for (var j = 0; j < length; j++) {
            var para = document.createElement('p');

            var offer = document.createElement('a');
            offer.text = offers[j].name;
            offer.setAttribute('href', offers[j].url);
            offer.setAttribute('style', 'margin: 20px 20px 0 0');
            offer.setAttribute('target', '_blank')
            para.appendChild(offer);

            var span = document.createElement('span');
            span.textContent = 'by ' + offers[j].seller.name + ' | ' + offers[j].price + ' ' + offers[j].priceCurrency;
            para.appendChild(span);

            div.appendChild(para);
        }
    }
```

### <a name="products-insights"></a>Product-inzichten

De `addProducts()` functie wordt een koppeling gemaakt voor alle producten resultaten geretourneerd door het doorlopen van de lijst met `Products` acties en toe te voegen een `<img>` tag aan de buitenkant `<div>` voor elk:

```javascript

    // Display the first 10 related products. Display a clickable image of the
    // product that takes the user to Bing.com search results for the product.
    // If there are any offers associated with the product, provide links to the offers.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addProducts(div, products) {
        var length = (products.length > 10) ? 10 : products.length;

        for (var j = 0; j < length; j++) {
            var childDiv = document.createElement('div');
            childDiv.setAttribute('class', 'stackLink');
            div.appendChild(childDiv);

            var img = document.createElement('img');
            img.setAttribute('src', products[j].thumbnailUrl + '&w=120&h=120');
            img.setAttribute('title', products[j].name);
            img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
            img.setAttribute('data-webSearchUrl', products[j].webSearchUrl)
            img.addEventListener('click', function(e) {
                var url = e.target.getAttribute('data-webSearchUrl');
                window.open(url, 'foo');
            })
            childDiv.appendChild(img);

            if (products[j].insightsMetadata.hasOwnProperty('aggregateOffer')) {
                if (products[j].insightsMetadata.aggregateOffer.offerCount > 0) {
                    var offers = products[j].insightsMetadata.aggregateOffer.offers;

                    // Show all the offers. Not all markets provide links to offers.
                    for (var i = 0; i < offers.length; i++) {  
                        var para = document.createElement('p');

                        var offer = document.createElement('a');
                        offer.text = offers[i].name;
                        offer.setAttribute('href', offers[i].url);
                        offer.setAttribute('style', 'margin: 20px 20px 0 0');
                        offer.setAttribute('target', '_blank')
                        para.appendChild(offer);

                        var span = document.createElement('span');
                        span.textContent = 'by ' + offers[i].seller.name + ' | ' + offers[i].price + ' ' + offers[i].priceCurrency;
                        para.appendChild(span);

                        childDiv.appendChild(para);
                    }
                }
                else {  // Otherwise, just show the lowest price that Bing found.
                    var offer = products[j].insightsMetadata.aggregateOffer;

                    var para = document.createElement('p');
                    para.textContent = `${offer.name} | ${offer.lowPrice} ${offer.priceCurrency}`; 

                    childDiv.appendChild(para);
                }
            }
        }
    }
```

### <a name="textresult-insights"></a>TextResult-inzichten

De `addTextResult()` functie wordt de tekst die wordt herkend in de afbeelding weergegeven:

```javascript

    function addTextResult(div, action) {
        var text = document.createElement('p');
        text.textContent = action.displayName;
        div.appendChild(text);
    }
```

De `addEntity()` functie wordt een koppeling die de gebruiker op Bing.com waar ze meer informatie over het entiteitstype kunnen krijgen in de afbeelding als een is gedetecteerd:

```javascript
    // If the image is of a person, the tag might include an entity
    // action type. Display a link that takes the user to Bing.com
    // where they can get details about the entity.
    function addEntity(div, action) {
        var entity = document.createElement('a');
        entity.text = action.displayName;
        entity.setAttribute('href', action.webSearchUrl);
        entity.setAttribute('style', 'margin: 20px 20px 0 0');
        entity.setAttribute('target', '_blank');
        div.appendChild(entity);
    }
```

De `addImageWithWebSearchUrl()` functie geeft een afbeelding naar de `<div>` die leidt de gebruiker om te zoeken naar resultaten in op Bing.com:

```javascript
    function addImageWithWebSearchUrl(div, image, action) {
        var img = document.createElement('img');
        img.setAttribute('src', image.thumbnailUrl + '&w=120&h=120');
        img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
        img.setAttribute('data-webSearchUrl', action.webSearchUrl);
        img.addEventListener('click', function(e) {
            var url = e.target.getAttribute('data-webSearchUrl');
            window.open(url, 'foo');
        })
        div.appendChild(img);
    }

```

## <a name="add-a-css-style"></a>Een CSS-stijl toevoegen

Voeg de volgende `<style>` sectie aan de `<head>` tag voor de indeling van de webpagina:

```html
        <style>

            .thumb {
                height: 75px;
                border: 1px solid #000;
            }

            .stackLink {
                width:180px;
                min-height:210px;
                display:inline-block;
            }
            .stackLink a {
                float:left;
                clear:left;
            }

            .section {
                float:left;
                display:none;
            }

            .subSection {
                clear:left;
                float:left;
            }

        </style>
```

## <a name="next-steps"></a>Volgende stappen

>[!div class="nextstepaction"]
> [Zelfstudie: Vergelijkbare afbeeldingen uit eerdere zoekacties met behulp van ImageInsightsToken zoeken](./tutorial-visual-search-insights-token.md)
