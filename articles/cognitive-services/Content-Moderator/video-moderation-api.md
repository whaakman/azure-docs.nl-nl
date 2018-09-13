---
title: Azure Content Moderator - videotoezicht | Microsoft Docs
description: Videotoezicht gebruiken om te scannen mogelijk erotische en ongepaste inhoud.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/02/2018
ms.author: sajagtap
ms.openlocfilehash: 27e189d93573dea139c2b67c237c376a28100c2b
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714306"
---
# <a name="video-moderation"></a>Beheer van video

Vandaag de dag online viewers genereren miljarden videoweergaven populaire en regionale sociale media-websites en verhogen. Door toe te passen op basis van machine-learning-services om te voorspellen van mogelijk erotische en ongepaste inhoud, moet u de kosten van uw inspanningen toezicht verlagen.

## <a name="sign-up-for-the-content-moderator-media-processor-preview"></a>Aanmelden voor de Content Moderator-media-processor (preview)

### <a name="create-a-free-azure-account"></a>Maak een gratis Azure-account

[Begin hier](https://azure.microsoft.com/free/) te maken van een gratis Azure-account als u er nog geen hebt.

### <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

De Content Moderator video mogelijkheid is beschikbaar als openbare preview **Mediaprocessor** in Azure Media Services (AMS) gratis.

[Maak een Azure Media Services-account](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) in uw Azure-abonnement.

### <a name="get-azure-active-directory-credentials"></a>Azure Active Directory-referenties ophalen

   1. Lees de [Azure Media Services-portal artikel](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad) voor meer informatie over het gebruik van Azure portal om op te halen van de referenties van uw Azure AD-verificatie.
   1. Lees de [Azure Media Services .NET-artikel](https://docs.microsoft.com/azure/media-services/media-services-dotnet-get-started-with-aad) voor informatie over het gebruik van uw Azure Active Directory-referenties met de .NET SDK.

   > [!NOTE]
   > De voorbeeldcode in deze snelstartgids maakt gebruik van de **service-principal verificatie** methode die wordt beschreven in zowel de artikelen.

Wanneer u uw AMS-referenties, zijn er twee manieren om te proberen de processor van de media Content Moderator.

## <a name="use-azure-media-services-explorer"></a>Azure Media Services Explorer gebruiken

Gebruik de interactieve [Azure Media Services (AMS) explorer](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) als u uw AMS-account, video's uploaden en scannen met de media Content Moderator processor. [Download en installeer deze](https://github.com/Azure/Azure-Media-Services-Explorer/releases) vanuit GitHub, en [de broncode Bladeren](http://github.com/Azure/Azure-Media-Services-Explorer) voor alles met de AMS-SDK.

![Azure Media Services explorer met Content Moderator](images/ams-explorer-content-moderator.PNG)

## <a name="net-quickstart-with-visual-studio-and-c"></a>Snelstartgids voor .NET met Visual Studio en C#

1. Toevoegen van een nieuwe **Console-app (.NET Framework)** project aan uw oplossing.

   Noem het project in de voorbeeldcode **VideoModeration**.

1. Selecteer dit project als opstartproject één voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeren van de volgende NuGet-pakketten beschikbaar op [NuGet](https://www.nuget.org/).

- windowsazure.mediaservices
- windowsazure.mediaservices.Extensions

### <a name="update-the-programs-using-statements"></a>Update het programma de using-instructies

Wijzig het programma de using-instructies toe.

    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.IO;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using System.Collections.Generic;


### <a name="initialize-application-specific-settings"></a>Initialiseren van toepassingsspecifieke instellingen

De volgende statische velden toevoegen aan de **programma** klasse in Program.cs.

    // declare constants and globals
    private static CloudMediaContext _context = null;
    private static CloudStorageAccount _StorageAccount = null;

    // Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
    // a list of Blobs to be processed.
    static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
    static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
    static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

    private static StorageCredentials _StorageCredentials = null;

    // Azure Media Services authentication. See the quickstart for how to get these.
    private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
    private const string CLIENT_ID = "YOUR CLIENT ID"
    private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

    // REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
    private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

    // Content Moderator Media Processor Nam
    private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

    // Input and Output files in the current directory of the executable
    private const string INPUT_FILE = "VIDEO FILE NAME";
    private const string OUTPUT_FOLDER = "";

### <a name="create-a-preset-file-json"></a>Maak een vooraf ingestelde bestand (json)

Maak een JSON-bestand in de huidige map met het versienummer.

    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";
    //Example file content:
    //        {
    //             "version": "2.0"
    //        }
    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

### <a name="add-the-following-code-to-the-main-method"></a>Voeg de volgende code toe aan de belangrijkste methode

De belangrijkste methode maakt eerst de Context van een Azure-Media en vervolgens een Azure Storage-Context voor het geval uw video's in blob-opslag zijn.
De resterende code scant een video van een lokale map, blob of meerdere blobs binnen een Azure storage-container.
Door de opmerkingen bij de andere regels code kunt u alle opties proberen.

    // Create Azure Media Context
    CreateMediaContext();

    // Create Storage Context
    CreateStorageContext();

    // Use a file as the input.
    // IAsset asset = CreateAssetfromFile();
    
    // -- OR ---
    
    // Or a blob as the input
    // IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

    // Then submit the asset to Content Moderator
    // RunContentModeratorJob(asset);

    //-- OR ----

    // Just run the content moderator on all blobs in a list (from a Blob Container)
    RunContentModeratorJobOnBlobs();

### <a name="add-the-code-to-create-an-azure-media-context"></a>Voeg de code voor het maken van de Context van een Azure-Media

    /// <summary>
    /// Creates a media context from azure credentials
    /// </summary>
    static void CreateMediaContext()
    {
        // Get Azure AD credentials
        var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
            new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
            AzureEnvironments.AzureCloudEnvironment);

        // Initialize an Azure AD token
        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        // Create a media context
        _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
    }

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Voeg de code voor het maken van een Azure Storage-Context
U de opslag-Context wordt gemaakt op basis van uw Storage-referenties gebruiken voor toegang tot de blob-opslag.

    /// <summary>
    /// Creates a storage context from the AMS associated storage name and key
    /// </summary>
    static void CreateStorageContext()
    {
        // Get a reference to the storage account associated with a Media Services account. 
        if (_StorageCredentials == null)
        {
            _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
        }
        _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
    }

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Voeg de code voor het maken van Azure Media-activa in lokale bestands- en blob
De processor van de media Content Moderator voert taken uit volgens **activa** binnen het Azure Media Services-platform.
Deze methoden maken de activa van een lokaal bestand of een bijbehorende blob.

    /// <summary>
    /// Creates an Azure Media Services Asset from the video file
    /// </summary>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromFile()
    {
        return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
    }

    /// <summary>
    /// Creates an Azure Media Services asset from your blog storage
    /// </summary>
    /// <param name="Blob"></param>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
    {
        // Create asset from the FIRST blob in the list and return it
        return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
    }

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>De code voor het scannen van een verzameling van video's (als blobs) binnen een container toevoegen

    /// <summary>
    /// Runs the Content Moderator Job on all Blobs in a given container name
    /// </summary>
    static void RunContentModeratorJobOnBlobs()
    {
        // Get the reference to the list of Blobs. See the following method.
        var blobList = GetBlobsList();

        // Iterate over the Blob list items or work on specific ones as needed
        foreach (var sourceBlob in blobList)
        {
            // Create an Asset
            IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                               _StorageCredentials, AssetCreationOptions.None);
            asset.Update();

            // Submit to Content Moderator
            RunContentModeratorJob(asset);
        }
    }

    /// <summary>
    /// Get all blobs in your container
    /// </summary>
    /// <returns></returns>
    static IEnumerable<IListBlobItem> GetBlobsList()
    {
        // Get a reference to the Container within the Storage Account
        // that has the files (blobs) for moderation
        CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
        CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

        // Get the reference to the list of Blobs 
        var blobList = MediaBlobContainer.ListBlobs();
        return blobList;
    }

### <a name="add-the-method-to-run-the-content-moderator-job"></a>De methode voor het uitvoeren van de Content Moderator-taak toevoegen

    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

### <a name="add-a-couple-of-helper-functions"></a>Toevoegen van een aantal ondersteunende functies

Deze methoden de Content Moderator-uitvoer-bestand (JSON) downloaden van de Azure Media Services-activa en het bijhouden van de status van de taak voor beheer, zodat het programma kan een actieve status Meld u aan de console.

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    // event handler for Job State
    static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job finished.");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
                Console.WriteLine("Job is canceled.\n");
                break;
            case JobState.Error:
                Console.WriteLine("Job failed.\n");
                break;
            default:
                break;
        }
    }

