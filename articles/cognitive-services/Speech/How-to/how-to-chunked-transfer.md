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
ms.openlocfilehash: 510bc258aae80582e85478936c3281198e89d755
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343160"
---
# <a name="chunked-transfer-encoding"></a>Gesegmenteerde overdrachtscodering

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

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
