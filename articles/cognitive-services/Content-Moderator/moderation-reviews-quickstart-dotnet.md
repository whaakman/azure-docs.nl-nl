---
title: Azure Content beheerder - maken beoordelingen met .NET | Microsoft Docs
description: Het maken van de beoordelingen met Azure inhoud beheerder SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 6a0ff48f4ea17f9c800f3e6c096df2492699f87a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344560"
---
# <a name="create-reviews-using-net"></a>Beoordelingen met .NET maken

Dit artikel bevat informatie en codevoorbeelden om u te helpen aan de slag met de inhoud beheerder SDK voor .NET aan:
 
- Maken van een reeks beoordelingen voor menselijke moderators
- De status van de bestaande beoordelingen voor menselijke moderators ophalen

Inhoud is over het algemeen doorloopt van sommige geautomatiseerde toezicht voordat het wordt gepland voor de menselijke controleren. Dit artikel behandelt alleen het maken van de revisie voor menselijke toezicht. Zie voor een uitgebreidere scenario is de [Facebook inhoud toezicht](facebook-post-moderation.md) en [e-commerce catalogus toezicht](ecommerce-retail-catalog-moderation.md) zelfstudies.

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.
Raadpleeg de [Quick Start](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Voeg een nieuwe **Console-app (.NET Framework)** project om uw oplossing.

   Noem het project in de voorbeeldcode **CreateReviews**.

1. Selecteer dit project als opstartproject één voor de oplossing.

1. Voeg een verwijzing naar de **ModeratorHelper** project assembly die u hebt gemaakt in de [inhoud beheerder client helper Quick Start](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>De vereiste pakketten installeren

De volgende NuGet-pakketten installeren:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Update van het programma de using-instructies

Wijzig het programma de using-instructies.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Maak een klasse is interne informatie over de inhoud koppelen aan een revisie-ID

Voeg de volgende klasse naar de **programma** klasse.
Deze klasse gebruiken om te koppelen van de revisie-ID aan uw interne inhoud-ID voor het item.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Initialiseren van toepassingsspecifieke instellingen

> [!NOTE]
> De sleutel van uw inhoud beheerder service heeft een aanvragen per frequentielimiet van tweede (RPS) en als u de limiet overschrijdt, de SDK er een uitzondering gegenereerd met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Voeg de volgende constanten de **programma** klasse in Program.cs.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Voeg de volgende constanten en statische velden voor de **programma** klasse in Program.cs.

Werk deze waarden om informatie die specifiek zijn voor uw abonnement en team te bevatten.

> [!NOTE]
> U de constante TeamName ingesteld op de naam die u hebt gebruikt bij het maken van uw [inhoud beheerder revisie hulpprogramma](https://contentmoderator.cognitive.microsoft.com/) abonnement. Ophalen van de TeamName van de **referenties** sectie het **instellingen** menu (tandwielpictogram).
>
> De teamnaam van uw is de waarde van de **Id** veld in de **API** sectie.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>De volgende statische velden toevoegen aan de **programma** klasse in Program.cs.

Gebruik deze velden voor het bijhouden van de status van de toepassing.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Maken van een methode voor berichten schrijven naar het logboekbestand

Voeg de volgende methode toe aan de klasse **Program**. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Een methode voor het maken van een reeks beoordelingen maken

Normaal gesproken hebt u een aantal zakelijke logica voor het identificeren van binnenkomende afbeeldingen, tekst, of video die moet worden gecontroleerd. Hier alleen gebruiken, een vaste lijst met afbeeldingen.

Voeg de volgende methode toe aan de klasse **Program**.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Een methode om de status van de bestaande beoordelingen maken

Voeg de volgende methode toe aan de klasse **Program**. 

> [!Note]
> In de praktijk, stelt u de URL van de callback `CallbackEndpoint` naar de URL die van de resultaten van de handmatige controle (via een HTTP POST-aanvraag ontvangen zou).
> U kunt deze methode om te controleren op de status van in behandeling zijnde beoordelingen kan wijzigen.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Voeg de code voor het maken van een reeks beoordelingen en de status ervan controleren

Voeg de volgende code naar de **Main** methode.

Deze code wordt gesimuleerd veel van de bewerkingen die u uitvoert in definiëren en beheren van de lijst, evenals met behulp van de lijst om installatiekopieën van het scherm. De logboekregistratiefuncties kunnen u zien de antwoord-objecten die worden gegenereerd door de SDK-aanroepen naar de inhoud van beheerder-service.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Voer het programma en controleer de uitvoer

Ziet u de volgende voorbeelduitvoer:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Meld u aan bij de beheerder van de inhoud bekijken hulpprogramma om te zien van de installatiekopie van het in behandeling controleren met de **sc** label ingesteld op **true**. U ziet ook de standaard **een** en **r** tags en eventuele aangepaste labels die u mogelijk hebt gedefinieerd in het hulpprogramma voor beoordeling. 

Gebruik de **volgende** knop verzenden.

![Installatiekopie van een menselijke moderators revisie](images/moderation-reviews-quickstart-dotnet.PNG)

Druk op een willekeurige toets om door te gaan.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Bekijk de volgende uitvoer in het logboekbestand.

> [!NOTE]
> In uw bestand voor uitvoer, de tekenreeksen '\{teamname} ' en '\{callbackUrl} ' overeenstemming met de waarden voor de `TeamName` en `CallbackEndpoint` velden, respectievelijk.

De revisie-id's en de installatiekopie van het inhoud-URL's verschillend telkens wanneer u de toepassing uitvoeren en wanneer een revisie is voltooid, de `reviewerResultTags` het veld wordt weergegeven hoe de revisor tags voor het item.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>De Url van de callback ontvangt als hebt opgegeven, deze reactie

Er is een antwoord op het volgende voorbeeld:

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Volgende stappen

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET, en op uw integratie aan de slag.
