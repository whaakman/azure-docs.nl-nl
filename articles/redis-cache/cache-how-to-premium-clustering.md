---
title: Het configureren van Redis-clustering voor een Premium Azure Redis Cache | Microsoft Docs
description: Meer informatie over het maken en beheren van Redis-clustering voor de laag Premium Azure Redis Cache-exemplaren
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 06/13/2018
ms.author: wesmc
ms.openlocfilehash: b61c5860cb18f5a5b4ffa96212d66b7becad9928
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092353"
---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-redis-cache"></a>Het configureren van Redis-clustering voor een Premium Azure Redis Cache
Azure Redis-Cache heeft een ander cache-aanbiedingen, waardoor u flexibiliteit bij de keuze van de grootte van de cache en -onderdelen, met inbegrip van Premium-functies zoals clustering, persistentie en virtual network-ondersteuning. In dit artikel wordt beschreven hoe het configureren van clustering in een premium Azure Redis-Cache-exemplaar.

Zie voor meer informatie over andere functies van de cache premium [Inleiding tot de Azure Redis Cache Premium-laag](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Wat is de Redis-Cluster?
Azure Redis Cache biedt Redis-cluster als [geïmplementeerd in Redis](http://redis.io/topics/cluster-tutorial). Met Redis-Cluster krijgt u de volgende voordelen: 

* De mogelijkheid om op te splitsen automatisch uw gegevensset tussen meerdere knooppunten. 
* De mogelijkheid om te blijven werken als een subset van de knooppunten fouten ondervindt of kan niet communiceren met de rest van het cluster zijn. 
* Meer doorvoer: doorvoer neemt lineair toe naarmate u het aantal shards verhoogt. 
* Grootte van meer geheugen: Lineair neemt toe naarmate u het aantal shards verhogen.  

Clustering verhoogt niet het aantal verbindingen die beschikbaar zijn voor een geclusterde cache. Zie voor meer informatie over de grootte, doorvoer en bandbreedte met premium-caches [welke Redis-Cache-aanbieding en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

In Azure wordt Redis-cluster aangeboden als een primaire/replica-model waarbij de elke shard heeft een primaire/replica-paar met replicatie waar de replicatie wordt beheerd door Azure Redis-Cache-service. 

## <a name="clustering"></a>Clustering
Clustering is ingeschakeld op de **nieuwe Redis-Cache** blade tijdens het maken van de cache. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Clustering is geconfigureerd op de **Redis Cluster** blade.

![Clustering][redis-cache-clustering]

U kunt maximaal 10 shards in het cluster hebben. Klik op **ingeschakeld** en schuift u de schuifregelaar of typ een getal tussen 1 en 10 voor **aantal Shards** en klikt u op **OK**.

Elke shard is de combinatie van een primaire/replica-cache door Azure beheerd en de totale grootte van de cache wordt berekend door het aantal shards vermenigvuldigen met de grootte van de cache in de prijscategorie hebt geselecteerd. 

![Clustering][redis-cache-clustering-selected]

Nadat de cache is gemaakt verbindt u het en gebruik deze net als een niet-geclusterde cache en Redis de gegevens in de Cache-shards verdeeld. Als diagnostische gegevens is [ingeschakeld](cache-how-to-monitor.md#enable-cache-diagnostics), metrische gegevens voor elke shard afzonderlijk worden vastgelegd en kan [weergegeven](cache-how-to-monitor.md) in de Redis-Cache-blade. 

> [!NOTE]
> 
> Er zijn enkele kleine verschillen in uw clienttoepassing vereist wanneer clustering is geconfigureerd. Zie voor meer informatie, [heb ik wijzigingen aanbrengen in mijn clienttoepassing voor het gebruik van clustering nodig?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Voorbeeld van code over het werken met clustering met de StackExchange.Redis-client, Zie de [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) gedeelte van de [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Wijzigen van de clustergrootte van de op een premium-cache
Als u de clustergrootte op een actief premium-cache met clustering is ingeschakeld, klikt u op **clustergrootte Redis** uit de **menu Resource**.

> [!NOTE]
> De Azure Redis Cache Premium-laag is nu vrijgegeven voor algemene beschikbaarheid, is de grootte van de Redis-Cluster-functie momenteel in preview.
> 
> 

![Redis-clusters][redis-cache-redis-cluster-size]

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in de **aantal Shards** tekstvak en klik op **OK** om op te slaan.

Het cluster vergroten, verhoogt maximale doorvoer en de grootte van de cache. De grootte van het cluster vergroot, niet de maximale waarde. verbindingen beschikbaar is voor clients.

> [!NOTE]
> Schalen van een cluster wordt uitgevoerd de [migreren](https://redis.io/commands/migrate) opdracht, dit is een opdracht, duur, dus voor minimale impact, houd rekening met bewerking tijdens de niet-piekuren wordt uitgevoerd. Tijdens het migratieproces ziet u een piek in de belasting van de server. Schalen van een cluster wordt uitgevoerd een lang proces en de hoeveelheid tijd die nodig is, is afhankelijk van het aantal sleutels en de grootte van de waarden die zijn gekoppeld aan deze sleutels.
> 
> 

## <a name="clustering-faq"></a>Veelgestelde vragen over clustering
De volgende lijst bevat antwoorden op veelgestelde vragen over Azure Redis-Cache-clustering.

* [Moet ik breng wijzigingen aan in mijn clienttoepassing voor het gebruik van clustering?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Hoe worden de sleutels in een cluster gedistribueerd?](#how-are-keys-distributed-in-a-cluster)
* [Wat is de grootste cachegrootte die ik kan maken?](#what-is-the-largest-cache-size-i-can-create)
* [Alle Redis-clients ondersteunen clustering?](#do-all-redis-clients-support-clustering)
* [Hoe maak ik verbinding met mijn cache als clustering is ingeschakeld?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Kan ik rechtstreeks verbinden met de afzonderlijke shards van mijn cache?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Kan ik de clustering voor een eerder gemaakte cache configureren?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Kan ik de clustering voor een basis of standaard-cache configureren?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Kan ik de clustering met Redis ASP.NET-sessiestatus en uitvoercaching-providers gebruiken?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Ik krijg verplaatsen uitzonderingen bij het gebruik van StackExchange.Redis en clustering, wat moet ik doen?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Moet ik breng wijzigingen aan in mijn clienttoepassing voor het gebruik van clustering?
* Als clustering is ingeschakeld, wordt alleen database 0 beschikbaar is. Als u de clienttoepassing maakt gebruik van meerdere databases en er wordt geprobeerd om te lezen of schrijven naar een andere database dan 0, wordt de volgende uitzondering opgetreden. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Zie voor meer informatie, [Redis clusterspecificatie - geïmplementeerde subset](http://redis.io/topics/cluster-spec#implemented-subset).
* Als u [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), moet u 1.0.481 of hoger. U verbinding maken met de cache met behulp van dezelfde [eindpunten, poorten en sleutels](cache-configure.md#properties) die u gebruikt bij het verbinden met een cache die geen clustering is ingeschakeld. Het enige verschil is dat alle lees- en schrijfbewerkingen met 0-database moeten worden uitgevoerd.
  
  * Andere clients hebben mogelijk verschillende vereisten. Zie [alle Redis-clients ondersteunen clustering?](#do-all-redis-clients-support-clustering)
* Als uw toepassing gebruikmaakt van meerdere sleutelbewerkingen batch worden opgenomen in één opdracht, moeten u alle sleutels zich in dezelfde shard. Zie voor informatie over het zoeken van sleutels in dezelfde shard [manier waarop worden sleutels gedistribueerd in een cluster?](#how-are-keys-distributed-in-a-cluster)
* Als u van Redis ASP.NET-sessiestatus-provider gebruikmaakt moet u 2.0.1 of hoger. Zie [kan ik gebruiken met de providers Redis ASP.NET-sessiestatus en uitvoercaching clustering?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Hoe worden de sleutels in een cluster gedistribueerd?
Per de Redis [sleutels distributiemodel](http://redis.io/topics/cluster-spec#keys-distribution-model) documentatie: de sleutel ruimte is opgesplitst in 16384 sleuven. Elke sleutel wordt gehasht en toegewezen aan een van deze sleuven, die worden verdeeld over de knooppunten van het cluster. U kunt configureren die deel van de sleutel wordt gehasht om ervoor te zorgen dat er meerdere sleutels bevinden zich in dezelfde shard met hash-tags.

* Met de tag van een hash - sleutels als een deel van de sleutel is ingesloten in `{` en `}`, alleen dat deel van de sleutel voor de toepassing voor het bepalen van de hash-sleuf van een sleutel wordt gehasht. Bijvoorbeeld, zou de volgende 3 sleutels zich bevinden in dezelfde shard: `{key}1`, `{key}2`, en `{key}3` omdat alleen de `key` deel van de naam wordt gehasht. Zie voor een volledige lijst van de sleutels hash-code specificaties [hash-sleutels tags](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Sleutels zonder een hash-code - de naam van de gehele sleutel wordt gebruikt voor hashing. Dit resulteert in een statistisch verdelen over de shards van de cache.

Voor optimale prestaties en doorvoer, wordt u aangeraden de sleutels gelijkmatig verdelen. Als u gebruikmaakt van sleutels met een hash-code is de verantwoordelijkheid van de toepassing om te controleren of de sleutels worden gelijkmatig verdeeld.

Zie voor meer informatie, [sleutels distributiemodel](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis Cluster gegevenssharding](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), en [tags-hash-sleutels](http://redis.io/topics/cluster-spec#keys-hash-tags).

Voorbeeld van code voor het werken met clustering en sleutels in dezelfde shard zoeken met de StackExchange.Redis-client, Zie de [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) gedeelte van de [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Wat is de grootste cachegrootte die ik kan maken?
De grootte van de grootste premium-cache is 53 GB. U kunt maximaal 10 shards, waardoor u met een maximale grootte van 530 GB maken. Als u een groter formaat moet kunt u [meer aanvragen](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Zie voor meer informatie, [prijzen voor Azure Redis Cache](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Alle Redis-clients ondersteunen clustering?
Redis-clustering op dit moment die niet alle clients ondersteunen. StackExchange.Redis is een die ondersteuning biedt voor het. Zie voor meer informatie over andere clients de [spelen met het cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) sectie van de [zelfstudie Redis-cluster](http://redis.io/topics/cluster-tutorial). 

Het protocol van Redis-clustering is vereist voor elke client te verbinden met elke shard clustering-modus. Poging tot het gebruiken van een client die Failoverclustering leidt waarschijnlijk tot biedt geen ondersteuning in veel [VERPLAATST omleiding uitzonderingen](https://redis.io/topics/cluster-spec#moved-redirection).

> [!NOTE]
> Als u StackExchange.Redis als de client, controleert u of u met behulp van de meest recente versie van [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 of hoger voor clustering correct te laten werken. Als u problemen met verplaatsen uitzonderingen hebt, raadpleegt u [verplaatsen uitzonderingen](#move-exceptions) voor meer informatie.
> 
> 

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Hoe maak ik verbinding met mijn cache als clustering is ingeschakeld?
U kunt verbinding maken met uw cache met behulp van dezelfde [eindpunten](cache-configure.md#properties), [poorten](cache-configure.md#properties), en [sleutels](cache-configure.md#access-keys) die u gebruikt bij het verbinden met een cache die geen clustering is ingeschakeld. Redis beheert de clustering op de back-end, zodat u niet hoeft te beheren vanaf uw client.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Kan ik rechtstreeks verbinden met de afzonderlijke shards van mijn cache?
De clustering protocol is vereist dat de client de juiste shard-verbindingen maken. Zodat moet de client dit doen correct voor u. Basis hiervan dient bestaat elke shard uit de cache combinatie van een primaire/replica, gezamenlijk bekend als een cache-instantie. U kunt verbinding maken met deze cache-exemplaren met behulp van de redis-cli-hulpprogramma in de [instabiel](http://redis.io/download) vertakking van de Redis-opslagplaats op GitHub. Deze versie wordt geïmplementeerd voor basic-ondersteuning wanneer aan de slag met de `-c` overschakelen. Zie voor meer informatie [spelen met het cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) op [ http://redis.io ](http://redis.io) in de [zelfstudie Redis-cluster](http://redis.io/topics/cluster-tutorial).

Voor niet-ssl, moet u de volgende opdrachten gebruiken.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

Voor ssl, Vervang `1300N` met `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Kan ik de clustering voor een eerder gemaakte cache configureren?
U kunt op dit moment alleen inschakelen bij het maken van een cache clustering. U kunt de clustergrootte wijzigen nadat de cache is gemaakt, maar u niet toevoegen aan een premium-cache clustering of verwijderen van een premium-cache clustering nadat de cache is gemaakt. Een premium-cache met clustering is ingeschakeld en slechts één shard is anders dan een premium-cache van dezelfde grootte met geen clustering.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kan ik de clustering voor een basis of standaard-cache configureren?
Clustering is alleen beschikbaar voor premium-caches.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Kan ik de clustering met Redis ASP.NET-sessiestatus en uitvoercaching-providers gebruiken?
* **Cacheprovider voor uitvoer redis** -geen wijzigingen vereist.
* **Redis-sessiestatusprovider** : als u wilt gebruiken clustering, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger of een uitzondering is opgetreden. Dit is een belangrijke wijziging; Zie voor meer informatie [v2.0.0 Details van de belangrijke wijziging](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Ik krijg verplaatsen uitzonderingen bij het gebruik van StackExchange.Redis en clustering, wat moet ik doen?
Als u van StackExchange.Redis gebruikmaakt en ontvangt `MOVE` uitzonderingen bij het gebruik van clustering, zorg ervoor dat u gebruikmaakt van [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) of hoger. Zie voor instructies over het configureren van uw .NET-toepassingen voor gebruik van StackExchange.Redis [de cacheclients configureren](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer functies voor premium-cache.

* [Inleiding tot de Azure Redis Cache Premium-laag](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







