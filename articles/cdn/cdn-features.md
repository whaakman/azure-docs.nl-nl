---
title: Azure Content Delivery Network (CDN) productfuncties | Microsoft Docs
description: Meer informatie over de functies waarvoor elk product van Azure Content Delivery Network (CDN) ondersteuning biedt.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/23/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: 40638e2180b63c90fbcbe15cc2c1e944a97e166e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cdn-product-features"></a>Azure CDN-producteigenschappen

Er zijn drie Azure Content Delivery Network (CDN) producten: **Azure CDN Standard van Akamai**, **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon**. De volgende tabel vergelijkt de functies die beschikbaar zijn voor elk product.

| **Prestatiefuncties en -optimalisatie** | Standard Akamai | Standard Verizon | Premium Verizon |
| --- | --- | --- | --- |
| [Dynamische siteversnelling](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - adaptieve afbeeldingscompressie](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - vooraf ophalen van objecten](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Optimalisatie van videostreaming](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Optimalisatie van grote bestanden](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Global server load balancing (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Snel leegmaken](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Vooraf laden van assets](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| Instellingen voor cache/koptekst (met behulp van [regels voor opslaan in cache](cdn-caching-rules.md)) |**&#x2713;** |**&#x2713;** | |
| Instellingen voor cache/koptekst (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| [Queryreeksen opslaan in cache](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 dual stack |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor HTTP/2](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Beveiliging** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| HTTPS-ondersteuning met CDN-eindpunt |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTPS voor aangepaste domeinen](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor aangepaste domeinnamen](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geofilters](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenverificatie](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS-beveiliging](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
| **Analytics and reporting** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Diagnostische logboeken in Azure](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon-kernrapporten](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Aangepaste Verizon-rapporten](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Realtime statistieken](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Prestaties van edge-knooppunt](cdn-edge-performance.md) | | |**&#x2713;** |
| [Realtime waarschuwingen](cdn-real-time-alerts.md) | | |**&#x2713;** |
||||
| **Gebruiksgemak** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| Eenvoudige integratie met Azure-services, zoals [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) en [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Beheer via [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) of [PowerShell](cdn-manage-powershell.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Aanpasbare, op regels gebaseerde engine voor contentlevering](cdn-rules-engine.md) | | |**&#x2713;** |
| URL-omleidings-/herschrijfbewerking (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| Regels voor mobiele apparaten (met behulp van [regels-engine](cdn-rules-engine.md)) | | |**&#x2713;** |

\* Verizon ondersteunt de levering van grote bestanden en media rechtstreeks via de optimalisatie van algemene webweergave.



