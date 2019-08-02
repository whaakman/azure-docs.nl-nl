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
ms.date: 07/26/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8809bf25c3bcfb26fb0ad251a2b09dfdca2a3e04
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679193"
---
# <a name="content-key-policies"></a>Beleid voor inhoudssleutels

Met Media Services kunt u uw Live en on-demand inhoud dynamisch versleutelen met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste Digital Rights Management (DRM)-systemen: Micro soft PlayReady, Google Widevine en Apple FairPlay. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. 

Als u versleutelings opties wilt opgeven voor uw stream, moet u een [streaming-beleid](streaming-policy-concept.md) maken en dit koppelen aan uw [streaming-Locator](streaming-locators-concept.md). U maakt het [beleid voor inhouds sleutels](https://docs.microsoft.com/rest/api/media/contentkeypolicies) om te configureren hoe de inhouds sleutel (die beveiligde toegang tot uw [assets](assets-concept.md)biedt) wordt geleverd aan de eind clients. U moet de vereisten (beperkingen) instellen voor het beleid voor de inhouds sleutel waaraan moet worden voldaan om sleutels met de opgegeven configuratie aan clients te kunnen leveren. Het beleid voor inhouds sleutels is niet nodig voor het wissen van streams of downloaden. 

Normaal gesp roken koppelt u het beleid voor inhouds sleutels aan uw [streaming-Locator](streaming-locators-concept.md). U kunt ook het beleid voor de inhouds sleutel binnen een [streaming-beleid](streaming-policy-concept.md) opgeven (bij het maken van een aangepast streaming-beleid voor geavanceerde scenario's). 

> [!NOTE]
> Eigenschappen van het beleid voor de inhouds sleutel van het `Datetime` type zijn altijd in UTC-indeling.

## <a name="best-practices-and-considerations"></a>Aanbevolen procedures en overwegingen

> [!IMPORTANT]
> Controleer de volgende aanbevelingen.

* U dient een beperkt aantal beleids regels te ontwerpen voor uw media service-account en deze opnieuw te gebruiken voor uw streaming-Locators wanneer dezelfde opties nodig zijn. Zie [quota's en beperkingen](limits-quotas-constraints.md)voor meer informatie.
* Beleid voor inhouds sleutels kan worden bijgewerkt. Het kan tot vijf tien minuten duren voordat de sleutel bezorgings caches zijn bijgewerkt en het bijgewerkte beleid wordt opgehaald. 

   Door het beleid bij te werken, wordt uw bestaande CDN-cache overschreven, waardoor er problemen kunnen ontstaan met het afspelen van klanten die inhoud in de cache gebruiken.  
* We raden u aan geen nieuw beleid voor inhouds sleutels voor elke Asset te maken. De belangrijkste voor delen van het delen van hetzelfde beleid voor inhouds sleutels tussen activa die dezelfde beleids opties nodig hebben, zijn:
   
   * Het is eenvoudiger om een klein aantal beleids regels te beheren.
   * Als u updates moet maken voor het beleid voor inhouds sleutels, worden de wijzigingen bijna direct van kracht op alle nieuwe licentie aanvragen.
* Als u een nieuw beleid moet maken, moet u een nieuwe streaming-Locator voor de Asset maken.
* Het wordt aanbevolen Media Services automatisch de inhouds sleutel te genereren. 

   Normaal gesp roken gebruikt u een langdurige sleutel en controleert u of het beleid voor de inhouds sleutel met [Get](https://docs.microsoft.com/rest/api/media/contentkeypolicies/get). Als u de sleutel wilt ophalen, moet u een afzonderlijke actie methode aanroepen om geheimen of referenties op te halen. Zie het volgende voor beeld.

## <a name="example"></a>Voorbeeld

Als u de sleutel wilt weer geven `GetPolicyPropertiesWithSecretsAsync`, gebruikt u, zoals wordt weer gegeven in het voor beeld [een handtekening sleutel van het bestaande beleid ophalen](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) .

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, ordenen, pagineren van Media Services entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)
* [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
