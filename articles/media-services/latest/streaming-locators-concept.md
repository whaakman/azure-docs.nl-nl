---
title: Streaming-Locators in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat Streaming-Locators zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/26/2019
ms.author: juliako
ms.openlocfilehash: 5897b7df2460257784c40eb974c473573ec4003d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299170"
---
# <a name="streaming-locators"></a>Streaming-locators

Om video's in de uitvoerasset beschikbaar te maken voor clients om af te spelen, moet u een [streaming-locator](https://docs.microsoft.com/rest/api/media/streaminglocators) maken en streaming-URL's maken. Voor het bouwen van een URL, moet u de Streaming-eindpunt hostnaam en het pad van de Streaming-Locator gemaakt samenvoegen. Zie [Een streaming-locator ophalen](stream-files-tutorial-with-api.md#get-a-streaming-locator) voor een voorbeeld met .NET.

Het proces van het maken van een **streaming-locator** wordt publiceren genoemd. De **streaming-locator** is standaard onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een **Streaming-Locator gemaakt**, moet u een **Asset** naam en een **beleid Streaming** naam. Zie de volgende onderwerpen voor meer informatie:

* [Activa](assets-concept.md)
* [Beleid voor streaming](streaming-policy-concept.md)
* [Beleid voor inhoudssleutels](content-key-policy-concept.md)

U kunt ook opgeven de begin- en tijd op uw Streaming-Locator gemaakt, waarmee de gebruiker alleen de inhoud tussen deze tijden (bijvoorbeeld tussen 5/1/2019 aan 5/5/2019) af te spelen.  

## <a name="considerations"></a>Overwegingen

* **Streaming-Locators** kunnen niet worden bijgewerkt. 
* Eigenschappen van **Streaming-Locators** die van de datum/tijd zijn altijd in UTC-notatie zijn.
* U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. Zie voor meer informatie, [quota en beperkingen](limits-quotas-constraints.md).

## <a name="create-streaming-locators"></a>Streaming-Locators maken  

### <a name="not-encrypted"></a>Niet-versleuteld

Als u wilt het streamen van uw bestand in-the-wissen (niet-versleutelde), stelt u de vooraf gedefinieerde duidelijk streaming beleid: naar 'Predefined_ClearStreamingOnly' (in .NET, kunt u de enum PredefinedStreamingPolicy.ClearStreamingOnly).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Versleuteld 

Als u nodig hebt voor het versleutelen van uw inhoud met de CENC-versleuteling, moet u het beleid aan 'Predefined_MultiDrmCencStreaming' instellen. De Widevine-versleuteling wordt aan een streepje stream en PlayReady worden toegepast op Smooth. De sleutel wordt aan een afspelen-client op basis van de geconfigureerde DRM-licenties worden geleverd.

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Als u wilt dat ook voor het versleutelen van uw HLS-stream met CBCS (FairPlay), gebruikt u 'Predefined_MultiDrmStreaming'.

## <a name="associate-filters-with-streaming-locators"></a>Filters met Streaming-Locators koppelen

Zie [Filters: koppelen met Streaming-Locators](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filter, de volgorde, de pagina Streaming-Locator gemaakt entiteiten

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="list-streaming-locators-by-asset-name"></a>Lijst met Streaming-Locators met de naam van de Asset

Voor Streaming-Locators die zijn gebaseerd op de naam van de gekoppelde Asset, gebruikt u de volgende bewerkingen:

|Taal|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|CLI|[az ams asset list-streaming-locators](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[ListStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[AssetStreamingLocator](https://docs.microsoft.com/java/api/com.microsoft.azure.management.mediaservices.v2018_07_01.assetstreaminglocator?view=azure-java-stable)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/assets?view=azure-node-latest#liststreaminglocators-string--string--string--object-)|

## <a name="also-see"></a>Zie ook

* [Activa](assets-concept.md)
* [Beleid voor streaming](streaming-policy-concept.md)
* [Beleid voor inhoudssleutels](content-key-policy-concept.md)

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Video’s uploaden, coderen en streamen met behulp van .NET](stream-files-tutorial-with-api.md)
