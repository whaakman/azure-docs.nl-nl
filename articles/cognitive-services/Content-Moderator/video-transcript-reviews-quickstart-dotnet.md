---
title: Videotranscriptie beoordelingen met behulp van .NET - Content Moderator maken
titlesuffix: Azure Cognitive Services
description: Maken van videotranscriptie beoordelingen met de Content Moderator-SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: sajagtap
ms.openlocfilehash: 4e862a8b74339bc8dd1de6c0b231ddb15425974c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220938"
---
# <a name="create-video-transcript-reviews-using-net"></a>Maken van videotranscriptie beoordelingen met behulp van .NET

In dit artikel vindt u informatie en voorbeelden van code om u te helpen snel aan de slag met de [Content Moderator-SDK met C#](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) aan:

- Maken van een video bekijken voor menselijke moderators
- Een gecontroleerde transcript toevoegen aan de beoordeling
- Publiceren van de beoordeling

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [onder toezicht van de video](video-moderation-api.md) en [gemaakt van de video beoordeling](video-reviews-quickstart-dotnet.md) in het controlehulpprogramma menselijke beslissingen te nemen. Nu u met toezicht transcripts van video in het beoordelingsprogramma toevoegen.

In dit artikel wordt ook van uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator"></a>Aanmelden voor Content Moderator

Voordat u de Content Moderator-services via de REST-API of de SDK gebruiken kunt, moet u de abonnementssleutel van een.
Raadpleeg de [snelstartgids](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Zich registreren voor een hulpprogramma voor beoordeling account als niet in de vorige stap is voltooid

Als u uw Content Moderator vanuit de Azure-portal ook hebt [zich registreren voor de beoordeling hulpprogramma account](https://contentmoderator.cognitive.microsoft.com/) en maken van een beoordelingsteam. U moet het team-Id en het beoordelingsprogramma de API controleren als u wilt een taak starten en de evaluaties weergeven in het controlehulpprogramma aan te roepen.

## <a name="ensure-your-api-key-can-call-the-review-api-job-creation"></a>Zorg ervoor dat uw API-sleutel (project maken) van de beoordeling-API kunt aanroepen

Na het voltooien van de vorige stappen, zou u uiteindelijk met twee Content Moderator-sleutels als u vanuit de Azure-portal gestart. 

Als u van plan bent de Azure-opgegeven API-sleutel in de SDK-voorbeeld gebruiken, volgt u de stappen in de [met behulp van Azure-sleutel met de API controleren](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) sectie waarmee uw toepassing in de beoordeling-API aanroepen en beoordelingen te maken.

Als u de gratis proefversie sleutel gegenereerd door het beoordelingsprogramma, uw beoordeling hulpprogramma-account al op de hoogte van de sleutel en daarom geen extra stappen zijn vereist.

## <a name="prepare-your-video-for-review"></a>Uw video voorbereiden voor controle

Het transcript toevoegen aan een video beoordeling. De video moet online zijn gepubliceerd. U moet het streaming-eindpunt. Het streaming-eindpunt kunt de video speler van revisie hulpprogramma de video afspelen.

![Videodemo miniatuur](images/ams-video-demo-view.PNG)

- Kopieer de **URL** op deze [demo van Azure Media Services](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) -pagina voor de manifest-URL.

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Toevoegen van een nieuwe **Console-app (.NET Framework)** project aan uw oplossing.

1. Noem het project **VideoTranscriptReviews**.

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

De volgende persoonlijke eigenschappen toevoegen aan de naamruimte VideoTranscriptReviews, klasse Program.

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

De methodedefinitie van de volgende aan naamruimte VideoTranscriptReviews, klasse programma toevoegen.

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
> De sleutel van uw Content Moderator-service heeft een aantal aanvragen per limiet voor tweede (RPS). Als u de limiet overschrijdt, wordt in de SDK een uitzondering met een foutcode 429 genereert. 
>
> De sleutel van een gratis laag heeft een limiet van één RPS.

## <a name="add-transcript-to-video-review"></a>Transcript aan video revisie toevoegen

U een transcript toevoegen aan een video bekijken met **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** heeft de volgende vereiste parameters:
1. Uw Content Moderator-team-id.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een **Stream** -object dat het transcript bevat.

Het transcript moet zich in de WebVTT-indeling. Zie voor meer informatie, [WebVTT: The Web Video nummers tekstindeling](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> Het programma gebruikt een transcript voorbeeld in de VTT-indeling. In een echte oplossing, gebruikt u de service Azure Media Indexer [genereren een transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video.

De methodedefinitie van de volgende aan naamruimte VideotranscriptReviews, klasse programma toevoegen.

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

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Toevoegen van een transcript toezicht resultaat naar video-overzicht

Naast het toevoegen van een transcript naar een video-overzicht, moet u ook het resultaat van toezicht houden op dit transcript toevoegen. U doet met **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Zie voor meer informatie de [API-verwijzing](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet ' application/json'. 
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. IList<TranscriptModerationBodyItem>. Een **TranscriptModerationBodyItem** heeft de volgende eigenschappen:
- **Voorwaarden**. IList<TranscriptModerationBodyItemTermsItem>. Een **TranscriptModerationBodyItemTermsItem** heeft de volgende eigenschappen:
- **Index**. De op nul gebaseerde index van de termijn.
- **Term**. Een tekenreeks is die de term bevat.
- **Tijdstempel**. Een tekenreeks is die bevat, in seconden, de tijd in het transcript waarop de voorwaarden worden gevonden.

Het transcript moet zich in de WebVTT-indeling. Zie voor meer informatie, [WebVTT: The Web Video nummers tekstindeling](https://www.w3.org/TR/webvtt1/).

De methodedefinitie van de volgende aan naamruimte VideoTranscriptReviews, klasse programma toevoegen. Deze methode verzendt een transcript naar de **ContentModeratorClient.TextModeration.ScreenText** methode. Ook wordt het resultaat omgezet in een IList<TranscriptModerationBodyItem>, en verzendt naar **AddVideoTranscriptModerationResult**.

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

Voeg de **Main** methodedefinitie naamruimte VideoTranscriptReviews, klasse programma. Tot slot sluit de klasse Program en de VideoTranscriptReviews-naamruimte.

> [!NOTE]
> Het programma gebruikt een transcript voorbeeld in de VTT-indeling. In een echte oplossing, gebruikt u de service Azure Media Indexer [genereren een transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video. 

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
            Console.ReadKey();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Voer het programma uit en controleer de uitvoer

Als u de toepassing uitvoert, ziet u uitvoer op de volgende regels:

    Creating a video review.
    Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="navigate-to-your-video-transcript-review"></a>Navigeer naar de videotranscriptie controleren

Ga naar de videotranscriptie controle in uw Content Moderator-controlehulpprogramma op de **bekijken**>**Video**>**Transcript** scherm.

Ziet u de volgende functies:
- De twee regels van de tekst die u hebt toegevoegd
- De term grof taalgebruik gevonden en gemarkeerd door de service van de toezicht op tekst
- De video begint selecteren van een tekst transcriptie met tijdstempel

![Videotranscriptie beoordeling van menselijke moderators](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Volgende stappen

Krijgen de [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en de [Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET.

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) in het controlehulpprogramma.

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [videotoezicht oplossing voltooien](video-transcript-moderation-review-tutorial-dotnet.md).
