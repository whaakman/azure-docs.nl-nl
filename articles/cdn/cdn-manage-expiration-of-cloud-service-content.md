---
title: Vervaldatum van webinhoud in Azure CDN beheren | Microsoft Docs
description: Informatie over het beheren van de verlooptijd van Azure Web Apps/Cloud Services, ASP.NET of IIS inhoud in Azure CDN.
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Vervaldatum van Azure Web Apps/Cloud Services, ASP.NET of IIS inhoud in Azure CDN beheren
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET of IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)
> 
> 

Kunnen bestanden van de webserver van elke openbaar toegankelijke oorsprong in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken.  De TTL-waarde wordt bepaald door de [ *Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in het HTTP-antwoord op de bronserver.  In dit artikel wordt beschreven hoe instelt `Cache-Control` headers voor de Web-Apps van Azure, Azure Cloud Services, ASP.NET-toepassingen en Internet Information Services-sites, die allemaal op dezelfde manier zijn geconfigureerd.

> [!TIP]
> U kunt geen TTL ingesteld op een bestand.  In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot bestanden en andere bronnen de [overzicht van Azure CDN](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Cache-Control Headers instelling in configuratie
Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen door het wijzigen van de **applicationHost.config** of **web.config** bestanden voor uw webtoepassing.  De **system.webServer\staticContent\clientCache** element in het configuratiebestand stelt de `Cache-Control` -header voor uw inhoud. Voor **web.config**, de configuratie-instellingen heeft gevolgen voor alles wat u in de map en alle submappen, tenzij genegeerd op het niveau van de submap.  U kunt bijvoorbeeld, stel een standaard time-to-live in de hoofdmap hebben alle statische inhoud in cache opgeslagen 3 dagen, maar hebben een submap die meer variabele inhoud met een cache-instelling van de 6 uur bevat.  Voor **applicationHost.config**, alle toepassingen op de site worden beïnvloed, maar kunnen worden overschreven in **web.config** bestanden in de toepassingen.

De volgende XML-code toont een voorbeeld van de instelling **clientCache** om op te geven van een maximale leeftijd van 3 dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Geven **UseMaxAge** voegt een `Cache-Control: max-age=<nnn>` header aan het antwoord op basis van de opgegeven waarde in de **CacheControlMaxAge** kenmerk. De indeling van de timespan is voor de **cacheControlMaxAge** kenmerk <days>.<hours>:<min>:<sec>. Voor meer informatie over de **clientCache** knooppunt, Zie [clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Cache-Control van Headers instellen in Code
U kunt de CDN cachegedrag programmatisch door in te stellen instellen voor ASP.NET-toepassingen, de **HttpResponse.Cache** eigenschap. Voor meer informatie over de **HttpResponse.Cache** eigenschap, Zie [HttpResponse.Cache eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Als u maken via een programma cache toepassingsinhoud in ASP.NET wilt, zorgt u ervoor dat de inhoud is gemarkeerd als caching geschikte door HttpCacheability in te stellen op *openbare*. Controleer ook of er een cache-validatiefunctie is ingesteld. De validatie van de cache mag een laatst gewijzigd tijdstempel ingesteld door het aanroepen van SetLastModified of een etag-waarde ingesteld door het aanroepen van SetETag. U kunt ook een cache-verlooptijd opgeven door het aanroepen van SetExpires eventueel of kunt u vertrouwen op de standaard cache methodiek die eerder in dit document worden beschreven.  

Bijvoorbeeld tot de cache inhoud gedurende één uur, Voeg het volgende toe:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Volgende stappen
* [Lees meer informatie over de **clientCache** element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Raadpleeg de documentatie van de **HttpResponse.Cache** eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Raadpleeg de documentatie van de **HttpCachePolicy klasse**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

