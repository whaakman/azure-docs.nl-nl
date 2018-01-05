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
ms.openlocfilehash: dca6ca5f21f4a4f1701af57eb40d92094b6a4754
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Vervaldatum van webinhoud in Azure inhoud Delivery Network beheren
> [!div class="op_single_selector"]
> * [Azure-webinhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

De bestanden van de webserver van elke openbaar toegankelijke oorsprong kunnen opslaan in de cache in Azure Content Delivery Network (CDN) totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord op de bronserver. In dit artikel wordt beschreven hoe instelt `Cache-Control` headers voor de functie Web Apps van Microsoft Azure App Service, Azure Cloud Services, ASP.NET-toepassingen en Internet Information Services (IIS)-sites, die allemaal op dezelfde manier zijn geconfigureerd. U kunt instellen de `Cache-Control` header met behulp van de configuratiebestanden of programmatisch. 

U kunt de cache-instellingen van de Azure-portal ook beheren door in te stellen [CDN regels opslaan in cache](cdn-caching-rules.md). Als u slechts één of meer caching regels en hun gedrag ingesteld op **overschrijven** of **cache overslaan**, de geleverde oorsprong cache-instellingen in dit artikel wordt beschreven, worden genegeerd. Zie voor meer informatie over algemene concepten voor caching [werking van cacheopslag](cdn-how-caching-works.md).

> [!TIP]
> U kunt geen TTL ingesteld op een bestand. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen, tenzij u een cache van de regels in de Azure-portal hebt ingesteld. Deze standaard TTL geldt alleen voor levering optimalisaties van algemene webtoepassingen. De standaard TTL-waarde is één dag voor optimalisatie van grote bestanden, en voor mediastreaming optimalisaties de standaard TTL-waarde is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot bestanden en andere bronnen [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Cache-Control headers instellen met behulp van configuratiebestanden
Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen door het wijzigen van de **applicationHost.config** of **Web.config** configuratiebestanden voor uw webtoepassing. De `<system.webServer>/<staticContent>/<clientCache>` -element in beide bestandssets de `Cache-Control` -header voor uw inhoud.

### <a name="using-applicationhostconfig-files"></a>Met behulp van ApplicationHost.config bestanden
De **ApplicationHost.config** bestand is het bestand van de hoofdmap van het IIS-configuratie-systeem. De configuratie-instellingen in een **ApplicationHost.config** bestand invloed op alle toepassingen op de site, maar worden overschreven door de instellingen van **Web.config** bestanden die aanwezig zijn voor een webtoepassing.

### <a name="using-webconfig-files"></a>Met behulp van Web.config-bestanden
Met een **Web.config** -bestand, kunt u bepalen hoe uw hele webtoepassing of een specifieke map op uw webtoepassing gedraagt zich. Gewoonlijk hebt u ten minste één **Web.config** bestand in de hoofdmap van uw webtoepassing. Voor elk **Web.config** bestand in een specifieke map, de configuratie-instellingen van invloed op alles in die map en alle submappen, tenzij ze op het niveau van de submap worden overschreven door een andere **Web.config**bestand. U kunt bijvoorbeeld instellen een `<clientCache>` -element in een **Web.config** bestand in de hoofdmap van uw webtoepassing in de cache van alle statische inhoud op uw webtoepassing drie dagen. U kunt ook toevoegen een **Web.config** bestand in een submap met meer variabele inhoud (bijvoorbeeld `\frequent`) en stel de `<clientCache>` element in de cache van de submap inhoud gedurende zes uur. Het resultaat is dat inhoud op de gehele website zal worden opgeslagen voor drie dagen, met uitzondering van inhoud in de `\frequent` directory, dat wordt alleen de zes uur in cache opgeslagen.  

De volgende XML-voorbeeld laat zien hoe in te stellen de `<clientCache>` element in een configuratiebestand om op te geven van een maximale leeftijd van drie dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Gebruik de **cacheControlMaxAge** kenmerk, u moet de waarde van de **cacheControlMode** kenmerk `UseMaxAge`. Deze instelling veroorzaakt de HTTP-header en Richtlijn `Cache-Control: max-age=<nnn>`, worden toegevoegd aan het antwoord. De indeling van de timespan-waarde voor de **cacheControlMaxAge** kenmerk `<days>.<hours>:<min>:<sec>`. De waarde wordt geconverteerd naar seconden en wordt gebruikt als de waarde van de `Cache-Control` `max-age` richtlijn. Voor meer informatie over de `<clientCache>` element, Zie [clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Cache-Control headers instellen programmatisch
Voor ASP.NET-toepassingen die u beheert de CDN cachegedrag programmatisch door in te stellen de **HttpResponse.Cache** eigenschap van de .NET API. Voor informatie over de **HttpResponse.Cache** eigenschap, Zie [HttpResponse.Cache eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Programmatisch cache toepassing inhoud in ASP.NET, de volgende stappen uit:
   1. Controleer of dat de inhoud is gemarkeerd als caching geschikte door in te stellen `HttpCacheability` naar `Public`. 
   2. Validatie van de cache ingesteld door het aanroepen van een van de volgende `HttpCachePolicy` methoden:
      - Roep `SetLastModified` een tijdstempelwaarde instellen voor de `Last-Modified` header.
      - Roep `SetETag` naar een waarde instellen voor de `ETag` header.
   3. Geef eventueel een verlooptijd cache door aan te roepen `SetExpires` naar een waarde instellen voor de `Expires` header. Anders wordt toepassing de standaard cache methodiek eerder in dit document beschreven.

Bijvoorbeeld tot de cache inhoud gedurende één uur, voeg de volgende C#-code:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Testen van de Cache-Control-header
U kunt eenvoudig de TTL-instellingen van de inhoud van uw website controleren. Met uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test die inhoud van uw website bevat de `Cache-Control` antwoordheader. U kunt ook een hulpprogramma zoals gebruiken **wget**, [Postman](https://www.getpostman.com/), of [Fiddler](http://www.telerik.com/fiddler) om te onderzoeken de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Lees meer informatie over de **clientCache** element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Raadpleeg de documentatie van de **HttpResponse.Cache** eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Raadpleeg de documentatie van de **HttpCachePolicy-klasse**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)  
* [Meer informatie over concepten opslaan in cache](cdn-how-caching-works.md)
