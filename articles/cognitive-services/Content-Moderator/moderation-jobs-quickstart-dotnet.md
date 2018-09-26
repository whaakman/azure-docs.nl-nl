---
title: Azure Content Moderator - Start beheer van taken met behulp van .NET | Microsoft Docs
description: Het starten van toezicht op taken met Azure Content Moderator-SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 3761552f81bd733f9c93fab40db07ef6f5a6a7f6
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181597"
---
# <a name="start-moderation-jobs-using-net"></a>Start beheer van taken met behulp van .NET

In dit artikel vindt u informatie en voorbeelden van code om u te helpen aan de slag met de [Content Moderator-SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) aan:
 
- Een taak toezicht op om te scannen en beoordelingen voor menselijke moderators maken starten
- Haal de status van de in behandeling controleren
- Bijhouden en de definitieve status van de beoordeling ophalen
- Verzenden van het resultaat dat de callback-Url

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator"></a>Aanmelden voor Content Moderator

Voordat u de Content Moderator-services via de REST-API of de SDK gebruiken kunt, moet u de abonnementssleutel van een.
Raadpleeg de [snelstartgids](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Zich registreren voor een hulpprogramma voor beoordeling account als niet in de vorige stap is voltooid

Als u uw Content Moderator vanuit de Azure-portal ook hebt [zich registreren voor de beoordeling hulpprogramma account](https://contentmoderator.cognitive.microsoft.com/) en maken van een beoordelingsteam. U moet het team-Id en het beoordelingsprogramma de API controleren als u wilt een taak starten en de evaluaties weergeven in het controlehulpprogramma aan te roepen.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Zorg ervoor dat uw API-sleutel de beoordeling-API kunt aanroepen voor het maken van de beoordeling

Na het voltooien van de vorige stappen, zou u uiteindelijk met twee Content Moderator-sleutels als u vanuit de Azure-portal gestart. 

Als u van plan bent de Azure-opgegeven API-sleutel in de SDK-voorbeeld gebruiken, volgt u de stappen in de [met behulp van Azure-sleutel met de API controleren](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) sectie waarmee uw toepassing in de beoordeling-API aanroepen en beoordelingen te maken.

Als u de gratis proefversie sleutel gegenereerd door het beoordelingsprogramma, uw beoordeling hulpprogramma-account al op de hoogte van de sleutel en daarom geen extra stappen zijn vereist.

## <a name="define-a-custom-moderation-workflow"></a>Een werkstroom aangepaste toezicht definiëren

Een beheer-taak scant uw inhoud met behulp van de API's en maakt gebruik van een **werkstroom** om te bepalen of beoordelingen of niet maken.
Terwijl het beoordelingsprogramma een standaardwerkstroom, gaan we bevat [definiëren van een aangepaste werkstroom](Review-Tool-User-Guide/Workflows.md) voor deze Quick Start.

U kunt de naam van de werkstroom gebruiken in uw code waarmee de beheer-taak wordt gestart.

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Toevoegen van een nieuwe **Console-app (.NET Framework)** project aan uw oplossing.

   Noem het project in de voorbeeldcode **CreateReviews**.

1. Selecteer dit project als opstartproject één voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeer de volgende NuGet-pakketten:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Update het programma de using-instructies

Wijzig het programma de using-instructies toe.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>De Content Moderator-client maken

Voeg de volgende code voor het maken van een Content Moderator-client voor uw abonnement.

> [!IMPORTANT]
> Update de **Azureregio** en **CMSubscriptionKey** velden met de waarden van uw regio-id en de abonnement-sleutel.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

### <a name="initialize-application-specific-settings"></a>Initialiseren van toepassingsspecifieke instellingen

Voeg de volgende constanten en statische velden die u wilt de **programma** klasse in Program.cs.

> [!NOTE]
> U de constante TeamName ingesteld op de naam die u hebt gebruikt toen u uw abonnement Content Moderator gemaakt. Ophalen van TeamName uit de [website Content Moderator](https://westus.contentmoderator.cognitive.microsoft.com/).
> Zodra u zich aanmeldt, selecteert u **referenties** uit de **instellingen** (tandwielpictogram)-menu.
>
> De teamnaam van uw is de waarde van de **Id** veld in de **API** sectie.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Voeg code toe aan automatische matige, maken van een beoordeling en de taakgegevens ophalen

> [!Note]
> In de praktijk moet u de callback-URL instellen **CallbackEndpoint** naar de URL die de resultaten van de handmatige controle (via een HTTP POST-aanvraag) ontvangt.

Beginnen met het toevoegen van de volgende code om de **Main** methode.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the name of the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> De sleutel van uw Content Moderator-service heeft een aantal aanvragen per limiet voor tweede (RPS). Als u de limiet overschrijdt, wordt in de SDK een uitzondering met een foutcode 429 genereert. 
>
> De sleutel van een gratis laag heeft een limiet van één RPS.

## <a name="run-the-program-and-review-the-output"></a>Voer het programma uit en controleer de uitvoer

U ziet het volgende voorbeeld van uitvoer in de console:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Meld u aan bij de Content Moderator-controlehulpprogramma om te zien van de installatiekopie van het in behandeling controleren.

Gebruik de **volgende** knop om in te dienen.

![Beoordeling van de installatiekopie van menselijke moderators](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Zie het voorbeeld van uitvoer in het logboekbestand

> [!NOTE]
> In uw bestand voor uitvoer, de tekenreeksen **Teamname**, **ContentId**, **CallBackEndpoint**, en **WorkflowId** weerspiegelen de waarden die u hebt gebruikt eerder.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>De Url voor terugbellen voor ontvangt als is opgegeven, deze reactie.

U ziet een antwoord weergegeven zoals in het volgende voorbeeld:

> [!NOTE]
> In de callback-antwoord wordt de tekenreeksen **ContentId** en **WorkflowId** weerspiegelen de waarden die u eerder hebt gebruikt.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Volgende stappen

Krijgen de [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en de [Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET, en aan de slag met uw integratie.
