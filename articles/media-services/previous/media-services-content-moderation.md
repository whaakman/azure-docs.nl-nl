---
title: Azure Media Content Moderator gebruiken voor het detecteren van mogelijk erotische en ongepaste inhoud | Microsoft Docs
description: Videotoezicht helpt bij het detecteren van mogelijke erotische en ongepaste inhoud in video's.
services: media-services
documentationcenter: ''
author: sanjeev3
manager: mikemcca
editor: ''
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: eb16f5e1e72e5a9379ad530ab9677adba2ccbbcd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57899560"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Azure Media Content Moderator gebruiken voor het detecteren van mogelijk erotische en ongepaste inhoud 

## <a name="overview"></a>Overzicht
De **Azure Media Content Moderator** Mediaprocessor (MP) kunt u geautomatiseerd beheer gebruiken voor uw video's. U wilt bijvoorbeeld detecteren van mogelijk erotische en ongepaste inhoud in video's en bekijk de gemarkeerde inhoud door uw teams menselijk toezicht.

De **Azure Media Content Moderator** MP is momenteel in Preview.

Dit artikel geeft meer informatie over **Azure Media Content Moderator** en laat zien hoe u met Media Services SDK voor .NET gebruiken.

## <a name="content-moderator-input-files"></a>Content Moderator-invoerbestanden
Videobestanden. Op dit moment worden de volgende indelingen ondersteund: MP4, MOV en WMV.

## <a name="content-moderator-output-files"></a>Content Moderator-uitvoerbestanden
De uitvoer van de gecontroleerde in de JSON-indeling bevat opnamen automatisch gedetecteerd en hoofdframes. De hoofdframes worden geretourneerd met vertrouwen scores voor mogelijke volwassenen of ongepaste inhoud. Ze bevatten ook een Booleaanse vlag die aangeeft of een beoordeling wordt aanbevolen. De vlag van de aanbeveling controleren is waarden op basis van de interne drempelwaarden voor erotische en ongepaste scores toegewezen.

## <a name="elements-of-the-output-json-file"></a>Elementen van het JSON-bestand voor uitvoer

De taak levert een JSON-uitvoer-bestand met metagegevens over gedetecteerde foto's en hoofdframes en dat ze volwassenen of ongepaste inhoud bevatten.

De JSON-uitvoer bevat de volgende elementen:

### <a name="root-json-elements"></a>Hoofdmap JSON-elementen

| Element | Description |
| --- | --- |
| versie |De versie van de Content Moderator. |
| tijdschaal |'Tikken"per seconde van de video. |
| offset |De tijd-offset voor tijdstempels. Versie 1.0 van Video-API's, wordt deze waarde altijd 0 zijn. Deze waarde kan in de toekomst worden gewijzigd. |
| framerate |Aantal frames per seconde video. |
| Breedte |De breedte van de video frame van uitvoer in pixels.|
| Hoogte |De hoogte van de video frame van uitvoer in pixels.|
| totalDuration |De duur van de invoervideo, in "tikken." |
| [fragments](#fragments-json-elements) |De metagegevens wordt gesegmenteerde overdrachtscodering omhoog in verschillende segmenten fragmenten genoemd. Elk fragment wordt een foto automatisch is gedetecteerd met een begindatum, duur, Intervalnummer en gebeurtenis(sen). |

### <a name="fragments-json-elements"></a>Fragmenten JSON-elementen

|Element|Description|
|---|---|
| start |De begintijd van de eerste gebeurtenis in "tikken." |
| duur |De lengte van het fragment, in "tikken." |
| interval |Het interval van elke vermelding in het fragment, in "tikken." |
| [gebeurtenissen](#events-json-elements) |Elke gebeurtenis stelt een clip en elke clip hoofdframes gedetecteerd en bijgehouden binnen deze tijdsduur bevat. Er is een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De binnenste matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden gedurende de interval.|

### <a name="events-json-elements"></a>Gebeurtenissen JSON-elementen

|Element|Description|
|---|---|
| reviewRecommended | `true` of `false` afhankelijk van of de **adultScore** of **racyScore** de interne drempelwaarden overschrijden. |
| adultScore | De betrouwbaarheidsscore van mogelijk pornografisch materiaal op een schaal van 0,00 en 0,99. |
| racyScore | Betrouwbaarheidsscore voor mogelijke ongepaste inhoud, klikt u op een schaal van 0,00 en 0,99. |
| index | index van het kader op een schaal van de eerste frame index met de laatste frame-index. |
| tijdstempel | De locatie van het kader in "tikken." |
| shotIndex | De index van het bovenliggende object opgenomen. |


## <a name="content-moderation-quickstart-and-sample-output"></a>De uitvoer van de Quick Start en voorbeeld van de toezicht op inhoud

### <a name="task-configuration-preset"></a>Taakconfiguratie (standaardoptie)
Bij het maken van een taak met **Azure Media Content Moderator**, moet u een configuratie-definitie opgeven. De volgende configuratie-voorinstelling is alleen voor inhoudstoezicht.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>.NET-codevoorbeeld dat

De volgende .NET-voorbeeldcode maakt gebruik van de Media Services .NET SDK een Content Moderator-taak uit te voeren. Het duurt een media services actief als de invoer met de video om te worden gecontroleerd.
Zie de [Content Moderator video snelstartgids](../../cognitive-services/Content-Moderator/video-moderation-api.md) voor de volledige broncode en het Visual Studio-project.


```csharp
    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
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
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

For the full source code and the Visual Studio project, check out the [Content Moderator video quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md).

### JSON output

The following example of a Content Moderator JSON output was truncated.

> [!NOTE]
> Location of a keyframe in seconds = timestamp/timescale

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }
```

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Content Moderator [video oplossing voor beheer en controle](../../cognitive-services/Content-Moderator/video-moderation-human-review.md).

Lees de volledige broncode en het Visual Studio-project van de [videotoezicht snelstartgids](../../cognitive-services/Content-Moderator/video-moderation-api.md). 

Meer informatie over het genereren van [video bekijkt](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) van de gecontroleerde uitvoer en [modereren Transcripten](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) in .NET.

Bekijk de gedetailleerde .NET [video met zelfstudie beheer en controle](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md). 
