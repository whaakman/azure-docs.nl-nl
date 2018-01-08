---
title: Azure CDN cachebeleid in Azure Media Services beheren | Microsoft Docs
description: Informatie over het beheren van Azure CDN cachebeleid in Azure Media Services.
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: bcb5a6e118c779e7e13ba8472d5bba510cddeb12
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure CDN cachebeleid in Azure Media Services beheren
Azure Media Services biedt dat HTTP gebaseerde adaptief streamen en progressief downloaden. HTTP op basis van streaming is zeer schaalbaar voordelen opslaan in cache in de proxy- en CDN lagen, evenals van caching aan clientzijde. Streaming-eindpunten bevat algemene streaming mogelijkheden en configuratie voor cache-HTTP-headers. Streaming-eindpunten ingesteld HTTP-Cache-Control: maximumleeftijd en -Expires-koppen. U kunt meer informatie ophalen voor HTTP-headers cache van [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Standaard opslaan in cache headers
Standaard is voor streaming-eindpunten 3 dagen cache headers voor op aanvraag streamen gegevens (echte media fragmenten/segmenten) en manifest(playlist) toepassen. Streaming-eindpunten toepassing 3 dagen cache headers voor gegevens (echte media fragmenten/segmenten) en 2 seconden in de cache-header voor manifest(playlist) aanvragen voor live streamen. Wanneer live programma verandert in op-verzoek (live archiveren), klikt u vervolgens op aanvraag streamen cache headers van toepassing.

## <a name="azure-cdn-integration"></a>Integratie van Azure CDN
Azure Media Services biedt [CDN geïntegreerd](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) voor streaming-eindpunten. Cache-control van headers is van toepassing op dezelfde manier als streaming-eindpunten naar CDN ingeschakeld streaming-eindpunten. Azure maakt gebruik van CDN streaming-eindpunt geconfigureerd cachewaarden om te definiëren van de levensduur van de objecten intern in de cache en deze waarde ook wordt de levering van cache headers instellen. Wanneer met behulp van CDN streaming-eindpunten is ingeschakeld, wordt het niet raadzaam kleine cachewaarden in te stellen. Kleine instellingswaarden de prestaties verminderen en de voordelen van CDN verminderen. Het is niet toegestaan in te stellen cache headers kleiner is dan 600 seconden voor CDN ingeschakeld streaming-eindpunten.

> [!IMPORTANT]
>Azure Media Services is volledig geïntegreerd met Azure CDN. U kunt alle beschikbare Azure CDN providers (Akamai en Verizon) naar uw streaming-eindpunt met inbegrip van producten CDN Standard en Premium integreren met een enkele klik. Zie voor meer informatie dit [aankondiging](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Kosten van de gegevens van streaming-eindpunt naar CDN alleen opgehaald uitgeschakeld als de CDN is ingeschakeld via het streaming-eindpunt API's of met behulp van Azure portal streaming-eindpunt sectie. Handmatige integratie of rechtstreeks maken van een CDN-eindpunt met CDN-API's of portal sectie uitschakelen niet voor de kosten van de gegevens.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Cache-headers configureren met Azure Media Services
U kunt Azure-portal of Azure Media Services-API's gebruiken voor het configureren van de cache met de headerwaarden.

1. Raadpleeg voor het configureren van cache-headers met Azure portal [Streaming-eindpunten beheren](../media-services/media-services-portal-manage-streaming-endpoints.md) sectie het Streaming-eindpunt configureren.
2. Azure Media Services REST-API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl eigenschappen](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Voorkeursvolgorde cache-configuratie
1. Azure Media Services geconfigureerd cachewaarde overschrijft de standaardwaarde.
2. Als er geen handmatige configuratie, wordt standaardwaarden toepassen.
3. Door standaard twee seconden cache headers is van toepassing op live streaming manifest(playlist) ongeacht de configuratie van Azure Media of Azure Storage en onderdrukken deze waarde is niet beschikbaar.

