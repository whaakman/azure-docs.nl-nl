---
title: Video-inhoud voor aanstootgevende materiaal in analyseren C# -Content Moderator
titlesuffix: Azure Cognitive Services
description: Over het analyseren van video-inhoud voor verschillende ongewenst materiaal met de inhoud Moderator SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 7e987c1249360b14fddf8af57c61fdd1a46ee6c5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864788"
---
# <a name="analyze-video-content-for-objectionable-material-in-c"></a>Video-inhoud voor aanstootgevende materiaal in analyserenC#

In dit artikel vindt u informatie en voorbeelden van code om u te helpen aan de slag met de [inhoud Moderator SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) voor het scannen van video-inhoud voor volwassenen of ongepaste inhoud.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/)

## <a name="set-up-azure-resources"></a>Azure-resources instellen

De Content Moderator videotoezicht mogelijkheid is beschikbaar als een preview-versie gratis **Mediaprocessor** in Azure Media Services (AMS). Azure Media Services is een speciale Azure-service voor het opslaan en streamen van video-inhoud. 

### <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

Volg de instructies in [maken van een Azure Media Services-account](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) abonneren op AMS en een gekoppelde Azure storage-account maken. Maak een nieuwe Blob storage-container in dat opslagaccount.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Navigeer naar het nieuwe AMS-abonnement in Azure portal en selecteer **API-toegang** in het menu aan clientzijde. Selecteer **verbinding maken met Azure Media Services met service-principal**. Noteer de waarde in de **REST API-eindpunt** veld; u deze later nodig.

In de **Azure AD-app** sectie, selecteer **nieuw** en noem uw nieuwe Azure AD-toepassing registreren (bijvoorbeeld ' VideoModADApp'). Klik op **opslaan** en een paar minuten wachten terwijl de toepassing is geconfigureerd. Vervolgens ziet u uw nieuwe app-registratie onder de **Azure AD-app** sectie van de pagina.

Selecteer uw app-registratie en klik op de **toepassing beheren** knop eronder. Noteer de waarde in de **toepassings-ID** veld; u deze later nodig. Selecteer **instellingen** > **sleutels**, en voer een beschrijving in voor een nieuwe sleutel (zoals 'VideoModKey'). Klik op **opslaan**, en vervolgens ziet u de waarde van de nieuwe sleutel. Kopieer deze tekenreeks en sla deze ergens veilig.

Zie voor een uitgebreider overzicht van het bovenstaande proces, [aan de slag met Azure AD-verificatie](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad).

Als u dit hebt gedaan, kunt u de Mediaprocessor die videotoezicht op twee verschillende manieren kunt gebruiken.

## <a name="use-azure-media-services-explorer"></a>Azure Media Services Explorer gebruiken

Azure Media Services Explorer is een gebruiksvriendelijke frontend voor AMS. Gebruik dit voor uw AMS-account te bladeren, video's uploaden en inhoud met de Content Moderator Mediaprocessor scannen. Download en installeer deze vanaf [GitHub](https://github.com/Azure/Azure-Media-Services-Explorer/releases), of Raadpleeg de [Azure Media Services Explorer-blogbericht](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) voor meer informatie.

![Azure Media Services explorer met Content Moderator](images/ams-explorer-content-moderator.PNG)

## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

1. In Visual Studio, maak een nieuwe **Console-app (.NET Framework)** project en geef deze de naam **VideoModeration**. 
1. Als uw oplossing nog meer projecten bevat, selecteert u deze als het enkele opstartproject.
1. Download de vereiste NuGet-pakketten. Klik met de rechtermuisknop op uw project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Zoek en installeer vervolgens de volgende pakketten:
    - windowsazure.mediaservices
    - windowsazure.mediaservices.extensions

## <a name="add-video-moderation-code"></a>Videotoezicht code toevoegen

Vervolgens kopieert en plakt u de code uit deze handleiding in uw project om een eenvoudig scenario voor inhoudstoezicht te implementeren.

### <a name="update-the-programs-using-statements"></a>De using-instructies van het programma bijwerken

Voeg aan het begin van het bestand _Program.cs_ de volgende `using`-instructies toe.

```csharp
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
```

### <a name="set-up-resource-references"></a>Verwijzingen van de resource instellen

Voeg de volgende statische velden toe aan de klasse **Program** in _Program.cs_. Deze velden bevatten de informatie die nodig zijn om verbinding te maken met uw AMS-abonnement. Vul deze met de waarden die u hebt verkregen in de bovenstaande stappen. Houd er rekening mee dat `CLIENT_ID` is de **toepassings-ID** waarde van uw Azure AD-app en `CLIENT_SECRET` is de waarde van de 'VideoModKey' die u hebt gemaakt voor die app.

