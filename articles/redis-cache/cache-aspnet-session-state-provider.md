---
title: Cache ASP.NET Session State-Provider | Microsoft Docs
description: Meer informatie over het opslaan van ASP.NET-sessiestatus met Azure Redis-Cache
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: wesmc
ms.openlocfilehash: 485375f2f2ffb83b7d0fdeef8daab5880a8bbc27
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET-sessiestatusprovider voor Azure Redis-Cache
Azure Redis-Cache biedt een sessiestatus-provider die u kunt de sessiestatus wordt opgeslagen in een cache in plaats van in het geheugen of in een SQL Server-database. Voor het gebruik van de cache in sessiestatus-provider voor uw cache eerst configureren en configureer vervolgens uw ASP.NET-toepassing voor de cache met behulp van het Redis-Cache sessie status NuGet-pakket.

Is het vaak niet praktisch in een echte cloud-app om te voorkomen dat een vorm van de status op te slaan voor een gebruikerssessie, maar sommige benaderingen invloed op prestaties en schaalbaarheid meer dan andere. Als u hebt voor het opslaan van de status, is de beste oplossing de hoeveelheid status klein te houden en in cookies opslaan. Als dat niet mogelijk is, is de volgende aanbevolen oplossing voor het gebruik van ASP.NET-sessiestatus met een provider voor gedistribueerde, in het geheugen-cache. De slechtste oplossing van een prestaties en schaalbaarheid oogpunt is back sessiestatus-provider voor een database te gebruiken. In dit onderwerp bevat richtlijnen over het gebruik van het ASP.NET Session State-Provider voor Azure Redis-Cache. Zie voor informatie over andere opties van de status sessie [opties voor ASP.NET-sessiestatus](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>De ASP.NET-sessiestatus in de cache opslaan
Voor het configureren van een client-toepassing in Visual Studio met het Redis-Cache sessie status NuGet-pakket, klikt u op **NuGet Package Manager**, **Package Manager Console** van de **extra**menu.

Voer de volgende opdracht uit vanuit het venster `Package Manager Console`.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Als u de functie Failoverclustering van de laag premium gebruikt, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger, of een uitzondering gegenereerd. Verplaatsen naar 2.0.1 of nieuwer is een belangrijke wijziging; Zie voor meer informatie [v2.0.0 Details van de wijziging op te splitsen](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). De huidige versie van dit pakket is op het moment van deze update artikel 2.2.3.
> 
> 

Het Redis-sessie status Provider NuGet-pakket heeft een afhankelijkheid op het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project is, wordt deze geïnstalleerd.

>[!NOTE]
>Naast het pakket StackExchange.Redis.StrongName sterke naam is er ook de versie StackExchange.Redis niet-sterke naam. Als uw project van de niet-sterke naam StackExchange.Redis-versie dat moet u deze verwijderen gebruikmaakt, ophalen anders u naamconflicten in uw project. Zie voor meer informatie over deze pakketten [cacheclients configureren .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Het NuGet-pakket downloadt en voegt de vereiste assembly-verwijzingen en voegt u de volgende sectie toe aan het web.config-bestand. Deze sectie bevat de vereiste configuratie voor uw ASP.NET-toepassing voor het gebruik van de Redis-Cache sessiestatus-Provider.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

De opmerkingen sectie biedt een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cacheblade in de Microsoft Azure-portal en de andere waarden naar wens configureren. Zie voor instructies over de toegang tot de eigenschappen van uw cache [configureren Redis-cache-instellingen](cache-configure.md#configure-redis-cache-settings).

* **host** – Geef uw cache-eindpunt.
* **poort** – uw niet-SSL-poort of uw SSL-poort, afhankelijk van de ssl-instellingen gebruiken.
* **accessKey** – de primaire of secundaire sleutel gebruiken voor uw cache.
* **SSL** – ' True ' wanneer u wilt beveiligen cache/clientcommunicatie met ssl; anders ONWAAR. Zorg ervoor dat de juiste poort opgeven.
  * De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Geef waar voor deze instelling om de SSL-poort te gebruiken. Zie voor meer informatie over het inschakelen van de niet-SSL-poort, het [toegangspoorten](cache-configure.md#access-ports) sectie het [een cache configureren](cache-configure.md) onderwerp.
* **throwOnError** : true als u wilt dat een uitzondering gegenereerd als er een fout of ONWAAR als u wilt dat de bewerking mislukt achtergrond. U kunt voor een mislukte controleren door het controleren van de statische Microsoft.Web.Redis.RedisSessionStateProvider.LastException-eigenschap. De standaardwaarde is true.
* **retryTimeoutInMilliseconds** – bewerkingen die mislukken worden opnieuw uitgevoerd tijdens dit interval, in milliseconden opgegeven. De eerste poging plaatsvindt na 20 milliseconden en vervolgens nieuwe pogingen uitgevoerd voor elke seconde totdat het retryTimeoutInMilliseconds-interval is verstreken. De bewerking is één van de laatste keer opnieuw geprobeerd onmiddellijk na dit interval. Als de bewerking steeds mislukt, de uitzondering terug naar de aanroeper, afhankelijk van de instelling throwOnError. De standaardwaarde is 0, wat betekent er geen nieuwe pogingen dat.
* **databaseId** – Hiermee geeft u op welke database moet worden gebruikt voor uitvoergegevens van de cache. Als niet wordt opgegeven, wordt de standaardwaarde van 0 gebruikt.
* **applicationName** – sleutels worden opgeslagen in redis als `{<Application Name>_<Session ID>}_Data`. Deze schematische kan meerdere toepassingen hetzelfde Redis-exemplaar. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde wordt gebruikt.
* **connectionTimeoutInMilliseconds** – deze instelling kunt u de instelling connectTimeout in de client StackExchange.Redis te negeren. Als niet wordt opgegeven, wordt de standaardinstelling connectTimeout van 5000 gebruikt. Zie voor meer informatie [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – deze instelling kunt u de instelling syncTimeout in de client StackExchange.Redis te negeren. Als niet wordt opgegeven, wordt de standaardinstelling syncTimeout 1000 gebruikt. Zie voor meer informatie [configuratiemodel StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Zie voor meer informatie over deze eigenschappen op de oorspronkelijke blogberichtaankondiging [aankondigen van ASP.NET Session State-Provider voor Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Vergeet niet te uitcommentariëren de standaard InProc sessie status provider sectie in het web.config-bestand.

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

Zodra deze stappen worden uitgevoerd, wordt uw toepassing geconfigureerd voor gebruik van de Redis-Cache sessiestatus-Provider. Wanneer u sessiestatus in uw toepassing gebruikt, wordt deze opgeslagen in een Azure Redis-Cache-exemplaar.

> [!IMPORTANT]
> Gegevens die zijn opgeslagen in de cache moet worden geserialiseerd, in tegenstelling tot de gegevens die kunnen worden opgeslagen in de standaard ASP.NET-sessiestatus uit het geheugen Provider status. Wanneer de sessiestatus-Provider voor Redis wordt gebruikt, moet u of de gegevenstypen die worden opgeslagen in de sessiestatus serialiseerbaar zijn.
> 
> 

## <a name="aspnet-session-state-options"></a>Opties voor ASP.NET-sessiestatus
* In het geheugen Session State-Provider - slaat deze provider de sessiestatus in het geheugen. Het voordeel van het gebruik van deze provider is dat het eenvoudig en snel is. U kunt uw Web-Apps echter kan niet schalen als u in het geheugen provider omdat deze niet wordt gedistribueerd.
* SQL Server Session State-Provider - deze provider slaat de sessiestatus in Sql Server. Gebruik deze provider als u wilt de sessiestatus wordt opgeslagen in de permanente opslag. U kunt uw Web-App schalen maar invloed op de prestaties met Sql Server voor sessie heeft op uw Web-App.
* Gedistribueerd In geheugen sessiestatus-Provider zoals Redis-Cache sessiestatus-Provider - deze provider kunt u het beste van beide werelden. Uw Web-App kan een eenvoudige, snelle en schaalbare sessiestatus-Provider hebben. Omdat deze provider is de sessiestatus in een Cache opgeslagen, moet uw app in overweging nemen van de kenmerken die zijn gekoppeld om een gedistribueerd In cachegeheugen, zoals tijdelijke netwerkfouten. Zie voor aanbevolen procedures voor het gebruik van Cache [opslaan in cache richtlijnen](../best-practices-caching.md) van Microsoft Patterns & procedures [Azure Cloud toepassingsontwerp en implementatie richtlijnen](https://github.com/mspnp/azure-guidance).

Zie voor meer informatie over de status van de sessie en andere aanbevelingen [aanbevolen procedures voor de ontwikkeling van Web (gebouw echte Cloud-Apps met Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Volgende stappen
Bekijk de [ASP.NET-Cacheprovider voor Azure Redis-Cache](cache-aspnet-output-cache-provider.md).

