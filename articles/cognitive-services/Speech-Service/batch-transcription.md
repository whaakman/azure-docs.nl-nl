---
title: Azure Batch transcriptie API
description: Voorbeelden
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 8f9a033ebf9cdfdb96ae8511b14202e49ec0a85e
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884456"
---
# <a name="batch-transcription"></a>Batchtranscriptie

Batch transcriptie is ideaal als u grote hoeveelheden van audio. U kunt verwijzen naar audio bestanden door de URI en transcripties teruggaan in asynchrone modus wordt uitgevoerd.

## <a name="batch-transcription-api"></a>Batch transcriptie API

De Batch-transcriptie API biedt asynchrone spraak naar tekst transcriptie, samen met extra functies.

> [!NOTE]
> De Batch-API voor transcriptie is ideaal voor callcenters, die meestal worden verzameld duizenden uur aan audio. De API wordt geleid door een 'geactiveerd en vergeten' filosofie, waardoor u gemakkelijk grote hoeveelheid audio-opnamen transcriberen.

### <a name="supported-formats"></a>Ondersteunde indelingen

De Batch-transcriptie API ondersteunt de volgende indelingen:

Naam| Kanaal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
wav |   Mono   |
wav |  Stereo  |

Voor stereo audiostreams, wordt de linker- en -kanaal in Batch transcriptie gesplitst tijdens de transcriptie. De twee JSON-bestanden met het resultaat zijn alle gemaakt van één kanaal. De tijdstempels per utterance bieden de ontwikkelaar te maken van een geordende definitieve transcriptie. De volgende JSON-voorbeeld wordt de uitvoer van een kanaal.

```json
       {
        "recordingsUrl": "https://mystorage.blob.core.windows.net/cris-e2e-datasets/TranscriptionsDataset/small_sentence.wav?st=2018-04-19T15:56:00Z&se=2040-04-21T15:56:00Z&sp=rl&sv=2017-04-17&sr=b&sig=DtvXbMYquDWQ2OkhAenGuyZI%2BYgaa3cyvdQoHKIBGdQ%3D",
        "resultsUrls": {
            "channel_0": "https://mystorage.blob.core.windows.net/bestor-87a0286f-304c-4636-b6bd-b3a96166df28/TranscriptionData/24265e4c-e459-4384-b572-5e3e7795221f?sv=2017-04-17&sr=b&sig=IY2qd%2Fkgtz2PwRe2C88BphH4Hv%2F1VCb1UVJ33xsw%2BEY%3D&se=2018-04-23T14:48:24Z&sp=r"
        },
        "statusMessage": "None.",
        "id": "0bb95356-ff06-469d-acc7-81f9144a269a",
        "createdDateTime": "2018-04-20T14:11:57.167",
        "lastActionDateTime": "2018-04-20T14:12:54.643",
        "status": "Succeeded",
        "locale": "en-US"
    },
```

> [!NOTE]
> De Batch-transcriptie API maakt gebruik van een REST-service voor het aanvragen van transcripties, hun status en de bijbehorende resultaten. U kunt de API vanuit een willekeurige taal gebruiken. De volgende sectie wordt beschreven hoe deze wordt gebruikt.

## <a name="authorization-token"></a>Autorisatietoken

Als met alle functies van de Speech-Service die u een abonnementssleutel van maakt de [Azure-portal](https://portal.azure.com) volgende onze [Get-Started guide](get-started.md). Als u van plan bent om op te halen transcripties van onze basislijn-modellen vervolgens is dit alles die wat u nodig om te doen. 

Als u van plan bent aanpassen en het gebruik van een aangepast model moet u deze sleutel subscritpion als volgt toevoegen aan de portal voor aangepaste spraak:

1. Aanmelden bij [aangepaste spraak](https://customspeech.ai).

2. Selecteer **Abonnementen**.

3. Selecteer **verbinding maken met bestaande abonnement**.

4. De abonnementssleutel en een alias toevoegen in de weergave die wordt weergegeven

    ![Schermafbeelding van de aangepaste spraak abonnementen pagina](media/stt/Subscriptions.jpg)

5. Kopieer en plak deze sleutel in de clientcode in het volgende voorbeeld.

> [!NOTE]
> Als u van plan bent te gebruiken van een aangepast model, moet u de ID van dit model te. Houd er rekening mee dat dit niet de eindpunt-ID die u op de detailweergave van het eindpunt vinden is. Het is de model-ID die u ophalen kunt wanneer u de details van dit model selecteert.

## <a name="sample-code"></a>Voorbeeldcode

Pas de volgende voorbeeldcode met een abonnementssleutel en een API-sleutel. Hiermee kunt u een bearer-token verkrijgen.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);
         
            return new CrisClient(client);
        }
