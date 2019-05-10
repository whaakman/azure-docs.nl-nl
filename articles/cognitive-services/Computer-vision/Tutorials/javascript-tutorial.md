---
title: Bewerkingen op een afbeelding uitvoeren - JavaScript
titlesuffix: Azure Cognitive Services
description: Verken een eenvoudige JavaScript-app die gebruikmaakt van de Computer Vision-API in Azure Cognitive Services. Voer OCR uit, maak miniaturen en werk met visuele kenmerken in een afbeelding.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 91af70406590ab8e65a5d4a4b53835e9e4d4ed2a
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231662"
---
# <a name="use-computer-vision-features-with-the-rest-api-and-javascript"></a>Computer Vision-functies gebruiken met de REST-API en JavaScript

Deze handleiding bevat de functies van de Azure Cognitive Services-Computer Vision-REST-API.

Verken een JavaScript-toepassing die de Computer Vision-REST API gebruikt om optische tekenherkenning (OCR) uit te voeren, slim bijgesneden miniaturen te maken en visuele kenmerken, inclusief gezichten, in een afbeelding te detecteren, categoriseren, labelen en beschrijven. In dit voorbeeld kunt u een afbeeldings-URL verzenden voor analyse en verwerking. U kunt dit open source-voorbeeld gebruiken als sjabloon voor het maken van uw eigen JavaScript-app voor gebruik met de Computer Vision-REST API.

De JavaScript-formuliertoepassing is al geschreven, maar heeft geen Computer Vision-functionaliteit. In deze handleiding kunt u de specifieke code toevoegen aan de Computer Vision-REST-API voor het voltooien van de functionaliteit van de toepassing.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

U kunt de stappen in deze handleiding met een eenvoudige teksteditor.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Abonneren op Computer Vision-API en een abonnementssleutel ophalen

