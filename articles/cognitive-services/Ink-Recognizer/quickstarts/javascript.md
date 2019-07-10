---
title: 'Quickstart: Herkent digitale inkt met de Ink herkenning REST-API en Node.js'
titleSuffix: Azure Cognitive Services
description: De Ink-herkenning-API gebruiken om te beginnen met het herkennen van digitale inkt lijnen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 7e158b0ae27780eeecb1ee7948087bf59b1502e1
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721266"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-javascript"></a>Quickstart: Herkent digitale inkt met de Ink herkenning REST API en JavaScript

Gebruik deze Quick Start om te beginnen met behulp van de Ink-API voor herkenning van digitale inkt lijnen. Deze JavaScript-toepassing een API-aanvraag met JSON-indeling ink lijn gegevens verzendt en het antwoord weergegeven.

Hoewel deze toepassing is geschreven in Javascript en wordt uitgevoerd in uw webbrowser, de API is een RESTful-web-compatibel is met de meeste moderne programmeertalen.

Doorgaans wilt u de API aanroepen vanuit een digitale bij app. In deze snelstartgids verzendt ink lijn gegevens voor het volgende voorbeeld van handgeschreven vanuit een JSON-bestand.

![een afbeelding van handgeschreven tekst](../media/handwriting-sample.jpg)

De broncode voor deze Quick Start kunt u vinden op [GitHub](https://go.microsoft.com/fwlink/?linkid=2089905).

## <a name="prerequisites"></a>Vereisten

- Een webbrowser
- De voorbeeld ink lijn gegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-ink-strokes.json).


[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak in uw favoriete IDE of editor, een nieuwe `.html` bestand. Vervolgens voegt u aan deze eenvoudige HTML die gaan we later toevoegen met de code.
    
    ```html
    <!DOCTYPE html>
    <html>
    
        <head>
            <script type="text/javascript">
            </script>
        </head>
        
        <body>
        </body>
    
    </html>
    ```

2. Binnen de `<body>` taggen, het toevoegen van de volgende HTML-code:
    1. Twee aspecten van de tekst voor het weergeven van de JSON-aanvraag en respons.
    2. Een knop voor het aanroepen de `recognizeInk()` functie die later worden gemaakt.
    
    ```HTML
    <!-- <body>-->
        <h2>Send a request to the Ink Recognition API</h2>
        <p>Request:</p>
        <textarea id="request" style="width:800px;height:300px"></textarea>
        <p>Response:</p>
        <textarea id="response" style="width:800px;height:300px"></textarea>
        <br>
        <button type="button" onclick="recognizeInk()">Recognize Ink</button>
    <!--</body>-->
    ```

## <a name="load-the-example-json-data"></a>De voorbeeld-JSON-gegevens laden

1. Binnen de `<script>` taggen, het maken van een variabele voor de sampleJson. Maak vervolgens een JavaScript-functie met de naam `openFile()` die een Verkenner wordt geopend, zodat u kunt uw JSON-bestand selecteren. Wanneer de `Recognize ink` knop wordt geklikt, wordt deze functie aanroepen en beginnen met het bestand te lezen.
2. Gebruik een `FileReader` van het object `onload()` functie het bestand om asynchroon te verwerken. 
    1. Vervang alle `\n` of `\r` tekens in het bestand met een lege tekenreeks. 
    2. Gebruik `JSON.parse()` de tekst omzetten in geldige JSON
    3. Update de `request` in het tekstvak in de toepassing. Gebruik `JSON.stringify()` om de opmaak van de JSON-tekenreeks. 
    
    ```javascript
    var sampleJson = "";
    function openFile(event) {
        var input = event.target;
    
        var reader = new FileReader();
        reader.onload = function(){
            sampleJson = reader.result.replace(/(\\r\\n|\\n|\\r)/gm, "");
            sampleJson = JSON.parse(sampleJson);
            document.getElementById('request').innerHTML = JSON.stringify(sampleJson, null, 2);
        };
        reader.readAsText(input.files[0]);
    };
    ```

## <a name="send-a-request-to-the-ink-recognizer-api"></a>Een aanvraag verzenden naar de API Ink-herkenning

1. Binnen de `<script>` taggen, het maken van een functie met de naam `recognizeInk()`. Deze functie wordt later de API aanroepen en het bijwerken van de pagina met het antwoord. Voeg de code uit de volgende stappen in deze functie. 
        
    ```javascript
    function recognizeInk() {
    // add the code from the below steps here 
    }
    ```

    1. Variabelen maken voor uw eindpunt-URL de abonnementssleutel en de voorbeeld-JSON. Maak vervolgens een `XMLHttpRequest` -object voor verzending van de API-aanvraag. 
        
        ```javascript
        // Replace the below URL with the correct one for your subscription. 
        // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
        var SERVER_ADDRESS = "YOUR-SUBSCRIPTION-URL";
        var ENDPOINT_URL = SERVER_ADDRESS + "/inkrecognizer/v1.0-preview/recognize";
        // Replace the subscriptionKey string value with your valid subscription key.
        var SUBSCRIPTION_KEY = "YOUR-SUBSCRIPTION-KEY";
        var xhttp = new XMLHttpRequest();
        ```
    2. Maken van de retourfunctie van de `XMLHttpRequest` object. Deze functie wordt de API-reactie van een geslaagde aanvraag parseren en weer te geven in de toepassing. 
            
        ```javascript
        function returnFunction(xhttp) {
            var response = JSON.parse(xhttp.responseText);
            console.log("Response: %s ", response);
            document.getElementById('response').innerHTML = JSON.stringify(response, null, 2);
        }
        ```
    3. De foutfunctie voor de request-object maken. Deze functie registreert de fout in de console. 
            
        ```javascript
        function errorFunction() {
            console.log("Error: %s, Detail: %s", xhttp.status, xhttp.responseText);
        }
        ```

    4. Maak een functie voor het Aanvraagobject `onreadystatechange` eigenschap. Wanneer de gereedheidsstatus van het Aanvraagobject wordt gewijzigd, wordt de bovenstaande terug en fout-functies worden toegepast.
            
        ```javascript
        xhttp.onreadystatechange = function () {
            if (this.readyState === 4) {
                if (this.status === 200) {
                    returnFunction(xhttp);
                } else {
                    errorFunction(xhttp);
                }
            }
        };
        ```
    
    5. De API-aanvraag verzenden. Uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` -header en stel de `content-type` aan `application/json`
    
        ```javascript
        xhttp.open("PUT", ENDPOINT_URL, true);
        xhttp.setRequestHeader("Ocp-Apim-Subscription-Key", SUBSCRIPTION_KEY);
        xhttp.setRequestHeader("content-type", "application/json");
        xhttp.send(JSON.stringify(sampleJson));
        };
        ```

## <a name="run-the-application-and-view-the-response"></a>Voer de toepassing en de reactie weergeven

Deze toepassing kan worden uitgevoerd in uw webbrowser. Een geslaagde respons wordt geretourneerd in JSON-indeling. U kunt ook het JSON-antwoord vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-response.json):

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://go.microsoft.com/fwlink/?linkid=2089907)

Als u wilt zien hoe de Ink-API in een digitale bij app werkt, bekijk de volgende voorbeeldtoepassingen op GitHub:
* [C# en Universal Windows Platform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# en Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Webbrowser-app (Javascript)](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobiele app (Java en Android)](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobiele app (Swift en iOS)](https://go.microsoft.com/fwlink/?linkid=2089805)
