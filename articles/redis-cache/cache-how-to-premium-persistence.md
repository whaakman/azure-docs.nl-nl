---
title: Persistentie van gegevens voor een Premium Azure Redis Cache configureren
description: Meer informatie over het configureren en beheren van uw Azure Redis-Cache-instanties van de Premium-laag voor persistentie van gegevens
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: wesmc
ms.openlocfilehash: 270158bbf85a58a48a367a091ad2b09a9d114b2b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38700880"
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Persistentie van gegevens voor een Premium Azure Redis Cache configureren
Azure Redis-Cache heeft een ander cache-aanbiedingen die over de benodigde flexibiliteit bij de keuze van de grootte van de cache en -onderdelen, met inbegrip van Premium-functies zoals clustering, persistentie en virtual network-ondersteuning. In dit artikel wordt beschreven hoe u persistentie configureren in een premium Azure Redis-Cache-exemplaar.

Zie voor meer informatie over andere functies van de cache premium [Inleiding tot de Azure Redis Cache Premium-laag](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Wat is de persistentie van gegevens?
[Redis-persistentie](https://redis.io/topics/persistence) kunt u het behoud van gegevens die zijn opgeslagen in Redis. U kunt ook momentopnamen maken en back-up van de gegevens die u in het geval van een hardwarestoring optreedt laden kunt. Dit is een enorm voordeel ten opzichte van Basic of Standard-laag waarin alle gegevens worden opgeslagen in het geheugen en kunnen er mogelijk gegevensverlies in geval van een storing waar Cache-knooppunten niet beschikbaar zijn. 

Azure Redis Cache biedt Redis-persistentie met behulp van de volgende modellen:

* **De RDB-persistentie** -persistentie wanneer RDB (Redis-database) is geconfigureerd, Azure Redis-Cache zich blijft voordoen een momentopname van de Redis-cache in een Redis die binaire indeling op schijf op basis van een back-upfrequentie die kunnen worden geconfigureerd. Als er een catastrofale gebeurtenis optreedt die zowel de primaire en replica-cache wordt uitgeschakeld, kan de cache is opnieuw opgebouwd met behulp van de meest recente momentopname. Meer informatie over de [voordelen](https://redis.io/topics/persistence#rdb-advantages) en [nadelen](https://redis.io/topics/persistence#rdb-disadvantages) van RDB persistentie.
* **AOF-persistentie** -persistentie wanneer AOF (alleen Append-bestand) is geconfigureerd, Azure Redis Cache slaat elke schrijfbewerking in een logboek dat ten minste één keer per seconde in een Azure Storage-account is opgeslagen. Als er een catastrofale gebeurtenis optreedt die zowel de primaire en replica-cache wordt uitgeschakeld, kan de cache is opnieuw opgebouwd met behulp van de opgeslagen schrijfbewerkingen. Meer informatie over de [voordelen](https://redis.io/topics/persistence#aof-advantages) en [nadelen](https://redis.io/topics/persistence#aof-disadvantages) van AOF persistentie.

Persistentie wordt geconfigureerd via de **nieuwe Redis-Cache** blade tijdens het maken van de cache en klik op de **resourcemenu** voor bestaande premium in de cache opslaat.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nadat een premium-prijscategorie is geselecteerd, klikt u op **Redis-persistentie**.

![Redis-persistentie][redis-cache-persistence]

De stappen in de volgende sectie wordt beschreven hoe u Redis-persistentie configureren op uw nieuwe premium-cache. Nadat de Redis-persistentie is geconfigureerd, klikt u op **maken** naar uw nieuwe premium-cache maken met Redis-persistentie.

## <a name="enable-redis-persistence"></a>Redis-persistentie inschakelen

Redis-persistentie is ingeschakeld op de **Redis-gegevenspersistentie** blade door het kiezen van een **RDB** of **AOF** persistentie. Deze blade wordt geopend tijdens het maakproces cache voor nieuwe caches, zoals beschreven in de vorige sectie. Voor bestaande caches de **Redis-gegevenspersistentie** blade is toegankelijk vanuit de **resourcemenu** voor uw cache.

![Redis-instellingen][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>De RDB-persistentie configureren

Als de RDB-persistentie, klikt u op **RDB**. Als u wilt de RDB-persistentie op een eerder ingeschakelde premium-cache uitschakelen, klikt u op **uitgeschakelde**.

![Redis-persistentie RDB][redis-cache-rdb-persistence]

Voor het configureren van de back-upinterval, selecteer een **back-upfrequentie** uit de vervolgkeuzelijst. U kunt kiezen uit **15 minuten**, **30 minuten**, **60 minuten**, **6 uur**, **12 uur**, en **24 uur**. Dit interval begint te tellen omlaag nadat de vorige back-upbewerking is voltooid en wanneer deze is verstreken een nieuwe back-up wordt gestart.

Klik op **Storage-Account** om te selecteren van de storage-account te gebruiken, kiest u de **primaire sleutel** of **secundaire sleutel** gebruiken vanuit de **opslagsleutel** vervolgkeuzelijst. Moet u een opslagaccount in dezelfde regio als de cache en een **Premium Storage** account wordt aanbevolen omdat premium-opslag heeft een hogere doorvoer. 

> [!IMPORTANT]
> Als u de toegangssleutel voor uw account voor de persistentie opnieuw is gegenereerd, moet u opnieuw configureren de gewenste sleutel in de **opslagsleutel** vervolgkeuzelijst.
> 
> 

Klik op **OK** om op te slaan van de configuratie van de persistentie.

De volgende back-up (of de eerste back-up voor nieuwe caches) wordt gestart zodra de back-upfrequentie-interval is verstreken.

## <a name="configure-aof-persistence"></a>AOF-persistentie configureren

Als AOF persistentie, klikt u op **AOF**. Als u wilt uitschakelen AOF-persistentie op een eerder ingeschakelde premium-cache, klikt u op **uitgeschakelde**.

![Redis-persistentie AOF][redis-cache-aof-persistence]

Als u wilt configureren AOF persistentie, Geef een **eerste Opslagaccount**. Dit opslagaccount moet zich in dezelfde regio bevinden als de cache en een **Premium Storage** account wordt aanbevolen omdat premium-opslag heeft een hogere doorvoer. U kunt eventueel een extra opslagaccount met de naam **tweede Opslagaccount**. Als een tweede storage-account is geconfigureerd, worden de schrijfbewerkingen naar de replica-cache met deze tweede storage-account geschreven. Voor elke geconfigureerde storage-account, kiest u de **primaire sleutel** of **secundaire sleutel** gebruiken vanuit de **opslagsleutel** vervolgkeuzelijst. 

> [!IMPORTANT]
> Als u de toegangssleutel voor uw account voor de persistentie opnieuw is gegenereerd, moet u opnieuw configureren de gewenste sleutel in de **opslagsleutel** vervolgkeuzelijst.
> 
> 

Wanneer AOF persistentie is ingeschakeld, schrijft u bewerkingen met de cache worden opgeslagen in de aangewezen storage-account (of de accounts als u een tweede storage-account hebt geconfigureerd). De opgeslagen AOF-logboek wordt gebruikt in het geval van een onherstelbare fout waarmee u zowel de primaire en replica-cache, opnieuw opbouwen van de cache.

## <a name="persistence-faq"></a>Veelgestelde vragen over persistentie
De volgende lijst bevat antwoorden op veelgestelde vragen over Azure Redis Cache persistentie.

* [Kan ik de persistentie voor een eerder gemaakte cache inschakelen?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Kan ik AOF en RDB persistentie op hetzelfde moment inschakelen?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Welk model u persistentie moet ik kiezen?](#which-persistence-model-should-i-choose)
* [Wat gebeurt er als ik naar een andere grootte hebt geschaald, en een back-up die is gemaakt voordat de bewerking vergroten/verkleinen is hersteld?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>De RDB-persistentie
* [Kan ik de RDB-back-upfrequentie wijzigen nadat ik de cache maken?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Waarom als ik een back-upfrequentie RDB van 60 minuten er meer dan 60 minuten tussen back-ups is?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Wat gebeurt er met de oude RDB-back-ups wanneer een nieuwe back-up wordt gemaakt?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF-persistentie
* [Wanneer moet ik een tweede storage-account gebruiken?](#when-should-i-use-a-second-storage-account)
* [AOF persistentie hebben invloed op in de gehele, latentie, of de prestaties van mijn cache gebruikt?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Hoe kan ik de tweede storage-account verwijderen?](#how-can-i-remove-the-second-storage-account)
* [Wat is een herschrijven en wat betekent dat mijn cache voor?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Wat kan ik verwachten wanneer u een cache met AOF ingeschakeld?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Hoe wordt mijn AOF-gegevens georganiseerd in opslag?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kan ik de persistentie voor een eerder gemaakte cache inschakelen?
Ja, Redis-persistentie kan worden geconfigureerd tijdens het maken van cache en bestaande premium-caches.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>Kan ik AOF en RDB persistentie op hetzelfde moment inschakelen?

Nee, u kunt alleen RDB of AOF, maar niet beide op hetzelfde moment inschakelen.

### <a name="which-persistence-model-should-i-choose"></a>Welk model u persistentie moet ik kiezen?

AOF-persistentie slaat elke schrijven naar een logboek dat enige invloed op de doorvoer heeft, vergeleken met de RDB-persistentie Hiermee slaat u back-ups op basis van de geconfigureerde back-interval, met minimale gevolgen voor prestaties. AOF-persistentie kiezen als het primaire doel is om gegevensverlies te minimaliseren en u kunt een afname in de doorvoer voor uw cache verwerken. Kies de RDB-persistentie als u wilt optimale doorvoer voor uw cache behouden, maar toch een mechanisme voor het herstel van gegevens.

* Meer informatie over de [voordelen](https://redis.io/topics/persistence#rdb-advantages) en [nadelen](https://redis.io/topics/persistence#rdb-disadvantages) van RDB persistentie.
* Meer informatie over de [voordelen](https://redis.io/topics/persistence#aof-advantages) en [nadelen](https://redis.io/topics/persistence#aof-disadvantages) van AOF persistentie.

Zie voor meer informatie over de prestaties bij het gebruik van AOF persistentie [AOF wordt persistentie hebben invloed op in de gehele, latentie, of de prestaties van mijn cache?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Wat gebeurt er als ik naar een andere grootte hebt geschaald, en een back-up die is gemaakt voordat de bewerking vergroten/verkleinen is hersteld?

Voor zowel RDB en AOF-persistentie:

* Als u in een groter formaat hebt geschaald, zijn er geen gevolgen.
* Als u op een kleinere grootte hebt geschaald, en u een aangepaste hebt [databases](cache-configure.md#databases) instellen die groter is dan de [databases limiet](cache-configure.md#databases) voor de grootte van uw nieuwe gegevens in deze databases wordt niet hersteld. Zie voor meer informatie, [Is mijn aangepaste databases betrokken instellen tijdens het schalen?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Als u op een kleinere grootte hebt geschaald, en er niet genoeg ruimte is in de kleinere grootte voor alle van de gegevens van de laatste back-up, sleutels wordt onbeschikbaar gemaakt tijdens het herstelproces, meestal met behulp van de [allkeys lru](http://redis.io/topics/lru-cache) verwijderingsbeleid.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>Kan ik de RDB-back-upfrequentie wijzigen nadat ik de cache maken?
Ja, kunt u de back-upfrequentie voor RDB persistentie wijzigen op de **Redis-gegevenspersistentie** blade. Zie voor instructies [configureren van Redis-persistentie](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Waarom als ik een back-upfrequentie RDB van 60 minuten er meer dan 60 minuten tussen back-ups is?
De RDB-persistentie back-upfrequentie interval start niet totdat de vorige back-upproces is voltooid. Als de back-upfrequentie 60 minuten is en wordt een back-upproces 15 minuten te voltooien, start de volgende back-up niet tot 75 minuten na de starttijd van de vorige back-up.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Wat gebeurt er met de oude RDB-back-ups wanneer een nieuwe back-up wordt gemaakt?
Alle RDB persistentie back-ups, behalve voor de meest recente worden automatisch verwijderd. Deze verwijdering mogelijk niet direct plaats, maar oudere back-ups zijn niet voor onbepaalde tijd bewaard.


### <a name="when-should-i-use-a-second-storage-account"></a>Wanneer moet ik een tweede storage-account gebruiken?

Als u denkt dat u hoger dan de verwachte set-bewerkingen op de cache hebt, moet u een tweede opslagaccount voor persistentie AOF.  Instellen van het secundaire opslagaccount zorgt ervoor dat uw cache bandbreedtelimieten opslag niet bereiken.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>AOF persistentie hebben invloed op in de gehele, latentie, of de prestaties van mijn cache gebruikt?

AOF-persistentie is van invloed op de doorvoer van ongeveer 15-20% wanneer de cache lager dan de maximale belasting is (CPU en Server laden beide onder 90%). Er mag niet zijn latentieproblemen wanneer de cache zich binnen deze limieten. Echter, de cache bereiken deze limieten sneller met AOF ingeschakeld.

### <a name="how-can-i-remove-the-second-storage-account"></a>Hoe kan ik de tweede storage-account verwijderen?

U kunt de AOF persistentie secundaire storage-account verwijderen door in te stellen van de tweede storage-account moet hetzelfde zijn als het eerste opslagaccount. Zie voor instructies [AOF configureren persistentie](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Wat is een herschrijven en wat betekent dat mijn cache voor?

Wanneer het bestand AOF groot genoeg is wordt, een herschrijven wordt automatisch in de wachtrij in de cache. Het herschrijven vergroot/verkleint de AOF-bestand met de minimale set bewerkingen die nodig zijn voor het maken van de huidige gegevensset. Tijdens de regeneraties, verwachten dat eerder prestatielimieten bereiken met name wanneer er sprake is van grote gegevenssets. Regeneraties optreden minder vaak als het bestand AOF groter wordt, maar duurt het een aanzienlijke hoeveelheid tijd wanneer het gebeurt.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Wat kan ik verwachten wanneer u een cache met AOF ingeschakeld?

Als het AOF-bestand op het moment van de schaalbaarheid aanzienlijk groot is, klikt u vervolgens verwachten dat de schaalbewerking langer duren dan verwacht, omdat deze worden opnieuw van het bestand laden wordt nadat schalen is voltooid.

Zie voor meer informatie over het omhoog schalen [wat gebeurt er als ik naar een andere grootte hebt geschaald en een back-up is hersteld die is gemaakt voordat de bewerking vergroten/verkleinen?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Hoe wordt mijn AOF-gegevens georganiseerd in opslag?

Gegevens die zijn opgeslagen in AOF-bestanden is onderverdeeld in meerdere pagina-blobs per knooppunt voor betere prestaties van het opslaan van de gegevens naar de opslag. De volgende tabel geeft weer hoeveel pagina-blobs worden gebruikt voor elke prijscategorie:

| Premiumlaag | Blobs |
|--------------|-------|
| P1           | 4 per shard    |
| P2           | 8 per shard    |
| P3           | 16 per shard   |
| P4           | 20 per shard   |

Als clustering is ingeschakeld, heeft elke shard in de cache van een eigen set pagina-blobs, zoals aangegeven in de vorige tabel. Bijvoorbeeld, verdeeld een P2-cache met drie shards over de AOF-bestand 24 pagina-blobs (8 blobs per shard, met 3 shards).

Na een herschrijft bestaan twee sets AOF-bestanden in de opslag. Regeneraties plaatsvinden op de achtergrond en toevoegen aan de eerste set van bestanden, terwijl de set-bewerkingen die worden verzonden naar de cache tijdens het herschrijven toevoegen aan de tweede set. Een back-up wordt tijdelijk opgeslagen tijdens regeneraties in geval van storing, maar wordt onmiddellijk verwijderd nadat een herschrijven is voltooid.


## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer functies voor premium-cache.

* [Inleiding tot de Azure Redis Cache Premium-laag](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
