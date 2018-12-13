---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 71d19b5d922616944c176cdda98470607c515af9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111641"
---
| Resource | Limiet |
| --- | --- |
| Cachegrootte |530 GB |
| Databases |64 |
| Maximum aantal verbonden clients |40,000 |
| Azure Cache voor Redis-replica's (voor hoge beschikbaarheid) |1 |
| Shards in een premium-cache met clustering |10 |

Azure Redis-Cache wordt beperkt en -grootten zijn verschillend voor elke prijscategorie. Zie voor de Prijscategorieën en de bijbehorende grootte [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/pricing/details/cache/).

Zie voor meer informatie over Azure Cache voor Redis-configuratie limieten, [configuratie van de standaardconfiguratie voor Redis-server](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Omdat de configuratie en beheer van Azure Cache voor instanties van Redis wordt uitgevoerd door Microsoft, worden niet alle Redis-opdrachten in Azure Cache ondersteund voor Redis. Zie voor meer informatie, [Redis-opdrachten niet ondersteund in Azure Cache voor Redis](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

