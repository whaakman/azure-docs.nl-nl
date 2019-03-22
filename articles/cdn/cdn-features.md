---
title: Productfuncties van Azure Content Delivery Network (CDN) vergelijken| Microsoft Docs
description: Meer informatie over de functies waarvoor elk product van Azure Content Delivery Network (CDN) ondersteuning biedt.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/28/2019
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: cce28b031b146d8a56d37647022261294f07f0be
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57213565"
---
# <a name="compare-azure-cdn-product-features"></a>Azure CDN-producteigenschappen vergelijken

Azure Content Delivery Network (CDN) bevat vier producten: **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon**. Voor informatie over het migreren van een **Azure CDN Standard van Verizon**-profiel naar **Azure CDN Premium van Verizon**, raadpleegt u [Een Azure CDN-profiel migreren van Standard Verizon naar Premium Verizon](cdn-migrate.md).

De volgende tabel vergelijkt de functies die beschikbaar zijn voor elk product.

| **Prestatiefuncties en -optimalisatie** | **Standard Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** |
| --- | --- | --- | --- | --- |
| [Dynamische siteversnelling](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  |  | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - adaptieve afbeeldingscompressie](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dynamische siteversnelling - vooraf ophalen van objecten](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| [Algemene web delivery optimization](https://docs.microsoft.com/azure/cdn/cdn-optimization-overview#general-web-delivery)  | **&#x2713;** | **&#x2713;**, Selecteer dit optimalisatietype als de gemiddelde grootte kleiner dan 10 MB is  | **&#x2713;** |  **&#x2713;** |
| [Optimalisatie van videostreaming](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | via algemene webweergave | **&#x2713;**  | via algemene webweergave |  via algemene webweergave |
| [Optimalisatie van grote bestanden](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | via algemene webweergave | **&#x2713;**, Selecteert u dit type eindpuntoptimalisatie als de gemiddelde grootte groter is dan 10 MB   | via algemene webweergave |  via algemene webweergave |
| Optimalisatietype wijzigen | |**&#x2713;** | | |
| Poort van oorsprong |Alle TCP-poorten |[Toegestane oorsprong](https://docs.microsoft.com/previous-versions/azure/mt757337(v%3Dazure.100)#allowed-origin-ports) |Alle TCP-poorten |Alle TCP-poorten |
| [Global server load balancing (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Snel leegmaken](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;**, Alles leegmaken en leegmaken met jokertekens worden momenteel niet ondersteund door Azure CDN van Akamai |**&#x2713;** |**&#x2713;** |
| [Vooraf laden van assets](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| Instellingen voor cache/koptekst (met behulp van [regels voor opslaan in cache](cdn-caching-rules.md))  |  |**&#x2713;** |**&#x2713;** | |
| Bestemde, aanpasbare regels op basis van de engine voor contentlevering (met behulp van [regels-engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| Instellingen voor cache/koptekst (met behulp van [regels-engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| Omleidings-URL/herschrijfbewerking (met behulp van [regels-engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| Regels voor mobiele apparaten (met behulp van [regels-engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| [Queryreeksen opslaan in cache](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 dual stack | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor HTTP/2](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Beveiliging** | **Standard Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| HTTPS-ondersteuning met CDN-eindpunt | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTPS voor aangepaste domeinen](cdn-custom-ssl.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Ondersteuning voor aangepaste domeinnamen](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geofilters](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenverificatie](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [DDOS-beveiliging](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Uw eigen certificaat gebruiken](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) |**&#x2713;** |  |  |  |
||||
| **Analytics and reporting** | **Standard Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Diagnostische logboeken in Azure](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon-kernrapporten](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Aangepaste Verizon-rapporten](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [Realtime statistieken](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [Prestaties van edge-knooppunt](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [Realtime waarschuwingen](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **Gebruiksgemak** | **Standard Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| Eenvoudige integratie met Azure-services, zoals [Storage](cdn-create-a-storage-account-with-cdn.md), [Web Apps](cdn-add-to-web-app.md) en [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Beheer via [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) of [PowerShell](cdn-manage-powershell.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Compressie MIME-typen](https://docs.microsoft.com/azure/cdn/cdn-improve-performance)  |Standaard alleen |Configureerbare |Configureerbare  |Configureerbare  |
| Compressie coderingen  |gzip, brotli |gzip |gzip, deflate, bzip2, brotili  |gzip, deflate, bzip2, brotili  |






