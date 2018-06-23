---
title: Python-Snelstartgids voor Azure cognitieve Services, Microsoft Translator Speech-API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Microsoft Translator Speech-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 29722b3fa7fe61aff0b2406002453020d999ea41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344677"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>Quick Start voor Microsoft Translator spraak API met behulp van Python 
<a name="HOLTop"></a>

Dit artikel laat zien hoe u met de Microsoft Translator Speech-API woorden gesproken wav-bestanden.

## <a name="prerequisites"></a>Vereisten

U moet [Python 3.x](https://www.python.org/downloads/) deze code uit te voeren.

U moet installeren de [websocket-clientpakket](https://pypi.python.org/pypi/websocket-client) voor Python.

U moet een wav-bestand met de naam 'speak.wav' in dezelfde map als het uitvoerbare bestand dat van de volgende code worden gecompileerd. Dit WAV-bestand moet zich in de standaard PCM, 16-bits, 16kHz mono-indeling. U ontvangt deze een wav-bestand van de [conversieprogramma tekst uitspreken API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Microsoft Translator Speech-API**. U moet een betaald abonnementssleutel van uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

De volgende code vertaalt spraak van één taal.

1. Maak een nieuwe Python-project in uw favoriete IDE.
2. Voeg de code hieronder.
3. Vervang de `key` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

```python
# -*- coding: utf-8 -*-

# To install this package, run:
#   pip install websocket-client
import websocket

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'wss://dev.microsofttranslator.com'
path = '/speech/translate';
params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa'
uri = host + path + params

input_file = 'speak.wav'
output_file = 'speak2.wav'

output = bytearray ()

def on_open (client):
    print ("Connected.")

# r = read. b = binary.
    with open (input_file, mode='rb') as file:
        data = file.read()

    print ("Sending audio.")
    client.send (data, websocket.ABNF.OPCODE_BINARY)
# Make sure the audio file is followed by silence.
# This lets the service know that the audio input is finished.
    print ("Sending silence.")
    client.send (bytearray (32000), websocket.ABNF.OPCODE_BINARY)

def on_data (client, message, message_type, is_last):
    global output
    if (websocket.ABNF.OPCODE_TEXT == message_type):
        print ("Received text data.")
        print (message)
# For some reason, we receive the data as type websocket.ABNF.OPCODE_CONT.
    elif (websocket.ABNF.OPCODE_BINARY == message_type or websocket.ABNF.OPCODE_CONT == message_type):
        print ("Received binary data.")
        print ("Is last? " + str(is_last))
        output = output + message
        if (True == is_last):
# w = write. b = binary.
            with open (output_file, mode='wb') as file:
                file.write (output)
                print ("Wrote data to output file.")
            client.close ()
    else:
        print ("Received data of type: " + str (message_type))

def on_error (client, error):
    print ("Connection error: " + str (error))

def on_close (client):
    print ("Connection closed.")

client = websocket.WebSocketApp(
    uri,
    header=[
        'Ocp-Apim-Subscription-Key: ' + key
    ],
    on_open=on_open,
    on_data=on_data,
    on_error=on_error,
    on_close=on_close
)

print ("Connecting...")
client.run_forever()
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
