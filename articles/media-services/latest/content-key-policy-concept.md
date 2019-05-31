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
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 04808dda193a3a472813cda852a1a2f24d00e112
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417995"
---
# <a name="content-key-policies"></a>Beleid voor inhoudssleutels

Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. 

Als u de versleutelingsopties voor uw stroom, moet u maken een [beleid Streaming](streaming-policy-concept.md) en koppel deze aan uw [Streaming-Locator gemaakt](streaming-locators-concept.md). U maakt de [inhoud sleutel beleid](https://docs.microsoft.com/rest/api/media/contentkeypolicies) configureren hoe de inhoudssleutel (waarmee de beveiligde toegang tot uw [activa](assets-concept.md)) als u wilt beëindigen van clients wordt geleverd. U moet de vereisten (beperkingen) instellen in het beleid dat inhoud sleutel die moet worden voldaan opdat sleutels met de opgegeven configuratie moet worden geleverd aan clients. Deze inhoud sleutel-beleid is niet nodig om duidelijk streamen of te downloaden. 

Meestal, koppelt u uw **inhoud sleutel beleid** met uw [Streaming-Locator gemaakt](streaming-locators-concept.md). U kunt ook kunt u de inhoud sleutel beleid binnen een [beleid Streaming](streaming-policy-concept.md) (bij het maken van een aangepast beleid Streaming voor geavanceerde scenario's). 

Het verdient aanbeveling om te laten Media Services-inhoud sleutels automatisch worden gegenereerd. Normaal gesproken zou u een lange levensduur sleutel wordt gebruikt en controleren of er sprake beleidsregels met **ophalen**. Als u de sleutel, moet u een aparte actie-methode voor het ontvangen van geheimen of referenties, Zie het volgende voorbeeld aan te roepen.

**Beleid voor sleutels inhoud** kunnen worden bijgewerkt. Het kan tot 15 minuten voor de levering van sleutel-caches om te werken en het bijgewerkte beleid pikken duren. 

> [!IMPORTANT]
> * Eigenschappen van **Inhoudbeleidsregels sleutel** die van de datum/tijd zijn altijd in UTC-notatie zijn.
> * U moet een beperkte set beleidsregels ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer dezelfde opties nodig zijn. Zie voor meer informatie, [quota en beperkingen](limits-quotas-constraints.md).

## <a name="example"></a>Voorbeeld

Als u naar de sleutel, gebruikt **GetPolicyPropertiesWithSecretsAsync**, zoals weergegeven in de [ophalen van een ondertekeningssleutel van het bestaande beleid](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) voorbeeld.

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
