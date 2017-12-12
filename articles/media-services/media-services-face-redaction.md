---
title: Redigeren vlakken met Azure Media Analytics | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u Redigeren vlakken met Azure media analytics.
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
ms.author: juliako;
ms.openlocfilehash: 2e936379968f74eb8bea420916acea2b8d96bb24
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a>Redigeren vlakken met Azure Media Analytics
## <a name="overview"></a>Overzicht
**Azure Media Redactor** is een [Azure Media Analytics](media-services-analytics-overview.md) Mediaprocessor (MP) die schaalbare face redactie in de cloud biedt. Face redactie kunt u uw video te wijzigen als u wilt vervagen vlakken van geselecteerde gebruikers. U wilt de redactie face service in openbare veiligheid en nieuws media scenario's gebruiken. Kunnen een paar minuten beeldmateriaal waarin meerdere vlakken uren handmatig Redigeren duren, maar met deze service het face redactie proces een paar eenvoudige stappen is vereist. Zie voor meer informatie [dit](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

In dit artikel geeft informatie over **Azure Media Redactor** en laat zien hoe u deze gebruiken met Media Services SDK voor .NET.

## <a name="face-redaction-modes"></a>Face redactie modi
Analyseert redactie werkt door het detecteren van vlakken in elke frame van video en het face-object beide voorwaarts en achterwaarts bijhouden in de tijd, zodat dezelfde persoon kan worden vervaagd vanuit andere hoeken ook. Het proces geautomatiseerde redactie is complex en komt niet altijd produceren 100% van de gewenste uitvoer, om deze reden Media Analytics beschikt u over een aantal manieren om te wijzigen van de uiteindelijke uitvoer.

Naast een volledig automatische modus is er een werkstroom twee keer, waardoor de selectie/de-selection gevonden vlakken via een lijst met id. Ook als u wilt willekeurige per frame aanpassingen aan het Management Pack maken maakt gebruik van een bestand met metagegevens in JSON-indeling. Deze werkstroom is onderverdeeld in **analyseren** en **Redact** modi. U kunt de twee modi in één iteratie die beide taken in één taak uitvoert; combineren Deze modus wordt aangeroepen **gecombineerde**.

### <a name="combined-mode"></a>Gecombineerde modus
Dit resulteert in een geredigeerde mp4 automatisch zonder handmatige invoer.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoer asset |foo.bar |Video in WMV, MOV of MP4-indeling |
| Invoer config |De configuratie van definitie |{{'version':'1.0 ","opties": {'mode': 'gecombineerd'}} |
| Uitvoerasset |foo_redacted.mp4 |Video met zorgt voor een vervaging toegepast |

#### <a name="input-example"></a>Voorbeeld van invoer:
[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Voorbeeld van uitvoer:
[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Modus analyseren
De **analyseren** op te geven van de werkstroom twee keer een video-invoer en genereert een JSON-bestand van face locaties en jpg-afbeeldingen van elke face gedetecteerd.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoer asset |foo.bar |Video in WMV, MPV of MP4-indeling |
| Invoer config |De configuratie van definitie |{{'version':'1.0 ","opties": {'mode': 'analyseren'}} |
| Uitvoerasset |foo_annotations.JSON |De gegevens van de aantekening van face locaties in JSON-indeling. Dit kan worden bewerkt door de gebruiker te wijzigen van de vakken voor begrenzingsvak vervagen. Zie onderstaand voorbeeld. |
| Uitvoerasset |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Een bijgesneden jpg van elke gedetecteerd gezicht, waarbij het nummer geeft aan de labelId van het oppervlak |

#### <a name="output-example"></a>Voorbeeld van uitvoer:

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

### <a name="redact-mode"></a>Modus Redigeren
De tweede stap van de werkstroom wordt een groter aantal invoerwaarden die moeten worden samengevoegd in één element.

Dit omvat een lijst met de id's om te vervagen: de oorspronkelijke video en aantekeningen JSON. Deze modus gebruikt de aantekeningen toe te passen zorgt voor een vervaging van de video invoer.

De uitvoer van de Geef analyseren omvat niet de oorspronkelijke video. De video moet worden geüpload naar de invoer asset voor de modus Redact taak en als het primaire bestand geselecteerd.

| Fase | Bestandsnaam | Opmerkingen |
| --- | --- | --- |
| Invoer asset |foo.bar |Video WMV, MPV of MP4-indeling. Hetzelfde als in stap 1 video. |
| Invoer asset |foo_annotations.JSON |metagegevensbestand aantekeningen uit stap 1, optionele wijzigingen. |
| Invoer asset |foo_IDList.txt (optioneel) |Optionele nieuwe regel gescheiden lijst van gezicht Redigeren-id. Als dit leeg laat, vervaging dit alle vlakken. |
| Invoer config |De configuratie van definitie |{{'version':'1.0 ","opties": {'mode': 'Redigeren'}} |
| Uitvoerasset |foo_redacted.mp4 |Video met zorgt voor een vervaging toegepast op basis van aantekeningen |

#### <a name="example-output"></a>Voorbeeld van uitvoer
Dit is de uitvoer van een IDList met een ID die zijn geselecteerd.

[Bekijk deze video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Voorbeeld foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Typen vervagen

In de **gecombineerde** of **Redact** modus, er zijn 5 vervaging verschillende modi, u uit via de configuratie van de JSON-invoer kiezen kunt: **laag**, **Med**, **Hoge**, **vak**, en **zwart**. Standaard **Med** wordt gebruikt.

Hier vindt u voorbeelden van de onderstaande vervaging-typen.

### <a name="example-json"></a>Voorbeeld JSON:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

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

## <a name="elements-of-the-output-json-file"></a>Elementen van de JSON-bestand voor uitvoer

De MP redactie biedt hoge precisie face locatie detectie en bijhouden die kan maximaal 64 menselijke vlakken detecteren in een video frame. Voorzijde vlakken bieden de beste resultaten, tijdens het kant vlakken en in kleine vlakken (kleiner dan of gelijk aan 24 x 24 pixels) lastig zijn.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>Voorbeeldcode voor .NET

De volgende programma toont hoe:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak met een face redactie taak op basis van een configuratiebestand met de volgende json-definitie maken: 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
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

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

