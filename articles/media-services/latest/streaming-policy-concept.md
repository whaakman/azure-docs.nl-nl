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
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66392925"
---
# <a name="streaming-policies"></a>Beleid voor streaming

In Azure Media Services v3, [beleid Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) kunt u voor het definiëren van protocollen voor streaming- en versleutelingsopties voor uw [Streaming-Locators](streaming-locators-concept.md). Media Services v3 biedt dat enkele vooraf gedefinieerde Streaming beleid zodat u ze voor evaluatie- of productie gebruiken kunt. 

De momenteel beschikbare vooraf gedefinieerd beleid Streaming:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

De volgende "beslissingsstructuur' kunt u een vooraf gedefinieerd beleid Streaming voor uw scenario kiezen.

> [!IMPORTANT]
> * Eigenschappen van **beleid Streaming** die van de datum/tijd zijn altijd in UTC-notatie zijn.
> * U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. Zie voor meer informatie, [quota en beperkingen](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Beslissingsstructuur

Klik op de afbeelding om deze in volledig formaat weer te geven.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

Als uw inhoud versleutelen, moet u maken een [inhoud sleutel beleid](content-key-policy-concept.md), wordt de **inhoud sleutel beleid** is niet nodig voor het wissen te streamen of te downloaden. 

Als u speciale vereisten (bijvoorbeeld, als u wilt opgeven van verschillende protocollen, moet een aangepaste sleutelleveringsservice gebruiken of wilt gebruiken, schakel audiotrack) hebt, kunt u [maken](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) een aangepast beleid voor Streaming. 

## <a name="get-a-streaming-policy-definition"></a>Een Streaming-beleidsdefinitie ophalen  

Als u zien van de definitie van een Streaming-beleid wilt, gebruikt u [ophalen](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) en geef de naam van het beleid. Bijvoorbeeld:

### <a name="rest"></a>REST

Aanvraag:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Reactie:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
