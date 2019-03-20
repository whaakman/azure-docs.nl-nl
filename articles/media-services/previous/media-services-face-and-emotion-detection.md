---
title: Detecteer gezichten en emoties met Azure Media Analytics | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u voor het detecteren van gezichten en emoties met Azure Media Analytics.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: milanga;juliako;
ms.openlocfilehash: 46e60583da79006c133c8d9fac63e27f28bd699f
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188183"
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Detecteer gezichten en emoties met Azure Media Analytics
## <a name="overview"></a>Overzicht
De **Azure Media Face Detector** Mediaprocessor (MP) kunt u tellen, bijhouden van bewegingen en zelfs doelgroep deelname en reactie via gezichtsuitdrukkingen te meten. Deze service bevat twee functies: 

* **Gezichtsherkenning**
  
    Gezichtsdetectie zoekt en menselijke gezichten op een video wordt bijgehouden. Meerdere gezichten kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de metagegevens van de tijd en locatie geretourneerd in een JSON-bestand. Tijdens het bijhouden, wordt geprobeerd een consistente ID geven tot de dezelfde face terwijl de persoon is verplaatsen op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het kader laat.
  
  > [!NOTE]
  > Deze service biedt gezichtsherkenning niet uitvoeren. Een persoon die het frame blijft of raakt ondervindt hinder van obstakels voor te lang krijgt een nieuwe ID wanneer ze terugkeren.
  > 
  > 
* **Detectie van emoties**
  
    Gevoelsdetectie is een optioneel onderdeel van de Mediaprocessor die Face Detection die analyse op meerdere emotionele kenmerken geactiveerd vanuit de gezichten gedetecteerd wordt, met inbegrip van blijdschap, verdriet, angst, boosheid, en meer. 

De **Azure Media Face Detector** MP is momenteel in Preview.

Dit artikel geeft meer informatie over **Azure Media Face Detector** en laat zien hoe u met Media Services SDK voor .NET gebruiken.

## <a name="face-detector-input-files"></a>Face Detector invoerbestanden
Videobestanden. Op dit moment worden de volgende indelingen ondersteund: MP4, MOV en WMV.

## <a name="face-detector-output-files"></a>Face Detector uitvoerbestanden
De face-API voor detectie en tracking biedt hoge precisie locatie gezichtsdetectie en bijhouden die maximaal 64 gezichten van mensen in een video kunt detecteren. Voorzijde gezichten bieden de beste resultaten, terwijl de zijkanten en kleine gezichten (kleiner dan of gelijk aan 24 x 24 pixels) mogelijk niet nauwkeurig.

De gezichten gedetecteerd en bijgehouden worden geretourneerd met de coördinaten (links, boven, breedte en hoogte) die wijzen op de locatie van gezichten wordt uitgevoerd in de afbeelding in pixels, evenals een face id-nummer die wijzen op het volgen van deze persoon. Face id-nummers zijn gevoelig zijn voor het opnieuw instellen onder omstandigheden wanneer de voorzijde gezicht is zoekgeraakt of overlappende in het kader, wat resulteert in bepaalde personen meerdere id's ophalen van toegewezen.

## <a id="output_elements"></a>Elementen van het JSON-bestand voor uitvoer

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

Face Detector maakt gebruik van technieken van fragmentatie (waar de metagegevens kan worden opgedeeld in segmenten op basis van tijd en u kunt downloaden alleen wat u nodig hebt) en segmentering (waar de gebeurtenissen worden opgesplitst in het geval ze te groot krijgen). Met enkele eenvoudige berekeningen kunt u de gegevens transformeren. Bijvoorbeeld, als een gebeurtenis wordt gestart om 6300 (maten), met een tijdschaal van 2997 (maten/sec) en framesnelheid van 29,97 (frames per seconde), klikt u vervolgens:

* Start/tijdschaal = 2,1 seconde
* X Framerate seconden = 63 frames

