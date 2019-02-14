---
title: Migreren van Managed Cache Service-toepassingen met Redis - Azure | Microsoft Docs
description: Meer informatie over het migreren van Managed Cache Service en In-Role Cache-toepassingen met Azure-Cache voor Redis
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/30/2017
ms.author: yegu
ms.openlocfilehash: 116e54fd39af801cf8941a974da2b72c483097dc
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237018"
---
# <a name="migrate-from-managed-cache-service-to-azure-cache-for-redis"></a>Migreren vanuit Managed cacheservice naar Azure Cache voor Redis
Migreren van uw toepassingen die gebruikmaken van Azure Managed Cache Service met Azure-Cache voor Redis kan worden bewerkstelligd met minimale wijzigingen in uw toepassing, afhankelijk van de Managed Cache Service-functies die worden gebruikt door uw toepassing in het cachegeheugen. Terwijl de API's niet precies hetzelfde zijn ze zijn vergelijkbaar en veel van uw bestaande code die gebruikmaakt van Managed Cache Service voor toegang tot een cache met minimale wijzigingen opnieuw kan worden gebruikt. Dit artikel wordt beschreven hoe u de vereiste configuratie en toepassing verandert als u wilt migreren van uw Managed Cache Service-toepassingen met Azure Cache voor Redis en laat zien hoe sommige van de functies van Azure voor Redis-Cache kan worden gebruikt voor het implementeren van de functionaliteit van een Managed Cache Service-cache.

