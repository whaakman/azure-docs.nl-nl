---
title: Beheren van Azure CDN-beleid voor caching in Azure Media Services | Microsoft Docs
description: Informatie over het beheren van Azure CDN-beleid voor caching in Azure Media Services.
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: ''
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 516df2f6177303987fc0354dde647c1fc26820ef
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918277"
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure CDN-beleid voor caching in Azure Media Services beheren
Azure Media Services biedt HTTP op basis van adaptief streamen en progressief downloaden. HTTP op basis van streaming is zeer schaalbaar met de voordelen van opslaan in cache in de proxy- en CDN-lagen, evenals caching aan clientzijde. Streaming-eindpunten biedt algemene mogelijkheden voor streamen en ook de configuratie van cache-HTTP-headers. HTTP-Cache-Control streaming-eindpunten worden ingesteld: max-age en Expires-koppen. U krijgt meer informatie voor HTTP-headers cache van [W3.org](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Standaard Caching-headers
Streaming-eindpunten past standaard drie dagen cache headers voor on-demand streaming-gegevens (echte media fragmenten/segmenten) en manifest(playlist). Streaming-eindpunten toepassen drie dagen cache headers voor gegevens (echte media fragmenten/segmenten) en 2 seconden in de cache-header voor manifest(playlist) aanvragen voor live streamen. Wanneer u live programma kunnen op de vraag (live archief), wordt vervolgens on-demand streaming cache headers van toepassing.

## <a name="azure-cdn-integration"></a>Azure CDN-integratie
Azure Media Services biedt [geïntegreerde CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) voor streaming-eindpunten. Cache-control-headers is van toepassing op dezelfde manier zoals streaming-eindpunten naar het CDN-streaming-eindpunten ingeschakeld. Azure CDN maakt gebruik van streaming-eindpunt cachewaarden voor het definiëren van de levensduur van de objecten intern in de cache geconfigureerd en gebruikt deze waarde ook naar de cache-headers voor de levering instellen. Het verdient aanbeveling niet om in te stellen kleine cache bij het gebruik van CDN-streaming-eindpunten ingeschakeld. Kleine instellingswaarden invloed hebben op de prestaties en verminder het voordeel van het CDN. Het is niet toegestaan om in te stellen de cache-headers kleiner is dan 600 seconden voor CDN-streaming-eindpunten ingeschakeld.

> [!IMPORTANT]
>Azure Media Services is volledig geïntegreerd met Azure CDN. Met één klik, kunt u alle beschikbare providers voor Azure CDN integreren met uw streaming-eindpunt met inbegrip van producten voor standard en premium. Zie voor meer informatie dit [aankondiging](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Kosten voor gegevens van streaming-eindpunt naar het CDN haalt alleen uitgeschakeld als het CDN is ingeschakeld via het streaming-eindpunt API's of met behulp van de streaming-eindpunt-gedeelte van Azure portal. Handmatige integratie of rechtstreeks het maken van een CDN-eindpunt met behulp van CDN API's of portal sectie uitschakelen niet de kosten voor gegevens.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Cache-headers configureren met Azure Media Services
U kunt Azure portal of Azure Media Services-API's om waarden voor cache-header te configureren.

1. Als u wilt configureren met behulp van Azure portal cache-headers, verwijzen naar [Streaming-eindpunten beheren](../media-services/previous/media-services-portal-manage-streaming-endpoints.md) sectie configureren van de Streaming-eindpunt.
2. Azure Media Services REST API, [StreamingEndpoint](/rest/api/media/operations/streamingendpoint#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK, [StreamingEndpointCacheControl Properties](https://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Volgorde van prioriteit voor cache-configuratie
1. Azure Media Services geconfigureerd cachewaarde overschrijft de standaardwaarde.
2. Als er geen handmatige configuratie is, wordt standaardwaarden toepassen.
3. Door standaard 2 seconden cache headers is van toepassing op live streaming manifest(playlist), ongeacht de configuratie van Azure Media of Azure Storage en overschrijft deze waarde is niet beschikbaar.

