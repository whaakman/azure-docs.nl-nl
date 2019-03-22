---
title: Tekst met Azure Media Analytics OCR digitaliseren | Microsoft Docs
description: Azure Media Analytics OCR (optische tekenherkenning) kunt u tekstinhoud in video's converteren naar bewerkbare, doorzoekbaar digitale tekst.  Hiermee kunt u voor het automatiseren van de extractie van nuttige metagegevens van de video signaal van de media.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 91fad34073d7505c596bedfb6c93946ee7393dd7
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315127"
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Azure Media Analytics gebruiken om te converteren tekstinhoud in video's naar digitale tekst  
## <a name="overview"></a>Overzicht
Als u nodig hebt voor tekstinhoud onttrekken aan uw videobestanden en het genereren van een digitale tekst worden bewerkt, doorzoekbaar, moet u Azure Media Analytics OCR (optische tekenherkenning). Deze Azure-Mediaprocessor detecteert tekstinhoud in uw videobestanden en genereert tekstbestanden voor het gebruik. OCR, kunt u de extractie van nuttige metagegevens van de video signaal van uw media automatiseren.

Wanneer gebruikt in combinatie met een zoekprogramma, kunt u eenvoudig index van uw media op tekst en verbeter de zichtbaarheid van uw inhoud. Dit is bijzonder nuttig in maximaal tekstuele video, zoals een video-opname of een schermopname van een diavoorstelling. De Azure OCR Mediaprocessor die is geoptimaliseerd voor digitale tekst.

De **Azure Media OCR** Mediaprocessor is momenteel in Preview.

Dit artikel geeft meer informatie over **Azure Media OCR** en laat zien hoe u met Media Services SDK voor .NET gebruiken. Zie voor meer informatie en voorbeelden, [deze blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>OCR-invoerbestanden
Videobestanden. Op dit moment worden de volgende indelingen ondersteund: MP4, MOV en WMV.

## <a name="task-configuration"></a>Configuratie van de taak
Taakconfiguratie van de (standaardoptie). Bij het maken van een taak met **Azure Media OCR**, moet u een configuratie met behulp van JSON of XML-definitie opgeven. 

>[!NOTE]
>De OCR-engine gaat alleen de regio van een installatiekopie met minimaal 40 pixels naar maximale 32000 pixels als een geldige invoer in beide hoogte/breedte.
>

### <a name="attribute-descriptions"></a>Beschrijvingen van kenmerken
| De naam van kenmerk | Description |
| --- | --- |
|AdvancedOutput| Als u AdvancedOutput ingesteld op true, wordt de JSON-uitvoer positionele gegevens voor elke één woord (naast zinnen en regio's) bevatten. Als u niet zien deze gegevens wilt, moet u de vlag ingesteld op false. De standaardwaarde is false. Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).|
| Taal |(optioneel) een beschrijving van de taal van de tekst waarnaar moet worden gezocht. Een van de volgende: Automatische detectie (standaard), Arabisch, ChineseSimplified, ChineseTraditional, Tsjechisch Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, SerbianCyrillic, SerbianLatin , Slowaaks, Spaans, Zweeds, Turks. |
| TextOrientation |(optioneel) een beschrijving van de richting van tekst waarvoor u wilt zoeken.  "Links" betekent dat er met de bovenkant van alle letters waarnaar wordt verwezen naar de linkerkant.  Standaardtekst (zoals dat kan worden gevonden in een boek) kan worden aangeroepen 'Van' geplaatst.  Een van de volgende: Autodetectie (standaard), maximaal, rechts, omlaag naar links. |
| TimeInterval |(optioneel) een beschrijving van de samplingfrequentie.  Standaard is elke seconde 1/2.<br/>JSON-indeling: uu: mm:. SSS (standaard 00:00:00.500)<br/>XML-indeling: W3C XSD-duur primitieve (standaard PT0.5) |
| DetectRegions |(optioneel) Een matrix met DetectRegion objecten op te geven de regio's binnen de video waarin u kunt tekst detecteren.<br/>Een DetectRegion-object wordt gemaakt van de volgende vier waarden:<br/>Links: pixels vanaf de linkermarge<br/>Top – pixels vanaf de bovenmarge<br/>Breedte-breedte van de regio in pixels<br/>Hoogte-hoogte van de regio in pixels |

#### <a name="json-preset-example"></a>Vooraf gedefinieerde JSON-voorbeeld

```json
    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }
```

#### <a name="xml-preset-example"></a>Vooraf gedefinieerde XML-voorbeeld

```xml
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""https://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""https://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""https://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>
```

## <a name="ocr-output-files"></a>Uitvoerbestanden OCR
De uitvoer van de Mediaprocessor die OCR is een JSON-bestand.

### <a name="elements-of-the-output-json-file"></a>Elementen van het JSON-bestand voor uitvoer
De Video OCR-uitvoer kunt u tijd-gesegmenteerde gegevens van de tekens die is gevonden in de video.  U kunt kenmerken, zoals taal of de afdrukstand hone in op woorden dat u geïnteresseerd in analyseren bent. 

De uitvoer bevat de volgende kenmerken:

| Element | Description |
| --- | --- |
| Tijdschaal |'maatstreepjes' per seconde van de video |
| Offset |tijd offset voor tijdstempels. Versie 1.0 van Video-API's, wordt dit altijd 0 zijn. |
| Framesnelheid |Frames per seconde van de video |
| Breedte |breedte van de video in pixels |
| Hoogte |hoogte van de video in pixels |
| Fragmenten |matrix van segmenten van de video waarin de metagegevens van de gesegmenteerde overdrachtscodering op basis van tijd |
| start |Begintijd van een fragment in 'maatstreepjes' |
| duur |lengte van een fragment in 'maatstreepjes' |
| interval |interval van elke gebeurtenis in het opgegeven fragment |
| events |matrix met regio 's |
| regio |object voor woorden of zinsdelen gedetecteerd |
| language |taal van de tekst gedetecteerd binnen een regio |
| Afdrukstand |richting van de tekst gedetecteerd binnen een regio |
| regels |matrix van regels tekst gedetecteerd binnen een regio |
| tekst |de werkelijke tekst |

### <a name="json-output-example"></a>Voorbeeld van JSON-uitvoer
Het volgende Uitvoervoorbeeld bevat de algemene informatie over de video en verschillende video fragmenten. In elke video fragment bevat elke regio die wordt gedetecteerd door OCR MP met de taal en de richting van de tekst. De regio bevat ook elke regel woord in deze regio met een van de regel tekst, positie van de regel en elke word-gegevens (word-inhoud, positie en vertrouwen) in deze regel. Hier volgt een voorbeeld en ik bepaalde inline opmerkingen plaatsen.

```json
    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }
```

## <a name="net-sample-code"></a>.NET-voorbeeldcode

De volgende programma toont hoe u:

1. Maak een asset en upload een mediabestand naar de asset.
2. Een taak maken met een OCR-configuratie/definitie-bestand.
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

namespace OCR
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

            // Run the OCR job.
            var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                        @"C:\supportFiles\OCR\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
        }

        static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My OCR Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My OCR Job");

            // Get a reference to Azure Media OCR.
            string MediaProcessorName = "Azure Media OCR";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My OCR Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

