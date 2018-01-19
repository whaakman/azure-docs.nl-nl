---
title: Het configureren van Azure Redis-Cache | Microsoft Docs
description: Inzicht in de standaardconfiguratie voor Redis voor Azure Redis-Cache en informatie over het configureren van uw Azure Redis-Cache-exemplaren
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
ms.openlocfilehash: a65832a30a570944ff30d02c2f173df345bde32c
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-azure-redis-cache"></a>Het configureren van Azure Redis-Cache
Dit onderwerp wordt beschreven hoe u om te controleren en bijwerken van de configuratie voor uw Azure Redis-Cache-exemplaren en bevat informatie over de configuratie standaard Redis-server voor Azure Redis-Cache-exemplaren.

> [!NOTE]
> Zie voor meer informatie over het configureren en het gebruik van Premiumfuncties cache [persistentie configureren](cache-how-to-premium-persistence.md), [clustering configureren](cache-how-to-premium-clustering.md), en [het configureren van Virtual Network-ondersteuning ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Redis-cache-instellingen configureren
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis-Cache-instellingen worden weergegeven en geconfigureerd op de **Redis-Cache** blade met behulp van de **Resource Menu**.

![Redis-Cache-instellingen](./media/cache-configure/redis-cache-settings.png)

U kunt bekijken en configureren van de volgende instellingen met behulp van de **Resource Menu**.

* [Overzicht](#overview)
* [Activiteitenlogboek](#activity-log)
* [Toegangsbeheer (IAM)](#access-control-iam)
* [Tags](#tags)
* [Problemen vaststellen en oplossen](#diagnose-and-solve-problems)
* [Instellingen](#settings)
    * [Toegangstoetsen](#access-keys)
    * [Geavanceerde instellingen](#advanced-settings)
    * [Redis-Cache Advisor](#redis-cache-advisor)
    * [Schalen](#scale)
    * [Clustergrootte redis](#cluster-size)
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
* [Monitoring](#monitoring)
    * [Metrische gegevens redis](#redis-metrics)
    * [Regels voor waarschuwingen](#alert-rules)
    * [Diagnostics](#diagnostics)
* [Ondersteuning en probleemoplossing van instellingen](#support-amp-troubleshooting-settings)
    * [Resourcestatus](#resource-health)
    * [Nieuw ondersteuningsverzoek](#new-support-request)


## <a name="overview"></a>Overzicht

**Overzicht** biedt u basisinformatie over uw cache, zoals naam, poort, prijscategorie, en geselecteerde cache metrische gegevens.

### <a name="activity-log"></a>Activiteitenlogboek

Klik op **activiteitenlogboek** om acties die worden uitgevoerd op uw cache weer te geven. U kunt ook filters gebruiken om uit te breiden in deze weergave zodanig dat andere bronnen. Zie voor meer informatie over het werken met controlelogboeken [bewerkingen met Resource Manager controleren](../azure-resource-manager/resource-group-audit.md). Zie voor meer informatie over het controleren van gebeurtenissen van Azure Redis-Cache [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Toegangsbeheer (IAM)

De **toegangsbeheer (IAM)** sectie biedt ondersteuning voor op rollen gebaseerde toegangsbeheer (RBAC) in de Azure portal om te voldoen aan de beheervereisten toegang eenvoudig en nauwkeurig organisaties. Zie voor meer informatie [toegangsbeheer op basis van rollen in Azure portal](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Tags

De **labels** sectie helpt u bij uw resources te organiseren. Zie voor meer informatie [met labels om uw Azure-resources te organiseren](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Problemen vaststellen en oplossen

Klik op **diagnosticeren en oplossen van problemen** worden opgegeven met veelvoorkomende problemen en strategieën voor het oplossen ervan.



## <a name="settings"></a>Instellingen
De **instellingen** sectie kunt u toegang tot en configureer de volgende instellingen voor uw cache.

* [Toegangstoetsen](#access-keys)
* [Geavanceerde instellingen](#advanced-settings)
* [Redis-Cache Advisor](#redis-cache-advisor)
* [Schalen](#scale)
* [Clustergrootte redis](#cluster-size)
* [Redis-gegevenspersistentie](#redis-data-persistence)
* [Updates plannen](#schedule-updates)
* [Geo-replicatie](#geo-replication)
* [Virtueel netwerk](#virtual-network)
* [Firewall](#firewall)
* [Eigenschappen](#properties)
* [Hiermee vergrendelt u](#locks)
* [Automatiseringsscript](#automation-script)



### <a name="access-keys"></a>Toegangssleutels
Klik op **toegangssleutels** weergeven of de toegangssleutels voor uw cache opnieuw genereren. Deze sleutels worden gebruikt door de clients verbinding maken met uw cache.

![Redis-Cache toegangstoetsen](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Geavanceerde instellingen
De volgende instellingen zijn geconfigureerd op de **geavanceerde instellingen** blade.

* [-Poorten](#access-ports)
* [Geheugen-beleid](#memory-policies)
* [Met Keyspace-kennisgevingen (geavanceerde instellingen)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Toegangspoorten
Voor nieuwe caches is niet-SSL-toegang standaard uitgeschakeld. Als de niet-SSL-poort, klikt u op **Nee** voor **toestaan alleen toegang met SSL** op de **geavanceerde instellingen** blade en klik vervolgens op **opslaan**.

![Redis-Cache-poorten](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Geheugen-beleid
De **Maxmemory beleid**, **maxmemory gereserveerd**, en **maxfragmentationmemory gereserveerd** instellingen op de **geavanceerde instellingen** Blade de geheugen beleidsregels configureren voor de cache.

![Redis-Cache Maxmemory beleid](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Beleid voor Maxmemory** configureert u het beleid verwijderen voor de cache en kunt u kiezen uit de volgende beleidsregels voor verwijdering:

* `volatile-lru`-Dit is de standaardinstelling.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Voor meer informatie over `maxmemory` beleid, Zie [Taakverwijdering beleid](http://redis.io/topics/lru-cache#eviction-policies).

De **maxmemory gereserveerd** instelling configureert u de hoeveelheid geheugen in MB die is gereserveerd voor niet-cache-bewerkingen zoals replicatie tijdens failover. Als u deze waarde instelt, kunt u een consistente gebruikerservaring voor Redis-server hebben als de belasting van uw varieert. Deze waarde moet worden ingesteld voor de werkbelastingen die zijn geschreven zware hoger. Wanneer het geheugen is gereserveerd voor dergelijke bewerkingen, is het niet beschikbaar voor de opslag van gegevens in de cache.

De **maxfragmentationmemory gereserveerd** instelling configureert u de hoeveelheid geheugen in MB die is gereserveerd voor geheugenfragmentatie. Als u deze waarde instelt, kunt u een consistente ervaring bieden bij de cache vol is of bijna vol is en de fragmentatie verhouding ook hoge is Redis-server hebben. Wanneer het geheugen is gereserveerd voor dergelijke bewerkingen, is het niet beschikbaar voor de opslag van gegevens in de cache.

Één ding rekening moet houden bij het kiezen van een nieuwe waarde voor geheugen reservering (**maxmemory gereserveerd** of **maxfragmentationmemory gereserveerd**) is hoe deze wijziging van invloed kan zijn op een cache die al wordt uitgevoerd met grote hoeveelheden gegevens. Als u een cache 53 GB met 49 GB aan gegevens, wijzig de waarde van de reservering in 8 GB, wordt dit bijvoorbeeld de maximale hoeveelheid beschikbaar geheugen voor het systeem naar beneden 45 GB verwijderen. Als uw huidige `used_memory` of uw `used_memory_rss` waarden hoger zijn dan de nieuwe limiet van 45 GB en vervolgens het systeem heeft onbeschikbaar maken van gegevens tot beide `used_memory` en `used_memory_rss` hieronder 45 GB zijn. Verwijderen kan de fragmentatie van de belasting en geheugen van de server te verhogen. Voor meer informatie over metrische gegevens cache zoals `used_memory` en `used_memory_rss`, Zie [beschikbare metrische gegevens en de rapportage van intervallen](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> De **maxmemory gereserveerd** en **maxfragmentationmemory gereserveerd** instellingen zijn alleen beschikbaar voor Standard en Premium in cache opgeslagen.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Met Keyspace-kennisgevingen (geavanceerde instellingen)
Redis keyspace meldingen zijn geconfigureerd op de **geavanceerde instellingen** blade. Keyspace-kennisgevingen kunnen clients meldingen wilt ontvangen wanneer bepaalde gebeurtenissen optreden.

![Redis-Cache geavanceerde instellingen](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Met Keyspace-kennisgevingen en de **melden keyspace gebeurtenissen** instelling zijn alleen beschikbaar voor Standard en Premium-caches.
> 
> 

Zie voor meer informatie [Keyspace-kennisgevingen Redis](http://redis.io/topics/notifications). Zie voor een voorbeeld van code, de [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) bestand de [Hallo wereld](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis-cache Advisor
De **Redis-Cache Advisor** blade geeft aanbevelingen voor uw cache. Tijdens normale bewerkingen worden geen aanbevelingen weergegeven. 

![Aanbevelingen](./media/cache-configure/redis-cache-no-recommendations.png)

Als alle voorwaarden wordt voldaan tijdens de bewerkingen van uw cache zoals hoog geheugengebruik, netwerkbandbreedte of belasting van de server, een waarschuwing wordt weergegeven op de **Redis-Cache** blade.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations-alert.png)

Meer informatie vindt u op de **aanbevelingen** blade.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations.png)

U kunt deze metrische gegevens controleren op de [grafieken bewaking](cache-how-to-monitor.md#monitoring-charts) en [gebruik grafieken](cache-how-to-monitor.md#usage-charts) secties van de **Redis-Cache** blade.

Elke prijscategorie heeft verschillende beperkingen voor clientverbindingen, geheugen en bandbreedte. Als uw cache maximale capaciteit voor deze metrische gegevens gedurende een langere periode tijd nadert, wordt een aanbeveling gemaakt. Voor meer informatie over de metrische gegevens en limieten gecontroleerd door de **aanbevelingen** hulpprogramma, Zie de volgende tabel:

| Redis-Cache metrische gegevens | Meer informatie |
| --- | --- |
| Gebruik van netwerkbandbreedte |[Prestaties van de cache - bandbreedte](cache-faq.md#cache-performance) |
| Verbonden clients |[Standaard Redis-serverconfiguratie - maxclients](#maxclients) |
| Belasting van de server |[Gebruik grafieken - belasting van de Redis-Server](cache-how-to-monitor.md#usage-charts) |
| Geheugengebruik |[Prestaties van de cache - grootte](cache-faq.md#cache-performance) |

Als u wilt uw cache bijwerken, klikt u op **nu bijwerken** als de prijscategorie wilt wijzigen en [scale](#scale) uw cache. Zie voor meer informatie over het kiezen van een prijscategorie [welke aanbieding Redis-Cache en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Schalen
Klik op **Scale** weergeven of wijzigen van de prijscategorie voor uw cache. Zie voor meer informatie over het schalen [How to Scale Azure Redis-Cache](cache-how-to-scale.md).

![Redis-Cache prijscategorie](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Clustergrootte redis
Klik op **(PREVIEW) Redis clustergrootte** wijzigen van de clustergrootte voor een actieve premium-cache met clustering is ingeschakeld.

> [!NOTE]
> Houd er rekening mee dat bij de Azure Redis-Cache Premium-laag is vrijgegeven voor algemene beschikbaarheid, de clustergrootte Redis-functie momenteel in preview is.
> 
> 

![Clustergrootte redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in de **Shard aantal** tekstvak en klik op **OK** om op te slaan.

> [!IMPORTANT]
> Redis clustering is alleen beschikbaar voor Premium-caches. Zie [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Clustering voor een Premium Azure Redis Cache configureren) voor meer informatie.
> 
> 


### <a name="redis-data-persistence"></a>Redis-gegevenspersistentie
Klik op **Redis-gegevenspersistentie** als wilt inschakelen, uitschakelen of gegevenspersistentie configureren voor uw cache premium. Azure Redis-Cache met behulp van Redis-persistentie biedt [RDB persistentie](cache-how-to-premium-persistence.md#configure-rdb-persistence) of [AOF persistentie](cache-how-to-premium-persistence.md#configure-aof-persistence).

Zie voor meer informatie [persistentie voor een Premium Azure Redis-Cache configureren](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Redis-gegevenspersistentie is alleen beschikbaar voor Premium-caches. 
> 
> 

### <a name="schedule-updates"></a>Updates plannen
De **updates plannen** blade kunt u een onderhoudsvenster voor Redis-serverupdates voor uw cache aanwijzen. 

> [!IMPORTANT]
> Het onderhoudsvenster is alleen van toepassing op Redis-serverupdates, en niet naar een Azure-updates of updates voor het besturingssysteem van de virtuele machines die als host fungeren van de cache.
> 
> 

![Updates plannen](./media/cache-configure/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren de gewenste dagen het Beginuur van het venster Onderhoud voor elke dag opgeeft, en klik **OK**. Houd er rekening mee dat de duur van het onderhoudsvenster is ingesteld op UTC. 

> [!IMPORTANT]
> De **updates plannen** functionaliteit is alleen beschikbaar voor Premium-laag caches. Zie voor meer informatie en instructies [beheer van Azure Redis-Cache - updates plannen](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Geo-replicatie

De **Geo-replicatie** blade biedt een mechanisme voor het koppelen van twee exemplaren van Premium-laag Azure Redis-Cache. Een cache is aangewezen als het primaire gekoppelde cachegeheugen en de andere als de secundaire gekoppelde cache. De secundaire gekoppelde cache wordt alleen-lezen en gegevens naar de primaire cache geschreven worden gerepliceerd naar de secundaire gekoppelde cache. Deze functionaliteit kan worden gebruikt voor het repliceren van een cache in Azure-regio's.

> [!IMPORTANT]
> **Geo-replicatie** is alleen beschikbaar voor Premium-laag caches. Zie voor meer informatie en instructies [Geo-replicatie configureren voor Azure Redis-Cache](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Virtueel netwerk
De **virtueel netwerk** sectie kunt u de instellingen van het virtuele netwerk voor uw cache configureren. Voor informatie over het maken van een premium-cache met VNET ondersteunen en bijwerken van de instellingen, Zie [ondersteuning voor virtueel netwerk configureren voor een Premium Azure Redis-Cache](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Instellingen voor virtuele netwerken zijn alleen beschikbaar voor premium-caches die zijn geconfigureerd met ondersteuning voor VNET tijdens het maken van de cache. 
> 
> 

### <a name="firewall"></a>Firewall

Klik op **Firewall** bekijken en firewallregels configureren voor uw Premium Azure Redis-Cache.

![Firewall](./media/cache-configure/redis-firewall-rules.png)

U kunt firewallregels opgeven met een begin- en IP-adresbereik. Firewallregels zijn geconfigureerd, alleen clientverbindingen van het opgegeven IP-adresbereiken verbinding kunnen maken met de cache. Wanneer een firewallregel wordt opgeslagen. is er een korte vertraging optreden voordat de regel is van kracht. Dit uitstel is meestal minder dan een minuut.

> [!IMPORTANT]
> Verbindingen van Azure Redis-Cache bewaken altijd toegestaan, zelfs als de firewallregels zijn geconfigureerd.
> 
> Firewallregels zijn alleen beschikbaar voor Premium-laag caches.
> 
> 

### <a name="properties"></a>Eigenschappen
Klik op **eigenschappen** informatie bekijken over uw cache, met inbegrip van de cache-eindpunt en -poorten.

![Redis-Cache-eigenschappen](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Vergrendelingen
De **vergrendelt** sectie kunt u een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Automation-script

Klik op **automatiseringsscript** te exporteren van een sjabloon van uw geïmplementeerde resources voor toekomstige implementaties. Zie voor meer informatie over het werken met sjablonen [implementeren van resources met Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Instellingen voor beheer
De instellingen in de **beheer** sectie kunt u de volgende beheertaken uitvoeren voor uw cache. 

![Beheer](./media/cache-configure/redis-cache-administration.png)

* [Gegevens importeren](#importexport)
* [Gegevens exporteren](#importexport)
* [Opnieuw opstarten](#reboot)


### <a name="importexport"></a>Import/Export
Import/Export is een Azure Redis-Cache gegevensbewerking voor het beheer, zodat u kunt importeren en exporteren van gegevens in de cache door te importeren en exporteren van een momentopname van een Redis-Cache Database (RDB) uit de cache premium naar een paginablob in Azure Storage-Account. Voor importeren/exporteren kunt u migreren tussen verschillende exemplaren van Azure Redis-Cache of het vullen van de cache met gegevens voor het gebruik.

Invoer kan worden gebruikt om Redis compatibele RDB bestanden vanaf een willekeurige Redis-server uitgevoerd in een cloud of de omgeving, inclusief Redis uitgevoerd op Linux, Windows of elke cloudprovider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier voor het maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure Redis-Cache laadt de bestanden RDB uit Azure storage in het geheugen en voegt vervolgens de sleutels in de cache.

Exporteren kunt u de gegevens die zijn opgeslagen in Azure Redis-Cache voor Redis-compatibele RDB bestanden exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van een Azure Redis-Cache-exemplaar naar een andere of een andere Redis-server. Tijdens het exportproces een tijdelijk bestand wordt gemaakt op de virtuele machine die als host fungeert voor de server-exemplaar van Azure Redis-Cache en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor Premium-laag caches. Zie voor meer informatie en instructies [importeren en exporteren van gegevens in Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Opnieuw opstarten
De **opnieuw opstarten** blade kunt u opnieuw opstarten van de knooppunten van de cache. Deze mogelijkheid opnieuw opstarten, kunt u de toepassing te testen voor tolerantie als er een storing van een cacheknooppunt.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot.png)

Als u een premium-cache hebt met clusteren is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot-cluster.png)

Opnieuw opstarten van een of meer knooppunten van de cache, selecteer de gewenste knooppunten en klikt u op **opnieuw opstarten**. Als u een premium-cache hebt met clusteren is ingeschakeld, selecteert u de shard(s) op te starten en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw worden opgestart en er wordt on line later een paar minuten.

> [!IMPORTANT]
> Opnieuw opstarten is nu beschikbaar voor alle Prijscategorieën. Zie voor meer informatie en instructies [beheer van Azure Redis-Cache - opnieuw opstarten](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Bewaking

De **bewaking** sectie kunt u voor het configureren van diagnostische gegevens en de bewaking van uw Redis-Cache. Zie voor meer informatie over het controleren van Azure Redis-Cache en diagnostische gegevens [Azure Redis-Cache controleren](cache-how-to-monitor.md).

![Diagnostiek](./media/cache-configure/redis-cache-diagnostics.png)

* [Metrische gegevens redis](#redis-metrics)
* [Regels voor waarschuwingen](#alert-rules)
* [Diagnostics](#diagnostics)

### <a name="redis-metrics"></a>Metrische Redis-gegevens
Klik op **Redis metrische gegevens** naar [metrische gegevens weergeven](cache-how-to-monitor.md#view-cache-metrics) voor uw cache.

### <a name="alert-rules"></a>Waarschuwingsregels

Klik op **waarschuwing regels** voor het configureren van waarschuwingen op basis van Redis-Cache metrische gegevens. Zie voor meer informatie [waarschuwingen](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Diagnostiek

Cache metrische gegevens in de Azure-Monitor zijn standaard [30 dagen bewaard](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) en vervolgens verwijderd. Om te blijven behouden de metrische gegevens van uw cache langer dan 30 dagen, klikt u op **Diagnostics** naar [configureren van het opslagaccount](cache-how-to-monitor.md#export-cache-metrics) gebruikt voor het opslaan van cache diagnostische gegevens.

>[!NOTE]
>Naast het archiveren van de metrische gegevens van uw cache naar de opslag, kunt u ook [ze naar een Event hub te streamen of ze verzenden naar logboekanalyse](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

## <a name="support--troubleshooting-settings"></a>Ondersteuning en probleemoplossing van instellingen
De instellingen in de **ondersteuning + probleemoplossing** sectie bieden u opties voor het oplossen van problemen met uw cache.

![Ondersteuning en probleemoplossing](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Resourcestatus](#resource-health)
* [Nieuw ondersteuningsverzoek](#new-support-request)

### <a name="resource-health"></a>Status van resources
**Resourcestatus** controleert uw resource en geeft u aan als deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de health-service van Azure Resource [overzicht van Azure Resource health](../resource-health/resource-health-overview.md).

> [!NOTE]
> De resourcestatus is momenteel niet mogelijk om te rapporteren over de status van Azure Redis-Cache-exemplaren die worden gehost in een virtueel netwerk. Zie voor meer informatie [alle functies van de cache werken bij het hosten van een cache in een VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Nieuw ondersteuningsverzoek
Klik op **nieuw ondersteuningsverzoek** om ondersteuning te vragen voor uw cache.





## <a name="default-redis-server-configuration"></a>Standaardconfiguratie van het Redis-server
Nieuwe exemplaren van Azure Redis-Cache zijn geconfigureerd met de volgende Redis configuratie standaardwaarden.

> [!NOTE]
> De instellingen in deze sectie kunnen niet worden gewijzigd met de `StackExchange.Redis.IServer.ConfigSet` methode. Als deze methode wordt aangeroepen met een van de opdrachten in deze sectie, een vergelijkbaar met de volgende uitzondering:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Alle waarden die kunnen worden geconfigureerd, zoals **max-geheugen-policy**, kunnen worden geconfigureerd via de Azure-portal of het opdrachtregelprogramma beheerprogramma's, zoals Azure CLI of PowerShell.
> 
> 

| Instelling | Standaardwaarde | Beschrijving |
| --- | --- | --- |
| `databases` |16 |Het aantal databases 16 is, maar u kunt een ander nummer is gebaseerd op de prijscategorie configureren. <sup>1</sup> de standaarddatabase DB 0 is, kunt u een andere naam op een afzonderlijke verbinding basis met `connection.GetDatabase(dbid)` waar `dbid` is een getal tussen `0` en `databases - 1`. |
| `maxclients` |Afhankelijk van de prijscategorie<sup>2</sup> |Dit is het maximum aantal verbonden clients tegelijk toegestaan. Zodra de limiet is bereikt Redis Hiermee sluit u de nieuwe verbindingen, een 'maximum aantal clients bereikt'-fout. |
| `maxmemory-policy` |`volatile-lru` |Beleid voor Maxmemory is de instelling voor hoe Redis geselecteerd wat u moet verwijderen wanneer `maxmemory` (de grootte van de cache die u hebt geselecteerd bij het maken van de cache van de aanbieding) is bereikt. Met Azure Redis-Cache de standaardinstelling is `volatile-lru`, waarbij de sleutels worden verwijderd met een vervaldatum instellen met behulp van een LRU-algoritme. Deze instelling kan worden geconfigureerd in de Azure portal. Zie voor meer informatie [geheugen beleid](#memory-policies). |
| `maxmemory-samples` |3 |Voor het opslaan van geheugen zijn LRU- en minimale TTL-algoritmes redelijk algoritmen in plaats van nauwkeurige algoritmen. Standaard Redis controles drie sleutels en aanbiedingen die minder recent is gebruikt. |
| `lua-time-limit` |5,000 |Maximale uitvoeringstijd van een script Lua in milliseconden. Als de maximale uitvoeringstijd is bereikt, registreert Redis dat een script nog steeds uitgevoerd na de maximale toegestane tijd wordt en begint te reageren op query's met een fout. |
| `lua-event-limit` |500 |Maximale grootte van de wachtrij script. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |De limieten van de client uitvoer buffer kunnen worden gebruikt om af te dwingen verbreken van de verbinding van clients die niet van gegevens van de server snel genoeg voor een bepaalde reden lezen zijn (een veelvoorkomende reden is dat een client Pub subitems berichten zo snel als de publisher kan ze produceren kan niet gebruiken). Zie voor meer informatie [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>de limiet voor `databases` verschilt voor elk Azure Redis-Cache prijscategorie en kan worden ingesteld bij het maken van de cache. Als er geen `databases` instelling is opgegeven tijdens het maken van de cache, de standaardwaarde is 16.

* Basic en Standard caches
  * C0 (250 MB)-cache - maximaal 16-databases
  * C1 (1 GB)-cache - maximaal 16-databases
  * C2 (2,5 GB)-cache - maximaal 16-databases
  * C3 (6 GB)-cache - maximaal 16-databases
  * C4 (13 GB)-cache - maximaal 32 databases
  * C5 (26 GB)-cache - maximaal 48 databases
  * C6 (53 GB)-cache - maximaal 64 databases
* Premium-caches
  * P1 (6 GB - 60 GB) tot maximaal 16-databases
  * P2 (13 GB - 130 GB), maximaal 32 databases
  * P3 (26 GB - 260 GB) tot maximaal 48 databases
  * P4 (53 GB - 530 GB) tot maximaal 64 databases
  * Alle premium caches met Redis-cluster ingeschakeld - Redis-cluster biedt alleen ondersteuning voor gebruik van 0-database zodat de `databases` limiet voor elke premium-cache met Redis-cluster ingeschakeld is in feite 1 en de [Selecteer](http://redis.io/commands/select) opdracht is niet toegestaan. Zie voor meer informatie [moet ik mijn clienttoepassing clustering Breng eventuele wijzigingen?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Zie voor meer informatie over databases [wat Redis-databases zijn?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> De `databases` instelling kan worden geconfigureerd tijdens de cache maken en alleen met behulp van PowerShell, CLI of andere clients management. Voor een voorbeeld van de configuratie van `databases` tijdens het maken van de cache met behulp van PowerShell, Zie [nieuw AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` verschilt voor elk Azure Redis-Cache prijscategorie.

* Basic en Standard caches
  * C0 (250 MB)-cache - maximaal 256-verbindingen
  * C1 (1 GB)-cache - maximaal 1000 verbindingen
  * C2 (2,5 GB)-cache - maximaal 2.000 verbindingen
  * C3 (6 GB)-cache - maximaal 5000 verbindingen
  * C4 (13 GB)-cache - maximaal 10.000 verbindingen
  * C5 (26 GB)-cache - maximaal 15.000 verbindingen
  * C6 (53 GB)-cache - maximaal 20.000 verbindingen
* Premium-caches
  * P1 (6 GB - 60 GB) tot 7.500 verbindingen
  * P2 (13 GB - 130 GB) tot maximaal 15.000 verbindingen
  * P3 (26 GB - 260 GB) tot 30.000 verbindingen
  * P4 (53 GB - 530 GB) tot 40.000 verbindingen

> [!NOTE]
> Kunt u elke grootte van cache *tot* een bepaald aantal verbindingen, elke verbinding met Redis heeft overhead gekoppeld. Een voorbeeld van een dergelijke overhead zou zijn CPU- en geheugengebruik als gevolg van TLS/SSL-versleuteling. De maximale verbindingslimiet voor een opgegeven cachegrootte wordt ervan uitgegaan dat een licht geladen cache. Als uit verbinding overhead laden *plus* laden vanaf de clientbewerkingen dan er capaciteit voor het systeem, de cache kunt capaciteitsproblemen moeten ondervinden zelfs als u hebt niet de verbindingslimiet voor de huidige cachegrootte overschreden.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis opdrachten niet ondersteund in Azure Redis-Cache
> [!IMPORTANT]
> Omdat de configuratie en beheer van exemplaren van Azure Redis-Cache wordt beheerd door Microsoft, worden de volgende opdrachten zijn uitgeschakeld. Als u probeert aan te roepen ze, ontvangt u een foutbericht weergegeven dat vergelijkbaar is met `"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * CONFIGURATIE
> * DEBUG
> * MIGREREN
> * OPSLAAN
> * SHUTDOWN
> * SLAVEOF
> * CLUSTER - schrijven zijn uitgeschakeld, maar alleen-lezen Cluster opdrachten zijn toegestaan.
> 
> 

Zie voor meer informatie over Redis opdrachten [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis-console
Opdrachten kunnen veilig worden verleend aan uw Azure Redis-Cache-exemplaren die gebruikmaken van de **Redis-Console**, die beschikbaar is in de Azure-portal voor alle cachelagen.

> [!IMPORTANT]
> - De Redis-Console werkt niet met [VNET](cache-how-to-premium-vnet.md). Wanneer uw cache deel van een VNET uitmaakt, alleen de clients in het VNET, hebben toegang tot de cache. Omdat Redis-Console wordt uitgevoerd in uw lokale browser, die zich buiten het VNET, kan het verbinden met uw cache.
> - Niet alle Redis-opdrachten worden ondersteund in Azure Redis-Cache. Voor een lijst met Redis-opdrachten die zijn uitgeschakeld voor Azure Redis-Cache, Zie de vorige [Redis opdrachten niet ondersteund in Azure Redis-Cache](#redis-commands-not-supported-in-azure-redis-cache) sectie. Zie voor meer informatie over Redis opdrachten [http://redis.io/commands](http://redis.io/commands).
> 
> 

Voor toegang tot de Redis-Console, klikt u op **Console** van de **Redis-Cache** blade.

![Redis-console](./media/cache-configure/redis-console-menu.png)

Voor het uitgeven van opdrachten op basis van uw cache-exemplaar, typt u de gewenste opdracht in de console.

![Redis-console](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a>Gebruik de Redis-Console met een geclusterd premium-cache

Wanneer met de Redis-Console met een premium cache voor geclusterde, kunt u opdrachten uitgeven aan een enkele shard van de cache. Om een opdracht te verlenen aan een specifieke shard, eerst verbinding maken met de gewenste shard door erop te klikken op de shard-objectkiezer.

![Redis-console](./media/cache-configure/redis-console-premium-cluster.png)

Als u probeert te krijgen tot een sleutel die is opgeslagen in een andere shard dan de verbonden shard, ontvangt u een foutbericht weergegeven dat vergelijkbaar is met het volgende bericht.

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

In het vorige voorbeeld shard 1 wordt de geselecteerde shard maar `myKey` bevindt zich in de shard 0, zoals aangegeven door de `(shard 0)` gedeelte van het foutbericht. In dit voorbeeld voor toegang tot `myKey`, selecteert u shard 0 met behulp van de shard-picker en voert u de gewenste opdracht.


## <a name="move-your-cache-to-a-new-subscription"></a>Uw cache verplaatsen naar een nieuw abonnement
U kunt uw cache verplaatsen naar een nieuw abonnement door te klikken op **verplaatsen**.

![Verplaatsen van Redis-Cache](./media/cache-configure/redis-cache-move.png)

Zie voor meer informatie over het verplaatsen van resources uit één resourcegroep naar een andere, en één abonnement [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het werken met Redis-opdrachten [hoe kan ik Redis-opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)

