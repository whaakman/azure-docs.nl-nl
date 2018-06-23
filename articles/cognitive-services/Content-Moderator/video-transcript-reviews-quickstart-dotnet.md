---
title: Azure Content beheerder - maken van de tekst van de video-beoordelingen met .NET | Microsoft Docs
description: Het maken van de tekst van de video bekijkt met Azure inhoud beheerder SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/19/2018
ms.author: sajagtap
ms.openlocfilehash: 3286da6e38f0fba02386d877a835fb694ed0fdec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344763"
---
# <a name="create-video-transcript-reviews-using-net"></a>Maken van de tekst van de video-beoordelingen met .NET

Dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met C# met behulp van de inhoud beheerder SDK:

- Maken van een video bekijken voor menselijke moderators
- De tekst van een gecontroleerde toevoegen aan de revisie
- De controle publiceren

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [met controle van de video](video-moderation-api.md) en [gemaakt van de video revisie](video-reviews-quickstart-dotnet.md) in het hulpprogramma voor beoordeling voor menselijke besluitvorming. Nu u met toezicht video transcripties toevoegen in het hulpprogramma voor beoordeling.

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

### <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.

In het Dashboard inhoud beheerder treft u de abonnementssleutel van uw in **instellingen** > **referenties** > **API**  >   **Evaluatieversie Ocp-Apim-Subscription-Key**. Zie voor meer informatie [overzicht](overview.md).

### <a name="prepare-your-video-for-review"></a>Voorbereiden van uw video voor controleren

De tekst van de toevoegen aan een video revisie. De video moet online zijn gepubliceerd. U moet de streaming-eindpunt. Het streaming-eindpunt kan de revisie hulpprogramma video speler afspelen van de video.

![Video demo miniatuur](images/ams-video-demo-view.PNG)

- Kopieer de **URL** op deze [Azure Media Services-demo](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) pagina voor de manifest-URL.

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Voeg een nieuwe **Console-app (.NET Framework)** project om uw oplossing.

1. Noem het project **VideoTranscriptReviews**.

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

De volgende persoonlijke eigenschappen toevoegen aan de naamruimte VideoTranscriptReviews, klasse Program.

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

De volgende methodedefinitie toevoegen aan naamruimte VideoTranscriptReviews, klasse Program.

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
> De sleutel van uw inhoud beheerder-service heeft een aantal aanvragen per frequentielimiet van tweede (RPS). Als u de limiet overschrijdt, genereert de SDK een uitzondering met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

## <a name="add-transcript-to-video-review"></a>De tekst toevoegen aan video controleren

U een van de tekst toevoegen aan een video bekijken met **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** heeft de volgende vereiste parameters:
1. Uw team inhoud beheerder-id.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een **stroom** -object dat de tekst van de bevat.

