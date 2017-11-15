---
title: Vervaldatum van webinhoud in Azure inhoud Delivery Network beheren | Microsoft Docs
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
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: d58a245923242b3963b188ca869e8290d999c0a2
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Vervaldatum van webinhoud in Azure inhoud Delivery Network beheren
 in Azure CDN
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET of IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Bestanden van de webserver van elke openbaar toegankelijke oorsprong kunnen opslaan in de cache in Azure Content Delivery Network (CDN) totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de [ `Cache-Control` header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in het HTTP-antwoord op de bronserver. In dit artikel wordt beschreven hoe instelt `Cache-Control` headers voor de functie Web Apps van Microsoft Azure App Service, Azure Cloud Services, ASP.NET-toepassingen en Internet Information Services-sites, die allemaal op dezelfde manier zijn geconfigureerd.

> [!TIP]
> U kunt geen TTL ingesteld op een bestand. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot bestanden en andere bronnen [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-in-configuration-files"></a>Cache-Control headers instellen in configuratiebestanden
Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen door het wijzigen van de **applicationHost.config** of **web.config** bestanden voor uw webtoepassing. De **system.webServer\staticContent\clientCache** -element in het bestand configuratiesets de `Cache-Control` -header voor uw inhoud. Voor **web.config** bestanden, de configuratie-instellingen van invloed op alles in de map en alle submappen, tenzij ze worden overschreven op het niveau van de submap. U kunt bijvoorbeeld een standaardinstelling voor de TTL-waarde ingesteld op de hoofdmap in de cache van statische inhoud alle drie dagen, en stelt een submap met meer variabele inhoud naar de inhoud ervan alleen zes uur in de cache. Hoewel **applicationHost.config** bestandsinstellingen gelden voor alle toepassingen op de site, ze worden overschreven door de instellingen van elke bestaande **web.config** bestanden in de toepassingen.

De volgende XML-voorbeeld toont hoe instelt **clientCache** om op te geven van een maximale leeftijd van drie dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Geven **UseMaxAge** zorgt ervoor dat een `Cache-Control: max-age=<nnn>` header moet worden toegevoegd aan de reactie op basis van de opgegeven waarde in de **CacheControlMaxAge** kenmerk. De indeling van het interval voor de **cacheControlMaxAge** kenmerk `<days>.<hours>:<min>:<sec>`. Voor meer informatie over de **clientCache** knooppunt, Zie [clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Cache-Control headers instellen in code
Voor ASP.NET-toepassingen, kunt u bepalen de CDN cachegedrag programmatisch door in te stellen de **HttpResponse.Cache** eigenschap. Voor meer informatie over de **HttpResponse.Cache** eigenschap, Zie [HttpResponse.Cache eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Programmatisch cache toepassing inhoud in ASP.NET, de volgende stappen uit:
   1. Controleer of dat de inhoud is gemarkeerd als caching geschikte door in te stellen `HttpCacheability` naar *openbare*. 
   2. Validatie van de cache ingesteld door een van de volgende methoden:
      - Roep `SetLastModified` een tijdstempel LastModified instellen.
      - Roep `SetETag` om in te stellen een `ETag` waarde.
   3. Geef eventueel een verlooptijd cache door het aanroepen van `SetExpires`. Anders wordt toepassing de standaard cache methodiek eerder in dit document beschreven.

Bijvoorbeeld tot de cache inhoud gedurende één uur, voeg de volgende C#-code:  

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

