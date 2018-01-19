---
title: Azure Redis-Cache-voorbeelden | Microsoft Docs
description: Informatie over het gebruik van Azure Redis-Cache
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: 0b6f89807d3252b750bd5208a7f758a06c9903d6
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-redis-cache-samples"></a>Azure Redis-Cache-voorbeelden
Dit onderwerp bevat een lijst met voorbeelden van Azure Redis-Cache, die betrekking hebben op scenario's zoals verbinding maken met een cache, lezen en schrijven van gegevens naar en uit de cache en het gebruik van de Redis-Cache van ASP.NET-providers. Sommige van de voorbeelden zijn downloadbare projecten en sommige bieden stapsgewijze instructies en codefragmenten bevatten, maar niet koppelen aan een project kan worden gedownload.

## <a name="hello-world-samples"></a>Hallo wereld-voorbeelden
De voorbeelden in deze sectie laten de basisbeginselen van verbindingen met een Azure Redis-Cache-exemplaar en lezen en schrijven van gegevens naar de cache met behulp van verschillende talen en Redis-clients.

De [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld laat zien hoe u verschillende cache uitvoert met behulp van de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-client.

In dit voorbeeld laat zien hoe:

* Gebruik verschillende verbindingsopties
* Lezen en schrijven van objecten naar en van de cache met behulp van synchrone en asynchrone bewerkingen
* MGET/MSET Redis-opdrachten gebruiken om te retourneren waarden van de opgegeven sleutels
* Redis transactionele bewerkingen uitvoeren
* Werken met Redis-lijsten en gesorteerde sets
* .NET-objecten met behulp van JsonConvert objectserializers opslaan
* Redis-sets te gebruiken voor het implementeren van tagging
* Werken met Redis-Cluster

Zie voor meer informatie de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) Zie documentatie op github, en voor meer informatie over het gebruik van scenario's de [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) eenheidstests.

[Azure Redis-Cache gebruiken met Python](cache-python-get-started.md) laat zien hoe u aan de slag met Azure Redis-Cache met behulp van Python en de [redis-py](https://github.com/andymccurdy/redis-py) client.

[Werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) ziet u een manier voor het serialiseren van .NET-objecten, zodat u kunt ze schrijven en ze te van een Azure Redis-Cache-exemplaar lezen. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Redis-Cache gebruiken als een Scale-out Backplane voor ASP.NET-SignalR
De [Redis-Cache gebruiken als een Scale-out Backplane voor ASP.NET-SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) voorbeeld laat zien hoe u Azure Redis-Cache kunt gebruiken als een SignalR backplane. Zie voor meer informatie over backplane [SignalR Scaleout met Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Voorbeeld van een Cache klant query redis
Dit voorbeeld demonstreert vergelijkt prestaties tussen toegang tot gegevens uit de cache en toegang tot gegevens uit de opslag voor persistentie. Dit voorbeeld heeft twee projecten.

* [Demo hoe Redis-Cache kunt de prestaties verbeteren door gegevens opslaan in cache](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [De Database en de Cache voor de demo seed](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-sessiestatus en uitvoercaching
De [gebruik van Azure Redis-Cache voor het opslaan van ASP.NET SessionState en OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) voorbeeld laat zien hoe u Azure Redis-Cache gebruiken voor het opslaan van ASP.NET-sessie en gebruik van de providers SessionState en OutputCache voor Redis Cache-uitvoer.

## <a name="manage-azure-redis-cache-with-maml"></a>Azure Redis-Cache met MAML beheren
De [beheren van Azure Redis-Cache met behulp van Azure Management-bibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) voorbeeld laat zien hoe u kunt Azure Management-bibliotheken beheren - (maken / bijwerken / verwijderen) uw Cache. 

## <a name="custom-monitoring-sample"></a>Aangepaste bewaking voorbeeld
De [gegevens voor het bewaken van toegang tot Redis-Cache](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) voorbeeld laat zien hoe u toegang hebt tot bewakingsgegevens voor uw Azure Redis-Cache buiten de Azure-Portal.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Een Twitter-stijl-kloon geschreven met behulp van PHP en Redis
De [Retwis](https://github.com/SyntaxC4-MSFT/retwis) voorbeeld is de Redis Hallo wereld. Het is een minimale Twitter-stijl sociale netwerken kloon geschreven met behulp van Redis- en PHP met behulp van de [Predis](https://github.com/nrk/predis) client. De broncode is ontworpen om zeer eenvoudig en op hetzelfde moment inschakelen andere Redis gegevensstructuren.

## <a name="bandwidth-monitor"></a>Bandbreedte-monitor
De [bandbreedte monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) voorbeeld kunt u voor het bewaken van de bandbreedte die wordt gebruikt op de client. Voor het meten van de bandbreedte, het voorbeeld uitvoert op de clientcomputer van de cache, aanroepen naar de cache en de bandbreedte die zijn gerapporteerd door de monitor bandbreedte voorbeeld zien.

