---
title: Meerdere deelnemers gesprekken met de spraak-SDK - spraakservices transcriberen
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van conversatie transcriptie service met de spraak-SDK. Beschikbaar voor C++, C#, en Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: jhakulin
ms.openlocfilehash: 73ab4cfa92a1efc49dea16ba2941cf16b7a1cf3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025790"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Transcriberen meerdere deelnemers gesprekken met de spraak-SDK

De spraak-SDK **ConversationTranscriber** -API kunt u vergaderingen/gesprekken transcriberen met de mogelijkheid om toevoegen, verwijderen en deelnemers identificeren door het streamen van audio voor het gebruik van de spraakservices `PullStream` of `PushStream`.

## <a name="limitations"></a>Beperkingen

* Conversatie transcriber wordt ondersteund voor C++, C#, en Java in Windows, Linux- en Android.
* De DevKit ROOBO is de ondersteunde hardware-omgeving voor het maken van conversaties dat circulaire meerdere microfoon matrix die efficiënt kan worden gebruikt door de conversatie transcriptie-service voor de sprekeridentificatie biedt. [Zie voor meer informatie, spraak Devices SDK](speech-devices-sdk.md). 
* Spraak SDK-ondersteuning is beperkt tot gebruik voor pull-audio en push-modus-streams met acht kanalen van PCM-audio.

## <a name="prerequisites"></a>Vereisten

* [Informatie over het gebruik van spraak-naar-tekst met de spraak-SDK.](quickstart-csharp-dotnet-windows.md)
* [Uw proefabonnement voor spraak ophalen.](https://azure.microsoft.com/try/cognitive-services/)

## <a name="create-voice-signatures-for-participants"></a>Stem handtekeningen voor deelnemers maken

De eerste stap is het maken van spraak handtekeningen voor de conversatie-deelnemers. Het maken van spraak handtekeningen is vereist voor efficiënte sprekeridentificatie.
In het volgende voorbeeld wordt [de REST API gebruiken om op te halen van de handtekening van spraak.](https://aka.ms/cts/signaturegenservice)

Het volgende voorbeeld ziet u twee verschillende manieren om te maken van handtekeningen voor spraak:
```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://{region}.signature.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature can be extracted from the jsonData
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature can be extracted from the jsonData
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
    private static string endpoint = "YourOwnEndpoint";

    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own endpoint and subscription key.
        var config = SpeechConfig.FromEndpoint(new Uri(endpoint), "YourSubScriptionKey");
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
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, SpeakerID={e.Result.SpeakerId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create data for voice signatures using REST API described in the earlier section in this document.
                // How to create voice signatureA, signatureB & signatureC variables, please check the SDK API samples.

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