```csharp
// declare constants and globals
private static CloudMediaContext _context = null;
private static CloudStorageAccount _StorageAccount = null;

// Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
// a list of Blobs to be processed.
static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

private static StorageCredentials _StorageCredentials = null;

// Azure Media Services authentication. 
private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
private const string CLIENT_ID = "YOUR CLIENT ID";
private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

// REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

// Content Moderator Media Processor Nam
private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

// Input and Output files in the current directory of the executable
private const string INPUT_FILE = "VIDEO FILE NAME";
private const string OUTPUT_FOLDER = "";

// JSON settings file
private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

```

Als u wilt gebruiken van een lokale videobestand (meest eenvoudige geval), toe te voegen aan het project en voert u het pad als de `INPUT_FILE` waarde (relatieve paden zijn ten opzichte van de map kan worden uitgevoerd).

U moet ook maken de _preset.json_ -bestand in de huidige map en deze gebruiken om een uniek versienummer. Bijvoorbeeld:

```JSON
{
    "version": "2.0"
}
```

### <a name="load-the-input-videos"></a>De invoer video(s) laden

De **Main** -methode van de **programma** klasse maakt u de Context van een Azure-Media en vervolgens de Context van een Azure-opslag (in het geval uw video's in blob-opslag zijn). De resterende code scant een video van een lokale map, blob of meerdere blobs binnen een Azure storage-container. Door de opmerkingen bij de andere regels code kunt u alle opties proberen.

```csharp
// Create Azure Media Context
CreateMediaContext();

// Create Storage Context
CreateStorageContext();

// Use a file as the input.
IAsset asset = CreateAssetfromFile();

// -- OR ---

// Or a blob as the input
// IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

// Then submit the asset to Content Moderator
RunContentModeratorJob(asset);

//-- OR ----

// Just run the content moderator on all blobs in a list (from a Blob Container)
// RunContentModeratorJobOnBlobs();
```

### <a name="create-an-azure-media-context"></a>De Context van een Azure Media maken

Voeg de volgende methode toe aan de klasse **Program**. Dit maakt gebruik van uw AMS-referenties voor de communicatie met AMS.

```csharp
// Creates a media context from azure credentials
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
```

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Voeg de code voor het maken van een Azure Storage-Context

Voeg de volgende methode toe aan de klasse **Program**. U de opslag-Context wordt gemaakt op basis van uw storage-referenties gebruiken voor toegang tot de blob-opslag.

```csharp
// Creates a storage context from the AMS associated storage name and key
static void CreateStorageContext()
{
    // Get a reference to the storage account associated with a Media Services account. 
    if (_StorageCredentials == null)
    {
        _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
    }
    _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
}
```

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Voeg de code voor het maken van Azure Media-activa in lokale bestands- en blob

De processor van de media Content Moderator voert taken uit volgens **activa** binnen het Azure Media Services-platform.
Deze methoden maken de activa van een lokaal bestand of een bijbehorende blob.

```csharp
// Creates an Azure Media Services Asset from the video file
static IAsset CreateAssetfromFile()
{
    return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
}

// Creates an Azure Media Services asset from your blog storage
static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
{
    // Create asset from the FIRST blob in the list and return it
    return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
}
```

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>De code voor het scannen van een verzameling van video's (als blobs) binnen een container toevoegen

```csharp
// Runs the Content Moderator Job on all Blobs in a given container name
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

// Get all blobs in your container
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
```

### <a name="add-the-method-to-run-the-content-moderator-job"></a>De methode voor het uitvoeren van de Content Moderator-taak toevoegen

```csharp
// Run the Content Moderator job on the designated Asset from local file or blob storage
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
```

### <a name="add-helper-functions"></a>Ondersteunende functies toevoegen

Deze methoden de Content Moderator-uitvoer-bestand (JSON) downloaden van de Azure Media Services-activa en het bijhouden van de status van de taak voor beheer, zodat het programma kan een actieve status Meld u aan de console.

```csharp
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
```

### <a name="run-the-program-and-review-the-output"></a>Het programma uitvoeren en de uitvoer controleren

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

```json
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
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) uit de uitvoer van uw beheer.

Voeg [transcript toezicht](video-transcript-moderation-review-tutorial-dotnet.md) op uw video beoordelingen.

Bekijk de gedetailleerde zelfstudie op over het bouwen van een [voltooien van de oplossing voor video- en transcript toezicht](video-transcript-moderation-review-tutorial-dotnet.md).

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET.
