---
title: Azure Content beheerder - gemiddeld video's en transcripties in .NET | Microsoft Docs
description: Het gebruik van inhoud beheerder op normaal video's en transcripties in .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: a084b50e44fe26ba2547d0f7b7ed184fb71b190c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "35344788"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Video's en de tekst van controle-zelfstudie

Inhoud beheerder video API's kunnen u gemiddeld video's en video beoordelingen maken in het hulpprogramma menselijke controleren. 

Deze gedetailleerde zelfstudie helpt om te begrijpen hoe een volledige video's en de tekst van toezicht oplossing bouwen met toezicht machine ondersteund en human in de lus revisie maken.

Download de [C#-consoletoepassing](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) voor deze zelfstudie. De consoletoepassing maakt gebruik van de SDK en de verwante pakketten naar de volgende taken uitvoeren:

- De invoer video(s) voor een snellere verwerking comprimeren
- De video om schermafbeeldingen en frames met insights gemiddeld
- De tijdstempels frame gebruiken voor het maken van miniatuurweergaven (installatiekopieën)
- Verzenden van tijdstempels en miniaturen video beoordelingen maken
- De video spraak converteren naar tekst (tekst) met de Media Indexer-API
- De tekst van de met de tekst toezicht service gemiddeld
- De tekst van de gecontroleerde toevoegen aan de video controleren

## <a name="sample-program-outputs"></a>Voorbeeldcode levert

Voordat u doorgaat, bekijken we de uitvoer van dit voorbeeld van het programma:

- [Console-uitvoer](#program-output)
- [Video controleren](#video-review-default-view)
- [De tekst weergeven](#video-review-transcript-view)

## <a name="prerequisites"></a>Vereisten

1. Aanmelden voor de [inhoud beheerder revisie hulpprogramma](https://contentmoderator.cognitive.microsoft.com/) website en [aangepaste labels maken](Review-Tool-User-Guide/tags.md) waarmee de C#-consoletoepassing toegewezen uit binnen de code. Het volgende scherm ziet u de aangepaste labels.

  ![Aangepaste labels video toezicht](images/video-tutorial-custom-tags.png)

1. Als u wilt de voorbeeldtoepassing uitvoeren, moet u een Azure-account en een Azure Media Services-account. Bovendien moet u toegang tot de inhoud beheerder private preview. Tot slot moet u referenties voor Azure Active Directory-verificatie. Zie voor meer informatie over het verkrijgen van deze informatie [de Video toezicht API Quick Start](video-moderation-api.md).

1. Bewerk het bestand `App.config` en voegt u de naam van de Active Directory-tenant, service-eindpunten toe abonnement sleutels aangegeven door `#####`. U moet de volgende informatie:

|Sleutel|Beschrijving|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Eindpunt voor de API van Azure Media Services (AMS)|
|`ClientSecret`|Abonnementssleutel voor Azure Media Services|
|`ClientId`|Client-ID voor Azure mediaservices|
|`AzureAdTenantName`|Active Directory-tenantnaam voor uw organisatie|
|`ContentModeratorReviewApiSubscriptionKey`|Abonnementssleutel voor de beheerder van de inhoud bekijken API|
|`ContentModeratorApiEndpoint`|Eindpunt voor de inhoud beheerder API|
|`ContentModeratorTeamId`|Inhoud beheerder team-ID|

## <a name="getting-started"></a>Aan de slag

De klasse `Program` in `Program.cs` is het belangrijkste toegangspunt aan de toepassing video toezicht.

### <a name="methods-of-class-program"></a>Methoden van klasse programma

|Methode|Beschrijving|
|-|-|
|`Main`|Opdrachtregel parseert, invoer van de gebruiker moeten worden verzameld en begint met de verwerking.|
|`ProcessVideo`|Comprimeert, uploadt, matigt en video beoordelingen maakt.|
|`CreateVideoStreamingRequest`|Maakt een stroom voor het uploaden van een video|
|`GetUserInputs`|Verzamelt gebruikersinvoer; Als er geen opdrachtregelopties aanwezig zijn gebruikt|
|`Initialize`|Initialiseert de objecten die nodig zijn voor het toezichtproces|

### <a name="the-main-method"></a>De Main-methode

`Main()` is waarbij de uitvoering wordt gestart, dus is het beginpunt inzicht in het toezichtproces video.

    static void Main(string[] args)
    {
        if (args.Length == 0)
        {
            string videoPath = string.Empty;
            GetUserInputs(out videoPath);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(Path.GetDirectoryName(videoPath), "log.txt");
            try
            {
                ProcessVideo(videoPath).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        else
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(args[0]);
            if (args.Length == 2)
                bool.TryParse(args[1], out generateVtt);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(args[0], "log.txt");
            var files = directoryInfo.GetFiles("*.mp4", SearchOption.AllDirectories);
            foreach (var file in files)
            {
                try
                {
                    ProcessVideo(file.FullName).Wait();
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }

`Main()` de volgende opdrachtregelargumenten verwerkt:

- Het pad naar een map met video MPEG-4-bestanden kunnen worden verzonden voor controle. Alle `*.mp4` bestanden in deze map en submappen zijn ingediend voor controle.
- U kunt desgewenst een Booleaanse (waar/onwaar) vlag die aangeeft of tekst transcripties omwille van de benodigde audio moeten worden gegenereerd.

Als er geen opdrachtregelargumenten zijn, `Main()` aanroepen `GetUserInputs()`. Deze methode wordt de gebruiker het pad naar een enkele videobestand invoeren en op te geven of de tekst van een tekst moet worden gegenereerd.

> [!NOTE]
> De consoletoepassing maakt gebruik van de [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) transcripties genereren van de geüploade video-nummer. De resultaten zijn opgegeven in WebVTT-indeling. Zie voor meer informatie over deze indeling [Web Video tekstindeling houdt](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Initialiseren en ProcessVideo methoden

Ongeacht of het programma opties afkomstig zijn vanaf de opdrachtregel of van interactieve gebruikersinvoer `Main()` volgende aanroepen `Initialize()` voor het maken van de volgende gevallen:

|Klasse|Beschrijving|
|-|-|
|`AMSComponent`|Videobestanden comprimeren voordat u ze verzendt voor controle.|
|`AMSconfigurations`|De configuratiegegevens van de toepassing, gevonden in de interface `App.config`.|
|`VideoModerator`| Uploaden, coderen, versleuteling en toezicht met AMS SDK|
|`VideoReviewApi`|Video beoordelingen in de service inhoud beheerder beheert|

Deze klassen (aside van `AMSConfigurations`, die is eenvoudig) worden behandeld in meer detail in toekomstige gedeelten van deze zelfstudie.

Ten slotte de videobestanden verwerkte één op een tijdstip zijn door aan te roepen `ProcessVideo()` voor elk.

    private static async Task ProcessVideo(string videoPath)
    {
        Stopwatch sw = new Stopwatch();
        sw.Start();
        Console.ForegroundColor = ConsoleColor.White;
        Console.WriteLine("\nVideo compression process started...");

        var compressedVideoPath = amsComponent.CompressVideo(videoPath);
        if (string.IsNullOrWhiteSpace(compressedVideoPath))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Compression failed.");
        }

        Console.WriteLine("\nVideo compression process completed...");

        UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
        UploadAssetResult uploadResult = new UploadAssetResult();

        if (generateVtt)
        {
            uploadResult.GenerateVTT = generateVtt;
        }
        Console.WriteLine("\nVideo moderation process started...");

        if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Review process failed.");
        }

        Console.WriteLine("\nVideo moderation process completed...");
        Console.WriteLine("\nVideo review process started...");
        string reviewId = await videoReviewApi.CreateVideoReviewInContentModerator(uploadResult);
        Console.WriteLine("\nVideo review successfully completed...");
        sw.Stop();
        Console.WriteLine("\nTotal Elapsed Time: {0}", sw.Elapsed);
        using (var stw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            stw.WriteLine("Video File Name: " + Path.GetFileName(videoPath));
            stw.WriteLine($"ReviewId: {reviewId}");
            stw.WriteLine("Total Elapsed Time: {0}", sw.Elapsed);
        }
    }


De `ProcessVideo()` methode is zeer eenvoudig. De volgende bewerkingen worden uitgevoerd in de volgorde:

- De video comprimeren
- De video uploadt naar een Azure Media Services-asset
- Maakt een taak AMS voor gemiddelde van de video
- Maakt een video-overzicht in inhoud beheerder

Overweeg de volgende secties in meer detail aantal afzonderlijke processen aangeroepen door `ProcessVideo()`. 

## <a name="compressing-the-video"></a>Comprimeren van de video

Om het netwerkverkeer te minimaliseren, de toepassing videobestanden converteert naar H.264 (MPEG-4 AVC)-indeling en deze wordt aangepast aan een maximale breedte van 640 pixels. De codec H.264 wordt aanbevolen vanwege de hoge efficiëntie (compressie tarief). De compressie wordt gedaan met behulp van de gratis `ffmpeg` opdrachtregelprogramma, die is opgenomen in de `Lib` map van de Visual Studio-oplossing. De invoerbestanden mogelijk van een willekeurige indeling wordt ondersteund door `ffmpeg`, met inbegrip van de meest gebruikte video bestandsindelingen en codecs.

> [!NOTE]
> Wanneer u het programma opdrachtregelopties gebruiken start, geeft u een map met de bestanden kunnen worden verzonden voor controle. Alle bestanden in deze map met de `.mp4` bestandsnaamextensie worden verwerkt. Voor het verwerken van andere bestandsnaamextensies, werken de `Main()` methode in `Program.cs` om op te nemen van de gewenste uitbreidingen.

De code die een enkele videobestand comprimeert is de `AmsComponent` -klasse in `AMSComponent.cs`. De methode die verantwoordelijk is voor deze functionaliteit is `CompressVideo()`, hier weergegeven.

    public string CompressVideo(string videoPath)
    {
        string ffmpegBlobUrl;
        if (!ValidatePreRequisites())
        {
            Console.WriteLine("Configurations check failed. Please cross check the configurations!");
            throw new Exception();
        }

        if (File.Exists(_configObj.FfmpegExecutablePath))
        {
            ffmpegBlobUrl = this._configObj.FfmpegExecutablePath;
        }
        else
        {
            Console.WriteLine("ffmpeg.exe is missing. Please check the Lib folder");
            throw new Exception();
        }

        string videoFilePathCom = videoPath.Split('.')[0] + "_c.mp4";
        ProcessStartInfo processStartInfo = new ProcessStartInfo();
        processStartInfo.WindowStyle = ProcessWindowStyle.Hidden;
        processStartInfo.FileName = ffmpegBlobUrl;
        processStartInfo.Arguments = "-i \"" + videoPath + "\" -vcodec libx264 -n -crf 32 -preset veryfast -vf scale=640:-1 -c:a aac -aq 1 -ac 2 -threads 0 \"" + videoFilePathCom + "\"";
        var process = Process.Start(processStartInfo);
        process.WaitForExit();
        process.Close();
        return videoFilePathCom;
    }

De code voert de volgende stappen uit:

- Controles om te controleren of de configuratie in `App.config` bevat alle benodigde gegevens
- Controles om te controleren of de `ffmpeg` binaire aanwezig is
- Het uitvoerbestand builds door toe te voegen `_c.mp4` aan de basisnaam van het bestand (zoals `Example.mp4`  ->  `E>xample_c.mp4`)
- Maakt een opdrachtregeltekenreeks als de conversie wilt uitvoeren
- Start een `ffmpeg` verwerkt via de opdrachtregel
- Wacht tot de video om te worden verwerkt

> [!NOTE]
> Als u uw video's al zijn gecomprimeerd via H.264 en de juiste afmetingen hebben, kunt u herschrijven `CompressVideo()` overslaan van de compressie.

De methode retourneert de bestandsnaam van het gecomprimeerde uitvoerbestand.

## <a name="uploading-and-moderating-the-video"></a>Uploaden en toezicht houden de video

De video moet worden opgeslagen in Azure Media Services voordat deze kan worden verwerkt door de inhoud toezicht-service. De `Program` -klasse in `Program.cs` bevat een korte methode `CreateVideoStreamingRequest()` die retourneert een object dat u de streaming-aanvraag voor het uploaden van de video vertegenwoordigt.

    private static UploadVideoStreamRequest CreateVideoStreamingRequest(string compressedVideoFilePath)
    {
        return
            new UploadVideoStreamRequest
            {
                VideoStream = File.ReadAllBytes(compressedVideoFilePath),
                VideoName = Path.GetFileName(compressedVideoFilePath),
                EncodingRequest = new EncodingRequest()
                {
                    EncodingBitrate = AmsEncoding.AdaptiveStreaming
                },
                VideoFilePath = compressedVideoFilePath
            };
    }

De resulterende `UploadVideoStreamRequest` -object is gedefinieerd `UploadVideoStreamRequest.cs` (en de bovenliggende `UploadVideoRequest`in `UploadVideoRequest.cs`). Deze klassen worden niet hier; weergegeven ze zijn kort en alleen voor de gecomprimeerde video gegevens en informatie over het leveren. Een andere gegevens alleen-lezen-klasse, `UploadAssetResult` (`UploadAssetResult.cs`) wordt gebruikt om de resultaten van het uploadproces. Nu is het mogelijk om te begrijpen die regels op `ProcessVideo()`:

    UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
    UploadAssetResult uploadResult = new UploadAssetResult();

    if (generateVtt)
    {
        uploadResult.GenerateVTT = generateVtt;
    }
    Console.WriteLine("\nVideo moderation process started...");

    if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine("Video Review process failed.");
    }

Deze regels zijn de volgende taken uitvoeren:

- Maak een `UploadVideoStreamRequest` voor het uploaden van de gecomprimeerde video
- De aanvraag ingesteld `GenerateVTT` markeren als de gebruiker de tekst van een tekst heeft aangevraagd
- Aanroepen `CreateAzureMediaServicesJobToModerateVideo()` voor het uitvoeren van het uploaden en het resultaat ontvangen

## <a name="deep-dive-into-video-moderation"></a>Diepgaand in video toezicht

De methode `CreateAzureMediaServicesJobToModerateVideo()` wordt `VideoModerator.cs`, die het merendeel van de code die met Azure Media Services communiceert bevat. De broncode van de methode wordt weergegeven in het volgende extract.

    public bool CreateAzureMediaServicesJobToModerateVideo(UploadVideoStreamRequest uploadVideoRequest, UploadAssetResult uploadResult)
    {
        asset = CreateAsset(uploadVideoRequest);
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        // Encoding the asset , Moderating the asset, Generating transcript in parallel
        IAsset encodedAsset = null;
        //Creates the job for the tasks.
        IJob job = this._mediaContext.Jobs.Create("AMS Review Job");

        //Adding encoding task to job.
        ConfigureEncodeAssetTask(uploadVideoRequest.EncodingRequest, job);

        ConfigureContentModerationTask(job);

        //adding transcript task to job.
        if (uploadResult.GenerateVTT)
        {
            ConfigureTranscriptTask(job);
        }

        Stopwatch timer = new Stopwatch();
        timer.Start();
        //submit and execute job.
        job.Submit();
        job.GetExecutionProgressTask(new CancellationTokenSource().Token).Wait();
        timer.Stop();
        using (var sw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            sw.WriteLine("AMS Job Elapsed Time: {0}", timer.Elapsed);
        }

        if (job.State == JobState.Error)
        {
            throw new Exception("Video moderation has failed due to AMS Job error.");
        }

        UploadAssetResult result = uploadResult;
        encodedAsset = job.OutputMediaAssets[0];
        result.ModeratedJson = GetCmDetail(job.OutputMediaAssets[1]);
        // Check for valid Moderated JSON
        var jsonModerateObject = JsonConvert.DeserializeObject<VideoModerationResult>(result.ModeratedJson);

        if (jsonModerateObject == null)
        {
            return false;
        }
        if (uploadResult.GenerateVTT)
        {
            GenerateTranscript(job.OutputMediaAssets.Last());
        }

        uploadResult.StreamingUrlDetails = PublishAsset(encodedAsset);
        string downloadUrl = GenerateDownloadUrl(asset, uploadVideoRequest.VideoName);
        uploadResult.StreamingUrlDetails.DownloadUri = downloadUrl;
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        uploadResult.VideoFilePath = uploadVideoRequest.VideoFilePath;
        return true;
    }

Deze code uitvoert de volgende taken:

- Maakt een taak voor AMS voor de verwerking moet worden uitgevoerd
- Taken voor het videobestand codering, het toezicht te houden en genereren van de tekst van een tekst toevoegen
- De taak, het uploaden van het bestand en de verwerking van het begin worden verzonden
- Haalt de resultaten van toezicht, de tekst van de tekst (indien nodig) en andere informatie

## <a name="sample-video-moderation-response"></a>Video toezicht voorbeeldreactie

Het resultaat van de taak video interrupts (Zie [video toezicht Quick Start](video-moderation-api.md) is een JSON-gegevensstructuur met de controle-resultaten. Deze resultaten bevatten een overzicht van de fragmenten (schermafbeeldingen) binnen de video, elk met gebeurtenissen (illustraties) met keyframes die zijn gemarkeerd voor revisie. Elke Sleutelframe wordt berekend door de kans dat deze inhoud voor volwassenen of mooie bevat. Het volgende voorbeeld ziet u een JSON-antwoord:

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

Een schrijffouten van de audio van de video is ook gemaakt wanneer de `GenerateVTT` -vlag is ingesteld.

> [!NOTE]
> De consoletoepassing maakt gebruik van de [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) genereren transcripties van de de geüploade video-nummer. De resultaten zijn opgegeven in WebVTT-indeling. Zie voor meer informatie over deze indeling [Web Video tekstindeling houdt](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>Maken van de controle human in de lus

Het toezichtproces retourneert een lijst met keyframes van de video, samen met de tekst van de audio houdt. De volgende stap is het maken van een beoordeling in het hulpprogramma voor het controleren van inhoud beheerder voor menselijke moderators. Ga terug naar de `ProcessVideo()` methode in `Program.cs`, ziet u de aanroep van de `CreateVideoReviewInContentModerator()` methode. Deze methode is in de `videoReviewApi` klasse, die in `VideoReviewAPI.cs`, en wordt hier weergegeven.

    public async Task<string> CreateVideoReviewInContentModerator(UploadAssetResult uploadAssetResult)
    {
    
        string reviewId = string.Empty;
        List<ProcessedFrameDetails> frameEntityList = framegenerator.CreateVideoFrames(uploadAssetResult);
        string path = uploadAssetResult.GenerateVTT == true ? this._amsConfig.FfmpegFramesOutputPath + Path.GetFileNameWithoutExtension (uploadAssetResult.VideoName) + "_aud_SpReco.vtt" : "";
        TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult();
        
    if (File.Exists(path))
        {
            screenTextResult = await GenerateTextScreenProfanity(reviewId, path, frameEntityList);
            uploadAssetResult.Category1TextScore = screenTextResult.Category1Score;
            uploadAssetResult.Category2TextScore = screenTextResult.Category2Score;
            uploadAssetResult.Category3TextScore = screenTextResult.Category3Score;
            uploadAssetResult.Category1TextTag = screenTextResult.Category1Tag;
            uploadAssetResult.Category2TextTag = screenTextResult.Category2Tag;
            uploadAssetResult.Category3TextTag = screenTextResult.Category3Tag;
        }
        
        var reviewVideoRequestJson = CreateReviewRequestObject(uploadAssetResult, frameEntityList);
        if (string.IsNullOrWhiteSpace(reviewVideoRequestJson))
        {
            throw new Exception("Video review process failed in CreateVideoReviewInContentModerator");
        }
        
        reviewId = JsonConvert.DeserializeObject<List<string>>(ExecuteCreateReviewApi(reviewVideoRequestJson).Result).FirstOrDefault();
        frameEntityList = framegenerator.GenerateFrameImages(frameEntityList, uploadAssetResult, reviewId);
        await CreateAndPublishReviewInContentModerator(uploadAssetResult, frameEntityList, reviewId, path, screenTextResult);
        return reviewId;
    
    }

`CreateVideoReviewInContentModerator()` roept de verschillende methoden om de volgende taken uitvoeren:

> [!NOTE]
> De consoletoepassing maakt gebruik van de [FFmpeg](https://ffmpeg.org/) -bibliotheek voor het genereren van miniaturen. Deze miniaturen (installatiekopieën) komen overeen met de tijdstempels frame in de [video toezicht uitvoer](#sample-video-moderation-response).

|Taak|Methoden|File|
|-|-|-|
|Uitpakken van de sleutel van de video-frames en miniatuurafbeeldingen van deze gemaakt|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Scannen van de tekst van de tekst, indien beschikbaar, volwassenen of mooie audio vinden|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Voorbereiden en verzendt een aanvraag video revisie voor menselijke inspectie|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>De standaardweergave video controleren

Het volgende scherm toont de resultaten van de vorige stappen.

![De standaardweergave video controleren](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>De tekst van generatie

De code in deze zelfstudie is tot nu toe gericht op de visuele inhoud. Revisie van spraak inhoud is een afzonderlijke en optionele proces, zoals is vermeld, gebruikt een verslag gegenereerd op basis van de audio. Is het tijd nu om te kijken hoe tekst transcripties worden gemaakt en gebruikt in het controleproces. De taak voor het genereren van de tekst van de valt op de [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content) service.

De toepassing uitvoert de volgende taken:

|Taak|Methoden|File|
|-|-|-|
|Bepalen of de tekst transcripties zijn gegenereerd|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Zo ja, verzenden van een taak schrijffouten als onderdeel van toezicht|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Een lokale kopie van de tekst van de|`GenerateTranscript()`|`VideoModerator.cs`|
|Vlag frames van de video die ongeschikte audio bevatten|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|De resultaten voor de revisie toevoegen|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Taken configureren

Laten we meteen naar de schrijffouten uptaak. `CreateAzureMediaServicesJobToModerateVideo()` (al beschreven) aanroepen `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

De configuratie voor de taak de tekst van het bestand wordt gelezen `MediaIndexerConfig.json` in de oplossing `Lib` map. AMS-activa zijn gemaakt voor het configuratiebestand en voor de uitvoer van het proces schrijffouten. Wanneer de AMS-taak wordt uitgevoerd, maakt deze taak de tekst van een tekst uit het videobestand-nummer.

> [!NOTE]
> De voorbeeldtoepassing herkent alleen spraak in Amerikaans Engels.

### <a name="transcript-generation"></a>De tekst van generatie

De tekst van de is gepubliceerd als een AMS asset. Voor het doorzoeken van de tekst van de voor ongewenste inhoud, downloadt de toepassing de activa van Azure Media Services. `CreateAzureMediaServicesJobToModerateVideo()` aanroepen `GenerateTranscript()`, weergegeven hier het bestand worden opgehaald.

    public bool GenerateTranscript(IAsset asset)
    {
        try
        {
            var outputFolder = this._amsConfigurations.FfmpegFramesOutputPath;
            IAsset outputAsset = asset;
            IAccessPolicy policy = null;
            ILocator locator = null;
            policy = _mediaContext.AccessPolicies.Create("My 30 days readonly policy", TimeSpan.FromDays(360), AccessPermissions.Read);
            locator = _mediaContext.Locators.CreateLocator(LocatorType.Sas, outputAsset, policy, DateTime.UtcNow.AddMinutes(-5));
            DownloadAssetToLocal(outputAsset, outputFolder);
            locator.Delete();
            return true;
        }
        catch
        {   //TODO:  Logging
            Console.WriteLine("Exception occured while generating index for video.");
            throw;
        }
    }

Bij sommige nodig AMS-installatie wordt het downloaden van de werkelijke is uitgevoerd door het aanroepen van `DownloadAssetToLocal()`, een algemene functie die een asset AMS naar een lokaal bestand kopieert.

## <a name="transcript-moderation"></a>De tekst van toezicht

Met de tekst van de bij de hand, is het gescand en gebruikt in de evaluatie. Maken van de controle is de controlesfeer van `CreateVideoReviewInContentModerator()`, die aanroepen `GenerateTextScreenProfanity()` de taak uit te voeren. Deze methode roept `TextScreen()`, die de meeste functionaliteit bevat. 

`TextScreen()` de volgende taken uitvoert:

- Parseren van de tekst van de tijd tamps en bijschriften
- Elk bijschrift voor tekst toezicht verzenden
- Vlag frames dat mogelijk aanstootgevend spraak inhoud

We gaan worden al deze taken in meer detail:

### <a name="initialize-the-code"></a>Initialisatie van de code

Initialiseer eerst alle variabelen en verzamelingen.

    private async Task<TranscriptScreenTextResult> TextScreen(string filepath, List<ProcessedFrameDetails> frameEntityList)
    {
        List<TranscriptProfanity> profanityList = new List<TranscriptProfanity>();
        string responseContent = string.Empty;
        HttpResponseMessage response;
        bool category1Tag = false;
        bool category2Tag = false;
        bool category3Tag = false;
        double category1Score = 0;
        double category2Score = 0;
        double category3Score = 0;
        List<string> vttLines = File.ReadAllLines(filepath).Where(line => !line.Contains("NOTE Confidence:") && line.Length > 0).ToList();
        StringBuilder sb = new StringBuilder();
        List<CaptionScreentextResult> csrList = new List<CaptionScreentextResult>();
        CaptionScreentextResult captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };

        // Code from the next sections in the tutorial
    

### <a name="parse-the-transcript-for-captions"></a>De tekst van de voor bijschriften parseren

Vervolgens wordt de opgemaakte tekst van de VTT voor bijschriften en tijdstempels parseren. Deze bijschriften worden weergegeven op het tabblad tekst van op het scherm video revisie van het hulpprogramma voor beoordeling. De tijdstempels worden gebruikt om te synchroniseren met de bijbehorende video frames bijschrift.

        // Code from the previous section(s) in the tutorial

        //
        // Parse the transcript
        //
        foreach (var line in vttLines.Skip(1))
        {
                if (line.Contains("-->"))
                {
                    if (sb.Length > 0)
                    {
                        captionScreentextResult.Captions.Add(sb.ToString());
                        sb.Clear();
                    }
                    if (captionScreentextResult.Captions.Count > 0)
                    {
                        csrList.Add(captionScreentextResult);
                        captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };
                    }
                    string[] times = line.Split(new string[] { "-->" }, StringSplitOptions.RemoveEmptyEntries);
                    string startTimeString = times[0].Trim();
                    string endTimeString = times[1].Trim();
                    int startTime = (int)TimeSpan.ParseExact(startTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    int endTime = (int)TimeSpan.ParseExact(endTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    captionScreentextResult.StartTime = startTime;
                    captionScreentextResult.EndTime = endTime;
                }
                else
                {
                    sb.Append(line);
                }
                if (sb.Length + line.Length > 1024)
                {
                    captionScreentextResult.Captions.Add(sb.ToString());
                    sb.Clear();
                }
            }
            if (sb.Length > 0)
            {
                captionScreentextResult.Captions.Add(sb.ToString());
            }
            if (captionScreentextResult.Captions.Count > 0)
            {
                csrList.Add(captionScreentextResult);
            }

            // Code from the following section in the quickstart

### <a name="moderate-captions-with-the-text-moderation-service"></a>Gemiddeld bijschriften worden gemaakt met de tekst toezicht-service

Vervolgens wordt de geparseerde bijschriften door inhoud beheerder tekst API scannen.

> [!NOTE]
> De sleutel van uw inhoud beheerder-service heeft een aantal aanvragen per frequentielimiet van tweede (RPS). Als u de limiet overschrijdt, genereert de SDK een uitzondering met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

    //
    // Moderate the captions or cues
    //
    int waitTime = 1000;
    foreach (var csr in csrList)
    {
                bool captionAdultTextTag = false;
                bool captionRacyTextTag = false;
                bool captionOffensiveTextTag = false;
                bool retry = true;

                foreach (var caption in csr.Captions)
                {
                    while (retry)
                    {
                        try
                        {
                            System.Threading.Thread.Sleep(waitTime);
                            var lang = await CMClient.TextModeration.DetectLanguageAsync("text/plain", caption);
                            var oRes = await CMClient.TextModeration.ScreenTextWithHttpMessagesAsync(lang.DetectedLanguageProperty, "text/plain", caption, null, null, null, true);
                            response = oRes.Response;
                            responseContent = await response.Content.ReadAsStringAsync();
                            retry = false;
                        }
                        catch (Exception e)
                        {
                            if (e.Message.Contains("429"))
                            {
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                                waitTime = (int)(waitTime * 1.5);
                                Console.WriteLine($"wait time: {waitTime}");
                            }
                            else
                            {
                                retry = false;
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                            }
                        }
                    }
                    var jsonTextScreen = JsonConvert.DeserializeObject<TextScreen>(responseContent);
                    if (jsonTextScreen != null)
                    {
                        TranscriptProfanity transcriptProfanity = new TranscriptProfanity();
                        transcriptProfanity.TimeStamp = "";
                        List<Terms> transcriptTerm = new List<Terms>();
                        if (jsonTextScreen.Terms != null)
                        {
                            foreach (var term in jsonTextScreen.Terms)
                            {
                                var profanityobject = new Terms
                                {
                                    Term = term.Term,
                                    Index = term.Index
                                };
                                transcriptTerm.Add(profanityobject);
                            }
                            transcriptProfanity.Terms = transcriptTerm;
                            profanityList.Add(transcriptProfanity);
                        }
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) captionAdultTextTag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) captionRacyTextTag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) captionOffensiveTextTag = true;
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) category1Tag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) category2Tag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) category3Tag = true;
                        category1Score = jsonTextScreen.Classification.Category1.Score > category1Score ? jsonTextScreen.Classification.Category1.Score : category1Score;
                        category2Score = jsonTextScreen.Classification.Category2.Score > category2Score ? jsonTextScreen.Classification.Category2.Score : category2Score;
                        category3Score = jsonTextScreen.Classification.Category3.Score > category3Score ? jsonTextScreen.Classification.Category3.Score : category3Score;
                    }
                    foreach (var frame in frameEntityList.Where(x => x.TimeStamp >= csr.StartTime && x.TimeStamp <= csr.EndTime))
                    {
                        frame.IsAdultTextContent = captionAdultTextTag;
                        frame.IsRacyTextContent = captionRacyTextTag;
                        frame.IsOffensiveTextContent = captionOffensiveTextTag;
                    }
                }
            }
            TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult()
            {
                TranscriptProfanity = profanityList,
                Category1Tag = category1Tag,
                Category2Tag = category2Tag,
                Category3Tag = category3Tag,
                Category1Score = category1Score,
                Category2Score = category2Score,
                Category3Score = category3Score
            };
            return screenTextResult;
    }

### <a name="breaking-down-the-text-moderation-step"></a>De tekst toezicht stap splitsen

`TextScreen()` is een aanzienlijke methode, dus we opsplitsen.

1. De methode leest eerst het bestand verslag per regel. Worden genegeerd lege regels en regels met een `NOTE` met een score vertrouwen. Pakt deze de tijdstempels en tekstitems uit de *hints* in het bestand. Een hint tekst vertegenwoordigt uit de audio bijhouden en begin- en eindtijden bevat. Een hint begint met de tijdlijn van stempel met de tekenreeks `-->`. Deze wordt gevolgd door een of meer regels tekst.

1. Exemplaren van `CaptionScreentextResult` (gedefinieerd in `TranscriptProfanity.cs`) worden gebruikt voor de informatie die wordt geparseerd uit elke hint.  Wanneer een nieuwe stempel tijdlijn wordt gedetecteerd of een tekst een maximale lengte van 1024 tekens is bereikt, een nieuwe `CaptionScreentextResult` wordt toegevoegd aan de `csrList`. 

1. De methode dient naast elke hint voor de tekst toezicht-API. Roept deze beide `ContentModeratorClient.TextModeration.DetectLanguageAsync()` en `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, die zijn gedefinieerd de `Microsoft.Azure.CognitiveServices.ContentModerator` assembly. De methode om te voorkomen wordt beperkt in de frequentie, wordt onderbroken voor een tweede vóór het verzenden van elke hint.

1. Na het ontvangen van resultaten van de tekst toezicht-service, analyseert de methode ze om te zien of ze voldoen aan de drempelwaarden voor vertrouwen. Deze waarden worden ingesteld `App.config` als `OffensiveTextThreshold`, `RacyTextThreshold`, en `AdultTextThreshold`. Ten slotte worden de aanstootgevende voorwaarden zelf ook opgeslagen. Alle frames binnen de hint tijdsbereik zijn gemarkeerd als waarin aanstootgevende, mooie en/of volwassenen tekst.

1. `TextScreen()` retourneert een `TranscriptScreenTextResult` -instantie die het resultaat van de tekst-controle van de video in zijn geheel bevat. Dit object bevat vlaggen en scores voor de verschillende typen aanstootgevend, samen met een lijst met alle ongewenste termen. De aanroeper `CreateVideoReviewInContentModerator()`, aanroepen `UploadScreenTextResult()` deze gegevens koppelen aan de evaluatie dus beschikbaar voor menselijke revisoren is.
 
## <a name="video-review-transcript-view"></a>Video Bekijk de tekst weergeven

Het volgende scherm wordt het resultaat van de tekst van de stappen voor het genereren en toezicht.

![Video toezicht tekst weergeven](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Programma-uitvoer

De volgende opdrachtregel uitvoer van het programma voor de verschillende taken weergegeven zodra ze gereed zijn. Het resultaat interrupts (in JSON-indeling) en de tekst van de spraak zijn ook beschikbaar in dezelfde directory als de oorspronkelijke videobestanden.

    Microsoft.ContentModerator.AMSComponentClient
    Enter the fully qualified local path for Uploading the video :
    "Your File Name.MP4"
    Generate Video Transcript? [y/n] : y
    
    Video compression process started...
    Video compression process completed...
    
    Video moderation process started...
    Video moderation process completed...
    
    Video review process started...
    Video Frames Creation inprogress...
    Frames(83) created successfully.
    Review Created Successfully and the review Id 201801va8ec2108d6e043229ba7a9e6373edec5
    Video review successfully completed...
    
    Total Elapsed Time: 00:05:56.8420355


## <a name="next-steps"></a>Volgende stappen

[Downloaden van de Visual Studio-oplossing](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) voorbeeld van bestanden en de vereiste bibliotheken voor deze zelfstudie en op uw integratie aan de slag.
