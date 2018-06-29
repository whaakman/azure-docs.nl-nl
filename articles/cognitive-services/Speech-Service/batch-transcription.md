---
title: Azure Batch schrijffouten API | Azure Microsoft Docs
description: Voorbeelden
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: cf58f676be52aa16ce6de59c3566613c7ee9276d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084079"
---
# <a name="batch-transcription"></a>Batch schrijffouten

Batch schrijffouten is ideaal voor gebruiksvoorbeelden met grote hoeveelheden audio. Kan de ontwikkelaar wijst audio-bestanden en transcriptie teruggaan in asynchrone modus wordt uitgevoerd.

## <a name="batch-transcription-api"></a>Batch schrijffouten API

De Batch-schrijffouten API maakt het bovenstaande scenario mogelijk. Asynchrone spraak naar tekst schrijffouten samen met aanvullende functies biedt.

> [!NOTE]
> De Batch-schrijffouten API is ideaal voor aanroep van datacenters die doorgaans uren audio verzamelt. De API, Fire & vergeten achter gemakkelijk grote hoeveelheid geluidsopnamen transcriberen.

### <a name="supported-formats"></a>Ondersteunde indelingen

De Batch-schrijffouten API is erop gericht de de-facto voor alle offline aanroep center-gerelateerde scenario's en bieden ondersteuning voor alle gerelateerde indelingen. Momenteel ondersteunde indelingen:

Naam| Kanaal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
wav |   Mono   |
wav |  Stereo  |

Voor de aansluiting audio gegevensstromen splitst Batch schrijffouten het kanaal linker- en tijdens de schrijffouten. De twee JSON-bestanden met het resultaat zijn elk gemaakt op basis van één kanaal. De tijdstempels per utterance inschakelen de ontwikkelaar de tekst van een geordende laatste maken. Het volgende JSON-voorbeeld ziet u de uitvoer van een kanaal.

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
> De Batch-schrijffouten API maakt gebruik van een REST-service voor het aanvragen van transcriptie, hun status en de bijbehorende resultaten. Dit is gebaseerd op .NET en heeft geen externe afhankelijkheden. De volgende sectie wordt beschreven hoe deze wordt gebruikt.

## <a name="authorization-token"></a>Verificatietoken

Als met alle functies van de Service Unified spraak, de gebruiker moet voor het maken van een abonnementssleutel van de [Azure-portal](https://portal.azure.com). Bovendien moet een API-sleutel moet worden verkregen via de Portal spraak. De stappen voor het genereren van een API-sleutel:

1. Meld u aan bij https://customspeech.ai.

2. Klik op abonnementen.

3. Klik op de optie `Generate API Key`.

    ![De weergave uploaden](media/stt/Subscriptions.jpg)

4. Kopieer en plak deze sleutel in de clientcode in het onderstaande voorbeeld.

> [!NOTE]
> Als u wilt gebruiken een aangepaste model wordt moet u de ID van het model te. Houd er rekening mee dat dit niet is de implementatie of de Endpoint-ID die zich in de detailweergave van het eindpunt, maar de model-ID die u ophalen kunt wanneer u de Details van dit model op

## <a name="sample-code"></a>Voorbeeldcode

Maakt gebruik van de API is redelijk meteen doorsturen. De voorbeeldcode hieronder moet worden aangepast met een abonnementssleutel en een API-sleutel, wat op zijn beurt kan de ontwikkelaar een bearer-token verkrijgen als de volgende code codefragment bevat:

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

Zodra het token is verkregen moet de ontwikkelaar van de door de SAS-Uri die verwijst naar het audiobestand schrijffouten vereisen. De rest van de code gewoon doorloopt de status en resultaten worden weergegeven.

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
> De sleutel van het abonnement vermeld in het bovenstaande codefragment is de sleutel van de Speech(Preview) resource die u op Azure-portal maakt. Sleutels die worden opgehaald uit de bron van de aangepaste spraak Service werkt niet.


U ziet de asynchrone instellingen voor het publiceren van audio en schrijffouten status ontvangen. De client gemaakt is een NET Http-client. Er is een `PostTranscriptions` methode voor het verzenden van de details audiobestand en een `GetTranscriptions` methode om de resultaten te ontvangen. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` methode is met behulp van deze koppeling te maken van een greep voor het verkrijgen van de status schrijffouten.

De huidige voorbeeldcode geeft geen aangepaste modellen. De service gebruikt de basislijn-modellen voor ze de bestanden. Als de gebruiker wil de modellen opgeven, een kan worden doorgegeven op dezelfde manier de modelIDs voor de acoustic en het taalmodel. 

Als een niet wil basislijn gebruiken, moet een model-id's voor acoustic- en taalinstellingen modellen doorgeven.

> [!NOTE]
> Voor de basislijn schrijffouten de gebruiker geen declareren van de eindpunten van de basislijn-modellen. Als de gebruiker wil gebruiken van aangepaste modellen hij zou moeten bieden hun eindpunten-id als de [voorbeeld](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Als de gebruiker wil een akoestisch baseline gebruiken met een model van de taal basislijn zou vervolgens hij alleen moeten declareren van het aangepaste model endpoint-ID. Intern ons systeem nagaan wat het partner-basislijn-model (het akoestisch of taal zijn) en gebruiken om te voldoen aan het verzoek schrijffouten.

### <a name="supported-storage"></a>Ondersteunde opslag

De enige opslag ondersteund is momenteel Azure-blob.

## <a name="downloading-the-sample"></a>Het voorbeeld downloaden

Het voorbeeld dat hier weergegeven is op [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Een audio schrijffouten is doorgaans een tijdsspanne gelijk zijn aan de duur van het audiobestand plus een minuut 2-3-overhead.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
