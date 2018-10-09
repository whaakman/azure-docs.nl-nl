---
title: 'Zelfstudie: Een web-app met één pagina bouwen - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Laat zien hoe u de Bing Visual Search-API kunt gebruiken in een webtoepassing met één pagina.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: e3cd36d799256406b3ae12f35303bd2406468b3c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227177"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Zelfstudie: Visual Search-web-app met één pagina

Bing Visual Search-API biedt een ervaring die lijkt op de afbeeldingsdetails die worden getoond op Bing.com/images. Met Visual Search kunt u een afbeelding opgeven en inzichten over de afbeelding terugkrijgen, zoals visueel vergelijkbare afbeeldingen, winkelbronnen, webpagina's met de afbeelding en meer. 

Deze zelfstudie breidt de web-app met één pagina van de Bing Image Search-zelfstudie uit (zie [Web-app met één pagina](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Zie [Web-app met één pagina (broncode)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md) voor de volledige broncode om deze zelfstudie te starten. Zie [Visual Search-web-app met één pagina](tutorial-bing-visual-search-single-page-app-source.md) voor de uiteindelijke broncode van deze zelfstudie.

De behandelde taken zijn:

> [!div class="checklist"]
> * Bing Visual Search-API aanroepen met een afbeeldingsinzichttoken
> * Vergelijkbare afbeeldingen weergeven

## <a name="call-bing-visual-search"></a>Bing Visual Search aanroepen
Bewerk de Bing Image Search zelfstudie en voeg de volgende code toe aan het einde van het scriptelement op regel 409. Deze code roept de Bing Visual Search API aan en geeft de resultaten weer.

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
Voeg de volgende code toe aan het `searchItemsRenderer`-object op regel 151. Deze code voegt de koppeling **find similar** toe (vergelijkbare zoeken) die de `bingVisualSearch`-functie aanroept wanneer erop wordt geklikt. De functie ontvangt imageInsightsToken als argument.

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
> [Broncode van de Visual Search-web-app van één pagina](tutorial-bing-visual-search-single-page-app-source.md)
> [Naslaginformatie over Bing Visual Search-API](https://aka.ms/bingvisualsearchreferencedoc)