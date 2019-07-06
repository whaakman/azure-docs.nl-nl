---
title: Stream codec gecomprimeerd audio met de spraak-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Meer informatie over het streamen van gecomprimeerde audio naar Azure Speech Services met de spraak-SDK. Beschikbaar voor C++ C#, en Java voor Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: d23190dc8f7980cb8a94ba295f45ae67fc7d4678
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605097"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Audio-invoer met de spraak-SDK met codec gecomprimeerd

De spraak-SDK **gecomprimeerde Audio-invoer Stream** API biedt een manier om te streamen gecomprimeerde audio naar de Speech-Service met behulp van PullStream of PushStream.

> [!IMPORTANT]
> Streaming van gecomprimeerde audio wordt alleen ondersteund voor C++, C#, en Java in Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9).
> Spraak SDK versie 1.4.0 of hoger vereist.

Raadpleeg de spraakdocumentatie hoofdlijnen voor wav PCM /.  Buiten wav/PCM, worden de volgende codec gecomprimeerde invoer indelingen ondersteund:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Vereisten voor het gebruik van codec gecomprimeerd audio-invoer

Deze extra afhankelijkheden voor het gebruik van gecomprimeerde audio-invoer met de spraak-SDK voor Linux installeren:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Van voorbeeldcode met codec gecomprimeerd audio-invoer

Om te streamen in een gecomprimeerde audio-indeling met de Speech Services, maken `PullAudioInputStream` of `PushAudioInputStream`. Vervolgens maakt u een `AudioConfig` van een exemplaar van uw stream-klasse, op te geven de compressie-indeling van de stroom.

Stel dat u een invoerstroom klasse genaamd hebt `myPushStream` en OPUS/OGG gebruikt. Uw code ziet er als volgt:

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

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
