---
title: Azure Redis-cache gebruiken met behulp van Python | Microsoft Docs
description: Aan de slag met Azure Redis-cache met behulp van Python
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: cdbee52574d0ffbe82ef3dc98f2848f4d00ba2ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-python"></a>Azure Redis-cache gebruiken met behulp van Python
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

In dit onderwerp wordt beschreven hoe u aan de slag kunt met Azure Redis-cache met behulp van Python.

## <a name="prerequisites"></a>Vereisten
Installeer [redis-py](https://github.com/andymccurdy/redis-py).

## <a name="create-a-redis-cache-on-azure"></a>Een Redis-cache maken op Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De hostnaam en toegangssleutels ophalen
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-non-ssl-endpoint"></a>Het eindpunt zonder SSL-beveiliging inschakelen
Sommige Redis-clients bieden geen ondersteuning voor SSL. De [poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe exemplaren van Azure Redis-cache](cache-configure.md#access-ports). Op het moment van publicatie van dit artikel biedt de [redis-py](https://github.com/andymccurdy/redis-py) geen ondersteuning voor SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en dit ophalen
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Vervang `<name>` door de cachenaam en `key` door uw toegangssleutel.

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
