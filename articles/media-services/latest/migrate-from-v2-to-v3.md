---
title: Migreren van Azure Media Services v2 naar v3 | Microsoft Docs
description: Dit artikel worden wijzigingen die zijn geïntroduceerd in Azure Media Services v3 en ziet u de verschillen tussen twee versies.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: a17bad5beb651aaa085c626296c200a00c30647e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376359"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migreren van Media Services v2 naar v3

In dit artikel worden wijzigingen die zijn geïntroduceerd in Azure Media Services (AMS) v3 beschreven en ziet u de verschillen tussen twee versies.

## <a name="why-should-you-move-to-v3"></a>Waarom moet u verplaatsen naar v3?

### <a name="api-is-more-approachable"></a>API is meer toegankelijk

*  V3 is gebaseerd op een uniforme API-oppervlak dat beheer- en operations functionaliteit gebouwd op Azure Resource Manager bevat. Azure Resource Manager-sjablonen kunnen maken en implementeren van transformaties, Streaming-eindpunten en LiveEvents worden gebruikt.
* Open API (ook wel Swagger)-specificatiedocument.
* SDK's beschikbaar voor .net, .net Core, Node.js, Python, Java, Ruby.
* Integratie van Azure CLI.

### <a name="new-features"></a>Nieuwe functies

* Codering biedt nu ondersteuning voor HTTPS (op basis van Url-invoer) opnemen.
* Transformaties zijn nieuw in v3. Een transformatie wordt gebruikt voor het delen van configuraties, Azure Resource Manager-sjablonen maken en isoleren encoding-instellingen voor een specifieke klant of tenant. 
* Een Asset kan meerdere StreamingLocators hebben met verschillende instellingen voor dynamische pakketten en dynamische versleuteling.
* Beveiliging van inhoud biedt ondersteuning voor meerdere belangrijke functies. 
* LiveEvent Preview biedt ondersteuning voor dynamische pakketten en dynamische versleuteling. Hierdoor kunnen de beveiliging van inhoud op Preview-versie, evenals DASH en HLS-verpakking.
* LiveOuput is eenvoudiger dan de oudere programma-entiteit. 
* RBAC-ondersteuning voor entiteiten is toegevoegd.

## <a name="changes-from-v2"></a>Wijzigingen van v2

* In Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

    Voor de activa die zijn gemaakt met v3, Media Services ondersteunt de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) opslagversleuteling aan de serverzijde.
    
* Media Services SDK's ontkoppeld van de Storage-SDK die u meer controle over de Storage-SDK hebt gebruikt en voorkomt u problemen met versiebeheer. 
* In v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit is anders dan de REST-v2 voorinstellingen van Media Encoder Standard. Bijvoorbeeld, de bitrate in v2 zou worden opgegeven als 128, maar in v3 128000 zou zijn. 
* IngestManifests AssetFiles, AccessPolicies, bestaan niet in v3.
* Inhoudssleutels zijn niet langer een entiteit, een eigenschap van de StreamingLocator.
* Ondersteuning voor Event Grid vervangt NotificationEndpoints.
* Sommige entiteiten zijn gewijzigd

  * JobOutput vervangt taak nu alleen deel uitmaken van de taak.
  * LiveEvent vervangt kanaal.
  * LiveOutput vervangt programma.
  * StreamingLocator vervangt Locator.

## <a name="code-changes"></a>Codewijzigingen

### <a name="create-an-asset-and-upload-a-file"></a>Maak een asset en upload een bestand 

#### <a name="v2"></a>v2

```csharp
IAsset asset = context.Assets.Create(assetName, storageAccountName, options);

IAssetFile assetFile = asset.AssetFiles.Create(assetFileName);

assetFile.Upload(filePath);
```

#### <a name="v3"></a>v3

```csharp
Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, permissions: AssetContainerPermission.ReadWrite, expiryTime: DateTime.Now.AddHours(1));

var sasUri = new Uri(response.AssetContainerSasUrls.First());
CloudBlobContainer container = new CloudBlobContainer(sasUri);

var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
blob.UploadFromFile(fileToUpload);
```

### <a name="submit-a-job"></a>Een taak verzenden

#### <a name="v2"></a>v2

```csharp
IMediaProcessor processor = context.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

IJob job = jobs.Create($"Job for {inputAsset.Name}");

ITask task = job.Tasks.AddNew($"Task for {inputAsset.Name}", processor, taskConfiguration);

task.InputAssets.Add(inputAsset);

task.OutputAssets.AddNew(outputAssetName, outputAssetStorageAccountName, outputAssetOptions);

job.Submit();
```

#### <a name="v3"></a>v3

```csharp
client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, new Asset());

JobOutput[] jobOutputs = { new JobOutputAsset(outputAssetName)};

JobInput jobInput = JobInputAsset(assetName: assetName);

Job job = client.Jobs.Create(resourceGroupName,
accountName, transformName, jobName,
new Job {Input = jobInput, Outputs = jobOutputs});
```

### <a name="publish-an-asset-with-aes-encryption"></a>Publiceren van een asset met AES-versleuteling 

#### <a name="v2"></a>v2

1. ContentKeyAuthorizationPolicyOption maken
2. ContentKeyAuthorizationPolicy maken
3. AssetDeliveryPolicy maken
4. Asset maken en uploaden van inhoud taak of verzenden en uitvoerasset gebruiken
5. AssetDeliveryPolicy koppelen aan Asset
6. ContentKey maken
7. ContentKey koppelen aan Asset
8. AccessPolicy maken
9. Locator maken

#### <a name="v3"></a>v3

1. Beleid voor inhoud sleutels maken
2. Asset maken
3. Inhoud uploaden of activa zoals JobOutput
4. StreamingLocator maken

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe eenvoudig het is om videobestanden te coderen en streamen, gaat u naar [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md). 

