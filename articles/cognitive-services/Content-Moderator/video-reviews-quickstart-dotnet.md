---
title: Azure Content beheerder - maken video beoordelingen met .NET | Microsoft Docs
description: Het maken van video bekijkt met Azure inhoud beheerder SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/18/2018
ms.author: sajagtap
ms.openlocfilehash: cb487314b8695f3676fdb22a9d7e3ec5ca3ed9f2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344572"
---
# <a name="create-video-reviews-using-net"></a>Maken van video beoordelingen met .NET

Dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met C# met behulp van de inhoud beheerder SDK:

- Maken van een video bekijken voor menselijke moderators
- Frames toevoegen aan een beoordeling
- De frames ophalen voor de controle 
- De status en details van de evaluatie van ophalen
- De controle publiceren

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [met controle van de video (Zie Quick Start)](video-moderation-api.md) en de antwoordgegevens hebt. U moet voor het maken op basis van het frame beoordelingen voor menselijke moderators.

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

### <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.

In het Dashboard inhoud beheerder vindt u de abonnementssleutel van uw in **instellingen** > **referenties** > **API**  >  **Proefversie Ocp-Apim-Subscription-Key**. Zie voor meer informatie [overzicht](overview.md).

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Voorbereiden van uw video's en video frames voor controleren

De video frames video en voorbeelden om te controleren moeten online zijn gepubliceerd omdat u de URL's nodig hebt.

