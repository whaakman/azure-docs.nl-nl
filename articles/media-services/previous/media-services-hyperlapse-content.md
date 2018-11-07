---
title: Hyperlapse Media-bestanden met Azure Media Hyperlapse | Microsoft Docs
description: Azure Media Hyperlapse maakt smooth timelapsevideo's te versoepelen van de eerste persoon of het actie-camera-inhoud. In dit onderwerp laat zien hoe Media Indexer gebruiken.
services: media-services
documentationcenter: ''
author: asolanki
manager: johndeu
editor: ''
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2018
ms.author: adsolank
ms.openlocfilehash: 268e679bb052bce4c972c940333147edc5c7d721
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242582"
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse Media-bestanden met Azure Media Hyperlapse
Azure Media Hyperlapse is een medium Processor (KP) die smooth timelapsevideo's te versoepelen worden gemaakt van de eerste persoon of het actie-camera-inhoud.  De cloud gebaseerde op hetzelfde niveau naar [van Microsoft Research bureaublad Hyperlapse Pro en telefonische Hyperlapse Mobile](https://aka.ms/hyperlapse), Microsoft Hyperlapse voor Azure Media Services maakt gebruik van de enorme schaal van de Azure Media Services Media verwerken platform voor horizontaal schalen en parallelliseren bulksgewijs Hyperlapse-verwerking.

> [!IMPORTANT]
> Microsoft Hyperlapse is ontworpen om te werken het beste op de eerste persoon inhoud met een zwevend camera. Hoewel nog camerabeelden nog steeds werken kunt, kunnen niet de prestaties en kwaliteit van de Azure Media Hyperlapse Media-Processor worden gegarandeerd voor andere typen inhoud.
> 
> 

Een Azure Media Hyperlapse-taak wordt gebruikt als invoer een MP4-, MOV of WMV assetbestand samen met een configuratiebestand waarmee wordt aangegeven welke frames van de video moeten zijn verstreken tijd en op welke snelheid (bijvoorbeeld de eerste 10.000 frames op 2 x).  De uitvoer is een weergave gestabiliseerde en verstreken tijd van de video-invoer.

## <a name="hyperlapse-an-asset"></a>Hyperlapse een asset
U moet eerst uw gewenste invoerbestand uploaden naar Azure Media Services.  Lees voor meer informatie over de concepten die betrokken zijn bij het uploaden en beheren van inhoud, de [inhoudsbeheer artikel](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Definitie van de configuratie voor Hyperlapse
Nadat de inhoud van uw Media Services-account is, moet u uw configuratie-definitie maken.  De volgende tabel beschrijft de gebruiker opgegeven velden:

| Veld | Beschrijving |
| --- | --- |
| StartFrame |Het frame waarop de Microsoft Hyperlapse-verwerking moet beginnen. |
| NumFrames |Het aantal frames verwerken |
| Snelheid |De factor op waarmee de invoervideo versnellen. |

Hier volgt een voorbeeld van een configuratiebestand in overeenstemming in XML en JSON:

**XML-definitie:**
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>
```

**JSON-definitie:**
```json
    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }
```

### <a id="sample_code"></a> Microsoft Hyperlapse met de AMS .NET SDK
De volgende methode uploadt een mediabestand als een asset en maakt u een taak met de Azure Media Hyperlapse Media Processor.

> [!NOTE]
> U hebt al een CloudMediaContext binnen het bereik met de naam 'context' voor deze code om te werken.  Lees voor meer informatie over dit de [inhoudsbeheer artikel](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> Het tekenreeksargument 'hyperConfig' wordt verwacht in overeenstemming configuratie in JSON of XML-definitie, zoals hierboven beschreven.
> 
> 

```csharp
        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }
```

### <a id="file_types"></a>Ondersteunde bestandstypen
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

