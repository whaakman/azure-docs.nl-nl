---
title: Azure Batch transcriptie API | Azure Microsoft Docs
description: Voorbeelden
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 9dd7479ae95f74123d9b762e42ec95e8dbf25818
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346437"
---
# <a name="batch-transcription"></a>Batchtranscriptie

Batch transcriptie is ideaal voor toepassingen met grote hoeveelheden audio. Hiermee kan de ontwikkelaar verwijzen naar audio bestanden, waarna transcripties in asynchrone modus wordt uitgevoerd.

## <a name="batch-transcription-api"></a>Batch transcriptie API

De Batch-transcriptie API maakt het bovenstaande scenario mogelijk. Het biedt asynchrone spraak naar tekst transcriptie samen met extra functies.

> [!NOTE]
> De Batch-transcriptie API is ideaal voor callcenters die meestal worden verzameld duizenden uur aan audio. De brand & vergeten filosofie van de API kunt gemakkelijk grote hoeveelheid audio-opnamen transcriberen.

### <a name="supported-formats"></a>Ondersteunde indelingen

De Batch-transcriptie API is erop gericht de vermeldden voor alle offline call center-gerelateerde scenario's en bieden ondersteuning voor alle gerelateerde indelingen. Momenteel ondersteunde indelingen:

Naam| Kanaal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
wav |   Mono   |
wav |  Stereo  |

Voor stereo audiostreams, zal Batch transcriptie de linker- en -kanaal tijdens de transcriptie splitsen. De twee JSON-bestanden met het resultaat zijn alle gemaakt van één kanaal. De tijdstempels per utterance bieden de ontwikkelaar te maken van een geordende definitieve transcriptie. De volgende JSON-voorbeeld wordt de uitvoer van een kanaal.

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
> De Batch-transcriptie API maakt gebruik van een REST-service voor het aanvragen van transcripties, hun status en de bijbehorende resultaten. De API kan worden gebruikt vanuit een willekeurige taal. De volgende sectie wordt beschreven hoe deze wordt gebruikt.

## <a name="authorization-token"></a>Autorisatietoken

Als met alle functies van de spraakherkenning Unified Service, de gebruiker moet te maken van een abonnementssleutel van de [Azure-portal](https://portal.azure.com). Bovendien moet een API-sleutel worden verkregen via de Portal van spraak. De stappen voor het genereren van een API-sleutel:

1. Meld u aan bij https://customspeech.ai.

2. Klik op abonnementen.

3. Klik op de optie `Generate API Key`.

    ![De weergave uploaden](media/stt/Subscriptions.jpg)

4. Kopieer en plak deze sleutel in de clientcode in het onderstaande voorbeeld.

> [!NOTE]
> Als u van plan bent te gebruiken van een aangepast model vervolgens moet u de ID van dit model te. Houd er rekening mee dat dit niet is de implementatie of de eindpunt-ID die u op de detailweergave van het eindpunt, maar de model-ID die u ophalen vinden kunt wanneer u de Details van dit model op

## <a name="sample-code"></a>Voorbeeldcode

Maakt gebruik van de API is redelijk eenvoudig. De voorbeeldcode hieronder moet worden aangepast met een abonnementssleutel en een API-sleutel, die op zijn beurt kan de ontwikkelaar een bearer-token, als het volgende codefragment laat zien:

```cs
    public static async Task<CrisClient> CreateApiV1ClientAsync(string username, string key, string hostName, int port)
        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;

            var tokenProviderPath = "/oauth/ctoken";
            var clientToken = await CreateClientTokenAsync(client, hostName, port, tokenProviderPath, username, key).ConfigureAwait(false);
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", clientToken.AccessToken);

            return new CrisClient(client);
        }
```

Zodra het token is verkregen moet de ontwikkelaar van de door de SAS-Uri die verwijst naar het audiobestand transcriptie vereisen. De rest van de code gewoon doorloopt de status en worden resultaten weergegeven.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating a Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscription key>", // Subscription Key
                    new Uri("<SAS URI to your file>")).ConfigureAwait(false);

            var transcription = await client.GetTranscriptionAsync(newLocation).ConfigureAwait(false);

            while (true)
            {
                transcription = await client.GetTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                if (transcription.Status == "Failed" || transcription.Status == "Succeeded")
                {
                    Console.WriteLine("Transcription complete!");

                    if (transcription.Status == "Succeeded")
                    {
                        var resultsUri = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri, filename);

                        var results = File.ReadAllText(filename);
                        Console.WriteLine(results);
                    }

                    await client.DeleteTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                    break;
                }
                else
                {
                    Console.WriteLine("Transcription status: " + transcription.Status);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.ReadLine();
        }
```

> [!NOTE]
> De abonnementssleutel die worden vermeld in het bovenstaande codefragment is de sleutel van de Speech(Preview)-resource die u in Azure portal maakt. Sleutels die zijn verkregen van de resource Custom Speech Service werkt niet.


U ziet de asynchrone instellingen voor het plaatsen van audio en transcriptie status ontvangen. De client die gemaakt is een .NET-Http-client. Er is een `PostTranscriptions` methode voor het verzenden van de details van de audio-bestand en een `GetTranscriptions` methode om de resultaten te ontvangen. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` methode deze alias wordt gebruikt om te maken van een greep om op te halen van de status van de transcriptie.

De huidige voorbeeldcode geeft geen aangepaste modellen. De service gebruikt de basislijn-modellen voor de bestanden te transcriberen. Als de gebruiker wil om op te geven van de modellen, kunt een doorgeven op dezelfde manier de modelIDs voor de akoestische en het taalmodel. 

Als een niet wil basislijn gebruiken, moet een model-id's voor zowel akoestische en modellen doorgeven.

> [!NOTE]
> Voor basislijn heeft transcriptie de gebruiker geen om aan te geven van de eindpunten van de basislijn-modellen. Als de gebruiker wil gebruiken van aangepaste modellen hij zou hebben voor hun eindpunten-id als de [voorbeeld](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Als de gebruiker wil een akoestisch basislijn gebruiken met een basislijn taalmodel zou hij alleen moeten declareren van het aangepaste model endpoint-ID. Ons systeem wordt intern achterhalen van de partner basislijn-model (niet het akoestisch of taal) en gebruikt deze om te voldoen aan het verzoek transcriptie.

### <a name="supported-storage"></a>Ondersteunde opslag

De enige opslag wordt ondersteund is momenteel Azure-blob.

## <a name="downloading-the-sample"></a>Het voorbeeld downloaden

Het voorbeeld dat hier wordt weergegeven is op [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Een audiotranscriptie is doorgaans een tijdspanne gelijk zijn aan de duur van het audiobestand plus een 2-3 minuten overhead vereist.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
