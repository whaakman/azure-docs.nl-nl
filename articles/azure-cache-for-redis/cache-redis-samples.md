---
title: Azure voor voorbeelden van Redis-Cache | Microsoft Docs
description: Azure Cache voor Redis gebruiken
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: 73c771ab18d1cc2944298818c1cab90eb2f277ff
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238399"
---
# <a name="azure-cache-for-redis-samples"></a>Voorbeelden voor Azure Cache voor Redis
In dit onderwerp bevat een lijst van Azure Cache voor Redis-voorbeelden, scenario's zoals verbinding maken met een cache, lezen en schrijven van gegevens naar en uit de cache en het gebruik van de Azure-Cache van ASP.NET voor Redis-providers die betrekking hebben. Sommige van de voorbeelden zijn downloadbare projecten en sommige stapsgewijze begeleiding gegeven en codefragmenten bevatten, maar niet koppelen aan een project kan worden gedownload.

## <a name="hello-world-samples"></a>Hello world-voorbeelden
De voorbeelden in deze sectie laten de basisbeginselen van het verbinden met een Azure-Cache voor Redis-exemplaar en lezen en schrijven van gegevens naar de cache met behulp van een groot aantal talen en Redis-clients.

De [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld laat zien hoe u uitvoeren van verschillende cachebewerkingen met behulp van de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-client.

Dit voorbeeld laat zien hoe u:

* Gebruik verschillende verbindingsopties
* Lezen en schrijven van objecten naar en van de cache met behulp van synchrone en asynchrone bewerkingen
* Redis MGET-/ MSET-opdrachten gebruiken om te retourneren waarden van de opgegeven sleutels
* Redis transactionele bewerkingen uitvoeren
* Werken met een lijst met Redis en gesorteerde sets
* .NET-objecten met behulp van JsonConvert objectserializers Store
* Gebruik Redis ingesteld voor het implementeren van tags
* Werken met Redis-Cluster

Zie voor meer informatie de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) documentatie op GitHub en meer gebruiksscenario's Zie de [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/tests) eenheidstests.

[Over het gebruik van Azure Cache voor Redis met Python](cache-python-get-started.md) laat zien hoe u aan de slag met Azure Cache voor Redis met behulp van Python en de [redis-py](https://github.com/andymccurdy/redis-py) client.

[Werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) toont één manier om te serialiseren van .NET-objecten, zodat u kunt ze om te schrijven en van een Azure-Cache voor Redis-exemplaar lezen. 

## <a name="use-azure-cache-for-redis-as-a-scale-out-backplane-for-aspnet-signalr"></a>Azure-Cache voor Redis gebruiken als een Scale-out Backplane voor ASP.NET SignalR
De [Azure-Cache gebruiken voor Redis als een Scale-out Backplane voor ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) voorbeeld laat zien hoe u Azure Cache kunt gebruiken voor Redis als een SignalR-backplane. Zie voor meer informatie over backplane [SignalR Scaleout met Redis](https://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="azure-cache-for-redis-customer-query-sample"></a>Azure voor voorbeeld van een klant query Redis-Cache
In dit voorbeeld ziet u vergelijkt prestaties tussen toegang tot gegevens uit de cache en toegang tot gegevens uit de permanente opslag. Dit voorbeeld heeft twee projecten.

* [Demo hoe Azure-Cache voor Redis prestaties kunt verbeteren door gegevens opslaan in cache](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Voor de Database en de Cache voor de demo](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-sessiestatus en uitvoercaching
De [Azure Cache gebruik van Redis voor het opslaan van ASP.NET SessionState en OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) voorbeeld laat zien hoe u Azure Redis Cache gebruiken voor het opslaan van ASP.NET-sessie en gebruik van de providers SessionState en OutputCache voor Redis Cache-uitvoer .

## <a name="manage-azure-cache-for-redis-with-maml"></a>Azure Cache voor Redis met MAML beheren
De [Manage Azure Cache voor Redis met behulp van Azure-beheerbibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) voorbeeld laat zien hoe kunt u Azure-beheerbibliotheken voor het beheren van - (maken / bijwerken / verwijderen) uw Cache. 

## <a name="custom-monitoring-sample"></a>Aangepaste bewaking voorbeeld
De [toegang tot Azure Cache voor Redis-bewaking gegevens](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) voorbeeld laat zien hoe u kunt toegang tot bewakingsgegevens voor uw Azure-Cache voor Redis buiten de Azure-Portal.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Een Twitter-stijl-kloon geschreven met behulp van PHP en Redis
De [Retwis](https://github.com/SyntaxC4-MSFT/retwis) voorbeeld is de Redis-Hallo wereld. Het is een minimale Twitter-stijl sociaal netwerk-kloon geschreven met behulp van Redis en PHP met behulp van de [Predis](https://github.com/nrk/predis) client. De broncode is ontworpen om zeer eenvoudig en op hetzelfde moment om weer te geven verschillende Redis gegevensstructuren.

## <a name="bandwidth-monitor"></a>Bandbreedte-monitor
De [bandbreedte monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) voorbeeld kunt u voor het bewaken van de bandbreedte die wordt gebruikt op de client. Als u wilt meten van de bandbreedte, het voorbeeld uitvoeren in de cache-clientcomputer, aanroepen naar de cache en bekijk de bandbreedte die is gerapporteerd door het voorbeeld van de monitor bandbreedte.

