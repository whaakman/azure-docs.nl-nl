---
title: Het configureren van Redis-clustering voor een Premium Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het maken en beheren van Redis-clustering voor de laag Premium Azure Redis-Cache-exemplaren
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 16281cca4e4bc95e145317365d42382ab11fde93
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-redis-cache"></a>Redis clustering voor een Premium Azure Redis-Cache configureren
Azure Redis-Cache heeft verschillende cache aanbiedingen die flexibiliteit bij de keuze van cachegrootte en -functies bieden, zoals de Premium-laag functies zoals clustering, persistentie en virtual network-ondersteuning. Dit artikel wordt beschreven hoe u configureert in een premium Azure Redis-Cache-exemplaar clustering.

Zie voor informatie over andere functies van premium-cache, [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Wat is de Redis-Cluster?
Azure Redis-Cache biedt Redis-cluster als [geïmplementeerd in Redis](http://redis.io/topics/cluster-tutorial). Met Redis-Cluster beschikt u over de volgende voordelen: 

* De mogelijkheid om automatisch gesplitst uw gegevensset tussen meerdere knooppunten. 
* De mogelijkheid om door te gaan bewerkingen wanneer een subset van de knooppunten ondervindt fouten of niet kunnen communiceren met de rest van het cluster. 
* Meer doorvoer: doorvoer lineair neemt toe naarmate u Verhoog het aantal shards. 
* Grootte van meer geheugen: Lineair neemt toe naarmate u Verhoog het aantal shards.  

Zie voor meer informatie over de grootte, doorvoer en bandbreedte van premiumcaches [welke aanbieding Redis-Cache en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

In Azure wordt Redis-cluster aangeboden als een primair/replica model waarin de elke shard heeft een paar primair/replica met replicatie waar de replicatie wordt beheerd door service van Azure Redis-Cache. 

## <a name="clustering"></a>Clustering
Clustering is ingeschakeld op de **nieuwe Redis-Cache** blade tijdens het maken van de cache. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Clustering is geconfigureerd op de **Redis-Cluster** blade.

![Clustering][redis-cache-clustering]

U kunt maximaal 10 shards in het cluster hebben. Klik op **ingeschakeld** en schuif de schuifregelaar of typ een getal tussen 1 en 10 voor **Shard aantal** en klik op **OK**.

Elke shard is een primair/replica cache paar beheerd door Azure en de totale grootte van de cache wordt berekend door het aantal shards vermenigvuldigen met de grootte van de cache in de prijscategorie is geselecteerd. 

![Clustering][redis-cache-clustering-selected]

Zodra de cache is gemaakt u verbinding maakt met en gebruik deze net zoals een niet-geclusterde cache en Redis distribueert de gegevens in de Cache shards. Als u diagnostische gegevens is [ingeschakeld](cache-how-to-monitor.md#enable-cache-diagnostics), metrische gegevens voor elke shard afzonderlijk worden vastgelegd en kunnen worden [bekeken](cache-how-to-monitor.md) in de blade Redis-Cache. 

> [!NOTE]
> 
> Er zijn een aantal kleine verschillen in de clienttoepassing vereist wanneer clustering is geconfigureerd. Zie voor meer informatie [moet ik mijn clienttoepassing clustering Breng eventuele wijzigingen?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Voorbeeld van code over het werken met met de client StackExchange.Redis clustering, Zie de [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) gedeelte van de [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Wijzig de clustergrootte van de op een actieve premium-cache
Wijzigen van de clustergrootte van de op een actieve premium-cache met clustering is ingeschakeld, klikt u op **clustergrootte Redis** van de **Resource menu**.

> [!NOTE]
> Terwijl de Azure Redis-Cache Premium-laag is vrijgegeven voor algemene beschikbaarheid, is de clustergrootte Redis-functie momenteel preview.
> 
> 

![Clustergrootte redis][redis-cache-redis-cluster-size]

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in de **Shard aantal** tekstvak en klik op **OK** om op te slaan.

> [!NOTE]
> Schalen van een cluster wordt uitgevoerd de [migreren](https://redis.io/commands/migrate) opdracht, die een dure opdracht, dus voor minimale invloed, kunt u deze bewerking tijdens daluren uitvoert. Tijdens de migratie ziet u een piek in de belasting van de server. Schalen van een cluster lang proces actief is en de hoeveelheid tijd is afhankelijk van het aantal sleutels en de grootte van de waarden die zijn gekoppeld aan deze sleutels.
> 
> 

## <a name="clustering-faq"></a>Veelgestelde vragen over clustering
De volgende lijst bevat antwoorden op veelgestelde vragen over Azure Redis-Cache clustering.

* [Moet ik mijn clienttoepassing clustering Breng eventuele wijzigingen?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Hoe worden de sleutels in een cluster gedistribueerd?](#how-are-keys-distributed-in-a-cluster)
* [Wat is de grootste cachegrootte die ik kan maken?](#what-is-the-largest-cache-size-i-can-create)
* [Alle Redis-clients ondersteunen clustering?](#do-all-redis-clients-support-clustering)
* [Hoe kan ik verbinden naar Mijn cache als clustering is ingeschakeld?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Kan ik rechtstreeks verbinding maken met de afzonderlijke shards van mijn cache?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Kan ik de clustering voor een eerder gemaakte cache configureren?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Kan ik de clustering voor een basis of standaard-cache configureren?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Kan ik gebruiken met de ASP.NET-sessiestatus Redis en uitvoercaching providers clustering?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Ik krijg verplaatsen uitzonderingen bij gebruik van StackExchange.Redis en clustering, wat moet ik doen?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Moet ik mijn clienttoepassing clustering Breng eventuele wijzigingen?
* Wanneer clustering is ingeschakeld, wordt alleen database 0 beschikbaar is. Als u de clienttoepassing gebruikmaakt van meerdere databases en probeert te lezen of schrijven naar een andere database dan 0, wordt de volgende uitzondering gegenereerd. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Zie voor meer informatie [clusterspecificatie Redis - geïmplementeerde subset](http://redis.io/topics/cluster-spec#implemented-subset).
* Als u [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), moet u 1.0.481 of hoger. U verbinding maken met de cache met behulp van dezelfde [eindpunten, poorten en sleutels](cache-configure.md#properties) die u gebruikt bij het verbinden met een cache die geen clusteren is ingeschakeld. Het enige verschil is dat alle lees- en schrijfbewerkingen met 0-database moeten worden uitgevoerd.
  
  * Andere clients mogelijk andere vereisten. Zie [alle Redis-clients ondersteunen clustering?](#do-all-redis-clients-support-clustering)
* Als uw toepassing gebruikmaakt van meerdere sleutelbewerkingen batch worden opgenomen in één opdracht, worden alle sleutels moeten zich in de shard dezelfde. Zie voor informatie over het zoeken van sleutels in de dezelfde shard [hoe worden sleutels verdeeld in een cluster?](#how-are-keys-distributed-in-a-cluster)
* Als u van ASP.NET Session State-provider Redis gebruikmaakt moet u 2.0.1 of hoger. Zie [kan ik gebruiken met de ASP.NET-sessiestatus Redis en uitvoercaching providers clustering?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Hoe worden de sleutels in een cluster gedistribueerd?
Per de Redis [sleutels distributiemodel](http://redis.io/topics/cluster-spec#keys-distribution-model) documentatie: de sleutel ruimte 16384 sleuven opgesplitst. Elke sleutel wordt gehasht en toegewezen aan een van deze sleuven, zijn verdeeld over de knooppunten van het cluster. U kunt configureren die deel van de sleutel wordt opgedeeld om ervoor te zorgen dat er meerdere sleutels bevinden zich in de dezelfde shard met hash-tags.

* Met een hash-code - sleutels als een deel van de sleutel is ingesloten in `{` en `}`, alleen deel van de sleutel wordt opgedeeld voor het vaststellen van de sleuf hash van een sleutel van. Bijvoorbeeld de volgende 3 sleutels zou bevinden in de dezelfde shard: `{key}1`, `{key}2`, en `{key}3` omdat alleen de `key` deel van de naam wordt opgedeeld. Zie voor een volledige lijst met sleutels hash-code specificaties [sleutels hash-tags](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Sleutels zonder een hash-code - de naam van de gehele sleutel wordt gebruikt voor de hash. Dit resulteert in een statistisch zelfs distributiepunt via de shards van de cache.

Voor optimale prestaties en doorvoer, wordt u aangeraden de sleutels gelijkmatig distribueren. Als u gebruikmaakt van sleutels met een hash-label is de verantwoordelijkheid van de toepassing om te controleren of de sleutels worden evenredig verdeeld.

Zie voor meer informatie [sleutels distributiemodel](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis-Cluster gegevens sharding](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), en [sleutels hash-tags](http://redis.io/topics/cluster-spec#keys-hash-tags).

Zie voor een voorbeeld van code over het werken met clustering en sleutels in de dezelfde shard vinden met de client StackExchange.Redis, de [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) gedeelte van de [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Wat is de grootste cachegrootte die ik kan maken?
De grootte van de grootste premium-cache is 53 GB. U kunt maximaal 10 shards zodat u een maximale grootte van 530 GB maken. Als u een groter formaat moet kunt u [aanvragen meer](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Zie voor meer informatie [prijzen van Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Alle Redis-clients ondersteunen clustering?
Redis clustering op dit moment die niet alle clients ondersteunen. StackExchange.Redis is een versie die ondersteuning biedt voor. Zie voor meer informatie over andere clients de [met het cluster speelt](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) sectie van de [Redis-cluster zelfstudie](http://redis.io/topics/cluster-tutorial).

> [!NOTE]
> Als u als de client StackExchange.Redis gebruikt, controleert u de nieuwste versie van [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 of hoger voor clustering correct te laten werken. Als u problemen met verplaatsen uitzonderingen hebt, raadpleegt u [uitzonderingen verplaatsen](#move-exceptions) voor meer informatie.
> 
> 

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Hoe kan ik verbinden naar Mijn cache als clustering is ingeschakeld?
U kunt verbinding maken met uw cache met behulp van dezelfde [eindpunten](cache-configure.md#properties), [poorten](cache-configure.md#properties), en [sleutels](cache-configure.md#access-keys) die u gebruikt bij het verbinden met een cache die geen clusteren is ingeschakeld. Redis beheert de clustering op de back-end, zodat u niet hoeft te beheren vanaf uw client.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Kan ik rechtstreeks verbinding maken met de afzonderlijke shards van mijn cache?
Dit wordt niet officieel ondersteund. Met die gezegd, wordt elke shard bestaat uit de cache combinatie van een primair/replica, gezamenlijk aangeduid als een cache-exemplaar. U kunt verbinding maken met de exemplaren van deze cache met behulp van het redis cli-hulpprogramma in de [onstabiel](http://redis.io/download) branche van de Redis-opslagplaats op GitHub. Deze versie implementeert basisondersteuning wanneer gestart met de `-c` overschakelen. Zie voor meer informatie [met het cluster speelt](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) op [http://redis.io](http://redis.io) in de [Redis-cluster zelfstudie](http://redis.io/topics/cluster-tutorial).

Gebruik de volgende opdrachten voor niet-ssl.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

Voor ssl, vervangt u `1300N` met `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Kan ik de clustering voor een eerder gemaakte cache configureren?
U kunt momenteel alleen inschakelen wanneer u een cache maken clustering. U kunt de clustergrootte wijzigen nadat de cache is gemaakt, maar u niet toevoegen aan een premium-cache of verwijderen uit de cache premium clustering nadat de cache is gemaakt. Een premium-cache met clusteren is ingeschakeld en slechts één shard is anders dan een premium-cache van dezelfde grootte hebben van geen clustering.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kan ik de clustering voor een basis of standaard-cache configureren?
Clustering is alleen beschikbaar voor premium-caches.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Kan ik gebruiken met de ASP.NET-sessiestatus Redis en uitvoercaching providers clustering?
* **Redis-Cache-uitvoer provider** -er zijn geen wijzigingen vereist.
* **Redis Session State-provider** - gebruiken clustering, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger, of een uitzondering gegenereerd. Dit is een belangrijke wijziging; Zie voor meer informatie [v2.0.0 Details van de wijziging op te splitsen](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Ik krijg verplaatsen uitzonderingen bij gebruik van StackExchange.Redis en clustering, wat moet ik doen?
Als u van StackExchange.Redis gebruikmaakt en ontvangt `MOVE` uitzonderingen bij gebruik van clustering, zorg ervoor dat u gebruikmaakt van [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) of hoger. Zie voor instructies over het configureren van uw .NET-toepassingen voor gebruik van StackExchange.Redis [de cacheclients configureren](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer Premiumfuncties cache.

* [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







