---
title: Gecomprimeerde Stream audio met de spraak-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Meer informatie over het streamen van gecomprimeerde audio naar Azure Speech Services met de spraak-SDK. Beschikbaar voor C++ C#, en Java voor Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: 2066dc3e20ab9fc92b23fd071728ea6a920d3324
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012420"
---
# <a name="stream-compressed-audio-with-the-speech-sdk"></a>Gecomprimeerde Stream audio met de spraak-SDK

De spraak-SDK **gecomprimeerde Audio-invoer Stream** API biedt een manier om te streamen gecomprimeerde audio naar de Speech-Service met behulp van PullStream of PushStream.

> [!IMPORTANT]
> Streaming van gecomprimeerde audio wordt alleen ondersteund voor C++ C#, en Java in Linux (Ubuntu 16.04 of Ubuntu 18.04).
> Ondersteuning is beperkt tot de MP3- en OPUS/OGG.

## <a name="prerequisites"></a>Vereisten

U moet deze afhankelijkheden voor het gebruik van gecomprimeerde audio-invoer met de spraak-SDK voor Linux installeren:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="streaming-compressed-audio"></a>Gecomprimeerde audio streamen

Om te streamen in een gecomprimeerde audio-indeling met de Speech Services, maken `PullAudioInputStream` of `PushAudioInputStream`. Vervolgens maakt u een `AudioConfig` van een exemplaar van uw stream-klasse, op te geven de compressie-indeling van de stroom.

Stel dat u een invoerstroom klasse genaamd hebt `myPushStream` en OPUS/OGG gebruikt. Dit is wat de code kan er als volgt uitzien: 

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
