---
title: Stream-codec gecomprimeerde audio met de Speech SDK-Speech-Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het streamen van gecomprimeerde audio naar Azure speech Services met de spraak-SDK. Beschikbaar voor C++, C#en Java voor Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: b29b42dea9522526d49c1bda017a522855946def
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559548"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Gecomprimeerde audio-invoer van codec gebruiken met de Speech SDK

De **gecomprimeerde audio-invoer stroom** -API van de Speech SDK biedt een manier om gecomprimeerde audio naar de spraak service te streamen met behulp van PullStream of PushStream.

> [!IMPORTANT]
> Gecomprimeerde audio stromen worden alleen ondersteund voor C++, C#en Java op Linux (Ubuntu 16,04, Ubuntu 18,04, Debian 9).
> Speech SDK-versie 1.4.0 of hoger is vereist.

Zie de mainline speech-documentatie voor WAV/PCM.  Buiten WAV/PCM worden de volgende in de codec gecomprimeerde invoer indelingen ondersteund:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Vereisten voor het gebruik van gecomprimeerde audio-invoer van codec

Installeer deze extra afhankelijkheden voor het gebruik van gecomprimeerde audio-invoer met de Speech SDK voor Linux:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Voorbeeld code met behulp van gecomprimeerde audio-invoer van de codec

Als u een gecomprimeerde audio-indeling naar de spraak Services wilt streamen `PushAudioInputStream`, maakt `PullAudioInputStream` of. Maak vervolgens een `AudioConfig` van een exemplaar van uw Stream-klasse, waarbij u de compressie-indeling van de stroom opgeeft.

We gaan ervan uit dat u een invoer stroom klasse hebt `myPushStream` aangeroepen en gebruikmaakt van Opus/OGG. Uw code kan er als volgt uitzien:

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
