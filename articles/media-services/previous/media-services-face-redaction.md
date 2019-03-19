---
title: Redigeer gezichten met Azure Media Analytics | Microsoft Docs
description: In dit onderwerp ziet u hoe u redigeer gezichten met Azure media analytics.
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
ms.date: 02/09/2019
ms.author: juliako;
ms.openlocfilehash: a0d6eb67c255a40856683c8a5c47c1e1e9f6753e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57837793"
---
# <a name="redact-faces-with-azure-media-analytics"></a>Redigeer gezichten met Azure Media Analytics 
## <a name="overview"></a>Overzicht
**Azure Media Redactor** is een [Azure Media Analytics](media-services-analytics-overview.md) Mediaprocessor (MP) biedt schaalbare gezichten onherkenbaar maken in de cloud. Gezichten onherkenbaar maken kunt u uw video om gezichten van geselecteerde personen onherkenbaar wijzigen. U wilt de gezichten onherkenbaar maken-service gebruikt in scenario's met openbare veiligheid en nieuwsmedia. Kunnen een paar minuten beeldmateriaal met meerdere gezichten uren redigeer handmatig duren, maar het proces van gezichten onherkenbaar maken moet met deze service slechts een paar eenvoudige stappen. Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Dit artikel geeft meer informatie over **Azure Media Redactor** en laat zien hoe u met Media Services SDK voor .NET gebruiken.

## <a name="face-redaction-modes"></a>Modi voor gezichten onherkenbaar maken
Gezichtsherkenning redactie werkt volgens het detecteren van gezichten in elke frame van video en het bijhouden van de face-object die beide doorstuurt en achterwaartse in-time, zodat de dezelfde persoon kunt appwisselaar vervaagd vanuit andere hoeken ook. Het proces van automatische redactie is complex en wordt niet altijd produceren 100% van de gewenste uitvoer, daarom Media Analytics beschikt u over een aantal manieren om te wijzigen van de uiteindelijke uitvoer.

Naast een volledig automatische modus is er een werkstroom twee keer, waardoor de selectie/de-selection van gezichten wordt uitgevoerd via een lijst met id's gevonden. Ook als u wilt maken van willekeurige per frame aanpassingen aan het Management Pack maakt gebruik van een bestand met metagegevens in JSON-indeling. Deze werkstroom is onderverdeeld in **analyseren** en **Redact** modi. U kunt de twee modi in één iteratie die beide taken worden uitgevoerd in één taak; combineren in deze modus wordt aangeroepen **gecombineerde**.

### <a name="combined-mode"></a>Gecombineerde modus
Dit resulteert in een geredigeerde mp4 automatisch zonder handmatige invoer.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoeractivum |foo.bar |Video in WMV, MOV of MP4-indeling |
| Invoer config |De configuratie van definitie |{'version':'1.0 ', '-opties: {'modus': 'gecombineerd'}} |
| Uitvoerasset |foo_redacted.mp4 |Video met betrekking tot toegepast |

#### <a name="input-example"></a>Voorbeeld van invoer:
[Bekijk deze video](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Voorbeeld van uitvoer:
[Bekijk deze video](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Modus analyseren
De **analyseren** pass van de werkstroom twee keer een video-invoer en produceert een JSON-bestand van de face-locaties en jpg-afbeeldingen voor elk gezicht gedetecteerd.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoeractivum |foo.bar |Video in WMV, MPV of MP4-indeling |
| Invoer config |De configuratie van definitie |{'version':'1.0 ', '-opties: {'modus': "analyseren"}} |
| Uitvoerasset |foo_annotations.json |De gegevens van de aantekening van face locaties in JSON-indeling. Dit kan worden bewerkt door de gebruiker te wijzigen van de vakken voor begrenzingsvak vervagen. Zie het voorbeeld hieronder. |
| Uitvoerasset |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Een bijgesneden jpg van elk gezicht, waarbij het nummer geeft aan de labelId van het gezicht gedetecteerd |

#### <a name="output-example"></a>Voorbeeld van uitvoer:

```json
    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated
```

### <a name="redact-mode"></a>Redigeer modus
De tweede fase van de werkstroom wordt een groter aantal invoerwaarden die moeten worden samengevoegd in één element.

Dit omvat een lijst-id's om te vervagen, de oorspronkelijke video en de aantekeningen JSON. In deze modus maakt gebruik van de aantekeningen om toe te passen op de video-invoer vervagen.

De uitvoer van de pas analyseren omvat niet de oorspronkelijke video. De video moet worden geüpload naar het invoeractivum voor de taak van de modus Redact, en als het primaire bestand geselecteerd.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoeractivum |foo.bar |De video in WMV, MPV of MP4-indeling. Hetzelfde als in stap 1 video. |
| Invoeractivum |foo_annotations.json |metagegevensbestand aantekeningen uit stap 1, met optionele wijzigingen. |
| Invoeractivum |foo_IDList.txt (optioneel) |Optionele nieuwe regel gescheiden lijst met face id's aan het redigeren. Als dit veld leeg blijft, vervaagt dit alle gezichten. |
| Invoer config |De configuratie van definitie |{'version':'1.0 ', '-opties: {'modus': 'redigeer'}} |
| Uitvoerasset |foo_redacted.mp4 |Video met betrekking tot toegepast op basis van aantekeningen |

#### <a name="example-output"></a>Voorbeeld van uitvoer
Dit is de uitvoer van een IDList met één ID geselecteerd.

[Bekijk deze video](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Voorbeeld foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>De voorbeeldafbeelding van typen

In de **gecombineerde** of **Redact** modus 5 verschillende vervagen modi u uit via de configuratie van de JSON-invoer kiezen kunt: **Lage**, **Med**, **hoge**, **vak**, en **zwart**. Standaard **Med** wordt gebruikt.

Hier vindt u voorbeelden van de onderstaande vervagen-typen.

### <a name="example-json"></a>Voorbeeld van JSON:

```json
    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}
```

#### <a name="low"></a>Laag

![Laag](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Hoog

![Hoog](./media/media-services-face-redaction/blur3.png)

#### <a name="box"></a>Box

![Box](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Zwart

![Zwart](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a>Elementen van het JSON-bestand voor uitvoer

Het Management Pack voor redactie biedt hoge precisie locatie gezichtsdetectie en maximaal 64 gezichten van mensen in een video frame bijhouden die kan detecteren. Voorzijde gezichten bieden de beste resultaten, terwijl de zijkanten en kleine gezichten (kleiner dan of gelijk aan 24 x 24 pixels) uitdaging zijn.

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET-voorbeeldcode

De volgende programma toont hoe u:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak maken met een gezichten onherkenbaar maken-taak die op basis van een configuratiebestand met de volgende json-definitie: 

    ```json
            {
                'version':'1.0',
                'options': {
                    'mode':'combined'
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

namespace FaceRedaction
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

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