### <a name="run-the-program-and-review-the-output"></a>Voer het programma uit en controleer de uitvoer

Nadat de taak Content Moderation is voltooid, Analyseer het JSON-antwoord. Er bestaat een van deze elementen bevatten:

- Informatie over de video samenvatting
- **Foto's** als '**fragmenten**"
- **Sleutel frames** als '**gebeurtenissen**"met een **reviewRecommended ' (= true of false)"** vlag op basis van **volwassenen** en **Racy** scores
- **Start**, **duur**, **totalDuration**, en **timestamp** in 'maatstreepjes' zijn. Delen door **tijdschaal** om op te halen van het getal in seconden.
 
> [!NOTE]

> - `adultScore` Hiermee geeft u de mogelijke aanwezigheid en voorspelling score van inhoud die kan worden beschouwd als seksueel expliciet of volwassenen in bepaalde situaties.
> - `racyScore` Hiermee geeft u de mogelijke aanwezigheid en voorspelling score van inhoud die kan worden beschouwd als seksueel suggestieve of volwassen in bepaalde situaties.
> - `adultScore` en `racyScore` zijn tussen 0 en 1. Hoe hoger de score, hoe hoger het model is voorspellen van de categorie mogelijk van toepassing zijn. Deze Preview-versie, is afhankelijk van een statistische model in plaats van handmatig gecodeerde resultaten. Het is raadzaam om eerst te testen met uw eigen inhoud om te bepalen hoe elke categorie overeenstemt met uw vereisten.
> - `reviewRecommended` waar of ONWAAR afhankelijk van de interne score drempelwaarden is. Klanten moeten beoordelen of u wilt deze waarde wordt gebruikt of moet u besluiten welke aangepaste drempelwaarden op basis van hun inhoud beleid.
>

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) uit de uitvoer van uw beheer.

Voeg [transcript toezicht](video-transcript-moderation-review-tutorial-dotnet.md) op uw video beoordelingen.

Bekijk de gedetailleerde zelfstudie op over het bouwen van een [voltooien van de oplossing voor video- en transcript toezicht](video-transcript-moderation-review-tutorial-dotnet.md).

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET.
