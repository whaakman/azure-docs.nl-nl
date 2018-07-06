---
title: Azure Content Moderator - matig video's en transcripties in .NET | Microsoft Docs
description: Het gebruik van Content Moderator tot gemiddelde video's en transcripties in .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: 0f851c030a05880d79a998ed4b4a941082c057b9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865468"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Video en transcriptie toezicht-zelfstudie

Content Moderator-video-API's kunnen u video's modereren en video beoordelingen maken in het hulpprogramma voor menselijke beoordeling. 

Deze gedetailleerde zelfstudie biedt om te begrijpen hoe u een volledige video en transcriptie toezicht om oplossing te bouwen met geautomatiseerd beheer en het maken van human-in-the-loop revisie.

Download de [C#-consoletoepassing](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) voor deze zelfstudie. De consoletoepassing maakt gebruik van de SDK en de bijbehorende pakketten naar de volgende taken uitvoeren:

- De invoer video(s) voor een snellere verwerking comprimeren
- Gemiddelde van de video om te slagen en frames met inzichten
- De tijdstempels frame gebruiken voor het maken van miniatuurweergaven (installatiekopieën)
- Tijdstempels en miniaturen te maken van de video beoordelingen worden ingediend
- Converteer de video spraak naar tekst (transcript) met de Media Indexer-API
- Gemiddelde van het transcript met de service van de toezicht op tekst
- Toevoegen van de gecontroleerde transcriptie naar de video-overzicht

## <a name="sample-program-outputs"></a>Voorbeeld van programma-uitvoer

Voordat u doorgaat, gaan we de uitvoer van follwing voorbeeld bekijken van het programma:

- [Console-uitvoer](#program-output)
- [Video controleren](#video-review-default-view)
- [Transcript weergeven](#video-review-transcript-view)

## <a name="prerequisites"></a>Vereisten

1. Zich aanmelden voor de [Content Moderator-controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/) website en [maken van aangepaste labels](Review-Tool-User-Guide/tags.md) waarmee de C#-consoletoepassing uit in de code worden toegewezen. Het volgende scherm ziet u de aangepaste labels.

  ![Videotoezicht aangepaste labels](images/video-tutorial-custom-tags.png)

1. Als u wilt de voorbeeldtoepassing uitvoert, moet u een Azure-account en een Azure Media Services-account. Daarnaast moet u toegang tot de Content Moderator private preview. Tot slot moet u referenties voor Azure Active Directory-verificatie. Zie voor meer informatie over het verkrijgen van deze informatie [de toezicht-API voor Video-snelstartgids](video-moderation-api.md).

1. Bewerk het bestand `App.config` en voeg de naam van de Active Directory-tenant, de service-eindpunten en abonnementssleutels aangegeven door `#####`. U moet de volgende informatie:

|Sleutel|Beschrijving|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Eindpunt voor de API van Azure Media Services (AMS)|
|`ClientSecret`|Abonnementssleutel voor Azure Media Services|
|`ClientId`|Client-ID voor Azure mediaservices|
|`AzureAdTenantName`|Active Directory-tenantnaam voor uw organisatie|
|`ContentModeratorReviewApiSubscriptionKey`|Abonnementssleutel voor de Content Moderator API bekijken|
|`ContentModeratorApiEndpoint`|Eindpunt voor de Content Moderator-API|
|`ContentModeratorTeamId`|Content moderator team-ID|

## <a name="getting-started"></a>Aan de slag

De klasse `Program` in `Program.cs` is de belangrijkste beginpunt voor het beheer van video-toepassing.

### <a name="methods-of-class-program"></a>Methoden van de klasse Program

|Methode|Beschrijving|
|-|-|
|`Main`|Vanaf de opdrachtregel parseert, verzamelt de invoer van de gebruiker en begint met de verwerking.|
|`ProcessVideo`|Comprimeert, uploadt, matigt en video beoordelingen maakt.|
|`CreateVideoStreamingRequest`|Hiermee maakt u een stream een video uploaden|
|`GetUserInputs`|Verzamelt de invoer van de gebruiker; Als er geen opdrachtregelopties aanwezig zijn gebruikt|
|`Initialize`|Initialiseert de objecten die nodig zijn voor het beheer van proces|

### <a name="the-main-method"></a>De Main-methode

`Main()` is waarbij de uitvoering wordt gestart, dus dit is de plaats om te beginnen met informatie over het beheer van video-proces.

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

- Het pad naar een map met MPEG-4 video bestanden kunnen worden verzonden voor beheer. Alle `*.mp4` bestanden in deze map en submappen zijn ingediend voor beheer.
- (Optioneel) een Booleaanse waarde (true/false) vlag die aangeeft of tekst Transcripten ten behoeve van toezicht audio moeten worden gegenereerd.

Als er geen opdrachtregelargumenten aanwezig zijn, `Main()` aanroepen `GetUserInputs()`. Deze methode wordt de gebruiker voert u het pad naar een enkel bestand van de video en om op te geven of een tekstversie moet worden gegenereerd.

> [!NOTE]
> De consoletoepassing maakt gebruik van de [API van Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) Transcripten genereren uit van de geüploade video audiotrack. De resultaten zijn opgegeven in WebVTT-indeling. Zie voor meer informatie over deze indeling [Web Video tekstindeling nummers](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Initialiseren en ProcessVideo methoden

Ongeacht of de opties van het programma afkomstig zijn uit vanaf de opdrachtregel of invoer van de interactieve gebruiker, `Main()` volgende aanroepen `Initialize()` te maken van de volgende gevallen:

|Klasse|Beschrijving|
|-|-|
|`AMSComponent`|Comprimeert videobestanden voordat u ze verzendt voor beheer.|
|`AMSconfigurations`|Interface de configuratiegegevens van de toepassing, gevonden in `App.config`.|
|`VideoModerator`| Uploaden, codering, versleuteling en beheer met behulp van AMS SDK|
|`VideoReviewApi`|Video beoordelingen in de Content Moderator-service beheert|

Deze klassen (aside van `AMSConfigurations`, die is vrij eenvoudig) worden behandeld in meer detail in toekomstige gedeelten van deze zelfstudie.

Ten slotte de videobestanden verwerkte één bewerking tegelijk worden door het aanroepen van `ProcessVideo()` voor elk.

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


De `ProcessVideo()` methode is redelijk eenvoudig. De volgende bewerkingen worden uitgevoerd in de volgorde:

- Comprimeert de video
- De video uploadt naar een Azure Media Services-asset
- Hiermee maakt u een AMS-taak voor het gemiddelde van de video
- Hiermee maakt u een video beoordeling in Content Moderator

Overweeg de volgende secties in meer detail enkele van de afzonderlijke processen aangeroepen door `ProcessVideo()`. 

## <a name="compressing-the-video"></a>Comprimeren van de video

Voor het netwerkverkeer te minimaliseren, de toepassing videobestanden converteert naar H.264 (MPEG-4 AVC)-indeling en deze kan worden geschaald naar een maximale breedte van 640 pixels. De compressiecodec H.264 wordt aanbevolen vanwege de hoge efficiëntie (prijs compressie). De compressie wordt gedaan met behulp van de gratis `ffmpeg` opdrachtregelprogramma, die is opgenomen in de `Lib` map van de Visual Studio-oplossing. De invoerbestanden kunnen worden van een indeling die wordt ondersteund door `ffmpeg`, met inbegrip van de meest gebruikte videobestandsindelingen en codecs voor.

> [!NOTE]
> Wanneer u begint met het programma met behulp van opdrachtregelopties, geeft u een map met de video bestanden kunnen worden verzonden voor beheer. Alle bestanden in deze map met de `.mp4` extensie worden verwerkt. Voor het verwerken van andere bestandsnaamextensies, werken de `Main()` methode in `Program.cs` om op te nemen van de gewenste extensies.

De code die een enkele videobestand comprimeert is de `AmsComponent` in klasse `AMSComponent.cs`. De methode die verantwoordelijk is voor deze functionaliteit is `CompressVideo()`, die hier worden weergegeven.

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
- Controles om ervoor te zorgen de `ffmpeg` binaire aanwezig is
- Naam van het uitvoerbestand bouwt door toe te voegen `_c.mp4` op de naam van het bestand (zoals `Example.mp4`  ->  `E>xample_c.mp4`)
- Maakt een opdrachtregeltekenreeks om uit te voeren van de conversie
- Start een `ffmpeg` verwerken met behulp van de opdrachtregel
- Wacht tot de video om te worden verwerkt

> [!NOTE]
> Als u uw video's al zijn gecomprimeerd met H.264 en de juiste afmetingen hebben, kunt u herschrijven `CompressVideo()` om over te slaan van de compressie.

De methode retourneert de bestandsnaam van het gecomprimeerde uitvoerbestand.

## <a name="uploading-and-moderating-the-video"></a>Uploaden en toezicht houden op de video

De video moet worden opgeslagen in Azure Media Services voordat deze kan worden verwerkt door de Content Moderation-service. De `Program` in klasse `Program.cs` heeft een korte methode `CreateVideoStreamingRequest()` die geeft als resultaat een object waarmee de streaming-aanvraag voor het uploaden van de video.

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

De resulterende `UploadVideoStreamRequest` object wordt gedefinieerd in `UploadVideoStreamRequest.cs` (en de bovenliggende `UploadVideoRequest`in `UploadVideoRequest.cs`). Deze klassen worden niet hier; weergegeven ze korte en dienen alleen voor het opslaan van de gecomprimeerde video gegevens en informatie erover. Een andere klasse voor alleen-gegevens, `UploadAssetResult` (`UploadAssetResult.cs`) wordt gebruikt voor het opslaan van de resultaten van het uploadproces. Nu is het mogelijk om te begrijpen van deze regels in `ProcessVideo()`:

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

Deze regels uitvoeren de volgende taken:

- Maak een `UploadVideoStreamRequest` gecomprimeerde video uploaden
- Instellen van de aanvraag `GenerateVTT` markeren als de gebruiker heeft een tekstversie aangevraagd
- Aanroepen `CreateAzureMediaServicesJobToModerateVideo()` voor het uitvoeren van het uploaden en het resultaat ontvangen

## <a name="deep-dive-into-video-moderation"></a>Nauwkeuriger beheer van video

De methode `CreateAzureMediaServicesJobToModerateVideo()` is in `VideoModerator.cs`, bevat het grootste deel van de code die met Azure Media Services communiceert. De broncode van de methode wordt weergegeven in het volgende uittreksel.

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

Deze code worden de volgende taken uitgevoerd:

- Hiermee maakt u een AMS-taak voor de verwerking moet worden uitgevoerd
- Taken voor het coderen van het videobestand, toezicht houden op deze en genereert een tekstversie toevoegen
- De taak, uploaden van het bestand en de verwerking van het begin worden verzonden
- Haalt de resultaten van het beheer, het transcript tekst (indien nodig) en andere informatie

## <a name="sample-video-moderation-response"></a>Videotoezicht voorbeeldantwoord

Het resultaat van de taak videotoezicht (Zie [videotoezicht snelstartgids](video-moderation-api.md) is een JSON-gegevensstructuur met het beheer van resultaten. Deze resultaten bevatten een overzicht van de fragmenten (foto's) binnen de video, elk met gebeurtenissen (clips) met de belangrijkste frames die zijn gemarkeerd voor controle. Elke sleutel frame wordt berekend door de kans dat deze volwassen of ongepaste inhoud bevat. Het volgende voorbeeld ziet u een JSON-antwoord:

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

Een transcriptie van de audio van de video wordt ook gemaakt wanneer de `GenerateVTT` vlag is ingesteld.

> [!NOTE]
> De consoletoepassing maakt gebruik van de [API van Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) Transcripten genereren uit van de geüploade video audiotrack. De resultaten zijn opgegeven in WebVTT-indeling. Zie voor meer informatie over deze indeling [Web Video tekstindeling nummers](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>Het maken van de beoordeling human-in-the-loop

Het toezichtproces retourneert een lijst van belangrijke frames van de video, samen met een transcript van audio sporen te wissen. De volgende stap is het maken van een beoordeling in het Content Moderator-controlehulpprogramma voor menselijke moderators. Ga terug naar de `ProcessVideo()` methode in `Program.cs`, ziet u de aanroep van de `CreateVideoReviewInContentModerator()` methode. Deze methode is in de `videoReviewApi` klasse, die in `VideoReviewAPI.cs`, en wordt hier weergegeven.

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
> De consoletoepassing maakt gebruik van de [FFmpeg](https://ffmpeg.org/) bibliotheek voor het genereren van miniaturen. Deze miniaturen (installatiekopieën) komen overeen met de tijdstempels frame in de [beheer van video-uitvoer](#sample-video-moderation-response).

|Taak|Methoden|File|
|-|-|-|
|Pak de sleutel frames van de video en maakt u de miniatuurafbeeldingen van deze|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Scan de tekstversie, indien beschikbaar, volwassene of ongepaste audio vinden|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Voorbereiden en verzendt de aanvraag van een video beoordelen voor menselijke inspectie|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>De standaardweergave video controleren

Het volgende scherm toont de resultaten van de vorige stappen.

![De standaardweergave video controleren](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Transcript genereren

De code in deze zelfstudie is tot nu toe gericht op de visuele inhoud. Overzicht van de gesproken inhoud is een afzonderlijke en optionele proces dat zoals gezegd, gebruikt een transcript gegenereerd op basis van de audio. Het is nu tijd om te kijken hoe tekst Transcripten worden gemaakt en gebruikt in het controleproces. De taak voor het genereren van het transcript valt op de [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content) service.

De toepassing uitvoert de volgende taken:

|Taak|Methoden|File|
|-|-|-|
|Bepalen of tekst Transcripten zijn gegenereerd|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Als dit het geval is, dient u een taak transcriptie als onderdeel van beheer|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Een lokale kopie van het transcript|`GenerateTranscript()`|`VideoModerator.cs`|
|Markering waarmee wordt aangegeven frames van de video met onjuiste audio|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|De resultaten voor de revisie toevoegen|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Taakconfiguratie

Laten we meteen in de taak transcriptie verzenden. `CreateAzureMediaServicesJobToModerateVideo()` (al eerder beschreven) aanroepen `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

De configuratie voor de taak transcript wordt gelezen uit het bestand `MediaIndexerConfig.json` in van de oplossing `Lib` map. AMS-assets worden gemaakt voor het configuratiebestand en de uitvoer van het proces transcriptie. Wanneer de AMS-taak wordt uitgevoerd, maakt deze taak een tekstversie van het videobestand audiotrack.

> [!NOTE]
> De voorbeeld-App herkent alleen spraak in Amerikaans Engels.

### <a name="transcript-generation"></a>Transcript genereren

De transcript is gepubliceerd als een AMS-asset. Als u wilt scannen op het transcript voor aanstootgevende inhoud, downloadt de toepassing de activa van Azure Media Services. `CreateAzureMediaServicesJobToModerateVideo()` aanroepen `GenerateTranscript()`, zoals weergegeven hier om op te halen van het bestand.

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

Na de installatie van bepaalde vereiste AMS, het downloaden van de werkelijke is uitgevoerd door het aanroepen van `DownloadAssetToLocal()`, een algemene functie waarmee een AMS-asset worden gekopieerd naar een lokaal bestand.

## <a name="transcript-moderation"></a>Transcript toezicht

Het is met het transcript dicht bij de hand, gescand en gebruikt in de evaluatie. Het maken van de beoordeling is de controlesfeer van `CreateVideoReviewInContentModerator()`, die aanroepen `GenerateTextScreenProfanity()` om het werk te doen. Op zijn beurt deze methode aanroept `TextScreen()`, die de meeste functionaliteit bevat. 

`TextScreen()` de volgende taken worden uitgevoerd:

- Parseren van het transcript voor tijd tamps en bijschriften
- Indienen van elk bijschrift voor het beheer van tekst
- Markering waarmee wordt aangegeven dat mogelijk aanstootgevend spraakinhoud frames

Laten we een toelichting van elk deze taken in meer detail:

### <a name="initialize-the-code"></a>Initialisatie van de code

Eerst, initialiseren alle variabelen en verzamelingen.

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
    

### <a name="parse-the-transcript-for-captions"></a>Parseren van het transcript voor bijschriften

Vervolgens wordt het opgemaakte transcript VTT voor bijschriften en tijdstempels parseren. Deze bijschriften worden weergegeven op het tabblad Transcript op het scherm video revisie van het beoordelingsprogramma. De tijdstempels worden gebruikt voor het synchroniseren van de bijschriften met de bijbehorende videoframes.

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

### <a name="moderate-captions-with-the-text-moderation-service"></a>Gemiddeld bijschriften met de service van de toezicht op tekst

Vervolgens controleren we de bijschriften geparseerde tekst met Content Moderator tekst-API.

> [!NOTE]
> De sleutel van uw Content Moderator-service heeft een aantal aanvragen per limiet voor tweede (RPS). Als u de limiet overschrijdt, wordt in de SDK een uitzondering met een foutcode 429 genereert. 
>
> De sleutel van een gratis laag heeft een limiet van één RPS.

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

### <a name="breaking-down-the-text-moderation-step"></a>De toezicht op tekst stap afbreken

`TextScreen()` is een aanzienlijke methode, laten we deze opsplitsen.

1. De methode leest eerst het bestand transcriptie per regel. Deze worden genegeerd lege regels en regels met een `NOTE` met een betrouwbaarheidsscore. Pakt deze de tijdstempels en de tekstitems uit de *hints* in het bestand. Een hint tekst uit het audiospoor vertegenwoordigt en begin- en eindtijden bevat. Een hint begint met de tijdlijn van de stempel met de tekenreeks `-->`. Deze wordt gevolgd door een of meer regels tekst.

1. Exemplaren van `CaptionScreentextResult` (gedefinieerd in `TranscriptProfanity.cs`) worden gebruikt voor het opslaan van de gegevens geparseerd van elke hint.  Wanneer een nieuwe stempel tijdlijn wordt gedetecteerd of een tekst een maximale lengte van 1024 tekens is bereikt, een nieuwe `CaptionScreentextResult` wordt toegevoegd aan de `csrList`. 

1. De methode verzendt vervolgens elke hint aan de Text-API voor beheer. Wordt de beide `ContentModeratorClient.TextModeration.DetectLanguageAsync()` en `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, die zijn gedefinieerd in de `Microsoft.Azure.CognitiveServices.ContentModerator` assembly. Om te voorkomen wordt beperkt in de snelheid, onderbreekt de methode voor een tweede voordat u verzendt elke hint.

1. Na ontvangst van de resultaten van de service beheer van tekst, analyseert de methode ze om te zien of ze voldoen aan de drempelwaarden voor vertrouwen. Deze waarden zijn gevestigd `App.config` als `OffensiveTextThreshold`, `RacyTextThreshold`, en `AdultTextThreshold`. Ten slotte worden de aanstootgevende voorwaarden zelf ook opgeslagen. Alle frames binnen het tijdsbereik van de hint zijn gemarkeerd als aanstootgevend, ongepaste waarin en/of volwassene tekst.

1. `TextScreen()` retourneert een `TranscriptScreenTextResult` exemplaar met het resultaat van het beheer van tekst van de video als geheel. Dit object bevat vlaggen en beoordeelt voor de verschillende typen aanstootgevende inhoud, samen met een lijst met alle ongewenste termen. De aanroeper `CreateVideoReviewInContentModerator()`, gesprekken `UploadScreenTextResult()` deze informatie koppelen aan de evaluatie, zodat deze beschikbaar om menselijke revisoren is.
 
## <a name="video-review-transcript-view"></a>Video revisie transcript weergeven

Het volgende scherm ziet u het resultaat van het transcript genereren en beheer van stappen.

![Videotoezicht transcript weergeven](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Programma-uitvoer

De volgende opdrachtregel uitvoer van het programma bevat de verschillende taken, zoals ze zijn voltooid. Bovendien, het resultaat beheer (in JSON-indeling) en de transcriptie van spraak beschikbaar in dezelfde map als de oorspronkelijke videobestanden.

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

[Downloaden van de Visual Studio-oplossing](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) en voorbeeld van bestanden en de vereiste bibliotheken voor deze zelfstudie en aan de slag met uw integratie.
