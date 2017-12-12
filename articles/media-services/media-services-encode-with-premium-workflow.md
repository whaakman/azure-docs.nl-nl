---
title: Geavanceerde codering met Media Encoder Premium Workflow | Microsoft Docs
description: Informatie over het coderen met Media Encoder Premium Workflow. Codevoorbeelden zijn geschreven in C# en gebruiken van de Media Services SDK voor .NET.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 0cb7c7c33866ea078ab597e71cb56a601929f8f5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Geavanceerde codering met Media Encoder Premium Workflow
> [!NOTE]
> Media Encoder Premium media werkstroomverwerking besproken in dit artikel is niet beschikbaar in China.
>
>

Premium-encoder vragen, e-mepd op Microsoft.com.

## <a name="overview"></a>Overzicht
Microsoft Azure Media Services introduceert de **Media Encoder Premium werkstroom** Mediaprocessor. Deze processor biedt geavanceerde mogelijkheden voor uw werkstromen premium-op-verzoek-codering.

De volgende onderwerpen worden gegevens over **Media Encoder Premium werkstroom**:

* [Ondersteund door de werkstroom met Media Encoder Premium indelingen](media-services-premium-workflow-encoder-formats.md) : het bestand Discusses indelingen en codecs ondersteund door **Media Encoder Premium werkstroom**.
* [Overzicht en een vergelijking van Azure op aanvraag media coderingsprogramma's](media-services-encode-asset.md) vergelijkt de codering mogelijkheden van **Media Encoder Premium werkstroom** en **Media Encoder Standard**.

In dit artikel laat zien hoe coderen met **Media Encoder Premium werkstroom** met .NET.

Codering van taken voor de **Media Encoder Premium werkstroom** vereisen een afzonderlijke configuratie-bestand, een werkstroom wordt genoemd. Deze bestanden hebben de extensie .workflow en worden gemaakt met de [Workflow Designer](media-services-workflow-designer.md) hulpprogramma.

U kunt de standaard ook ophalen Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De Werkstroombestanden moeten worden geüpload naar uw Media Services-account als een actief en dit activum moet worden doorgegeven aan de codering taak.

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 

## <a name="encoding-example"></a>Voorbeeld van de codering

Het volgende voorbeeld laat zien hoe coderen met **Media Encoder Premium werkstroom**.

De volgende stappen worden uitgevoerd:

1. Maak een asset en upload het werkstroombestand van een.
2. Maak een asset en upload een bronbestand voor media.
3. De media-processor 'Media Encoder Premium Workflow' worden opgehaald.
4. Een taak en een taak maken.

    In de meeste gevallen de configuratietekenreeks voor de taak is leeg (zoals in het volgende voorbeeld). Er zijn een aantal geavanceerde scenario's (waarvoor u eigenschappen van de runtime dynamisch) in dat geval zou u een XML-tekenreeks voor de codering taak opgeven. Voorbeelden van dergelijke scenario's zijn: een overlay, parallelle en sequentiële media wilt, subtitling maken.
5. Twee invoer elementen toevoegen aan de taak.

    1. 1e: de werkstroom asset.
    2. 2e: de video asset.

    >[!NOTE]
    >De asset werkstroom moet worden toegevoegd aan de taak voordat de asset media.
   De configuratietekenreeks voor deze taak mag niet leeg zijn.
   
6. De coderingstaak verzenden.

```
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace MediaEncoderPremiumWorkflowSample
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

        private static readonly string _supportFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _workflowFilePath =
            Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

        private static readonly string _singleMP4InputFilePath =
            Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

        static void Main(string[] args)
        {

            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
            var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
            IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

        }

        static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
        {
            var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
            var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return asset;
        }

        static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Premium Workflow encoding job");
            // Get a media processor reference, and pass to it the name of the
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                processor,
                "",
                TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(workflow);
            task.InputAssets.Add(video); // we add one asset
                                         // Add an output asset to contain the results of the job.
                                         // This output is specified as AssetCreationOptions.None, which
                                         // means the output asset is not encrypted.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Use the following event handler to check job progress.  
            job.StateChanged += new
                    EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch the job.
            job.Submit();

            // Check job execution and wait for job to finish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                throw new Exception("\nExiting method due to job error.");
            }

            return job.OutputMediaAssets[0];
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
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

Premium-encoder vragen, e-mepd op Microsoft.com.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