>[!NOTE]
>Managed Cache Service en In-Role Cache zijn [buiten gebruik gesteld](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30 November 2016. Als u een In-Role Cache-implementaties die u wilt migreren naar Azure Cache voor Redis hebt, kunt u de stappen in dit artikel.

## <a name="migration-steps"></a>Migratiestappen
De volgende stappen zijn vereist voor het migreren van een Managed Cache Service-toepassing met Azure Cache voor Redis.

* Managed Cache Service-functies worden toegewezen aan Azure-Cache voor Redis
* Kies een Cache-aanbieding
* Een Cache maken
* De Cacheclients configureren
  * Verwijderen van de configuratie van Managed Cache Service
  * Een cacheclient met behulp van het NuGet-pakket StackExchange.Redis configureren
* Managed Cache Service code migreren
  * Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer
  * Toegang tot primitieve gegevenstypen in de cache
  * Werken met .NET-objecten in de cache
* ASP.NET-sessiestatus migreren en de uitvoer naar Azure Cache voor Redis cache 

## <a name="map-managed-cache-service-features-to-azure-cache-for-redis"></a>Managed Cache Service-functies worden toegewezen aan Azure-Cache voor Redis
Azure Managed Cache Service en Azure voor Redis-Cache zijn vergelijkbaar, maar sommige van de functies op verschillende manieren implementeren. In deze sectie beschrijft een aantal van de verschillen en richtlijnen over het implementeren van de functies van Managed Cache Service in Azure Cache voor Redis.

| Beheerde Cache Service-functie | Ondersteuning voor beheerde Cache Service | Azure Cache voor Redis-ondersteuning |
| --- | --- | --- |
| Benoemde caches |Een standaardcache is geconfigureerd en in de cache standaard en Premium-aanbiedingen, maximaal negen extra caches met de naam kunnen worden geconfigureerd als gewenst. |Azure Redis-Cache heeft een configureerbaar aantal databases (standaard 16) die kunnen worden gebruikt voor het implementeren van een vergelijkbare functionaliteit als benoemde caches. Zie [Wat zijn Redis-databases?](cache-faq.md#what-are-redis-databases) en [Standaardconfiguratie voor Redis-server](cache-configure.md#default-redis-server-configuration) voor meer informatie. |
| Hoge beschikbaarheid |Biedt hoge beschikbaarheid voor items in de cache in de Standard en Premium-cache-aanbiedingen. Als artikelen verbroken vanwege een fout zijn, wordt back-ups van de items in de cache nog steeds beschikbaar zijn. Schrijfbewerkingen naar de secundaire cache worden synchroon gemaakt. |Hoge beschikbaarheid is beschikbaar in de Standard en Premium-cache-aanbiedingen, waarvoor een configuratie primair/Replica met twee knooppunten (elke shard in een Premium-cache heeft de combinatie van een primaire/replica). Schrijfbewerkingen naar de replica worden asynchroon gedaan. Zie voor meer informatie, [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/pricing/details/cache/). |
| Meldingen |Hiermee kunnen clients asynchrone meldingen ontvangen wanneer diverse cachebewerkingen worden uitgevoerd in een benoemde cache. |Clienttoepassingen kunnen gebruikmaken van Redis pub/sub of [Keyspace-meldingen](cache-configure.md#keyspace-notifications-advanced-settings) op een vergelijkbare functionaliteit om meldingen te bereiken. |
| Lokale cache |Lokaal een kopie van de objecten in de cache opgeslagen op de client voor extra snelle toegang. |Clienttoepassingen moet voor het implementeren van deze functionaliteit met behulp van een woordenlijst of een vergelijkbare gegevensstructuur. |
| Verwijderingsbeleid |Geen of LRU. Het standaardbeleid is LRU. |Azure Redis-Cache biedt ondersteuning voor de volgende beleidsregels voor taakverwijdering: vluchtig lru, allkeys lru, vluchtig willekeurige, allkeys-willekeurige vluchtig-ttl, noeviction. Het standaardbeleid is vluchtige lru. Zie voor meer informatie, [configuratie van de standaardconfiguratie voor Redis-server](cache-configure.md#default-redis-server-configuration). |
| Vervaldatum van het beleid |Het standaardbeleid voor de vervaldatum is Absolute en het standaardinterval voor de vervaldatum is 10 minuten. Schuiven en nooit beleidsregels zijn ook beschikbaar. |Standaard items in de cache niet verlopen, maar een vervaldatum kan worden geconfigureerd op basis van per schrijven met behulp van cache set overloads. |
| Regio's en labels |Regio's zijn subgroepen voor items in de cache. Regio's ondersteunen ook het commentaar van items in de cache met extra beschrijvende tekenreeksen met de naam tags. Regio's ondersteunen de mogelijkheid om te zoekbewerkingen uitvoeren op de gemarkeerde items in deze regio. Alle items in een regio bevinden zich in een enkel knooppunt in de cachecluster. |een Azure-Cache voor Redis bestaat uit één knooppunt, (tenzij de Redis-cluster is ingeschakeld), zodat het concept van regio's van Managed Cache Service is niet van toepassing. Redis ondersteunt zoeken en jokertekenbewerkingen bij het ophalen van sleutels, zodat u beschrijvende labels kunnen worden ingesloten in de sleutelnamen en gebruikt voor het ophalen van de items later opnieuw. Zie voor een voorbeeld van de implementatie van een taggen oplossing met behulp van Redis [labelen met Redis cache implementeren](https://stackify.com/implementing-cache-tagging-redis/). |
| Serialisatie |Managed Cache biedt ondersteuning voor NetDataContractSerializer BinaryFormatter en het gebruik van aangepaste objectserializers. De standaardwaarde is NetDataContractSerializer. |Het is de verantwoordelijkheid van de clienttoepassing .NET-objecten serialiseren voordat ze worden geplaatst in de cache, met de keuze van de serializer tot aan de ontwikkelaar van de client-toepassing. Zie voor meer informatie en voorbeeldcode [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Cache-emulator |Managed Cache biedt een lokale cache-emulator. |Azure Redis-Cache heeft geen een emulator, maar u kunt [lokaal uitvoeren van de build MSOpenTech van redis-server.exe](cache-faq.md#cache-emulator) om een emulator-ervaring te bieden. |

## <a name="choose-a-cache-offering"></a>Kies een Cache-aanbieding
Microsoft Azure Cache voor Redis is beschikbaar in de volgende lagen:

* **Basic**: één knooppunt. Meerdere groottes tot 53 GB.
* **Standard**: twee knooppunten (primair/replica). Meerdere groottes tot 53 GB. 99,9% SLA.
* **Premium**: twee knooppunten (primair/replica) met maximaal 10 shards. Verschillende groottes tussen 6 en 530 GB. Alle functies van de lagen Standard en Premium bieden ondersteuning voor [Redis-cluster](cache-how-to-premium-clustering.md), [Redis-persistentie](cache-how-to-premium-persistence.md) en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Elke laag verschilt wat functies en prijzen betreft. Verderop in deze handleiding, en voor meer informatie over prijzen voor de functies worden behandeld, Zie [prijsdetails voor caches](https://azure.microsoft.com/pricing/details/cache/).

Een startpunt voor de migratie is het kiezen van de grootte die overeenkomt met de grootte van uw vorige Managed Cache Service-cache, en vervolgens omhoog of omlaag schalen afhankelijk van de vereisten van uw toepassing. Zie voor meer informatie over het kiezen van de juiste Azure Cache voor Redis-aanbieding [welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Een Cache maken
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>De Cacheclients configureren
Nadat de cache is gemaakt en geconfigureerd, wordt de volgende stap is het verwijderen van de configuratie van Managed Cache Service en de Azure-Cache voor Redis-configuratie toevoegen en verwijst naar zodat cache-clients toegang hebben tot de cache.

* Verwijderen van de configuratie van Managed Cache Service
* Een cacheclient met behulp van het NuGet-pakket StackExchange.Redis configureren

### <a name="remove-the-managed-cache-service-configuration"></a>Verwijderen van de configuratie van Managed Cache Service
Voordat de clienttoepassingen kunnen worden geconfigureerd voor Cache voor Azure Redis, de bestaande Managed Cache Service configuratie en de assembly-verwijzingen worden verwijderd door het verwijderen van de Managed Cache Service NuGet-pakket.

Voor het verwijderen van de Managed Cache Service NuGet-pakket met de rechtermuisknop op de client-project in **Solution Explorer** en kies **NuGet-pakketten beheren**. Selecteer de **installatiepakketten** -knooppunt en typ W**indowsAzure.Caching** in het zoekvak van de geïnstalleerde pakketten. Selecteer **Windows** **Azure Cache** (of **Windows** **Azure Caching** afhankelijk van de versie van het NuGet-pakket), klikt u op **Verwijderen**, en klik vervolgens op **sluiten**.

![Azure Managed Cache Service NuGet-pakket verwijderen](./media/cache-migrate-to-redis/IC757666.jpg)

De Managed Cache Service NuGet-pakket te verwijderen, worden de assembly's van Managed Cache Service en de Managed Cache Service vermeldingen in het app.config- of web.config van de clienttoepassing verwijderd. Omdat sommige aangepaste instellingen kunnen niet worden verwijderd tijdens het verwijderen van het NuGet-pakket, open web.config of app.config en zorg ervoor dat de volgende elementen zijn verwijderd.

Zorg ervoor dat de `dataCacheClients` vermelding wordt verwijderd uit de `configSections` element. Verwijder niet de gehele `configSections` element; alleen verwijderen de `dataCacheClients` vermelding, indien aanwezig.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Zorg ervoor dat de `dataCacheClients` sectie is verwijderd. De `dataCacheClients` sectie is vergelijkbaar met het volgende voorbeeld.

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

Wanneer de configuratie van Managed Cache Service is verwijderd, kunt u de cacheclient configureren zoals beschreven in de volgende sectie.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Een cacheclient met behulp van het NuGet-pakket StackExchange.Redis configureren
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Managed Cache Service code migreren
De API voor de StackExchange.Azure Cache voor Redis-client is vergelijkbaar met de Managed Cache Service. In deze sectie biedt een overzicht van de verschillen.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer
In de Managed Cache Service, de verbindingen met de cache zijn verwerkt door de `DataCacheFactory` en `DataCache` klassen. In Azure-Cache voor Redis, deze verbindingen worden beheerd door de `ConnectionMultiplexer` klasse.

Voeg de volgende gebruiksinstructie aan het begin van alle bestanden die u wilt krijgen tot de cache.

```csharp
using StackExchange.Redis
```

Als deze naamruimte niet kan worden opgelost, moet u dat u het StackExchange.Redis-pakket hebt toegevoegd zoals beschreven in [Quick Start: Azure Cache voor Redis gebruiken met een .NET-toepassing](cache-dotnet-how-to-use-azure-redis-cache.md).

> [!NOTE]
> Houd er rekening mee dat de client StackExchange.Redis is .NET Framework 4 of hoger vereist.
> 
> 

Als u wilt verbinding maken met een Azure-Cache voor Redis-exemplaar, roept u de statische `ConnectionMultiplexer.Connect` methode en geeft u het eindpunt en de sleutel. U kunt een exemplaar van `ConnectionMultiplexer` in uw toepassing delen door een statische eigenschap in te stellen die een verbonden exemplaar retourneert, zoals in het volgende voorbeeld. Deze benadering biedt een thread-veilige manier om te initialiseren van één verbonden `ConnectionMultiplexer` exemplaar. In dit voorbeeld `abortConnect` is ingesteld op false, dit betekent dat de aanroep slaagt, zelfs als een verbinding met de cache niet tot stand wordt gebracht. Een belangrijke functie van `ConnectionMultiplexer` is dat deze de verbinding met de cache automatisch herstelt als het netwerkprobleem of de andere oorzaken zijn opgelost.

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

De cache-eindpunt, sleutels en poorten kunnen worden opgehaald van de **Azure Cache voor Redis** blade voor uw cache-exemplaar. Zie voor meer informatie, [Azure Cache voor Redis-eigenschappen](cache-configure.md#properties).

Zodra de verbinding tot stand is gebracht, retourneert een verwijzing naar de Azure-Cache voor Redis-database door het aanroepen van de `ConnectionMultiplexer.GetDatabase` methode. Het object dat door de `GetDatabase`-methode wordt geretourneerd, is een lichtgewicht doorvoerobject en hoeft niet te worden opgeslagen.

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

De StackExchange.Redis-client gebruikt de `RedisKey` en `RedisValue` typen voor het openen en opslaan van items in de cache. Deze typen toewijzen op meest primitieve taaltypen, met inbegrip van tekenreeks, en vaak worden niet gebruikt rechtstreeks. Redis-tekenreeksen zijn het meest eenvoudige type van de Redis-waarde en kunnen bevatten veel soorten gegevens, waaronder geserialiseerde binaire streams, en u mogelijk het type niet rechtstreeks gebruiken, gaat u methoden die bevatten `String` in de naam. Voor meest primitieve gegevenstypen, u gegevens kunt opslaan en ophalen van items uit de cache met behulp van de `StringSet` en `StringGet` methoden, tenzij u verzamelingen of andere Redis-gegevenstypen in de cache opslaat. 

`StringSet` en `StringGet` zijn vergelijkbaar met de Managed Cache Service `Put` en `Get` methoden, met een groot verschil is dat de voordat u een .NET-object in de cache ophalen en u deze eerst serialiseren moet. 

Bij het aanroepen van `StringGet`, als het object bestaat, deze wordt geretourneerd, en als dat niet het geval is, wordt null geretourneerd. In dit geval kunt u de waarde van de gewenste gegevensbron ophalen en opslaan in de cache voor later gebruik. Dit patroon wordt de cache-aside-patroon genoemd.

Als u de vervaldatum van een item in de cache wilt opgeven, gebruikt u de parameter `TimeSpan` van `StringSet`.

```csharp
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis-Cache kunt werken met .NET-objecten als oudere gegevenstypen opslaan, maar voordat een .NET-object in de cache kan worden opgeslagen, moet worden geserialiseerd. Deze serialisatie is de verantwoordelijkheid van de ontwikkelaar van de toepassing en geeft de ontwikkelaar flexibiliteit bij de keuze van de serializer. Zie voor meer informatie en voorbeeldcode [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-cache-for-redis"></a>ASP.NET-sessiestatus migreren en de uitvoer naar Azure Cache voor Redis cache
Azure Redis-Cache heeft providers voor ASP.NET-sessiestatus en pagina-uitvoer opslaan in cache. Als u wilt migreren van uw toepassing die gebruikmaakt van de Managed Cache Service-versies van deze providers, verwijdert u eerst de bestaande secties uit uw web.config en configureer vervolgens de Azure-Cache voor Redis-versies van de providers. Zie voor instructies over het gebruik van de Azure-Cache voor Redis-ASP.NET-providers [ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis](cache-aspnet-session-state-provider.md) en [Cacheprovider voor ASP.NET voor Azure Cache voor Redis](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Volgende stappen
Verken de [Azure Cache voor Redis-documentatie](https://azure.microsoft.com/documentation/services/cache/) voor zelfstudies, voorbeelden en video's.

