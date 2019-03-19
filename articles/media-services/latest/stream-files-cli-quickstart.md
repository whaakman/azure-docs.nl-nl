---
title: Stream-video's met Azure Media Services en de Azure CLI | Microsoft Docs
description: Volg de stappen in deze snelstartgids om een nieuw Azure Media Services-account te maken, een bestand te coderen en dit vervolgens te streamen naar Azure Media Player.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: ''
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: a323cbe4188207fa77525648297b366c9c57121b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244720"
---
# <a name="quickstart-stream-video-files---cli"></a>Quickstart: Videobestanden streamen - CLI

In deze Quick Start laat zien hoe eenvoudig coderen en streamen van video's op een aantal verschillende browsers en apparaten met behulp van Azure Media Services en de Azure CLI. U kunt invoerinhoud opgeven met behulp van HTTPS of SAS-URL's of paden naar bestanden in Azure Blob-opslag.

In het voorbeeld in dit artikel worden de inhoud die u toegankelijk zijn via een HTTPS-URL gecodeerd. Media Services v3 ondersteunt momenteel geen gesegmenteerde overdrachtscodering via HTTPS-URL's.

Aan het einde van deze Quick Start zult u een video te streamen.  

![De video afspelen](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-media-services-account"></a>Een Media Services-account kunt maken

Voordat u kunt versleutelen, coderen, analyseren, beheren en streamen van media-inhoud in Azure, moet u een Media Services-account maken. Dit account moet worden gekoppeld aan een of meer opslagaccounts.

Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement. U wordt aangeraden dat u het storage-accounts die zich in dezelfde locatie als het Media Services-account om te beperken van de kosten voor uitgaand latentie en gegevens.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

```azurecli
az group create -n amsResourceGroup -l westus2
```

### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

In dit voorbeeld maken we een voor algemeen gebruik v2 Standard LRS-account.

Als u wilt experimenteren met opslagaccounts, gebruikt u `--sku Standard_LRS`. Wanneer u een SKU voor de productie hebt gekozen, kunt u overwegen `--sku Standard_RAGRS`, waarmee u geografische replicatie voor bedrijfscontinuïteit. Zie [Opslagaccounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest) voor meer informatie.
 
```azurecli
az storage account create -n amsstorageaccount --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```

### <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

```azurecli
az ams account create --n amsaccount -g amsResourceGroup --storage-account amsstorageaccount -l westus2
```

U krijgt een reactie als volgt:

```
{
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "location": "West US 2",
  "mediaServiceId": "8b569c2e-d648-4fcb-9035-c7fcc3aa7ddf",
  "name": "amsaccount",
  "resourceGroup": "amsResourceGroupTest",
  "storageAccounts": [
    {
      "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Storage/storageAccounts/amsstorageaccount",
      "resourceGroup": "amsResourceGroupTest",
      "type": "Primary"
    }
  ],
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```

## <a name="start-the-streaming-endpoint"></a>Het streaming-eindpunt starten

De volgende Azure CLI-opdracht start de standaardwaarde **Sstreaming eindpunt**.

```azurecli
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
```

U krijgt een reactie als volgt:

```
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
{
  "accessControl": null,
  "availabilitySetName": null,
  "cdnEnabled": true,
  "cdnProfile": "AzureMediaStreamingPlatformCdnProfile-StandardVerizon",
  "cdnProvider": "StandardVerizon",
  "created": "2019-02-06T21:58:03.604954+00:00",
  "crossSiteAccessPolicies": null,
  "customHostNames": [],
  "description": "",
  "freeTrialEndTime": "2019-02-21T22:05:31.277936+00:00",
  "hostName": "amsaccount-usw22.streaming.media.azure.net",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingendpoints/default",
  "lastModified": "2019-02-06T21:58:03.604954+00:00",
  "location": "West US 2",
  "maxCacheAge": null,
  "name": "default",
  "provisioningState": "Succeeded",
  "resourceGroup": "amsResourceGroup",
  "resourceState": "Running",
  "scaleUnits": 0,
  "tags": {},
  "type": "Microsoft.Media/mediaservices/streamingEndpoints"
}
```

Als het streaming-eindpunt wordt al uitgevoerd, krijgt u dit bericht:

```
(InvalidOperation) The server cannot execute the operation in its current state.
```

## <a name="create-a-transform-for-adaptive-bitrate-encoding"></a>Maak een transformatie voor adaptieve bitrate codering

Maak een **Transformatie** om veelvoorkomende taken voor het coderen of analyseren van video's te configureren. In dit voorbeeld doen we adaptieve bitrate codering. Vervolgens verzenden we een taak onder de transformatie die we hebben gemaakt. De taak is het verzoek aan Media Services om de transformatie toepassen op de opgegeven video of audio-inhoud invoer.

```azurecli
az ams transform create --name testEncodingTransform --preset AdaptiveStreaming --description 'a simple Transform for Adaptive Bitrate Encoding' -g amsResourceGroup -a amsaccount
```

U krijgt een reactie als volgt:

```
{
  "created": "2019-02-15T00:11:18.506019+00:00",
  "description": "a simple Transform for Adaptive Bitrate Encoding",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform",
  "lastModified": "2019-02-15T00:11:18.506019+00:00",
  "name": "testEncodingTransform",
  "outputs": [
    {
      "onError": "StopProcessingJob",
      "preset": {
        "odatatype": "#Microsoft.Media.BuiltInStandardEncoderPreset",
        "presetName": "AdaptiveStreaming"
      },
      "relativePriority": "Normal"
    }
  ],
  "resourceGroup": "amsResourceGroup",
  "type": "Microsoft.Media/mediaservices/transforms"
}
```

## <a name="create-an-output-asset"></a>Een uitvoeractivum maken

Uitvoer maken **Asset** als u de uitvoer van de coderingstaak wilt gebruiken.

```azurecli
az ams asset create -n testOutputAssetName -a amsaccount -g amsResourceGroup
```

U krijgt een reactie als volgt:

```
{
  "alternateId": null,
  "assetId": "96427438-bbce-4a74-ba91-e38179b72f36",
  "container": null,
  "created": "2019-02-14T23:58:19.127000+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/assets/testOutputAssetName",
  "lastModified": "2019-02-14T23:58:19.127000+00:00",
  "name": "testOutputAssetName",
  "resourceGroup": "amsResourceGroup",
  "storageAccountName": "amsstorageaccount",
  "storageEncryptionFormat": "None",
  "type": "Microsoft.Media/mediaservices/assets"
}
```

## <a name="start-a-job-by-using-https-input"></a>Start een taak met behulp van HTTPS-invoer

Wanneer u taken voor het verwerken van video's verzenden, hebt u Media Services zien waar vind ik de invoervideo. Een optie is om op te geven van een HTTPS-URL als de Taakinvoer, zoals wordt weergegeven in dit voorbeeld.

Wanneer u `az ams job start` uitvoert, kunt u een label instellen op de uitvoer van de taak. U kunt het label vervolgens gebruiken om te bepalen wat de uitvoerasset voor is.

- Als u een waarde aan het label toewijst, stelt u '---uitvoerassets naar ' assetname label = ".
- Als u niet een waarde aan het label toewijst, stelt u '---uitvoerassets naar ' assetname = ".

  U ziet dat we "=" toevoegen aan de `output-assets`.

```azurecli
az ams job start --name testJob001 --transform-name testEncodingTransform --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' --files 'Ignite-short.mp4' --output-assets testOutputAssetName= -a amsaccount -g amsResourceGroup 
```

U krijgt een reactie als volgt:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

### <a name="check-status"></a>Status controleren

Controleer de status van de taak in vijf minuten. Deze moet worden "is voltooid." Het niet voltooid, controleert u het over enkele minuten opnieuw. Als deze voltooid, gaat u naar de volgende stap en maakt u een **Streaming-Locator gemaakt**.

```azurecli
az ams job show -a amsaccount -g amsResourceGroup -t testEncodingTransform -n testJob001
```

## <a name="create-a-streaming-locator-and-get-a-path"></a>Een streaming-locator te maken en een pad ophalen

Wanneer de codering is voltooid, bestaat de volgende stap eruit om de video in de uitvoerasset beschikbaar te maken voor weergave door clients. Om dit te doen, maakt u eerst een Streaming-Locator gemaakt. Vervolgens kunt samenstellen streaming-URL's die clients kunnen gebruiken.

### <a name="create-a-streaming-locator"></a>Een streaming-locator te maken

```azurecli
az ams streaming-locator create -n testStreamingLocator --asset-name testOutputAssetName --streaming-policy-name Predefined_ClearStreamingOnly  -g amsResourceGroup -a amsaccount 
```

U krijgt een reactie als volgt:

```
{
  "alternativeMediaId": null,
  "assetName": "output-3b6d7b1dffe9419fa104b952f7f6ab76",
  "contentKeys": [],
  "created": "2019-02-15T04:35:46.270750+00:00",
  "defaultContentKeyPolicyName": null,
  "endTime": "9999-12-31T23:59:59.999999+00:00",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingLocators/testStreamingLocator",
  "name": "testStreamingLocator",
  "resourceGroup": "amsResourceGroup",
  "startTime": null,
  "streamingLocatorId": "e01b2be1-5ea4-42ca-ae5d-7fe704a5962f",
  "streamingPolicyName": "Predefined_ClearStreamingOnly",
  "type": "Microsoft.Media/mediaservices/streamingLocators"
}
```

### <a name="get-streaming-locator-paths"></a>Streaming-locator paden ophalen

```azurecli
az ams streaming-locator get-paths -a amsaccount -g amsResourceGroup -n testStreamingLocator
```

U krijgt een reactie als volgt:

```
{
  "downloadPaths": [],
  "streamingPaths": [
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)"
      ],
      "streamingProtocol": "Hls"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=mpd-time-csf)"
      ],
      "streamingProtocol": "Dash"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest"
      ],
      "streamingProtocol": "SmoothStreaming"
    }
  ]
}
```

Kopieer de HTTP live streaming (HLS)-pad. In dit geval heeft `/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)`.

## <a name="build-the-url"></a>De URL maken 

### <a name="get-the-streaming-endpoint-host-name"></a>De naam van de streaming ophalen

```azurecli
az ams streaming-endpoint list -a amsaccount -g amsResourceGroup -n default
```
Kopieer de waarde van `hostName`. In dit geval heeft `amsaccount-usw22.streaming.media.azure.net`.

### <a name="assemble-the-url"></a>De URL samen te stellen

'https://' + &lt;waarde van hostnaam&gt; + &lt;padwaarde Hls&gt;

Hier volgt een voorbeeld:

`https://amsaccount-usw22.streaming.media.azure.net/7f19e783-927b-4e0a-a1c0-8a140c49856c/ignite.ism/manifest(format=m3u8-aapl)`

## <a name="test-playback-by-using-azure-media-player"></a>Afspelen testen met behulp van Azure Media Player

> [!NOTE]
> Als een speler wordt gehost op een HTTPS-site, zorg ervoor dat de URL met 'https' start.

1. Open een webbrowser en Ga naar [ https://aka.ms/azuremediaplayer/ ](https://aka.ms/azuremediaplayer/).
2. In de **URL** vak, plak de URL die u in de vorige sectie hebt gemaakt. U kunt de URL plakken in HLS, Dash of Smooth indeling. Azure Media Player wordt automatisch een juiste streaming-protocol gebruiken om te worden afgespeeld op uw apparaat.
3. Selecteer **Player bijwerken**.

>[!NOTE]
>Azure Media Player kan worden gebruikt voor testdoeleinden, maar mag niet worden gebruikt in een productieomgeving.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet meer nodig van de resources in de resourcegroep hebt, met inbegrip van de Media Services en storage-accounts die u hebt gemaakt voor deze Quick Start, verwijdert u de resourcegroep.

Dit CLI-opdracht uitvoeren:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Zie ook

Zie [Foutcodes in taak](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Volgende stappen

> [CLI-voorbeelden](cli-samples.md)
