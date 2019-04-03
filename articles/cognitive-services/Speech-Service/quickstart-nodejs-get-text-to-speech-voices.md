---
title: 'Quickstart: Lijst met tekst naar spraak stemmen, Node.js - spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u de volledige lijst met standaard- en neurale stemmen voor een regio/het eindpunt met behulp van Node.js. De lijst wordt geretourneerd als JSON en stem beschikbaarheid kan variëren per regio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: erhopf
ms.openlocfilehash: 62187ddbe587a81038f8424b079e3c0c313d1ae2
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887095"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-nodejs"></a>Quickstart: De lijst met tekst naar spraak stemmen met behulp van Node.js

In deze snelstartgids leert u hoe u de volledige lijst met standaard- en neurale stemmen voor een regio/het eindpunt met behulp van Node.js. De lijst wordt geretourneerd als JSON en stem beschikbaarheid kan variëren per regio. Zie voor een lijst met ondersteunde regio's, [regio's](regions.md).

Deze snelstartgids moet een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een resource Speech Services. Als u geen account hebt, kunt u de [gratis proefversie](get-started.md) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Knooppunt 8.12.x of hoger](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* De sleutel van een Azure-abonnement voor de spraakservices. [Vraag een gratis! ](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Maak een project en afhankelijkheden vereist

Maak een nieuwe Node.js-project met behulp van uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `get-voices.js`.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write the list of languagesto a file
const fs = require('fs');
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `npm install request request-promise` uit.

## <a name="get-an-access-token"></a>Een toegangstoken opvragen

De Text to Speech REST-API is een toegangstoken voor verificatie vereist. Als u een toegangstoken, is een exchange vereist. Deze functie vervangt uw abonnementssleutel Speech Services voor een access token met de `issueToken` eindpunt.

In dit voorbeeld wordt ervan uitgegaan dat uw abonnement spraakservices in de regio VS-West. Als u een andere regio, werk de waarde voor `uri`. Zie voor een volledige lijst [regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieer deze code naar uw project:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Zie voor meer informatie over verificatie [verifiëren met een toegangstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

In de volgende sectie maken we de functie voor het ophalen van de lijst met stemmen en de JSON-uitvoer opslaan in bestand.

## <a name="make-a-request-and-save-the-response"></a>Een aanvraag indienen en het antwoord opslaan

Hier gaat u naar de aanvraag voor het samenstellen en opslaan van de lijst met geretourneerde stemmen. In dit voorbeeld wordt ervan uitgegaan dat u het eindpunt van de VS-West. Als de bron naar een andere regio is geregistreerd, moet u bijwerken de `uri`. Zie voor meer informatie, [Speech Services-regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Voeg vervolgens de vereiste headers voor de aanvraag. Ten slotte maakt u een aanvraag naar de service. Als de aanvraag geslaagd is, en een 200-statuscode is geretourneerd, wordt het antwoord wordt geschreven naar het bestand.

```javascript
function textToSpeech(accessToken) {
    let options = {
        method: 'GET',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/voices/list',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json'
        }
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('voices.json'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Alles samenvoegen

U bent bijna klaar. De laatste stap is het maken van een asynchrone-functie. Deze functie wordt uw abonnementssleutel lezen van een omgevingsvariabele, een token verkrijgen, wacht totdat de aanvraag om te voltooien en vervolgens de JSON-antwoord naar bestand schrijven.

Als u niet bekend met omgevingsvariabelen bent of wil testen met uw abonnement sleutel vastgelegd als een tekenreeks, vervangt u `process.env.SPEECH_SERVICE_KEY` met uw abonnementssleutel als een tekenreeks.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Nu kunt u de voorbeeld-app gaan uitvoeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
node get-voices.js
```

## <a name="clean-up-resources"></a>Resources opschonen

Denk eraan dat u eventuele vertrouwelijke informatie, zoals abonnementssleutels, verwijdert uit de broncode van de voorbeeld-app.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Node.js-voorbeelden op GitHub verkennen](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie voor de Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md)
* [Record stem voorbeelden voor het maken van een aangepaste spraak](record-custom-voice-samples.md)
