---
title: Vervaldatum van webinhoud in Azure CDN beheren | Microsoft Docs
description: Informatie over het beheren van Azure Web Apps/Cloud Services, ASP.NET of IIS-inhoud in Azure CDN is verlopen.
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
ms.openlocfilehash: fc74d7fdd082cf497b7cabf30d96509ebe8b6b68
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426015"
---
# <a name="manage-expiration-of-web-content-in-azure-cdn"></a>Vervaldatum van webinhoud in Azure CDN beheren
> [!div class="op_single_selector"]
> * [Azure-webinhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Bestanden van openbaar toegankelijke oorsprong webservers kunnen worden in de cache opgeslagen in Azure Content Delivery Network (CDN) totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord van de oorspronkelijke server. In dit artikel wordt beschreven hoe u om in te stellen `Cache-Control` headers voor de functie Web Apps van Microsoft Azure App Service, Azure Cloud Services, ASP.NET-toepassingen en websites van Internet Information Services (IIS), die allemaal op dezelfde manier zijn geconfigureerd. U kunt instellen dat de `Cache-Control` koptekst met behulp van configuratiebestanden of via een programma. 

U kunt de cache-instellingen van de Azure-portal ook beheren door in te stellen [CDN regels voor caching](cdn-caching-rules.md). Als u een of meer caching-regels en hun cachegedrag ingesteld op **overschrijven** of **cache overslaan**, de oorsprong-opgegeven cache-instellingen die worden beschreven in dit artikel worden genegeerd. Zie voor meer informatie over algemene concepten voor opslaan in cache [hoe caching werkt](cdn-how-caching-works.md).

> [!TIP]
> U kunt geen TTL ingesteld voor een bestand. In dit geval Azure CDN automatisch van toepassing is een standaard-TTL zeven dagen, tenzij u caching-regels in de Azure-portal hebt ingesteld. Deze standaardwaarde voor TTL geldt alleen voor leveringsoptimalisatie gewoon op Internet. De standaard-TTL is één dag voor optimalisatie van grote bestanden, en voor optimalisaties voor streaming van media, de standaard-TTL is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot bestanden en andere bronnen [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Cache-Control-headers instellen met behulp van CDN-regels voor caching
De aanbevolen methode voor het instellen van een webserver `Cache-Control` header is met regels voor opslaan in cache in Azure portal. Zie voor meer informatie over CDN regels voor caching [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md).

> [!NOTE] 
> Regels voor opslaan in cache zijn alleen beschikbaar voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. Voor **Azure CDN Premium van Verizon** profielen, moet u de [regels-engine van Azure CDN](cdn-rules-engine.md) in de **beheren** portal voor vergelijkbare functionaliteit.

**Om te navigeren naar de pagina CDN caching-regels**:

1. In de Azure-portal, selecteert u een CDN-profiel, en selecteer vervolgens het eindpunt voor de webserver.

1. Selecteer in het linkerdeelvenster onder Instellingen de optie **Regels voor opslaan in cache**.

   ![Knop voor opslaan in cache regels CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   De pagina **Regels voor opslaan in cache** wordt weergegeven.

   ![CDN caching-pagina](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Een web-Cache-Control-headers van de server met behulp van algemene regels voor opslaan in cache instellen:**

1. Onder **algemene regels voor caching**, stel **queryreeks cachegedrag** naar **querytekenreeksen negeren** en stel **cachegedrag** naar  **Overschrijven**.
      
1. Voor **Vervaltijd van de Cache**, voer 3600 in de **seconden** box of 1 in de **uur** vak. 

   ![CDN algemene opslaan in cache regels voorbeeld](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Deze algemene regel voor opslaan in cache wordt de Cacheduur van een van één uur en is van invloed op alle aanvragen naar het eindpunt. Dit vervangt alle `Cache-Control` of `Expires` HTTP-headers die worden verzonden door de originele server door het eindpunt opgegeven.   

1. Selecteer **Opslaan**.

**Een webserver Cache-Control-headers van het bestand met behulp van aangepaste cacheregels instellen:**

1. Onder **aangepaste cacheregels**, maakt u twee criteria voor overeenkomst:

     a. Voor de eerste voorwaarde voor overeenkomst, stelt u **overeenkomen met de voorwaarde** naar **pad** en voer `/webfolder1/*` voor **overeenkomen met waarde**. Instellen **cachegedrag** naar **overschrijven** en voert u 4 in de **uur** vak.

     b. Voor de tweede voorwaarde voor overeenkomst, stelt u **overeenkomen met de voorwaarde** naar **pad** en voer `/webfolder1/file1.txt` voor **overeenkomen met waarde**. Instellen **cachegedrag** naar **overschrijven** en voer 2 in de **uur** vak.

    ![Aangepaste caching regels voorbeeld van CDN](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    De eerste aangepaste cacheregel Hiermee stelt u een Cacheduur van vier uur voor alle bestanden in de `/webfolder1` map op de oorspronkelijke server door het eindpunt opgegeven. De tweede regel overschrijft de eerste regel voor de `file1.txt` alleen bestands- en Hiermee stelt u een Cacheduur van twee uur.

1. Selecteer **Opslaan**.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Cache-Control-headers instellen met behulp van configuratiebestanden
Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie beheren door het wijzigen van de **applicationHost.config** of **Web.config** configuratiebestanden voor uw webtoepassing. Om in te stellen de `Cache-Control` -header voor de inhoud, gebruik de `<system.webServer>/<staticContent>/<clientCache>` element in een bestand.

### <a name="using-applicationhostconfig-files"></a>Met behulp van ApplicationHost.config bestanden
De **ApplicationHost.config** bestand is het bestand van het basiscertificaat van de IIS-configuratie-systeem. De configuratie-instellingen in een **ApplicationHost.config** bestand invloed op alle toepassingen op de site, maar worden overschreven door de instellingen van een **Web.config** bestanden die aanwezig zijn voor een webtoepassing.

### <a name="using-webconfig-files"></a>Met behulp van Web.config-bestanden
Met een **Web.config** -bestand, kunt u de manier waarop uw hele web-App of een specifieke map op uw web-App-gedrag aanpassen. Normaal gesproken hebt u ten minste één **Web.config** bestand in de hoofdmap van uw webtoepassing. Voor elk **Web.config** bestand in een specifieke map, de configuratie-instellingen van invloed zijn op alles in die map en submappen, tenzij ze worden overschreven op het niveau van de submap door een andere **Web.config** het bestand. 

U kunt bijvoorbeeld instellen een `<clientCache>` -element in een **Web.config** bestand in de hoofdmap van uw webtoepassing alle statische inhoud in uw webtoepassing drie dagen in de cache. U kunt ook toevoegen een **Web.config** bestand in een submap met meer variabele inhoud (bijvoorbeeld `\frequent`) en stel de `<clientCache>` element van de submap inhoud in de cache voor de zes uur. Het resultaat is dat inhoud op de gehele website opgeslagen in de cache voor drie dagen, met uitzondering van alle inhoud in de `\frequent` directory, dat is opgeslagen in de cache voor alleen de zes uur.  

De volgende XML-configuratie-bestand-voorbeeld laat zien hoe om in te stellen de `<clientCache>` element om op te geven van een maximale leeftijd van drie dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Gebruik de **cacheControlMaxAge** kenmerk, moet u de waarde van instellen de **cacheControlMode** kenmerk `UseMaxAge`. Deze instelling veroorzaakt de HTTP-header en Richtlijn `Cache-Control: max-age=<nnn>`, moeten worden toegevoegd aan het antwoord. De indeling van de timespan-waarde voor de **cacheControlMaxAge** kenmerk is `<days>.<hours>:<min>:<sec>`. De waarde wordt geconverteerd naar seconden en wordt gebruikt als de waarde van de `Cache-Control` `max-age` richtlijn. Voor meer informatie over de `<clientCache>` -element, Zie [clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Cache-Control-koppen die instellen via een programma
Voor ASP.NET-toepassingen die u beheert de CDN-cachinggedrag via een programma door in te stellen de **HttpResponse.Cache** eigenschap van de .NET API. Voor informatie over de **HttpResponse.Cache** eigenschap, Zie [HttpResponse.Cache eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Voor het programmatisch cache toepassingsinhoud in ASP.NET, de volgende stappen uit:
   1. Controleer of dat de inhoud is gemarkeerd als gecachet kan worden door in te stellen `HttpCacheability` naar `Public`. 
   1. De validatie van een cache ingesteld door het aanroepen van een van de volgende `HttpCachePolicy` methoden:
      - Bel `SetLastModified` om in te stellen een waarde timestamp voor de `Last-Modified` header.
      - Bel `SetETag` om in te stellen een waarde voor de `ETag` header.
   1. (Optioneel) Geef de verlooptijd van een cache door het aanroepen van `SetExpires` om in te stellen een waarde voor de `Expires` header. Anders wordt de standaard-cache-methodiek eerder in dit document worden beschreven zijn van toepassing.

Bijvoorbeeld voor cache-inhoud voor één uur, voeg de volgende C#-code:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Testen van de Cache-Control-header
U kunt eenvoudig de TTL-instellingen van de inhoud van uw website controleren. Met van uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test of de webinhoud bevat de `Cache-Control` response-header. U kunt ook een hulpprogramma zoals gebruiken **wget**, [Postman](https://www.getpostman.com/), of [Fiddler](http://www.telerik.com/fiddler) om te controleren van de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Lees meer informatie over de **clientCache** element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Raadpleeg de documentatie bij de **HttpResponse.Cache** eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Raadpleeg de documentatie bij de **HttpCachePolicy klasse**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)  
* [Meer informatie over concepten opslaan in cache](cdn-how-caching-works.md)
