---
title: Cacheprovider voor ASP.NET-uitvoer-Cache
description: Meer informatie over het cache-uitvoer van de ASP.NET-pagina met Azure Redis Cache
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: wesmc
ms.openlocfilehash: 3cf906830965959709a8c7e8dc7d2acc3f3a6f32
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35917164"
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Provider voor de uitvoercache van ASP.NET voor Azure Redis-Cache
Cacheprovider Redis is een opslagmechanisme voor out-of-process-voor de uitvoergegevens van de cache. Deze gegevens is specifiek bedoeld voor volledige HTTP-antwoorden (pagina-opslaan in uitvoercache). De provider wordt in de nieuwe uitvoer cache provider extensibility point die is geïntroduceerd in ASP.NET 4.

Uw cache voor het eerst configureert voor het gebruik van de Redis-Provider voor de uitvoercache, en configureer vervolgens uw ASP.NET-toepassing met behulp van het Redis-uitvoer Cache Provider NuGet-pakket. Dit onderwerp bevat richtlijnen over het configureren van uw toepassing voor het gebruik van de Redis-Provider voor de uitvoercache. Zie voor meer informatie over het maken en configureren van een Azure Redis-Cache-exemplaar, [een cache maken](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET-pagina-uitvoer Store in de cache
Als u wilt een clienttoepassing configureren in Visual Studio met de Redis-Cache-sessie status NuGet-pakket, klikt u op **NuGet Package Manager**, **Package Manager Console** uit de **extra**menu.

Voer de volgende opdracht uit vanuit het venster `Package Manager Console`.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Het Redis-uitvoer Cache Provider NuGet-pakket is afhankelijk van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project is, wordt deze geïnstalleerd. Zie voor meer informatie over het Redis-uitvoer Cache Provider NuGet-pakket, de [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-pagina.

>[!NOTE]
>Naast het pakket StackExchange.Redis.StrongName sterke naam, er worden ook de StackExchange.Redis-versie niet-sterke naam. Als uw project van de niet-sterke naam StackExchange.Redis-versie dat moet u deze verwijderen gebruikmaakt, ophalen anders u naamconflicten in uw project. Zie voor meer informatie over deze pakketten [cacheclients configureren .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Het NuGet-pakket downloadt en voegt de vereiste assembly-verwijzingen en voegt de volgende sectie in het bestand web.config. In deze sectie bevat de vereiste configuratie voor uw ASP.NET-toepassing voor het gebruik van de Redis-Provider voor de uitvoercache.

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!-- For more details check https://github.com/Azure/aspnet-redis-providers/wiki -->
      <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
      <!-- 'databaseId' and 'applicationName' can be used with both options. -->
      <!--
      <add name="MyRedisOutputCache" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "1000" [number]
        connectionString = "<Valid StackExchange.Redis connection string>" [String]
        settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
        settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
        loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
        loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
        redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
  </outputCache>
</caching>
```

De opmerkingen sectie biedt een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache-blade in de Microsoft Azure portal en de andere waarden naar wens configureren. Zie voor instructies over het verkrijgen van toegang tot de eigenschappen van de cache, [configureren van Redis cache-instellingen](cache-configure.md#configure-redis-cache-settings).

* **host** – Geef het eindpunt van uw cache.
* **poort** – uw niet-SSL-poort of het SSL-poort, afhankelijk van de ssl-instellingen gebruiken.
* **accessKey** : de primaire of secundaire sleutel voor uw cache gebruiken.
* **SSL** – ' True ' als u wilt beveiligen van communicatie met de cache/client met ssl; anders ONWAAR. Zorg ervoor dat de juiste poort opgeven.
  * De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Geef true voor deze instelling in om de SSL-poort. Zie voor meer informatie over het inschakelen van de niet-SSL-poort, de [toegangspoorten](cache-configure.md#access-ports) sectie de [een cache configureren](cache-configure.md) onderwerp.
* **database-id** – opgegeven welke database u wilt gebruiken voor cache uitvoergegevens. Indien niet opgegeven, wordt de standaardwaarde van 0 wordt gebruikt.
* **applicationName** -sleutels worden opgeslagen in redis als `<AppName>_<SessionId>_Data`. Deze naamgevingsconventie schema kunt meerdere toepassingen delen dezelfde sleutel. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde wordt gebruikt.
* **connectionTimeoutInMilliseconds** : deze instelling kunt u de instelling connectTimeout in de StackExchange.Redis-client te negeren. Indien niet opgegeven, is de standaardinstelling connectTimeout van 5000 wordt gebruikt. Zie voor meer informatie, [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** : deze instelling kunt u de instelling syncTimeout in de StackExchange.Redis-client te negeren. Indien niet opgegeven, is de standaardinstelling syncTimeout van 1000 wordt gebruikt. Zie voor meer informatie, [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Een instructie OutputCache toevoegen aan elke pagina die u wilt de uitvoer in de cache.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

In het vorige voorbeeld wordt de pagina in de cache-gegevens blijven in de cache gedurende 60 seconden en een andere versie van de pagina voor elke combinatie van parameters in de cache is opgeslagen. Zie voor meer informatie over de instructie OutputCache [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

Zodra deze stappen worden uitgevoerd, wordt uw toepassing is geconfigureerd voor het gebruik van de Redis-Provider voor de uitvoercache.

## <a name="next-steps"></a>Volgende stappen
Bekijk de [ASP.NET-Sessiestatusprovider voor Azure Redis Cache](cache-aspnet-session-state-provider.md).

