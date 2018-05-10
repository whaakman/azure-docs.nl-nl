---
title: De invoer van een Azure Media Services-taak maken op basis van een URL http (s) | Microsoft Docs
description: Dit onderwerp leest het maken van een taak voor invoer van de URL van een HTTP (s).
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: a47dc3e4939dd27d7ac11be823a09b8a6cba1f60
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Maken van een taak voor invoer van de URL van een HTTP (s)

In Media Services-v3 bij het verzenden van taken voor het verwerken van uw video's, hebt u Media Services uitgelegd waar vind ik het invoervideo. Een van de opties is een HTTP (s)-URL opgeven als een taak invoer (zoals weergegeven in dit voorbeeld). Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een taak te maken met een HTTPS-URL invoeren.

```csharp
private static Job SubmitJob(IAzureMediaServicesClient client, string transformName, string jobName)
{
    Asset outputAsset = client.Assets.CreateOrUpdate(Guid.NewGuid().ToString() + "-output", new Asset());

    JobInputHttp jobInput = 
        new JobInputHttp(files: new[] { "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4" });

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAsset.Name),
    };

    Job job = client.Jobs.CreateOrUpdate(
        transformName,
        jobName,
        new Job
        {
            Input = jobInput,
            Outputs = jobOutputs,
        });

    return job;
}
```

## <a name="next-steps"></a>Volgende stappen

[De invoer van een taak maken op basis van een lokaal bestand](job-input-from-local-file-how-to.md).
