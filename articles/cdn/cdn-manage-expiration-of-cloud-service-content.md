---
title: Vervaldatum van webinhoud in Azure inhoud Delivery Network beheren | Microsoft Docs
description: Informatie over het beheren van de verlooptijd van Azure Web Apps/Cloud Services, ASP.NET of IIS inhoud in Azure CDN.
services: cdn
documentationcenter: .NET
author: dksimpson
manager: akucer
editor: ''
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2018
ms.author: mazha
ms.openlocfilehash: d1a12e0d5bd5852cf8de3d5fec93f2bfdd3ab257
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Vervaldatum van webinhoud in Azure inhoud Delivery Network beheren
> [!div class="op_single_selector"]
> * [Azure-webinhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Bestanden van de webservers openbaar toegankelijk oorsprong kunnen opslaan in de cache in Azure Content Delivery Network (CDN) totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord op de bronserver. In dit artikel wordt beschreven hoe instelt `Cache-Control` headers voor de functie Web Apps van Microsoft Azure App Service, Azure Cloud Services, ASP.NET-toepassingen en Internet Information Services (IIS)-sites, die allemaal op dezelfde manier zijn geconfigureerd. U kunt instellen de `Cache-Control` header met behulp van de configuratiebestanden of programmatisch. 

U kunt de cache-instellingen van de Azure-portal ook beheren door in te stellen [CDN regels opslaan in cache](cdn-caching-rules.md). Als u een of meer caching regels en hun gedrag ingesteld op **overschrijven** of **cache overslaan**, de geleverde oorsprong cache-instellingen in dit artikel wordt beschreven, worden genegeerd. Zie voor meer informatie over algemene concepten voor caching [werking van cacheopslag](cdn-how-caching-works.md).

> [!TIP]
> U kunt geen TTL ingesteld op een bestand. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen, tenzij u opslaan in cache regels in de Azure-portal hebt ingesteld. Deze standaard TTL geldt alleen voor levering optimalisaties van algemene webtoepassingen. De standaard TTL-waarde is één dag voor optimalisatie van grote bestanden, en voor mediastreaming optimalisaties de standaard TTL-waarde is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot bestanden en andere bronnen [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Cache-Control headers instellen met behulp van CDN regels opslaan in cache
De voorkeursmethode voor het instellen van een webserver `Cache-Control` header is het gebruik van cachebewerkingen regels in de Azure portal. Zie voor meer informatie over CDN caching regels [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md).

> [!NOTE] 
> In het cachegeheugen regels zijn alleen beschikbaar voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. Voor **Azure CDN Premium van Verizon** profielen, moet u de [Azure CDN regelengine](cdn-rules-engine.md) in de **beheren** portal voor vergelijkbare functionaliteit.

**Om te navigeren naar de pagina in het cachegeheugen regels CDN**:

1. In de Azure portal een CDN-profiel te selecteren en selecteer vervolgens het eindpunt voor de webserver.

2. Selecteer in het linkerdeelvenster onder instellingen **regels opslaan in cache**.

   ![Knop regels in het cachegeheugen van CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   De **regels opslaan in cache** pagina wordt weergegeven.

   ![In het cachegeheugen CDN-pagina](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Een web-Cache-Control-headers van de server met behulp van de algemene regels voor opslaan in cache instellen:**

1. Onder **Global caching regels**stelt **queryreeks cachegedrag** naar **querytekenreeksen negeren** en stel **cachegedrag** naar  **Overschrijven**.
      
2. Voor **duur voor het verlopen in de Cache**, voer 3600 in de **seconden** box of 1 in de **uren** vak. 

   ![Globale cachebewerkingen regels voorbeeld van CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Deze globale cacheregel Hiermee stelt u een Cacheduur van één uur en is van invloed op alle aanvragen voor het eindpunt. Deze voorrang op een `Cache-Control` of `Expires` HTTP-headers zijn verzonden door de bronserver die is opgegeven door het eindpunt.   

3. Selecteer **Opslaan**.

**Een webserver Cache-Control-headers van het bestand met behulp van aangepaste regels voor opslaan in cache instellen:**

1. Onder **aangepaste regels opslaan in cache**, twee identieke voorwaarden maken:

     a. Voor de eerste voorwaarde van de overeenkomst ingesteld **overeenkomen met de voorwaarde** naar **pad** en voer `/webfolder1/*` voor **overeenkomen met waarde**. Stel **cachegedrag** naar **overschrijven** en voer 4 in de **uren** vak.

     b. Voor de tweede voorwaarde van de overeenkomst ingesteld **overeenkomen met de voorwaarde** naar **pad** en voer `/webfolder1/file1.txt` voor **overeenkomen met waarde**. Stel **cachegedrag** naar **overschrijven** en voer 2 in de **uren** vak.

    ![Aangepaste cachebewerkingen regels voorbeeld van CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    De eerste aangepaste cacheregel stelt een Cacheduur van vier uur voor alle bestanden in de `/webfolder1` map op de bronserver die is opgegeven door het eindpunt. De tweede regel heeft voorrang op de eerste regel voor de `file1.txt` alleen bestands- en wordt de Cacheduur van een van twee uur voor ingesteld.

2. Selecteer **Opslaan**.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Cache-Control headers instellen met behulp van configuratiebestanden
Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen door het wijzigen van de **applicationHost.config** of **Web.config** configuratiebestanden voor uw webtoepassing. Om in te stellen de `Cache-Control` -header voor de inhoud, gebruik de `<system.webServer>/<staticContent>/<clientCache>` element in een bestand.

### <a name="using-applicationhostconfig-files"></a>Met behulp van ApplicationHost.config bestanden
De **ApplicationHost.config** bestand is het bestand van de hoofdmap van het IIS-configuratie-systeem. De configuratie-instellingen in een **ApplicationHost.config** bestand invloed op alle toepassingen op de site, maar worden overschreven door de instellingen van **Web.config** bestanden die aanwezig zijn voor een webtoepassing.

### <a name="using-webconfig-files"></a>Met behulp van Web.config-bestanden
Met een **Web.config** -bestand, kunt u bepalen hoe uw hele webtoepassing of een specifieke map op uw webtoepassing gedraagt zich. Gewoonlijk hebt u ten minste één **Web.config** bestand in de hoofdmap van uw webtoepassing. Voor elk **Web.config** bestand in een specifieke map, de configuratie-instellingen van invloed op alles in die map en submappen, tenzij ze op het niveau van de submap worden overschreven door een andere **Web.config** het bestand. 

U kunt bijvoorbeeld instellen een `<clientCache>` -element in een **Web.config** bestand in de hoofdmap van uw webtoepassing in de cache van alle statische inhoud op uw webtoepassing drie dagen. U kunt ook toevoegen een **Web.config** bestand in een submap met meer variabele inhoud (bijvoorbeeld `\frequent`) en stel de `<clientCache>` element in de cache van de submap inhoud gedurende zes uur. Het resultaat is dat inhoud op de gehele website opgeslagen in de cache voor drie dagen, met uitzondering van inhoud in de `\frequent` directory, dat alleen de zes uur in cache wordt opgeslagen.  

Het volgende voorbeeld van een XML-configuratie wordt ingesteld de `<clientCache>` element een maximale leeftijd van drie dagen opgeven:  

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
