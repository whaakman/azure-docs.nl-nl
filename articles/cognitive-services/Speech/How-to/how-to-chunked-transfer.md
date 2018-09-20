---
title: Het gesegmenteerde overdracht Audio Stream | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Het gebruik van gesegmenteerde TransferSQLDatabase audiostream verzenden naar de Bing Speech-service
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 1796a82b85182e6d231cb0bf1536cda2406e2c53
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364904"
---
# <a name="chunked-transfer-encoding"></a>Gesegmenteerde overdrachtscodering

Om te transcriberen spraak naar tekst, kunt u voor het verzenden van de audio als één geheel segment of de audio in kleine stukken afgeknipt Microsoft the spraakherkennings-API. Voor het efficiënt audio streamen en transcriptie latentie wordt verminderd, is het raadzaam dat u [gesegmenteerde overdrachtscodering](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) het streamen van audio naar de service. Andere implementaties kunnen leiden tot hogere latentie van de gebruiker beschouwd. Zie voor meer informatie de [Audio Streams](../concepts.md#audio-streams) pagina.

> [!NOTE]
> U kunt niet meer dan 10 seconden van de audio in elk één verzoek uploaden en duur van de totale aanvraag mag niet meer dan 14 seconden.
> [!NOTE]
> U moet opgeven van de gesegmenteerde overdrachtscodering alleen als u de [REST-API's](../GetStarted/GetStartedREST.md) de spraakservice aanroepen. Toepassingen die gebruikmaken van [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) hoeft niet te configureren van de gesegmenteerde overdrachtscodering.

De volgende code laat zien hoe de gesegmenteerde overdrachtscodering instellen en voor het verzenden van een geluidsbestand wordt gesegmenteerde overdrachtscodering in segmenten van 1024 bytes.

```cs

HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```
