---
title: Azure Media Encoder Standard gebruikt om automatisch een bitrateladder genereren | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u automatisch een bitrateladder op basis van de invoerresolutie en bitrate genereren met Media Encoder Standard (MES). De invoerresolutie en bitrate wordt nooit worden overschreden. Bijvoorbeeld, als de invoer 720p op 3 Mbps, uitvoer wordt met de beste 720p blijven, en tarieven lager is dan 3 Mbps wordt gestart.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 5ccd53ac03235bd114218135488bef25891754be
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004058"
---
#  <a name="use-azure-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a>Azure Media Encoder Standard gebruikt om automatisch een bitrateladder genereren  

## <a name="overview"></a>Overzicht

Dit artikel wordt beschreven hoe u automatisch een bitrateladder (bitrate-resolutie paren) op basis van de invoerresolutie en bitrate genereren met Media Encoder Standard (MES). De automatisch gegenereerde voorinstelling wordt nooit groter zijn dan de invoerresolutie en de bitsnelheid. Bijvoorbeeld, als de invoer 720p met 3 Mbps is, uitvoer blijft 720p met de beste en tarieven lager is dan 3 Mbps wordt gestart.

### <a name="encoding-for-streaming-only"></a>Voor het streamen van alleen-codering

Als uw bedoeling is aan de bronvideo coderen voor het streamen van, moet klikt u vervolgens u de 'adaptief streamen"vooraf ingesteld bij het maken van een coderingstaak. Wanneer u de **adaptief streamen** definitie de MES-coderingsprogramma wordt op intelligente wijze gegevenslimiet een bitrateladder. Echter, u pas weer om te bepalen de codering kosten, omdat de service wordt bepaald hoeveel lagen als u wilt gebruiken en met welke resolutie. Ziet u voorbeelden van uitvoer lagen die worden geproduceerd door de MES als gevolg van de codering met de **adaptief streamen** vooraf aan het einde van dit artikel. De Asset bevat MP4-bestanden waar de audio en video-uitvoer is niet interleaved.

### <a name="encoding-for-streaming-and-progressive-download"></a>Encoding voor streamen en progressief downloaden

Als het uw bedoeling is aan de bronvideo coderen voor streaming, evenals voor het produceren MP4-bestanden voor het progressief downloaden, moet u de 'inhoud adaptieve meerdere Bitrate MP4"vooraf ingesteld bij het maken van een coderingstaak gebruiken. Wanneer u de **inhoud adaptieve meerdere Bitrate MP4** vooraf ingesteld, de codering met MES geldt codering dezelfde logica als hierboven, maar de uitvoerasset wordt nu MP4-bestanden bevatten, waar de audio en video interleaved is. U kunt een van deze MP4-bestanden (bijvoorbeeld, de hoogste versie bitrate) gebruiken als een bestand progressief downloaden.

## <a id="encoding_with_dotnet"></a>Coderen met mediaservices .NET SDK

Het volgende codevoorbeeld maakt gebruik van Media Services .NET SDK aan de volgende taken uitvoeren:

- Maak een coderingstaak.
- Een verwijzing naar de Media Encoder Standard encoder worden opgehaald.
- Een coderingstaak toevoegen aan het project en geef voor het gebruik van de **adaptief streamen** vooraf ingestelde. 
- Maak een uitvoerasset met de gecodeerde asset.
- Voeg een gebeurtenis-handler om te controleren of de taak wordt uitgevoerd.
- Verzend de taak.

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

## <a id="output"></a>Output

In deze sectie ziet u drie voorbeelden van uitvoer lagen die worden geproduceerd door de MES als gevolg van de codering met de **adaptief streamen** vooraf ingestelde. 

### <a name="example-1"></a>Voorbeeld 1
Bron met hoogte "1080" en "29.970" framesnelheid produceert 6 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Voorbeeld 2
Bron met hoogte "720" en "23.970" framesnelheid produceert 5 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Voorbeeld 3
Bron met hoogte "360" en "29.970" framesnelheid produceert 3 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services Encoding overzicht](media-services-encode-asset.md)

