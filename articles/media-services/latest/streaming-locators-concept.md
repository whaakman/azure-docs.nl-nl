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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 51aa33e4ff387a1030dac42bce8d12cf72343b35
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317473"
---
# <a name="streaming-locators"></a>Streaming-locators

Om video's in de uitvoer van de Asset die beschikbaar zijn voor clients om te worden afgespeeld, die u moet maken een [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators) en bouw vervolgens de streaming-URL's. Zie voor .NET-voorbeeld, [ophalen van een Streaming-Locator gemaakt](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Het proces van het maken van een **streaming-locator** wordt publiceren genoemd. De **streaming-locator** is standaard onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een **Streaming-Locator gemaakt**, moet u opgeven de [Asset](https://docs.microsoft.com/rest/api/media/assets) naam en de [beleid Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) naam. U kunt een van de vooraf gedefinieerde Streaming beleidsregels gebruiken of een aangepast beleid gemaakt. De vooraf gedefinieerde beleidsregels die momenteel beschikbaar zijn: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' and 'Predefined_MultiDrmStreaming'. Wanneer u een aangepaste moet streaming-beleid u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties en protocollen die nodig zijn. 

Als u opgeven van de versleutelingsopties voor uw stroom wilt, maakt u de [inhoud sleutel beleid](https://docs.microsoft.com/rest/api/media/contentkeypolicies) die Hiermee configureert u hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients via het onderdeel sleutel levering van Media Services. Koppel uw Streaming-Locator met de **inhoud sleutel beleid** en de inhoudssleutel. U kunt Media Services naar de sleutel automatisch worden gegenereerd. De volgende .NET-voorbeeld laat zien hoe het configureren van AES-versleuteling met een beperking van de token in Media Services v3: [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted). **Beleid voor sleutels inhoud** bij te werken, kunt u het beleid bijwerkt als u wilt een rouleren van de sleutel zijn. Het kan tot 15 minuten voor de levering van sleutel-caches om te werken en het bijgewerkte beleid pikken duren. Het verdient aanbeveling met dit beleid kunt u een nieuwe inhoud sleutel niet maken voor elke Streaming-Locator gemaakt. U moet proberen om de bestaande beleidsregels gebruiken wanneer u dezelfde opties nodig zijn.

> [!IMPORTANT]
> * Eigenschappen van **Streaming-Locators** die van de datum/tijd zijn altijd in UTC-notatie zijn.
> * U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. 

## <a name="associate-filters-with-streaming-locators"></a>Filters met Streaming-Locators koppelen

U kunt een lijst opgeven met [asset of account filters](filters-concept.md), die gelden voor uw [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body). De [dynamische packager](dynamic-packaging-overview.md) deze lijst met filters samen met die de client Hiermee geeft u in de URL van toepassing is. Deze combinatie genereert een [dyanamic manifest](filters-dynamic-manifest-overview.md), die is gebaseerd op filters in de URL en filters die u op Streaming-Locator gemaakt opgeeft. U wordt aangeraden dat u deze functie gebruiken als u filters wilt toepassen, maar niet wilt weergeven van de filterfunctie namen in de URL.

## <a name="filter-order-page-streaming-locator-entities"></a>Filter, de volgorde, de pagina Streaming-Locator gemaakt entiteiten

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Uploaden, coderen en streamen van video's met behulp van .NET](stream-files-tutorial-with-api.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
