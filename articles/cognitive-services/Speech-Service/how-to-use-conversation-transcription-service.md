---
title: Meerdere deelnemers gesprekken met de spraak-SDK - spraakservices transcriberen
titleSuffix: Azure Cognitive Services
description: Informatie over het gesprek transcriptie gebruiken met de spraak-SDK. Beschikbaar voor C++, C#, en Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: jhakulin
ms.openlocfilehash: 215209a5b8e3ed46b25fbfa492c305785a9a0070
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606470"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Transcriberen meerdere deelnemers gesprekken met de spraak-SDK

De spraak-SDK **ConversationTranscriber** -API kunt u vergaderingen/gesprekken transcriberen met de mogelijkheid om toevoegen, verwijderen en deelnemers identificeren door het streamen van audio voor het gebruik van de spraakservices `PullStream` of `PushStream`.

## <a name="limitations"></a>Beperkingen

* Conversatie transcriber wordt ondersteund voor C++, C#, en Java in Windows, Linux- en Android.
* De DevKit ROOBO is de ondersteunde hardware-omgeving voor het maken van de conversatie transcripties die efficiënt circulaire meerdere microfoon matrix die kan worden gebruikt biedt voor de sprekerherkenning-identificatie. [Zie voor meer informatie, spraak Devices SDK](speech-devices-sdk.md).
* Spraak SDK-ondersteuning voor transcriptie van de conversatie is beperkt tot gebruik voor pull-audio en push-modus-streams met acht kanalen van 16-bits 16 kHz PCM audio.
* Conversatie transcriptie is momenteel beschikbaar in 'en-US' en 'zh-CN' talen in de volgende regio's: centralus en Oost-Aziatische.

## <a name="prerequisites"></a>Vereisten

* [Informatie over het gebruik van spraak-naar-tekst met de spraak-SDK.](quickstart-csharp-dotnet-windows.md)
* [Uw proefabonnement voor spraak ophalen.](https://azure.microsoft.com/try/cognitive-services/)
* Spraak SDK-versie 1.5.1 of hoger is vereist.

## <a name="create-voice-signatures-for-participants"></a>Stem handtekeningen voor deelnemers maken

De eerste stap is het maken van spraak handtekeningen voor de conversatie-deelnemers. Het maken van spraak handtekeningen is vereist voor efficiënte sprekeridentificatie.

### <a name="requirements-for-input-wave-file"></a>Vereisten voor invoer wave-bestand

* Het wavebestand audio-invoer voor het maken van spraak handtekeningen worden in voorbeelden van 16-bits, 16 kHz samplefrequentie en een één-(Mono)-kanaalindeling.
* De lengte van de aanbevolen voor elk audio voorbeeld ligt tussen 30 seconden en twee minuten.

Het volgende voorbeeld ziet u twee verschillende manieren om te maken van stem-ondertekening door [met behulp van de REST-API](https://aka.ms/cts/signaturegenservice) van C#:

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>Te transcriberen gesprekken

Om te transcriberen gesprekken met meerdere deelnemers, maken de `ConversationTranscriber` -object dat gekoppeld aan de `AudioConfig` object gemaakt voor de conversatie-sessie en stream audio met behulp van `PullAudioInputStream` of `PushAudioInputStream`.

Stel dat u een ConversationTranscriber klasse genaamd hebt `MyConversationTranscriber`. Uw code ziet er als volgt:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document.
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)
