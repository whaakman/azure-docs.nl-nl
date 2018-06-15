---
title: AudioInputStream concepten | Microsoft Docs
description: Een overzicht van de mogelijkheden van de API AudioInputStream.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
manager: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: 528356473c4221a815fa68cbec3426866c4cbd23
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35350013"
---
# <a name="about-the-audio-input-stream-api"></a>Over de audio-invoer stream API

De **Audio-invoer stroom** API biedt een manier om te streamen audio streams in de kenmerken in plaats van de microfoon of het bestand voor invoer API's.

## <a name="api-overview"></a>API-overzicht

De API maakt gebruikt van twee onderdelen: de `AudioInputStream` (de onbewerkte audiogegevens) en de `AudioInputStreamFormat`.

De `AudioInputStreamFormat` definieert de indeling van de gegevens. Het kan worden vergeleken op de standaard `WAVEFORMAT` structuur voor wave-bestanden in Windows.

  - `FormatTag`

    De indeling van de audio. De SDK spraak ondersteunt momenteel alleen `format 1` (PCM - little endian).

  - `Channels`

    Het aantal kanalen. De huidige spraak-service ondersteunt slechts één kanaal (mono) audio materiaal.

  - `SamplesPerSec`

    De samplefrequentie. Een typische microfoon opname heeft 16000 samples per seconde.

  - `AvgBytesPerSec`

    Gemiddelde aantal bytes per seconde worden berekend als `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Gemiddelde aantal bytes per seconde kunnen afwijken voor audio stromen die variabele bitsnelheden gebruiken.

  - `BlockAlign`

    De grootte van een enkel frame berekend als `Channels * ceil(wBitsPerSample, 8)`. Als gevolg van de opvulling, kan de werkelijke waarde hoger is dan deze waarde zijn.

  - `BitsPerSample`

    Het aantal bits per voorbeeld. Een typische audiostroom maakt gebruik van 16 bits per sample (kwaliteit CD).

De `AudioInputStream` basisklasse wordt overschreven door uw aangepaste stroomadapter. Deze adapter is voor het implementeren van deze functies:

   - `GetFormat()`

     Deze functie wordt aangeroepen om de indeling van de audiostroom. Een verwijzing krijgt naar de buffer AudioInputStreamFormat.

   - `Read()`

     Deze functie is aangeroepen voor het ophalen van gegevens uit de audiostroom. Een parameter is een verwijzing naar de buffer voor het kopiëren van de gegevens in. De tweede parameter is de grootte van de buffer. De functie retourneert het aantal bytes dat wordt gekopieerd naar de buffer. Een retourwaarde van `0` geeft aan het einde van de stroom.

   - `Close()`

     Deze functie wordt aangeroepen om te sluiten van de audiostroom.

## <a name="usage-examples"></a>Gebruiksvoorbeelden

In het algemeen zijn de volgende stappen betrokken bij gebruik van audio-invoer streams:

  - De indeling van de audiostroom identificeren. De indeling moet worden ondersteund door de SDK en de service spraak. De volgende configuratie wordt momenteel ondersteund:

    Label van een audio-indeling (PCM), een kanaal, 16000 samples per seconde, 32000 bytes per seconde, twee blok uitlijnen (16-bits inclusief opvulling voor een voorbeeld), 16 bits per voorbeeld

  - Zorg ervoor dat uw code krijgt u de ONBEWERKTE gegevens over specificaties hierboven. Als de audio brongegevens komt niet overeen met de ondersteunde indelingen, moet de audio getranscodeerd naar de vereiste indeling.

  - Uw aangepaste audio invoerstroom klasse afgeleid `AudioInputStream`. Implementeer de `GetFormat()`, `Read()`, en `Close()` bewerking. De exacte functiehandtekening taal afhankelijk is, maar de code ziet er ongeveer als dit codevoorbeeld::

    ```
     public class ContosoAudioStream : AudioInputStream {
        ContosoConfig config;

        public ContosoAudioStream(const ContosoConfig& config) {
            this.config = config;
        }

        public void GetFormat(AudioInputStreamFormat& format) {
            // returns format data to the caller.
            // e.g. format.FormatTag = config.XXX;
            // ...
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

  - Uw invoer audiostroom gebruiken:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    var result = await recognizer.RecognizeAsync();

    var text = result.GetText();

    // In some languages you need to delete the stream explicitly.
    // delete contosoStream;
    ```

  - In sommige talen de `contosoStream` moeten expliciet worden verwijderd nadat de erkenning voltooid is. U kunt de AudioStream niet vrijgeven voordat de volledige invoer wordt gelezen. In een scenario met `StopContinuousRecognitionAsync` en `StopContinuousRecognitionAsync` vereist een concept geïllustreerd in dit voorbeeld:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    await recognizer.StartContinuousRecognitionAsync();

    // ERROR: do not delete the contosoStream before ending recognition!
    // delete contosoStream;

    await recognizer.StopContinuousRecognitionAsync();

    // OK: Safe to delete the contosoStream.
    // delete contosoStream;
    ```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
