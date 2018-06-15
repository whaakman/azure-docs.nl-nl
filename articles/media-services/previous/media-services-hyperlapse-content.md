---
title: Hyperlapse Media-bestanden met Azure Media Hyperlapse | Microsoft Docs
description: Azure Media Hyperlapse maakt smooth verstreken tijd video's van de eerste persoon of actie camera inhoud. Dit onderwerp leest het gebruik van Media indexeerfunctie.
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
ms.openlocfilehash: ed64a616538ed4699abc03225a2dcf27d164521f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788485"
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse Media-bestanden met Azure Media Hyperlapse
Azure Media Hyperlapse is een Media Processor (MP) die u smooth video's verstreken tijd van eerste persoon of actie camera inhoud maakt.  De cloud-gebaseerd op hetzelfde niveau naar [van Microsoft Research bureaublad Hyperlapse Pro en Hyperlapse mobiele telefoon](http://aka.ms/hyperlapse), Microsoft Hyperlapse voor Azure Media Services maakt gebruik van de grote schaal van de verwerking van Azure Media Services Media platform horizontaal schalen en parallelize bulksgewijs Hyperlapse verwerken.

> [!IMPORTANT]
> Microsoft Hyperlapse is ontworpen om te werken het beste op de eerste persoon inhoud met een zwevend camera. Hoewel u nog steeds camerabeelden kunt nog steeds werken, kunnen niet de prestaties en kwaliteit van de Azure Media Hyperlapse Media-Processor worden gegarandeerd voor andere typen inhoud.
> 
> 

Een Azure Media Hyperlapse taak als neemt invoer een MP4, MOV of WMV assetbestand samen met een configuratiebestand waarmee wordt aangegeven welke frames van video moeten zijn verstreken tijd en op welke snelheid (bijvoorbeeld de eerste 10.000 frames 2 x).  De uitvoer is een weergave gestabiliseerd en verstreken tijd van de video invoer.

## <a name="hyperlapse-an-asset"></a>Hyperlapse een asset
U moet eerst uw gewenste invoerbestand uploaden naar Azure Media Services.  Lees voor meer informatie over de concepten die betrokken zijn bij het uploaden en beheren van inhoud, de [inhoudsbeheer artikel](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Definitie van de configuratie voor Hyperlapse
Nadat de inhoud van uw Media Services-account is, moet u uw configuratie-definitie maken.  De volgende tabel beschrijft de gebruiker gedefinieerde velden:

| Veld | Beschrijving |
| --- | --- |
| StartFrame |Het frame waarop de verwerking van de Microsoft Hyperlapse moet beginnen. |
| NumFrames |Het aantal frames verwerken |
| Snelheid |De factor waarmee de invoer video versnellen. |

Hier volgt een voorbeeld van een configuratiebestand overeenstemmende in XML en JSON:

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
De volgende methode uploadt een mediabestand als een actief en maakt een taak met de Azure Media Hyperlapse Media-Processor.

> [!NOTE]
> U hebt al een CloudMediaContext binnen het bereik met de naam 'context' voor deze code werkt.  Lees voor meer informatie over deze de [inhoudsbeheer artikel](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> Het argument van de tekenreeks 'hyperConfig' wordt verwacht een conform de configuratie in JSON of XML-definitie zoals hierboven is beschreven.
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

