---
title: Detecteren bewegingen met Azure Media Analytics | Microsoft Docs
description: "De processor voor Azure Media beweging detectie media (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: milanga;juliako;
ms.openlocfilehash: dd422308ed728ed4e8bc35daee3bd50f0f02aaac
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a>Bewegingen met Azure Media Analytics detecteren
## <a name="overview"></a>Overzicht
De **Azure Media beweging detectie** Mediaprocessor (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren. Bewegingsdetectie kan worden gebruikt voor statische camerabeelden identificeren secties van de video waar beweging optreedt. Er wordt een JSON-bestand met een metagegevens met tijdstempels en het selectiegebied waarop de gebeurtenis heeft plaatsgevonden gegenereerd.

Deze technologie is gericht op beveiliging video-kanalen, kunnen beweging categoriseren in relevante gebeurtenissen en fout-positieven zoals schaduwen en licht wijzigingen. Hiermee kunt u voor het genereren van beveiligingswaarschuwingen van de camera feeds zonder kunnen momenten van belang extraheren van zeer lange toezicht video's met oneindige irrelevante gebeurtenissen, spam.

De **Azure Media beweging detectie** MP is momenteel in Preview.

In dit artikel geeft informatie over **Azure Media beweging detectie** en laat zien hoe u deze gebruiken met Media Services SDK voor .NET

## <a name="motion-detector-input-files"></a>Beweging detectie invoerbestanden
Videobestanden. Op dit moment wordt de volgende indelingen worden ondersteund: MP4 MOV en WMV.

## <a name="task-configuration-preset"></a>Taken configureren (standaardoptie)
Bij het maken van een taak met **Azure Media beweging detectie**, moet u een configuratie-definitie opgeven. 

### <a name="parameters"></a>Parameters
U kunt de volgende parameters:

| Naam | Opties | Beschrijving | Standaard |
| --- | --- | --- | --- |
| sensitivityLevel |Tekenreeks: 'laag', 'Gemiddeld', 'hoog' |Hiermee stelt u het gevoeligheidsniveau op welke bewegingen wordt gerapporteerd. Pas dit als het aantal fout-positieven wilt aanpassen. |'Gemiddeld' |
| frameSamplingValue |Positief geheel getal |Hiermee stelt u de frequentie aan welk algoritme wordt uitgevoerd. elk frame gelijk aan-1, 2 betekent dat elke 2e frame, enzovoort. |1 |
| detectLightChange |Boolean-waarde: 'true', 'onwaar' |Hiermee wordt ingesteld of lichte wijzigingen in de resultaten worden gemeld |'False' |
| mergeTimeThreshold |Tijd voor xs:: Mm: ss<br/>Voorbeeld: 00:00:03 |Hiermee geeft u de periode tussen beweging gebeurtenissen waarbij 2 gebeurtenissen wordt gecombineerd en gerapporteerd als 1. |00:00:00 |
| detectionZones |Een matrix van zones voor detectie:<br/>-Zone detectie is een matrix met punten 3 of meer<br/>-Punt is een x en y coördinaat tussen 0 en 1. |Hierin wordt beschreven in de lijst met detectie van veelhoekige zones moet worden gebruikt.<br/>Resultaten met de zones worden gerapporteerd als een ID, met de eerste afbeelding wordt 'id': 0 |Enkele zone, dat wordt ingegaan op het gehele frame. |

### <a name="json-example"></a>JSON-voorbeeld
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a>Beweging detectie uitvoerbestanden
Een motion detection-taak retourneren een JSON-bestand in de uitvoerasset waarin de beweging waarschuwingen en hun categorieën, binnen de video worden beschreven. Het bestand bevat informatie over de tijd en de duur van gedetecteerd in de video motion.

De beweging detectie-API biedt indicatoren zodra er objecten in beweging in een vaste achtergrond video (bijvoorbeeld een toezicht video zijn). De detectie van de beweging wordt te verminderen gegeven, zoals licht en wijzigingen van de schaduw getraind. Huidige beperkingen van de algoritmen zijn nacht visie video's, semi-transparante objecten en kleine objecten.

### <a id="output_elements"></a>Elementen van de JSON-bestand voor uitvoer
> [!NOTE]
> In de nieuwste versie, de uitvoer JSON-indeling is gewijzigd en een belangrijke wijziging voor sommige klanten kan vertegenwoordigen.
> 
> 

De volgende tabel beschrijft de elementen van de JSON-bestand voor uitvoer.

| Element | Beschrijving |
| --- | --- |
| Versie |Dit verwijst naar de versie van de Video-API. De huidige versie is 2. |
| Tijdschaal |'Maatstreepjes' per seconde van de video. |
| Offset |De time-offset voor tijdstempels in 'maatstreepjes'. Versie 1.0 van Video-API's, zal dit altijd 0 zijn. In de toekomst scenario's die wordt ondersteund, deze waarde kan worden gewijzigd. |
| framesnelheid |Frames per seconde van de video. |
| Breedte, hoogte |Verwijst naar de breedte en hoogte van de video in pixels. |
| Starten |De tijdstempel begin in 'maatstreepjes'. |
| Duur |De lengte van de gebeurtenis, in 'maatstreepjes'. |
| Interval |Het interval van elke vermelding in de gebeurtenis, in 'maatstreepjes'. |
| Gebeurtenissen |Elke gebeurtenis fragment bevat de beweging binnen deze tijdsduur gedetecteerd. |
| Type |In de huidige versie is dit altijd '2' voor algemene beweging. Hierdoor kunnen label Video-API's de flexibiliteit om te categoriseren beweging in toekomstige versies. |
| RegionID |Zoals hierboven vermeld, zal dit altijd 0 in deze versie zijn. De dit label kunt API Video motion niet vinden in verschillende regio's in toekomstige versies. |
| Regio's |Verwijst naar het gedeelte in uw video waarin u het belangrijkst over beweging. <br/><br/>-het gebied regio 'id' aangeeft: in deze versie er is slechts één ID 0. <br/>-'type' de vorm van de regio die u voor beweging interesseren vertegenwoordigt. Op dit moment worden 'rechthoek' en 'veelhoek' ondersteund.<br/> Als u 'rechthoek' hebt opgegeven, heeft de regio dimensies in X, Y, Width en Height. De X en Y-coördinaten vertegenwoordigen de bovenste links Xyserver coördinaten van de regio in een genormaliseerde schaal van 0,0 en 1,0. De breedte en hoogte vertegenwoordigen de grootte van de regio in een genormaliseerde schaal van 0,0 en 1,0. In de huidige versie zijn X, Y, Width en Height altijd vast op 0, 0 en 1, 1. <br/>Als u 'veelhoek' hebt opgegeven, heeft de regio dimensies in punten. <br/> |
| fragmenten |De metagegevens wordt gesegmenteerde in verschillende segmenten fragmenten aangeroepen. Elke fragment bevat een start, duur, Intervalnummer en gebeurtenis(sen). Een fragment met er zijn geen gebeurtenissen betekent dat er geen beweging is aangetroffen tijdens die begintijd en duur. |
| []-Haken |Elke accolade vertegenwoordigt één interval in de gebeurtenis. Lege haakjes voor dat interval betekent dat er geen beweging is gedetecteerd. |
| Locaties |Deze nieuwe vermelding onder gebeurtenissen vindt u de locatie waar de beweging zich heeft voorgedaan. Dit is een meer specifiek zijn dan de zones van de detectie. |

Hier volgt een voorbeeld van een JSON-uitvoer

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a>Beperkingen
* De ondersteunde video-invoerindelingen zijn MP4 MOV en WMV.
* Bewegingsdetectie is geoptimaliseerd voor stilstaan achtergrond video's. De algoritme is gericht op het verminderen van gegeven, zoals licht wijzigingen en schaduwen.
* Sommige beweging kan niet worden herkend vanwege technische moeilijkheden; bijvoorbeeld 's avonds visie video's, semi-transparante objecten en kleine objecten.

## <a name="net-sample-code"></a>Voorbeeldcode voor .NET

De volgende programma toont hoe:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak met een video motion detection-taak op basis van een configuratiebestand met de volgende json-definitie maken: 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
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

namespace VideoMotionDetection
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

            // Run the VideoMotionDetection job.
            var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\VideoMotionDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
        }

        static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Video Motion Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Video Motion Detection Job");

            // Get a reference to Azure Media Motion Detector.
            string MediaProcessorName = "Azure Media Motion Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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
[Azure Media Services beweging detectie-blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