Voordat u het voorbeeld maakt, moet u zich abonneren op de Computer Vision-API, die deel uitmaakt van Azure Cognitive Services. Zie [Abonnementen](https://azure.microsoft.com/try/cognitive-services/) voor gedetailleerde informatie over het abonnement en sleutelbeheer. De primaire en secundaire sleutels zijn geldig voor gebruik in deze handleiding.

## <a name="acquire-incomplete-tutorial-project"></a>In deze les onvolledig verkrijgen

### <a name="download-the-project"></a>Het project downloaden

Kloon de [zelfstudie Cognitive Services JavaScript Computer Vision](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial) of download het zipbestand en pak het uit in een lege map.

Als u liever het gebruik van het voltooide project met alle zelfstudie code hebt toegevoegd, kunt u de bestanden in de **voltooid** map.

## <a name="add-tutorial-code-to-the-project"></a>Zelfstudie code toevoegen aan het project

De JavaScript-toepassing is ingesteld met zes HTML-bestanden, één voor elke functie. Elk bestand ziet u een andere functie van de Computer Vision (analyseren, OCR, enz.). De zes gedeeltes beschikt niet over de afhankelijkheden, zodat u de zelfstudie code aan een bestand, alle zes bestanden of een aantal bestanden toevoegen kunt is. En u kunt de code van de zelfstudie in een willekeurige volgorde toevoegen.

### <a name="analyze-an-image"></a>Een afbeelding analyseren

De functie analyseren van Computer Vision-scant een afbeelding voor duizenden herkenbare objecten, levende dingen, landschappen en acties. Zodra de analyse is voltooid, retourneert Analyseren een JSON-object dat de afbeelding beschrijft met labels, kleuranalyse, bijschriften en meer.

Voor het voltooien van de functie analyseren van de toepassing, moet u de volgende stappen uitvoeren:

#### <a name="add-the-event-handler-code-for-the-analyze-button"></a>Voeg de code van de gebeurtenis-handler voor de knop analyseren

Open het bestand **analyze.html** in een teksteditor en zoek de functie **analyzeButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **analyzeButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **AnalyzeImage** aangeroepen voor analyse van de afbeelding. Kopieer en plak de volgende code in de functie **analyzeButtonClick**.

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

Met de functie **AnalyzeImage** wordt de REST-API-aanroep voor analyse van de afbeelding verpakt. Nadat de analyseresultaten zijn geretourneerd, wordt de opgemaakte JSON-analyse weergegeven in de opgegeven textarea en het bijschrift wordt weergegeven in de opgegeven span.

Kopieer de **AnalyzeImage**-functiecode en plak deze direct onder de functie **analyzeButtonClick**.

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-analyze-function"></a>De functie analyseren uitvoeren

Sla het bestand **analyze.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding die u wilt analyseren en klik vervolgens op de knop **Afbeelding analyseren** om de afbeelding te analyseren en het resultaat te bekijken.

### <a name="recognize-a-landmark"></a>Een oriëntatiepunten herkennen

Met de functie Oriëntatiepunten van Computer Vision wordt een afbeelding geanalyseerd op natuurlijke en kunstmatige oriëntatiepunten, zoals bergen of beroemde gebouwen. Wanneer de analyse is voltooid, retourneert de functie Oriëntatiepunt een JSON-object waarin de in de afbeelding gevonden oriëntatiepunten zijn geïdentificeerd.

Voor het voltooien van de functie oriëntatiepunten van de toepassing, moet u de volgende stappen uitvoeren:

#### <a name="add-the-event-handler-code-for-the-landmark-button"></a>De code van de gebeurtenis-handler voor de knop oriëntatiepunten toevoegen

Open het bestand **landmark.html** in een teksteditor en zoek de functie **landmarkButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **landmarkButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **IdentifyLandmarks** aangeroepen voor analyse van de afbeelding. Kopieer en plak de volgende code in de functie **landmarkButtonClick**.

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

Met de functie **IdentifyLandmarks** wordt de REST-API-aanroep voor analyse van de afbeelding verpakt. Nadat de analyseresultaten zijn geretourneerd, wordt de opgemaakte JSON-analyse weergegeven in de opgegeven textarea en het bijschrift wordt weergegeven in de opgegeven span.

Kopieer de **IdentifyLandmarks**-functiecode en plak deze direct onder de functie **landmarkButtonClick**.

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-landmark-function"></a>De functie oriëntatiepunt uitvoeren

Sla het bestand **landmark.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding die u wilt analyseren en klik vervolgens op de knop **Afbeelding analyseren** om de afbeelding te analyseren en het resultaat te bekijken.

### <a name="recognize-celebrities"></a>Beroemdheden herkennen

De functie Beroemdheden van de Computer Vision analyseert een afbeelding op beroemde mensen. Wanneer de analyse is voltooid, retourneert de functie Beroemdheden een JSON-object waarin de in de afbeelding gevonden beroemdheden zijn geïdentificeerd.

Als u wilt de beroemdheden-functie van de toepassing hebt voltooid, moet u de volgende stappen uitvoeren:

#### <a name="add-the-event-handler-code-for-the-celebrities-button"></a>De code van de gebeurtenis-handler voor de beroemdheden-knop

Open het bestand **celebrities.html** in een teksteditor en zoek de functie **celebritiesButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **celebritiesButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **IdentifyCelebrities** aangeroepen voor analyse van de afbeelding. Kopieer en plak de volgende code in de functie **celebritiesButtonClick**.

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-celebrities-function"></a>Voer de beroemdheden-functie

Sla het bestand **celebrities.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding die u wilt analyseren en klik vervolgens op de knop **Afbeelding analyseren** om de afbeelding te analyseren en het resultaat te bekijken.

### <a name="intelligently-generate-a-thumbnail"></a>Intelligent een miniatuur genereren

Met de functie Miniatuur van Computer Vision wordt een miniatuur van een afbeelding gegenereerd. De functie Miniatuur identificeert het interessantste gebied in een afbeelding en centreert de miniatuur op dit gebied. Vervolgens wordt de functie **Slim bijsnijden** gebruikt om een zo fraai mogelijke miniatuurafbeelding te genereren.

Als u wilt de miniatuur-functie van de toepassing hebt voltooid, moet u de volgende stappen uitvoeren:

#### <a name="add-the-event-handler-code-for-the-thumbnail-button"></a>De code van de gebeurtenis-handler voor de miniatuur knop toevoegen

Open het bestand **thumbnail.html** in een teksteditor en zoek de functie **thumbnailButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **thumbnailButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **getThumbnail** aangeroepen. Deze functie wordt twee keer aangeroepen voor het maken van twee miniaturen, een die is gemaakt met slim bijsnijden en een zonder. Kopieer de volgende code en plak deze in de functie **thumbnailButtonClick**.

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

Met de functie **getThumbnail** wordt de REST-API-aanroep voor analyse van de afbeelding verpakt. Nadat de analyseresultaten zijn geretourneerd, wordt de miniatuur weergegeven in het opgegeven img-element.

Kopieer de volgende **getThumbnail**-functie en plak deze direct onder de functie **thumbnailButtonClick**.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumbnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

#### <a name="run-the-thumbnail-function"></a>De miniaturen functie uitvoeren

Sla het bestand **thumbnail.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding die u wilt analyseren en klik vervolgens op de knop **Miniaturen analyseren** om de afbeelding te analyseren en het resultaat te bekijken.

### <a name="read-printed-text-ocr"></a>Gedrukte tekst lezen (OCR)

De functie voor optische tekenherkenning (OCR) van Computer Vision analyseert een afbeelding van gedrukte tekst. Nadat de analyse is voltooid is, retourneert OCR een JSON-object met de tekst en de locatie van de tekst in de afbeelding.

Als u wilt de OCR-functie van de toepassing hebt voltooid, moet u de volgende stappen uitvoeren:

### <a name="add-the-event-handler-code-for-the-ocr-button"></a>De code van de gebeurtenis-handler voor de OCR-knop

Open het bestand **ocr.html** in een teksteditor en zoek de functie **ocrButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **ocrButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **ReadOcrImage** aangeroepen voor analyse van de afbeelding. Kopieer en plak de volgende code in de functie **ocrButtonClick**.

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

Met de functie **ReadOcrImage** wordt de REST-API-aanroep voor analyse van de afbeelding verpakt. Nadat de analyseresultaten zijn geretourneerd, wordt de opgemaakte JSON met de tekst en de locatie van de tekst weergegeven in de opgegeven textarea.

Kopieer de volgende **ReadOcrImage**-functie en plak deze direct onder de functie **ocrButtonClick**.

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-ocr-function"></a>De functie OCR uitvoeren

Sla het bestand **ocr.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding van tekst die moet worden gelezen en klik vervolgens op de knop **Afbeelding lezen** om de afbeelding te analyseren en het resultaat te bekijken.

### <a name="read-handwritten-text-handwriting-recognition"></a>Handgeschreven tekst lezen (handschriftherkenning)

De functie Handschriftherkenning van Computer Vision analyseert een afbeelding van handgeschreven tekst. Nadat de analyse is voltooid is, retourneert Handschriftherkenning een JSON-object met de tekst en de locatie van de tekst in de afbeelding.

Voor het voltooien van de functie handschriftherkenning van de toepassing, moet u de volgende stappen uitvoeren:

#### <a name="add-the-event-handler-code-for-the-handwriting-button"></a>De code van de gebeurtenis-handler voor het aanpassen van handschriftherkenning knop toevoegen

Open het bestand **handwriting.html** in een teksteditor en zoek de functie **handwritingButtonClick** onderaan het bestand.

Met de gebeurtenis-handlerfunctie **handwritingButtonClick** wordt het formulier gewist, en vervolgens wordt de afbeelding weergegeven die is opgegeven in de URL en de functie **HandwritingImage** aangeroepen voor analyse van de afbeelding.

Kopieer en plak de volgende code in de functie **handwritingButtonClick**.

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>De wrapper voor de REST API-aanroep toevoegen

Met de funnctie **ReadHandwrittenImage** worden de twee REST API-aanroepen voor analyse van de afbeelding verpakt. Aangezien handschriftherkenning een tijdrovend proces is, wordt het proces in twee stappen uitgevoerd. Bij de eerste aanroep wordt de afbeelding verzonden voor verwerking en bij de tweede aanroep wordt de gedetecteerde tekst opgehaald wanneer de verwerking is voltooid.

Nadat de tekst is opgehaald, wordt de opgemaakte JSON met de tekst en de locatie van de tekst weergegeven in de opgegeven textarea.

Kopieer de volgende **ReadHandwrittenImage**-functie en plak deze direct onder de functie **handwritingButtonClick**.

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-handwriting-function"></a>De functie handschriftherkenning uitvoeren

Sla het bestand **handwriting.html** op en open het in een webbrowser. Plaats uw abonnementssleutel in het veld **Abonnementssleutel** en controleer in **Abonnementregio** of u de juiste regio gebruikt. Voer een URL in naar een afbeelding van tekst die moet worden gelezen en klik vervolgens op de knop **Afbeelding lezen** om de afbeelding te analyseren en het resultaat te bekijken.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding gebruikt u de Computer Vision REST-API met JavaScript voor het testen van veel van de functies van de analyse van de installatiekopie van de beschikbare. Vervolgens, Zie de documentatie voor meer informatie over de API's die betrokken zijn.

- [Computer Vision-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)
