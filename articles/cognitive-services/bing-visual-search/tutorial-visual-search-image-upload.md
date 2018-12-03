---
title: 'Zelfstudie: afbeelding uploaden voor Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Hierin wordt het proces geanalyseerd waarmee een afbeelding naar Bing wordt geüpload om er inzichten over te verkrijgen en vervolgens het antwoord wordt geparseerd en weergegeven.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: scottwhi
ms.openlocfilehash: b13f2e5c96ba50fc76d7bfae256e5fcc1c8b56ce
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443454"
---
# <a name="tutorial-breaking-down-bing-visual-search-upload"></a>Zelfstudie: uploaden naar Bing Visual Search analyseren

In deze zelfstudie wordt het proces van het uploaden van een afbeelding naar Bing en het terugkrijgen van inzichten geanalyseerd. Ook ziet u hoe u toegang kunt krijgen tot de inzichten in het JSON-antwoord en deze weergeven. Zie voor het volledige HTML- en JavaScript-voorbeeld de [volledige code](#complete-code).

Deze zelfstudie is bedoeld voor de ontwikkelaar die de inhoud van het antwoord van Bing Visual Search wil verkennen. Er wordt niet voldaan aan alle gebruiks- en weergavevereisten (er wordt bijvoorbeeld geen koppeling naar het privacybeleid van Microsoft gemaakt). Zie [Bing Use and Display Requirements](./use-and-display-requirements.md) (vereisten voor gebruik en weergave) voor alle gebruiksvereisten.

Voor deze zelfstudie moet u een abonnement in de prijscategorie S9 starten, zoals wordt weergegeven in [Prijsinformatie Cognitive Services Bing Zoeken-API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Zo start u een abonnement in de Azure-portal:
1. Typ 'BingSearchV7' in het tekstvak bovenin de Azure-portal waar `Search resources, services, and docs` wordt weergegeven.  
2. Selecteer `Bing Search v7` in de vervolgkeuzelijst onder Marketplace.
3. Voer `Name` in voor de nieuwe resource.
4. Selecteer `Pay-As-You-Go`-abonnement.
5. Selecteer prijscategorie `S9`.
6. Klik op `Enable` om het abonnement te starten.

## <a name="where-to-start"></a>Waar beginnen?

Laten we beginnen met een HTML-pagina die Bing een afbeelding stuurt en inzichten terugkrijgt en weergeeft. Maak in uw favoriete editor een bestand met de naam uploaddemo.html. Voeg de volgende eenvoudige HTML-structuur toe aan het bestand.

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

Om te beginnen verdelen we de pagina in een aanvraagsectie, waar de gebruiker alle informatie verschaft die nodig is om de aanvraag te doen, en een antwoordsectie waar de inzichten worden weergegeven. Voeg de volgende \<div\> -tags toe aan de \<body\>. De \<hr\>-tag scheidt de aanvraagsectie visueel van de antwoordsectie.

```html
        <div id="requestSection"></div>

        <hr />
        
        <div id="responseSection"></div>
```

## <a name="get-the-file-to-upload"></a>Het te uploaden bestand ophalen

Om de gebruiker de te uploaden afbeelding te laten selecteren, gebruikt de demo de \<input\>-tag met het kenmerk ‘type’ ingesteld op ‘file’ (bestand). De gebruikersinterface moet duidelijk maken dat de demo Bing gebruikt om de zoekresultaten te krijgen. 

Voeg de volgende \<div\> toe aan de div requestSection. De bestandsinvoer accepteert één bestand, van elk afbeeldingstype (bijvoorbeeld .jpg, .gif, .png). De `onchange`-gebeurtenis geeft de handler aan die wordt aangeroepen wanneer een gebruiker een bestand selecteert.

De \<output\>-tag wordt gebruikt om een ​​miniatuur van de geselecteerde afbeelding weer te geven.


```html
            <div>
                <p>Select image to get insights from Bing:
                    <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
                </p>

                <output id="thumbnail"></output>
            </div>
```

Voordat u de handler toevoegt, voegt u een \<script\>-tag toe aan de \<head\>-tag.

```html
        <script>
        <\script>
```

Hieronder ziet u de handler waarmee de geselecteerde afbeelding wordt vastgelegd. De handler bevat logica om ervoor te zorgen dat het geselecteerde bestand een afbeeldingsbestand is en dat de grootte 1 MB of minder is. U kunt de gebruiker toestaan ​​om grotere bestanden te selecteren, maar u moet de afbeeldingsgrootte verkleinen tot minder dan 1 MB voordat u deze uploadt naar Bing. Het laatste dat de handler doet, is een miniatuur van de afbeelding weergeven, zodat de gebruiker een visuele herinnering heeft aan het bestand dat deze heeft geselecteerd.

```javascript
        function handleFileSelect(selector) {
            var files = document.getElementById(selector).files; // FileList object

            // The list will contain only one file.
            for (var i = 0, f; f = files[i]; i++) {
      
                // Only process image files.
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


## <a name="what-else-is-needed-before-making-the-call-to-bing"></a>Wat is er nog meer nodig voor de aanroep van Bing?

De demo heeft nog een abonnementssleutel nodig. In de praktijk haalt u de abonnementssleutel waarschijnlijk op uit beveiligde opslag, maar om deze demo eenvoudig te houden, moet u de sleutel opgeven in de gebruikersinterface. Voeg de volgende \<input\>-tag (met het kenmerk ‘type’ ingesteld op ‘text’) toe aan de \<body\>, vlak onder de \<output\>-tag van het bestand.

```html
        <div>
            <p>Subscription key: 
                <input type="text" id="key" name="subscription" size=40 maxlength="32" />
            </p>
        </div>
```

Aan de hand van de afbeelding en de abonnementssleutel kunt u Bing Visual Search aanroepen om inzicht te krijgen in de afbeelding. De aanroep zou de standaardwaarden voor de markt en Veilig Zoeken gebruiken (respectievelijk ‘en-us’ en ‘moderate’ (gemiddeld)).

Deze demo biedt de gebruiker de mogelijkheid om deze waarden te wijzigen. Voeg het volgende \<div\>-element toe onder de div voor de abonnementssleutel. De demo gebruikt een \<select\>-tag om een ​​vervolgkeuzelijst te bieden voor de waarden voor de markt en Veilig Zoeken. In beide lijsten wordt de standaardwaarde van Bing weergegeven.

 
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

De demo verbergt de lijsten in een samenvouwbaar div-element dat wordt bestuurd met de koppeling ‘Query options’ (zoekopties). Wanneer u op de koppeling ‘Query options’ klikt, wordt de div uitgevouwen, zodat u de zoekopties kunt zien en wijzigen. Als u nogmaals op de koppeling ‘Query options’ klikt, wordt het div-element samengevouwen en verborgen. Hieronder ziet u de onclick-handler van ‘Query options’. De handler bepaalt of het div-element wordt uitgevouwen of samengevouwen. Voeg deze handler toe aan de \<script\>-sectie. De handler wordt gebruikt door alle samenvouwbare div-elementen in de demo.

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


## <a name="making-the-call"></a>De aanroep doen

Voeg de volgende knop ‘Get insights’ (inzichten ophalen) toe onder het div-element met opties in de body. Met deze knop kan de gebruiker de aanroep doen. Wanneer de gebruiker op de knop klikt, verandert de cursor in de draaiende wachtcursor en wordt de onclick-handler aangeroepen.

```html
        <p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>
```

Voeg de onclick-handler van de knop toe in de \<script\>-tag. De handler controleert of de abonnementssleutel aanwezig is en 32 tekens lang is, en of er een afbeelding is geselecteerd. Verder worden eventuele inzichten uit een eerdere query gewist. Als alles goed is, wordt de functie sendRequest aangeroepen om de aanroep te doen.

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

De sendRequest-functie deelt de eindpunt-URL in, stelt de header Ocp-Apim-Subscription-Key in op de abonnementssleutel, voegt het binaire bestand van de afbeelding toe om te uploaden, geeft de antwoordhandler aan en voert de aanroep uit. 

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

## <a name="handling-the-response"></a>Het antwoord verwerken

De handleResponse-functie behandelt het antwoord van de aanroep van Bing Visual Search. Als de aanroep slaagt, parseert deze functie de JSON-reactie in de afzonderlijke tags, die de inzichten bevatten. Vervolgens wordt de tekenreeks ‘Bing internet search results’ (internetzoekresultaten van Bing) toegevoegd aan de pagina om de gebruiker te laten weten dat de gegevens afkomstig zijn van Bing.

De demo kan alle inzichten op de pagina dumpen, maar sommige afbeeldingen retourneren veel gegevens, waardoor ze moeilijk te gebruiken zijn. In plaats daarvan maakt de demo een samenvouwbaar div-element voor elke tag, zodat de gebruiker kan bepalen hoeveel gegevens deze wil zien.

Voeg de handler toe aan de \<script\>-sectie.

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

            document.body.style.cursor = 'default'; // reset the wait curor set by query insights button
        }

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

De functie buildTagSections doorloopt de geparseerde JSON-tags en roept de buildDiv-functie aan om een ​​div-element samen te stellen voor elke tag. Net als bij de optie Query, wordt elke tag weergegeven als een koppeling. Wanneer de gebruiker op de koppeling klikt, wordt de tag uitgevouwen en worden de inzichten weergegeven die aan de tag zijn gekoppeld. Als de gebruiker nogmaals op de koppeling klikt, wordt de sectie samengevouwen en worden de inzichten voor de gebruiker verborgen.

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

De buildDiv-functie roept de functie addDivContent aan om de inhoud van het samenvouwbare div-element voor elke tag op te bouwen.

De inhoud van een tag omvat de JSON uit het antwoord voor de tag. De demo bevat de JSON voor ontwikkelaars die de JSON achter het antwoord willen zien. Aanvankelijk worden alleen de eerste 100 tekens van de JSON weergegeven, maar u kunt op de JSON-tekenreeks klikken om alle JSON te tonen. Als u hier nogmaals op klikt, wordt de JSON-tekenreeks weer samengevouwen tot 100 tekens.

Voeg vervolgens de in de tag gevonden actietypen toe. Roep voor elk actietype de verschillende functies aan om de inzichten ervan toe te voegen.

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

Hieronder volgen alle functies waarmee inzichten voor de verschillende acties worden weergegeven. De meeste van deze functies zijn ongecompliceerd; ze bieden een afbeelding of koppeling waarop kan worden geklikt om de gebruiker naar een webpagina te sturen waar meer informatie over de afbeelding beschikbaar is (Bing.com of de webpagina waar de afbeelding wordt gehost). De zelfstudie geeft niet alle gegevens weer die aan het inzicht zijn gekoppeld. Zie [Bing Visual Search Reference](https://aka.ms/bingvisualsearchreferencedoc) om alle beschikbare velden voor een inzicht te bekijken.

Onthoud dat er een minimale hoeveelheid gegevens is die u moet weergeven; de rest is aan u. Zie [Bing Use and Display Requirements](./use-and-display-requirements.md) (vereisten voor gebruik en weergave) om ervoor te zorgen dat u aan de voorwaarden voldoet.


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


        // If Bing recognized any text in the image, display the text.
        function addTextResult(div, action) {
            var text = document.createElement('p');
            text.textContent = action.displayName;
            div.appendChild(text);
        }


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


        // Adds a clickable image to the div that takes the user to
        // Bing.com search results.
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



## <a name="adding-styles-to-make-the-page-display-correctly"></a>Stijlen toevoegen om de pagina correct weer te geven

Voeg de volgende \<style\>-sectie toe aan de \<head\>-tag.

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



## <a name="complete-code"></a>Volledige code

Hier is het volledige HTML- en JavaScript-voorbeeld.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Bing Visual Search Demo</title>
        
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

        <script>

            // Handles the user's file selection. The file input button specifies this handler.
            function handleFileSelect(selector) {
                var files = document.getElementById(selector).files; // FileList object

                // The list will contain only one file.
                for (var i = 0, f; f = files[i]; i++) {
        
                    // Only process image files.
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


            // Called when the user clicks the Query insights button.
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


            // Format the request and send it.
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


            // Handles the response from Bing. Parses the response and 
            // the tag divs.
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

                document.body.style.cursor = 'default'; // reset the wait curor set by query insights button
            }


            // Parses the json response by tags.
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


            // Builds divs fro each tag in the response.
            function buildTagSections(tags) {
                for (var tag in tags) {
                    if (tags.hasOwnProperty(tag)) {
                        var tagSection = buildDiv(tags, tag);
                        document.getElementById('responseSection').appendChild(tagSection);
                    }
                }  
            }


            // Builds the div for the specified tag. The div is shown as a
            // link that when clicked, expands or collapses. The divs are 
            // initially collapsed.
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


            // Adds the tag's action types to the div.
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


            // Display the first 10 related images.
            // TODO: Add 'more' link in case the user wants to see all of them.
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


            // If Bing recognized any text in the image, display the text.
            function addTextResult(div, action) {
                var text = document.createElement('p');
                text.textContent = action.displayName;
                div.appendChild(text);
            }


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


            // Adds a clickable image to the div that takes the user to
            // Bing.com search results.
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

        </script>

    </head>

    <body>

        <div id="requestSection">
            <div>
                <p>Select image to get insights from Bing:
                    <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
                </p>

                <output id="thumbnail"></output>
            </div>

            <div>
                <p>Subscription key: 
                    <input type="text" id="key" name="subscription" size=40 maxlength="32" />
                </p>
            </div>

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
        </div>
        
        <p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>

        <hr />
        
        <div id="responseSection"></div>
    </body>
</html>      
```

## <a name="next-steps"></a>Volgende stappen

Zie de [zelfstudie over Bing Visual Search SDK ImageInsightsToken](.\tutorial-visual-search-insights-token.md) om te zien hoe het ophalen van inzichten met een inzichttoken werkt.