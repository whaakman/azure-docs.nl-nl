---
title: 'Quickstart: Lijst met ondersteunde talen ophalen, Node.js - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids haalt u een lijst met ondersteunde talen op voor vertaling, transliteratie en opzoeken in woordenlijsten en voorbeelden met behulp van de Translator Text-API met Node.js.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 013902927e0b59e5c1723868f7a3b3d553412000
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183064"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-with-nodejs"></a>Quickstart: De Translator Text-API gebruiken om een lijst met ondersteunde talen op te halen met Node.js

In deze snelstart leert u hoe u een GET-aanvraag maakt waarmee u een lijst met ondersteunde talen kunt ophalen met behulp van Node.js en de Translator Text-REST API.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Node 8.12.x of hoger](https://nodejs.org/en/)

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `get-languages.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `npm install request uuidv4` uit.

Deze modules zijn vereist om de HTTP-aanvraag te maken en om een unieke id voor de `'X-ClientTraceId'`-header te maken.

## <a name="configure-the-request"></a>Aanvraag configureren

Met de methode `request()`, beschikbaar gesteld via de aanvraagmodule, kunt u de HTTP-methode, URL, aanvraagparameters, headers en de JSON-hoofdtekst doorgeven als een `options`-object. In dit codefragment configureert u de aanvraag:

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: talen](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

```javascript
let options = {
    method: 'GET',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'languages',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    json: true,
};
```

## <a name="make-the-request-and-print-the-response"></a>De aanvraag maken en het antwoord afdrukken

Vervolgens maakt u de aanvraag via de methode `request()`. Deze gebruikt het `options`-object dat in de vorige sectie als eerste argument is gemaakt, waarna het opgemaakte JSON-antwoord wordt afgedrukt.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> In dit voorbeeld definieert u de HTTP-aanvraag in het `options`-object. De aanvraagmodule ondersteunt echter ook helpermethoden, zoals `.post` en `.get`. Zie [Helpermethoden](https://github.com/request/request#convenience-methods) voor meer informatie.

## <a name="put-it-all-together"></a>Alles samenvoegen

Dat was het. U hebt een eenvoudig programma gemaakt dat we de Translator Text-API zullen noemen. Er is een JSON-antwoord geretourneerd. Het is nu tijd om uw programma uit te voeren:

```console
node get-languages.js
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="sample-response"></a>Voorbeeldantwoord

De afkorting van land/regio niet vinden in dit [lijst met talen](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/language-support).

Dit voorbeeld is ingekort en u ziet een fragment van het resultaat:

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

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Node.js-voorbeelden op GitHub bekijken](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)

## <a name="see-also"></a>Zie ook

Naast taaldetectie kunt u de Translator Text-API ook gebruiken voor het volgende:

* [Tekst vertalen](quickstart-nodejs-translate.md)
* [Tekst transcriberen](quickstart-nodejs-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-nodejs-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-nodejs-dictionary.md)
* [De zinlengte in invoer bepalen](quickstart-nodejs-sentences.md)
