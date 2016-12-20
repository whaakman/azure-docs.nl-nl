---
title: Azure Redis-cache gebruiken met behulp van Java | Microsoft Docs
description: Aan de slag met Azure Redis-cache met behulp van Java
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/24/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 408026a8e75272cea92ad62e3a75aabaadf98351
ms.openlocfilehash: 00c4d2e31391297955ecba891e919bda65ddaf8e


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>Azure Redis-cache gebruiken met behulp van Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis-cache geeft u toegang tot een toegewezen Redis-cache, beheerd door Microsoft. Uw cache is toegankelijk vanuit elke toepassing in Microsoft Azure.

In dit onderwerp wordt beschreven hoe u aan de slag kunt met Azure Redis-cache met gebruik van Java.

## <a name="prerequisites"></a>Vereisten
[Jedis](https://github.com/xetorthio/jedis): Java-client voor Redis

In deze zelfstudie wordt Jedis gebruikt, maar u kunt elke andere Java-client gebruiken die wordt vermeld op [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Een Redis-cache maken op Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De hostnaam en toegangssleutels ophalen
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-non-ssl-endpoint"></a>Het eindpunt zonder SSL-beveiliging inschakelen
Sommige Redis-clients bieden geen ondersteuning voor SSL. De [poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe exemplaren van Azure Redis-cache](cache-configure.md#access-ports). Op het moment van publicatie van dit artikel biedt de [Jedis](https://github.com/xetorthio/jedis)-client geen ondersteuning voor SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en dit ophalen
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Volgende stappen
* [Schakel de diagnostische gegevens van de cache in](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), zodat u de status van de cache kunt [bewaken](https://msdn.microsoft.com/library/azure/dn763945.aspx).
* Lees de officiële [Redis-documentatie](http://redis.io/documentation).




<!--HONumber=Nov16_HO5-->


