---
title: Detecteren van bewegingen met Azure Media Analytics | Microsoft Docs
description: De Mediaprocessor die Azure Media Motion Detector (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/10/2019
ms.author: milanga;juliako;
ms.openlocfilehash: 26090067923c468b7102ac5b7bb78b9d7b7960bb
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995609"
---
# <a name="detect-motions-with-azure-media-analytics"></a>Detecteren van bewegingen met Azure Media Analytics
## <a name="overview"></a>Overzicht
De **Azure Media Motion Detector** Mediaprocessor (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren. Bewegingsdetectie kan worden gebruikt op statische camerabeelden voor het identificeren van de secties van de video waar beweging optreedt. Er wordt een JSON-bestand met een metagegevens met tijdstempels en de omringende regio waar de gebeurtenis heeft plaatsgevonden gegenereerd.

Deze technologie is gericht op beveiliging videofeeds, kunnen beweging categoriseren in relevante gebeurtenissen en fout-positieven zoals schaduwen en wijzigingen in belichting. Hiermee kunt u voor het genereren van beveiligingswaarschuwingen van camera-feeds zonder terwijl deze kunnen ogenblikken van belang onttrekken aan lang toezicht video's met oneindige irrelevante gebeurtenissen, spam.

De **Azure Media Motion Detector** MP is momenteel in Preview.

Dit artikel geeft meer informatie over **Azure Media Motion Detector** en laat zien hoe u met Media Services SDK voor .NET gebruiken

## <a name="motion-detector-input-files"></a>Motion Detector invoerbestanden
Videobestanden. Op dit moment worden de volgende indelingen ondersteund: MP4, MOV en WMV.

## <a name="task-configuration-preset"></a>Taakconfiguratie (standaardoptie)
Bij het maken van een taak met **Azure Media Motion Detector**, moet u een configuratie-definitie opgeven. 

### <a name="parameters"></a>Parameters
U kunt de volgende parameters:

| Name | Opties | Description | Standaard |
| --- | --- | --- | --- |
| sensitivityLevel |String:'low', 'medium', 'high' |Hiermee stelt u de gevoeligheid niveau op welke bewegingen worden gerapporteerd. Hiermee kunt u het pas het aantal fout-positieven aanpassen. |'medium' |
| frameSamplingValue |positief geheel getal zijn |Hiermee stelt u de frequentie op waaronder de algoritme wordt uitgevoerd. 1 is gelijk aan elk frame, 2 betekent dat elke tweede frame, enzovoort. |1 |
| detectLightChange |Boolean:'true', 'false' |Wordt ingesteld of kleine wijzigingen in de resultaten zijn gerapporteerd |'False' |
| mergeTimeThreshold |Xs-time: Uu: mm:<br/>Voorbeeld: 00:00:03 |Hiermee geeft u de periode tussen bewegingsgebeurtenissen waar 2 gebeurtenissen zijn worden gecombineerd en gerapporteerd als 1. |00:00:00 |
| detectionZones |Een matrix van zones voor detectie:<br/>-Zone detectie is een matrix met punten 3 of meer<br/>-Punt is een x- en y coördinaat van 0 op 1. |Hierin wordt beschreven in de lijst met zones veelhoek detectie moet worden gebruikt.<br/>Resultaten met de zones worden gerapporteerd als een ID, met de eerste afbeelding wordt 'id': 0 |Één zone bevat informatie over de hele frame. |

### <a name="json-example"></a>Voorbeeld van JSON

```json
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
```

## <a name="motion-detector-output-files"></a>Uitvoerbestanden van Motion Detector
Een motion detection-taak retourneert een JSON-bestand in de uitvoer, waarin de actieve waarschuwingen en hun categorieën, binnen de video worden beschreven. Het bestand bevat informatie over de tijd en de duur van de beweging gedetecteerd in de video.

De Motion Detector-API biedt indicatoren zodra er objecten in beweging op een vaste achtergrond video (bijvoorbeeld een toezicht video zijn). De bewegingsherkenning wordt getraind op vals alarm, zoals verlichting en wijzigingen van de schaduw te beperken. Huidige beperkingen van de algoritmen zijn 's nachts vision-video's, semi-transparant objecten en kleine objecten.

### <a id="output_elements"></a>Elementen van het JSON-bestand voor uitvoer
> [!NOTE]
> In de nieuwste versie, de uitvoer-JSON-indeling is gewijzigd en kan een belangrijke wijziging voorstellen voor bepaalde klanten.
> 
> 

De volgende tabel beschrijft de elementen van de uitvoer-JSON-bestand.

| Element | Description |
| --- | --- |
| Versie |Dit verwijst naar de versie van de Video-API. De huidige versie is 2. |
| Tijdschaal |'Tikken"per seconde van de video. |
| Offset |De time-offset voor tijdstempels in "tikken." Versie 1.0 van Video-API's, wordt dit altijd 0 zijn. In de toekomst scenario's die wordt ondersteund, deze waarde kan worden gewijzigd. |
| Framesnelheid |Aantal frames per seconde video. |
| Breedte, hoogte |Verwijst naar de breedte en hoogte van de video in pixels. |
| Starten |De tijdstempel begin in 'maatstreepjes'. |
| Duur |De lengte van de gebeurtenis, in 'maatstreepjes'. |
| Interval |Het interval van elk item in de gebeurtenis, in 'maatstreepjes'. |
| Gebeurtenissen |Elke gebeurtenis fragment bevat de beweging gedetecteerd binnen deze tijdsduur. |
| Type |In de huidige versie is dit altijd '2' voor algemene beweging. Dit label biedt Video-API's de flexibiliteit voor het categoriseren van beweging in toekomstige versies. |
| RegionID |Zoals hierboven is uitgelegd, is dit altijd 0 in deze versie. Dit label biedt Video-API de flexibiliteit om te zoeken beweging in verschillende regio's in toekomstige versies. |
| Regio's |Verwijst naar het gebied in uw video waarin u geïnteresseerd beweging bent. <br/><br/>-het gebied van de regio 'id' aangeeft: in deze versie er is slechts één ID 0. <br/>-"type" vertegenwoordigt de vorm van de regio die u het belangrijkst voor beweging. Op dit moment worden "rechthoek" en 'veelhoek' ondersteund.<br/> Als u "rechthoek" hebt opgegeven, heeft de regio dimensies in X, Y, Width en Height. De X- en Y-coördinaten vertegenwoordigen de bovenste links XY-coördinaten van de regio in een genormaliseerde schaal van 0,0 en 1,0. De breedte en hoogte vertegenwoordigen de grootte van de regio in een genormaliseerde schaal van 0,0 en 1,0. In de huidige versie, zijn X, Y, Width en Height altijd vast bij 0, 0 en 1, 1. <br/>Als u 'veelhoek' hebt opgegeven, heeft de regio dimensies in punten. <br/> |
| Fragmenten |De metagegevens wordt gesegmenteerde overdrachtscodering omhoog in verschillende segmenten fragmenten genoemd. Elk fragment bevat een start, een duur, een intervalnummer en een of meer gebeurtenissen. Een fragment met Nee gebeurtenissen kunnen geen beweging gedetecteerd tijdens de begintijd en duur. |
| Vierkante haken] |Elke haakje vertegenwoordigt één interval in de gebeurtenis. Lege vierkante haken voor dat interval betekent dat er geen beweging is gedetecteerd. |
| locaties |Deze nieuwe vermelding onder gebeurtenissen geeft een lijst van de locatie waar de beweging optreedt. Dit is ook specifieker dan de detectie van zones. |

De volgende JSON-voorbeeld wordt de uitvoer weergegeven:

```json
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
```

## <a name="limitations"></a>Beperkingen
* De ondersteunde indelingen voor invoer video opnemen MP4 MOV en WMV.
* Bewegingsdetectie is geoptimaliseerd voor een stilstaande achtergrond video's. Het algoritme is gericht op het verkorten van vals alarm, zoals wijzigingen in belichting en schaduwen.
* Sommige beweging mogelijk niet gedetecteerd door technische uitdagingen; bijvoorbeeld, 's nachts vision video's, semi-transparant objecten en kleine objecten.

## <a name="net-sample-code"></a>.NET-voorbeeldcode

De volgende programma toont hoe u:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak maken met een video motion detection-taak die op basis van een configuratiebestand met de volgende json-definitie: 
   
    ```json
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
            task.OutputAssets.AddNew("My Video Motion Detection Output Asset", AssetCreationOptions.None);

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
[Blog over Azure Media Services Motion Detector](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