> [!NOTE]
> Handmatig opgeslagen schermafbeeldingen van de video met willekeurige volwassene/mooie scores wordt gebruikt ter illustratie van het gebruik van de evaluatie-API. In een situatie met een echte, gebruikt u de [video toezicht uitvoer](video-moderation-api.md#run-the-program-and-review-the-output) installatiekopieën maken en toewijzen van scores. 

Voor de video moet u een streaming-eindpunt zodanig dat het hulpprogramma Bekijk de video in de weergave player speelt.

![Video demo miniatuur](images/ams-video-demo-view.PNG)

- Kopieer de **URL** op deze [Azure Media Services-demo](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) pagina voor de manifest-URL.

Gebruik de volgende afbeeldingen voor de video frames (installatiekopieën):

![Video frame miniatuur 1](images/ams-video-frame-thumbnails-1.PNG) | ![Video frame miniatuur 2](images/ams-video-frame-thumbnails-2.PNG) | ![Video frame miniatuur 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Frame 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Frame 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Frame 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Voeg een nieuwe **Console-app (.NET Framework)** project om uw oplossing.

1. Noem het project **VideoReviews**.

1. Selecteer dit project als opstartproject één voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

De volgende NuGet-pakketten voor het project TermLists installeren.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Update van het programma de using-instructies

Wijzig het programma de using-instructies als volgt.

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;
    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;


### <a name="add-private-properties"></a>Privé-eigenschappen toevoegen

De volgende persoonlijke eigenschappen toevoegen aan de naamruimte VideoReviews, klasse Program.

Indien aangegeven, vervang de voorbeeldwaarden voor deze eigenschappen.


    namespace VideoReviews
    {
        class Program
        {
            // NOTE: Replace this example location with the location for your Content Moderator account.
            /// <summary>
            /// The region/location for your Content Moderator account, 
            /// for example, westus.
            /// </summary>
            private static readonly string AzureRegion = "YOUR CONTENT MODERATOR REGION";

            // NOTE: Replace this example key with a valid subscription key.
            /// <summary>
            /// Your Content Moderator subscription key.
            /// </summary>
            private static readonly string CMSubscriptionKey = "YOUR CONTENT MODERATOR KEY";

            // NOTE: Replace this example team name with your Content Moderator team name.
            /// <summary>
            /// The name of the team to assign the job to.
            /// </summary>
            /// <remarks>This must be the team name you used to create your 
            /// Content Moderator account. You can retrieve your team name from
            /// the Conent Moderator web site. Your team name is the Id associated 
            /// with your subscription.</remarks>
            public static readonly string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

            /// <summary>
            /// The base URL fragment for Content Moderator calls.
            /// </summary>
            private static readonly string AzureBaseURL =
                $"{AzureRegion}.api.cognitive.microsoft.com";

            /// <summary>
            /// The minimum amount of time, in milliseconds, to wait between calls
            /// to the Content Moderator APIs.
            /// </summary>
            private const int throttleRate = 2000;


### <a name="create-content-moderator-client-object"></a>Inhoud beheerder Client-object maken

De volgende methodedefinitie toevoegen aan naamruimte VideoReviews, klasse Program.

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        return new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey))
        {
            BaseUrl = AzureBaseURL
        };
    }

## <a name="create-a-video-review"></a>Maken van een video-overzicht

Maken van een video bekijken met **ContentModeratorClient.Reviews.CreateVideoReviews**. Zie voor meer informatie de [API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet "application/json." 
1. De naam van uw inhoud beheerder-team.
1. Een **IList<CreateVideoReviewsBodyItem>**  object. Elke **CreateVideoReviewsBodyItem** object vertegenwoordigt een video revisie. Deze snelstartgids maakt een revisie tegelijk.

**CreateVideoReviewsBodyItem** heeft verschillende eigenschappen. Ten minste instellen de volgende eigenschappen:
- **Inhoud**. De URL van de video moet worden gecontroleerd.
- **ContentId**. Een ID toewijzen aan de video revisie.
- **Status**. Stel de waarde voor 'Unpublished'. Als u deze niet instelt, wordt standaard naar 'In behandeling', wat betekent dat de video revisie is gepubliceerd en in afwachting van menselijke controleren. Zodra een video revisie is gepubliceerd, kunt u niet langer video frames, een tekst of een resultaat van de tekst van toezicht aan toevoegen.

> [!NOTE]
> **CreateVideoReviews** retourneert een IList<string>. Elk van deze tekenreeksen bevat een ID voor een video revisie. Deze id's zijn GUID's en zijn niet hetzelfde zijn als de waarde van de **ContentId** eigenschap. 

De volgende methodedefinitie toevoegen aan naamruimte VideoReviews, klasse Program.

    /// <summary>
    /// Create a video review. For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="id">The ID to assign to the video review.</param>
    /// <param name="content">The URL of the video to review.</param>
    /// <returns>The ID of the video review.</returns>
    private static string CreateReview(ContentModeratorClient client, string id, string content)
    {
        Console.WriteLine("Creating a video review.");

        List<CreateVideoReviewsBodyItem> body = new List<CreateVideoReviewsBodyItem>() {
            new CreateVideoReviewsBodyItem
            {
                Content = content,
                ContentId = id,
                /* Note: to create a published review, set the Status to "Pending".
                However, you cannot add video frames or a transcript to a published review. */
                Status = "Unpublished",
            }
        };

        var result = client.Reviews.CreateVideoReviews("application/json", TeamName, body);

        Thread.Sleep(throttleRate);

        // We created only one review.
        return result[0];
    }

> [!NOTE]
> De sleutel van uw inhoud beheerder service heeft een aanvragen per frequentielimiet van tweede (RPS) en als u de limiet overschrijdt, de SDK er een uitzondering gegenereerd met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

## <a name="add-video-frames-to-the-video-review"></a>Video frames toevoegen aan de video controleren

U video frames toevoegen aan een video bekijken met **ContentModeratorClient.Reviews.AddVideoFrameUrl** (indien de video frames online worden gehost) of **ContentModeratorClient.Reviews.AddVideoFrameStream** () Als uw video frames zijn lokaal gehost). Deze snelstartgids wordt ervan uitgegaan dat uw video frames online worden gehost en gebruikt dus **AddVideoFrameUrl**. Zie voor meer informatie de [API-referentiemateriaal](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet "application/json."
1. De naam van uw inhoud beheerder-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een **IList<VideoFrameBodyItem>**  object. Elke **VideoFrameBodyItem** object vertegenwoordigt een video frame.

**VideoFrameBodyItem** heeft de volgende eigenschappen:
- **Tijdstempel**. Een tekenreeks die bevat, in seconden, de tijd in de video waarvan de video frame is genomen.
- **FrameImage**. De URL van de video frame.
- **Metagegevens**. Een IList<VideoFrameBodyItemMetadataItem>. **VideoFrameBodyItemMetadataItem** is gewoon een sleutel-waardepaar. Geldige sleutels zijn onder andere:
- **reviewRecommended**. True als een menselijke revisie van het kader van de video wordt aanbevolen.
- **adultScore**. Een waarde tussen 0 en 1 die de ernst van de inhoud voor volwassenen in de video-frame classificeert.
- **een**. True als de video inhoud voor volwassenen bevat.
- **racyScore**. Een waarde tussen 0 en 1 die de ernst van mooie inhoud in de video-frame classificeert.
- **r**. True als de video frame mooie inhoud bevat.
- **ReviewerResultTags**. Een IList<VideoFrameBodyItemReviewerResultTagsItem>. **VideoFrameBodyItemReviewerResultTagsItem** is gewoon een sleutel-waardepaar. Een toepassing kan deze tags gebruiken om te organiseren video frames.

> [!NOTE]
> Deze snelstartgids genereert willekeurige waarden voor de **adultScore** en **racyScore** eigenschappen. In een productietoepassing, zou u deze waarden uit de [video toezicht service](video-moderation-api.md), geïmplementeerd als een Azure Media Service.

De volgende methodedefinities toevoegen aan naamruimte VideoReviews, klasse Program.

    <summary>
    /// Create a video frame to add to a video review after the video review is created.
    /// </summary>
    /// <param name="url">The URL of the video frame image.</param>
    /// <returns>The video frame.</returns>
    private static VideoFrameBodyItem CreateFrameToAddToReview(string url, string timestamp_seconds)
    {
        // We generate random "adult" and "racy" scores for the video frame.
        Random rand = new Random();

        var frame = new VideoFrameBodyItem
        {
            // The timestamp is measured in milliseconds. Convert from seconds.
            Timestamp = (int.Parse(timestamp_seconds) * 1000).ToString(),
            FrameImage = url,

            Metadata = new List<VideoFrameBodyItemMetadataItem>
            {
                new VideoFrameBodyItemMetadataItem("reviewRecommended", "true"),
                new VideoFrameBodyItemMetadataItem("adultScore", rand.NextDouble().ToString()),
                new VideoFrameBodyItemMetadataItem("a", "false"),
                new VideoFrameBodyItemMetadataItem("racyScore", rand.NextDouble().ToString()),
                new VideoFrameBodyItemMetadataItem("r", "false")
            },

            ReviewerResultTags = new List<VideoFrameBodyItemReviewerResultTagsItem>()
            {
                new VideoFrameBodyItemReviewerResultTagsItem("tag1", "value1")
            }
        };

        return frame;
    }

    /// <summary>
    /// Add a video frame to the indicated video review. For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="url">The URL of the video frame image.</param>
    static void AddFrame(ContentModeratorClient client, string review_id, string url, string timestamp_seconds)
    {
        Console.WriteLine("Adding a frame to the review with ID {0}.", review_id);

        var frames = new List<VideoFrameBodyItem>()
        {
            CreateFrameToAddToReview(url, timestamp_seconds)
        };
            
        client.Reviews.AddVideoFrameUrl("application/json", TeamName, review_id, frames);

        Thread.Sleep(throttleRate);
    

## <a name="get-video-frames-for-video-review"></a>Video frames ophalen voor video controleren

U kunt de video frames ophalen voor een video bekijken met **ContentModeratorClient.Reviews.GetVideoFrames**. **GetVideoFrames** heeft de volgende vereiste parameters:
1. De naam van uw inhoud beheerder-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. De op nul gebaseerde index van het eerste video frame om op te halen.
1. Het aantal video frames om op te halen.

De volgende methodedefinitie toevoegen aan naamruimte VideoReviews, klasse Program.

    /// <summary>
    /// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    static void GetFrames(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

        Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0, Int32.MaxValue);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="get-video-review-information"></a>Ophalen van gegevens video bekijken

Ophalen van informatie voor een video bekijken met **ContentModeratorClient.Reviews.GetReview**. **GetReview** heeft de volgende vereiste parameters:
1. De naam van uw inhoud beheerder-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.

De volgende methodedefinitie toevoegen aan naamruimte VideoReviews, klasse Program.

    /// <summary>
    /// Get the information for the indicated video review. For more information, see the reference API:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    private static void GetReview(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting the status for the review with ID {0}.", review_id);

        var result = client.Reviews.GetReview(ModeratorHelper.Clients.TeamName, review_id);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="publish-video-review"></a>Publiceren van video controleren

Publiceren van een video bekijken met **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** heeft de volgende vereiste parameters:
1. De naam van uw inhoud beheerder-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.

De volgende methodedefinitie toevoegen aan naamruimte VideoReviews, klasse Program.

    /// <summary>
    /// Publish the indicated video review. For more information, see the reference API:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7bb29e7151f0b10d45201
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    private static void PublishReview(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Publishing the review with ID {0}.", review_id);
        client.Reviews.PublishVideoReview(TeamName, review_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Kort samengevat

Voeg de **Main** methodedefinitie naamruimte VideoReviews, klasse Program. Tot slot sluit de klasse Program en de naamruimte VideoReviews.

    static void Main(string[] args)
    {
        using (ContentModeratorClient client = NewClient())
        {
            // Create a review with the content pointing to a streaming endpoint (manifest)
            var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
            string review_id = CreateReview(client, "review1", streamingcontent);

            var frame1_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG";
            var frame2_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG";
            var frame3_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG";

            // Add the frames from 17, 64, and 144 seconds.
            AddFrame(client, review_id, frame1_url, "17");
            AddFrame(client, review_id, frame2_url, "64");
            AddFrame(client, review_id, frame3_url, "144");

            // Get frames information and show
            GetFrames(client, review_id);
            GetReview(client, review_id);

            // Publish the review
            PublishReview(client, review_id);

            Console.WriteLine("Open your Content Moderator Dashboard and select Review > Video to see the review.");
            Console.WriteLine("Press any key to close the application.");
            Console.Read();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Voer het programma en controleer de uitvoer
Wanneer u de toepassing uitvoert, ziet u uitvoer op de volgende regels:

    Creating a video review.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Getting frames for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    {
        "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
        "VideoFrames": [
        {
            "Timestamp": "17000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_17000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.808312381528463"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.846378884206702"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
        ]
        },
        {
            "Timestamp": "64000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_64000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.576078300166912"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.244768953064815"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
        ]
        },
        {
            "Timestamp": "144000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_144000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.664480847150311"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.933817870418456"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
            ]
        }
        ]
    }
    
    Getting the status for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    {
        "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
        "SubTeam": "public",
        "Status": "UnPublished",
        "ReviewerResultTags": [],
        "CreatedBy": "testreview6",
        "Metadata": [
        {
            "Key": "FrameCount",
            "Value": "3"
        }
        ],
        "Type": "Video",
        "Content": "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
        "ContentId": "review1",
        "CallbackEndpoint": null
    }

    Publishing the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="check-out-your-video-review"></a>Bekijk de video revisie

Ten slotte ziet u de video herziening in uw inhoud beheerder hulpprogramma account controleren op de **bekijken**>**Video** scherm.

![Video revisie voor menselijke moderators](images/ams-video-review.PNG)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het toevoegen van [de tekst van toezicht](video-transcript-moderation-review-tutorial-dotnet.md) voor de video revisie. 

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [voltooien video toezicht oplossing](video-transcript-moderation-review-tutorial-dotnet.md).

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET.
