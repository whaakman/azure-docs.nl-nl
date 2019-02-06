---
title: Inhoud van de belangrijkste beleidsregels in mediaservices - Azure | Microsoft Docs
description: Dit artikel bevat een uitleg over wat inhoudsbeleid van de sleutels zijn, en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d9e86c45d535862e0c3d02b3f331bc40ebb7f6c7
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745118"
---
# <a name="content-key-policies"></a>Beleid voor inhoudssleutels

Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

Als u de versleutelingsopties voor uw stroom, moet u maken de [inhoud sleutel beleid](https://docs.microsoft.com/rest/api/media/contentkeypolicies) en koppel deze aan uw **Streaming-Locator gemaakt**. De **inhoud sleutel beleid** configureert u hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients via het onderdeel sleutel levering van Media Services. U kunt Media Services voor de inhoudssleutel automatisch worden gegenereerd. Normaal gesproken zou u een lange levensduur sleutel wordt gebruikt en controleren of er sprake beleidsregels met Get. Als u de sleutel, moet u een aparte actie-methode voor het ontvangen van geheimen of referenties, Zie het volgende voorbeeld aan te roepen.

**Beleid voor sleutels inhoud** kunnen worden bijgewerkt. Mogelijk wilt u bijvoorbeeld het beleid bijwerken als u wilt een rouleren van de sleutel. U kunt de primaire verificatiesleutel en de lijst met alternatieve verificatiesleutels in het bestaande beleid bijwerken. Het kan tot 15 minuten voor de levering van sleutel-caches om te werken en het bijgewerkte beleid pikken duren. 

> [!IMPORTANT]
> * Eigenschappen van **Inhoudbeleidsregels sleutel** die van de datum/tijd zijn altijd in UTC-notatie zijn.
> * U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. 

## <a name="example"></a>Voorbeeld

Als u naar de sleutel, gebruikt **GetPolicyPropertiesWithSecretsAsync**, zoals wordt weergegeven in het onderstaande voorbeeld.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
