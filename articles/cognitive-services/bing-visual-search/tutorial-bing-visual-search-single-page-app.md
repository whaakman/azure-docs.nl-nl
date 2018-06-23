---
title: Zoeken naar Bing-afbeelding één pagina Web-app | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe de Bing installatiekopie zoeken-API gebruiken in een webtoepassing van één pagina.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: 303d7745167d2ea25fda083ed99881ac4e0a7ec7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345326"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Zelfstudie: Visual zoeken één pagina Web-app

Bing Visual zoeken-API biedt een vergelijkbaar met de details van de afbeelding die wordt weergegeven op Bing.com/images ervaring. U kunt met Visual Search opgeven van een installatiekopie van een en terughalen van inzicht in de afbeelding zoals visueel soortgelijke afbeeldingen, winkelwagen bronnen, webpagina's met de installatiekopie en andere elementen bevatten. 

Deze zelfstudie breidt de web-app van één pagina van de zelfstudie zoeken naar Bing-afbeelding (Zie [Single-page-Web-app](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Zie voor de volledige broncode om te starten in deze zelfstudie [Single-page-Web-app (broncode)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Zie voor de laatste broncode van deze zelfstudie [Visual zoeken Single-page-Web-app](tutorial-bing-visual-search-single-page-app-source.md).

De taken besproken zijn:

> [!div class="checklist"]
> * Bing Visual zoeken-API aanroepen met een installatiekopie insights-token
> * Soortgelijke afbeeldingen weergeven

## <a name="call-bing-visual-search"></a>Aanroepen van Bing Visual zoeken
Bewerk de Bing installatiekopie Search-zelfstudie en voeg de volgende code toe aan het einde van het script-element op de regel 409. Deze code roept de API van Bing Visual zoeken en de resultaten worden weergegeven.

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

## <a name="capture-insights-token"></a>Insights token vastleggen
Voeg de volgende code in de `searchItemsRenderer` object op de regel 151. Deze code wordt toegevoegd een **zoeken lijken** koppeling die roept de `bingVisualSearch` werken wanneer geklikt. De functie ontvangt de imageInsightsToken als argument.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Soortgelijke afbeeldingen weergeven
Voeg de volgende HTML-code op de regel 601. Deze code markup voegt een element dat wordt gebruikt om de resultaten van de Bing Visual zoeken-API-aanroep weer te geven.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Met alle nieuwe JavaScript-code en HTML-elementen in de plaats, zoekresultaten worden weergegeven met een **zoeken lijken** koppeling. Klik op de koppeling voor het vullen van de **vergelijkbaar** sectie met afbeeldingen die vergelijkbaar zijn met die u verzameld. Wellicht hebt u uit te breiden de **vergelijkbaar** sectie om de installatiekopieën van het weer te geven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Visual zoeken één pagina app webbron](tutorial-bing-visual-search-single-page-app-source.md)
> [Bing Visual Search API-verwijzingen](https://aka.ms/bingvisualsearchreferencedoc)