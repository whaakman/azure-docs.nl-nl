---
title: Azure Media Video Thumbnails gebruiken voor het maken van een Video-overzicht | Microsoft Docs
description: Samenvatting van de video kunt u bij het maken van samenvattingen van lange video's door het automatisch selecteren interessante codefragmenten in de bronvideo. Dit is handig als u geven een kort overzicht van wat wilt u kunt verwachten in een lange video.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: milanga;juliako;
ms.openlocfilehash: 0fcacf68f4b41ed8945a6a40d7da125aef499947
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313257"
---
# <a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Azure Media Video Thumbnails gebruiken voor het maken van een Video-overzicht  
## <a name="overview"></a>Overzicht
De **Azure Media Video Thumbnails** Mediaprocessor (MP) kunt u een samenvatting van een video die nuttig is voor klanten die willen bekijken van een samenvatting van een lange video maken. Bijvoorbeeld, klanten verstandig om te zien van een korte 'Samenvatting video' wanneer ze de muisaanwijzer op een miniatuur. Door het wijzigen van de parameters van **Azure Media Video Thumbnails** via vooraf gedefinieerde een configuratie-instellingen u kunt de MP krachtige schermopname detectie en samenvoeging technologie gebruiken voor het genereren van een beschrijvend subclip algoritmisch.  

De **Azure Media Video miniatuur** MP is momenteel in Preview.

Dit artikel geeft meer informatie over **Azure Media Video miniatuur** en laat zien hoe u met Media Services SDK voor .NET gebruiken.

## <a name="limitations"></a>Beperkingen

In sommige gevallen, als uw video niet uit verschillende schermen bestaat, wordt de uitvoer alleen worden een één beeld.

## <a name="video-summary-example"></a>Voorbeeld van video-overzicht
Hier volgen enkele voorbeelden van wat de Mediaprocessor die Azure Media Video Thumbnails kunt doen:

### <a name="original-video"></a>Oorspronkelijke video
[Oorspronkelijke video](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

### <a name="video-thumbnail-result"></a>Video miniaturen resultaat
[Video miniaturen resultaat](https://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="task-configuration-preset"></a>Taakconfiguratie (standaardoptie)
Bij het maken van een video miniaturen taak met **Azure Media Video Thumbnails**, moet u een configuratie-definitie opgeven. Het bovenstaande voorbeeld van de miniatuur is gemaakt met de volgende eenvoudige JSON-configuratie:

```json
    {
        "version":"1.0"
    }
```

Op dit moment kunt u de volgende parameters:

| Param | Description |
| --- | --- |
| outputAudio |Hiermee geeft u op of de resulterende video audio bevat. <br/>Toegestane waarden zijn: Waar of ONWAAR. De standaardwaarde is True. |
| fadeInFadeOut |Hiermee geeft u op of vervagen overgangen tussen de afzonderlijke beweging miniaturen worden gebruikt.  <br/>Toegestane waarden zijn: Waar of ONWAAR.  De standaardwaarde is True. |
| maxMotionThumbnailDurationInSecs |Geheel getal dat aangeeft hoe lang de hele resulterende video moet zijn.  Standaard is afhankelijk van op de duur van de oorspronkelijke video. |

De volgende tabel beschrijft de standaardduur wanneer **maxMotionThumbnailInSecs** wordt niet gebruikt.

|  |  |  |
| --- | --- | --- |
| Videoduur |d < 3 min |3 minuten < d < 15 minuten |
| Duur van de miniatuur |15 sec (2-3 scenes) |30 seconden (3-5-scènes) |

Beschikbare parameters Hiermee stelt u de volgende JSON.

```json
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }
```

## <a name="net-sample-code"></a>.NET-voorbeeldcode

De volgende programma toont hoe u:

1. Maak een asset en upload een mediabestand naar de asset.
2. Maakt een taak met een video miniaturen taak op basis van een configuratiebestand met de volgende json-definitie: 
    
    ```json
            {                
                "version": "1.0",
                "options": {
                    "outputAudio": "true",
                    "maxMotionThumbnailDurationInSecs": "30",
                    "fadeInFadeOut": "false"
                }
            }
    ```

3. Downloadt de uitvoerbestanden. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Voorbeeld

```csharp
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = 
                    new AzureAdTokenCredentials(_AADTenantDomain,
                        new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                        AzureEnvironments.AzureCloudEnvironment);

                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);


                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }

            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");

                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

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
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

        }
    }
```

### <a name="video-thumbnail-output"></a>Miniaturen video-uitvoer
[Miniaturen video-uitvoer](https://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

