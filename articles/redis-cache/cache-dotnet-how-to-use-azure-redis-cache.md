---
title: Azure Redis-cache gebruiken | Microsoft Docs
description: Meer informatie over het verbeteren van de prestaties van uw Azure-toepassingen met Azure Redis-cache
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 3dfc026490093523446650c510dbebdd660e8b6b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-redis-cache"></a>Azure Redis-cache gebruiken
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Deze handleiding beschrijft hoe u aan de slag kunt gaan met **Azure Redis-cache**. Microsoft Azure Redis-cache is gebaseerd op de populaire open-source Redis-cache. Via Microsoft Azure Redis-cache hebt u toegang tot een beveiligde, toegewezen Redis-cache, beheerd door Microsoft. Een cache die u met Azure Redis-cache hebt gemaakt, is toegankelijk vanuit elke toepassing in Microsoft Azure.

Microsoft Azure Redis-cache is beschikbaar in de volgende lagen:

* **Basic**: één knooppunt. Meerdere groottes tot 53 GB.
* **Standard**: twee knooppunten (primair/replica). Meerdere groottes tot 53 GB. 99,9% SLA.
* **Premium**: twee knooppunten (primair/replica) met maximaal 10 shards. Verschillende groottes tussen 6 en 530 GB. Alle functies van de lagen Standard en Premium bieden ondersteuning voor [Redis-cluster](cache-how-to-premium-clustering.md), [Redis-persistentie](cache-how-to-premium-persistence.md) en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Elke laag verschilt wat functies en prijzen betreft. Zie [Prijsdetails voor caches][Cache Pricing Details] voor prijsinformatie.

In deze handleiding wordt beschreven hoe u de client [StackExchange.Redis][StackExchange.Redis] kunt gebruiken met C\#-code. De volgende scenario's worden behandeld: **een cache maken en configureren**, **cacheclients configureren** en **objecten toevoegen aan en verwijderen uit de cache**. Zie [Volgende stappen][Next Steps] voor meer informatie over het gebruik van Azure Redis Cache. Zie [Een web-app maken met Redis-cache](cache-web-app-howto.md) voor een stapsgewijze zelfstudie over het maken van een ASP.NET MVC-web-app met Redis-cache.

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Aan de slag met Azure Redis-cache
Het is gemakkelijk om aan de slag te gaan met Azure Redis-cache Allereerst richt u een cache in en configureert u deze. Vervolgens configureert u de cacheclients, zodat deze toegang krijgen tot de cache. Nadat de cacheclients zijn geconfigureerd, kunt u deze gaan gebruiken.

