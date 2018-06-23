---
title: Computer Vision-API JavaScript-zelfstudie | Microsoft Docs
description: Gebruik een eenvoudige JavaScript-app die gebruikmaakt van de Computer Vision-API in cognitieve Microsoft-Services. OCR uitvoeren, miniaturen en werken met de visual functies in een afbeelding.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 09/19/2017
ms.author: kefre
ms.openlocfilehash: 89bdc0524e07c1cb6a1473e0a52791fe20271e06
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344584"
---
# <a name="computer-vision-api-javascript-tutorial"></a>Computer Vision-API JavaScript-zelfstudie

Deze zelfstudie ziet de functies van de Microsoft cognitieve Services Computer Vision REST-API.

Verken een JavaScript-toepassing die gebruikmaakt van de Computer Vision REST-API (optische tekenherkenning) uitvoeren, smartcard bijgesneden miniaturen maken plus detecteren, categoriseren, tag en beschrijven visual functies, zoals vlakken, in een afbeelding. In dit voorbeeld kunt u een afbeeldings-URL voor analyse en verwerking verzenden. U kunt in dit voorbeeld open-source als een sjabloon voor het bouwen van uw eigen app in JavaScript Computer Vision REST API gebruiken.

De JavaScript-formulier-toepassing is al geschreven, maar heeft geen Computer Vision-functionaliteit. In deze zelfstudie kunt u de specifieke code toevoegen aan de Computer Vision REST-API voor het voltooien van de functionaliteit van de toepassing.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Deze zelfstudie is ontwikkeld met een eenvoudige teksteditor.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Abonneer u op de Computer Vision-API en de abonnementssleutel voor een ophalen 

