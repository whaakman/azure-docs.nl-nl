---
title: 'Snelstart: ondersteunde talen ophalen - Translator Text, Node.js'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids haalt u een lijst met ondersteunde talen op voor vertaling, transliteratie en opzoeken in woordenlijsten en voorbeelden met behulp van de Translator Text-API met Node.js.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: a3b9dde8983b87a63858b3eb1381f7c74545cb1a
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122855"
---
# <a name="quickstart-get-supported-languages-with-nodejs"></a>Snelstartgids: ondersteunde talen ophalen met Node.js

In deze snelstartgids haalt u een lijst met ondersteunde talen op voor vertaling, transliteratie en opzoeken in woordenlijsten en voorbeelden met behulp van de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Node.js 6](https://nodejs.org/en/download/) nodig om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Zie [Hoe u zich registreert voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="languages-request"></a>Languages-aanvraag

Met de volgende code wordt een lijst opgehaald met ondersteunde talen voor vertaling, transliteratie en opzoeken in woordenlijsten en voorbeelden met behulp van de methode [Languages](./reference/v3-0-languages.md).

1. Maak een nieuw Node.js-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'api.cognitive.microsofttranslator.com';
let path = '/languages?api-version=3.0';

let output_path = 'output.txt';
let ws = fs.createWriteStream(output_path);

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let json = JSON.stringify(JSON.parse(body), null, 4);
        ws.write (json);
        ws.close ();
        console.log("File written.");

    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let GetLanguages = function () {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.end ();
}

GetLanguages ();
```

## <a name="languages-response"></a>Languages-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere, zoals vertaling en transliteratie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Node.js-voorbeelden op GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=javascript)
