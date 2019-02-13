---
title: Inleiding tot de Azure-Cache voor Premium-laag Redis | Microsoft Docs
description: Informatie over het maken en beheren van Redis-persistentie, Redis-clustering en VNET-ondersteuning voor uw Premium-laag Azure Cache voor instanties van Redis
services: azure-cache-for-redis
documentationcenter: ''
author: yegu-ms
manager: cfowler
editor: ''
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: 35ecb5156a8b81fa9e10123f7700cf91af91aa0a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115841"
---
# <a name="introduction-to-the-azure-cache-for-redis-premium-tier"></a>Inleiding tot de Azure-Cache voor Premium-laag Redis
Azure Redis-Cache is een gedistribueerde beheerde cache waarmee u zeer schaalbare en snel reagerende toepassingen kunt maken met zeer snelle toegang tot uw gegevens. 

De nieuwe Premium-laag is een Enterprise laag die geschikt is, alle functies van de Standard-laag en meer, zoals betere prestaties, grotere workloads, herstel na noodgevallen, import/export- en verbeterde beveiliging omvat. Lees verder voor meer informatie over de aanvullende functies van de Premium-laag voor cache.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Betere prestaties in vergelijking met de Standard- of Basic-laag
**Betere prestaties via Standard of Basic laag.** Caches in de Premium-laag worden geïmplementeerd op hardware die snellere processoren en biedt betere prestaties in vergelijking tot de Basic of Standard-laag. Premium-laag-Caches hebben een hogere doorvoer en een lagere latentie. 

**Doorvoer voor de Cache met dezelfde grootte is hoger in Premium in vergelijking met de Standard-laag.** Bijvoorbeeld, de doorvoer van een 53 GB P4 (Premium)-cache is 250K aanvragen per seconde in vergelijking met 150 K voor C6 (Standard).

