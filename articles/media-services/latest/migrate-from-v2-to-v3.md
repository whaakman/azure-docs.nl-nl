---
title: Migreren van Azure Media Services-v2 aan v3 | Microsoft Docs
description: Dit artikel worden wijzigingen die zijn geïntroduceerd in Azure Media Services v3 beschreven en ziet u verschillen tussen twee versies.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 06/12/2018
ms.author: juliako
ms.openlocfilehash: a382af644d30f9f0ebb586273c982ef1766f50b0
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295685"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migreren van Media Services-v2 aan v3

> [!NOTE]
> De nieuwste versie van Azure Media Services is momenteel in preview en wordt ook wel v3 of versie 3 genoemd.

Dit artikel beschrijft de wijzigingen die zijn geïntroduceerd in Azure Media Services (AMS) v3 en verschillen tussen twee versies bevat.

## <a name="why-should-you-move-to-v3"></a>Waarom moet u naar v3 verplaatsen?

### <a name="api-is-more-approachable"></a>API is meer gebruiksvriendelijke

*  V3 is gebaseerd op een geïntegreerde API-gebied die zowel beheer van en bewerkingen functionaliteit die zijn gebouwd op Azure Resource Manager. Azure Resource Manager-sjablonen kunnen maken en implementeren van transformaties, Streaming-eindpunten en LiveEvents worden gebruikt.
* Open API (aka Swagger) specificatie van het document.
* SDK's die beschikbaar zijn voor .net, .net Core, Node.js, Python, Java, Ruby.
* Integratie van Azure CLI.

### <a name="new-features"></a>Nieuwe functies

* Codering biedt nu ondersteuning voor HTTPS (op basis van een Url-invoer) opnemen.
* Transformaties zijn nieuw in v3. Een transformatie wordt gebruikt voor het delen van configuraties, Azure Resource Manager-sjablonen maken en isoleren coderingsinstellingen voor een specifieke klant of tenant. 
* Een Asset kan meerdere StreamingLocators hebben met verschillende instellingen voor dynamische pakketten en dynamische versleuteling.
* Beveiliging van inhoud biedt ondersteuning voor meerdere belangrijke functies. 
* LiveEvent Preview biedt ondersteuning voor dynamische pakketten en dynamische versleuteling. Hierdoor kunnen de beveiliging van inhoud op Preview en DASH- en HLS verpakking.
* LiveOuput is eenvoudiger om te gebruiken dan de oudere programma entiteit. 
* RBAC-ondersteuning voor entiteiten is toegevoegd.

## <a name="changes-from-v2"></a>Wijzigingen van v2

* In Media Services-v3 versleuteling van opslag (AES-256-versleuteling) wordt alleen ondersteund voor compatibiliteit met eerdere wanneer uw Assets met Media Services-v2 zijn gemaakt. Dit betekent dat v3 werkt met bestaande opslag activa versleuteld, maar staat niet toe dat het maken van nieuwe activiteiten.

    Voor de activa met v3 gemaakt, Media Services ondersteunt de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) serverversleuteling opslag.
    
* Media Services SDK's ontkoppeld van de opslag-SDK die u meer controle over de opslag-SDK hebt gebruikt en versiebeheer problemen voorkomt. 
* V3 zijn alle van de codering bitsnelheden in bits per seconde. Dit is anders dan de REST-v2 die Media Encoder Standard voorinstellingen. Bijvoorbeeld, de bitrate in v2 zou worden opgegeven als 128, maar in v3-128000 zou zijn. 
* AssetFiles, AccessPolicies, IngestManifests bestaan niet in v3.
* ContentKeys zijn niet langer een entiteit, eigenschap van de StreamingLocator.
* Gebeurtenis raster ondersteuning vervangt NotificationEndpoints.
* Sommige entiteiten zijn gewijzigd

  * JobOutput vervangt taak nu alleen deel uitmaken van de taak.
  * LiveEvent vervangt kanaal.
  * LiveOutput vervangt programma.
  * StreamingLocator vervangt Locator.

## <a name="code-changes"></a>Codewijzigingen

### <a name="create-an-asset-and-upload-a-file"></a>Een asset maken en uploaden van een bestand 

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

### <a name="submit-a-job"></a>Verzenden van een taak

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
4. Asset maken en uploaden van inhoud of Submit job en uitvoerasset gebruiken
5. AssetDeliveryPolicy koppelen aan activum
6. ContentKey maken
7. ContentKey koppelen aan Asset
8. AccessPolicy maken
9. Locator maken

#### <a name="v3"></a>v3

1. Beleid voor inhoud sleutels maken
2. Asset maken
3. Inhoud uploaden of Asset gebruiken als JobOutput
4. StreamingLocator maken

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe eenvoudig het is om videobestanden te coderen en streamen, gaat u naar [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md). 

