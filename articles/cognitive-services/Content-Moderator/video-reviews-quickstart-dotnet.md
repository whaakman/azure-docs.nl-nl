---
title: Azure Content Moderator - video beoordelingen met behulp van .NET maken | Microsoft Docs
description: Over het maken van video controleert met Azure Content Moderator-SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/18/2018
ms.author: sajagtap
ms.openlocfilehash: f0e0867728e8a7e9ab4f54d6a0ec68038260997d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182758"
---
# <a name="create-video-reviews-using-net"></a>Maak video beoordelingen met behulp van .NET

In dit artikel vindt u informatie en voorbeelden van code om u te helpen snel aan de slag met de [Content Moderator-SDK met C#](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) aan:

- Maken van een video bekijken voor menselijke moderators
- Frames toevoegen aan een beoordeling
- De frames ophalen voor de beoordeling 
- De status en details van de beoordeling
- Publiceren van de beoordeling

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [onder toezicht van de video (Zie de snelstart)](video-moderation-api.md) en beschikt over de antwoordgegevens. U hebt deze nodig voor het maken van beoordelingen op basis van een kader voor menselijke moderators.

In dit artikel wordt ook van uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator"></a>Aanmelden voor Content Moderator

Voordat u de Content Moderator-services via de REST-API of de SDK gebruiken kunt, moet u de abonnementssleutel van een.
Raadpleeg de [snelstartgids](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Zich registreren voor een hulpprogramma voor beoordeling account als niet in de vorige stap is voltooid

Als u uw Content Moderator vanuit de Azure-portal ook hebt [zich registreren voor de beoordeling hulpprogramma account](https://contentmoderator.cognitive.microsoft.com/) en maken van een beoordelingsteam. U moet het team-Id en het beoordelingsprogramma de API controleren als u wilt een taak starten en de evaluaties weergeven in het controlehulpprogramma aan te roepen.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Zorg ervoor dat uw API-sleutel de beoordeling-API kunt aanroepen voor het maken van de beoordeling

Na het voltooien van de vorige stappen, zou u uiteindelijk met twee Content Moderator-sleutels als u vanuit de Azure-portal gestart. 

Als u van plan bent de Azure-opgegeven API-sleutel in de SDK-voorbeeld gebruiken, volgt u de stappen in de [met behulp van Azure-sleutel met de API controleren](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) sectie waarmee uw toepassing in de beoordeling-API aanroepen en beoordelingen te maken.

Als u de gratis proefversie sleutel gegenereerd door het beoordelingsprogramma, uw beoordeling hulpprogramma-account al op de hoogte van de sleutel en daarom geen extra stappen zijn vereist.

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Uw video en de videoframes voorbereiden voor controle

De video en voorbeeldgegevens videoframes om te controleren moeten online zijn gepubliceerd omdat u de URL's nodig hebt.

> [!NOTE]
> Het programma gebruikt handmatig opgeslagen schermafbeeldingen van de video met willekeurige volwassene gebruik scores ter illustratie van het gebruik van de API controleren. In een situatie praktijk, gebruikt u de [beheer van video-uitvoer](video-moderation-api.md#run-the-program-and-review-the-output) installatiekopieën maken en toewijzen van scores. 

Voor de video moet u een streaming-eindpunt, zodat het beoordelingsprogramma wordt de video afgespeeld in de weergave van de speler.

![Videodemo miniatuur](images/ams-video-demo-view.PNG)

- Kopieer de **URL** op deze [demo van Azure Media Services](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) -pagina voor de manifest-URL.

Gebruik de volgende afbeeldingen voor de videoframes (installatiekopieën):

![Miniatuur van video frame 1](images/ams-video-frame-thumbnails-1.PNG) | ![Miniatuur van video kader 2](images/ams-video-frame-thumbnails-2.PNG) | ![Miniatuur van video kader 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Frame 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Kader 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Kader 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Toevoegen van een nieuwe **Console-app (.NET Framework)** project aan uw oplossing.

1. Noem het project **VideoReviews**.

1. Selecteer dit project als opstartproject één voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeer de volgende NuGet-pakketten voor het project TermLists.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Update het programma de using-instructies

Wijzig het programma de using-instructies toe als volgt.

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

Indien vermeld, vervang de voorbeeldwaarden voor deze eigenschappen.


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
            /// the Content Moderator web site. Your team name is the Id associated 
            /// with your subscription.</remarks>
            private const string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

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


### <a name="create-content-moderator-client-object"></a>Content Moderator-clientobject maken

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

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
            Endpoint = AzureBaseURL
        };
    }

## <a name="create-a-video-review"></a>Maken van een video beoordeling

Maken van een video bekijken met **ContentModeratorClient.Reviews.CreateVideoReviews**. Zie voor meer informatie de [API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet ' application/json'. 
1. De naam van uw Content Moderator-team.
1. Een **IList<CreateVideoReviewsBodyItem>**  object. Elke **CreateVideoReviewsBodyItem** object vertegenwoordigt een video beoordeling. In deze Quick Start maakt een beoordeling op een tijdstip.

**CreateVideoReviewsBodyItem** heeft een aantal eigenschappen. Ten minste, moet u de volgende eigenschappen instellen:
- **Inhoud**. De URL van de video moet worden gecontroleerd.
- **ContentId**. Een ID om toe te wijzen aan de video beoordeling.
- **Status**. Stel de waarde "Unpublished." Als u deze niet instelt, wordt de standaardwaarde op 'In behandeling', wat betekent dat de video beoordeling is gepubliceerd en in afwachting van menselijke beoordeling. Zodra een beoordeling van de video is gepubliceerd, kunt u niet langer videoframes, een transcript of een transcript toezicht resultaat aan toevoegen.

> [!NOTE]
> **CreateVideoReviews** retourneert een IList<string>. Deze tekenreeksen bevat een ID voor een video beoordeling. Deze id's zijn GUID's en zijn niet hetzelfde als de waarde van de **ContentId** eigenschap. 

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

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
> De sleutel van uw Content Moderator-service heeft een aantal aanvragen per limiet voor tweede (RPS) en als u de limiet overschrijdt, de SDK een uitzondering met een foutcode 429 genereert. 
>
> De sleutel van een gratis laag heeft een limiet van één RPS.

## <a name="add-video-frames-to-the-video-review"></a>Videoframes voor de video revisie toevoegen

U videoframes toevoegen aan een video bekijken met **ContentModeratorClient.Reviews.AddVideoFrameUrl** (als uw videoframes online worden gehost) of **ContentModeratorClient.Reviews.AddVideoFrameStream** () Als uw videoframes worden gehost lokaal). In deze snelstartgids wordt ervan uitgegaan dat uw videoframes online worden gehost en gebruikt dus **AddVideoFrameUrl**. Zie voor meer informatie de [API-verwijzing](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet ' application/json'.
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een **IList<VideoFrameBodyItem>**  object. Elke **VideoFrameBodyItem** object vertegenwoordigt een video frame.

**VideoFrameBodyItem** heeft de volgende eigenschappen:
- **Tijdstempel**. Een tekenreeks is die bevat, in seconden, de tijd in de video van waaruit u de video-frame gehaald.
- **FrameImage**. De URL van de video-frame.
- **Metagegevens**. IList<VideoFrameBodyItemMetadataItem>. **VideoFrameBodyItemMetadataItem** is gewoon een sleutel/waarde-paar. Geldige sleutels zijn onder andere:
- **reviewRecommended**. Waar, als een menselijke beoordeling van het kader van de video wordt aanbevolen.
- **adultScore**. Een waarde tussen 0 en 1 die tarieven van de ernst van inhoud voor volwassenen in het kader van de video.
- **een**. True als de video inhoud voor volwassenen bevat.
- **racyScore**. Een waarde tussen 0 en 1 die tarieven van de ernst van ongepaste inhoud in de video-frame.
- **r**. Waar als de video-frame ongepaste inhoud bevat.
- **ReviewerResultTags**. IList<VideoFrameBodyItemReviewerResultTagsItem>. **VideoFrameBodyItemReviewerResultTagsItem** is gewoon een sleutel/waarde-paar. Een toepassing kan deze tags gebruiken om te organiseren videoframes.

> [!NOTE]
> In deze snelstartgids genereert willekeurige waarden voor de **adultScore** en **racyScore** eigenschappen. In een productietoepassing, zou u deze waarden uit de [service voor videotoezicht](video-moderation-api.md)geïmplementeerde als een Azure Media Service.

De volgende methodedefinities aan de naamruimte VideoReviews, klasse programma toevoegen.

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
    

## <a name="get-video-frames-for-video-review"></a>Videoframes ophalen voor video controleren

U kunt de videoframes ophalen voor een video bekijken met **ContentModeratorClient.Reviews.GetVideoFrames**. **GetVideoFrames** heeft de volgende vereiste parameters:
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. De op nul gebaseerde index van het eerste video kader om op te halen.
1. Het aantal videoframes om op te halen.

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

    /// <summary>
    /// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    static void GetFrames(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

        Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="get-video-review-information"></a>Ophalen van informatie over video weergeven

Ophalen van gegevens voor een video bekijken met **ContentModeratorClient.Reviews.GetReview**. **GetReview** heeft de volgende vereiste parameters:
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

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

## <a name="publish-video-review"></a>Bekijk video publiceren

Publiceren van een video bekijken met **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** heeft de volgende vereiste parameters:
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

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

## <a name="putting-it-all-together"></a>Dit alles

Voeg de **Main** methodedefinitie naamruimte VideoReviews, klasse programma. Tot slot sluit de klasse Program en de VideoReviews-naamruimte.

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
            Console.ReadKey();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Voer het programma uit en controleer de uitvoer
Als u de toepassing uitvoert, ziet u uitvoer op de volgende regels:

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

Ten slotte ziet u de video controle in uw Content Moderator hulpprogramma account bekijken op de **bekijken**>**Video** scherm.

![Video beoordeling van menselijke moderators](images/ams-video-review.PNG)

## <a name="next-steps"></a>Volgende stappen

Krijgen de [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en de [Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET.

Informatie over het toevoegen [transcript toezicht](video-transcript-moderation-review-tutorial-dotnet.md) naar de video-overzicht. 

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [videotoezicht oplossing voltooien](video-transcript-moderation-review-tutorial-dotnet.md).
