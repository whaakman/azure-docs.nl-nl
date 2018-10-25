---
title: 'Snelstart: Bing Visual Search SDK, Node'
titleSuffix: Azure Cognitive Services
description: Lees hier informatie over de instellingen voor het maken van een Node-consoletoepassing met behulp van Visual Search SDK.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 05/18/2018
ms.author: v-gedod
ms.openlocfilehash: 0956e88bd293b62935f5c8e2b9ef2d30466af8e4
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456606"
---
# <a name="quickstart-bing-visual-search-sdk-node"></a>Snelstart: Bing Visual Search SDK voor Node 

De Bing Visual Search SDK gebruikt de functionaliteit van de REST-API voor webaanvragen en het parseren van resultaten.
De [broncode voor voorbeelden van Bing Video Search SDK voor Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/visualSearch.js) is beschikbaar op Git Hub.

Documentatie van codescenario's vindt u onder de volgende koppen:
* [Visual Search-client](#client)
* [Consoletoepassing voltooien](#complete)

## <a name="prerequisites"></a>Vereisten

* Er is een code van de Cognitive Services API vereist voor het verifiëren van SDK-aanroepen. Meld u aan voor een [gratis proefcode](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7). De proefcode is gedurende zeven dagen geldig, met één aanroep per seconde. Voor productiescenario's kunt u [een toegangscode kopen](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7). Bekijk ook de [prijsgegevens](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing

* Voer `npm install Microsoft.Azure.CognitiveServices.Search.VisualSearch` uit.

<a name="client"></a>
## <a name="visual-search-client"></a>Visual Search-client
Als u een exemplaar van de client `VisualSearchAPI` wilt maken, voegt u using-instructies toe:
```
const Search = require('azure-cognitiveservices-visualsearch');
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;

```
Instantieer vervolgens de client:
```
let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';
let credentials = new CognitiveServicesCredentials(keyVar);
let visualSearchApiClient = new Search.VisualSearchAPIClient(credentials);

```
De client gebruiken om te zoeken naar afbeeldingen:
```
let fileStream = fs.createReadStream('../Data/image.jpg');
let visualSearchRequest = JSON.stringify({});
let visualSearchResults;
try {
    visualSearchResults = await visualSearchApiClient.images.visualSearch({
        image: fileStream,
        knowledgeRequest: visualSearchRequest
    });
    console.log("Search visual search request with binary of image");
} catch (err) {
    console.log("Encountered exception. " + err.message);
}

```
De resultaten van de vorige query parseren:
```
// Visual Search results
if (visualSearchResults.image.imageInsightsToken) {
    console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
}
else {
    console.log("Couldn't find image insights token!");
}

// List of tags
if (visualSearchResults.tags.length > 0) {
    let firstTagResult = visualSearchResults.tags[0];
    console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);

    // List of actions in first tag
    if (firstTagResult.actions.length > 0) {
        let firstActionResult = firstTagResult.actions[0];
        console.log(`First tag action count: ${firstTagResult.actions.length}`);
        console.log(`First tag action type: ${firstActionResult.actionType}`);
    }
    else {
        console.log("Couldn't find tag actions!");
    }

}
else {
    console.log("Couldn't find image tags!");
}

```
<a name="complete"></a>
## <a name="complete-console-application"></a>Consoletoepassing voltooien

De volgende consoletoepassing voert de eerder gedefinieerde code uit, andere scenario's en parseert vervolgens de resultaten:
```
/*
 * Copyright (c) Microsoft Corporation. All rights reserved.
 * Licensed under the MIT License. See License.txt in the project root for
 * license information.
 */
'use strict';

const os = require("os");
const async = require('async');
const fs = require('fs');
const Search = require('azure-cognitiveservices-visualsearch');
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;


let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';

let credentials = new CognitiveServicesCredentials(keyVar);
let visualSearchApiClient = new Search.VisualSearchAPIClient(credentials);
let visualModels = visualSearchApiClient.models;
sample();

function sample() {
    async.series([
        async function () {
            let fileStream = fs.createReadStream('../Data/image.jpg');
            let visualSearchRequest = JSON.stringify({});
            let visualSearchResults;
            try {
                visualSearchResults = await visualSearchApiClient.images.visualSearch({
                    image: fileStream,
                    knowledgeRequest: visualSearchRequest
                });
                console.log("Visual search request with binary of dog image");
            } catch (err) {
                console.log("Encountered exception. " + err.message);
            }
            if (!visualSearchResults) {
                console.log("No visual search result data. ");
            } else {
                    // Visual Search results
                    if (visualSearchResults.image.imageInsightsToken) {
                        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
                    }
                else {
                    console.log("Couldn't find image insights token!");
                }

                // List of tags
                if (visualSearchResults.tags.length > 0) {
                    let firstTagResult = visualSearchResults.tags[0];
                    console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);

                    // List of actions in first tag
                    if (firstTagResult.actions.length > 0) {
                        let firstActionResult = firstTagResult.actions[0];
                        console.log(`First tag action count: ${firstTagResult.actions.length}`);
                        console.log(`First tag action type: ${firstActionResult.actionType}`);
                    }
                    else {
                        console.log("Couldn't find tag actions!");
                    }

                }
                else {
                    console.log("Couldn't find image tags!");
                }
            }
        },
        async function () {
            let fileStream = fs.createReadStream('../Data/image.jpg');
            let cropArea = { top: 0.1, bottom: 0.5, left: 0.1, right: 0.9 };
            let imageInfo = { cropArea: cropArea };
            let visualSearchRequest = JSON.stringify({ imageInfo: imageInfo });
            let visualSearchResults;
            try {
                visualSearchResults = await visualSearchApiClient.images.visualSearch({
                    image: fileStream,
                    knowledgeRequest: visualSearchRequest
                });
                console.log("\r\nVisual search request with binary of image with JSON info of cropArea");
                if (!visualSearchResults) {
                    console.log("No visual search result data.");
                }
                else {
                    // Visual Search results
                    if (visualSearchResults.image.imageInsightsToken) {
                        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
                    }
                    else {
                        console.log("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.tags.length > 0) {
                        var firstTagResult = visualSearchResults.tags[0];
                        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);

                        // List of actions in first tag
                        if (firstTagResult.actions.length > 0) {
                            var firstActionResult = firstTagResult.actions[0];
                            console.log(`First tag action count: ${firstTagResult.actions.length}`);
                            console.log(`First tag action type: ${firstActionResult.actionType}`);
                        }
                        else {
                            console.log("Couldn't find tag actions!");
                        }

                    }
                    else {
                        console.log("Couldn't find image tags!");
                    }
                }
            } catch (err) {
                console.log("Encountered exception. " + err.message);
            }
        },
        async function () {
            let imageInfo = { url: "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80" };
            let filters = { site: "pinterest.com" };
            let knowledgeRequest = { filters: filters };
            let visualSearchRequest = JSON.stringify({ imageInfo: imageInfo, knowledgeRequest: knowledgeRequest });
            let visualSearchResults;
            try {
                visualSearchResults = await visualSearchApiClient.images.visualSearch({
                    knowledgeRequest: visualSearchRequest
                });
                console.log("\r\nSearch visual search request with imageInfo and filters in JSON text");
                if (!visualSearchResults) {
                    console.log("No visual search result data.");
                }
                else {
                    // Visual Search results
                    if (visualSearchResults.image.imageInsightsToken) {
                        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
                    }
                    else {
                        console.log("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.tags.length > 0) {
                        var firstTagResult = visualSearchResults.tags[0];
                        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);

                        // List of actions in first tag
                        if (firstTagResult.actions.length > 0) {
                            var firstActionResult = firstTagResult.actions[0];
                            console.log(`First tag action count: ${firstTagResult.actions.length}`);
                            console.log(`First tag action type: ${firstActionResult.actionType}`);
                        }
                        else {
                            console.log("Couldn't find tag actions!");
                        }

                    }
                    else {
                        console.log("Couldn't find image tags!");
                    }
                }
            } catch (err) {
                console.log("Encountered exception. " + err.message);
            }
        },
        async function () {
            var imageInsightsToken = "bcid_CA6BDBEA28D57D52E0B9D4B254F1DF0D*ccid_6J+8V1zi*thid_R.CA6BDBEA28D57D52E0B9D4B254F1DF0D";
            let cropArea = { top: 0.1, bottom: 0.5, left: 0.1, right: 0.9 };
            let imageInfo = { imageInsightsToken: imageInsightsToken, cropArea: cropArea };
            let visualSearchRequest = JSON.stringify({ imageInfo: imageInfo });
            let visualSearchResults;
            try {
                visualSearchResults = await visualSearchApiClient.images.visualSearch({
                    knowledgeRequest: visualSearchRequest
                });
                console.log("\r\nVisual search request with ImageInsightsToken and crop area of image");
                if (!visualSearchResults) {
                    console.log("No visual search result data.");
                }
                else {
                    // Visual Search results
                    if (visualSearchResults.image.imageInsightsToken) {
                        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
                    }
                    else {
                        console.log("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.tags.length > 0) {
                        var firstTagResult = visualSearchResults.tags[0];
                        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);

                        // List of actions in first tag
                        if (firstTagResult.actions.length > 0) {
                            var firstActionResult = firstTagResult.actions[0];
                            console.log(`First tag action count: ${firstTagResult.actions.length}`);
                            console.log(`First tag action type: ${firstActionResult.actionType}`);
                        }
                        else {
                            console.log("Couldn't find tag actions!");
                        }

                    }
                    else {
                        console.log("Couldn't find image tags!");
                    }
                }
            } catch (err) {
                console.log("Encountered exception. " + err.message);
            }
        },
        function () {
            return new Promise((resolve) => {
                console.log(os.EOL);
                console.log("Finished running Visual-Search sample.");
                resolve();
            })
        }
    ], (err) => {
        throw (err);
        });
}

exports.sample = sample;
```

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services .NET SDK](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
