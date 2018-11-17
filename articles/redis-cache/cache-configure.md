---
title: Het configureren van Azure Redis-Cache | Microsoft Docs
description: Inzicht in de standaardconfiguratie van Redis voor Azure Redis Cache en informatie over het configureren van uw Azure Redis Cache-exemplaren
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: wesmc
ms.openlocfilehash: 39c72dde6bcfec2879efd05a1769ad443c9ffd2f
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823959"
---
# <a name="how-to-configure-azure-redis-cache"></a>Azure Redis-Cache configureren
Dit onderwerp beschrijft de configuraties die beschikbaar zijn voor uw Azure Redis Cache-exemplaren. Dit onderwerp bevat ook de standaardconfiguratie voor het server van Redis voor Azure Redis Cache-exemplaren.

> [!NOTE]
> Zie voor meer informatie over het configureren en gebruiken premium-functies voor caching [persistentie configureren](cache-how-to-premium-persistence.md), [clustering configureren](cache-how-to-premium-clustering.md), en [ondersteuning voor virtuele netwerken configureren ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Redis cache-instellingen configureren
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis-Cache-instellingen worden bekeken en geconfigureerd op de **Redis-Cache** blade met behulp van de **Resourcemenu**.

![Redis-Cache-instellingen](./media/cache-configure/redis-cache-settings.png)

U kunt bekijken en configureren van de volgende instellingen met behulp van de **Resourcemenu**.

* [Overzicht](#overview)
* [Activiteitenlogboek](#activity-log)
* [Toegangsbeheer (IAM)](#access-control-iam)
* [Tags](#tags)
* [Problemen vaststellen en oplossen](#diagnose-and-solve-problems)
* [Instellingen](#settings)
    * [Toegangssleutels](#access-keys)
    * [Geavanceerde instellingen](#advanced-settings)
    * [Redis-Cache Advisor](#redis-cache-advisor)
    * [Schalen](#scale)
    * [Redis-clusters](#cluster-size)
    * [Redis-gegevenspersistentie](#redis-data-persistence)
    * [Updates plannen](#schedule-updates)
    * [Geo-replicatie](#geo-replication)
    * [Virtueel netwerk](#virtual-network)
    * [Firewall](#firewall)
    * [Eigenschappen](#properties)
    * [Hiermee vergrendelt u](#locks)
    * [Automatiseringsscript](#automation-script)
* [Beheer](#administration)
    * [Gegevens importeren](#importexport)
    * [Gegevens exporteren](#importexport)
    * [Opnieuw opstarten](#reboot)
* [Controle](#monitoring)
    * [Metrische redis-gegevens](#redis-metrics)
    * [Waarschuwingsregels](#alert-rules)
    * [Diagnostics](#diagnostics)
* [Ondersteuning en probleemoplossing van instellingen](#support-amp-troubleshooting-settings)
    * [Resourcestatus](#resource-health)
    * [Nieuwe ondersteuningsaanvraag](#new-support-request)


## <a name="overview"></a>Overzicht

**Overzicht** vindt u basisinformatie over uw cache, zoals naam, poort, prijzen laag en geselecteerde cache metrische gegevens.

### <a name="activity-log"></a>Activiteitenlogboek

Klik op **activiteitenlogboek** om acties die worden uitgevoerd op uw cache weer te geven. U kunt ook filters gebruiken om uit te vouwen in deze weergave zodanig dat andere resources. Zie voor meer informatie over het werken met auditlogboeken [bewerkingen controleren met Resource Manager](../azure-resource-manager/resource-group-audit.md). Zie voor meer informatie over het bewaken van Azure Redis Cache gebeurtenissen [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Toegangsbeheer (IAM)

De **toegangsbeheer (IAM)** sectie biedt ondersteuning voor op rollen gebaseerd toegangsbeheer (RBAC) in Azure portal. Deze configuratie helpt organisaties bij het voldoen aan hun vereisten voor gegevenstoegang management eenvoudig en nauwkeurig. Zie voor meer informatie, [rollen gebaseerd toegangsbeheer in Azure portal](../role-based-access-control/role-assignments-portal.md).

### <a name="tags"></a>Tags

De **Tags** sectie helpt u uw resources te organiseren. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Problemen vaststellen en oplossen

Klik op **vaststellen en oplossen van problemen met** worden opgegeven met veelvoorkomende problemen en strategieën voor het oplossen ervan.



## <a name="settings"></a>Instellingen
De **instellingen** sectie kunt u toegang tot en configureer de volgende instellingen voor uw cache.

* [Toegangssleutels](#access-keys)
* [Geavanceerde instellingen](#advanced-settings)
* [Redis-Cache Advisor](#redis-cache-advisor)
* [Schalen](#scale)
* [Redis-clusters](#cluster-size)
* [Redis-gegevenspersistentie](#redis-data-persistence)
* [Updates plannen](#schedule-updates)
* [Geo-replicatie](#geo-replication)
* [Virtueel netwerk](#virtual-network)
* [Firewall](#firewall)
* [Eigenschappen](#properties)
* [Hiermee vergrendelt u](#locks)
* [Automatiseringsscript](#automation-script)



### <a name="access-keys"></a>Toegangssleutels
Klik op **toegangssleutels** te bekijken of genereren van de toegangssleutels voor uw cache. Deze sleutels worden gebruikt door de clients die verbinding maken met uw cache.

![Redis-Cache-toegangssleutels](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Geavanceerde instellingen
De volgende instellingen zijn geconfigureerd op de **geavanceerde instellingen** blade.

* [Toegangspoorten](#access-ports)
* [Beleid voor geheugen](#memory-policies)
* [Keyspace-meldingen (geavanceerde instellingen)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Toegangspoorten
Voor nieuwe caches is niet-SSL-toegang standaard uitgeschakeld. Als de poort zonder SSL-beveiliging, klikt u op **Nee** voor **toestaan alleen toegang met SSL** op de **geavanceerde instellingen** blade en klik vervolgens op **opslaan**.

![Redis-Cache-poorten](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Beleid voor geheugen
De **Maxmemory-beleid**, **maxmemory-gereserveerde**, en **maxfragmentationmemory gereserveerd** instellingen op de **geavanceerde instellingen** Blade de geheugen-beleid voor de cache configureren.

![Redis-Cache Maxmemory-beleid](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory-beleid** configureert u het verwijderingsbeleid voor de cache en kunt u kiezen uit de volgende beleidsregels voor taakverwijdering:

* `volatile-lru` -Dit is het standaardbeleid voor verwijdering.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Voor meer informatie over `maxmemory` beleid, Zie [verwijderingsbeleid](http://redis.io/topics/lru-cache#eviction-policies).

De **maxmemory-gereserveerde** instelling configureert u de hoeveelheid geheugen in MB, die is gereserveerd voor niet-cache-bewerkingen, zoals replicatie tijdens de failover. Deze waarde instelt, kunt u een consistente gebruikerservaring van de Redis-server hebben wanneer de belasting varieert. Deze waarde moet hoger zijn voor workloads die zijn geschreven zware worden ingesteld. Wanneer het geheugen is gereserveerd voor dergelijke bewerkingen plaatsvindt, is het niet beschikbaar voor de opslag van gegevens in de cache.

De **maxfragmentationmemory gereserveerd** instelling configureert u de hoeveelheid geheugen in MB die is gereserveerd voor fragmentatie van het geheugen. Deze waarde in te stellen, kunt u een consistente gebruikerservaring van de Redis-server hebben als de cache vol is of bijna vol is en de fragmentatie van de verhouding hoog is. Wanneer het geheugen is gereserveerd voor dergelijke bewerkingen plaatsvindt, is het niet beschikbaar voor de opslag van gegevens in de cache.

Eén ding te overwegen bij het kiezen van een nieuwe waarde voor de reservering van geheugen (**maxmemory-gereserveerde** of **maxfragmentationmemory gereserveerd**) is deze wijziging kan invloed van een cache die al wordt uitgevoerd met grote hoeveelheden gegevens. Als u een cache van 53 GB met 49 GB aan gegevens en vervolgens wijzigt u de waarde van de reservering in 8 GB, wordt deze wijziging bijvoorbeeld de maximale hoeveelheid beschikbaar geheugen voor het systeem op 45 GB verwijderen. Als uw huidige `used_memory` of uw `used_memory_rss` waarden hoger is dan de nieuwe limiet van 45 GB zijn, en vervolgens het systeem heeft onbeschikbaar maken van gegevens tot beide `used_memory` en `used_memory_rss` hieronder 45 GB zijn. Verwijdering kan de fragmentatie van de belasting en het geheugen van de server vergroten. Voor meer informatie over cache metrische gegevens zoals `used_memory` en `used_memory_rss`, Zie [beschikbare metrische gegevens en rapportage van intervallen](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> De **maxmemory-gereserveerde** en **maxfragmentationmemory gereserveerd** instellingen zijn alleen beschikbaar voor Standard en Premium-caches.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Keyspace-meldingen (geavanceerde instellingen)
Redis keyspace-meldingen zijn geconfigureerd op de **geavanceerde instellingen** blade. Keyspace-kennisgevingen kunnen clients meldingen ontvangen wanneer bepaalde gebeurtenissen optreden.

![Redis-Cache geavanceerde instellingen](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspace-meldingen en de **op de hoogte stellen-keyspace-gebeurtenissen** instelling zijn alleen beschikbaar voor Standard en Premium-caches.
> 
> 

Zie voor meer informatie, [Keyspace-meldingen Redis](http://redis.io/topics/notifications). Zie voor een voorbeeld van code, de [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) -bestand in de [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis-cache Advisor
De **Redis Cache Advisor** blade aanbevelingen voor uw cache wordt weergegeven. Tijdens normale bewerkingen, worden geen aanbevelingen weergegeven. 

![Aanbevelingen](./media/cache-configure/redis-cache-no-recommendations.png)

Als geen voorwaarden tijdens de bewerkingen van uw cache zoals hoog geheugengebruik, de bandbreedte van het netwerk of de belasting van de server optreden, een waarschuwing wordt weergegeven op de **Redis-Cache** blade.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations-alert.png)

Meer informatie vindt u op de **aanbevelingen** blade.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations.png)

U kunt deze metrische gegevens controleren op de [Controlegrafieken](cache-how-to-monitor.md#monitoring-charts) en [gebruik grafieken](cache-how-to-monitor.md#usage-charts) secties van de **Redis-Cache** blade.

Elke prijscategorie heeft verschillende beperkingen voor clientverbindingen, het geheugen en bandbreedte. Als uw cache nadert de maximale capaciteit voor deze metrische gegevens gedurende een langere periode tijd wordt opgelost, wordt een aanbeveling gemaakt. Voor meer informatie over de metrische gegevens en -beperkingen gecontroleerd door de **aanbevelingen** hulpprogramma, raadpleegt u de volgende tabel:

| Redis-Cache metrische gegevens | Meer informatie |
| --- | --- |
| Gebruik van netwerkbandbreedte |[Prestaties van de cache - beschikbare bandbreedte](cache-faq.md#cache-performance) |
| Verbonden clients |[Standaard Redis-serverconfiguratie - maxclients](#maxclients) |
| Belasting van de server |[Gebruik grafieken - Redis-Serververmogen](cache-how-to-monitor.md#usage-charts) |
| Geheugengebruik |[Prestaties van de cache - grootte](cache-faq.md#cache-performance) |

Als u uw cache bijwerken, klikt u op **nu een upgrade uitvoeren** de prijscategorie wijzigen en [schaal](#scale) uw cache. Zie voor meer informatie over het kiezen van een prijscategorie [welke Redis-Cache-aanbieding en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Schalen
Klik op **schaal** weergeven of wijzigen van de prijscategorie voor uw cache. Zie voor meer informatie over het omhoog schalen [hoe Scale Azure Redis Cache](cache-how-to-scale.md).

![Redis-Cache prijscategorie](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis-clusters
Klik op **(PREVIEW) Redis clustergrootte** te wijzigen van de grootte van het cluster voor een actieve premium-cache met clustering is ingeschakeld.

> [!NOTE]
> Houd er rekening mee dat de Azure Redis Cache Premium-laag is nu vrijgegeven voor algemene beschikbaarheid, de grootte van de Redis-Cluster-functie momenteel in preview is.
> 
> 

![Redis-clusters](./media/cache-configure/redis-cache-redis-cluster-size.png)

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in de **aantal Shards** tekstvak en klik op **OK** om op te slaan.

> [!IMPORTANT]
> Redis-clustering is alleen beschikbaar voor Premium-caches. Zie [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Clustering voor een Premium Azure Redis Cache configureren) voor meer informatie.
> 
> 


### <a name="redis-data-persistence"></a>Redis-gegevenspersistentie
Klik op **Redis-gegevenspersistentie** als u wilt inschakelen, uitschakelen of persistentie van gegevens voor uw premium-cache configureren. Azure Redis Cache biedt met behulp van Redis-persistentie [RDB persistentie](cache-how-to-premium-persistence.md#configure-rdb-persistence) of [AOF persistentie](cache-how-to-premium-persistence.md#configure-aof-persistence).

Zie voor meer informatie, [persistentie voor een Premium Azure Redis Cache configureren](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Redis-gegevenspersistentie is alleen beschikbaar voor Premium-caches. 
> 
> 

### <a name="schedule-updates"></a>Updates plannen
De **updates plannen** blade kunt u om een onderhoudsvenster voor updates van de Redis-server voor uw cache toe te wijzen. 

> [!IMPORTANT]
> Het onderhoudsvenster is alleen van toepassing op server-updates, Redis en niet voor alle Azure-updates of updates voor het besturingssysteem van de virtuele machines die als host de cache fungeren.
> 
> 

![Updates plannen](./media/cache-configure/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleert u de gewenste dagen en geef de begintijd van onderhoud-venster voor elke dag en op **OK**. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

> [!IMPORTANT]
> De **updates plannen** functionaliteit is alleen beschikbaar voor Premium-laag-caches. Zie voor meer informatie en instructies [beheer van Azure Redis-Cache - updates plannen](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Geo-replicatie

De **Geo-replicatie** blade biedt een mechanisme voor het koppelen van twee instanties van de laag Azure Redis Cache Premium. Een cache is ingesteld als de primaire gekoppelde cache, en de andere als de gekoppelde secundaire cache. De gekoppelde secundaire cache wordt alleen-lezen en gegevens geschreven naar de primaire cache gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache Azure-regio's.

> [!IMPORTANT]
> **Geo-replicatie** is alleen beschikbaar voor Premium-laag-caches. Zie voor meer informatie en instructies [Geo-replicatie configureren voor Azure Redis Cache](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Virtual Network
De **Virtueelnetwerk** sectie kunt u de instellingen van het virtuele netwerk voor uw cache kunt configureren. Voor informatie over het maken van een premium-cache met VNET ondersteunen en de instellingen worden bijgewerkt, Zie [over het configureren van ondersteuning voor het virtuele netwerk voor een Premium Azure Redis Cache](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Instellingen voor virtuele netwerken zijn alleen beschikbaar voor premium-caches die zijn geconfigureerd met VNET-ondersteuning tijdens het maken van de cache. 
> 
> 

### <a name="firewall"></a>Firewall

Configuratie van de firewall-regels is beschikbaar voor alle lagen van Azure Redis-Cache.

Klik op **Firewall** wilt weergeven en configureren van de firewallregels voor de cache.

![Firewall](./media/cache-configure/redis-firewall-rules.png)

U kunt de firewall-regels met een begin- en IP-adresbereik opgeven. Wanneer de firewall-regels zijn geconfigureerd, worden alleen clientverbindingen van het opgegeven IP-adresbereiken worden verbonden met de cache. Wanneer een firewallregel wordt opgeslagen, is er een korte vertraging voordat de regel is van kracht. Deze vertraging is meestal minder dan één minuut.

> [!IMPORTANT]
> Verbindingen van Azure Redis Cache bewakingssystemen zijn altijd toegestaan, zelfs als er firewallregels zijn geconfigureerd.
> 
> 

### <a name="properties"></a>Eigenschappen
Klik op **eigenschappen** om informatie over de cache, met inbegrip van de cache-eindpunt en de poorten te bekijken.

![Redis-Cache-eigenschappen](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Vergrendelingen
De **Hiermee vergrendelt u** sectie kunt u een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources vergrendelen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Automation-script

Klik op **automatiseringsscript** te exporteren van een sjabloon van uw geïmplementeerde resources voor toekomstige implementaties. Zie voor meer informatie over het werken met sjablonen [resources implementeren met Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Instellingen voor beheer
De instellingen in de **beheer** sectie kunt u de volgende beheertaken uitvoeren voor uw cache. 

![Beheer](./media/cache-configure/redis-cache-administration.png)

* [Gegevens importeren](#importexport)
* [Gegevens exporteren](#importexport)
* [Opnieuw opstarten](#reboot)


### <a name="importexport"></a>Import/Export
Import/Export is een Azure Redis Cache gegevensbewerking voor het beheer, zodat u kunt importeren en exporteren van gegevens in de cache door te importeren en exporteren van een momentopname Redis Cache Database (RDB) van een premium-cache naar een pagina-blobs in een Azure Storage-Account. Import/Export kunt u om te migreren tussen verschillende exemplaren van Azure Redis-Cache of vullen van de cache met gegevens voor het gebruik.

Invoer kan worden gebruikt om Redis compatibele RDB-bestanden vanaf een willekeurige Redis-server die wordt uitgevoerd in een cloud of de omgeving, met inbegrip van Redis die worden uitgevoerd op Linux, Windows of elke andere cloudprovider, zoals Amazon Web Services en andere. Importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingevulde gegevens. Tijdens het importproces Azure Redis Cache laadt de RDB-bestanden van Azure storage in het geheugen, en voegt vervolgens de sleutels in de cache.

Export kunt u de gegevens die zijn opgeslagen in Azure Redis Cache voor Redis-compatibele RDB-bestanden exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van één Azure Redis-Cache-exemplaar naar een andere of naar een andere Redis-server. Tijdens het exportproces een tijdelijke bestand wordt gemaakt op de virtuele machine die als host fungeert voor de server-exemplaar van Azure Redis-Cache en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status geslaagd of mislukt, wordt het tijdelijke bestand verwijderd.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor Premium-laag-caches. Zie voor meer informatie en instructies [importeren en exporteren van gegevens in Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Opnieuw opstarten
De **opnieuw opstarten** blade kunt u de knooppunten van uw cache opnieuw opstarten. Deze mogelijkheid opnieuw opstarten, kunt u uw testtoepassing voor tolerantie als er een storing van een cacheknooppunt.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot.png)

Als u een premium-cache hebt met clustering is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot-cluster.png)

Een of meer knooppunten van uw cache opnieuw opstarten, selecteer de gewenste knooppunten en klikt u op **opnieuw opstarten**. Als u een premium-cache hebt met clustering is ingeschakeld, selecteert u de shards die opnieuw op en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw opstarten, en zijn online een paar minuten later opnieuw.

> [!IMPORTANT]
> Opnieuw opstarten is nu beschikbaar voor alle Prijscategorieën. Zie voor meer informatie en instructies [beheer van Azure Redis-Cache - opnieuw opstarten](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Bewaking

De **bewaking** sectie kunt u diagnostische gegevens en controle van uw Redis-Cache configureren. Zie voor meer informatie over Azure Redis Cache controle en diagnose [Azure Redis Cache controleren](cache-how-to-monitor.md).

![Diagnostiek](./media/cache-configure/redis-cache-diagnostics.png)

* [Metrische redis-gegevens](#redis-metrics)
* [Waarschuwingsregels](#alert-rules)
* [Diagnostics](#diagnostics)

### <a name="redis-metrics"></a>Metrische Redis-gegevens
Klik op **metrische Redis** naar [Bekijk metrische gegevens](cache-how-to-monitor.md#view-cache-metrics) voor uw cache.

### <a name="alert-rules"></a>Waarschuwingsregels

Klik op **waarschuwingsregels** waarschuwingen op basis van metrische gegevens over Redis-Cache configureren. Zie voor meer informatie, [waarschuwingen](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Diagnostiek

Metrische cache-gegevens in Azure Monitor zijn standaard [30 dagen bewaard](../azure-monitor/platform/data-collection.md#metrics) en vervolgens verwijderd. Als u wilt uw cache metrische gegevens langer dan 30 dagen behouden, klikt u op **diagnostische gegevens** naar [configureren van het opslagaccount](cache-how-to-monitor.md#export-cache-metrics) gebruikt voor het opslaan van cache diagnostische gegevens.

>[!NOTE]
>Naast het archiveren van uw cache metrische gegevens naar de opslag, kunt u ook [ze naar een Event hub streamen of ze verzenden naar Log Analytics](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).
>
>

## <a name="support--troubleshooting-settings"></a>Ondersteuning en probleemoplossing van instellingen
De instellingen in de **ondersteuning en probleemoplossing** sectie u voorzien van opties voor het oplossen van problemen met uw cache.

![Ondersteuning en probleemoplossing](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Resourcestatus](#resource-health)
* [Nieuwe ondersteuningsaanvraag](#new-support-request)

### <a name="resource-health"></a>Status van resources
**Resourcestatus** wordt de resource gecontroleerd en wordt aangegeven of deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de Azure Resource health-service, [overzicht van Azure Resource health](../resource-health/resource-health-overview.md).

> [!NOTE]
> Resource health is momenteel niet om te rapporteren over de status van Azure Redis Cache-exemplaren die worden gehost in een virtueel netwerk. Zie voor meer informatie, [, werken alle functies van de cache bij het hosten van een cache in een VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Nieuw ondersteuningsverzoek
Klik op **nieuwe ondersteuningsaanvraag** om een ondersteuningsaanvraag voor uw cache te openen.





## <a name="default-redis-server-configuration"></a>Standaardconfiguratie voor Redis-server
Nieuwe Azure Redis-Cache-exemplaren worden geconfigureerd met de volgende waarden van standaard Redis-configuratie:

> [!NOTE]
> De instellingen in deze sectie kunnen niet worden gewijzigd met behulp van de `StackExchange.Redis.IServer.ConfigSet` methode. Als deze methode wordt aangeroepen met een van de opdrachten in deze sectie, wordt er een vergelijkbaar met het volgende voorbeeld uitzondering gegenereerd:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Alle waarden die kunnen worden geconfigureerd, zoals **max-geheugen-policy**, kunnen worden geconfigureerd via de Azure portal of beheer via de opdrachtregel-hulpprogramma's zoals Azure CLI of PowerShell.
> 
> 

| Instelling | Standaardwaarde | Beschrijving |
| --- | --- | --- |
| `databases` |16 |Het standaardnummer van databases is 16, maar u kunt een ander nummer op basis van de prijscategorie configureren. <sup>1</sup> de standaarddatabase DB 0 is, kunt u een ander account op een per verbinding uit te voeren met `connection.GetDatabase(dbid)` waar `dbid` is een getal tussen `0` en `databases - 1`. |
| `maxclients` |Afhankelijk van de prijscategorie<sup>2</sup> |Deze waarde is het maximum aantal verbonden clients toegestaan op hetzelfde moment. Zodra de limiet is bereikt gesloten Redis alle nieuwe verbindingen, een 'maximum aantal clients bereikt'-fout. |
| `maxmemory-policy` |`volatile-lru` |Maxmemory-beleid is de instelling voor hoe Redis wat u selecteert wilt verwijderen wanneer `maxmemory` (de grootte van de cache die u hebt geselecteerd tijdens het maken van de cache-aanbieding) is bereikt. Met Azure Redis Cache de standaardinstelling is `volatile-lru`, waarbij de sleutels worden verwijderd met een vervaldatum instellen met behulp van een LRU-algoritme. Deze instelling kan worden geconfigureerd in Azure portal. Zie voor meer informatie, [geheugen beleid](#memory-policies). |
| `maxmemory-samples` |3 |Zijn redelijk algoritmen in plaats van exacte algoritmen te besparen, LRU- en minimale TTL-algoritmen. Redis standaard drie sleutels controles en aanbiedingen die minder recent is gebruikt. |
| `lua-time-limit` |5.000 |Maximale uitvoeringstijd van een Lua-script in milliseconden. Als de maximum uitvoeringstijd is bereikt, wordt Redis registreert dat een script nog steeds uitgevoerd na de maximale toegestane tijd wordt, en begint met de antwoorden op query's met een fout. |
| `lua-event-limit` |500 |Maximale grootte van de wachtrij script. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |De limieten van client uitvoer buffer kunnen worden gebruikt om af te dwingen ontkoppeling van clients die niet van gegevens van de server snel genoeg om een bepaalde reden lezen zijn (een veelvoorkomende reden is dat een Pub/Sub-client zo snel mogelijk de publisher kunt maken ze berichten kan niet gebruiken). Zie voor meer informatie, [ http://redis.io/topics/clients ](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>de limiet voor `databases` verschilt voor elke Azure Redis Cache-prijscategorie en bij het maken van cache kan worden ingesteld. Als er geen `databases` instelling is opgegeven tijdens het maken van de cache, de standaardwaarde is 16.

* Basic en Standard-caches
  * C0 (250 MB)-cache - maximaal 16 databases
  * C1 (1 GB)-cache - maximaal 16 databases
  * C2 (2,5 GB)-cache - maximaal 16 databases
  * C3 (6 GB)-cache - maximaal 16 databases
  * C4 (13 GB)-cache - maximaal 32 databases
  * C5 (26 GB)-cache - maximaal 48 databases
  * C6 (53 GB)-cache - maximaal 64 databases
* Premium-caches
  * P1 (6 GB - 60 GB), maximaal 16 databases
  * P2 (13 GB - 130 GB), maximaal 32 databases
  * P3 (26 GB - 260 GB), maximaal 48 databases
  * P4 (53 GB - 530 GB), maximaal 64 databases
  * Alle premium-caches met Redis-cluster ingeschakeld - Redis-cluster biedt alleen ondersteuning voor gebruik van 0-database zodat de `databases` beperken voor elke premium-cache met Redis-cluster ingeschakeld is in feite 1 en de [Selecteer](http://redis.io/commands/select) opdracht is niet toegestaan. Zie voor meer informatie, [heb ik wijzigingen aanbrengen in mijn clienttoepassing voor het gebruik van clustering nodig?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Zie voor meer informatie over databases [wat zijn Redis-databases?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> De `databases` instelling kan worden geconfigureerd tijdens de cache maken en alleen met behulp van PowerShell, CLI of andere clients beheren. Voor een voorbeeld van het configureren van `databases` tijdens het maken van de cache met behulp van PowerShell, Zie [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` verschilt voor elke Azure Redis Cache-prijscategorie.

* Basic en Standard-caches
  * C0 (250 MB)-cache - maximaal 256-verbindingen
  * C1 (1 GB)-cache - maximaal 1000 verbindingen
  * C2 (2,5 GB)-cache - maximaal 2000 verbindingen
  * C3 (6 GB)-cache - maximaal 5.000 verbindingen
  * C4 (13 GB)-cache - maximaal 10.000 verbindingen
  * C5 (26 GB)-cache - maximaal 15.000 verbindingen
  * C6 (53 GB)-cache - maximaal 20.000-verbindingen
* Premium-caches
  * P1 (6 GB - 60 GB), maximaal 7.500 verbindingen
  * P2 (13 GB - 130 GB), maximaal 15.000 verbindingen
  * P3 (26 GB - 260 GB), maximaal 30.000 verbindingen
  * P4 (53 GB - 530 GB), maximaal 40.000 verbindingen

> [!NOTE]
> Kunt u elke grootte van cache *tot* een bepaald aantal verbindingen, elke verbinding met Redis is overhead die ermee verbonden zijn. Een voorbeeld van dergelijke overhead zou CPU- en geheugengebruik als gevolg van TLS/SSL-codering zijn. De limiet voor maximum aantal verbindingen voor een opgegeven cachegrootte wordt ervan uitgegaan dat een licht geladen cache. Als laden uit verbinding overhead *plus* laden uit clientbewerkingen groter is dan de capaciteit voor het systeem, de cache kan consistentieproblemen capaciteit, zelfs als u hebt de limiet voor verbindingen voor de huidige cachegrootte niet overschreden.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis-opdrachten niet ondersteund in Azure Redis-Cache
> [!IMPORTANT]
> Omdat de configuratie en beheer van exemplaren van Azure Redis Cache wordt beheerd door Microsoft, worden de volgende opdrachten zijn uitgeschakeld. Als u probeert om aan te roepen ze, ontvangt u een foutbericht weergegeven die vergelijkbaar is met `"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * CONFIGURATIE
> * FOUTEN OPSPOREN
> * MIGREREN
> * OPSLAAN
> * AFSLUITEN
> * SLAVEOF
> * CLUSTER - schrijven zijn uitgeschakeld, maar alleen-lezen Cluster opdrachten zijn toegestaan.
> 
> 

Zie voor meer informatie over Redis-opdrachten [ http://redis.io/commands ](http://redis.io/commands).

## <a name="redis-console"></a>Redis-console
Kunt u veilig opdrachten uitgeven voor uw Azure Redis-Cache-exemplaren met behulp van de **Redis-Console**, die beschikbaar is in de Azure-portal voor alle cachelagen.

> [!IMPORTANT]
> - De Redis-Console werkt niet met [VNET](cache-how-to-premium-vnet.md). Wanneer uw cache deel van een VNET uitmaakt, kan alleen clients in het VNET toegang krijgen tot de cache. Omdat Redis-Console wordt uitgevoerd in uw lokale browser, die zich buiten het VNET, wordt er geen verbinding met uw cache.
> - Niet alle Redis-opdrachten worden ondersteund in Azure Redis Cache. Voor een lijst van Redis-opdrachten die zijn uitgeschakeld voor Azure Redis Cache, Zie de vorige [Redis-opdrachten niet ondersteund in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) sectie. Zie voor meer informatie over Redis-opdrachten [ http://redis.io/commands ](http://redis.io/commands).
> 
> 

Voor toegang tot de Redis-Console, klikt u op **Console** uit de **Redis-Cache** blade.

![Redis-console](./media/cache-configure/redis-console-menu.png)

Voor het uitgeven van opdrachten op basis van uw cache-exemplaar, typ u de gewenste opdracht in de console.

![Redis-console](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a>Met behulp van de Redis-Console met een geclusterde premium-cache

Wanneer de cache met behulp van de Redis-Console met een premium worden geclusterd, kunt u opdrachten geven aan een enkele shard van de cache. Als u wilt een opdracht geven voor een specifieke shard, eerst verbinding maken met de gewenste shard door erop te klikken op de kiezer shard.

![Redis-console](./media/cache-configure/redis-console-premium-cluster.png)

Als u probeert te krijgen tot een sleutel die is opgeslagen in een andere shard dan de verbonden shard, ontvangt u een foutbericht weergegeven die vergelijkbaar is met het volgende bericht:

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

In het vorige voorbeeld shard 1 wordt de geselecteerde shard, maar `myKey` bevindt zich in de shard 0, zoals aangegeven door de `(shard 0)` gedeelte van het foutbericht. In dit voorbeeld, voor toegang tot `myKey`shard met behulp van de shard-kiezer 0, en selecteer de gewenste opdracht.


## <a name="move-your-cache-to-a-new-subscription"></a>Uw cache verplaatsen naar een nieuw abonnement
U kunt uw cache verplaatsen naar een nieuw abonnement door te klikken op **verplaatsen**.

![Redis-Cache verplaatsen](./media/cache-configure/redis-cache-move.png)

Zie voor meer informatie over het verplaatsen van resources van een resourcegroep naar een andere en van één abonnement naar een andere [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het werken met Redis-opdrachten [hoe kan ik Redis-opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)