* [De cache maken][Create the cache]
* [De cacheclients configureren][Configure the cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Een cache maken
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Voor toegang tot de cache nadat deze is gemaakt
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Zie [Azure Redis-cache configureren](cache-configure.md) voor meer informatie over het configureren van uw cache. 

<a name="NuGet"></a>

## <a name="configure-the-cache-clients"></a>De cacheclients configureren
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Nadat uw clientproject is geconfigureerd voor het opslaan in de cache, kunt u de technieken die in de volgende gedeelten worden beschreven, gebruiken om met uw cache te werken.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Werken met caches
De stappen in deze sectie beschrijven hoe u met uw cache algemene taken uitvoert.

* [Verbinding maken met de cache][Connect to the cache]
* [Objecten toevoegen aan en ophalen uit de cache][Add and retrieve objects from the cache]
* [Werken met .NET-objecten in de cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-to-the-cache"></a>Verbinding maken met de cache
Als u via een programma met een cache wilt werken, hebt u een verwijzing naar de cache nodig. Voeg het volgende toe boven aan alle bestanden die u wilt gebruiken om met de client StackExchange.Redis toegang te krijgen tot een Azure Redis-cache.

    using StackExchange.Redis;

> [!NOTE]
> Voor de client StackExchange.Redis is .NET Framework 4 of hoger vereist.
> 
> 

De verbinding met de Azure Redis-cache wordt beheerd door de klasse `ConnectionMultiplexer`. Deze klasse moet worden gedeeld en opnieuw worden gebruikt in uw clienttoepassing en hoeft niet per bewerking te worden gemaakt. 

Als u verbinding wilt maken met een Azure Redis-cache en een exemplaar van een verbonden `ConnectionMultiplexer` wilt ontvangen, roept u de statische `Connect`-methode aan en geeft u het eindpunt en de sleutel van de cache door. Als wachtwoordparameter gebruikt u de sleutel die vanuit Azure Portal is gegenereerd.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Waarschuwing: sla nooit referenties op in de broncode. Ik geef ze hier weer in de broncode om dit voorbeeld eenvoudig te houden. Zie [Tekenreeksen van toepassingen en verbindingsreeksen][How Application Strings and Connection Strings Work] voor informatie over het opslaan van referenties.
> 
> 

Als u geen gebruik wilt maken van SSL, kunt u `ssl=false` instellen of de parameter `ssl` weglaten.

> [!NOTE]
> De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe caches. Zie [Toegangspoorten](cache-configure.md#access-ports) voor instructies over het inschakelen van de poort zonder SSL-beveiliging.
> 
> 

U kunt een exemplaar van `ConnectionMultiplexer` in uw toepassing delen door een statische eigenschap in te stellen die een verbonden exemplaar retourneert, zoals in het volgende voorbeeld. Deze benadering biedt een thread-veilige manier om slechts één verbonden exemplaar van `ConnectionMultiplexer` te initialiseren. In deze voorbeelden is `abortConnect` ingesteld op false. Dit betekent dat de aanroep slaagt, zelfs als er geen verbinding is gemaakt met de Azure Redis-cache. Een belangrijke functie van `ConnectionMultiplexer` is dat deze de verbinding met de cache automatisch herstelt als het netwerkprobleem of de andere oorzaken zijn opgelost.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Zie [Configuratiemodel StackExchange.Redis][StackExchange.Redis configuration model] voor meer informatie over geavanceerde opties voor de configuratie van verbindingen.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Wanneer de verbinding is gemaakt, moet u een verwijzing retourneren naar de database van Redis-cache door de `ConnectionMultiplexer.GetDatabase`-methode aan te roepen. Het object dat door de `GetDatabase`-methode wordt geretourneerd, is een lichtgewicht doorvoerobject en hoeft niet te worden opgeslagen.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Azure Redis-caches hebben een configureerbaar aantal databases (standaard 16) die kunnen worden gebruikt om de gegevens in een Redis-cache logisch te scheiden. Zie [Wat zijn Redis-databases?](cache-faq.md#what-are-redis-databases) en [Standaardconfiguratie voor Redis-server](cache-configure.md#default-redis-server-configuration) voor meer informatie.

Nu u weet hoe u verbinding maakt met een exemplaar van een Azure Redis-cache en hoe u een verwijzing naar de cachedatabase retourneert, leggen we uit hoe u de cache gebruikt.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-the-cache"></a>Objecten toevoegen aan en ophalen uit de cache
Items kunnen worden opgeslagen in en opgehaald uit de cache met behulp van de `StringSet`- en `StringGet`-methoden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis slaat de meeste gegevens op als Redis-tekenreeksen, maar deze tekenreeksen kunnen veel soorten gegevens bevatten, waaronder geserialiseerde binaire gegevens die kunnen worden gebruikt voor het opslaan van .NET-objecten in de cache.

Als u `StringGet` aanroept en het object bestaat, wordt het geretourneerd. Als dit niet het geval is, wordt `null` geretourneerd. Als `null` wordt geretourneerd, kunt u de waarde van de gewenste gegevensbron ophalen en voor later gebruik opslaan in de cache. Dit gebruikspatroon wordt het 'cache-aside'-patroon genoemd.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

U kunt ook `RedisValue` gebruiken, zoals wordt weergegeven in het volgende voorbeeld. `RedisValue` bevat impliciete operators voor het werken met integrale gegevenstypen en kan nuttig zijn als `null` een verwachte waarde is voor een item in de cache.


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


Als u de vervaldatum van een item in de cache wilt opgeven, gebruikt u de parameter `TimeSpan` van `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Werken met .NET-objecten in de cache
Azure Redis Cache kan zowel .NET-objecten als oudere gegevenstypen opslaan in de cache, maar voordat een .NET-object in de cache kan worden opgeslagen, moet het worden geserialiseerd. Deze serialisatie van het .NET-object is de verantwoordelijkheid van de ontwikkelaar van de toepassing. Het geeft de ontwikkelaar flexibiliteit bij de keuze van de serializer.

Een eenvoudige manier om objecten te serialiseren is met de serialisatiemethoden `JsonConvert` in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1). Hiermee serialiseert u van en naar JSON. In het volgende voorbeeld worden gegevens opgehaald en ingesteld met een exemplaar van het object `Employee`.

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes hebt geleerd, kunt u deze koppelingen volgen voor meer informatie over Azure Redis-cache.

* Bekijk de ASP.NET-providers voor Azure Redis-cache.
  * [State-provider voor Azure Redis-sessies](cache-aspnet-session-state-provider.md)
  * [Cacheprovider voor ASP.NET-uitvoer in Azure Redis-cache](cache-aspnet-output-cache-provider.md)
* [Schakel de diagnostische gegevens van de cache in](cache-how-to-monitor.md#enable-cache-diagnostics), zodat u de status van de cache kunt [bewaken](cache-how-to-monitor.md). U kunt de metrische gegevens weergeven in Azure Portal. U kunt ze ook [downloaden en bekijken](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) met een hulpprogramma van uw keuze.
* Bekijk de [documentatie over de cacheclient StackExchange.Redis][StackExchange.Redis cache client documentation].
  * Azure Redis-cache is toegankelijk vanuit veel Redis-clients en ontwikkelingstalen. Zie [http://redis.io/clients][http://redis.io/clients] voor meer informatie.
* Azure Redis-cache kan ook worden gebruikt met services en hulpprogramma’s van derden, zoals Redsmin en Redis Desktop Manager.
  * Zie [How to retrieve an Azure Redis connection string and use it with Redsmin][How to retrieve an Azure Redis connection string and use it with Redsmin] (Een Azure Redis-verbindingsreeks ophalen en gebruiken met Redsmin) voor meer informatie over Redsmin.
  * Benader en controleer uw gegevens in Azure Redis-cache met een grafische gebruikersinterface die gebruikmaakt van [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
* Zie de [Redis][redis]-documentatie en lees meer informatie over de [Redis-gegevenstypen][redis data types] en [een inleiding van vijftien minuten tot de Redis-gegevenstypen][a fifteen minute introduction to Redis data types].

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


