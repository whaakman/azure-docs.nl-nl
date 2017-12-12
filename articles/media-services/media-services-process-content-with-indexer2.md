---
title: Indexeren van Media-bestanden met Azure Media Indexer 2 Preview | Microsoft Docs
description: Azure Media Indexer kunt u de inhoud van uw mediabestanden doorzoekbaar maken en voor het genereren van de tekst van een volledige-tekstindex voor gesloten ondertiteling en sleutelwoorden. Dit onderwerp wordt beschreven hoe Media Indexer 2 Preview gebruiken.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: 075daadae42340dfc9c7510a8010e337210e6e01
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indexeren van Media-bestanden met Azure Media Indexer 2 Preview
## <a name="overview"></a>Overzicht
De **Azure Media Indexer 2 Preview** Mediaprocessor (MP) kunt u doorzoekbaar maken van media-bestanden en inhoud, evenals gesloten closed captioning houdt genereren. Vergeleken met de vorige versie van [Azure Media Indexer](media-services-index-content.md), **Azure Media Indexer 2 Preview** voert sneller te indexeren en biedt taalondersteuning voor uitgebreidere. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees (Mandarijn, vereenvoudigd), Portugees, Arabisch, Russisch en Japans.

De **Azure Media Indexer 2 Preview** MP is momenteel in Preview.

In dit artikel wordt beschreven hoe maakt indexeertaken met **Azure Media Indexer 2 Preview**.

> [!NOTE]
> Het volgende letten:
> 
> Indexer 2 wordt niet ondersteund in Azure voor China en Azure Government.
> 
> Wanneer u inhoud, zorg er dan voor dat gebruikmaken van mediabestanden die duidelijk speech (zonder achtergrondmuziek, ruis, effecten of microfoon hiss) hebben. Enkele voorbeelden van de betreffende inhoud zijn: vastgelegd vergaderingen, lezingen en presentaties. De volgende inhoud is mogelijk niet geschikt voor indexering: films, televisieprogramma alles met gemengde Beeld en geluid effecten slecht opgenomen inhoud met achtergrondruis (hiss).
> 
> 

In dit artikel geeft informatie over **Azure Media Indexer 2 Preview** en laat zien hoe u deze gebruiken met Media Services SDK voor .NET

## <a name="input-and-output-files"></a>Invoer- en bestanden
### <a name="input-files"></a>Invoerbestanden
Audio- of -bestanden

### <a name="output-files"></a>Uitvoerbestanden
Een taak voor indexering kan ondertitelingsbestanden bestanden genereren in de volgende indelingen:  

* **SAMI**
* **TTML**
* **WebVTT**

Gesloten bijschrift (CC) bestanden in de volgende indelingen kunnen worden gebruikt om ervoor te audio-en toegankelijk voor mensen met een handicap horen.

## <a name="task-configuration-preset"></a>Taken configureren (standaardoptie)
Wanneer een taak maken van een indexering met **Azure Media Indexer 2 Preview**, moet u een configuratie-definitie opgeven.

De volgende JSON stelt beschikbare parameters.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

## <a name="supported-languages"></a>Ondersteunde talen
Azure Media Indexer 2 Preview ondersteunt spraak-naar-tekst voor de volgende talen (bij het opgeven van de taalnaam in de taakconfiguratie, 4 tekens code Gebruik haakjes zoals hieronder wordt weergegeven):

* Engels [EnUs]
* Spaans [EsEs]
* Chinees (Mandarijn, vereenvoudigd) [ZhCn]
* Frans [FrFr]
* Duits [DeDe]
* Italiaans [ItIt]
* Portugees [PtBr]
* Arabisch (Egyptisch) [ArEg]
* Japans [JaJp]
* Russisch [RuRu]
* Brits Engels [EnGb]
* Spaans (Mexico) [EsMx] 

## <a name="supported-file-types"></a>Ondersteunde bestandstypen

Zie voor meer informatie over de ondersteunde bestanden de [ondersteunde codecs/indelingen](media-services-media-encoder-standard-formats.md#input-containerfile-formats) sectie.

## <a name="net-sample-code"></a>Voorbeeldcode voor .NET

De volgende programma toont hoe:

1. Maak een asset en upload een mediabestand naar de asset.
2. Maakt een taak met een indexering taak op basis van een configuratiebestand met de volgende json-definitie:
   
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }
3. Download de uitvoerbestanden op. 
   
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

namespace IndexContent
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

            // Run indexing job.
            var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                        @"C:\supportFiles\Indexer\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
        }

        static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Indexing Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Indexing Job");

            // Get a reference to Azure Media Indexer 2 Preview.
            string MediaProcessorName = "Azure Media Indexer 2 Preview";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Indexing Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset to be indexed.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

