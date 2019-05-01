---
title: Provider voor de uitvoercache van ASP.NET voor Azure Cache voor Redis
description: Leer hoe u ASP.NET-uitvoer met behulp van Azure Cache voor Redis cache
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 04/22/2018
ms.author: yegu
ms.openlocfilehash: a93d21b07dc486f743694ee99f60018ed4ef517c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943874"
---
# <a name="aspnet-output-cache-provider-for-azure-cache-for-redis"></a>Provider voor de uitvoercache van ASP.NET voor Azure Cache voor Redis

Cacheprovider Redis is een opslagmechanisme voor out-of-process-voor de uitvoergegevens van de cache. Deze gegevens is specifiek bedoeld voor volledige HTTP-antwoorden (pagina-opslaan in uitvoercache). De provider wordt in de nieuwe uitvoer cache provider extensibility point die is geïntroduceerd in ASP.NET 4.

Uw cache voor het eerst configureert voor het gebruik van de Redis-Provider voor de uitvoercache, en configureer vervolgens uw ASP.NET-toepassing met behulp van het Redis-uitvoer Cache Provider NuGet-pakket. Dit onderwerp bevat richtlijnen over het configureren van uw toepassing voor het gebruik van de Redis-Provider voor de uitvoercache. Zie voor meer informatie over het maken en configureren van een Azure-Cache voor Redis-exemplaar, [een cache maken](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET-pagina-uitvoer Store in de cache

Als u wilt een clienttoepassing configureren in Visual Studio met de Azure-Cache voor Redis-sessie status NuGet-pakket, klikt u op **NuGet Package Manager**, **Package Manager Console** uit de **hulpprogramma's**  menu.

Voer de volgende opdracht uit vanuit het venster `Package Manager Console`.

```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Het Redis-uitvoer Cache Provider NuGet-pakket is afhankelijk van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project is, wordt deze geïnstalleerd. Zie voor meer informatie over het Redis-uitvoer Cache Provider NuGet-pakket, de [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-pagina.

>[!NOTE]
>Naast het pakket StackExchange.Redis.StrongName sterke naam, er worden ook de StackExchange.Redis-versie niet-sterke naam. Als uw project met behulp van de niet-sterke naam StackExchange.Redis-versie moet u verwijderen. anders wordt er naamconflicten optreden in uw project. Zie voor meer informatie over deze pakketten [cacheclients configureren .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Het NuGet-pakket downloadt en voegt de vereiste assembly-verwijzingen en voegt de volgende sectie in het bestand web.config. In deze sectie bevat de vereiste configuratie voor uw ASP.NET-toepassing voor het gebruik van de Redis-Provider voor de uitvoercache.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

De kenmerken configureren met de waarden uit de cache-blade in de Microsoft Azure portal en de andere waarden naar wens configureren. Zie voor instructies over het verkrijgen van toegang tot de eigenschappen van de cache, [Azure-Cache configureren voor Redis-instellingen](cache-configure.md#configure-azure-cache-for-redis-settings).

| Kenmerk | Type | Standaard | Description |
| --------- | ---- | ------- | ----------- |
| *host* | string | "localhost" | Het Redis-server IP-adres of de hostnaam naam |
| *Poort* | positief geheel getal zijn | 6379 (niet-SSL)<br/>6380 (SSL) | Redis-server-poort |
| *accessKey* | string | "" | Redis serverwachtwoord als de Redis-autorisatie is ingeschakeld. De waarde is een lege tekenreeks Standaard, wat betekent dat de sessiestatusprovider niet elk wachtwoord gebruikt dat bij het verbinden met Redis-server. **Als uw Redis-server zich in een openbaar netwerk, zoals Azure Redis Cache, zorg ervoor dat u het inschakelen van Redis-autorisatie om beveiliging te verbeteren en geef een veilig wachtwoord.** |
| *ssl* | booleaans | **false** | Hiermee geeft u op of u verbinding maken met Redis-server via SSL. Deze waarde is **false** standaard omdat Redis biedt geen ondersteuning voor SSL buiten het vak. **Als u gebruikmaakt van Azure Redis Cache die ondersteuning biedt SSL gebruiksklaar, moet u deze optie instelt op waar de beveiliging verbeteren.**<br/><br/>De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Geef **waar** voor deze instelling om de SSL-poort te gebruiken. Zie voor meer informatie over het inschakelen van de niet-SSL-poort, de [toegangspoorten](cache-configure.md#access-ports) sectie de [een cache configureren](cache-configure.md) onderwerp. |
| *databaseIdNumber* | positief geheel getal zijn | 0 | *Dit kenmerk kan alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Geef op welke Redis-database moet worden gebruikt. |
| *connectionTimeoutInMilliseconds* | positief geheel getal zijn | Geleverd door StackExchange.Redis | Gebruikt voor het instellen *ConnectTimeout* bij het maken van StackExchange.Redis.ConnectionMultiplexer. |
| *operationTimeoutInMilliseconds* | positief geheel getal zijn | Geleverd door StackExchange.Redis | Gebruikt voor het instellen *SyncTimeout* bij het maken van StackExchange.Redis.ConnectionMultiplexer. |
| *connectionString* (verbindingsreeks geldig StackExchange.Redis) | string | *n/a* | Een van beide een parameterverwijzing naar AppSettings of web.config, anders een geldige StackExchange.Redis-verbindingsreeks. Dit kenmerk kunt waarden opgeven voor *host*, *poort*, *accessKey*, *ssl*, en andere kenmerken StackExchange.Redis. Voor nader bekijken *connectionString*, Zie [connectionString instellen](#setting-connectionstring) in de [kenmerk opmerkingen bij de](#attribute-notes) sectie. |
| *settingsClassName*<br/>*settingsMethodName* | string<br/>string | *n/a* | *Deze kenmerken kunnen alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Deze kenmerken gebruiken om een verbindingsreeks. *settingsClassName* moet de naam van een assembly-gekwalificeerde assemblyklasse met de methode die is opgegeven door *settingsMethodName*.<br/><br/>De methode die is opgegeven door *settingsMethodName* moet openbare, statische en void (geen rekening parameters), met het resultaattype **tekenreeks**. Deze methode retourneert de werkelijke verbindingsreeks. |
| *loggingClassName*<br/>*loggingMethodName* | string<br/>string | *n/a* | *Deze kenmerken kunnen alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Deze kenmerken gebruiken voor foutopsporing van uw toepassing door te geven van Logboeken van de sessiestatus/uitvoercache samen met de logboeken van StackExchange.Redis. *loggingClassName* moet de naam van een assembly-gekwalificeerde assemblyklasse met de methode die is opgegeven door *loggingMethodName*.<br/><br/>De methode die is opgegeven door *loggingMethodName* moet openbare, statische en void (geen rekening parameters), met het resultaattype **System.IO.TextWriter**. |
| *applicationName* | string | De naam van de module van het huidige proces of '/' | *Alleen SessionStateProvider*<br/>*Dit kenmerk kan alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Het voorvoegsel van de app te gebruiken in Redis-cache. De klant kan de dezelfde Redis-cache gebruiken voor verschillende doeleinden. Als u wilt ervoor zorgen dat de sessiesleutels geen conflict veroorzaken, kan deze worden voorafgegaan door de naam van de toepassing. |
| *throwOnError* | booleaans | true | *Alleen SessionStateProvider*<br/>*Dit kenmerk kan alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Hiermee geeft u op of u een uitzondering is inzetten wanneer er een fout optreedt.<br/><br/>Voor meer informatie over *throwOnError*, Zie [op notities *throwOnError* ](#notes-on-throwonerror) in de [kenmerk opmerkingen bij de](#attribute-notes) sectie. |>*Microsoft.Web.Redis.RedisSessionStateProvider.LastException*. |
| *retryTimeoutInMilliseconds* | positief geheel getal zijn | 5000 | *Alleen SessionStateProvider*<br/>*Dit kenmerk kan alleen via web.config of AppSettings worden opgegeven.*<br/><br/>Hoe lang is om opnieuw te proberen wanneer een bewerking is mislukt. Als deze waarde is minder dan *operationTimeoutInMilliseconds*, de provider wordt niet opnieuw uitgevoerd.<br/><br/>Voor meer informatie over *retryTimeoutInMilliseconds*, Zie [op notities *retryTimeoutInMilliseconds* ](#notes-on-retrytimeoutinmilliseconds) in de [kenmerk opmerkingen bij de](#attribute-notes) sectie. |
| *redisSerializerType* | string | *n/a* | Hiermee geeft u de assembly gekwalificeerde typenaam van een klasse die Microsoft.Web.Redis implementeert. ISerializer en dat bevat de aangepaste logica te serialiseren en deserialiseren van de waarden. Zie voor meer informatie, [over *redisSerializerType* ](#about-redisserializertype) in de [kenmerk opmerkingen bij de](#attribute-notes) sectie. |
|

## <a name="attribute-notes"></a>Opmerkingen bij de kenmerk

### <a name="setting-connectionstring"></a>Instellen van *connectionString*

De waarde van *connectionString* als sleutel wordt gebruikt voor het ophalen van de werkelijke verbindingsreeks van AppSettings, als deze een tekenreeks in AppSettings bestaat. Als dat niet gevonden in AppSettings, de waarde van *connectionString* als sleutel wordt gebruikt voor het ophalen van de werkelijke verbindingsreeks uit het bestand web.config **ConnectionString** sectie, als die sectie bestaat. Als de verbindingsreeks niet bestaat in AppSettings of het bestand web.config **ConnectionString** sectie, de letterlijke waarde van *connectionString* als de verbindingstekenreeks wordt gebruikt bij het maken van StackExchange.Redis.ConnectionMultiplexer.

De volgende voorbeelden ziet u hoe *connectionString* wordt gebruikt.

#### <a name="example-1"></a>Voorbeeld 1

```xml
<connectionStrings>
    <add name="MyRedisConnectionString" connectionString="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</connectionStrings>
```

In `web.config`, hierboven sleutel als waarde voor de parameter in plaats van de werkelijke waarde gebruiken.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-2"></a>Voorbeeld 2

```xml
<appSettings>
    <add key="MyRedisConnectionString" value="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</appSettings>
```

In `web.config`, hierboven sleutel als waarde voor de parameter in plaats van de werkelijke waarde gebruiken.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-3"></a>Voorbeeld 3

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False"/>
    </providers>
</sessionState>
```

### <a name="notes-on-throwonerror"></a>Opmerkingen bij de op *throwOnError*

Als er een fout optreedt tijdens een sessiebewerking, wordt de sessiestatusprovider op dit moment een uitzondering genereert. Hiermee wordt de toepassing afgesloten.

Dit gedrag is gewijzigd op een manier die ondersteuning biedt voor de verwachtingen van de bestaande ASP.NET-sessie status provider-gebruikers en tegelijkertijd de mogelijkheid om te reageren op uitzonderingen, indien gewenst. Het standaardgedrag worden nog steeds een uitzondering genereert wanneer er een fout optreedt, consistent zijn met andere statusproviders; voor de ASP.NET-sessie bestaande code moet werken hetzelfde als voorheen.

Als u instelt *throwOnError* naar **false**, in plaats van die een uitzondering veroorzaakt wanneer er een fout optreedt, wordt deze mislukt op de achtergrond. Als u wilt zien als er een fout opgetreden is en, als dit het geval is, Ontdek wat de uitzondering was, controleert u de statische eigenschap *Microsoft.Web.Redis.RedisSessionStateProvider.LastException*.

### <a name="notes-on-retrytimeoutinmilliseconds"></a>Opmerkingen bij de op *retryTimeoutInMilliseconds*

Dit biedt enkele logica voor opnieuw proberen ter vereenvoudiging van het geval waarbij een sessiebewerking moet opnieuw bij de fout vanwege een netwerkfout opgetreden, zoals terwijl het ook zodat u kunt de time-out voor opnieuw proberen of helemaal afmelden voor opnieuw proberen.

Als u instelt *retryTimeoutInMilliseconds* naar een getal, bijvoorbeeld 2000, klikt u vervolgens als een sessiebewerking is mislukt, wordt opnieuw uitgevoerd voor 2000 milliseconden voordat u deze behandelen als een fout. Dus als u wilt de sessiestatusprovider om toe te passen deze logica voor opnieuw proberen, alleen de time-out te configureren. De eerste poging gebeurt na 20 milliseconden, die voldoende in de meeste gevallen is als een netwerkfout opgetreden gebeurt. Hierna wordt opnieuw uitgevoerd per seconde totdat er een optreedt time-out. Er wordt opnieuw geprobeerd nog een keer om ervoor te zorgen dat deze wordt niet de time-out (maximaal) één seconde afgekapt direct na de time-out.

Als u niet denkt dat u nodig hebt (bijvoorbeeld, wanneer u de Redis-server op dezelfde computer als uw toepassing uitvoert) probeer het opnieuw of als u wilt verwerken van de logica voor opnieuw proberen zelf, stelt u *retryTimeoutInMilliseconds* op 0.

### <a name="about-redisserializertype"></a>Over *redisSerializerType*

De serialisatie voor het opslaan van de waarden van Redis wordt standaard uitgevoerd in een binaire indeling die is geleverd door de **BinaryFormatter** klasse. Gebruik *redisSerializerType* om op te geven van de assembly gekwalificeerde typenaam van een klasse die **Microsoft.Web.Redis.ISerializer** en heeft de aangepaste logica te serialiseren en deserialiseren van de waarden. Dit is bijvoorbeeld een Json-serializer-klasse met behulp van JSON.NET:

```cs
namespace MyCompany.Redis
{
    public class JsonSerializer : ISerializer
    {
        private static JsonSerializerSettings _settings = new JsonSerializerSettings() { TypeNameHandling = TypeNameHandling.All };

        public byte[] Serialize(object data)
        {
            return Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(data, _settings));
        }

        public object Deserialize(byte[] data)
        {
            if (data == null)6t6
            {
                return null;
            }
            return JsonConvert.DeserializeObject(Encoding.UTF8.GetString(data), _settings);
        }
    }
}
```

Ervan uitgaande dat deze klasse is gedefinieerd in een assembly met de naam **MyCompanyDll**, kunt u instellen dat de parameter *redisSerializerType* om deze te gebruiken:

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvider"
             name = "MySessionStateStore"
             redisSerializerType = "MyCompany.Redis.JsonSerializer,MyCompanyDll"
             ... />
    </providers>
</sessionState>
```

## <a name="output-cache-directive"></a>De instructie cache uitvoer

Een instructie OutputCache toevoegen aan elke pagina die u wilt de uitvoer in de cache.

```xml
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

In het vorige voorbeeld wordt de pagina in de cache-gegevens blijven in de cache gedurende 60 seconden en een andere versie van de pagina voor elke combinatie van parameters in de cache is opgeslagen. Zie voor meer informatie over de instructie OutputCache [ @OutputCache ](https://go.microsoft.com/fwlink/?linkid=320837).

Zodra deze stappen worden uitgevoerd, wordt uw toepassing is geconfigureerd voor het gebruik van de Redis-Provider voor de uitvoercache.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis](cache-aspnet-session-state-provider.md).
