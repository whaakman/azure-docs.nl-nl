---
title: Azure Cache controleren voor Redis | Microsoft Docs
description: Informatie over het bewaken van de status en prestaties van uw Azure-Cache voor instanties van Redis
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: e6292c97d3e7bbbe74477188586257b4fbf91218
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582708"
---
# <a name="how-to-monitor-azure-cache-for-redis"></a>Azure Cache controleren voor Redis
Azure maakt gebruik van Redis-Cache [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) verschillende opties voor het bewaken van uw cache-exemplaren. U kunt metrische gegevens weergeven, grafieken met metrische gegevens aan het Startboard vastmaken, aanpassen van het bereik van datum en tijd van de bewaking van grafieken, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Deze hulpprogramma's kunnen u de status van uw Azure-Cache voor instanties van Redis en hulp bij het beheren van uw cache in toepassingen worden gecontroleerd.

Metrische gegevens voor Azure Cache voor instanties van Redis worden verzameld met behulp van de Redis [INFO](http://redis.io/commands/info) opdracht ongeveer twee keer per minuut en automatisch opgeslagen voor 30 dagen (Zie [cache metrische gegevens exporteren](#export-cache-metrics) het configureren van een verschillende bewaarbeleid), zodat ze kunnen worden weergegeven in de grafieken met metrische gegevens en geëvalueerd door regels voor waarschuwingen. Zie voor meer informatie over de verschillende informatie-waarden gebruikt voor alle gegevens van de cache [beschikbare metrische gegevens en rapportage van intervallen](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

Om weer te geven van de cache metrische gegevens, [Bladeren](cache-configure.md#configure-azure-cache-for-redis-settings) naar uw cache-exemplaar in de [Azure-portal](https://portal.azure.com).  Azure Redis-Cache bevat enkele ingebouwde grafieken voor de **overzicht** blade en de **metrische Redis** blade. Elke grafiek kan worden aangepast door het toevoegen of verwijderen van metrische gegevens en het wijzigen van het interval voor rapportage.

![Metrische Redis-gegevens](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Grafieken met metrische gegevens weergeven die vooraf is geconfigureerd

De **overzicht** blade bevat de volgende vooraf geconfigureerde controlegrafieken.

* [Bewakingsgrafieken](#monitoring-charts)
* [Gebruik grafieken](#usage-charts)

### <a name="monitoring-charts"></a>Bewakingsgrafieken
De **bewaking** sectie de **overzicht** blade bevat **treffers en missers**, **opgehaald en ingesteld**, **verbindingen**, en **totaal aantal opdrachten** grafieken.

![Bewakingsgrafieken](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Gebruik grafieken
De **gebruik** sectie de **overzicht** blade bevat **Redis-serverbelasting**, **geheugengebruik**, **netwerkbandbreedte**, en **CPU-gebruik** grafieken en geeft ook de **prijscategorie** voor het cache-exemplaar.

![Gebruik grafieken](./media/cache-how-to-monitor/redis-cache-usage-part.png)

De **prijscategorie** geeft de cache prijscategorie, en kunnen worden gebruikt om [schaal](cache-how-to-scale.md) de cache naar een andere prijscategorie.

## <a name="view-metrics-with-azure-monitor"></a>Metrische gegevens weergeven met Azure monitor
Redis metrische gegevens weergeven en maken van aangepaste grafieken met behulp van Azure Monitor, klikt u op **metrische gegevens** uit de **resourcemenu**, en de grafiek met de gewenste metrische gegevens, interval en grafiektype rapportage aanpassen.

![Metrische Redis-gegevens](./media/cache-how-to-monitor/redis-cache-monitor.png)

Zie voor meer informatie over het werken met metrische gegevens met behulp van Azure Monitor [overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Metrische cache-gegevens exporteren
Metrische cache-gegevens in Azure Monitor zijn standaard [30 dagen bewaard](../azure-monitor/platform/data-collection.md#metrics) en vervolgens verwijderd. Als u wilt uw cache metrische gegevens langer dan 30 dagen behouden, kunt u [aanwijzen van een storage-account](../azure-monitor/platform/archive-diagnostic-logs.md) en geef een **bewaarperiode (dagen)** beleid voor uw cache metrische gegevens. 

Het configureren van een opslagaccount voor uw cache metrische gegevens:

1. Klik op **Diagnostics** uit de **menu Resource** in de **Azure Cache voor Redis** blade.
2. Klik op **op**.
3. Controleer **archiveren naar een opslagaccount**.
4. Selecteer het opslagaccount waarin de metrische gegevens de cache opslaan.
5. Controleer de **1 minuut** selectievakje in en geef een **bewaarperiode (dagen)** beleid. Als u niet wilt toepassen van een beleid voor het bewaren en gegevens oneindig wilt bewaren, stelt u **bewaarperiode (dagen)** naar **0**.
6. Klik op **Opslaan**.

![Diagnostische gegevens van redis](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>Naast het archiveren van uw cache metrische gegevens naar de opslag, kunt u ook [ze naar een Event hub streamen of ze verzenden naar Log Analytics](../azure-monitor/platform/rest-api-walkthrough.md#retrieve-metric-values).
>
>

Voor toegang tot uw metrische gegevens, kunt u deze bekijken in Azure portal zoals eerder in dit artikel wordt beschreven en u kunt ze ook openen met behulp van de [REST-API van Azure Monitor Metrics](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

> [!NOTE]
> Als u storage-accounts wijzigen, de gegevens in de eerder geconfigureerde opslagaccount blijft beschikbaar voor downloaden, maar deze niet wordt weergegeven in de Azure-portal.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Beschikbare metrische gegevens en rapportage van intervallen
Metrische cache-gegevens worden gerapporteerd met behulp van verschillende reporting intervallen, met inbegrip van **afgelopen uur**, **vandaag**, **afgelopen week**, en **aangepaste**. De **Metric** blade voor elke grafiek met metrische gegevens geeft de gemiddelde, minimale en maximale waarden voor elke metrische gegevens in de grafiek en een totaal sommige metrische gegevens weergeven voor het interval voor rapportage. 

Alle gegevens bevat twee versies. Prestaties voor het volledige cachegeheugen en caches die gebruikmaken van een waarde worden gemeten [clustering](cache-how-to-premium-clustering.md), een tweede versie van de metrische gegevens met `(Shard 0-9)` in de prestaties van de naam van metingen voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 4 shards `Cache Hits` is de totale hoeveelheid treffers voor het volledige cachegeheugen en `Cache Hits (Shard 3)` wordt alleen de treffers voor shard van de cache.

> [!NOTE]
> Zelfs wanneer de cache niet actief is zonder actieve client verbonden toepassingen, ziet u een cache-activiteit, zoals verbonden clients, geheugengebruik en bewerkingen die worden uitgevoerd. Deze activiteit is normaal tijdens de bewerking van een Azure-Cache voor Redis-exemplaar.
> 
> 

| Gegevens | Description |
| --- | --- |
| Cachetreffers |Het aantal geslaagde sleutel zoekacties tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `keyspace_hits` uit de Redis [INFO](http://redis.io/commands/info) opdracht. |
| Latentie van de cache (Preview) | De latentie van de cache berekend op basis van uit de latentie tussen knooppunten van de cache. Met deze metriek wordt gemeten in microseconden en drie dimensies heeft: "Gem.", "Min" en "Max" Dit is de gemiddelde, minimale en maximale latentie van de cache respectievelijk tijdens het opgegeven interval voor rapportage. |
| Cachemissers |Het aantal mislukte sleutel zoekacties tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `keyspace_misses` van de Redis-INFO-opdracht. Missers in cache betekenen niet noodzakelijkerwijs dat er is een probleem met de cache. Wanneer u het patroon cache-aside programmeren, een toepassing, ziet er bijvoorbeeld eerste in de cache voor een item. Als het item is er geen (Cachemisser), wordt het item uit de database opgehaald en toegevoegd aan de cache voor later. Missers in cache zijn normaal gedrag voor het programmeren cache-aside-patroon. Als het aantal missers in cache hoger is dan verwacht, controleert u de toepassingslogica waarmee vult en leest uit de cache. Als items zijn verwijderd uit de cache vanwege geheugendruk en er enkele missers in cache zijn mogelijk, maar een betere metrische waarde om te controleren op geheugendruk zou `Used Memory` of `Evicted Keys`. |
| Gelezen uit cache |De hoeveelheid gegevens gelezen uit de cache in MB per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde wordt afgeleid van de lijst met netwerkadapters die ondersteuning bieden voor de virtuele machine die als host fungeert voor de cache en is geen specifieke Redis. **Deze waarde komt overeen met de netwerkbandbreedte die wordt gebruikt door deze cache. Als u wilt voor het instellen van waarschuwingen voor bandbreedtelimieten van server-side '-netwerk, maakt u het met dit `Cache Read` teller. Zie [deze tabel](cache-faq.md#cache-performance) voor de waargenomen bandbreedtelimieten voor verschillende cache prijzen van lagen en -grootten.** |
| Cache schrijven |De hoeveelheid gegevens geschreven naar de cache in MB per seconde (MB/s) tijdens de opgegeven reporting interval. Deze waarde wordt afgeleid van de lijst met netwerkadapters die ondersteuning bieden voor de virtuele machine die als host fungeert voor de cache en is geen specifieke Redis. Deze waarde komt overeen met de bandbreedte van het netwerk van de gegevens worden verzonden naar de cache van de client. |
| Verbonden clients |Het aantal clientverbindingen met de cache tijdens het opgegeven interval voor rapportage. Dit wordt toegewezen aan `connected_clients` van de Redis-INFO-opdracht. Zodra de [verbindingslimiet](cache-configure.md#default-redis-server-configuration) is bereikt latere verbindingspogingen met de cache zal mislukken. Houd er rekening mee dat, zelfs als er geen actieve client-toepassingen zijn, er nog steeds mogelijk een paar exemplaren van verbonden clients vanwege interne processen en verbindingen. |
| CPU |Het CPU-gebruik van de Azure-Cache voor Redis-server als een percentage tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan het besturingssysteem `\Processor(_Total)\% Processor Time` prestatiemeteritem. |
| Fouten | Specifieke fouten en prestatieproblemen die de cache tijdens een opgegeven interval voor rapportage ondervinden. Met deze metriek acht dimensies die verschillende fouttypen heeft, maar kan in de toekomst meer hebt toegevoegd. De fouttypen weergegeven nu zijn als volgt: <br/><ul><li>**Failover** – wanneer wordt overgenomen door een cache (ondergeschikte server verhogen naar hoofdniveau)</li><li>**Crash** – wanneer de cache onverwacht in elk van de knooppunten vastloopt</li><li>**Dataloss** : wanneer er dataloss op de cache</li><li>**UnresponsiveClients** – wanneer de clients niet van gegevens van de server snel genoeg lezen zijn</li><li>**AOF** : wanneer er een probleem met betrekking tot AOF persistentie</li><li>**De RDB** : wanneer er een probleem met betrekking tot de RDB-persistentie</li><li>**Importeren** : wanneer er een probleem met betrekking tot RDB importeren</li><li>**Exporteren** : wanneer er een probleem met betrekking tot RDB exporteren</li></ul> |
| Verwijderde sleutels |Het aantal items uit de cache verwijderd tijdens het opgegeven reporting interval vanwege de `maxmemory` limiet. Dit wordt toegewezen aan `evicted_keys` van de Redis-INFO-opdracht. |
| Verlopen sleutels |Het aantal items die tijdens het opgegeven interval voor rapportage uit de cache is verlopen. Deze waarde wordt toegewezen aan `expired_keys` van de Redis-INFO-opdracht.|
| Opgehaalde items |Het aantal get-bewerkingen uit de cache tijdens het opgegeven interval voor rapportage. Deze waarde is de som van de volgende waarden van de gegevens van de Redis-opdracht Alles: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, en `cmdstat_getrange`, en is gelijk aan de som van de cachetreffers en missers tijdens het rapportage-interval. |
| Bewerkingen per seconde | Het totale aantal opdrachten verwerkt per seconde door de cacheserver tijdens het opgegeven interval voor rapportage.  Deze waarde wordt toegewezen aan 'instantaneous_ops_per_sec' van de Redis-INFO-opdracht. |
| Redis-serververmogen |Het percentage van de cycli waarin de Redis-server is bezet wordt verwerkt en niet wachten op niet-actieve voor berichten. Als deze teller 100 betekent dit dat de Redis-server heeft bereikt een plafond prestaties en de CPU kan niet worden verwerkt moet u een sneller werken. Als u hoge belasting van Redis-Server ziet, ziet u time-outuitzonderingen in de client. In dit geval moet u overwegen omhoog of u uw gegevens in meerdere caches. |
| Sets |Het aantal set bewerkingen aan de cache tijdens het opgegeven interval voor rapportage. Deze waarde is de som van de volgende waarden van de gegevens van de Redis-opdracht Alles: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , en `cmdstat_setnx`. |
| Totaal aantal sleutels  | Het maximale aantal sleutels in de cache in de afgelopen periode reporting. Dit wordt toegewezen aan `keyspace` van de Redis-INFO-opdracht. Totaal aantal sleutels retourneert vanwege een beperking van het onderliggende systeem metrische gegevens voor caches met clustering is ingeschakeld, het maximum aantal sleutels van de shard die het maximum aantal sleutels tijdens het rapportage-interval was.  |
| Totaal aantal bewerkingen |Het totale aantal opdrachten verwerkt door de cacheserver tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `total_commands_processed` van de Redis-INFO-opdracht. Houd er rekening mee dat wanneer Azure voor Redis-Cache wordt gebruikt voor pub/sub er worden geen metrische gegevens voor `Cache Hits`, `Cache Misses`, `Gets`, of `Sets`, maar er zijn ook `Total Operations` metrische gegevens die overeenkomen met het cachegebruik voor pub/sub-bewerkingen. |
| Gebruikt geheugen |De hoeveelheid van het cachegeheugen voor sleutel/waarde-paren in de cache in MB gebruikt tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `used_memory` van de Redis-INFO-opdracht. Dit omvat geen metagegevens of fragmentatie. |
| Percentage gebruikt geheugen | De % van de totale hoeveelheid geheugen die wordt gebruikt tijdens het opgegeven interval voor rapportage.  Dit verwijst naar de waarde 'used_memory' van de Redis-INFO-opdracht om het percentage te berekenen. |
| Gebruikt geheugen RSS |De hoeveelheid cache-geheugen in MB gebruikt tijdens het opgegeven reporting interval, met inbegrip van fragmentatie en metagegevens. Deze waarde wordt toegewezen aan `used_memory_rss` van de Redis-INFO-opdracht. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Waarschuwingen
U kunt instellen dat u waarschuwingen ontvangt op basis van metrische gegevens en activiteitenlogboeken. Met Azure Monitor kunt u een waarschuwing zodanig configureren dat deze bij activering het volgende doet:

* Een e-mailmelding verzenden
* Een webhook aanroepen
* Een logische Azure-app aanroepen

Voor het configureren van regels voor waarschuwingen voor uw cache, klikt u op **waarschuwingsregels** uit de **resourcemenu**.

![Bewaking](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Zie voor meer informatie over het configureren en gebruiken van waarschuwingen, [overzicht van waarschuwingen](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Activiteitenlogboeken
Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-Cache voor instanties van Redis. Het was voorheen bekend als 'audit logs' of 'operational-logs'. Met activiteitenlogboeken kunt u bepalen de ' wat, wie, en wanneer ' voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op uw Azure-Cache voor instanties van Redis. 

> [!NOTE]
> Activiteitenlogboeken bevatten geen lees (GET)-bewerkingen.
>
>

Als u wilt de activiteitenlogboeken voor uw cache weergeven, klikt u op **activiteitenlogboeken** uit de **resourcemenu**.

Zie voor meer informatie over activiteitenlogboeken [overzicht van de Azure-activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md).











