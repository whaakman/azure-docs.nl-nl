---
title: C#-Snelstartgids voor Azure cognitieve Services, Microsoft Translator Speech-API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Microsoft Translator Speech-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 6489a0be72cedffdfa4f7021f889b2d39c1c358f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344679"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-c"></a>Quick Start voor Microsoft Translator spraak API met C# 
<a name="HOLTop"></a>

Dit artikel laat zien hoe u met de Microsoft Translator Speech-API woorden gesproken wav-bestanden.

## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) naar deze code in Windows worden uitgevoerd. (De editie free Community blijft werken.)

U moet een wav-bestand met de naam 'speak.wav' in dezelfde map als het uitvoerbare bestand dat van de volgende code worden gecompileerd. Dit WAV-bestand moet zich in de standaard PCM, 16-bits, 16kHz mono-indeling. U ontvangt deze een wav-bestand van de [conversieprogramma tekst uitspreken API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Microsoft Translator Speech-API**. U moet een betaald abonnementssleutel van uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

De volgende code vertaalt spraak van één taal.

1. Maak een nieuwe C#-project in uw favoriete IDE.
2. Voeg de code hieronder.
3. Vervang de `key` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

```csharp
using System;
using System.IO;
using System.Net.WebSockets;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace TranslateSpeechQuickStart
{
    class Program
    {
        static string host = "wss://dev.microsofttranslator.com";
        static string path = "/speech/translate";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        async static Task Send (ClientWebSocket client, string input_path)
        {
            var audio = File.ReadAllBytes(input_path);
            var audio_out_buffer = new ArraySegment<byte>(audio);
            Console.WriteLine("Sending audio.");
            await client.SendAsync(audio_out_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            /* Make sure the audio file is followed by silence.
             * This lets the service know that the audio input is finished. */
            var silence = new byte[3200000];
            var silence_buffer = new ArraySegment<byte>(silence);
            await client.SendAsync(silence_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            Console.WriteLine("Done sending.");
            await client.CloseAsync(WebSocketCloseStatus.NormalClosure, "", CancellationToken.None);
        }

        async static Task Receive(ClientWebSocket client, string output_path)
        {
            var inbuf = new byte[102400];
            var segment = new ArraySegment<byte>(inbuf);
            var stream = new FileStream(output_path, FileMode.Create);

            Console.WriteLine("Awaiting response.");
            while (client.State == WebSocketState.Open)
            {
                var result = await client.ReceiveAsync(segment, CancellationToken.None);
                switch (result.MessageType)
                {
                    case WebSocketMessageType.Close:
                        Console.WriteLine("Received close message. Status: " + result.CloseStatus + ". Description: " + result.CloseStatusDescription);
                        await client.CloseAsync(WebSocketCloseStatus.NormalClosure, string.Empty, CancellationToken.None);
                        break;
                    case WebSocketMessageType.Text:
                        Console.WriteLine("Received text.");
                        Console.WriteLine(Encoding.UTF8.GetString(inbuf).TrimEnd('\0'));
                        break;
                    case WebSocketMessageType.Binary:
                        Console.WriteLine("Received binary data: " + result.Count + " bytes.");
                        stream.Write(inbuf, 0, result.Count);
                        break;
                }
            }

            stream.Close();
            stream.Dispose();
        }

        async static void TranslateSpeech()
        {
            var client = new ClientWebSocket();
            client.Options.SetRequestHeader ("Ocp-Apim-Subscription-Key", key);

            string from = "en-US";
            string to = "it-IT";
            string features = "texttospeech";
            string voice = "it-IT-Elsa";
            string api = "1.0";

            string input_path = "speak.wav";
            string output_path = "speak2.wav";

            string uri = host + path +
                "?from=" + from +
                "&to=" + to +
                "&api-version=" + api +
                "&features=" + features +
                "&voice=" + voice;

            Console.WriteLine("uri: " + uri);
            Console.WriteLine("Opening connection.");
            await client.ConnectAsync(new Uri(uri), CancellationToken.None);
            Console.WriteLine("Connection open.");
            Task.WhenAll(Send(client, input_path), Receive(client, output_path)).Wait();
        }

        static void Main()
        {
            TranslateSpeech();
            Console.ReadLine();
        }

    }
}
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
