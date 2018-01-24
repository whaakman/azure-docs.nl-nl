---
title: Migreren van Redis - Azure Managed Cache Service toepassingen | Microsoft Docs
description: Meer informatie over het migreren van Managed Cache Service en In-Role Cache toepassingen met Azure Redis-Cache
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: wesmc
ms.openlocfilehash: 0d52454ae1c2159814d4601d07259aba319e8598
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migreren van Managed cacheservice naar Azure Redis-Cache
Migreren van uw toepassingen die gebruikmaken van Azure Managed Cache Service met Azure Redis-Cache kan worden bewerkstelligd met minimale wijzigingen in uw toepassing, afhankelijk van de functies Managed Cache Service is gebruikt door de toepassing in het cachegeheugen. Hoewel de API's niet precies hetzelfde zijn ze zijn vergelijkbaar en veel van de bestaande code die Managed Cache Service voor toegang tot een cache met minimale wijzigingen opnieuw kan worden gebruikt. In dit onderwerp wordt beschreven hoe u de vereiste configuratie en de wijzigingen in de toepassing voor het migreren van uw toepassingen Managed Cache Service en het gebruik van Azure Redis-Cache en ziet u hoe sommige van de functies van Azure Redis-Cache kan worden gebruikt om de functionaliteit van een Managed Cache Service-cache te implementeren.