```

Nadat u het token verkregen, moet u de SAS-URI die verwijst naar het audiobestand transcriptie vereisen. De rest van de code doorloopt de status en het resultaat wordt weergegeven.

```cs
   static async Task TranscribeAsync()
        { 
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // Creating a Batch transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);

            // get the transcription Id from the location URI
            var createdTranscriptions = new List<Guid>();
            createdTranscriptions.Add(new Guid(transcriptionLocation.ToString().Split('/').LastOrDefault()))

            while (true)
            {
                // get all transcriptions for the user
                transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);
                completed = 0; running = 0; notStarted = 0;

                // for each transcription in the list we check the status
                foreach (var transcription in transcriptions)
                {
                    switch(transcription.Status)
                    {
                        case "Failed":
                        case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                            if (!createdTranscriptions.Contains(transcription.Id))
                            {
                                // not creted form here, continue
                                continue;
                            }
                            
                            completed++;
                            
                            // if the transcription was successfull, check the results
                            if (transcription.Status == "Succeeded")
                            {
                                var resultsUri = transcription.ResultsUrls["channel_0"];
                                WebClient webClient = new WebClient();
                                var filename = Path.GetTempFileName();
                                webClient.DownloadFile(resultsUri, filename);
                                var results = File.ReadAllText(filename);
                                Console.WriteLine("Transcription succedded. Results: ");
                                Console.WriteLine(results);
                            }
                            break;
                        case "Running":
                            running++;
                            break;
                        case "NotStarted":
                            notStarted++;
                            break;
                    }
                }

                Console.WriteLine(string.Format("Transcriptions status: {0} completed, {1} running, {2} not started yet", completed, running, notStarted));

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.WriteLine("Press any key...");
        }
```

> [!NOTE]
> In de bovenstaande code wordt de abonnementssleutel is van de spraak-resource die u in Azure portal maakt. Sleutels die zijn verkregen van de resource Custom Speech Service werken niet.

U ziet de asynchrone instellingen voor het plaatsen van audio en transcriptie status ontvangen. De client die gemaakt is een .NET-Http-client. Er is een `PostTranscriptions` methode voor het verzenden van de details van de audio-bestand en een `GetTranscriptions` methode om de resultaten te ontvangen. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` gebruikt deze ingang voor het maken van een greep om op te halen van de status van de transcriptie.

De huidige voorbeeldcode geeft geen aangepaste modellen. De service maakt gebruik van de basislijn-modellen voor te transcriberen van het bestand of de bestanden. Als u wilt de modellen opgeven, kunt u op dezelfde manier de model-id voor de akoestische en het taalmodel doorgeven. 

Als u niet wilt gebruiken van de basislijn, moet u de model-id's voor zowel akoestische en modellen doorgeven.

> [!NOTE]
> Voor basislijn transcriptie hebt u geen om aan te geven van de eindpunten van de basislijn-modellen. Als u gebruiken van aangepaste modellen wilt, bieden u de eindpunten id's als de [voorbeeld](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Als u een akoestisch basislijn gebruiken met een basislijn taalmodel wilt, hoeft u alleen te declareren van het aangepaste model endpoint-ID. Microsoft detecteert het model voor de partner basislijn (niet het akoestisch of taal), en die wordt gebruikt om te voldoen aan de aanvraag transcriptie.

### <a name="supported-storage"></a>Ondersteunde opslag

De enige opslag wordt ondersteund is momenteel Azure Blob-opslag.

## <a name="downloading-the-sample"></a>Het voorbeeld downloaden

Het voorbeeld hieronder is op [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Normaal gesproken vereist een audiotranscriptie dat een tijdsduur die gelijk is aan de duur van het audiobestand, plus de overhead van een 2-3 minuten.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
