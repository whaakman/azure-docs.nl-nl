---
title: Spraak SDK audio invoerstroom concepten
description: Een overzicht van de mogelijkheden van de spraak-SDK audio-invoerstroom API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: 6c2d7c5787305f60b73ab83ea17367b04e03ac12
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985177"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>Stream over het audio-invoer voor spraak SDK API

De spraak-SDK **Audio-invoer Stream** API biedt een manier om te streamen van audio stromen naar de kenmerken in plaats van de microfoon of het invoerbestand API's.

De volgende stappen zijn vereist wanneer met behulp van audio invoerstromen:

- Identificeer de indeling van de audio-stream. De indeling moet worden ondersteund door de spraak-SDK en de spraak-service. Alleen de volgende configuratie wordt momenteel ondersteund:

  Audio-voorbeelden in PCM-indeling, één kanaal, 16000 voorbeelden per seconde, 32000 bytes per seconde, uitlijnen twee blok (16 bits met inbegrip van de opvulling voor het voorbeeld), 16 bits per voorbeeld.

  De bijbehorende code in de SDK om te maken van de audio-indeling ziet er als volgt:

  ```
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000;
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Zorg ervoor dat uw code krijgt u de ONBEWERKTE gegevens op basis van deze specificaties. Als uw gegevens audiobron komt niet overeen met de ondersteunde indelingen, moet de audio getranscodeerd in de vereiste indeling.

- Maak uw eigen audio invoerstroom klasse afgeleid van `PullAudioInputStreamCallback`. Implementeer de `Read()` en `Close()` leden. De exacte functiehandtekening is afhankelijk van de taal, maar de code ziet eruit als dit codevoorbeeld:

  ```
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public size_t Read(byte *buffer, size_t size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Maak een audio-configuratie op basis van uw audio-indeling en invoer-stream. In de configuratie van uw normale spraak zowel de audio-invoer configuratie doorgeven bij het maken van uw herkenning. Bijvoorbeeld:

  ```
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
