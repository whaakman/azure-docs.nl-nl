---
title: Het schalen van Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het schalen van uw Azure Redis Cache-exemplaren
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: wesmc
ms.openlocfilehash: 885258379e71ea945e41c4b43c34b35b16dd4a7a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060156"
---
# <a name="how-to-scale-azure-redis-cache"></a>Het schalen van Azure Redis-Cache
Azure Redis-Cache heeft een ander cache-aanbiedingen, waardoor u flexibiliteit bij de keuze van de grootte van de cache en functies. Nadat een cache is gemaakt, kunt u de grootte en de prijscategorie van de cache schalen als de vereisten van uw toepassing veranderen. Dit artikel ziet u hoe u de schaal van uw cache met behulp van de Azure-portal en hulpprogramma's, zoals Azure PowerShell en Azure CLI.

## <a name="when-to-scale"></a>Wanneer schalen
U kunt de [bewaking](cache-how-to-monitor.md) functies van Azure Redis Cache voor de status en prestaties van uw cache controleren en bepalen wanneer u de schaal van de cache. 

U kunt de volgende metrische gegevens om te bepalen of u wilt schalen bewaken.

* Redis-serververmogen
* Geheugengebruik
* Netwerkbandbreedte
* CPU-gebruik

Als u vaststelt dat de cache niet langer voldoen aan de vereisten van uw toepassing, kunt u schaalt naar een grotere of kleinere cache prijscategorie die geschikt is voor uw toepassing. Zie voor meer informatie over het bepalen van welke cache prijscategorie te gebruiken, [welke Redis-Cache-aanbieding en de grootte moet ik gebruiken](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Een cache schalen
Voor het schalen van uw cache [bladert u naar de cache](cache-configure.md#configure-redis-cache-settings) in de [Azure-portal](https://portal.azure.com) en klikt u op **schaal** uit de **resourcemenu**.

![Schalen](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Selecteer de gewenste prijscategorie van de **Selecteer prijscategorie** blad en klik op **Selecteer**.

![Prijscategorie][redis-cache-pricing-tier-blade]


U kunt schalen naar een andere prijscategorie met de volgende beperkingen:

* U kan niet uit een hogere prijscategorie schalen in een lagere prijscategorie.
  * Kan niet worden geschaald van een **Premium** omlaag naar de cache een **Standard** of een **Basic** cache.
  * Kan niet worden geschaald van een **Standard** omlaag naar de cache een **Basic** cache.
* Kunt u de schaal van een **Basic** in de cache op een **Standard** cache, maar u de grootte op hetzelfde moment niet wijzigen. Als u een andere grootte nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
* Kan niet worden geschaald van een **Basic** rechtstreeks naar de cache een **Premium** cache. Eerst schalen van **Basic** naar **Standard** in één bewerking voor vergroten/verkleinen en klik in **Standard** naar **Premium** in een latere schalen de bewerking.
* U kan niet schalen van een groter formaat omlaag naar de **C0 (250 MB)** grootte.
 
Terwijl de cache wordt geschaald naar de nieuwe prijscategorie, een **schalen** status wordt weergegeven in de **Redis-Cache** blade.

![Schalen][redis-cache-scaling]

Wanneer schalen voltooid is, wordt de status verandert van **schalen** naar **met**.

## <a name="how-to-automate-a-scaling-operation"></a>Over het automatiseren van een bewerking voor vergroten/verkleinen
Naast het schalen van uw cache-exemplaren in Azure portal, kunt u schalen met behulp van PowerShell-cmdlets, Azure CLI, en met behulp van de Microsoft Azure Management-bibliotheken (MAML). 

* [Schaal met behulp van PowerShell](#scale-using-powershell)
* [Schaal met behulp van Azure CLI](#scale-using-azure-cli)
* [Schaal met behulp van MAML](#scale-using-maml)

### <a name="scale-using-powershell"></a>Schaal met behulp van PowerShell
U kunt uw Azure Redis Cache-exemplaren met PowerShell schalen met behulp van de [Set-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/set-azurermrediscache?view=azurermps-6.6.0) cmdlet wanneer de `Size`, `Sku`, of `ShardCount` eigenschappen worden gewijzigd. Het volgende voorbeeld laat zien hoe de schaal van een cache met de naam `myCache` tot een 2,5 GB-cache. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Zie voor meer informatie over de schaal aanpassen met PowerShell [voor het schalen van een Redis-cache met behulp van Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Schaal met behulp van Azure CLI
Aanroepen voor het schalen van uw Azure Redis-Cache-exemplaren met behulp van Azure CLI, de `azure rediscache set` beheren en doorgeven in de gewenste configuratiewijzigingen die een nieuwe grootte, sku of grootte van het cluster, afhankelijk van de gewenste schaal bewerking bevatten.

Zie voor meer informatie over het omhoog schalen met Azure CLI [instellingen van een bestaand Redis-Cache wijzigen](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Schaal met behulp van MAML
Voor het schalen van uw Azure Redis Cache-exemplaren met behulp van de [Microsoft Azure Management-bibliotheken (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), Roep de `IRedisOperations.CreateOrUpdate` methode en geeft u de nieuwe grootte voor de `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Zie voor meer informatie de [Redis-Cache beheren met behulp van MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) voorbeeld.

## <a name="scaling-faq"></a>Veelgestelde vragen over schalen
De volgende lijst bevat antwoorden op veelgestelde vragen over het schalen van Azure Redis-Cache.

* [Kan ik schalen naar, vanuit of binnen een Premium-cache?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Na het schalen heb ik mijn cache naam of toegang tot de sleutels wijzigen?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Hoe schalen werkt?](#how-does-scaling-work)
* [Ik gaan gegevens verloren uit mijn cache tijdens schalen?](#will-i-lose-data-from-my-cache-during-scaling)
* [Wordt mijn aangepaste databases instellen waarin dit probleem optreedt tijdens schalen?](#is-my-custom-databases-setting-affected-during-scaling)
* [Mijn cache is beschikbaar tijdens de schalen?](#will-my-cache-be-available-during-scaling)
* [Met Geo-replicatie is geconfigureerd, waarom kan ik niet kunnen de schaal van mijn cache of wijzigen van de shards in een cluster?](#scaling-limitations-with-geo-relication)
* [Bewerkingen die worden niet ondersteund](#operations-that-are-not-supported)
* [Hoe lang duurt schalen voordat?](#how-long-does-scaling-take)
* [Hoe weet ik wanneer schalen is voltooid?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kan ik schalen naar, vanuit of binnen een Premium-cache?
* Kan niet worden geschaald van een **Premium** omlaag naar de cache een **Basic** of **Standard** prijscategorie.
* U kunt schalen van één **Premium** prijscategorie naar een andere cache.
* Kan niet worden geschaald van een **Basic** rechtstreeks naar de cache een **Premium** cache. Eerst schalen van **Basic** naar **Standard** in één bewerking voor vergroten/verkleinen en klik in **Standard** naar **Premium** in een latere schalen de bewerking.
* Als u de clustering ingeschakeld tijdens het maken van uw **Premium** cache, kunt u [wijzigen van de clustergrootte](cache-how-to-premium-clustering.md#cluster-size). Als uw cache is gemaakt zonder clustering is ingeschakeld, kunt u de clustering op een later tijdstip kunt configureren.
  
  Zie [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Clustering voor een Premium Azure Redis Cache configureren) voor meer informatie.

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Na het schalen heb ik mijn cache naam of toegang tot de sleutels wijzigen?
Nee, uw Cachenaam en sleutels zijn niet gewijzigd tijdens een bewerking voor vergroten/verkleinen.

### <a name="how-does-scaling-work"></a>Hoe schalen werkt?
* Wanneer een **Basic** cache wordt geschaald naar een andere grootte, deze wordt afgesloten en een nieuwe cache is ingericht met behulp van de nieuwe grootte. Gedurende deze tijd de cache niet beschikbaar is en de gegevens in de cache worden verwijderd.
* Wanneer een **Basic** cache wordt aangepast aan een **Standard** cache, een replica-cache is ingericht en de gegevens worden gekopieerd uit de primaire cache aan de replica-cache. De cache blijven beschikbaar tijdens het proces voor vergroten/verkleinen.
* Wanneer een **Standard** cache wordt geschaald naar een andere grootte of naar een **Premium** cache, op een van de replica's wordt afgesloten en ingericht naar de nieuwe grootte en de gegevens overgedragen en de andere replica voert een failover voordat deze is ingericht, vergelijkbaar met het proces dat tijdens een storing van een van de cache-knooppunten plaatsvindt.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Ik gaan gegevens verloren uit mijn cache tijdens schalen?
* Wanneer een **Basic** cache is geschaald naar een nieuwe omvang, alle gegevens verloren en wordt de cache niet beschikbaar is tijdens het vergroten/verkleinen.
* Wanneer een **Basic** cache wordt aangepast aan een **Standard** cache, de gegevens in de cache wordt doorgaans behouden.
* Wanneer een **Standard** cache wordt aangepast aan een groter formaat of laag, of een **Premium** cache wordt aangepast aan een groter, alle gegevens doorgaans blijven behouden. Wanneer u een **Standard** of **Premium** cache tot een kleinere, gegevens kan verloren gaan, afhankelijk van hoeveel gegevens in de cache met betrekking tot de nieuwe grootte is wanneer deze wordt geschaald. Als gegevens verloren gaan tijdens het omlaag schalen, sleutels worden verwijderd met behulp van de [allkeys lru](http://redis.io/topics/lru-cache) verwijderingsbeleid. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Wordt mijn aangepaste databases instellen waarin dit probleem optreedt tijdens schalen?
Als u een aangepaste waarde voor geconfigureerd de `databases` instellen tijdens het maken van de cache, houd er rekening mee dat sommige prijzen van lagen hebben verschillende [limieten voor databases](cache-configure.md#databases). Hier volgen enkele overwegingen wanneer u in dit scenario:

* Wanneer u naar een prijscategorie met een lagere `databases` limiet dan de huidige laag:
  * Als u het standaardaantal `databases`, welke 16 voor alle Prijscategorieën is, geen gegevens verloren zijn gegaan.
  * Als u een aangepaste aantal `databases` die valt binnen de grenzen voor de laag waarop u bent schalen, dit `databases` instelling blijft behouden en er worden geen gegevens verloren.
  * Als u een aangepaste aantal `databases` die groter is dan de limieten van de nieuwe laag de `databases` instelling met de limieten van de nieuwe laag worden verlaagd en alle gegevens in de verwijderde databases wordt verbroken.
* Wanneer u naar een prijscategorie met de dezelfde of een hogere `databases` limiet dan de huidige laag uw `databases` instelling blijft behouden en er worden geen gegevens verloren.

Standard en Premium-caches hebben een SLA van 99,9% voor beschikbaarheid, maar er is geen SLA voor verlies van gegevens.

### <a name="will-my-cache-be-available-during-scaling"></a>Mijn cache is beschikbaar tijdens de schalen?
* **Standard** en **Premium** caches beschikbaar blijven tijdens het vergroten/verkleinen. Verbinding blips kunnen echter optreden tijdens het schalen van Standard en Premium-caches, en ook tijdens van Basic naar Standard-caches te schalen. Deze verbinding blips wordt te klein en redis-clients kan opnieuw verbinding tot stand brengen hun direct.
* **Basic** caches zijn offline tijdens het schalen herverdelen naar een andere grootte. Basic caches beschikbaar blijven in geval van schalen **Basic** naar **Standard** maar een klein verbinding blip kunnen optreden. Als er een verbinding blip optreedt, redis-clients zou het mogelijk opnieuw verbinding tot stand brengen hun direct.


### <a name="scaling-limitations-with-geo-replication"></a>Beperkingen voor vergroten/verkleinen met Geo-replicatie

Nadat u een koppeling voor Geo-replicatie tussen twee caches hebt toegevoegd, wordt het niet meer mogelijk een vergroten/verkleinen bewerking starten of wijzigen van het aantal shards in een cluster. U moet de cache voor het uitgeven van deze opdrachten niet ontkoppelen. Zie voor meer informatie, [Geo-replicatie configureren](cache-how-to-geo-replication.md).


### <a name="operations-that-are-not-supported"></a>Bewerkingen die worden niet ondersteund
* U kan niet uit een hogere prijscategorie schalen in een lagere prijscategorie.
  * Kan niet worden geschaald van een **Premium** omlaag naar de cache een **Standard** of een **Basic** cache.
  * Kan niet worden geschaald van een **Standard** omlaag naar de cache een **Basic** cache.
* Kunt u de schaal van een **Basic** in de cache op een **Standard** cache, maar u de grootte op hetzelfde moment niet wijzigen. Als u een andere grootte nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
* Kan niet worden geschaald van een **Basic** rechtstreeks naar de cache een **Premium** cache. Eerst schalen van **Basic** naar **Standard** in één bewerking voor vergroten/verkleinen en schaal van **Standard** naar **Premium** in een toekomstige versies de bewerking.
* U kan niet schalen van een groter formaat omlaag naar de **C0 (250 MB)** grootte.

Als een vergroten/verkleinen bewerking mislukt, de service probeert om terug te zetten van de bewerking en de cache wordt hersteld naar de oorspronkelijke grootte.


### <a name="how-long-does-scaling-take"></a>Hoe lang duurt schalen voordat?
Schaal duurt ongeveer 20 minuten, afhankelijk van hoeveel gegevens zich in de cache.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Hoe weet ik wanneer schalen is voltooid?
In de Azure-portal ziet u de schaal bewerking wordt uitgevoerd. Wanneer schalen voltooid is, de status van de cache moet worden gewijzigd in **met**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



