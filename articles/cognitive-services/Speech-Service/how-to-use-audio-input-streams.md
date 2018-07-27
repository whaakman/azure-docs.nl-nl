---
title: AudioInputStream-concepten
description: Een overzicht van de mogelijkheden van de API AudioInputStream.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: b3e12fbc616c8d67b557102c6094467e119a23f1
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281902"
---
# <a name="about-the-audio-input-stream-api"></a>Over de audio-invoer stream API

De **Audio-invoer Stream** API biedt een manier om te streamen van audio stromen naar de kenmerken in plaats van de microfoon of het invoerbestand API's.

## <a name="api-overview"></a>API-overzicht

De API maakt gebruik van twee onderdelen: de `AudioInputStream` (de onbewerkte audiogegevens) en de `AudioInputStreamFormat`.

De `AudioInputStreamFormat` definieert de indeling van de gegevens. Deze kan worden vergeleken met de standaard `WAVEFORMAT` structuur voor wave bestanden op Windows.

  - `FormatTag`

    De indeling van de audio. De spraak-SDK ondersteunt momenteel alleen `format 1` (PCM - weinig endian).

  - `Channels`

    Het aantal kanalen. De huidige speech-service ondersteunt slechts één kanaal (mono) audio materiaal.

  - `SamplesPerSec`

    De samplefrequentie. Een typische microfoon opname heeft 16000 voorbeelden per seconde.

  - `AvgBytesPerSec`

    Gemiddelde aantal bytes per seconde, berekend als `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Gemiddelde aantal bytes dat per seconde kunnen afwijken voor audiostreams die variabele bitsnelheden gebruiken.

  - `BlockAlign`

    De grootte van één frame, berekend als `Channels * ceil(wBitsPerSample, 8)`. Vanwege opvulling, kan de werkelijke waarde hoger is dan deze waarde zijn.

  - `BitsPerSample`

    Het aantal bits per voorbeeld. Een typische audiostream maakt gebruik van 16 bits per voorbeeld (CD kwaliteit).

De `AudioInputStream` basisklasse wordt overschreven door uw aangepaste stream-adapter. Deze adapter is voor het implementeren van deze functies:

   - `GetFormat()`

     Deze functie is aangeroepen om op te halen van de indeling van de audio-stream. Het wordt een verwijzing naar de buffer AudioInputStreamFormat.

   - `Read()`

     Deze functie is aangeroepen voor het ophalen van gegevens uit de audio-stream. Een parameter is een verwijzing naar de buffer voor het kopiëren van de gegevens in. De tweede parameter is de grootte van de buffer. De functie retourneert het aantal bytes dat is gekopieerd naar de buffer. Een retourwaarde van `0` geeft aan het einde van de stroom.

   - `Close()`

     Deze functie is aangeroepen om te sluiten van de audio-stream.

## <a name="usage-examples"></a>Voorbeelden van het gebruik

In het algemeen zijn de volgende stappen uit betrokken bij het gebruik van invoer audiostreams:

  - Identificeer de indeling van de audio-stream. De indeling moet worden ondersteund door de SDK en de spraakservice. De volgende configuratie wordt momenteel ondersteund:

    Een audio-indeling tag (PCM), één kanaal, 16000 voorbeelden per seconde, 32000 bytes per seconde, twee blok uitlijnen (16 bits met inbegrip van de opvulling voor het voorbeeld), 16 bits per voorbeeld

  - Zorg ervoor dat uw code krijgt u de ONBEWERKTE gegevens over de specificaties van de hierboven aangegeven. Als uw gegevens audiobron komt niet overeen met de ondersteunde indelingen, moet de audio getranscodeerd in de vereiste indeling.

  - Uw aangepaste invoer audiostream klasse afleiden `AudioInputStream`. Implementeer de `GetFormat()`, `Read()`, en `Close()` bewerking. De exacte functiehandtekening is afhankelijk van de taal, maar de code ziet eruit als dit codevoorbeeld::

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

  - Gebruik uw audio-invoer stream:

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

  - In sommige talen kunt de `contosoStream` moeten expliciet worden verwijderd nadat de opname voltooid is. U kunt de AudioStream niet vrijgeven voordat de volledige invoer wordt gelezen. In een scenario met `StopContinuousRecognitionAsync` en `StopContinuousRecognitionAsync` hiervoor een concept in dit voorbeeld wordt geïllustreerd:

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

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