>[!NOTE]
>Beheerde Cache Service en In-Role Cache waren [buiten gebruik gesteld](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30 November 2016. Als u een In-Role Cache-implementaties die u wilt migreren naar Azure Redis-Cache hebt, kunt u de stappen in dit artikel.

## <a name="migration-steps"></a>Stappen voor migratie
De volgende stappen zijn vereist voor het migreren van een toepassing Managed Cache Service Azure Redis-Cache gebruiken.

* Functies van Managed Cache Service toewijzen aan Azure Redis-Cache
* Kies een Cache-aanbieding
* Een Cache maken
* De Cacheclients configureren
  * Verwijder de configuratie van Managed Cache Service
  * Een cacheclient met het NuGet-pakket StackExchange.Redis configureren
* Managed Cache Service code migreren
  * Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer
  * Toegang tot primitieve gegevenstypen in de cache
  * Werken met .NET-objecten in de cache
* Migreren van ASP.NET-sessiestatus en uitvoercaching met Azure Redis-Cache 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Functies van Managed Cache Service toewijzen aan Azure Redis-Cache
Azure Managed Cache Service en Azure Redis-Cache zijn vergelijkbaar, maar sommige functies op verschillende manieren implementeren. Deze sectie worden enkele van de verschillen beschreven en vindt u informatie over het implementeren van de functies van Managed Cache Service in Azure Redis-Cache.

| Beheerde onderdeel van de Cache Service | Ondersteuning van beheerde Cache Service | Ondersteuning van Azure Redis-Cache |
| --- | --- | --- |
| Benoemde caches |Een standaardcache is geconfigureerd en in de cache Standard en Premium-aanbiedingen, maximaal negen aanvullende caches met de naam kunnen worden geconfigureerd als gewenst. |Azure Redis-caches hebben een configureerbare aantal databases (standaard van 16) die kunnen worden gebruikt voor het implementeren van een vergelijkbare functionaliteit voor benoemde caches. Zie [Wat zijn Redis-databases?](cache-faq.md#what-are-redis-databases) en [Standaardconfiguratie voor Redis-server](cache-configure.md#default-redis-server-configuration) voor meer informatie. |
| Hoge beschikbaarheid |Biedt hoge beschikbaarheid voor items in de cache in de cache-aanbiedingen Standard en Premium. Als items verbroken vanwege een fout zijn, zijn back-ups van de items in de cache nog steeds beschikbaar. Schrijfbewerkingen naar de secundaire cache synchroon uitgevoerd. |Hoge beschikbaarheid is beschikbaar in de Standard en Premium-cache-aanbiedingen, waarvoor een twee knooppunten (primair/Replica)-configuratie (elke shard in een Premium-cache heeft een paar primair/replica). Schrijfbewerkingen naar de replica asynchroon uitgevoerd. Zie voor meer informatie [prijzen van Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/). |
| Meldingen |Hiermee kunnen clients asynchrone meldingen wilt ontvangen wanneer er een aantal cachebewerkingen plaatsvinden op een benoemde cache. |Clienttoepassingen kunnen gebruikmaken van Redis pub subitems of [Keyspace-kennisgevingen](cache-configure.md#keyspace-notifications-advanced-settings) als u een vergelijkbare functionaliteit op meldingen. |
| Lokale cache |Lokaal een kopie van de objecten in de cache opgeslagen op de client voor extra snelle toegang. |Clienttoepassingen moet deze functionaliteit met behulp van een woordenboek of vergelijkbare gegevensstructuur implementeren. |
| Beleid verwijderen |Geen of LRU. Het standaardbeleid is LRU. |Azure Redis-Cache ondersteunt de volgende beleidsregels voor verwijdering: vluchtige lru, allkeys lru, vluchtige willekeurige, allkeys-willekeurige vluchtige-ttl, noeviction. Het standaardbeleid is vluchtige lru. Zie voor meer informatie [serverconfiguratie standaard Redis](cache-configure.md#default-redis-server-configuration). |
| Een verloopbeleid voor |Het verloopbeleid standaard Absolute is en het standaardinterval voor de vervaldatum is tien minuten. Bij Verschuivend en nooit beleidsregels zijn ook beschikbaar. |Standaard items in de cache niet verlopen, maar een vervaldatum kan worden geconfigureerd op basis van per schrijven met behulp van de cache set overloads. Zie voor meer informatie [objecten toevoegen en opgehaald uit de cache](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache). |
| Regio's en labels |Gebieden zijn subgroepen voor in de cache-items. Regio's bieden ook ondersteuning voor de aantekening van items in de cache met extra beschrijvende tekenreeksen labels genoemd. Regio's ondersteunen de mogelijkheid om te zoekbewerkingen uitvoeren op alle gelabelde items in deze regio. Alle items in een regio bevinden zich in één knooppunt van het cache-cluster. |Een Redis-cache bestaat uit één knooppunt, (tenzij de Redis-cluster is ingeschakeld) zodat het concept van Managed Cache Service regio's is niet van toepassing. Redis ondersteunt zoeken en jokerteken bewerkingen bij het ophalen van sleutels, zodat u beschrijvende labels kunnen worden ingesloten in de sleutelnamen en gebruikt voor het ophalen van de items later. Zie voor een voorbeeld van de implementatie van een tagging oplossing met behulp van Redis [uitvoering van labels met Redis cache](http://stackify.com/implementing-cache-tagging-redis/). |
| Serialisatie |Managed Cache ondersteunt NetDataContractSerializer, BinaryFormatter en het gebruik van aangepaste objectserializers. De standaardwaarde is NetDataContractSerializer. |Het is de verantwoordelijkheid van de clienttoepassing .NET om objecten te serialiseren voordat ze worden geplaatst in de cache, met de keuze van de serializer tot aan de ontwikkelaar van de client-toepassing. Zie voor meer informatie en voorbeeldcode [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Cache-emulator |Managed Cache biedt de emulator van een lokale cache. |Azure Redis-Cache heeft geen een emulator, maar u kunt [lokaal uitvoeren van de build MSOpenTech van redis-server.exe](cache-faq.md#cache-emulator) om een emulator ervaring te bieden. |

## <a name="choose-a-cache-offering"></a>Kies een Cache-aanbieding
Microsoft Azure Redis-cache is beschikbaar in de volgende lagen:

* **Basic**: één knooppunt. Meerdere groottes tot 53 GB.
* **Standard**: twee knooppunten (primair/replica). Meerdere groottes tot 53 GB. 99,9% SLA.
* **Premium**: twee knooppunten (primair/replica) met maximaal 10 shards. Verschillende groottes tussen 6 en 530 GB. Alle functies van de lagen Standard en Premium bieden ondersteuning voor [Redis-cluster](cache-how-to-premium-clustering.md), [Redis-persistentie](cache-how-to-premium-persistence.md) en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Elke laag verschilt wat functies en prijzen betreft. De functies verderop in deze handleiding en voor meer informatie over prijzen worden behandeld, Zie [prijsdetails voor caches](https://azure.microsoft.com/pricing/details/cache/).

Een startpunt voor de migratie is het kiezen van de grootte die overeenkomt met de grootte van de vorige Managed Cache Service-cache en vervolgens omhoog of omlaag schalen afhankelijk van de vereisten van uw toepassing. Zie voor meer informatie over het kiezen van het juiste Azure Redis-Cache-aanbod [welke aanbieding Redis-Cache en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Een Cache maken
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>De Cacheclients configureren
Nadat de cache is gemaakt en geconfigureerd, wordt de volgende stap is het Managed Cache Service-configuratie verwijderen en het toevoegen van de configuratie van de Azure Redis-Cache toevoegen en verwijst naar zodat cacheclients toegang de cache tot hebben.

* Verwijder de configuratie van Managed Cache Service
* Een cacheclient met het NuGet-pakket StackExchange.Redis configureren

### <a name="remove-the-managed-cache-service-configuration"></a>Verwijder de configuratie van Managed Cache Service
Voordat de clienttoepassingen kunnen worden geconfigureerd voor Azure Redis-Cache, de bestaande Managed Cache Service-configuratie en assembly-verwijzingen moeten worden verwijderd met het verwijderen van het Managed Cache Service NuGet-pakket.

Voor het verwijderen van het Managed Cache Service NuGet-pakket met de rechtermuisknop op het clientproject in **Solution Explorer** en kies **NuGet-pakketten beheren**. Selecteer de **geïnstalleerde pakketten** -knooppunt en typ W**indowsAzure.Caching** in het zoekvak geïnstalleerde pakketten. Selecteer **Windows** **Azure Cache** (of **Windows** **Azure Caching** afhankelijk van de versie van het NuGet-pakket), klikt u op **verwijderen**, en klik vervolgens op **sluiten**.

![Azure Managed Cache Service NuGet-pakket verwijderen](./media/cache-migrate-to-redis/IC757666.jpg)

Verwijdert de assembly's van Managed Cache Service en de vermeldingen Managed Cache Service het Managed Cache Service NuGet-pakket te verwijderen in het app.config- of web.config van de clienttoepassing. Omdat een aantal aangepaste instellingen kunnen niet worden verwijderd wanneer u het NuGet-pakket verwijdert, open web.config of app.config en zorg ervoor dat de volgende elementen volledig worden verwijderd.

Zorg ervoor dat de `dataCacheClients` vermelding is verwijderd uit de `configSections` element. Verwijder niet de gehele `configSections` element; alleen verwijderen de `dataCacheClients` -item, indien aanwezig.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Zorg ervoor dat de `dataCacheClients` sectie is verwijderd. De `dataCacheClients` sectie zijn vergelijkbaar met het volgende voorbeeld.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
    <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Zodra de configuratie Managed Cache Service is verwijderd, kunt u de cacheclient configureren zoals beschreven in de volgende sectie.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Een cacheclient met het NuGet-pakket StackExchange.Redis configureren
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Managed Cache Service code migreren
De API voor de cacheclient StackExchange.Redis is vergelijkbaar met de Managed Cache Service. Deze sectie biedt een overzicht van de verschillen.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer
In Managed Cache Service verbindingen met de cache zijn verwerkt door de `DataCacheFactory` en `DataCache` klassen. Deze verbindingen zijn in Azure Redis-Cache, beheerd door de `ConnectionMultiplexer` klasse.

Voeg het volgende toe met de instructie naar de bovenkant van alle bestanden die u wilt toegang krijgen tot de cache.

```csharp
using StackExchange.Redis
```

Als deze naamruimte kan niet worden omgezet, moet u het NuGet-pakket StackExchange.Redis hebt toegevoegd zoals beschreven in [de cacheclients configureren](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

> [!NOTE]
> Houd er rekening mee dat de client StackExchange.Redis is .NET Framework 4 of hoger vereist.
> 
> 

Voor verbinding met een Azure Redis-Cache-exemplaar, roept u de statische `ConnectionMultiplexer.Connect` methode en geeft u het eindpunt en -sleutel. U kunt een exemplaar van `ConnectionMultiplexer` in uw toepassing delen door een statische eigenschap in te stellen die een verbonden exemplaar retourneert, zoals in het volgende voorbeeld. Dit is een thread-veilige manier om slechts één verbonden exemplaar van `ConnectionMultiplexer` te initialiseren. In dit voorbeeld `abortConnect` is ingesteld op false, dit betekent dat de aanroep slaagt, zelfs als een verbinding met de cache is niet gemaakt. Een belangrijke functie van `ConnectionMultiplexer` is dat deze de verbinding met de cache automatisch herstelt als het netwerkprobleem of de andere oorzaken zijn opgelost.

```csharp
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
```

De cache-eindpunt, sleutels en poorten kunnen worden verkregen van de **Redis-Cache** blade voor uw cache-exemplaar. Zie voor meer informatie [Redis-Cache-eigenschappen](cache-configure.md#properties).

Wanneer de verbinding is gemaakt, een verwijzing retourneren naar de database van Redis-cache door het aanroepen van de `ConnectionMultiplexer.GetDatabase` methode. Het object dat door de `GetDatabase`-methode wordt geretourneerd, is een lichtgewicht doorvoerobject en hoeft niet te worden opgeslagen.

```csharp
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using the cache object...
// Simple put of integral data types into the cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from the cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

De client StackExchange.Redis gebruikt de `RedisKey` en `RedisValue` typen voor toegang tot en het opslaan van items in de cache. Deze typen wijzen naar de meest primitieve taaltypen, string, inclusief en vaak worden niet gebruikt rechtstreeks. Redis-tekenreeksen zijn de meest eenvoudige vorm van Redis-waarde en kunnen bevatten veel soorten gegevens, waaronder geserialiseerde binaire gegevensstromen, en u mogelijk het type niet rechtstreeks gebruiken, gaat u methoden die bevatten `String` in de naam. Voor de meest primitieve gegevenstypen u opslaan en ophalen van items uit de cache met behulp van de `StringSet` en `StringGet` methoden, tenzij u verzamelingen of andere Redis-gegevenstypen in de cache opslaat. 

`StringSet`en `StringGet` zijn vergelijkbaar met de Managed Cache Service `Put` en `Get` methoden met een grote verschil is dat de voordat u instelt en ophalen van een .NET-object in de cache u het eerst serialiseren moet. 

Bij het aanroepen van `StringGet`, als het object bestaat, wordt deze geretourneerd en als dat niet het geval is, wordt null geretourneerd. In dit geval kunt u de waarde van de gewenste gegevensbron ophalen en voor later gebruik opslaan in de cache. Dit wordt het 'cache-aside'-patroon genoemd.

Als u de vervaldatum van een item in de cache wilt opgeven, gebruikt u de parameter `TimeSpan` van `StringSet`.

```csharp
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis-Cache kunt werken met .NET-objecten, evenals de primitieve gegevenstypen, maar voordat een .NET-object kan worden opgeslagen in de cache moet worden geserialiseerd. Dit is de verantwoordelijkheid van de ontwikkelaar van de toepassing. Hiermee geeft de ontwikkelaar flexibiliteit bij de keuze van de serializer. Zie voor meer informatie en voorbeeldcode [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migreren van ASP.NET-sessiestatus en uitvoercaching met Azure Redis-Cache
Azure Redis-Cache heeft providers voor ASP.NET-sessiestatus- en pagina van de uitvoercache. Als u wilt migreren van uw toepassing die gebruikmaakt van de Managed Cache Service versies van deze providers, verwijdert u eerst de bestaande secties uit uw web.config en configureer vervolgens de Azure Redis-Cache-versies van de providers. Zie voor instructies over het gebruik van de Azure Redis-Cache ASP.NET-providers [ASP.NET Session State-Provider voor Azure Redis-Cache](cache-aspnet-session-state-provider.md) en [ASP.NET-Cacheprovider voor Azure Redis-Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Volgende stappen
Verken de [documentatie van Azure Redis-Cache](https://azure.microsoft.com/documentation/services/cache/) voor zelfstudies, voorbeelden en video's.

