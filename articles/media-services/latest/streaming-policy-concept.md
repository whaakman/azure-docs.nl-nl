---
title: Streaming-beleid in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat Streaming beleidsregels zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 510899e44e4ea4a90e21473ee6af546744c2be2a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120199"
---
# <a name="streaming-policies"></a>Beleid voor streaming

In Azure Media Services v3, [beleid Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) kunt u voor het definiëren van protocollen voor streaming- en versleutelingsopties voor uw [Streaming-Locators](streaming-locators-concept.md). Media Services v3 biedt dat enkele vooraf gedefinieerde Streaming beleid zodat u ze voor evaluatie- of productie gebruiken kunt. 

De momenteel beschikbare vooraf gedefinieerd beleid Streaming:<br/>'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' and 'Predefined_MultiDrmStreaming'.

Als u speciale vereisten (bijvoorbeeld, als u wilt opgeven van verschillende protocollen, moet een aangepaste sleutelleveringsservice gebruiken of wilt gebruiken, schakel audiotrack) hebt, kunt u een aangepast beleid voor Streaming. 

 
> [!IMPORTANT]
> * Eigenschappen van **beleid Streaming** die van de datum/tijd zijn altijd in UTC-notatie zijn.
> * U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. Zie voor meer informatie, [quota en beperkingen](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Beslissingsstructuur

De volgende beslissingsstructuur wordt bij het kiezen van een vooraf gedefinieerd beleid Streaming voor uw scenario.

Klik op de afbeelding om deze in volledig formaat weer te geven.  <br/>
<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/small.png"></a> 

## <a name="examples"></a>Voorbeelden

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

Als u nodig hebt voor het versleutelen van uw inhoud met envelop en cenc versleuteling, moet u het beleid aan 'Predefined_MultiDrmCencStreaming' instellen. Dit beleid geeft aan dat u wilt dat er twee inhoudssleutels (envelop en CENC) worden gegenereerd en ingesteld voor de locator. Er worden dan de envelop-, PlayReady- en Widevine-coderingen toegepast (de sleutel wordt aan de afspeelclient geleverd op basis van de geconfigureerde DRM-licenties).

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

Als u wilt dat ook voor het versleutelen van uw stream met CBCS (FairPlay), gebruikt u 'Predefined_MultiDrmStreaming'.

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
