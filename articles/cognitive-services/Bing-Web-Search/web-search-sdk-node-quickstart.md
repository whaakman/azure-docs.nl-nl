---
title: 'Snelstartgids: de Bing Web Search SDK voor Node.js gebruiken'
description: Leer de Bing Web Search SDK voor Node.js gebruiken.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 08/16/2018
ms.author: erhopf
ms.openlocfilehash: 7c3003ab4ba40a9d0212e7c94b6dd3bfbc8f0ca2
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43186628"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Snelstartgids: de Bing Web Search SDK voor Node.js gebruiken

Met de Bing Web Search SDK kunt u Bing Web Search eenvoudig integreren in uw Node.js-toepassing. In deze snelstartgids leert u hoe u een instantie kunt maken voor een client, een aanvraag kunt verzenden en het antwoord kunt afdrukken.

Wilt u de code nu zien? De [voorbeelden voor de Bing Web Search SDK voor Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) zijn beschikbaar op GitHub.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Vereisten

U moet over het volgende beschikken voordat u verdergaat met deze snelstartgids:

* [Node.js 6](https://nodejs.org/en/download/) of hoger
* Een abonnementssleutel  

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

U moet eerst de ontwikkelomgeving voor het Node.js-project opzetten.

1. Maak een nieuwe map voor uw project:

    ```console
    mkdir YOUR_PROJECT
    ```

2. Maak een nieuw pakketbestand:

    ```console
    cd YOUR_PROJECT
    npm init
    ```

3. Nu gaan we enkele Azure-modules installeren en toevoegen aan de `package.json`:

    ```console
    npm install --save azure-cognitiveservices-websearch
    npm install --save ms-rest-azure
    ```

## <a name="create-a-project-and-declare-required-modules"></a>Een project maken en de vereiste modules declareren

Maak in dezelfde map als uw `package.json` een nieuw Node.js-project met behulp van uw favoriete IDE of editor. Bijvoorbeeld: `sample.js`.

Vervolgens kopieert u deze code naar uw project. Hiermee worden de modules geladen die in de vorige sectie zijn geïnstalleerd.

```javascript
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>Een instantie maken voor de client

Met deze code wordt een instantie gemaakt voor een client en wordt gebruikgemaakt van de module `azure-cognitiveservices-websearch`. Zorg ervoor dat u een geldige abonnementssleutel voor uw Azure-account invoert voordat u verdergaat.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>Een aanvraag maken en de resultaten weergeven

Gebruik de client om een zoekquery te verzenden naar Bing Web Search. Als het antwoord resultaten bevat voor items in de `properties`-matrix, wordt de `result.value` weergegeven in de console.

```javascript
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})
```

## <a name="run-the-program"></a>Het programma uitvoeren

Als laatste stap voert u het programma uit.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met dit project, moet u uw abonnementssleutel verwijderen uit de code van het programma.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voorbeelden voor Cognitive Services Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie over Azure Node SDK](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-websearch/)
