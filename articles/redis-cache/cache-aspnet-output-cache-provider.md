---
title: Provider voor de ASP.NET-uitvoercache cache
description: Meer informatie over het in de cache van de uitvoer van de ASP.NET-pagina met behulp van Azure Redis-Cache
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
ms.openlocfilehash: 81c95949971d54833ca7a15ec5148116c94767f7
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Provider voor de uitvoercache van ASP.NET voor Azure Redis-Cache
De Redis-Provider voor de uitvoercache is een opslagmechanisme voor out-of-process-voor de uitvoergegevens van de cache. Deze gegevens zijn specifiek voor volledige HTTP-antwoorden (pagina uitvoercaching). De provider in het nieuwe uitvoer cache provider uitbreidbaarheid punt die is geïntroduceerd in ASP.NET 4 wordt geplaatst.

Uw cache eerst configureren voor het gebruik van de Redis-Provider voor de uitvoercache, en configureer vervolgens uw ASP.NET-toepassing met behulp van het Redis-uitvoer Cache Provider NuGet-pakket. In dit onderwerp bevat richtlijnen over het configureren van uw toepassing gebruik van de Redis-Provider voor de uitvoercache. Zie voor meer informatie over het maken en configureren van een Azure Redis-Cache-exemplaar [een cache maken](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET-pagina-uitvoer in de cache opslaan
Voor het configureren van een client-toepassing in Visual Studio met het Redis-Cache sessie status NuGet-pakket, klikt u op **NuGet Package Manager**, **Package Manager Console** van de **extra**menu.

Voer de volgende opdracht uit vanuit het venster `Package Manager Console`.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Het Redis-uitvoer Cache Provider NuGet-pakket heeft een afhankelijkheid op het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project is, wordt deze geïnstalleerd. Zie voor meer informatie over het Redis-uitvoer Cache Provider NuGet-pakket, de [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-pagina.

>[!NOTE]
>Naast het pakket StackExchange.Redis.StrongName sterke naam is er ook de versie StackExchange.Redis niet-sterke naam. Als uw project van de niet-sterke naam StackExchange.Redis-versie dat moet u deze verwijderen gebruikmaakt, ophalen anders u naamconflicten in uw project. Zie voor meer informatie over deze pakketten [cacheclients configureren .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Het NuGet-pakket downloadt en voegt de vereiste assembly-verwijzingen en voegt u de volgende sectie toe aan het web.config-bestand. Deze sectie bevat de vereiste configuratie voor uw ASP.NET-toepassing voor het gebruik van de Redis-Provider voor de uitvoercache.

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

De opmerkingen sectie biedt een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cacheblade in de Microsoft Azure-portal en de andere waarden naar wens configureren. Zie voor instructies over de toegang tot de eigenschappen van uw cache [configureren Redis-cache-instellingen](cache-configure.md#configure-redis-cache-settings).

* **host** – Geef uw cache-eindpunt.
* **poort** – uw niet-SSL-poort of uw SSL-poort, afhankelijk van de ssl-instellingen gebruiken.
* **accessKey** – de primaire of secundaire sleutel gebruiken voor uw cache.
* **SSL** – ' True ' wanneer u wilt beveiligen cache/clientcommunicatie met ssl; anders ONWAAR. Zorg ervoor dat de juiste poort opgeven.
  * De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Geef waar voor deze instelling om de SSL-poort te gebruiken. Zie voor meer informatie over het inschakelen van de niet-SSL-poort, het [toegangspoorten](cache-configure.md#access-ports) sectie het [een cache configureren](cache-configure.md) onderwerp.
* **databaseId** – opgegeven welke database moet worden gebruikt voor cache uitvoergegevens. Als niet wordt opgegeven, wordt de standaardwaarde van 0 gebruikt.
* **applicationName** – sleutels worden opgeslagen in redis als `<AppName>_<SessionId>_Data`. Deze schematische kan meerdere toepassingen delen dezelfde sleutel. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde wordt gebruikt.
* **connectionTimeoutInMilliseconds** – deze instelling kunt u de instelling connectTimeout in de client StackExchange.Redis te negeren. Als niet wordt opgegeven, wordt de standaardinstelling connectTimeout van 5000 gebruikt. Zie voor meer informatie [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – deze instelling kunt u de instelling syncTimeout in de client StackExchange.Redis te negeren. Als niet wordt opgegeven, wordt de standaardinstelling syncTimeout 1000 gebruikt. Zie voor meer informatie [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Een instructie OutputCache toevoegen aan elke pagina waarvoor u de uitvoer van de cache wilt opslaan.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

In het vorige voorbeeld wordt de pagina in de cache-gegevens blijven in de cache gedurende 60 seconden en een andere versie van de pagina voor elke parametercombinatie in de cache wordt opgeslagen. Zie voor meer informatie over de instructie OutputCache [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

Zodra deze stappen worden uitgevoerd, wordt uw toepassing geconfigureerd voor gebruik van de Redis-Provider voor de uitvoercache.

## <a name="next-steps"></a>Volgende stappen
Bekijk de [ASP.NET Session State-Provider voor Azure Redis-Cache](cache-aspnet-session-state-provider.md).

