---
title: Azure Redis-Cache met Azure PowerShell beheren | Microsoft Docs
description: Informatie over het uitvoeren van beheertaken voor Azure Redis-Cache met Azure PowerShell.
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: 58f8601fa780ac86729f60e9e30f4c6a91c73deb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure Redis-Cache met Azure PowerShell beheren
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure-CLI](cache-manage-cli.md)
> 
> 

Dit onderwerp leest u hoe algemene taken is om uit te voeren, zoals maken, bijwerken en schalen van uw Azure Redis-Cache-exemplaren, hoe toegangstoetsen opnieuw genereren en hoe informatie bekijken over uw caches. Zie voor een volledige lijst met Azure Redis-Cache PowerShell-cmdlets, [cmdlets van Azure Redis-Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Zie voor meer informatie over het klassieke implementatiemodel [Azure Resource Manager versus klassieke implementatie: begrijpen implementatiemodellen en de status van uw resources](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Vereisten
Als u Azure PowerShell al hebt geïnstalleerd, moet u Azure PowerShell versie 1.0.0 hebben of hoger. U kunt controleren de versie van Azure PowerShell die u hebt geïnstalleerd met deze opdracht bij de Azure PowerShell-opdrachtprompt.

    Get-Module azure | format-table version


Eerst moet u zich bij Azure met deze opdracht.

    Login-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het bijbehorende wachtwoord in het dialoogvenster van de aanmeldingspagina Microsoft Azure.

Als u meerdere Azure-abonnementen hebt, moet u vervolgens uw Azure-abonnement instellen. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Geef het abonnement door de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Voordat u Windows PowerShell met Azure Resource Manager gebruiken kunt, moet u het volgende:

* Windows PowerShell versie 3.0 of 4.0. De versie van Windows PowerShell, typt:`$PSVersionTable` en controleer of de waarde van `PSVersion` 3.0 of 4.0. Zie het installeren van een compatibele versie [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Gebruik de cmdlet Get-Help voor gedetailleerde hulp bij voor elke cmdlet die u in deze zelfstudie ziet.

    Get-Help <cmdlet-name> -Detailed

Als u bijvoorbeeld ondersteuning voor de `New-AzureRmRedisCache` cmdlet, type:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Verbinding maken met andere clouds
Standaard de Azure-omgeving is `AzureCloud`, die staat voor het globale Azure-cloud-exemplaar. Gebruiken voor verbinding met een ander exemplaar, de `Add-AzureRmAccount` opdracht met de `-Environment` of -`EnvironmentName` opdrachtregelparameter met de gewenste omgeving of de omgevingsnaam.

Als u wilt zien van de lijst met beschikbare omgevingen, de `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbinding maken met de Azure Government-Cloud
Voor verbinding met de Azure Government Cloud, gebruikt u een van de volgende opdrachten.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Voor het maken van een cache in de Cloud van Azure Government, gebruikt u een van de volgende locaties.

* USGov Virginia
* USGov Iowa

Zie voor meer informatie over de Azure Government Cloud [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) en [Ontwikkelaarshandleiding voor Microsoft Azure Government](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbinding maken met de Azure China-Cloud
Voor verbinding met de Azure-Cloud China, moet u een van de volgende opdrachten gebruiken.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Voor het maken van een cache in de Azure China Cloud, gebruikt u een van de volgende locaties.

* China - oost
* China - noord

Zie voor meer informatie over de Azure-Cloud China [AzureChinaCloud voor Azure wordt beheerd door 21Vianet in China](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Verbinding maken met Microsoft Azure Duitsland
Voor verbinding met Microsoft Azure Duitsland, gebruikt u een van de volgende opdrachten.

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Voor het maken van een cache in Microsoft Azure Duitsland, gebruikt u een van de volgende locaties.

* Duitsland - centraal
* Duitsland - noordoost

Zie voor meer informatie over Microsoft Azure Duitsland [Microsoft Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschappen die worden gebruikt voor Azure Redis-Cache PowerShell
De volgende tabel bevat de eigenschappen en beschrijvingen voor vaak gebruikte parameters bij het maken en beheren van uw Azure Redis-Cache-exemplaren die gebruikmaken van Azure PowerShell.

| Parameter | Beschrijving | Standaard |
| --- | --- | --- |
| Naam |Naam van de cache | |
| Locatie |Locatie van de cache | |
| ResourceGroupName |Naam van resourcegroep waarin u de cache maken | |
| Grootte |De grootte van de cache. Geldige waarden zijn: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |Het aantal shards maken bij het maken van een premium-cache met clustering is ingeschakeld. Geldige waarden zijn: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Hiermee geeft u de SKU van de cache. Geldige waarden zijn: Basic, Standard, Premium |Standard |
| RedisConfiguration |Hiermee geeft u Redis-configuratie-instellingen. Voor meer informatie over iedere instelling, Zie de volgende [RedisConfiguration eigenschappen](#redisconfiguration-properties) tabel. | |
| EnableNonSslPort |Hiermee wordt aangegeven of de poort zonder SSL-beveiliging is ingeschakeld. |False |
| MaxMemoryPolicy |Deze parameter is afgeschaft - gebruik in plaats daarvan RedisConfiguration. | |
| StaticIP |Bij het hosten van uw cache in een VNET, geeft een uniek IP-adres in het subnet voor de cache. Als niet wordt opgegeven, wordt een gekozen voor u van het subnet. | |
| Subnet |Bij het hosten van uw cache in een VNET, geeft de naam van het subnet waarin de cache te implementeren. | |
| VirtualNetwork |Hiermee geeft u de resource-ID van de VNET waar voor het implementeren van de cache bij het hosten van uw cache in een VNET. | |
| KeyType |Hiermee geeft u op welke toegangssleutel opnieuw te genereren tijdens het vernieuwen van de toegangssleutels. Geldige waarden zijn: primaire, secundaire | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration eigenschappen
| Eigenschap | Beschrijving | Prijscategorieën |
| --- | --- | --- |
| RDB back-up is ingeschakeld |Of [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) is ingeschakeld |Alleen Premium |
| RDB---verbindingsreeks voor opslag |De verbindingsreeks naar het opslagaccount voor [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |Alleen Premium |
| back-up RDB frequentie |De back-upfrequentie voor [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |Alleen Premium |
| maxmemory gereserveerd |Hiermee configureert u de [geheugen gereserveerd](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor niet-cache-processen |Standard en Premium |
| maxmemory-beleid |Hiermee configureert u de [taakverwijdering beleid](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor de cache |Alle Prijscategorieën |
| melding-keyspace-gebeurtenissen |Hiermee configureert u [keyspace-kennisgevingen](cache-configure.md#keyspace-notifications-advanced-settings) |Standard en Premium |
| hash-max-ziplist-entries |Hiermee configureert u [geheugenoptimalisatie](http://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| hash-max-ziplist-value |Hiermee configureert u [geheugenoptimalisatie](http://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| set-max-intset-entries |Hiermee configureert u [geheugenoptimalisatie](http://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| zset-max-ziplist-entries |Hiermee configureert u [geheugenoptimalisatie](http://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| zset-max-ziplist-value |Hiermee configureert u [geheugenoptimalisatie](http://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| databases |Hiermee configureert u het aantal databases. Deze eigenschap kan alleen bij het maken van de cache worden geconfigureerd. |Standard en Premium |

## <a name="to-create-a-redis-cache"></a>Een Redis-Cache maken
Nieuwe Azure Redis-Cache-exemplaren worden gemaakt met de [nieuw AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.

> [!IMPORTANT]
> De eerste keer dat u een Redis-cache maken in een abonnement met Azure portal, de portal registreert de `Microsoft.Cache` naamruimte voor dat abonnement. Als u probeert te maken van de eerste Redis-cache in een abonnement met behulp van PowerShell, moet u eerst registreren die naamruimte met de volgende opdracht. anders cmdlets zoals `New-AzureRmRedisCache` en `Get-AzureRmRedisCache` mislukken.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `New-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Voer de volgende opdracht maakt een cache met standaardparameters.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, en `Location` vereiste parameters zijn, maar de overige zijn optioneel en standaardwaarden. De vorige opdracht uit te voeren, maakt een standaard SKU Azure Redis-Cache-exemplaar met de opgegeven naam, de locatie en de resourcegroep die 1 GB aan de grootte van de poort zonder SSL-beveiliging is uitgeschakeld.

Maken van een premium-cache, een grootte van P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), geef P3 (26 GB - 260 GB), of P4 (53 GB tot 530 GB). Als u wilt inschakelen voor clustering, geeft u een shard aantal met de `ShardCount` parameter. Het volgende voorbeeld maakt een premium P1 cache met 3 shards. Een cache van de premium P1 6 GB groot is en omdat we drie shards opgegeven de totale grootte is 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Waarden opgeven voor de `RedisConfiguration` parameter, moet u de waarden in `{}` als sleutel/waarde-paren, zoals `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Het volgende voorbeeld wordt een standaard 1 GB cache met `allkeys-random` maxmemory-beleid en keyspace meldingen geconfigureerd met `KEA`. Zie voor meer informatie [Keyspace-kennisgevingen (geavanceerde instellingen)](cache-configure.md#keyspace-notifications-advanced-settings) en [geheugen beleid](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Voor het configureren van de databases tijdens het maken van de cache instellen
De `databases` instelling kan alleen tijdens het maken van de cache worden geconfigureerd. Het volgende voorbeeld wordt een premium P3 (26 GB)-cache met 48-databases via de [nieuw AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Voor meer informatie over de `databases` eigenschap, Zie [serverconfiguratie Azure Redis-Cache standaard](cache-configure.md#default-redis-server-configuration). Voor meer informatie over het maken van een cache met behulp van de [nieuw AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, Zie de vorige [voor het maken van een Redis-Cache](#to-create-a-redis-cache) sectie.

## <a name="to-update-a-redis-cache"></a>Bijwerken van een Redis-cache
Azure Redis-Cache-exemplaren zijn bijgewerkt met de [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Set-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

De `Set-AzureRmRedisCache` cmdlet kan worden gebruikt voor het bijwerken van eigenschappen zoals `Size`, `Sku`, `EnableNonSslPort`, en de `RedisConfiguration` waarden. 

De volgende opdracht werkt het maxmemory-beleid voor de Redis-Cache met de naam myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a>Schalen van een Redis-cache
`Set-AzureRmRedisCache`kan worden gebruikt voor het schalen van een Azure Redis-cache-exemplaar wanneer de `Size`, `Sku`, of `ShardCount` eigenschappen zijn gewijzigd. 

> [!NOTE]
> Schalen van een cache met behulp van PowerShell is onderworpen aan dezelfde limieten en richtlijnen als het schalen van een cache van de Azure-portal. U kunt schalen naar een andere prijscategorie met de volgende beperkingen.
> 
> * U kan niet uit een hogere prijscategorie schalen naar een lagere prijscategorie.
> * U kunt geen schalen van een **Premium** omlaag naar de cache een **standaard** of een **Basic** cache.
> * U kunt geen schalen van een **standaard** omlaag naar de cache een **Basic** cache.
> * U kunt opschalen van een **Basic** in de cache op een **standaard** cache, maar de grootte niet wijzigen op hetzelfde moment. Als u een ander formaat nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
> * U kunt geen schalen van een **Basic** rechtstreeks naar de cache een **Premium** cache. Moet u de schaal van **Basic** naar **standaard** in één bewerking van de schaal en vervolgens van **standaard** naar **Premium** in een latere bewerking voor vergroten/verkleinen.
> * Kan niet worden geschaald uit een groter formaat omlaag naar de **C0 (250 MB)** grootte.
> 
> Zie voor meer informatie [How to Scale Azure Redis-Cache](cache-how-to-scale.md).
> 
> 

Het volgende voorbeeld laat zien hoe een cache met de naam schalen `myCache` naar een cache 2,5 GB. Houd er rekening mee dat deze opdracht voor zowel een Basislidmaatschap of een standaard-cache werkt.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nadat u deze opdracht wordt uitgevoerd, wordt de status van de cache geretourneerd (vergelijkbaar met aanroepen `Get-AzureRmRedisCache`). Houd er rekening mee dat de `ProvisioningState` is `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Wanneer de schaal voltooid is, de `ProvisioningState` wijzigingen in `Succeeded`. Als u wilt maken van een latere vergroten/verkleinen bewerking, zoals het wijzigen van Basic op standaard en vervolgens wijzigen de grootte, moet u wachten totdat de vorige bewerking voltooid is of er een ziet er als volgt foutbericht.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Voor informatie over een Redis-cache
U kunt informatie ophalen over een cache met behulp van de [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Get-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Voor informatie over alle caches in het huidige abonnement, voeren `Get-AzureRmRedisCache` zonder parameters.

    Get-AzureRmRedisCache

Voor informatie over alle caches in een specifieke resourcegroep, voeren `Get-AzureRmRedisCache` met de `ResourceGroupName` parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Voor informatie over een specifieke cache, voeren `Get-AzureRmRedisCache` met de `Name` parameter met de naam van de cache, en de `ResourceGroupName` parameter met de resourcegroep die cache met.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Voor het ophalen van de toegangssleutels voor Redis-cache
Voor het ophalen van de toegangssleutels voor uw cache, kunt u de [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Get-AzureRmRedisCacheKey`, voer de volgende opdracht.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aanroepen voor het ophalen van de sleutels voor uw cache, de `Get-AzureRmRedisCacheKey` cmdlet en de naam van de resourcegroep met de cache van de naam van uw cache doorgeeft.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Toegangstoetsen voor Redis-cache opnieuw genereren
Als u wilt genereren de toegangssleutels voor uw cache, kunt u de [nieuw AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `New-AzureRmRedisCacheKey`, voer de volgende opdracht.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aanroepen voor het genereren van de primaire of secundaire sleutel voor uw cache, de `New-AzureRmRedisCacheKey` cmdlet en geven de naam van de resourcegroep, en Geef ofwel `Primary` of `Secondary` voor de `KeyType` parameter. In het volgende voorbeeld wordt wordt de secundaire toegangssleutel voor een cache gegenereerd.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Een Redis-cache verwijderen
Als u wilt verwijderen een Redis-cache, gebruiken de [verwijderen AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Remove-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

In het volgende voorbeeld wordt de cache met de naam `myCache` wordt verwijderd.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Voor het importeren van een Redis-cache
U kunt gegevens importeren in een Azure Redis-Cache-exemplaar met de `Import-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over het importeren/exporteren [importeren en exporteren van gegevens in Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Import-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht importeert gegevens van de blob die is opgegeven door de SAS-uri in Azure Redis-Cache.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Exporteren van een Redis-cache
U kunt gegevens exporteren vanuit een Azure Redis-Cache-exemplaar met de `Export-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over het importeren/exporteren [importeren en exporteren van gegevens in Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Export-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht exporteert gegevens uit een Azure Redis-Cache-exemplaar in de container die is opgegeven door de SAS-uri.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Opnieuw opstarten van een Redis-cache
U kunt opstarten uw Azure Redis-Cache-exemplaar met de `Reset-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Opnieuw opstarten is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over het opnieuw opstarten van uw cache [beheer in de Cache - opnieuw opstarten](cache-administration.md#reboot).
> 
> 

Voor een overzicht van de beschikbare parameters en de bijbehorende beschrijvingen voor `Reset-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht wordt opnieuw opgestart beide knooppunten van de opgegeven cache.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over het gebruik van Windows PowerShell met Azure:

* [Azure Redis-Cache-cmdlet-documentatie op MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): informatie over het gebruik van de cmdlets in de module Azure Resource Manager.
* [Resourcegroepen gebruiken voor het beheren van uw Azure-resources](../azure-resource-manager/resource-group-template-deploy-portal.md): informatie over het maken en beheren van resourcegroepen in de Azure-portal.
* [Azure-blog](http://blogs.msdn.com/windowsazure): meer informatie over nieuwe functies in Azure.
* [Windows PowerShell-blog](http://blogs.msdn.com/powershell): meer informatie over nieuwe functies in Windows PowerShell.
* ["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): echte tips en trucs ophalen uit de Windows PowerShell-community.

