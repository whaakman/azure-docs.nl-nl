---
title: De invoer van een Azure Media Services-taak maken op basis van een lokaal bestand | Microsoft Docs
description: Dit onderwerp leest het maken van een taak voor invoer van een lokaal bestand.
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
ms.openlocfilehash: 6ca2da7fc43e509bec357ae47ada3a2f57e79e78
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-job-input-from-a-local-file"></a>De invoer van een taak maken op basis van een lokaal bestand

In Media Services-v3 bij het verzenden van taken voor het verwerken van uw video's, hebt u Media Services uitgelegd waar vind ik het invoervideo. De invoer video kan worden opgeslagen als een activum Media-Service in dat geval het maken van een invoer asset op basis van een bestand (lokaal of in Azure Blob-opslag is opgeslagen). Dit onderwerp leest het maken van een taak voor invoer van een lokaal bestand. Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een invoer-activum maken en deze gebruiken als invoer voor de taak. De functie CreateInputAsset voert de volgende acties:

* De Asset gemaakt 
* Haalt een beschrijfbare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) voor de activa [container in de opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container)
* Het bestand uploadt naar de container in opslag met SAS-URL

```csharp
private static Asset CreateInputAsset(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string assetName, string fileToUpload)
{
    // Call Media Services API to create an Asset.
    // This method creates a container in storage for the Asset.
    // The files (blobs) associated with the asset will be stored in this container.
    Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

    // Use Media Services API to get back a response that contains
    // SAS URL for the Asset container into which to upload blobs.
    // That is where you would specify read-write permissions 
    // and the exparation time for the SAS URL.
    var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, new ListContainerSasInput()
    {
        Permissions = AssetContainerPermission.ReadWrite,
        ExpiryTime = DateTimeOffset.Now.AddHours(4)
    });

    var sasUri = new Uri(response.AssetContainerSasUrls.First());
    
    // Use Storage API to get a reference to the Asset container
    // that was created by calling Asset's CreateOrUpdate method.  
    CloudBlobContainer container = new CloudBlobContainer(sasUri);
    var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
    
    // Use Strorage API to upload the file into the container in storage.
    blob.UploadFromFile(fileToUpload);

    return asset;
}

private static Job SubmitJob(IAzureMediaServicesClient client, string transformName, string jobName, string outputAssetName)
{
    string inputAssetName = Guid.NewGuid().ToString() + "-input";
    JobInput jobInput = new JobInputAsset(assetName: inputAssetName);

    CreateInputAsset(client, inputAssetName, "ignite.mp4")

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAssetName),
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

[Maken van een taak voor invoer van de URL van een HTTP (s)](job-input-from-http-how-to.md).
