---
title: Het gesegmenteerde overdracht audiostroom | Microsoft Docs
description: Het gebruik van gesegmenteerde TransferSQLDatabase audiostroom verzenden naar de service spraak
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 7d02340932dfc547893c4c40cbe08978b7b93756
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344670"
---
# <a name="chunked-transfer-encoding"></a>Gedeelde codering voor overdracht

Om transcriberen met gebruikmaking van spraak naar tekst, kunt Microsoft-spraakherkenning API u de audio verzenden als een hele chunk of de audio afgeknipt in kleine stukken verdeeld. Voor streaming-efficiënte audio en korte wachttijden schrijffouten, wordt aanbevolen dat u [gesegmenteerde overdrachtscodering](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) om te streamen audio naar de service. Andere implementaties kunnen leiden tot hogere latentie gebruiker waargenomen. Zie voor meer informatie de [Audio Streams](../concepts.md#audio-streams) pagina.

> [!NOTE]
> U mag niet meer dan 10 seconden audio in elk één verzoek uploaden en de duur van de totale aanvraag 14 seconden niet overschrijden.
> [!NOTE]
> U moet opgeven van de gesegmenteerde overdrachtscodering alleen als u de [REST-API's](../GetStarted/GetStartedREST.md) de spraak service aanroepen. Toepassingen die gebruikmaken van [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) hoeft niet te configureren van de gedeelde codering voor overdracht.

De volgende code laat zien hoe de gesegmenteerde overdrachtscodering instellen en voor het verzenden van een audiobestand wordt gesegmenteerde in stukken verdeeld 1024 bytes.

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
