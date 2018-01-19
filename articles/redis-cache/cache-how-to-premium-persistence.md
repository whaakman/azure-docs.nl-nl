---
title: Gegevenspersistentie voor een Premium Azure Redis-Cache configureren
description: Meer informatie over het configureren en beheren van uw Premium-laag Azure Redis-Cache-exemplaren voor gegevenspersistentie
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: wesmc
ms.openlocfilehash: 270158bbf85a58a48a367a091ad2b09a9d114b2b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Gegevenspersistentie voor een Premium Azure Redis-Cache configureren
Azure Redis-Cache heeft verschillende cache aanbiedingen die flexibiliteit bij de keuze van cachegrootte en -functies bieden, zoals de Premium-laag functies zoals clustering, persistentie en virtual network-ondersteuning. In dit artikel wordt beschreven hoe persistentie configureren in een premium Azure Redis-Cache-exemplaar.

Zie voor informatie over andere functies van premium-cache, [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Wat is gegevenspersistentie?
[Redis-persistentie](https://redis.io/topics/persistence) kunt u gegevens die zijn opgeslagen in Redis behouden. U kunt ook momentopnamen en back-up van de gegevens die u in het geval van een hardwarefout laden kunt. Dit is een grote voordeel ten opzichte van de Basic- of Standard-laag waarbij alle gegevens worden opgeslagen in het geheugen en kunnen er mogelijk gegevensverlies in geval van een storing waar Cache-knooppunten niet beschikbaar zijn. 

Azure Redis-Cache biedt met behulp van de volgende modellen Redis-persistentie:

* **RDB persistentie** -persistentie wanneer RDB (Redis database) is geconfigureerd, Azure Redis-Cache een momentopname van de Redis-cache in een binaire indeling naar schijf op basis van een configureerbare back-upfrequentie Redis blijft bestaan. Als er een onherstelbare gebeurtenis optreedt die zowel de primaire als de replica-cache wordt uitgeschakeld, kan de cache is opnieuw opgebouwd met behulp van de meest recente momentopname. Meer informatie over de [voordelen](https://redis.io/topics/persistence#rdb-advantages) en [nadelen](https://redis.io/topics/persistence#rdb-disadvantages) van RDB persistentie.
* **AOF persistentie** -persistentie wanneer AOF (alleen Append-bestand) is geconfigureerd, Azure Redis-Cache wordt elke schrijfbewerking opgeslagen in een logboek dat ten minste eenmaal per seconde bij een Azure Storage-account is opgeslagen. Als er een onherstelbare gebeurtenis optreedt die zowel de primaire als de replica-cache wordt uitgeschakeld, kan de cache is opnieuw opgebouwd met behulp van de opgeslagen schrijfbewerkingen. Meer informatie over de [voordelen](https://redis.io/topics/persistence#aof-advantages) en [nadelen](https://redis.io/topics/persistence#aof-disadvantages) van AOF persistentie.

Persistentie wordt geconfigureerd via de **nieuwe Redis-Cache** blade tijdens het maken van de cache en klik op de **Resource menu** voor bestaande premium in de cache opslaat.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nadat een premium-prijscategorie is geselecteerd, klikt u op **Redis-persistentie**.

![Redis-persistentie][redis-cache-persistence]

De stappen in de volgende sectie wordt beschreven hoe Redis-persistentie configureren op de nieuwe premium-cache. Zodra de Redis-persistentie is geconfigureerd, klikt u op **maken** uw nieuwe premium-cache maken met Redis-persistentie.

## <a name="enable-redis-persistence"></a>Redis-persistentie inschakelen

Redis-persistentie is ingeschakeld op de **Redis-gegevenspersistentie** blade door het kiezen van een **RDB** of **AOF** persistentie. Deze blade wordt geopend tijdens het maken van cache voor nieuwe caches, zoals beschreven in de vorige sectie. Voor bestaande caches de **Redis-gegevenspersistentie** blade wordt geopend vanuit de **Resource menu** voor uw cache.

![Instellingen voor redis][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>RDB persistentie configureren

Als u wilt RDB persistentie inschakelt, klikt u op **RDB**. Als u wilt uitschakelen RDB persistentie op een eerder ingeschakelde premium-cache, **uitgeschakelde**.

![Redis-persistentie RDB][redis-cache-rdb-persistence]

Voor het configureren van de back-interval, selecteer een **back-upfrequentie** uit de vervolgkeuzelijst. U kunt kiezen uit **15 minuten**, **30 minuten**, **60 minuten**, **6 uur**, **12 uur**, en **24 uur**. Dit interval begint te tellen omlaag nadat de vorige back-upbewerking is voltooid en wanneer deze is verstreken een nieuwe back-up wordt gestart.

Klik op **Storage-Account** selecteren van het opslagaccount wilt gebruiken en kiest u de **primaire sleutel** of **secundaire sleutel** gebruiken vanuit de **opslagsleutel** vervolgkeuzelijst. Moet u een opslagaccount in dezelfde regio bevinden als de cache en een **Premium-opslag** account wordt aanbevolen omdat premium-opslag heeft een hogere doorvoer. 

> [!IMPORTANT]
> Als de opslagsleutel voor uw account persistentie wordt opnieuw gegenereerd, moet u de gewenste sleutel in configureren de **opslagsleutel** vervolgkeuzelijst.
> 
> 

Klik op **OK** de persistentie configuratie opslaan.

De volgende back-up (of de eerste back-up voor nieuwe caches) wordt gestart zodra de back-upfrequentie-interval is verstreken.

## <a name="configure-aof-persistence"></a>AOF persistentie configureren

Als u wilt AOF persistentie inschakelt, klikt u op **AOF**. Als u wilt uitschakelen AOF persistentie op een eerder ingeschakelde premium-cache, **uitgeschakelde**.

![Redis-persistentie AOF][redis-cache-aof-persistence]

Geef voor het configureren van persistentie AOF een **eerste Opslagaccount**. Dit opslagaccount moet zich in dezelfde regio bevinden als de cache en een **Premium-opslag** account wordt aanbevolen omdat premium-opslag heeft een hogere doorvoer. U kunt desgewenst een extra storage-account met de naam **tweede Opslagaccount**. Als een tweede storage-account is geconfigureerd, worden schrijfbewerkingen naar de cache replica naar deze tweede storage-account geschreven. Voor elke geconfigureerde storage-account, kies de **primaire sleutel** of **secundaire sleutel** gebruiken vanuit de **opslagsleutel** vervolgkeuzelijst. 

> [!IMPORTANT]
> Als de opslagsleutel voor uw account persistentie wordt opnieuw gegenereerd, moet u de gewenste sleutel in configureren de **opslagsleutel** vervolgkeuzelijst.
> 
> 

Wanneer de persistentie AOF is ingeschakeld, schrijven bewerkingen aan de cache worden opgeslagen in de aangewezen storage-account (of de accounts als u een tweede storage-account hebt geconfigureerd). In het geval van een onherstelbare fout waarmee u zowel de primaire als de replica-cache, wordt het opgeslagen AOF logboek gebruikt voor het opnieuw opbouwen van de cache.

## <a name="persistence-faq"></a>Persistentie Veelgestelde vragen
De volgende lijst bevat antwoorden op veelgestelde vragen over Azure Redis-Cache persistentie.

* [Kan ik persistentie op een eerder gemaakte cache inschakelen?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Kan ik AOF en RDB persistentie op hetzelfde moment inschakelen?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Welk model persistentie moet ik kiezen?](#which-persistence-model-should-i-choose)
* [Wat gebeurt er als ik hebt geschaald naar een andere grootte en een back-up is hersteld die is gemaakt voordat de bewerking uit te schalen?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>RDB persistentie
* [Kan ik de back-upfrequentie RDB wijzigen nadat het maken van de cache?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Waarom als ik een back-upfrequentie RDB van 60 minuten er meer dan 60 minuten tussen back-ups is?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Wat gebeurt er met de oude RDB back-ups wanneer een nieuwe back-up wordt gemaakt?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF persistentie
* [Wanneer moet ik een tweede storage-account gebruiken?](#when-should-i-use-a-second-storage-account)
* [Ondersteunt AOF persistentie invloed in de gehele, latentie, of de prestaties van mijn cache?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Hoe kan ik het tweede storage-account verwijderen?](#how-can-i-remove-the-second-storage-account)
* [Wat is er een herschrijven en wat betekent dat voor mijn cache?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Wat moet ik verwachten tijdens het schalen van een cache met AOF ingeschakeld?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Hoe wordt mijn gegevens AOF ingedeeld in de opslag?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kan ik persistentie op een eerder gemaakte cache inschakelen?
Ja, Redis-persistentie kan worden geconfigureerd op de cache maken en bestaande premium in de cache van.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>Kan ik AOF en RDB persistentie op hetzelfde moment inschakelen?

Nee, kunt u alleen RDB of AOF, maar niet beide op hetzelfde moment inschakelen.

### <a name="which-persistence-model-should-i-choose"></a>Welk model persistentie moet ik kiezen?

AOF persistentie slaat elke schrijfbewerking in een logboek met enige impact op de doorvoer, vergeleken met RDB persistentie waarin wordt opgeslagen back-ups op basis van het geconfigureerde interval van de back-up met minimale gevolgen voor prestaties. Kies AOF persistentie als het primaire doel is om gegevensverlies te minimaliseren en u een afname in doorvoer voor uw cache verwerken kunt. Kies RDB persistentie als u wilt behouden optimale doorvoer in de cache, maar toch een mechanisme om gegevens te herstellen.

* Meer informatie over de [voordelen](https://redis.io/topics/persistence#rdb-advantages) en [nadelen](https://redis.io/topics/persistence#rdb-disadvantages) van RDB persistentie.
* Meer informatie over de [voordelen](https://redis.io/topics/persistence#aof-advantages) en [nadelen](https://redis.io/topics/persistence#aof-disadvantages) van AOF persistentie.

Zie voor meer informatie over prestaties bij gebruik van AOF persistentie [biedt AOF persistentie invloed in de gehele, latentie, of de prestaties van mijn cache?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Wat gebeurt er als ik hebt geschaald naar een andere grootte en een back-up is hersteld die is gemaakt voordat de bewerking uit te schalen?

Voor zowel RDB AOF persistentie:

* Als u hebt geschaald naar een groter, zijn er geen gevolgen.
* Als u hebt geschaald naar een kleinere, en u een aangepaste hebt [databases](cache-configure.md#databases) instelling die groter is dan de [databases limiet](cache-configure.md#databases) voor de grootte van uw nieuwe gegevens in deze databases is niet hersteld. Zie voor meer informatie [Mijn aangepaste databases betrokken instellen tijdens het schalen Is?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Als u een kleinere hebt aangepast, en er niet genoeg ruimte in de kleinere om te kunnen bevatten van de gegevens van de laatste back-up, sleutels wordt onbeschikbaar gemaakt tijdens het herstelproces, meestal met behulp van de [allkeys lru](http://redis.io/topics/lru-cache) taakverwijdering beleid.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>Kan ik de back-upfrequentie RDB wijzigen nadat het maken van de cache?
Ja, kunt u de back-upfrequentie voor RDB persistentie wijzigen op de **Redis-gegevenspersistentie** blade. Zie voor instructies [configureren Redis-persistentie](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Waarom als ik een back-upfrequentie RDB van 60 minuten er meer dan 60 minuten tussen back-ups is?
Het interval RDB persistentie back-upfrequentie start niet totdat de vorige back-upproces is voltooid. Als de back-upfrequentie is 60 minuten duurt het een back-upproces 15 minuten worden voltooid, start de volgende back-up niet pas 75 minuten na de begintijd van de vorige back-up.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Wat gebeurt er met de oude RDB back-ups wanneer een nieuwe back-up wordt gemaakt?
Alle RDB persistentie back-ups, met uitzondering van de meest recente taak worden automatisch verwijderd. Deze verwijdering mogelijk niet direct plaats, maar oudere back-ups zijn niet voor onbepaalde tijd bewaard.


### <a name="when-should-i-use-a-second-storage-account"></a>Wanneer moet ik een tweede storage-account gebruiken?

Als u denkt dat u hoger dan de verwachte set bewerkingen in de cache hebt, moet u een tweede storage-account voor AOF persistentie.  Instellen van de account van de secundaire opslag zorgt ervoor dat uw cache opslaglimieten van bandbreedte niet bereiken.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>Ondersteunt AOF persistentie invloed in de gehele, latentie, of de prestaties van mijn cache?

AOF persistentie is van invloed op doorvoer door ongeveer 15% – 20% wanneer de cache lager dan de maximale belasting is (CPU- en laden beide onder 90%). Er mag niet zijn latentieproblemen wanneer de cache binnen dit bereik is. Echter, de cache wordt komt deze limieten sneller met AOF ingeschakeld.

### <a name="how-can-i-remove-the-second-storage-account"></a>Hoe kan ik het tweede storage-account verwijderen?

U kunt het AOF persistentie secundaire storage-account verwijderen door het instellen van de tweede storage-account moet hetzelfde zijn als het eerste opslagaccount. Zie voor instructies [AOF configureren persistentie](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Wat is er een herschrijven en wat betekent dat voor mijn cache?

Als het bestand AOF erg groot genoeg is, een herschrijven wordt automatisch in de wachtrij in de cache. Het herschrijven wordt het bestand AOF met de minimale set van bewerkingen die nodig zijn voor het maken van de huidige gegevensset. Verwachten tijdens regeneraties, prestatielimieten sneller bereiken met name wanneer omgaan met grote gegevenssets. Regeneraties optreden minder vaak terwijl het bestand AOF groter wordt, maar duurt een aanzienlijke hoeveelheid tijd wanneer dit gebeurt.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Wat moet ik verwachten tijdens het schalen van een cache met AOF ingeschakeld?

Als het bestand AOF op het moment van de schaalbaarheid aanzienlijk groot is, klikt u vervolgens verwachten dat de schaalbewerking duurt langer dan verwacht, omdat deze worden opnieuw van het bestand laden wordt nadat het schalen is voltooid.

Zie voor meer informatie over het schalen [wat gebeurt er als ik hebt geschaald naar een andere grootte en een back-up is hersteld die is gemaakt voordat de bewerking uit te schalen?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Hoe wordt mijn gegevens AOF ingedeeld in de opslag?

Gegevens die zijn opgeslagen in AOF bestanden is onderverdeeld in meerdere pagina-blobs per knooppunt voor betere prestaties van het opslaan van de gegevens in de opslag. De volgende tabel geeft weer hoeveel pagina-BLOB's worden gebruikt voor elke prijscategorie:

| Premiumlaag | Blobs |
|--------------|-------|
| P1           | 4 per shard    |
| P2           | 8 per shard    |
| P3           | 16 per shard   |
| P4           | 20 per shard   |

Wanneer clustering is ingeschakeld, heeft elke shard in de cache een eigen set van pagina-blobs, zoals aangegeven in de vorige tabel. Een cache P2 met drie shards verdeelt bijvoorbeeld het bestand AOF over 24 pagina-blobs (8 BLOB's per shard met 3 shards).

Na een herschrijven bestaan twee sets van AOF bestanden in de opslag. Regeneraties plaatsvinden op de achtergrond en toevoegen aan de eerste set van bestanden, terwijl de set-bewerkingen die worden verzonden naar de cache tijdens het herschrijven toevoegen aan de tweede set. Een back-up wordt tijdelijk opgeslagen tijdens regeneraties in geval van storing, maar onmiddellijk is verwijderd nadat een herschrijven is voltooid.


## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer Premiumfuncties cache.

* [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
