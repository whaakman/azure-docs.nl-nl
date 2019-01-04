---
title: " Bouw een Web-app van één pagina - Bing visuele zoekopdrachten"
titleSuffix: Azure Cognitive Services
description: Informatie over het integreren van de Bing visuele zoekopdrachten-API in een Web-App van één pagina.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 10/04/2017
ms.author: aahi
ms.openlocfilehash: 8ff5e36e6189c522e00c7cdd126c26b1cef92912
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53745139"
---
# <a name="create-a-visual-search-single-page-web-app"></a>Een visuele zoekopdrachten één pagina web-app maken 

Bing Visual Search-API biedt een ervaring die lijkt op de afbeeldingsdetails die worden getoond op Bing.com/images. Met Visual Search kunt u een afbeelding opgeven en inzichten over de afbeelding terugkrijgen, zoals visueel vergelijkbare afbeeldingen, winkelbronnen, webpagina's met de afbeelding en meer. 

In dit artikel wordt uitgelegd hoe u een één pagina WebApp voor de Bing afbeeldingen zoeken-API uit te breiden. Als u wilt weergeven die zelfstudie of haal de broncode hier gebruikt, Zie [zelfstudie: Een app met één pagina maken voor de Bing afbeeldingen zoeken-API](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md). 

De volledige broncode voor deze toepassing (na het uitbreiden van het gebruik van de Bing visuele zoekopdrachten-API), is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>De Bing visuele zoekopdrachten-API aanroepen en de verwerking van het antwoord

De Bing afbeeldingen zoeken-zelfstudie bewerken en voeg de volgende code toe aan het einde van de `<script>` element (en voordat de afsluiting `</script>` tag). De volgende code een visuele zoekopdrachten-reactie van de API worden verwerkt, doorloopt de resultaten en geeft deze weer.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

De volgende code verzendt een zoekaanvraag naar de API met behulp van een gebeurtenislistener om aan te roepen `handleVisualSearchResponse()`.


```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>Inzichttoken vastleggen

Voeg de volgende code in de `searchItemsRenderer` object. Deze code voegt de koppeling **find similar** toe (vergelijkbare zoeken) die de `bingVisualSearch`-functie aanroept wanneer erop wordt geklikt. De functie ontvangt imageInsightsToken als argument.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Vergelijkbare afbeeldingen weergeven

Voeg de volgende HTML-code toe op regel 601. Deze code voegt een element toe dat wordt gebruikt om de resultaten van de Bing Visual Search-API-aanroep weer te geven.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Met alle nieuwe JavaScript-code en HTML-elementen op hun plaats, worden zoekresultaten weergegeven met een koppeling **find similar** (vergelijkbare zoeken). Klik op koppeling om de sectie **Similar** (vergelijkbaar) te vullen met afbeeldingen die lijken op de afbeelding die u hebt gekozen. Mogelijk moet u de sectie **Similar** uitvouwen om de afbeeldingen weer te geven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bijsnijden en upload een afbeelding](tutorial-visual-search-crop-area-results.md)