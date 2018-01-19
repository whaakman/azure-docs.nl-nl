---
title: Azure Redis-Cache controleren | Microsoft Docs
description: Informatie over het bewaken van de status en prestaties van uw Azure Redis-Cache-exemplaren
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: 3a68435866e6fb5bf0210144e53918c35b416449
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-monitor-azure-redis-cache"></a>Azure Redis-cache bewaken
Maakt gebruik van Azure Redis-Cache [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) om verschillende opties voor het bewaken van uw cache-exemplaren te bieden. U kunt metrische gegevens weergeven, vastmaken grafieken van de metrische gegevens aan het Startboard, aanpassen van het bereik datum en tijd van de bewaking van grafieken, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Deze hulpprogramma's kunnen u de status van uw Azure Redis-Cache-exemplaren en uw cache in toepassingen te beheren.

Metrische gegevens voor Azure Redis-Cache-exemplaren worden verzameld met behulp van de Redis [INFO](http://redis.io/commands/info) opdracht ongeveer twee keer per minuut en automatisch opgeslagen voor 30 dagen (Zie [cache metrische gegevens exporteren](#export-cache-metrics) voor het configureren van een andere bewaarbeleid) zodat ze kunnen worden weergegeven in de grafieken van de metrische gegevens en geëvalueerd door regels voor waarschuwingen. Zie voor meer informatie over de verschillende INFO waarden gebruikt voor elke metriek cache [beschikbare metrische gegevens en de rapportage van intervallen](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

Om weer te geven de metrische gegevens voor cache, [Bladeren](cache-configure.md#configure-redis-cache-settings) naar uw cache-exemplaar in de [Azure-portal](https://portal.azure.com).  Azure Redis-Cache geeft u een aantal ingebouwde grafieken van de **overzicht** blade en de **Redis metrische gegevens** blade. Elke grafiek kan worden aangepast door het toevoegen of verwijderen van metrische gegevens en het wijzigen van het interval voor rapportage.

![Metrische Redis-gegevens](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Diagrammen van vooraf geconfigureerde metrische gegevens weergeven

De **overzicht** blade bevat de volgende vooraf geconfigureerde bewaking grafieken.

* [Bewaking van grafieken](#monitoring-charts)
* [Gebruik grafieken](#usage-charts)

### <a name="monitoring-charts"></a>Bewaking van grafieken
De **bewaking** sectie het **overzicht** blade bevat **treffers en Cachemissers**, **opgehaald en ingesteld**, **verbindingen**, en **totale opdrachten** grafieken.

![Bewaking van grafieken](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Gebruik grafieken
De **gebruik** sectie het **overzicht** blade bevat **serverbelasting Redis**, **geheugengebruik**, **netwerkbandbreedte**, en **CPU-gebruik** grafieken en geeft ook de **prijscategorie** voor het cache-exemplaar.

![Gebruik grafieken](./media/cache-how-to-monitor/redis-cache-usage-part.png)

De **prijscategorie** geeft de cache-prijzen servicetier en kan worden gebruikt voor het [scale](cache-how-to-scale.md) de cache op een andere prijscategorie.

## <a name="view-metrics-with-azure-monitor"></a>Metrische gegevens weergeven met Azure-monitor
Redis metrische gegevens weergeven en aangepaste grafieken met behulp van Azure-Monitor maken, klikt u op **metrische gegevens** van de **Resource menu**, en de grafiek met de gewenste metrische gegevens reporting interval en grafiektype aanpassen.

![Metrische Redis-gegevens](./media/cache-how-to-monitor/redis-cache-monitor.png)

Zie voor meer informatie over het werken met metrische gegevens met behulp van Azure Monitor [overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Cache metrische gegevens exporteren
Cache metrische gegevens in de Azure-Monitor zijn standaard [30 dagen bewaard](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) en vervolgens verwijderd. Om te blijven behouden de metrische gegevens van uw cache langer dan 30 dagen, kunt u [aanwijzen van een opslagaccount](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) en geef een **bewaartermijn (dagen)** beleid voor de metrische gegevens van uw cache. 

Een opslagaccount voor de metrische gegevens van uw cache configureren:

1. Klik op **Diagnostics** van de **Resource menu** in de **Redis-Cache** blade.
2. Klik op **op**.
3. Controleer **archiveren naar een opslagaccount**.
4. Selecteer het opslagaccount waarin de metrische gegevens van cache opgeslagen.
5. Controleer de **1 minuut** selectievakje in en geef een **bewaartermijn (dagen)** beleid. Als u niet wilt toepassen bewaarbeleid en gegevens blijven behouden, stelt u **bewaartermijn (dagen)** naar **0**.
6. Klik op **Opslaan**.

![Diagnostische gegevens redis](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>Naast het archiveren van de metrische gegevens van uw cache naar de opslag, kunt u ook [ze naar een Event hub te streamen of ze verzenden naar logboekanalyse](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

Voor toegang tot uw metrische gegevens, kunt u deze bekijken in de Azure portal, zoals eerder in dit artikel wordt beschreven en u kunt ze ook openen met behulp van de [REST API voor de metrische gegevens van de Monitor van de Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

> [!NOTE]
> Als u opslagaccounts wijzigt, blijven de gegevens in de eerder geconfigureerde storage-account worden gedownload, maar deze niet wordt weergegeven in de Azure-portal.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Beschikbare metrische gegevens en de rapportage van intervallen
Cache metrische gegevens worden gerapporteerd met behulp van verschillende reporting intervallen, met inbegrip van **voorbij uur**, **vandaag**, **afgelopen week**, en **aangepaste**. De **metriek** blade voor elke grafiek metrische gegevens geeft de gemiddelde, de minimale en maximale waarden voor elke waarde in de grafiek en sommige metrische gegevens weergeven in totaal voor het interval voor rapportage. 

Elke metriek bevat twee versies. Een metriek meet prestaties voor het volledige cachegeheugen en caches die gebruikmaken van [clustering](cache-how-to-premium-clustering.md), een tweede versie van de metrische gegevens met `(Shard 0-9)` in de prestaties van de metingen naam voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 4 shards `Cache Hits` is de totale hoeveelheid treffers voor het volledige cachegeheugen en `Cache Hits (Shard 3)` is alleen de treffers voor die shard van de cache.

> [!NOTE]
> Zelfs wanneer de cache niet actief is zonder actieve client verbonden toepassingen, ziet u mogelijk een aantal cache-activiteit, zoals verbonden clients, geheugengebruik en bewerkingen die worden uitgevoerd. Deze activiteit is normaal tijdens de bewerking van een Azure Redis-Cache-exemplaar.
> 
> 

| Gegevens | Beschrijving |
| --- | --- |
| Cachetreffers |Het aantal treffers sleutel tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `keyspace_hits` van de Redis [INFO](http://redis.io/commands/info) opdracht. |
| Cachemissers |Het aantal mislukte sleutel zoekacties tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `keyspace_misses` van de opdracht Redis INFO. Cachemissers betekenen niet noodzakelijkerwijs dat er is een probleem met de cache. Wanneer u het cache-aside ' programming patroon, een toepassing ziet er bijvoorbeeld eerste in de cache voor een item. Als het item is er geen (Cachemisser), wordt het item uit de database opgehaald en toegevoegd aan de cache voor de volgende keer. Cachemissers zijn normaal gedrag voor het programmeren van cache-aside '-patroon. Als het aantal missers in cache hoger is dan verwacht, controleert u de toepassingslogica die gevuld en leest uit de cache. Als items zijn wordt verwijderd uit de cache vanwege geheugendruk en kan er een aantal missers in cache, maar een betere metriek om te controleren op geheugendruk zou zijn `Used Memory` of `Evicted Keys`. |
| Gekoppelde clients |Het aantal clientverbindingen met de cache tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `connected_clients` van de opdracht Redis INFO. Eenmaal de [verbindingslimiet](cache-configure.md#default-redis-server-configuration) is bereikt latere verbindingspogingen met de cache wordt niet uitgevoerd. Houd er rekening mee dat zelfs als er geen actieve clienttoepassing, er nog steeds mogelijk enkele exemplaren van de verbonden clients als gevolg van interne processen en verbindingen. |
| Verwijderde sleutels |Het aantal items uit de cache verwijderd tijdens het opgegeven reporting interval vanwege de `maxmemory` limiet. Dit wordt toegewezen aan `evicted_keys` van de opdracht Redis INFO. |
| Verlopen sleutels |Het aantal items die tijdens het opgegeven interval reporting uit de cache is verlopen. Deze waarde wordt toegewezen aan `expired_keys` van de opdracht Redis INFO. |
| Totaal aantal sleutels  | Het maximale aantal sleutels in de cache in de afgelopen periode reporting. Dit wordt toegewezen aan `keyspace` van de opdracht Redis INFO. Totale aantal sleutels retourneert vanwege een beperking van het onderliggende systeem metrische gegevens voor caches met clustering is ingeschakeld, het maximum aantal sleutels van de shard waarop het maximum aantal sleutels tijdens het rapportage-interval.  |
| Opgehaalde items |Het aantal get-bewerkingen tijdens het opgegeven interval reporting uit de cache. Deze waarde is de som van de volgende waarden van de gegevens van de Redis-opdracht Alles: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, en `cmdstat_getrange`, en is gelijk aan de som van treffers in cache en missers tijdens het rapportage-interval. |
| Redis-serververmogen |Het percentage van de cycli waarin de Redis-server is bezig de verwerking en niet wachten op niet-actieve voor berichten. Als deze teller wordt de Redis-server een prestaties maximum heeft bereikt en kan niet worden verwerkt door de CPU 100 moet u een sneller werken. Als u hoge belasting van Redis-Server ziet ziet u uitzonderingen voor time-outs in de client. In dit geval moet u omhoog schalen of uw gegevens in meerdere caches partitioneren. |
| Sets |Het aantal set-bewerkingen tijdens het opgegeven interval voor rapportage aan de cache. Deze waarde is de som van de volgende waarden van de gegevens van de Redis-opdracht Alles: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , en `cmdstat_setnx`. |
| Totaal aantal bewerkingen |Het totale aantal opdrachten verwerkt door de cacheserver tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `total_commands_processed` van de opdracht Redis INFO. Houd er rekening mee dat wanneer Azure Redis-Cache wordt gebruikt voor pub subitems er worden geen metrische gegevens voor `Cache Hits`, `Cache Misses`, `Gets`, of `Sets`, maar er zijn `Total Operations` metrische gegevens die overeenkomen met het gebruik van de cache voor pub subitems bewerkingen. |
| Gebruikt geheugen |De hoeveelheid cachegeheugen gebruikt voor de sleutel/waarde-paren in de cache in MB tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `used_memory` van de opdracht Redis INFO. Dit omvat geen metagegevens of fragmentatie. |
| Gebruikt geheugen RSS |De hoeveelheid geheugen van de cache in MB gebruikt tijdens het opgegeven reporting interval, met inbegrip van fragmentatie en metagegevens. Deze waarde wordt toegewezen aan `used_memory_rss` van de opdracht Redis INFO. |
| CPU |Het CPU-gebruik van de Azure Redis-Cache-server als een percentage tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan het besturingssysteem `\Processor(_Total)\% Processor Time` prestatiemeteritem. |
| Gelezen uit cache |De hoeveelheid gegevens gelezen uit de cache in MB per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde is afgeleid van de netwerkkaarten die ondersteuning bieden voor de virtuele machine die als host fungeert voor de cache en is geen specifieke Redis. **Deze waarde komt overeen met de netwerkbandbreedte gebruikt door deze cache. Als u wilt voor het instellen van waarschuwingen voor bandbreedtelimieten van server side netwerk, moet u deze maken gebruik van deze `Cache Read` teller. Zie [deze tabel](cache-faq.md#cache-performance) voor de waargenomen-bandbreedtelimieten voor verschillende cache lagen en grootten prijzen.** |
| Cache schrijven |De hoeveelheid gegevens geschreven naar de cache in MB per seconde (MB/s) tijdens het opgegeven interval rapportage. Deze waarde is afgeleid van de netwerkkaarten die ondersteuning bieden voor de virtuele machine die als host fungeert voor de cache en is geen specifieke Redis. Deze waarde komt overeen met de netwerkbandbreedte van gegevens die worden verzonden naar de cache van de client. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Waarschuwingen
U kunt configureren voor het ontvangen van meldingen op basis van de logboeken van metrische gegevens en de activiteit. Azure Monitor kunt u de volgende handelingen uit als er wordt een waarschuwing configureren:

* Een e-mailmelding verzenden
* een webhook aanroepen
* Een Azure Logic App aanroepen

Voor het configureren van regels voor waarschuwingen voor uw cache, klikt u op **waarschuwing regels** van de **Resource menu**.

![Bewaking](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Zie voor meer informatie over het configureren en het gebruik van waarschuwingen [overzicht van waarschuwingen](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Activiteitenlogboeken
Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure Redis-Cache-exemplaren. Het was voorheen bekend als 'controlelogboeken' of 'operationele logs'. Met activiteitenlogboeken, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op uw Azure Redis-Cache-exemplaren. 

> [!NOTE]
> Activiteitenlogboeken bevatten geen leesbewerkingen (GET).
>
>

Als u wilt weergeven van activiteitenlogboeken voor uw cache **activiteitenlogboeken** van de **Resource menu**.

Zie voor meer informatie over activiteitenlogboeken [overzicht van de Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).











