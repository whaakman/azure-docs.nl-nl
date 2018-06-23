---
title: Node.js-Snelstartgids voor Azure cognitieve Services, Microsoft Translator Speech-API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Microsoft Translator Speech-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: d469fa008ba8acaf505fa09596dd739d5cc7744c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344704"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-nodejs"></a>Quick Start voor Microsoft Translator spraak API met behulp van Node.js 
<a name="HOLTop"></a>

Dit artikel laat zien hoe u met de Microsoft Translator Speech-API woorden gesproken wav-bestanden.

## <a name="prerequisites"></a>Vereisten

U moet [Node.js 6](https://nodejs.org/en/download/) deze code uit te voeren.

U moet installeren de [Websocket pakket](https://www.npmjs.com/package/websocket) voor Node.js.

U moet een wav-bestand met de naam 'speak.wav' in dezelfde map als het uitvoerbare bestand dat van de volgende code worden gecompileerd. Dit WAV-bestand moet zich in de standaard PCM, 16-bits, 16 kHz mono-indeling. U ontvangt deze een wav-bestand van de [conversieprogramma tekst uitspreken API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Microsoft Translator Speech-API**. U moet een betaald abonnementssleutel van uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

De volgende code vertaalt spraak van één taal.

1. Maak een nieuwe Node.js-project in uw favoriete IDE.
2. Voeg de code hieronder.
3. Vervang de `key` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

```nodejs
/* To install this dependency, run:
npm install websocket
*/
var wsClient = require('websocket').client;
var fs = require('fs');
/* To install this dependency, run:
npm install stream-buffers
*/
var streamBuffers = require('stream-buffers');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let key = 'ENTER KEY HERE';

let host = 'wss://dev.microsofttranslator.com';
let path = '/speech/translate';
let params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa';
let uri = host + path + params;

/* The input .wav file is in PCM 16bit, 16kHz, mono format.
You can obtain such a .wav file using the Translator Text Speak API. See:
http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak
*/
let input_path = 'speak.wav';

let output_path = 'speak2.wav';

function receive(message) {
    console.log ("Received message of type: " + message.type + ".");

    if (message.type == 'utf8') {
        var result = JSON.parse(message.utf8Data)
        console.log("Response type: " + result.type + ".");
        console.log("Recognized input as: " + result.recognition);
        console.log("Translation: " + result.translation);
    }
    else if (message.type == 'binary') {
/* This is needed to make sure message.binaryData is defined. See:
https://stackoverflow.com/questions/17546953/cant-access-object-property-even-though-it-exists-returns-undefined
*/
        let binary_data = JSON.parse (JSON.stringify (message.binaryData));

        if (binary_data.type == 'Buffer') {
/* Put the binary data in a Buffer - do not write it directly to the file. */
            let buffer = new Buffer(binary_data.data, 'binary');
/* Write the contents of the Buffer to the file. */
            fs.writeFile (output_path, buffer, 'binary', function (error) {
/* fs.writeFile calls the error-handling function even if there is no error, in which case error === null. See:
https://stackoverflow.com/questions/44473868/node-js-fs-writefile-err-returns-null
*/
                if (error !== null) {
                    console.log ("Error: " + error);
                }
            });
        }
    }
}

function send(connection, filename) {
    
    var myReadableStreamBuffer = new streamBuffers.ReadableStreamBuffer({
        frequency: 100,
        chunkSize: 32000
    });

/* Make sure the audio file is followed by silence.
This lets the service know that the audio input is finished. */
    myReadableStreamBuffer.put(fs.readFileSync(filename));
    myReadableStreamBuffer.put(new Buffer(3200000));
    myReadableStreamBuffer.stop();

    myReadableStreamBuffer.on('data', function (data) {
        connection.sendBytes(data);
    });

    myReadableStreamBuffer.on('end', function () {
        console.log('Closing connection.');
        connection.close(1000);
    });
}

function connect() {
    var ws = new wsClient();

    ws.on('connectFailed', function (error) {
        console.log('Connection error: ' + error.toString());
    });
                            
    ws.on('connect', function (connection) {
        console.log('Connected.');

        connection.on('message', receive);
        
        connection.on('close', function (reasonCode, description) {
            console.log('Connection closed: ' + reasonCode);
        });

        connection.on('error', function (error) {
            console.log('Connection error: ' + error.toString());
        });
        
        send(connection, input_path);
    });

    ws.connect(uri, null, null, { 'Ocp-Apim-Subscription-Key' : key });
}

connect();
```

**Spraak antwoord vertalen**

Een geslaagde resultaat is het maken van een bestand met de naam 'speak2.wav'. Het bestand bevat de omzetting van de woorden 'speak.wav' gesproken.

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Conversieprogramma spraak-zelfstudie](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Zie ook 

[Conversieprogramma spraak-overzicht](../overview.md)
[API-referentiemateriaal](http://docs.microsofttranslator.com/speech-translate.html)
