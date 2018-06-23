---
title: Azure Content beheerder - Starttaken zijn interrupts met .NET | Microsoft Docs
description: Het starten van toezicht taken met Azure inhoud beheerder SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/06/2018
ms.author: sajagtap
ms.openlocfilehash: a103875607355993e216ce1ddea02009fc8fa1c4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344511"
---
# <a name="start-moderation-jobs-using-net"></a>Met .NET toezicht-taken starten

Dit artikel bevat informatie en codevoorbeelden om u te helpen aan de slag met de inhoud beheerder SDK voor .NET aan:
 
- Een taak controle om te controleren en beoordelingen voor menselijke moderators maken starten
- De status van de in behandeling controleren ophalen
- Volgen en de laatste status van de evaluatie
- Het resultaat dat de Url van de callback verzenden

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.
Raadpleeg de [Quick Start](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="define-a-custom-moderation-workflow"></a>Een werkstroom aangepaste toezicht definiëren

Een taak toezicht scant uw inhoud met behulp van de API's en gebruikt een **werkstroom** om te bepalen of beoordelingen of niet maken.
Hoewel het hulpprogramma voor beoordeling een standaardwerkstroom we bevat [definiëren van een aangepaste werkstroom](Review-Tool-User-Guide/Workflows.md) voor deze snelstartgids.

U kunt de naam van de werkstroom gebruiken in uw code die de taak toezicht wordt gestart.

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

### <a name="initialize-application-specific-settings"></a>Initialiseren van toepassingsspecifieke instellingen

Voeg de volgende constanten en statische velden voor de **programma** klasse in Program.cs.

> [!NOTE]
> U de constante TeamName ingesteld op de naam die u hebt gebruikt toen u uw abonnement inhoud beheerder gemaakt. Ophalen van TeamName van de [inhoud beheerder website](https://westus.contentmoderator.cognitive.microsoft.com/).
> Zodra u zich aanmeldt, selecteert u **referenties** van de **instellingen** menu (tandwielpictogram).
>
> De teamnaam van uw is de waarde van de **ID** veld in de **API** sectie.


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
    /// the Conent Moderator web site. Your team name is the Id associated 
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
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Voeg code toe aan de automatische matige, een beoordeling maken en de taakdetails van de ophalen

> [!Note]
> In de praktijk, stelt u de callback-URL **CallbackEndpoint** naar de URL die de resultaten van de handmatige controle (via een HTTP POST-aanvraag) ontvangt.

Gestart door de volgende code toe te voegen de **Main** methode.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the nameof the workflow defined in the online review tool.
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
> De sleutel van uw inhoud beheerder-service heeft een aantal aanvragen per frequentielimiet van tweede (RPS). Als u de limiet overschrijdt, genereert de SDK een uitzondering met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

## <a name="run-the-program-and-review-the-output"></a>Voer het programma en controleer de uitvoer

Ziet u de volgende voorbeelduitvoer in de console:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Meld u aan bij de beheerder van de inhoud bekijken hulpprogramma om te zien van de installatiekopie van het in behandeling controleren.

Gebruik de **volgende** knop verzenden.

![Installatiekopie van een menselijke moderators revisie](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Zie het voorbeeld in het logboekbestand

> [!NOTE]
> In uw bestand voor uitvoer, de tekenreeksen **Teamname**, **ContentId**, **CallBackEndpoint**, en **WorkflowId** overeenstemming met de waarden die u gebruikt oudere versies.

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


## <a name="your-callback-url-if-provided-receives-this-response"></a>De Url van de callback ontvangt als hebt opgegeven, deze reactie.

Er is een antwoord op het volgende voorbeeld:

> [!NOTE]
> In uw antwoord retouraanroep de tekenreeksen **ContentId** en **WorkflowId** overeenstemming met de waarden die u eerder hebt gebruikt.

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

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET, en op uw integratie aan de slag.
