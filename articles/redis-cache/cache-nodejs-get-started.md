---
title: Azure Redis-cache gebruiken met behulp van Node.js | Microsoft Docs
description: Aan de slag met Azure Redis-cache met behulp van Node.js en node_redis.
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: wesmc
ms.openlocfilehash: e8b4818ad2b876ce4d55cf8aa2aa0e0162dea963
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Azure Redis-cache gebruiken met behulp van Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis-cache geeft u toegang tot een beveiligde, toegewezen Redis-cache, beheerd door Microsoft. Uw cache is toegankelijk vanuit elke toepassing in Microsoft Azure.

In dit onderwerp wordt beschreven hoe u aan de slag kunt met Azure Redis-cache met gebruik van Node.js. 

## <a name="prerequisites"></a>Vereisten
[node_redis](https://github.com/mranney/node_redis) installeren:

    npm install redis

In deze zelfstudie wordt gebruikgemaakt van [node_redis](https://github.com/mranney/node_redis). Voor voorbeelden van het gebruik van andere Node.js-clients, verwijzen wij u naar de afzonderlijke documentatie voor de Node.js-clients die staat vermeld op [Node.js Redis-clients](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Een Redis-cache maken op Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De hostnaam en toegangssleutels ophalen
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Veilig verbinding maken met de cache via SSL
De meest recente versies van [node_redis](https://github.com/mranney/node_redis) bieden ondersteuning voor het via SSL maken van verbinding met Azure Redis-cache. Het volgende voorbeeld laat zien hoe u via SSL-eindpunt 6380 verbinding maakt met Azure Redis-cache. Vervang `<name>` door de naam van uw cache en `<key>` door ofwel uw primaire of secundaire sleutel zoals beschreven in het vorige gedeelte genaamd [De hostnaam en toegangssleutels ophalen](#retrieve-the-host-name-and-access-keys).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> De poort zonder SSL is uitgeschakeld voor nieuwe Azure Redis Cache-exemplaren. Gebruikt u een andere client die geen ondersteuning biedt voor SSL, raadpleeg dan [Toegang inschakelen voor poort zonder SSL](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en dit ophalen
Het volgende voorbeeld laat zien hoe u verbinding maakt met een exemplaar van de Azure Redis-cache, en hoe u een item opslaat en uit de cache ophaalt. Voor meer voorbeelden van het gebruik van Redis met de [node_redis](https://github.com/mranney/node_redis)-client, verwijzen wij u naar [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Uitvoer:

    OK
    value


## <a name="next-steps"></a>Volgende stappen
* [Schakel de diagnostische gegevens van de cache in](cache-how-to-monitor.md#enable-cache-diagnostics), zodat u de status van de cache kunt [bewaken](cache-how-to-monitor.md).
* Lees de officiële [Redis-documentatie](http://redis.io/documentation).