De tekst van de moet de indeling WebVTT. Zie voor meer informatie [WebVTT: The Web Video houdt tekstindeling](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> De tekst van een steekproef wordt gebruikt in de notatie VTT. In een echte-oplossing, gebruikt u de service Azure Media Indexer [genereren van de tekst van een](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video.

De volgende methodedefinitie toevoegen aan naamruimte VideotranscriptReviews, klasse Program.

    /// <summary>
    /// Add a transcript to the indicated video review.
    /// The transcript must be in the WebVTT format.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b8b2e7151f0b10d451fe
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscript(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript to the review with ID {0}.", review_id);
        client.Reviews.AddVideoTranscript(TeamName, review_id, new MemoryStream(System.Text.Encoding.UTF8.GetBytes(transcript)));
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Een resultaat van de tekst van toezicht toevoegen aan video controleren

Naast de tekst van een aan een video-overzicht toevoegt, moet u ook het resultaat besproken die de tekst toevoegen. U doet met **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Zie voor meer informatie de [API-referentiemateriaal](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet "application/json." 
1. De naam van uw inhoud beheerder-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een IList<TranscriptModerationBodyItem>. Een **TranscriptModerationBodyItem** heeft de volgende eigenschappen:
- **Voorwaarden**. Een IList<TranscriptModerationBodyItemTermsItem>. Een **TranscriptModerationBodyItemTermsItem** heeft de volgende eigenschappen:
- **Index**. De op nul gebaseerde index van de termijn.
- **Term**. Een tekenreeks met de term.
- **Tijdstempel**. Een tekenreeks die bevat, in seconden, de tijd in de tekst van de waarop de voorwaarden worden gevonden.

De tekst van de moet de indeling WebVTT. Zie voor meer informatie [WebVTT: The Web Video houdt tekstindeling](https://www.w3.org/TR/webvtt1/).

De volgende methodedefinitie toevoegen aan naamruimte VideoTranscriptReviews, klasse Program. Deze methode verzendt een tekst naar de **ContentModeratorClient.TextModeration.ScreenText** methode. Ook wordt het resultaat omgezet in een IList<TranscriptModerationBodyItem>, en verstuurt naar **AddVideoTranscriptModerationResult**.

    /// <summary>
    /// Add the results of moderating a video transcript to the indicated video review.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscriptModerationResult(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript moderation result to the review with ID {0}.", review_id);

        // Screen the transcript using the Text Moderation API. For more information, see:
        // https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f
        Screen screen = client.TextModeration.ScreenText("eng", "text/plain", transcript);

        // Map the term list returned by ScreenText into a term list we can pass to AddVideoTranscriptModerationResult.
        List<TranscriptModerationBodyItemTermsItem> terms = new List<TranscriptModerationBodyItemTermsItem>();
        if (null != screen.Terms)
        {
            foreach (var term in screen.Terms)
            {
                if (term.Index.HasValue)
                {
                    terms.Add(new TranscriptModerationBodyItemTermsItem(term.Index.Value, term.Term));
                }
            }
        }

        List<TranscriptModerationBodyItem> body = new List<TranscriptModerationBodyItem>()
        {
            new TranscriptModerationBodyItem()
            {
                Timestamp = "0",
                Terms = terms
            }
        };

        client.Reviews.AddVideoTranscriptModerationResult("application/json", TeamName, review_id, body);

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

Voeg de **Main** methodedefinitie naamruimte VideoTranscriptReviews, klasse Program. Tot slot sluit de klasse Program en de naamruimte VideoTranscriptReviews.

> [!NOTE]
> De tekst van een steekproef wordt gebruikt in de notatie VTT. In een echte-oplossing, gebruikt u de service Azure Media Indexer [genereren van de tekst van een](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video. 

    static void Main(string[] args)
    {
        using (ContentModeratorClient client = NewClient())
        {
            // Create a review with the content pointing to a streaming endpoint (manifest)
            var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
            string review_id = CreateReview(client, "review1", streamingcontent);

            var transcript = @"WEBVTT

            01:01.000 --> 02:02.000
            First line with a crap word in a transcript.

            02:03.000 --> 02:25.000
            This is another line in the transcript.
            ";

            AddTranscript(client, review_id, transcript);

            AddTranscriptModerationResult(client, review_id, transcript);

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
    Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="navigate-to-your-video-transcript-review"></a>Navigeer naar de tekst van de video gecontroleerd

Ga naar de revisie van de tekst van de video in uw inhoud beheerder controle-hulpprogramma op de **bekijken**>**Video**>**verslag** scherm.

Ziet u de volgende functies:
- De twee regels van de tekst die u hebt toegevoegd
- De term taalgebruik gevonden en gemarkeerd met de tekst toezicht-service
- De video begint selecteren van een tekst schrijffouten bij de tijdstempel

![De tekst van de video revisie voor menselijke moderators](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) in het hulpprogramma voor beoordeling.

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [voltooien video toezicht oplossing](video-transcript-moderation-review-tutorial-dotnet.md).

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET.
