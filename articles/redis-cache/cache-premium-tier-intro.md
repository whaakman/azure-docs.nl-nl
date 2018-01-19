---
title: Inleiding tot de Azure Redis-Cache Premium-laag | Microsoft Docs
description: Informatie over het maken en beheren van Redis-persistentie, Redis clustering en VNET-ondersteuning voor uw Azure Redis-Cache-exemplaren van Premium-laag
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 38a43756678a3628040b1b995966eff6dd9fb363
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Inleiding Premium-laag Azure Redis-Cache
Azure Redis-Cache is een gedistribueerd, beheerde cache waarmee u uiterst schaalbare en responsief toepassingen bouwen door middel van zeer snelle toegang tot uw gegevens. 

De nieuwe Premium-laag is een Enterprise gereed laag, waaronder alle functies van de Standard-laag en meer, zoals betere prestaties, grotere werkbelastingen, herstel na noodgevallen, importeren/exporteren en verbeterde beveiliging. Blijven lezen voor meer informatie over de aanvullende functies van de laag Premium-cache.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Betere prestaties in vergelijking met de categorie Standard of Basic
**Betere prestaties via standaard of Basic laag.** Caches in de laag Premium zijn geïmplementeerd op de snellere processors heeft en hogere prestaties in vergelijking tot het basis- of Standard-laag hardware. Premium-laag Caches hebben hogere doorvoer en lagere latenties. 

**Doorvoer voor de dezelfde grootte Cache is hoger in Premium in vergelijking met de Standard-laag.** Bijvoorbeeld, de doorvoer van een 53 GB P4 (Premium)-cache is 250K aanvragen per seconde in vergelijking met 150 K voor C6 (standaard).

