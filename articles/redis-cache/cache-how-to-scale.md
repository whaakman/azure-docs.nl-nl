---
title: Schalen van Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het schalen van uw Azure Redis-Cache-exemplaren
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: wesmc
ms.openlocfilehash: bee7771c53cfad4a925d5c270569b7a82e45b4d8
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-scale-azure-redis-cache"></a>Schalen van Azure Redis-Cache
Azure Redis-Cache heeft verschillende cache aanbiedingen die flexibiliteit bij de keuze van de cachegrootte en -functies bieden. Nadat een cache is gemaakt, kunt u de grootte en de prijscategorie van de cache schalen als de vereisten van uw toepassing veranderen. In dit artikel leest u hoe schalen van uw cache in de Azure portal en gebruik van hulpprogramma's zoals Azure PowerShell en Azure CLI.

## <a name="when-to-scale"></a>Wanneer schalen
U kunt de [bewaking](cache-how-to-monitor.md) functies van Azure Redis-Cache om te controleren van de status en prestaties van de cache en te bepalen wanneer schalen van de cache. 

U kunt de volgende metrische gegevens om te bepalen wat als u wilt schalen bewaken.

* Redis-serververmogen
* Geheugengebruik
* Netwerkbandbreedte
* CPU-gebruik

Als u vaststelt dat uw cache niet meer van uw toepassing vereisten voldoet, kunt u naar een grotere of kleinere cache prijscategorie die geschikt is voor uw toepassing schalen. Zie voor meer informatie over het bepalen van welke prijscategorie gebruiken cache [welke aanbieding Redis-Cache en de grootte moet ik gebruiken](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Een cache schalen
Schalen van uw cache [Blader naar de cache](cache-configure.md#configure-redis-cache-settings) in de [Azure-portal](https://portal.azure.com) en klik op **Scale** van de **Resource menu**.

![Schalen](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Selecteer de gewenste prijscategorie van de **Selecteer prijscategorie** blade en klik op **Selecteer**.

![Prijscategorie][redis-cache-pricing-tier-blade]


U kunt schalen naar een andere prijscategorie met de volgende beperkingen:

* U kan niet uit een hogere prijscategorie schalen naar een lagere prijscategorie.
  * U kunt geen schalen van een **Premium** omlaag naar de cache een **standaard** of een **Basic** cache.
  * U kunt geen schalen van een **standaard** omlaag naar de cache een **Basic** cache.
* U kunt opschalen van een **Basic** in de cache op een **standaard** cache, maar de grootte niet wijzigen op hetzelfde moment. Als u een ander formaat nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
* U kunt geen schalen van een **Basic** rechtstreeks naar de cache een **Premium** cache. Moet u de schaal van **Basic** naar **standaard** in één bewerking van de schaal en vervolgens van **standaard** naar **Premium** in een latere bewerking voor vergroten/verkleinen.
* Kan niet worden geschaald uit een groter formaat omlaag naar de **C0 (250 MB)** grootte.
 
Terwijl de cache wordt geschaald naar de nieuwe prijscategorie een **schaal** status wordt weergegeven in de **Redis-Cache** blade.

![Schalen][redis-cache-scaling]

Wanneer schalen voltooid is, wordt de status verandert van **schaal** naar **met**.

## <a name="how-to-automate-a-scaling-operation"></a>Een bewerking voor vergroten/verkleinen automatiseren
Naast het schalen van uw cache-exemplaren in de Azure portal, kunt u de schaal met behulp van PowerShell-cmdlets, Azure CLI en met behulp van de Microsoft Azure Management-bibliotheken (MAML). 

* [Schaal met behulp van PowerShell](#scale-using-powershell)
* [Schaal met Azure CLI](#scale-using-azure-cli)
* [Met behulp van MAML schaal](#scale-using-maml)

### <a name="scale-using-powershell"></a>Schaal met behulp van PowerShell
U kunt uw Azure Redis-Cache-exemplaren met PowerShell schalen met behulp van de [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet wanneer de `Size`, `Sku`, of `ShardCount` eigenschappen zijn gewijzigd. Het volgende voorbeeld laat zien hoe een cache met de naam schalen `myCache` naar een cache 2,5 GB. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Zie voor meer informatie over het schalen met PowerShell [schalen van een Redis-cache met behulp van Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Schaal met Azure CLI
Als u wilt schalen van uw Azure Redis-Cache-exemplaren die gebruikmaken van Azure CLI, roepen de `azure rediscache set` opdracht en in de gewenste configuratiewijzigingen die een nieuwe grootte, sku of clustergrootte, afhankelijk van de gewenste schaal bewerking bevatten.

Zie voor meer informatie over het schalen met Azure CLI [instellingen van een bestaande Redis-Cache wijzigen](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Met behulp van MAML schaal
Schalen van uw Azure Redis-Cache-exemplaren met de [Microsoft Azure Management-bibliotheken (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), Roep de `IRedisOperations.CreateOrUpdate` methode en geeft u de nieuwe grootte voor de `RedisProperties.SKU.Capacity`.

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

## <a name="scaling-faq"></a>Veelgestelde vragen over het schalen
De volgende lijst bevat antwoorden op veelgestelde vragen over het schalen van Azure Redis-Cache.

* [Kan ik worden geschaald naar, uit of binnen een Premium-cache?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Na de schaal heb ik mijn cache naam of toegang tot de sleutels wijzigen?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Hoe schalen werkt?](#how-does-scaling-work)
* [Ik verliest gegevens uit de cache tijdens schalen?](#will-i-lose-data-from-my-cache-during-scaling)
* [Mijn aangepaste databases configureert betrokken tijdens schalen?](#is-my-custom-databases-setting-affected-during-scaling)
* [Mijn cache is beschikbaar tijdens het schalen?](#will-my-cache-be-available-during-scaling)
* [Bewerkingen die worden niet ondersteund](#operations-that-are-not-supported)
* [Hoe lang duurt schalen voordat?](#how-long-does-scaling-take)
* [Hoe weet ik wanneer de schaal is voltooid?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kan ik worden geschaald naar, uit of binnen een Premium-cache?
* U kunt geen schalen van een **Premium** omlaag naar de cache een **Basic** of **standaard** prijscategorie.
* U kunt opschalen van een **Premium** cache prijscategorie naar een andere.
* U kunt geen schalen van een **Basic** rechtstreeks naar de cache een **Premium** cache. U moet eerst de schaal van **Basic** naar **standaard** in één bewerking van de schaal en vervolgens van **standaard** naar **Premium** in een toekomstige de bewerking is vergroten/verkleinen.
* Als u de clustering ingeschakeld tijdens het maken van uw **Premium** cache, kunt u [wijzigen van de clustergrootte](cache-how-to-premium-clustering.md#cluster-size). Als uw cache is gemaakt zonder clustering is ingeschakeld, kunt u op een later tijdstip clustering niet configureren.
  
  Zie [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Clustering voor een Premium Azure Redis Cache configureren) voor meer informatie.

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Na de schaal heb ik mijn cache naam of toegang tot de sleutels wijzigen?
Nee, uw Cachenaam en sleutels zijn niet gewijzigd tijdens een bewerking voor vergroten/verkleinen.

### <a name="how-does-scaling-work"></a>Hoe schalen werkt?
* Wanneer een **Basic** cache is geschaald naar een andere grootte, deze wordt afgesloten en een nieuwe cache is ingericht met behulp van de nieuwe grootte. Gedurende deze tijd de cache is niet beschikbaar en de gegevens in de cache worden verwijderd.
* Wanneer een **Basic** cache is geschaald naar een **standaard** cache, een replica-cache is ingericht en de gegevens uit de primaire cache wordt gekopieerd naar de replica-cache. De cache blijft beschikbaar tijdens het vergroten/verkleinen.
* Wanneer een **standaard** cache wordt aangepast aan een andere grootte of aan een **Premium** cache, op een van de replica's wordt afgesloten en opnieuw wordt ingericht voor de nieuwe grootte en de overgedragen gegevens en de andere replica voert een failover voordat deze opnieuw worden ingericht, vergelijkbaar met het proces dat bij uitval van een van de cache-knooppunten plaatsvindt.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Ik verliest gegevens uit de cache tijdens schalen?
* Wanneer een **Basic** cache is geschaald naar een nieuwe grootte, alle gegevens verloren en wordt de cache is niet beschikbaar tijdens de bewerking uit te schalen.
* Wanneer een **Basic** cache is geschaald naar een **standaard** cache, de gegevens in de cache wordt doorgaans behouden.
* Wanneer een **standaard** cache is geschaald naar een grotere grootte of laag, of een **Premium** cache is geschaald naar een groter, blijven doorgaans alle gegevens behouden. Wanneer een **standaard** of **Premium** cache naar beneden op een kleinere, gegevens zijn mogelijk verloren gegaan, afhankelijk van hoeveel gegevens zich in de cache die betrekking hebben op de nieuwe grootte wanneer deze wordt geschaald. Als gegevens verloren gaan tijdens het omlaag schalen, sleutels zijn verwijderd met behulp van de [allkeys lru](http://redis.io/topics/lru-cache) taakverwijdering beleid. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Mijn aangepaste databases configureert betrokken tijdens schalen?
Sommige Prijscategorieën hebben verschillende [databases limieten](cache-configure.md#databases), dus er enkele overwegingen zijn wanneer het verkleinen van als u een aangepaste waarde voor geconfigureerd de `databases` instellen tijdens het maken van de cache.

* Tijdens het schalen naar een prijscategorie met een lagere `databases` limiet dan de huidige tier:
  * Als u het standaardaantal `databases` waarmee 16 voor alle Prijscategorieën is, gegevens niet verloren.
  * Als u een aangepaste aantal `databases` die valt binnen de grenzen voor de laag waarop u hebben, dit `databases` instelling blijft behouden en gegevens niet verloren.
  * Als u een aangepaste aantal `databases` die overschrijdt de grenzen van de nieuwe laag, de `databases` instelling is verlaagd tot de grenzen van de nieuwe laag en alle gegevens in de verwijderde databases wordt verbroken.
* Wanneer een de dezelfde of een hogere prijscategorie schalen `databases` limiet dan de huidige tier uw `databases` instelling blijft behouden en gegevens niet verloren.

Houd er rekening mee dat Standard en Premium-caches hebben een SLA met 99,9% voor beschikbaarheid, maar er geen SLA gegevens verloren zijn gegaan is.

### <a name="will-my-cache-be-available-during-scaling"></a>Mijn cache is beschikbaar tijdens het schalen?
* **Standaard** en **Premium** caches beschikbaar blijven tijdens de bewerking uit te schalen.
* **Basic** caches zijn offline tijdens het schalen van bewerkingen die een ander formaat, maar beschikbaar blijven tijdens het schalen van **Basic** naar **standaard**.

### <a name="operations-that-are-not-supported"></a>Bewerkingen die worden niet ondersteund
* U kan niet uit een hogere prijscategorie schalen naar een lagere prijscategorie.
  * U kunt geen schalen van een **Premium** omlaag naar de cache een **standaard** of een **Basic** cache.
  * U kunt geen schalen van een **standaard** omlaag naar de cache een **Basic** cache.
* U kunt opschalen van een **Basic** in de cache op een **standaard** cache, maar de grootte niet wijzigen op hetzelfde moment. Als u een ander formaat nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
* U kunt geen schalen van een **Basic** rechtstreeks naar de cache een **Premium** cache. U moet eerst de schaal van **Basic** naar **standaard** in één bewerking van de schaal en vervolgens van **standaard** naar **Premium** in een toekomstige de bewerking is vergroten/verkleinen.
* Kan niet worden geschaald uit een groter formaat omlaag naar de **C0 (250 MB)** grootte.

Als een vergroten/verkleinen mislukt, wordt geprobeerd om de bewerking terug te zetten van de service en de cache wordt teruggezet naar de oorspronkelijke grootte.

### <a name="how-long-does-scaling-take"></a>Hoe lang duurt schalen voordat?
Schalen duurt ongeveer 20 minuten, afhankelijk van hoeveel gegevens zich in de cache.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Hoe weet ik wanneer de schaal is voltooid?
In de Azure portal ziet u de schaal bewerking uitgevoerd. Wanneer schalen voltooid is, wordt de status van de cache gewijzigd in **met**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



