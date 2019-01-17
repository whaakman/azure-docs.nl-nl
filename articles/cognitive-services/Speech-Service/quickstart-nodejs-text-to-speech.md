---
title: 'Quickstart: Converteren van tekst naar spraak, Node.js - spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u om te converteren tekst naar spraak met behulp van Node.js en de tekst naar spraak REST-API. De voorbeeldtekst opgenomen in deze handleiding is opgebouwd als spraak synthese Markup Language (SSML). Hiermee kunt u de spraak en taal van het antwoord spraak te kiezen.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 8cc676355f432a25550b337e5de747c4956d3142
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358878"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Snelstart: Converteert tekst naar spraak met behulp van Node.js

In deze snelstartgids leert u hoe u om te converteren tekst naar spraak met behulp van Node.js en de Text to Speech REST-API. De hoofdtekst van de aanvraag in deze handleiding is opgebouwd als [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md), waarmee u de spraak en taal van het antwoord te kiezen.

Deze snelstartgids moet een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een resource Speech-Service. Als u geen account hebt, kunt u de [gratis proefversie](get-started.md) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Node 8.12.x of hoger](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* Een Azure-abonnementssleutel voor de Spraakservice. [Vraag een gratis! ](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Maak een project en afhankelijkheden vereist

Maak een nieuwe Node.js-project met behulp van uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `tts.js`.

```javascript
// Requires request for HTTP requests
const request = require('request');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `npm install request readline-sync` uit.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Stel de abonnementssleutel en een prompt voor TTS maken

In de volgende gedeelten maakt u de functies voor verwerking van autorisatie, de Text to Speech-API aanroepen en valideren van het antwoord. Laten we beginnen door een abonnementssleutel toe te voegen en een prompt voor tekstinvoer te maken.

```javascript
/*
 * These lines will attempt to read your subscription key from an environment
 * variable. If you prefer to hardcode the subscription key for ease of use,
 * replace process.env.SUBSCRIPTION_KEY with your subscription key as a string.  
 */
const subscriptionKey = process.env.SUBSCRIPTION_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};

// Prompts the user to input text.
let text = readline.question('What would you like to convert to speech? ');
```

## <a name="get-an-access-token"></a>Een toegangstoken opvragen

De Text to Speech REST-API is een toegangstoken voor verificatie vereist. Als u een toegangstoken, is een exchange vereist. In dit voorbeeld uitwisselt uw abonnementssleutel Spraakservice voor een access token met de `issueToken` eindpunt.

Deze functie omvat twee argumenten, uw abonnementssleutel Speech Services en een retouraanroepfunctie. Nadat de functie een toegangstoken verkregen heeft, wordt de waarde voor de callbackfunctie doorgegeven. In de volgende sectie maken we de functie voor het aanroepen van de Text to Speech-API en het antwoord kunstmatige spraak opslaan.

In dit voorbeeld wordt ervan uitgegaan dat uw spraak-Service-abonnement in de regio VS-West. Als u een andere regio, werk de waarde voor `uri`. Zie voor een volledige lijst [regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieer deze code naar uw project:

```javascript
function textToSpeech(subscriptionKey, saveAudio) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    };
    // This function retrieve the access token and is passed as callback
    // to request below.
    function getToken(error, response, body) {
        console.log("Getting your token...\n")
        if (!error && response.statusCode == 200) {
            //This is the callback to our saveAudio function.
            // It takes a single argument, which is the returned accessToken.
            saveAudio(body)
        }
        else {
          throw new Error(error);
        }
    }
    request(options, getToken)
}
```

> [!NOTE]
> Zie voor meer informatie over verificatie [verifiëren met een toegangstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Een aanvraag indienen en het antwoord opslaan

U gaat hier de aanvraag voor de Text to Speech-API bouwen en sla het antwoord van spraak. In dit voorbeeld wordt ervan uitgegaan dat u het eindpunt van de VS-West. Als de bron naar een andere regio is geregistreerd, moet u bijwerken de `uri`. Zie voor meer informatie, [Spraakservice regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Vervolgens moet u vereiste headers voor de aanvraag toevoegen. Zorg ervoor dat bij te werken `User-Agent` met de naam van uw resource (te vinden in Azure portal) en stel `X-Microsoft-OutputFormat` aan uw gewenste audio-uitvoer. Zie voor een volledige lijst van de uitvoerindeling op te geven, [Audio-uitvoer](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#audio-outputs).

Vervolgens maken de hoofdtekst van de aanvraag met behulp van spraak synthese Markup Language (SSML). In dit voorbeeld definieert u de structuur en maakt gebruik van de `text` invoer u eerder hebt gemaakt.

>[!NOTE]
> In dit voorbeeld wordt de `JessaRUS` spraakstijl. Voor een volledige lijst van door Microsoft geleverd stemmen/talen, Zie [taalondersteuning](language-support.md).
> Als u geïnteresseerd bent in het maken van een unieke, herkenbare stem voor uw merk, Zie [het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md).

Ten slotte maakt u een aanvraag naar de service. Als de aanvraag voltooid is en een 200-statuscode is geretourneerd, wordt het antwoord spraak als geschreven `sample.wav`.

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function saveAudio(accessToken) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
      .att('version', '1.0')
      .att('xml:lang', 'en-us')
      .ele('voice')
      .att('xml:lang', 'en-us')
      .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
      .txt(text)
      .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    };
    // This function makes the request to convert speech to text.
    // The speech is returned as the response.
    function convertText(error, response, body){
      if (!error && response.statusCode == 200) {
        console.log("Converting text-to-speech. Please hold...\n")
      }
      else {
        throw new Error(error);
      }
      console.log("Your file is ready.\n")
    }
    // Pipe the response to file.
    request(options, convertText).pipe(fs.createWriteStream('sample.wav'));
}
```

## <a name="put-it-all-together"></a>Alles samenvoegen

U bent bijna klaar. De laatste stap is om aan te roepen de `textToSpeech` functie.

```javascript
// Start the sample app.
textToSpeech(subscriptionKey, saveAudio);
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Dat is alles, u kunt nu uw Text to Speech voorbeeldapp uit te voeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
node tts.js
```

Wanneer u hierom wordt gevraagd, typt u in alles wat u wilt converteren van tekst naar spraak. Als dit lukt, wordt de spraak-bestand bevindt zich in de projectmap. Spelen met behulp van uw favoriete MediaPlayer.

## <a name="clean-up-resources"></a>Resources opschonen

Denk eraan dat u eventuele vertrouwelijke informatie, zoals abonnementssleutels, verwijdert uit de broncode van de voorbeeld-app.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Node.js-voorbeelden op GitHub verkennen](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie voor de Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)
* [Het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md)
* [Record stem voorbeelden voor het maken van een aangepaste spraak](record-custom-voice-samples.md)