Zie voor meer informatie over de grootte, doorvoer en bandbreedte van premiumcaches [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis-gegevenspersistentie
De laag Premium kunt u de cache-gegevens in een Azure Storage-account. Alle gegevens wordt opgeslagen in een cache Basic/standaard alleen in het geheugen. In geval van een onderliggende infrastructuur kunnen er problemen zijn mogelijk gegevensverlies. U wordt aangeraden de tolerantie tegen verlies van gegevens verhoogd met de functie persistentie van Redis-gegevens in de laag Premium. Azure Redis-Cache biedt RDB en AOF (binnenkort) opties in [Redis-persistentie](http://redis.io/topics/persistence). 

Zie [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md) (Persistentie voor een Premium Azure Redis Cache configureren) voor instructies over het configureren van persistentie.

## <a name="redis-cluster"></a>Redis-cluster
Als u caches maken die groter zijn dan 53 GB of wilt gegevens verdelen over meerdere Redis-knooppunten wilt, kunt u Redis clustering die beschikbaar is in de laag Premium. Elk knooppunt bestaat uit een combinatie van primair/replica cache beheerd door Azure voor hoge beschikbaarheid. 

**Redis clustering biedt u maximale schaal en de doorvoer.** Doorvoer duurt lineair omdat u het aantal shards (knooppunten) in het cluster vergroten. Bv. Als u een cluster P4 van 10 shards maakt, wordt de beschikbare doorvoer 250 kB is * 10 = 2,5 miljoen aanvragen per seconde. Zie de [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte van premiumcaches.

Als u wilt beginnen met clustering, Zie [clustering voor een Premium Azure Redis-Cache configureren](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Verbeterde veiligheid en isolatie
Caches gemaakt in de categorie Basic of standaard zijn toegankelijk op het openbare internet. Toegang tot de Cache is beperkt op basis van de toegangssleutel. Aan de laag Premium kunt u verder ervoor zorgen dat alleen clients binnen een opgegeven netwerk toegang de Cache tot. U kunt implementeren Redis-Cache in een [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). U kunt alle functies van VNet, zoals subnetten, toegangscontrolebeleid en andere functies, gebruiken om de toegang tot Redis verder te beperken.

Zie [How to configure Virtual Network support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md) (Virtual Network-ondersteuning voor een Premium Azure Redis Cache configureren) voor meer informatie.

## <a name="importexport"></a>Import/Export
Import/Export is een Azure Redis-Cache data management bewerking waarmee u gegevens in Azure Redis-Cache importeren of exporteren van gegevens in Azure Redis-Cache door importeren en exporteren van een momentopname van een Redis-Cache Database (RDB) uit de cache premium naar een paginablob in Azure Storage-Account. Hiermee kunt u om te migreren tussen verschillende exemplaren van Azure Redis-Cache of het vullen van de cache met gegevens voor het gebruik.

Invoer kan worden gebruikt om Redis compatibel RDB bestanden vanaf een willekeurige Redis-server uitgevoerd in een cloud of de omgeving, inclusief Redis uitgevoerd op Linux, Windows of elke cloudprovider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier voor het maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure Redis-Cache laadt de bestanden RDB uit Azure storage in het geheugen en voegt vervolgens de sleutels in de cache.

Exporteren kunt u de gegevens die zijn opgeslagen in Azure Redis-Cache voor Redis compatibel RDB bestand(en) exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van een Azure Redis-Cache-exemplaar naar een andere of een andere Redis-server. Tijdens het exportproces een tijdelijk bestand wordt gemaakt op de virtuele machine die als host fungeert voor de server-exemplaar van Azure Redis-Cache en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

Zie voor meer informatie [het importeren van gegevens in en gegevens exporteren uit Azure Redis-Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Opnieuw opstarten
De laag premium kunt u een of meer knooppunten van de cache op aanvraag opnieuw. Hiermee kunt u de toepassing voor tolerantie bij een storing te testen. U kunt de volgende knooppunten opnieuw opstarten.

* Hoofdknooppunt van de cache
* Slave knooppunt van de cache
* Hoofd- en slave knooppunten van de cache
* Als u een premium-cache met clustering, kunt u het model, slave of beide knooppunten voor afzonderlijke shards in de cache opstarten

Zie voor meer informatie [opnieuw opstarten](cache-administration.md#reboot) en [Veelgestelde vragen over het opnieuw opstarten](cache-administration.md#reboot-faq).

>[!NOTE]
>Opnieuw opstarten functionaliteit is nu ingeschakeld voor alle lagen van de Azure Redis-Cache.
>
>

## <a name="schedule-updates"></a>Updates plannen
De functie geplande updates kunt u een onderhoudsvenster voor uw cache aanwijzen. Wanneer het onderhoudsvenster wordt opgegeven, wordt een Redis-server bijgewerkt tijdens dit venster. Als u wilt aanwijzen van een onderhoudsvenster, selecteer de gewenste dagen en geef het onderhoud venster Beginuur voor elke dag. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

Zie voor meer informatie [updates plannen](cache-administration.md#schedule-updates) en [updates plannen Veelgestelde vragen over](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Alleen Redis-server updates zijn aangebracht tijdens het geplande onderhoudsvenster. Het onderhoudsvenster is niet van toepassing op Azure-updates of updates voor het besturingssysteem van de VM.
> 
> 

## <a name="geo-replication"></a>Geo-replicatie

**Geo-replicatie** biedt een mechanisme voor het koppelen van twee exemplaren van Premium-laag Azure Redis-Cache. Een cache is aangewezen als het primaire gekoppelde cachegeheugen en de andere als de secundaire gekoppelde cache. De secundaire gekoppelde cache wordt alleen-lezen en gegevens naar de primaire cache geschreven worden gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache in Azure-regio's.

Zie voor meer informatie [Geo-replicatie configureren voor Azure Redis-Cache](cache-how-to-geo-replication.md).


## <a name="to-scale-to-the-premium-tier"></a>Schalen aan de laag premium
Als u wilt schalen aan de laag premium, kies een van de lagen premium in de **wijziging prijscategorie** blade. U kunt ook uw cache naar de premium-laag met PowerShell en CLI schalen. Zie voor stapsgewijze instructies [How to Scale Azure Redis-Cache](cache-how-to-scale.md) en [een vergroten/verkleinen bewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Volgende stappen
Een cache maken en de nieuwe functies voor premium-laag.

* [Persistentie configureren voor een Premium Azure Redis-cache](cache-how-to-premium-persistence.md)
* [Virtual Network-ondersteuning configureren voor een Premium Azure Redis-cache](cache-how-to-premium-vnet.md)
* [Clustering configureren voor een Premium Azure Redis-cache](cache-how-to-premium-clustering.md)
* [Hoe gegevens importeren en exporteren van gegevens van Azure Redis-Cache](cache-how-to-import-export-data.md)
* [Het beheren van Azure Redis-Cache](cache-administration.md)

