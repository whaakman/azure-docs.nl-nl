---
title: Media Services-activa downloaden naar uw computer - Azure | Microsoft Docs
description: Meer informatie over het downloaden van activa op uw computer. Codevoorbeelden zijn geschreven in C# en de Media Services SDK voor .NET gebruiken.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 21fcc6ae09718ffbb22e1d438926586dd3cde71d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182656"
---
# <a name="how-to-deliver-an-asset-by-download"></a>Procedure: Een asset leveren via download  
Dit artikel worden de opties voor het leveren van media-activa geüpload naar Media Services. Media Services-inhoud kan worden geleverd in verschillende toepassingsscenario's. Na codering, downloadt u de gegenereerde media-activa of toegang tot deze met behulp van een streaming-locator. Voor betere prestaties en schaalbaarheid, kunt u ook inhoud leveren met behulp van een Content Delivery Network (CDN).

In dit voorbeeld laat zien hoe media-activa van Media Services downloaden naar uw lokale computer. De code vraagt de taken die zijn gekoppeld aan de Media Services-account door de taak-ID en toegang tot de **OutputMediaAssets** verzameling (dit is het instellen van een of meer uitvoer media-activa die het resultaat is van een taak wordt uitgevoerd). Dit voorbeeld laat zien hoe u kunt uitvoer media-activa downloaden uit een taak, maar u kunt dezelfde benadering voor het downloaden van andere items toepassen.

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). Gebruik dezelfde beleids-ID als u altijd dezelfde dagen / toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om te blijven aanwezig gedurende een lange periode (niet-uploadbeleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.

```csharp
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }
```


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Lever streaminginhoud](media-services-deliver-streaming-content.md)

