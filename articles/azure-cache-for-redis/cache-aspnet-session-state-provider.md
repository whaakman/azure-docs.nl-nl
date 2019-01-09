---
title: Cache ASP.NET-sessiestatus-Provider | Microsoft Docs
description: Meer informatie over het opslaan van ASP.NET-sessiestatus met Azure Cache voor Redis
services: azure-cache-for-redis
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: azure-cache-for-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: wesmc
ms.openlocfilehash: a7f3e23cd74baa2e1fdef178be8c5b213a3905ef
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105284"
---
# <a name="aspnet-session-state-provider-for-azure-cache-for-redis"></a>ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis
Azure Redis-Cache biedt een sessiestatusprovider die u gebruiken kunt voor het opslaan van uw sessie staat in het geheugen met Azure Cache voor Redis in plaats van een SQL Server-database. Uw cache voor het eerst configureert voor het gebruik van de sessiestatusprovider voor opslaan in cache, en configureer vervolgens uw ASP.NET-toepassing voor de cache met behulp van de Azure-Cache voor Redis-sessie status NuGet-pakket.

Het is vaak niet praktisch in een echte cloud-app om te voorkomen dat een vorm van de status op te slaan voor een gebruikerssessie, maar sommige methoden invloed hebben op prestaties en schaalbaarheid van meer dan andere. Als u hebt voor het opslaan van status, is de beste oplossing voor het bedrag van de status klein te houden en op te slaan in cookies. Als dit niet haalbaar is, wordt de volgende aanbevolen oplossing is het gebruik van ASP.NET-sessiestatus met een provider voor gedistribueerde in-memory cache. De slechtste oplossing van prestaties en schaalbaarheid vanuit het oogpunt is het gebruik van een database back-sessiestatusprovider. Dit onderwerp bevat richtlijnen over het gebruik van de ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis. Zie voor meer informatie over andere opties van de status sessie [opties voor ASP.NET-sessiestatus](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>De ASP.NET-sessiestatus in de cache opslaan
Als u wilt een clienttoepassing configureren in Visual Studio met de Azure-Cache voor Redis-sessie status NuGet-pakket, klikt u op **NuGet Package Manager**, **Package Manager Console** uit de **hulpprogramma's**  menu.

Voer de volgende opdracht uit vanuit het venster `Package Manager Console`.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Als u van de functie voor failoverclustering van de premium-laag gebruikmaakt, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger of een uitzondering is opgetreden. Verplaatsen naar 2.0.1 of hoger is een belangrijke wijziging; Zie voor meer informatie, [v2.0.0 Details van de belangrijke wijziging](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). Op het moment van de update van dit artikel is de huidige versie van dit pakket 2.2.3.
> 
> 

Het Redis-sessie State-Provider NuGet-pakket is afhankelijk van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project is, wordt deze geïnstalleerd.

>[!NOTE]
>Naast het pakket StackExchange.Redis.StrongName sterke naam, er worden ook de StackExchange.Redis-versie niet-sterke naam. Als uw project van de niet-sterke naam StackExchange.Redis-versie dat moet u deze verwijderen gebruikmaakt, ophalen anders u naamconflicten in uw project. Zie voor meer informatie over deze pakketten [cacheclients configureren .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Het NuGet-pakket downloadt en voegt de vereiste assembly-verwijzingen en voegt de volgende sectie in het bestand web.config. In deze sectie bevat de vereiste configuratie voor uw ASP.NET-toepassing met de Azure-Cache voor Sessiestatusprovider Redis.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
  <providers>
    <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
    <!-- 'throwOnError','retryTimeoutInMilliseconds','databaseId' and 'applicationName' can be used with both options. -->
    <!--
      <add name="MySessionStateStore" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "5000" [number]
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
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider"
         host=""
         accessKey=""
         ssl="true" />
</sessionState>
```

De opmerkingen sectie biedt een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache-blade in de Microsoft Azure portal en de andere waarden naar wens configureren. Zie voor instructies over het verkrijgen van toegang tot de eigenschappen van de cache, [Azure-Cache configureren voor Redis-instellingen](cache-configure.md#configure-azure-cache-for-redis-settings).

* **host** – Geef het eindpunt van uw cache.
* **poort** – uw niet-SSL-poort of het SSL-poort, afhankelijk van de ssl-instellingen gebruiken.
* **accessKey** : de primaire of secundaire sleutel voor uw cache gebruiken.
* **SSL** – ' True ' als u wilt beveiligen van communicatie met de cache/client met ssl; anders ONWAAR. Zorg ervoor dat de juiste poort opgeven.
  * De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Geef true voor deze instelling in om de SSL-poort. Zie voor meer informatie over het inschakelen van de niet-SSL-poort, de [toegangspoorten](cache-configure.md#access-ports) sectie de [een cache configureren](cache-configure.md) onderwerp.
* **throwOnError** – ' True ' als u wilt dat een uitzondering gegenereerd als er een fout, of ONWAAR als u wilt dat de bewerking niet uitvoeren op de achtergrond. U kunt controleren wegens een fout door het controleren van de statische Microsoft.Web.Redis.RedisSessionStateProvider.LastException-eigenschap. De standaardwaarde is true.
* **retryTimeoutInMilliseconds** – bewerkingen die mislukken opnieuw kunnen worden uitgevoerd tijdens deze periode, in milliseconden opgegeven. De eerste poging gebeurt na 20 milliseconden en vervolgens nieuwe pogingen uitgevoerd per seconde totdat het retryTimeoutInMilliseconds-interval is verstreken. Direct na deze periode, is de bewerking opnieuw een laatste keer uitgevoerd. Als de bewerking nog steeds mislukt, wordt de uitzondering terug naar de aanroeper, afhankelijk van de instelling throwOnError opgetreden. De standaardwaarde is 0, wat betekent er geen nieuwe pogingen dat.
* **database-id** – Hiermee geeft u op welke database u wilt gebruiken voor de uitvoergegevens van de cache. Indien niet opgegeven, wordt de standaardwaarde van 0 wordt gebruikt.
* **applicationName** -sleutels worden opgeslagen in redis als `{<Application Name>_<Session ID>}_Data`. Deze naamgevingsconventie schema kunt meerdere toepassingen delen de dezelfde Redis-exemplaar. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde wordt gebruikt.
* **connectionTimeoutInMilliseconds** : deze instelling kunt u de instelling connectTimeout in de StackExchange.Redis-client te negeren. Indien niet opgegeven, is de standaardinstelling connectTimeout van 5000 wordt gebruikt. Zie voor meer informatie, [configuratiemodel StackExchange.Redis](https://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** : deze instelling kunt u de instelling syncTimeout in de StackExchange.Redis-client te negeren. Indien niet opgegeven, is de standaardinstelling syncTimeout van 1000 wordt gebruikt. Zie voor meer informatie, [configuratiemodel StackExchange.Redis](https://go.microsoft.com/fwlink/?LinkId=398705).
* **redisSerializerType** -deze instelling kunt u aangepaste serialisatie van sessie-inhoud die wordt verzonden naar Redis opgeven. Het opgegeven type moet implementeren `Microsoft.Web.Redis.ISerializer` en openbare constructor zonder parameters moet worden gedeclareerd. Standaard `System.Runtime.Serialization.Formatters.Binary.BinaryFormatter` wordt gebruikt.

Zie voor meer informatie over deze eigenschappen op de oorspronkelijke blogberichtaankondiging [aankondiging van ASP.NET-Sessiestatusprovider voor Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Vergeet niet te opmerkingen bij de standaard InProc-sessie status sectie provider in de web.config.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

Zodra deze stappen worden uitgevoerd, wordt uw toepassing is geconfigureerd voor het gebruik van de Azure-Cache voor Sessiestatusprovider Redis. Als u sessiestatus in uw toepassing gebruikt, wordt deze opgeslagen in een Azure-Cache voor Redis-exemplaar.

> [!IMPORTANT]
> Gegevens die zijn opgeslagen in de cache moet worden geserialiseerd, in tegenstelling tot de gegevens die kunnen worden opgeslagen in de standaard ASP.NET-sessie in het geheugen Provider status. Wanneer de Sessiestatusprovider voor Redis wordt gebruikt, moet u dat de gegevenstypen die worden opgeslagen in de status van een sessie geserialiseerd worden.
> 
> 

## <a name="aspnet-session-state-options"></a>Opties voor ASP.NET-sessiestatus
* In geheugen sessiestatus-Provider: slaat deze provider op de status van de sessie in het geheugen. Het voordeel van het gebruik van deze provider is dat het eenvoudig en snel is. U kunt echter uw Web-Apps kan niet schalen als u in het geheugen provider omdat deze niet wordt gedistribueerd.
* SQL Server-sessiestatus-Provider: deze provider slaat de sessiestatus in Sql Server. Gebruik deze provider als u wilt de sessiestatus wordt opgeslagen in een permanente opslag. U kunt uw Web-App schalen, maar met behulp van Sql Server voor de sessie een prestatie-impact heeft op uw Web-App. U kunt ook deze provider met een [In-Memory OLTP configuratie](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/11/28/asp-net-session-state-with-sql-server-in-memory-oltp/) om prestaties te verbeteren.
* Gedistribueerd In geheugen Sessiestatusprovider zoals Azure Cache voor Redis-Sessiestatusprovider - deze provider kunt u het beste van beide werelden. Uw Web-App kan een eenvoudige, snelle en schaalbare Sessiestatusprovider hebben. Omdat deze provider wordt de sessiestatus in een Cache opgeslagen, is uw app te nemen in overweging alle kenmerken die is gekoppeld als communicatie met een gedistribueerd In cachegeheugen, zoals tijdelijke netwerkfouten. Zie voor aanbevolen procedures voor het gebruik van Cache [richtlijnen voor Caching](../best-practices-caching.md) vanuit Microsoft Patterns & Practices [toepassingsontwerp voor Azure-Cloud en begeleiding bij implementatie](https://github.com/mspnp/azure-guidance).

Zie voor meer informatie over de sessiestatus en andere best practices, [aanbevolen procedures voor de ontwikkeling van Web (Building Real-World Cloud Apps with Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Volgende stappen
Bekijk de [Cacheprovider voor ASP.NET voor Azure Cache voor Redis](cache-aspnet-output-cache-provider.md).