## <a name="face-detection-input-and-output-example"></a>Face detection-invoer en uitvoer voorbeeld
### <a name="input-video"></a>Invoervideo
[Invoervideo](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Taakconfiguratie (standaardoptie)
Bij het maken van een taak met **Azure Media Face Detector**, moet u een configuratie-definitie opgeven. De volgende configuratie-voorinstelling is alleen voor gezichtsdetectie.

```json
    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }
```

#### <a name="attribute-descriptions"></a>Beschrijvingen van kenmerken
| De naam van kenmerk | Description |
| --- | --- |
| Modus |Snel verwerken Fast - snelheid, maar minder nauwkeurig (standaard).|

### <a name="json-output"></a>JSON-uitvoer
Het volgende voorbeeld van JSON-uitvoer is afgebroken.

```json
    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],
```


## <a name="emotion-detection-input-and-output-example"></a>Detectie van emoties in invoer en uitvoer voorbeeld
### <a name="input-video"></a>Invoervideo
[Invoervideo](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Taakconfiguratie (standaardoptie)
Bij het maken van een taak met **Azure Media Face Detector**, moet u een configuratie-definitie opgeven. De volgende configuratie-definitie Hiermee geeft u als u wilt maken op basis van de detectie van emoties in JSON.

```json
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }
```


#### <a name="attribute-descriptions"></a>Beschrijvingen van kenmerken
| De naam van kenmerk | Description |
| --- | --- |
| Modus |Gezichten: Alleen gezichtsdetectie.<br/>PerFaceEmotion: Emoties afzonderlijk voor elke gezichtsdetectie retourneren.<br/>AggregateEmotion: Emotion-gemiddelde waarden voor alle gezichten in frame retourneren. |
| AggregateEmotionWindowMs |Gebruik als AggregateEmotion modus hebt geselecteerd. Hiermee geeft u de lengte van de video die worden gebruikt voor het produceren van elke geaggregeerde resultaat in milliseconden. |
| AggregateEmotionIntervalMs |Gebruik als AggregateEmotion modus hebt geselecteerd. Hiermee geeft u aan met welke frequentie voor het produceren van resultaten samenvoegen. |

#### <a name="aggregate-defaults"></a>Cumulatieve standaardinstellingen
Hieronder worden aanbevolen waarden voor de cumulatieve venster en interval-instellingen. AggregateEmotionWindowMs moet langer zijn dan AggregateEmotionIntervalMs.

|| Standaardwaarden (s) | Max(s) | Minuut |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>JSON-uitvoer
JSON-uitvoer voor statistische emoties (afgekapt):

```json
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
```

## <a name="limitations"></a>Beperkingen
* De ondersteunde indelingen voor invoer video opnemen MP4 MOV en WMV.
* Het bereik van de grootte detecteerbare gezicht is 24 x 24 naar 2048 x 2048 pixels. De gezichten buiten dit bereik niet gedetecteerd.
* Voor elke video is het maximum aantal gezichten geretourneerd 64.
* Sommige gezichten worden mogelijk niet gedetecteerd door technische uitdagingen; bijvoorbeeld, zeer grote face hoeken (hoofd-houding) en grote bedekking. Gezichten voorzijde en in de buurt binnen handbereik hebben de beste resultaten.

## <a name="net-sample-code"></a>.NET-voorbeeldcode

De volgende programma toont hoe u:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak maken met een face detection-taak die op basis van een configuratiebestand met de volgende json-definitie: 

    ```json
            {
                "version": "1.0"
            }
    ```
3. De uitvoer-JSON-bestanden downloaden. 

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

namespace FaceDetection
{
    class Program
    {
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

            // Run the FaceDetection job.
            var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\FaceDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
        }

        static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Face Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Detection Job");

            // Get a reference to Azure Media Face Detector.
            string MediaProcessorName = "Azure Media Face Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Detection Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](https://amslabs.azurewebsites.net/demos/Analytics.html)

