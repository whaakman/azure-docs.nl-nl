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
ms.date: 08/31/2017
ms.author: sdanie
ms.openlocfilehash: b433eecb0424db85b616c40c5f0cdfc88692cef1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

## <a name="connect-to-the-cache-securely-using-ssl"></a>Veilig verbinding maken met de cache via SSL
De meest recente versies van [jedis](https://github.com/xetorthio/jedis) bieden ondersteuning voor een SSL-verbinding met Azure Redis Cache. Het volgende voorbeeld laat zien hoe u via SSL-eindpunt 6380 verbinding maakt met Azure Redis-cache. Vervang `<name>` door de naam van uw cache en `<key>` door ofwel uw primaire of secundaire sleutel zoals beschreven in het vorige gedeelte genaamd [De hostnaam en toegangssleutels ophalen](#retrieve-the-host-name-and-access-keys).

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> De poort zonder SSL is uitgeschakeld voor nieuwe Azure Redis Cache-exemplaren. Gebruikt u een andere client die geen ondersteuning biedt voor SSL, raadpleeg dan [Toegang inschakelen voor poort zonder SSL](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en dit ophalen
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Volgende stappen
* [Schakel de diagnostische gegevens van de cache in](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), zodat u de status van de cache kunt [bewaken](https://msdn.microsoft.com/library/azure/dn763945.aspx).
* Lees de officiële [Redis-documentatie](http://redis.io/documentation).
* Meer informatie over [het configureren van een Spring Initializr-app voor het gebruik van Redis Cache](cache-java-spring-boot-initializer-with-redis-cache.md).