Zie voor meer informatie over de grootte, doorvoer en bandbreedte met premium-caches [Azure voor veelgestelde vragen over Redis-Cache](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis-gegevenspersistentie
De Premium-laag kunt u het behoud van de cachegegevens in een Azure Storage-account. Alle de gegevens worden opgeslagen in een cache basis/standaard alleen in het geheugen. In het geval van onderliggende infrastructuur kunnen er problemen zijn mogelijk gegevensverlies. U wordt aangeraden de functie in de persistentie voor Redis-gegevens in de Premium-laag gebruik te maken om tolerantie tegen verlies van gegevens. Azure Redis-Cache biedt RDB en (binnenkort beschikbaar) AOF opties in [Redis-persistentie](https://redis.io/topics/persistence). 

Zie voor instructies over het configureren van persistentie [persistentie voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis-cluster
Als u wilt caches maken die groter dan 53 GB of shard-gegevens over meerdere Redis-knooppunten wilt, kunt u Redis-clustering die beschikbaar is in de Premium-laag. Elk knooppunt bestaat uit een primaire/replica-cache-paar beheerd door Azure voor hoge beschikbaarheid. 

**Redis-clustering biedt u maximale schaal en doorvoer.** Doorvoer neemt lineair toe naarmate u het aantal shards (knooppunten) in het cluster vergroten. Bijv. Als u een cluster P4 van 10 shards maakt, wordt de beschikbare doorvoer 250 kB is * 10 = 2,5 miljoen aanvragen per seconde. Raadpleeg de [Azure Cache voor veelgestelde vragen over Redis](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte met premium-caches.

Als u wilt beginnen met clustering, Zie [configureren voor een Premium Azure Cache voor Redis clustering](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Verbeterde veiligheid en isolatie
Gemaakt in de laag Basic of Standard-caches zijn toegankelijk via het openbare internet. Toegang tot de Cache wordt beperkt op basis van de toegangssleutel. Met de Premium-laag kunt u verder ervoor zorgen dat alleen clients binnen een opgegeven netwerk toegang heeft tot de Cache. U kunt Azure-Cache implementeren voor Redis in een [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). U kunt alle functies van VNet, zoals subnetten, toegangscontrolebeleid en andere functies, gebruiken om de toegang tot Redis verder te beperken.

Zie voor meer informatie, [het configureren van Virtual Network-ondersteuning voor een Premium Azure Cache voor Redis](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/Export
Import/Export wordt een Azure-Cache voor beheerbewerking van het Redis-gegevens waarmee u gegevens importeren in Azure voor Redis-Cache of exporteren van gegevens uit de Cache voor Redis door te importeren en exporteren van een Azure-Cache voor de momentopname Redis-Database (RDB) uit de cache van een premium Azure een pagina-blobs in een Azure Storage-Account. Hiermee kunt u om te migreren tussen verschillende Azure-Cache voor instanties van Redis of vullen van de cache met gegevens voor het gebruik.

Invoer kan worden gebruikt om Redis compatibel RDB-bestand(en) vanaf een willekeurige Redis-server die wordt uitgevoerd in een cloud of de omgeving, met inbegrip van Redis die worden uitgevoerd op Linux, Windows of elke andere cloudprovider, zoals Amazon Web Services en andere. Importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingevulde gegevens. Tijdens het importproces, Azure voor Redis-Cache laadt de RDB-bestanden van Azure storage in het geheugen en voegt vervolgens de sleutels in de cache.

Export kunt u de gegevens die zijn opgeslagen in Azure Cache voor Redis-Redis compatibel RDB-bestand(en) exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van een Azure-Cache voor Redis-exemplaar naar een andere of naar een andere Redis-server. Tijdens het exportproces een tijdelijke bestand wordt gemaakt op de virtuele machine die als host fungeert voor de Azure-Cache voor Redis-server-exemplaar en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status geslaagd of mislukt, wordt het tijdelijke bestand verwijderd.

Zie voor meer informatie, [gegevens importeren en exporteren van gegevens uit de Cache van Azure voor Redis](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Opnieuw opstarten
De premium-laag kunt u een of meer knooppunten van uw cache op aanvraag opnieuw. Hiermee kunt u voor het testen van uw toepassing voor tolerantie bij een storing. U kunt de volgende knooppunten opnieuw opstarten.

* Hoofdknooppunt van uw cache
* Ondergeschikte server-knooppunt van uw cache
* Hoofd- en de ondergeschikte knooppunten van uw cache
* Wanneer u een premium-cache met clustering, kunt u het model, slave of beide knooppunten voor afzonderlijke shards in de cache opnieuw

Zie voor meer informatie, [opnieuw opstarten](cache-administration.md#reboot) en [Veelgestelde vragen over het opnieuw opstarten](cache-administration.md#reboot-faq).

>[!NOTE]
>Opnieuw opstarten functionaliteit is nu ingeschakeld voor alle Azure-Cache voor Redis-lagen.
>
>

## <a name="schedule-updates"></a>Updates plannen
De functie voor geplande updates kunt u om een onderhoudsvenster voor uw cache toe te wijzen. Wanneer het onderhoudsvenster is opgegeven, worden eventuele updates van Redis-server gemaakt tijdens dit venster. Als u wilt aanwijzen van een onderhoudsvenster, selecteert u de gewenste dagen en geef het onderhoud starten venster uur per dag. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

Zie voor meer informatie, [updates plannen](cache-administration.md#schedule-updates) en [updates plannen Veelgestelde vragen over](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Alleen Redis-server updates worden uitgevoerd tijdens het geplande onderhoudsvenster. Het onderhoudsvenster is niet van toepassing op Azure-updates of updates van het besturingssysteem van de virtuele machine.
> 
> 

## <a name="geo-replication"></a>Geo-replicatie

**Geo-replicatie** biedt een mechanisme voor het koppelen van twee Premium-laag Azure Cache voor instanties van Redis. Een cache is ingesteld als de primaire gekoppelde cache, en de andere als de gekoppelde secundaire cache. De gekoppelde secundaire cache wordt alleen-lezen en gegevens geschreven naar de primaire cache gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache Azure-regio's.

Zie voor meer informatie, [Geo-replicatie configureren voor Azure Cache voor Redis](cache-how-to-geo-replication.md).


## <a name="to-scale-to-the-premium-tier"></a>Schalen om aan de premium-laag
Als u wilt schalen naar de premium-laag, kiest u gewoon een van de premium-lagen in de **prijscategorie wijzigen** blade. U kunt uw cache naar de premiumlaag met behulp van PowerShell en CLI ook schalen. Zie voor stapsgewijze instructies [hoe Scale Azure Cache voor Redis](cache-how-to-scale.md) en [over het automatiseren van een bewerking voor vergroten/verkleinen](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Volgende stappen
Een cache maken en de nieuwe premium-functies verkennen.

* [Persistentie voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-persistence.md)
* [Het configureren van Virtual Network-ondersteuning voor een Premium Azure Cache voor Redis](cache-how-to-premium-vnet.md)
* [Clustering voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-clustering.md)
* [Het importeren van gegevens in en exporteren van gegevens van Azure voor Redis-Cache](cache-how-to-import-export-data.md)
* [Azure Cache beheren voor Redis](cache-administration.md)

