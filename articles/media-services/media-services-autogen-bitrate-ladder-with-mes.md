---
title: Azure Media Encoder Standard gebruiken voor het automatisch genereren van een ladder bitrate | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe Media Encoder Standard (MES) gebruiken om een bitrate ladder op basis van de invoer resolutie en bitrate automatisch te genereren. De invoer resolutie en bitrate wordt nooit worden overschreden. Bijvoorbeeld, als de invoer is 720p op 3 Mbps, uitvoer wordt met de beste 720p blijven en wordt gestart volgens de tarieven voor lager is dan 3 Mbps.
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
ms.date: 12/10/2017
ms.author: juliako
ms.openlocfilehash: 4ffced8e11f05d214995f9fc8506dd7c6c7deaa5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
#  <a name="use-azure-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a>Azure Media Encoder Standard gebruiken om een ladder bitrate automatisch te genereren

## <a name="overview"></a>Overzicht

In dit artikel laat zien hoe Media Encoder Standard (MES) gebruiken om een bitrate ladder (bitrate resolutie paren) op basis van de invoer resolutie en bitrate automatisch te genereren. De automatisch gegenereerde voorinstelling zijn nooit overschrijden de invoer resolutie en de bitsnelheid. Bijvoorbeeld, als de invoer 720p met 3 Mbps is, uitvoer blijft 720p met de beste en volgens de tarieven voor lager is dan 3 Mbps wordt gestart.

### <a name="encoding-for-streaming-only"></a>Voor Streaming alleen codering

Als het uw bedoeling is om de bronvideo alleen voor streaming coderen, moet vervolgens u de 'adaptief streamen' vooraf ingesteld bij het maken van een taak die voor codering. Wanneer u de **adaptief streamen** definitie het coderingsprogramma MES wordt op intelligente wijze cap een ladder bitrate. Echter, u pas weer om te bepalen de codering kosten, omdat de service bepaalt hoeveel lagen als u wilt gebruiken en met welke resolutie. Ziet u voorbeelden van lagen van uitvoer geproduceerd door MES als gevolg van codering met de **adaptief streamen** voorinstelling aan het einde van dit artikel. De Asset MP4-bestanden bevat waar audio en video-uitvoer is niet interleaved.

### <a name="encoding-for-streaming-and-progressive-download"></a>Codering in voor streamen en progressief downloaden

Als het uw bedoeling is voor het coderen van de bronvideo voor streaming, evenals voor het produceren MP4-bestanden progressief downloaden, moet u de 'inhoud adaptieve meerdere Bitrate MP4' vooraf ingesteld bij het maken van een taak die voor codering gebruiken. Wanneer u de **inhoud adaptieve meerdere Bitrate MP4** definitie het coderingsprogramma MES geldt dezelfde codering logica zoals hierboven, maar nu de uitvoerasset bevat MP4-bestanden waar audio en video interleaved is. U kunt een van deze MP4-bestanden (bijvoorbeeld de hoogste versie bitrate) gebruiken als een bestand progressief downloaden.

## <a id="encoding_with_dotnet"></a>Codering met mediaservices .NET SDK

Het volgende codevoorbeeld maakt gebruik van Media Services .NET SDK naar de volgende taken uitvoeren:

- Maak een codeertaak.
- Een verwijzing naar de Media Encoder Standard encoder ophalen.
- Een codering taak toevoegen aan het project en geef voor het gebruik van de **adaptief streamen** vooraf ingestelde. 
- Maak een uitvoerasset met de gecodeerde asset.
- Een gebeurtenis-handler voor het controleren van de voortgang van de taak toevoegen.
- Verzenden van de taak.

#### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Voorbeeld

```
using System;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace AdaptiveStreamingMESPresest
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using the "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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

## <a id="output"></a>Uitvoer

Deze sectie ziet u drie voorbeelden van lagen van uitvoer geproduceerd door MES als gevolg van codering met de **adaptief streamen** vooraf ingestelde. 

### <a name="example-1"></a>Voorbeeld 1
Bron met hoogte '1080' en '29.970' framesnelheid produceert 6 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Voorbeeld 2
Bron met hoogte '720' en '23.970' framesnelheid produceert 5 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Voorbeeld 3
Bron met hoogte '360' en '29.970' framesnelheid produceert 3 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services-codering overzicht](media-services-encode-asset.md)