Voordat u het voorbeeld maakt, moet u zich abonneren op de Computer Vision-API die deel uitmaakt van de cognitieve Microsoft-Services. Zie voor abonnement en sleutelbeheer details [abonnementen](https://azure.microsoft.com/try/cognitive-services/). De primaire en secundaire sleutels zijn geldig voor gebruik in deze zelfstudie. 

## <a name="download-the-tutorial-project"></a>De zelfstudie project downloaden

Kloon de [cognitieve Services JavaScript Computer Vision-zelfstudie](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial), of het ZIP-bestand downloaden en pak het een lege map.

Als u liever gebruiken de zelfstudie klaar met alle zelfstudie code toegevoegd, kunt u de bestanden in de **voltooid** map.

## <a name="add-the-tutorial-code"></a>Voeg de code van de zelfstudie

De JavaScript-toepassing is ingesteld met zes HTML-bestanden, één voor elke functie. Elk bestand ziet u een andere functie van visie op Computer (analyseren, OCR, enzovoort). De zes zelfstudie secties beschikt niet over de afhankelijkheden, zodat u de zelfstudie code aan een bestand, alle zes bestanden of een aantal bestanden toevoegen kunt is. En u kunt de zelfstudie code toevoegen aan de bestanden in willekeurige volgorde.

Aan de slag.

## <a name="analyze-an-image"></a>Analyseer een afbeelding

De functie analyseren van Computer Vision analyseert een afbeelding voor meer dan 2.000 herkenbare objecten, levende wezens achtergrond en acties. Zodra de analyse voltooid is, retourneert analyseren een JSON-object dat de installatiekopie met de beschrijvende labels, analyse van kleur en bijschriften beschrijft.

Als u de functie analyseren van de zelfstudie toepassing, moet u de volgende stappen uitvoeren:

### <a name="analyze-step-1-add-the-event-handler-code-for-the-form-button"></a>Analyseren van stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **analyze.html** bestand in een teksteditor en zoek de **analyzeButtonClick** functie aan de onderkant van het bestand.

De **analyzeButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **AnalyzeImage** functie voor het analyseren van de installatiekopie.

Kopieer en plak de volgende code naar de **analyzeButtonClick** functie.

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

### <a name="analyze-step-2-add-the-wrapper-for-the-rest-api-call"></a>Analyseren van stap 2: de wrapper voor de REST-API-aanroep toevoegen

De **AnalyzeImage** functie loopt de REST-API-aanroep voor het analyseren van een installatiekopie. Na een geslaagde retour de opgemaakte JSON-analyse wordt weergegeven in de opgegeven textarea en het bijschrift wordt weergegeven in het opgegeven bereik.

Kopieer en plak de **AnalyzeImage** werken code alleen onder de **analyzeButtonClick** functie.

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

### <a name="analyze-step-3-run-the-application"></a>Analyseren van stap 3: de toepassing uitvoeren

Sla de **analyze.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een installatiekopie te analyseren en klik vervolgens op de **analyseren installatiekopie** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="recognize-a-landmark"></a>Een kenmerkende herkennen

De functie kenmerkende van Computer Vision analyseert een afbeelding voor natuurlijke en kunstmatige monumenten, zoals bergen of beroemdheid gebouwen. Zodra de analyse voltooid is, retourneert kenmerkende een JSON-object waarmee de monumenten gevonden in de installatiekopie.

Om te voltooien kenmerkende-functie van de zelfstudie toepassing, moet u de volgende stappen uitvoeren:

### <a name="landmark-step-1-add-the-event-handler-code-for-the-form-button"></a>Kenmerkende stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **landmark.html** bestand in een teksteditor en zoek de **landmarkButtonClick** functie aan de onderkant van het bestand.

De **landmarkButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **IdentifyLandmarks** functie voor het analyseren van de installatiekopie.

Kopieer en plak de volgende code naar de **landmarkButtonClick** functie.

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

### <a name="landmark-step-2-add-the-wrapper-for-the-rest-api-call"></a>Kenmerkende stap 2: de wrapper voor de REST-API-aanroep toevoegen

De **IdentifyLandmarks** functie loopt de REST-API-aanroep voor het analyseren van een installatiekopie. Na een geslaagde retour de opgemaakte JSON-analyse wordt weergegeven in de opgegeven textarea en het bijschrift wordt weergegeven in het opgegeven bereik.

Kopieer en plak de **IdentifyLandmarks** werken code alleen onder de **landmarkButtonClick** functie.

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

### <a name="landmark-step-3-run-the-application"></a>Kenmerkende stap 3: de toepassing uitvoeren

Sla de **landmark.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een installatiekopie te analyseren en klik vervolgens op de **analyseren installatiekopie** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="recognize-celebrities"></a>Beroemdheden herkennen

Een installatiekopie van analyseert het onderdeel beroemdheden van Computer Vision voor beroemdheden. Zodra de analyse is voltooid, retourneert beroemdheden een JSON-object waarmee de beroemdheden gevonden in de installatiekopie.

Als u de functie beroemdheden van de zelfstudie toepassing, moet u de volgende stappen uitvoeren:

### <a name="celebrities-step-1-add-the-event-handler-code-for-the-form-button"></a>Beroemdheden stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **celebrities.html** bestand in een teksteditor en zoek de **celebritiesButtonClick** functie aan de onderkant van het bestand.

De **celebritiesButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **IdentifyCelebrities** functie voor het analyseren van de installatiekopie.

Kopieer en plak de volgende code naar de **celebritiesButtonClick** functie.

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

### <a name="celebrities-step-2-add-the-wrapper-for-the-rest-api-call"></a>Beroemdheden stap 2: de wrapper voor de REST-API-aanroep toevoegen

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

### <a name="celebrities-step-3-run-the-application"></a>Beroemdheden stap 3: de toepassing uitvoeren

Sla de **celebrities.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een installatiekopie te analyseren en klik vervolgens op de **analyseren installatiekopie** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="intelligently-generate-a-thumbnail"></a>Een miniatuur op intelligente wijze maken

De functie voor de miniatuur van Computer Vision genereert een miniatuur van een installatiekopie. Met behulp van de **Smart bijsnijden** functie, de functie miniatuur identificeert het interessegebied in een installatiekopie en de miniatuur op dit gebied, voor het genereren van meer fraai resultaat miniatuurafbeeldingen.

Als u wilt de miniatuur-functie van de toepassing van de zelfstudie hebt voltooid, moet u de volgende stappen uitvoeren:

### <a name="thumbnail-step-1-add-the-event-handler-code-for-the-form-button"></a>Miniaturen stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **thumbnail.html** bestand in een teksteditor en zoek de **thumbnailButtonClick** functie aan de onderkant van het bestand.

De **thumbnailButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **getThumbnail** functie tweemaal voor het maken van twee miniaturen, een smartcard bijgesneden en een zonder slimme bijsnijden.

Kopieer en plak de volgende code naar de **thumbnailButtonClick** functie.

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

### <a name="thumbnail-step-2-add-the-wrapper-for-the-rest-api-call"></a>Miniaturen stap 2: de wrapper voor de REST-API-aanroep toevoegen

De **getThumbnail** functie loopt de REST-API-aanroep voor het analyseren van een installatiekopie. Na een geslaagde retour wordt de miniatuur weergegeven in het opgegeven img-element.

Kopieer en plak de volgende **getThumbnail** functie naar net onder de **thumbnailButtonClick** functie.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumnail image.
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

### <a name="thumbnail-step-3-run-the-application"></a>Miniaturen stap 3: de toepassing uitvoeren

Sla de **thumbnail.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een installatiekopie te analyseren en klik vervolgens op de **genereren miniaturen** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="read-printed-text-ocr"></a>Lees afgedrukte tekst (OCR)

De functie OCR (OCR) van de Computer Vision analyseert een image bevatten van afgedrukte tekst. Nadat de analyse voltooid is, retourneert OCR een JSON-object met de tekst en de locatie van de tekst in de installatiekopie.

Als u de functie OCR van de zelfstudie toepassing, moet u de volgende stappen uitvoeren:

### <a name="ocr-step-1-add-the-event-handler-code-for-the-form-button"></a>OCR stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **ocr.html** bestand in een teksteditor en zoek de **ocrButtonClick** functie aan de onderkant van het bestand.

De **ocrButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **ReadOcrImage** functie voor het analyseren van de installatiekopie.

Kopieer en plak de volgende code naar de **ocrButtonClick** functie.

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

### <a name="ocr-step-2-add-the-wrapper-for-the-rest-api-call"></a>OCR stap 2: de wrapper voor de REST-API-aanroep toevoegen

De **ReadOcrImage** functie loopt de REST-API-aanroep voor het analyseren van een installatiekopie. Na een geslaagde retourneren, de opgemaakte JSON met een beschrijving van de tekst en de locatie van de tekst wordt weergegeven in de opgegeven textarea.

Kopieer en plak de volgende **ReadOcrImage** functie naar net onder de **ocrButtonClick** functie.

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

### <a name="ocr-step-3-run-the-application"></a>OCR stap 3: de toepassing uitvoeren

Sla de **ocr.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een afbeelding van tekst te lezen en klik vervolgens op de **lezen installatiekopie** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="read-handwritten-text-handwriting-recognition"></a>Lees geschreven tekst (handschriftherkenning)

De functie handschriftherkenning van Computer Vision analyseert een image bevatten van de hand geschreven. Nadat de analyse voltooid is, retourneert handschriftherkenning een JSON-object met de tekst en de locatie van de tekst in de installatiekopie.

Als u de functie handschriftherkenning van de zelfstudie toepassing, moet u de volgende stappen uitvoeren:

### <a name="handwriting-recognition-step-1-add-the-event-handler-code-for-the-form-button"></a>Handschriftherkenning erkenning stap 1: de code van de gebeurtenis-handler voor de formulierknop toevoegen

Open de **handwriting.html** bestand in een teksteditor en zoek de **handwritingButtonClick** functie aan de onderkant van het bestand.

De **handwritingButtonClick** gebeurtenishandlerfunctie-handler het formulier wordt gewist, wordt weergegeven in de URL opgegeven installatiekopie van het roept vervolgens de **HandwritingImage** functie voor het analyseren van de installatiekopie.

Kopieer en plak de volgende code naar de **handwritingButtonClick** functie.

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

### <a name="handwriting-recognition-step-2-add-the-wrapper-for-the-rest-api-call"></a>Handschriftherkenning erkenning stap 2: de wrapper voor de REST-API-aanroep toevoegen

De **ReadHandwrittenImage** functie loopt de twee REST-API-aanroepen die nodig zijn voor het analyseren van een installatiekopie. Omdat handschriftherkenning een tijdrovend proces zijn is, worden twee stappen wordt gebruikt. De eerste aanroep verzendt de afbeelding voor de verwerking; de tweede aanroep gedetecteerde tekst opgehaald bij de verwerking voltooid is.

Nadat de tekst die is opgehaald, worden de opgemaakte JSON met een beschrijving van de tekst en de locatie van de tekst wordt weergegeven in de opgegeven textarea.

Kopieer en plak de volgende **ReadHandwrittenImage** functie naar net onder de **handwritingButtonClick** functie.

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

    // This operation requrires two REST API calls. One to submit the image for processing,
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

### <a name="handwriting-recognition-step-3-run-the-application"></a>Handschriftherkenning erkenning stap 3: de toepassing uitvoeren

Sla de **handwriting.html** bestand en open het in een webbrowser. Plaatst de abonnementssleutel van uw in de **Abonnementssleutel** veld en controleer of u de juiste regio in **abonnement regio**. Voer een URL naar een afbeelding van tekst te lezen en klik vervolgens op de **lezen installatiekopie** knop voor het analyseren van een installatiekopie en het resultaat te bekijken.

## <a name="next-steps"></a>Volgende stappen

- [Computer Vision-API C&#35; zelfstudie](CSharpTutorial.md)
- [Computer Vision-API Python-zelfstudie](PythonTutorial.md)
