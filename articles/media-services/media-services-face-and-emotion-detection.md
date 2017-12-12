---
title: Detecteren gezichts- en Emotion met Azure Media Analytics | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe vlakken en emoties met Azure Media Analytics te detecteren.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 5741a484dcda05e3143b5f896ddee2e8591dabee
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Face en Emotion met Azure Media Analytics detecteren
## <a name="overview"></a>Overzicht
De **Azure Media Face detectie** Mediaprocessor (MP) kunt u tellen en zelfs te meten wat doelgroep deelname en reactie via bedacht bewegingen bijhouden. Deze service bevat twee functies: 

* **Face-detectie**
  
    Face-detectie zoekt en houdt menselijke vlakken binnen een video. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de metagegevens van de tijd en de locatie in een JSON-bestand geretourneerd. Tijdens de bijhouden, wordt geprobeerd een consistente ID geven tot de dezelfde face terwijl de persoon is navigeren op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.
  
  > [!NOTE]
  > Deze service voert geen gezichtsherkenning. Een persoon die het frame verlaat of ondervindt voor wordt hinder van obstakels te lang krijgt een nieuwe ID wanneer ze terugkeren.
  > 
  > 
* **Emotiedetectie**
  
    Emotiedetectie is een optioneel onderdeel van de Processor Face Detection Media die analyse op meerdere emotionele kenmerken van de vlakken gedetecteerd retourneert, met inbegrip van gelukkig, sadness bang, volgt uitzien en meer. 

De **Azure Media Face detectie** MP is momenteel in Preview.

In dit artikel geeft informatie over **Azure Media Face detectie** en laat zien hoe u deze gebruiken met Media Services SDK voor .NET.

## <a name="face-detector-input-files"></a>Detectie-invoerbestanden geconfronteerd
Videobestanden. Op dit moment wordt de volgende indelingen worden ondersteund: MP4 MOV en WMV.

## <a name="face-detector-output-files"></a>Detectie uitvoerbestanden geconfronteerd
De face-API voor detectie en bijhouden biedt hoge precisie face locatie detectie en bijhouden die maximaal 64 menselijke vlakken in een video kan detecteren. Voorzijde vlakken bieden de beste resultaten, terwijl side vlakken en in kleine vlakken (kleiner dan of gelijk aan 24 x 24 pixels) mogelijk niet nauwkeurig.

De vlakken gedetecteerde en bijgehouden met coördinaten (links, top, breedte en hoogte) worden geretourneerd die de locatie van vlakken in de afbeelding in pixels, evenals een face-id die aangeeft dat afzonderlijke volgen. Face-id-nummers zijn kwetsbaar voor opnieuw omstandigheden worden ingesteld als de voorzijde face verloren is gegaan of elkaar overlappen in het kader waardoor bepaalde personen ophalen van meerdere id's toegewezen.

## <a id="output_elements"></a>Elementen van de JSON-bestand voor uitvoer

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

Face-detectie maakt gebruik van technieken van fragmentatie (waar de metagegevens kan worden opgedeeld in segmenten op basis van tijd en u kunt downloaden wat u nodig hebt) en segmentering (waar de gebeurtenissen worden opgedeeld geval ze te groot krijgen). Enkele eenvoudige berekeningen kunt u de gegevens transformeren. Bijvoorbeeld, als een gebeurtenis wordt gestart om 6300 (maten) met een tijdschaal van 2997 (ticks per seconde) en framesnelheid van vervolgens 29,97 (frames per seconde):

* Start/tijdschaal = 2.1 seconden
* Seconden x Framerate 63 frames =

## <a name="face-detection-input-and-output-example"></a>Geconfronteerd detectie invoer en uitvoer voorbeeld
### <a name="input-video"></a>Invoervideo
[Invoervideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Taken configureren (standaardoptie)
Bij het maken van een taak met **Azure Media Face detectie**, moet u een configuratie-definitie opgeven. De volgende configuratie-definitie is slechts voor face detection.

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a>Beschrijvingen van kenmerken
| Naam van kenmerk | Beschrijving |
| --- | --- |
| Modus |Snel - verwerking snel van de snelheid, maar minder nauwkeurig (standaard).|

### <a name="json-output"></a>JSON-uitvoer
Het volgende voorbeeld van JSON-uitvoer is afgebroken.

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

        . . . 

## <a name="emotion-detection-input-and-output-example"></a>Emotiedetectie invoer en uitvoer voorbeeld
### <a name="input-video"></a>Invoervideo
[Invoervideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Taken configureren (standaardoptie)
Bij het maken van een taak met **Azure Media Face detectie**, moet u een configuratie-definitie opgeven. De volgende configuratie voorinstelling geeft als u wilt maken op basis van de emotiedetectie JSON.

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a>Beschrijvingen van kenmerken
| Naam van kenmerk | Beschrijving |
| --- | --- |
| Modus |Vlakken: Komen alleen voor detectie.<br/>PerFaceEmotion: Retourneren emotion onafhankelijk voor elke face-detectie.<br/>AggregateEmotion: Return gemiddelde emotion waarden voor alle vlakken in frame. |
| AggregateEmotionWindowMs |Gebruik deze optie als AggregateEmotion modus geselecteerd. Hiermee geeft u de lengte van video gebruikt voor het produceren van elk cumulatieve resultaat, in milliseconden. |
| AggregateEmotionIntervalMs |Gebruik deze optie als AggregateEmotion modus geselecteerd. Hiermee geeft u aan met welke frequentie voor het produceren van statistische resultaten. |

#### <a name="aggregate-defaults"></a>Cumulatieve standaardinstellingen
Hieronder worden aanbevolen waarden voor de cumulatieve venster en interval-instellingen. AggregateEmotionWindowMs mag niet langer zijn dan AggregateEmotionIntervalMs.

|| Standaardinstellingen (s) | Max(s) | Min(s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>JSON-uitvoer
JSON-uitvoer voor cumulatieve emotion (afgekapt):

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

## <a name="limitations"></a>Beperkingen
* De ondersteunde video-invoerindelingen zijn MP4 MOV en WMV.
* Het bereik van de grootte waarneembaar face is 24 x 24 naar 2048 x 2048 pixels. De vlakken buiten dit bereik wordt niet gedetecteerd.
* Voor elke video is het maximum aantal vlakken geretourneerd 64.
* Sommige vlakken kunnen niet worden herkend vanwege technische moeilijkheden; bijvoorbeeld, zeer grote face hoeken (head-ven) en grote Occlusie. Voorzijde en in de buurt binnen handbereik vlakken hebben de beste resultaten.

## <a name="net-sample-code"></a>Voorbeeldcode voor .NET

De volgende programma toont hoe:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak met een face detection-taak op basis van een configuratiebestand met de volgende json-definitie maken: 
   
        {
            "version": "1.0"
        }
3. De uitvoer JSON-bestanden downloaden. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Voorbeeld

```
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
            task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://amslabs.azurewebsites.net/demos/Analytics.html)

