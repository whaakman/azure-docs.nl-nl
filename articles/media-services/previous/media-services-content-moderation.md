---
title: Azure Media-inhoud beheerder gebruiken voor het detecteren van mogelijke inhoud voor volwassenen en mooie | Microsoft Docs
description: Video controle helpt bij het detecteren van mogelijke inhoud voor volwassenen en mooie in video's.
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
ms.date: 02/06/2018
ms.author: sajagtap
ms.openlocfilehash: e44308f38a138c0e186e41fc8310f8b480cd4e09
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Azure Media-inhoud beheerder gebruiken voor het detecteren van mogelijke inhoud voor volwassenen en mooie

## <a name="overview"></a>Overzicht
De **Azure Media-inhoud beheerder** Mediaprocessor (MP) kunt u gebruikmaken van toezicht machine ondersteund voor uw video's. U wilt bijvoorbeeld mogelijk inhoud voor volwassenen en mooie detecteren in video's en controleer de gemarkeerde inhoud van uw menselijke toezicht.

De **Azure Media-inhoud beheerder** MP is momenteel in Preview.

In dit artikel geeft informatie over **Azure Media-inhoud beheerder** en laat zien hoe u deze gebruiken met Media Services SDK voor .NET.

## <a name="content-moderator-input-files"></a>Inhoud van de invoerbestanden beheerder
Videobestanden. Op dit moment wordt de volgende indelingen worden ondersteund: MP4 MOV en WMV.

## <a name="content-moderator-output-files"></a>Beheerder uitvoer inhoudsbestanden
De uitvoer van de gecontroleerde in de JSON-indeling bevat schermafbeeldingen automatisch gedetecteerd en keyframes. De keyframes worden met vertrouwen scores voor mogelijke inhoud voor volwassenen of mooie geretourneerd. Ze bevatten ook een Booleaanse vlag die aangeeft of een beoordeling wordt aanbevolen. De aanbeveling revisie vlag is toegewezen op basis van de interne drempelwaarden voor volwassenen en mooie scores waarden.

## <a name="elements-of-the-output-json-file"></a>Elementen van de JSON-bestand voor uitvoer

De taak wordt een JSON-uitvoer-bestand met metagegevens over gedetecteerde schermafbeeldingen en keyframes en dat ze de inhoud voor volwassenen of mooie bevatten.

De JSON-uitvoer bevat de volgende elementen:

### <a name="root-json-elements"></a>Hoofdmap JSON-elementen

| Element | Beschrijving |
| --- | --- |
| versie |De versie van de beheerder van inhoud. |
| Tijdschaal |'Maatstreepjes' per seconde van de video. |
| offset |De time-offset voor tijdstempels. Versie 1.0 van Video-API's, wordt deze waarde altijd 0 zijn. Deze waarde in de toekomst mogelijk gewijzigd. |
| framesnelheid |Frames per seconde van de video. |
| Breedte |De breedte van de video frame van uitvoer in pixels.|
| Hoogte |De hoogte van de video frame van uitvoer in pixels.|
| TotalDuration |De duur van de invoer-video in "tikken." |
| [fragmenten](#fragments-json-elements) |De metagegevens wordt gesegmenteerde in verschillende segmenten fragmenten aangeroepen. Elke fragment is een automatische detectie gemaakt met een begindatum, duur, Intervalnummer en gebeurtenis(sen). |

### <a name="fragments-json-elements"></a>Fragmenten JSON-elementen

|Element|Beschrijving|
|---|---|
| start |De begintijd van de eerste gebeurtenis in "tikken." |
| Duur |De lengte van het fragment, in "tikken." |
| interval |Het interval van elke gebeurtenisvermelding in het fragment, in "tikken." |
| [Gebeurtenissen](#events-json-elements) |Elke gebeurtenis geeft een illustratie en elke clip keyframes gedetecteerd en bijgehouden in die tijd bevat. Er is een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De interne matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden op dat moment.|

### <a name="events-json-elements"></a>JSON-elementen met gebeurtenissen

|Element|Beschrijving|
|---|---|
| reviewRecommended | `true` of `false` , afhankelijk van of de **adultScore** of **racyScore** interne drempelwaarden overschrijden. |
| adultScore | Score vertrouwen voor mogelijke inhoud voor volwassenen, op een schaal van 0,00 en 0,99. |
| racyScore | Score vertrouwen voor mogelijke mooie inhoud, klikt u op een schaal van 0,00 en 0,99. |
| index | index van het frame op een schaal van het eerste frame index op de laatste frame-index. |
| tijdstempel | De locatie van het frame in "tikken." |
| shotIndex | De index van het bovenliggende object opgenomen. |


## <a name="content-moderation-quickstart-and-sample-output"></a>De uitvoer van de Quick Start- en voorbeeld van de controle van inhoud

### <a name="task-configuration-preset"></a>Taken configureren (standaardoptie)
Bij het maken van een taak met **Azure Media-inhoud beheerder**, moet u een configuratie-definitie opgeven. De volgende configuratie-definitie is slechts voor inhoud toezicht.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>.NET-codevoorbeeld

Het volgende codevoorbeeld voor .NET gebruikt Media Services .NET SDK voor het uitvoeren van een taak inhoud beheerder. Het duurt een media services Asset als invoer met de video om te worden gecontroleerd.
Zie de [video Quick Start inhoud beheerder](../../cognitive-services/Content-Moderator/video-moderation-api.md) voor de volledige broncode en het Visual Studio-project.


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

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over inhoud beheerder [video-interrupts en bekijk oplossing](../../cognitive-services/Content-Moderator/video-moderation-human-review.md).

De volledige broncode en Visual Studio-project uit de [video toezicht Quick Start](../../cognitive-services/Content-Moderator/video-moderation-api.md). 

Meer informatie over het genereren van [video bekijkt](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) van uw uitvoer met toezicht en [transcripties gemiddeld](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) in .NET.

Bekijk de gedetailleerde .NET [video toezicht en bekijk zelfstudie](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md). 
